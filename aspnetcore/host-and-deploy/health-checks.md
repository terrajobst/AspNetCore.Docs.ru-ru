---
title: Проверки работоспособности в ASP.NET Core
author: guardrex
description: Узнайте, как настроить проверки работоспособности для инфраструктуры ASP.NET Core, например приложений и баз данных.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 04/23/2019
uid: host-and-deploy/health-checks
ms.openlocfilehash: 5119267a8da5c950989b14b7c2e818aa22806506
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64887929"
---
# <a name="health-checks-in-aspnet-core"></a>Проверки работоспособности в ASP.NET Core

Авторы: [Люк Лэтем](https://github.com/guardrex) (Luke Latham) и [Гленн Кондрон](https://github.com/glennc) (Glenn Condron)

ASP.NET Core предоставляет ПО промежуточного слоя и библиотеки для создания отчетов о работоспособности компонентов инфраструктуры приложения.

Проверки работоспособности предоставляются приложением как конечные точки HTTP. Конечные точки проверки работоспособности можно настроить для различных сценариев в реальном времени мониторинга:

* Проверки работоспособности можно использовать с оркестраторами контейнеров и подсистемами балансировки нагрузки, чтобы проверять состояние приложения. Например, оркестратор контейнеров может реагировать на неуспешную проверку работоспособности, остановив последовательное развертывание или перезапустив контейнер. Балансировщик нагрузки может реагировать на неработоспособное приложение путем перенаправления трафика от неисправного экземпляра к работающему экземпляру.
* Использование памяти, диска и других ресурсов физического сервера можно отслеживать с точки зрения работоспособности.
* Проверки работоспособности позволяют проверять зависимости приложения, такие как базы данных и конечные точки внешних служб, чтобы убедиться в доступности и нормальной работе.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([как скачивать](xref:index#how-to-download-a-sample))

Пример приложения включает примеры сценариев, описанных в этом разделе. Чтобы запустить пример приложения для заданного сценария, используйте команду [dotnet run](/dotnet/core/tools/dotnet-run) из папки проекта в командной строке. См. файл *README.md* приложения и описания сценариев в этом разделе, чтобы получить сведения о том, как использовать пример приложения.

## <a name="prerequisites"></a>Предварительные требования

Проверки работоспособности обычно используются в оркестраторе контейнеров или внешней службе мониторинга для проверки состояния приложения. Перед добавлением проверки работоспособности приложения выберите используемую систему мониторинга. Система мониторинга определяет, какие виды проверки работоспособности создавать и как настроить их конечные точки.

Добавьте ссылку на [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) или ссылку на пакет [Microsoft.AspNetCore.Diagnostics.HealthCheck](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks).

В примере приложения приведен код запуска для демонстрации проверки работоспособности для нескольких сценариев. Сценарий [проверки базы данных](#database-probe) проверяет работоспособность подключения базы данных с помощью [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks). Сценарий [проверки DbContext](#entity-framework-core-dbcontext-probe) проверяет базу данных с помощью EF Core `DbContext`. Чтобы изучить сценарии для баз данных, пример приложения выполняет следующие действия.

* Создает базу данных и указывает ее строку подключения в файле приложения *appsettings.json*.
* Содержит следующие ссылки на пакеты в своем файле проекта.
  * [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) — это порт [BeatPulse](https://github.com/xabaril/beatpulse), и он не поддерживается корпорацией Майкрософт.

В другом сценарии проверки работоспособности демонстрируется способ фильтрации проверок работоспособности по порту управления. Пример приложения требует создать файл *Properties/launchSettings.json*, который включает в себя URL-адрес управления и порт управления. Дополнительные сведения см. в разделе [Фильтр по портам](#filter-by-port).

## <a name="basic-health-probe"></a>Базовая проверка работоспособности

Для многих приложений достаточно конфигурации базовой проверки работоспособности, которая сообщает о доступности приложения для обработки запросов (*жизнеспособности*).

Базовая конфигурация регистрирует службы проверки работоспособности и вызывает ПО промежуточного слоя для ответа о работоспособности на конечной точке по URL-адресу. По умолчанию отдельные проверки работоспособности не регистрируются для тестирования какой-либо конкретной зависимости или подсистемы. Приложение считается исправным, если оно способно отвечать на запросы по URL-адресу конечной точки работоспособности. Модуль записи ответа по умолчанию передает состояние (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) в виде обычного текста в ответе клиенту. Это может быть состояние [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) или [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus).

Зарегистрируйте службы проверки работоспособности при помощи <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> в `Startup.ConfigureServices`. Добавьте ПО промежуточного слоя в <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> в конвейере обработки запросов `Startup.Configure`:

В примере приложения конечная точка проверки работоспособности создается в `/health` (*BasicStartup.cs*):

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/BasicStartup.cs?name=snippet1&highlight=5,10)]

Чтобы запустить сценарий базовой конфигурации с помощью примера приложения, выполните следующую команду из папки проекта в командной строке:

```console
dotnet run --scenario basic
```

### <a name="docker-example"></a>Пример для Docker

[Docker](xref:host-and-deploy/docker/index) предлагает встроенную директиву `HEALTHCHECK`, которую можно вызвать для проверки состояния приложения, использующего базовую конфигурацию проверки работоспособности:

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a>Создание проверки работоспособности

Проверки работоспособности создаются путем реализации интерфейса <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck>. Метод <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> возвращает `Task<` <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> `>`, что означает состояние работоспособности `Healthy`, `Degraded` или `Unhealthy`. Результат записывается в ответ в виде обычного текста с кодом состояния, который можно настроить (конфигурация описана в разделе [Варианты проверки работоспособности](#health-check-options)). <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> также может возвращать необязательные пары "ключ-значение".

### <a name="example-health-check"></a>Пример проверки работоспособности

Следующий класс `ExampleHealthCheck` демонстрирует структуру проверки работоспособности:

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

### <a name="register-health-check-services"></a>Зарегистрируйте службы проверки работоспособности

Тип `ExampleHealthCheck` будет добавлен к службам проверки работоспособности через <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck<ExampleHealthCheck>("example_health_check");
}
```

Перегрузка <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, показанная в следующем примере, устанавливает состояние сбоя (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) для отчета в ситуации, когда проверка работоспособности сообщает о сбое. Если состояние сбоя имеет значение `null` (по умолчанию), сообщается [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus). Эта перегрузка — полезный сценарий для авторов библиотек, где состояние сбоя от библиотеки особо учитывается приложением при сбое проверки работоспособности, если реализация проверки работоспособности учитывает этот параметр.

*Теги* можно использовать для фильтрации проверок работоспособности (описаны далее в разделе [Фильтрация проверок работоспособности](#filter-health-checks)).

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> также может выполнять лямбда-функции. В следующем примере имя проверки работоспособности указано как `Example`; проверка всегда возвращает работоспособное состояние:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck("Example", () =>
            HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" })
}
```

### <a name="use-health-checks-middleware"></a>Используйте ПО промежуточного слоя для проверки работоспособности

В `Startup.Configure` вызовите <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> в конвейере обработки с URL-адресом конечной точки или относительным путем:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health");
}
```

Если проверки работоспособности должны прослушивать определенный порт, используйте перегрузку <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*>, чтобы задать порт (описаны далее в разделе [Фильтр по портам](#filter-by-port)):

```csharp
app.UseHealthChecks("/health", port: 8000);
```

ПО промежуточного слоя — это *терминальное ПО промежуточного слоя* в конвейере обработки запросов приложения. Первая конечная точка проверки работоспособности, которая обнаружила точное соответствие URL-адресу запроса, выполняется и пропускает остаток конвейера ПО промежуточного слоя. В случае пропуска ПО промежуточного слоя после совпавшей проверки работоспособности не выполняется.

## <a name="health-check-options"></a>Варианты проверки работоспособности

<xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> предоставляет возможность настройки поведения для проверки работоспособности:

* [Фильтрация проверок работоспособности](#filter-health-checks)
* [Настройка кода состояния HTTP](#customize-the-http-status-code)
* [Подавление заголовков кэша](#suppress-cache-headers)
* [Настройка выходных данных](#customize-output)

### <a name="filter-health-checks"></a>Фильтрация проверок работоспособности

По умолчанию ПО промежуточного слоя для проверки работоспособности выполняет все зарегистрированные проверки работоспособности. Чтобы выполнить подмножество проверок работоспособности, укажите функцию, которая возвращает логическое значение через параметр <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate>. В следующем примере проверка работоспособности `Bar` отфильтровывается по тегу (`bar_tag`) в условном операторе функции, где `true` возвращается только в том случае, если свойство проверки работоспособности <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> соответствует `foo_tag` или `baz_tag`:

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

### <a name="customize-the-http-status-code"></a>Настройка кода состояния HTTP

Используйте <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> для настройки сопоставления состояний работоспособности и кодов состояния HTTP. Следующие назначения <xref:Microsoft.AspNetCore.Http.StatusCodes> являются значениями по умолчанию, используемыми ПО промежуточного слоя. Измените значения кодов состояния в соответствии с требованиями.

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

### <a name="suppress-cache-headers"></a>Подавление заголовков кэша

<xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> определяет, добавляет ли ПО промежуточного слоя HTTP-заголовки в ответ проверки, чтобы предотвратить кэширование ответов. Если значение равно `false` (по умолчанию), ПО промежуточного слоя задает или переопределяет заголовки `Cache-Control`, `Expires` и `Pragma`, чтобы предотвратить кэширование ответов. Если значение равно `true`, ПО промежуточного слоя не изменяет заголовки кэша в ответе.

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

### <a name="customize-output"></a>Настройка выходных данных

Параметр <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> возвращает или задает делегат, используемый для записи ответа. Делегат по умолчанию записывает минимальный ответ в формате открытого текста со строковым значением [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).

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

## <a name="database-probe"></a>Проверка базы данных

Проверка работоспособности может задать запрос для выполнения в качестве логического теста для указания того, отвечает ли база данных как обычно.

Пример приложения использует [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), библиотеку проверки работоспособности для приложений ASP.NET Core, чтобы проверить работоспособность базы данных SQL Server. `AspNetCore.Diagnostics.HealthChecks` выполняет запрос `SELECT 1` к базе данных, чтобы подтвердить, что подключение к базе данных находится в работоспособном состоянии.

> [!WARNING]
> При проверке подключения к базе данных с помощью запроса выберите запрос, возвращающий результат быстро. Метод запроса связан с риском перегрузки базы данных и снижения ее производительности. В большинстве случаев тестовый запрос не является обязательным. Достаточно успешного подключения к базе данных. Если вам понадобится выполнить запрос, выберите простой запрос SELECT, например `SELECT 1`.

Добавьте ссылку на пакет [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).

Укажите допустимую строку подключения к базе данных в файле примера приложения *appsettings.json*. Приложение использует базу данных SQL Server с именем `HealthCheckSample`:

[!code-json[](health-checks/samples/2.x/HealthChecksSample/appsettings.json?highlight=3)]

Зарегистрируйте службы проверки работоспособности при помощи <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> в `Startup.ConfigureServices`. Пример приложения вызывает метод `AddSqlServer` со строкой подключения базы данных (*DbHealthStartup.cs*):

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

Вызовите ПО промежуточного слоя для проверки работоспособности в конвейере обработки приложения в `Startup.Configure`:

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_Configure)]

Чтобы запустить сценарий проверки базы данных с помощью примера приложения, выполните следующую команду из папки проекта в командной строке:

```console
dotnet run --scenario db
```

> [!NOTE]
> [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) — это порт [BeatPulse](https://github.com/xabaril/beatpulse), и он не поддерживается корпорацией Майкрософт.

## <a name="entity-framework-core-dbcontext-probe"></a>Проверка DbContext в Entity Framework Core

Эта проверка `DbContext` подтверждает, что приложение может взаимодействовать с базой данных, настроенной для EF Core `DbContext`. Проверка `DbContext` поддерживается в приложениях, которые:

* используют [Entity Framework (EF) Core](/ef/core/);
* содержат ссылку на пакет [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).

`AddDbContextCheck<TContext>` регистрирует проверку работоспособности для `DbContext`. `DbContext` передается методу в качестве `TContext`. Доступна перегрузка, позволяющая настроить состояние ошибки, теги и запрос тестирования.

По умолчанию:

* `DbContextHealthCheck` вызывает метод EF Core `CanConnectAsync`. Вы можете указать, какая операция выполняется в том случае, когда проверка работоспособности производится с помощью перегрузок метода `AddDbContextCheck`.
* Имя проверки работоспособности — это имя типа `TContext`.

В примере приложения `AppDbContext` предоставляется для `AddDbContextCheck` и регистрируется в качестве службы в `Startup.ConfigureServices`.

*DbContextHealthStartup.cs*:

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

В примере приложения `UseHealthChecks` добавляет ПО промежуточного слоя в `Startup.Configure`.

*DbContextHealthStartup.cs*:

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_Configure)]

Прежде чем запускать сценарий проверки `DbContext` с помощью примера приложения, убедитесь, что база данных, указанная в строке подключения, не существует в экземпляре SQL Server. Если база данных существует, удалите ее.

Выполните следующую команду в папке проекта в командной строке:

```console
dotnet run --scenario dbcontext
```

После запуска приложения проверьте состояние работоспособности, выполнив запрос к конечной точке `/health` в браузере. Базы данных и `AppDbContext` не существуют, поэтому приложение дает следующий ответ:

```
Unhealthy
```

Заставьте пример приложения создать базу данных. Сделайте запрос к `/createdatabase`. Приложение выдает ответ:

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

Сделайте запрос к конечной точке `/health`. База данных и контекст существуют, поэтому приложение отвечает:

```
Healthy
```

Заставьте пример приложения удалить базу данных. Сделайте запрос к `/deletedatabase`. Приложение выдает ответ:

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

Сделайте запрос к конечной точке `/health`. Приложение предоставляет ответ о неработоспособности:

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a>Разделение проверок готовности и активности

В некоторых сценариях размещения используются пары проверок работоспособности, отличающие два состояния приложения:

* Приложение работает, но еще не готово к получению запросов. Это состояние *готовности* приложения.
* Приложение работает и отвечает на запросы. Это состояние *жизнеспособности* приложения.

Проверка готовности обычно выполняет ряд проверок, чтобы определить, все ли подсистемы и ресурсы приложения доступны; она более обширна и требует много времени. Проверка жизнеспособности лишь быстро определяет, доступно ли приложение для обработки запросов. По прохождении проверки готовности приложения нет необходимости создавать чрезмерную нагрузку на приложение с дорогостоящим набором проверок готовности&mdash;дальнейшие проверки требуют лишь проверки жизнеспособности.

Пример приложения содержит проверку работоспособности, которая сообщает о завершении длительной задачи запуска [размещенной службы](xref:fundamentals/host/hosted-services). `StartupHostedServiceHealthCheck` предоставляет свойство, `StartupTaskCompleted`, которое размещенная служба может задать как `true` после завершения длительной задачи (*StartupHostedServiceHealthCheck.cs*):

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

Длительная фоновая задача запущена [размещенной службой](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*). По завершении задачи `StartupHostedServiceHealthCheck.StartupTaskCompleted` получает значение `true`:

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

Проверка работоспособности регистрируется через <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> в `Startup.ConfigureServices` вместе с размещенной службой. Поскольку размещенной службе необходимо задать свойство проверки работоспособности, проверка работоспособности также регистрируется в контейнере службы (*LivenessProbeStartup.cs*):

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

Вызовите ПО промежуточного слоя для проверки работоспособности в конвейере обработки приложения в `Startup.Configure`. В примере приложения создаются конечные точки проверки работоспособности в `/health/ready` для проверки готовности и в `/health/live` для проверки жизнеспособности. Проверка готовности фильтрует проверки работоспособности по тегу `ready`. Проверка жизнеспособности отфильтровывает `StartupHostedServiceHealthCheck`, возвращая `false` в [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (дополнительные сведения см. в разделе [Фильтрация проверок работоспособности](#filter-health-checks)):

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_Configure)]

Чтобы запустить сценарий конфигурации проверок готовности и жизнеспособности с помощью примера приложения, выполните следующую команду из папки проекта в командной строке:

```console
dotnet run --scenario liveness
```

В браузере посетите `/health/ready` несколько раз, пока не пройдет 15 секунд. Проверка работоспособности будет передавать состояние *Unhealthy* первые 15 секунд. По истечении 15 секунд конечная точка передает состояние *Healthy*, что означает завершение длительной задачи размещенной службой.

В этом примере также создается издатель проверки работоспособности (реализация <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>), который выполнит первую проверку готовности с задержкой в две секунды. Дополнительные сведения см. в разделе [об издателе проверки работоспособности](#health-check-publisher).

### <a name="kubernetes-example"></a>Пример для Kubernetes

Использование отдельных проверок готовности и жизнеспособности полезно в таких средах, как [Kubernetes](https://kubernetes.io/). В Kubernetes приложениям может потребоваться долго выполнять задачи во время запуска, прежде чем начать принимать запросы; например, проверить доступность основной базы данных. Использование отдельных проверок позволяет оркестратору различать, когда приложение работает, но еще не готово, а когда приложение не удалось запустить. Дополнительные сведения о проверках готовности и жизнеспособности в Kubernetes см. в разделе [Настройка проверок готовности и жизнеспособности](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) в документации по Kubernetes.

В следующем примере показана конфигурация проверки готовности для Kubernetes:

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

## <a name="metric-based-probe-with-a-custom-response-writer"></a>Проба на основе метрики с настраиваемым модулем записи ответов

Этот пример демонстрирует проверку работоспособности памяти с настраиваемым модулем записи ответов.

Если приложение использует больше заданного порогового значения памяти (1 ГБ в примере приложения), `MemoryHealthCheck` сообщает о состоянии пониженной функциональности. <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> включает сведения от сборщика мусора для приложения (*MemoryHealthCheck.cs*):

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

Зарегистрируйте службы проверки работоспособности при помощи <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> в `Startup.ConfigureServices`. Вместо того чтобы включать проверку работоспособности, передав ее в <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, `MemoryHealthCheck` регистрируется в качестве службы. Все зарегистрированные службы <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> доступны в службах проверки работоспособности и ПО промежуточного слоя. Мы рекомендуем регистрировать службы проверки работоспособности в качестве одноэлементных служб.

*CustomWriterStartup.cs*:

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

Вызовите ПО промежуточного слоя для проверки работоспособности в конвейере обработки приложения в `Startup.Configure`. Делегат `WriteResponse` передается в свойство `ResponseWriter` для вывода настраиваемого ответа JSON при выполнении проверки работоспособности:

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_Configure&highlight=6)]

Метод `WriteResponse` форматирует `CompositeHealthCheckResult` в объект JSON и выдает выходные данные JSON в качестве ответа проверки работоспособности:

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

Чтобы запустить проверку метрики с настраиваемым модулем записи ответа с помощью примера приложения, выполните следующую команду из папки проекта в командной строке:

```console
dotnet run --scenario writer
```

> [!NOTE]
> [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) содержит сценарии для проверки работоспособности на основе метрик, включая проверки жизнеспособности для дискового хранилища и максимальных значений.
>
> [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) — это порт [BeatPulse](https://github.com/xabaril/beatpulse), и он не поддерживается корпорацией Майкрософт.

## <a name="filter-by-port"></a>Фильтр по портам

Вызов <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> с портом ограничивает запросы на проверку работоспособности указанным портом. Обычно это используется в контейнерной среде с целью предоставления порта для мониторинга служб.

Пример приложения настраивает порт с помощью [поставщика конфигурации переменных среды](xref:fundamentals/configuration/index#environment-variables-configuration-provider). Порт устанавливается в файле *launchSettings.json* и передается поставщику конфигурации с помощью переменной среды. Необходимо также настроить на сервере прослушивание запросов через порт управления.

Чтобы использовать пример приложения для демонстрации настройки портов управления, создайте файл *launchSettings.json* в папке *Properties*.

Следующий файл *launchSettings.json* не включен в файлы проекта примера приложения и должен быть создан вручную.

*Properties/launchSettings.json*:

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

Зарегистрируйте службы проверки работоспособности при помощи <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> в `Startup.ConfigureServices`. Вызов <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> задает порт управления (*ManagementPortStartup.cs*):

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ManagementPortStartup.cs?name=snippet1&highlight=12,18)]

> [!NOTE]
> Можно избежать создания файла *launchSettings.json* в примере приложения, явно указав URL-адрес и порт управления в коде. В *Program.cs*, где создается <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, добавьте вызов <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> и укажите обычную конечную точку ответа приложения и порт конечной точки управления. В *ManagementPortStartup.cs*, где вызывается <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*>, явно укажите порт управления.
>
> *Program.cs*:
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
> *ManagementPortStartup.cs*:
>
> ```csharp
> app.UseHealthChecks("/health", port: 5001);
> ```

Чтобы запустить сценарий с портом управления с помощью примера приложения, выполните следующую команду из папки проекта в командной строке:

```console
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a>Распространение библиотеки проверки работоспособности

Чтобы распространять проверки работоспособности в качестве библиотеки, выполните следующие действия.

1. Напишите проверку работоспособности, которая реализует интерфейс <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> как автономный класс. Класс может полагаться на [внедрение зависимостей (DI)](xref:fundamentals/dependency-injection), активацию типов и [именованные параметры](xref:fundamentals/configuration/options) для доступа к данным конфигурации.

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

1. Напишите метод расширения с параметрами, который приложение вызывает в своем методе `Startup.Configure`. В следующем примере предполагается следующая сигнатура метода проверки работоспособности:

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   Эта сигнатура указывает, что `ExampleHealthCheck` требуются дополнительные данные для обработки логики проверки работоспособности. Данные передаются делегату, используемому для создания экземпляра проверки работоспособности, когда проверка работоспособности регистрируется с помощью метода расширения. В следующем примере вызывающая сторона задает:

   * Необязательное имя проверки работоспособности (`name`). Если `null`, используется `example_health_check`.
   * Строковая точка данных для проверки работоспособности (`data1`).
   * Целочисленная точка данных для проверки работоспособности (`data2`). Если `null`, используется `1`.
   * состояние сбоя (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>). Значение по умолчанию — `null`. В случае `null` возвращается [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) как состояние сбоя.
   * Теги (`IEnumerable<string>`).

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

## <a name="health-check-publisher"></a>Издатель проверки работоспособности

Когда <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> добавляется в контейнер службы, система проверки работоспособности периодически выполняет ваши проверки работоспособности и вызывает `PublishAsync` с полученным результатом. Это полезно в ситуации принудительной передачи данных мониторинга работоспособности, когда ожидается, что каждый процесс периодически вызывает систему мониторинга для определения работоспособности.

Интерфейс <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> содержит один метод:

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> позволяет задать следующие параметры:

* <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> — начальная задержка, которая применяется после запуска приложения, но перед выполнением экземпляров <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>. Задержка применяется при запуске один раз и не распространяется на все последующие итерации. Значение по умолчанию — пять секунд.
* <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> — период выполнения <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>. Значение по умолчанию - 30 секунды.
* <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> — если <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> имеет значение `null` (по умолчанию), служба издателя проверки работоспособности выполняет все зарегистрированные проверки работоспособности. Чтобы выполнить ряд проверок работоспособности, укажите функцию, которая отфильтровывает нужный набор. Предикат вычисляется в каждом периоде.
* <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> — время ожидания до выполнения проверок работоспособности для всех экземпляров <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>. Используйте <xref:System.Threading.Timeout.InfiniteTimeSpan>, чтобы выполнить проверки без задержки. Значение по умолчанию - 30 секунды.

::: moniker range="= aspnetcore-2.2"

> [!WARNING]
> В выпуске ASP.NET Core 2.2 значение <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> не поддерживается в реализации <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>. Здесь задается значение <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>. Эта проблема будет устранена в выпуске ASP.NET Core 3.0. Дополнительные сведения см. в описании проблемы [HealthCheckPublisherOptions.Period sets the value of .Delay](https://github.com/aspnet/Extensions/issues/1041) (HealthCheckPublisherOptions.Period задает значение свойству .Delay).

::: moniker-end

В примере приложения `ReadinessPublisher` является реализацией <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>. Состояния проверки работоспособности записывается в `Entries` и в журнал для каждой проверки:

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=20,22-23)]

В примере приложения `LivenessProbeStartup` проверка готовности `StartupHostedService` использует задержку запуска в две секунды и выполняет проверку каждые 30 секунд. Чтобы активировать реализацию <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>, этот пример регистрирует `ReadinessPublisher` как отдельную службу в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection).

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices&highlight=12-17,28)]

::: moniker range="= aspnetcore-2.2"

> [!NOTE]
> Следующий вариант позволяет добавлять экземпляр <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> в контейнер службы, когда один или несколько других размещенных служб уже добавлены в приложение. Этот способ не нужно использовать в выпуске ASP.NET Core 3.0. Дополнительные сведения см. на странице https://github.com/aspnet/Extensions/issues/639.
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
> [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) содержит издатели для нескольких систем, в том числе [Application Insights](/azure/application-insights/app-insights-overview).
>
> [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) — это порт [BeatPulse](https://github.com/xabaril/beatpulse), и он не поддерживается корпорацией Майкрософт.
