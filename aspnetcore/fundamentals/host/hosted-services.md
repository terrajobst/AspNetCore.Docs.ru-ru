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
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a>Фоновые задачи с размещенными службами в ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

::: moniker range=">= aspnetcore-3.0"

В ASP.NET Core фоновые задачи реализуются как *размещенные службы*. Размещенная служба — это класс с логикой фоновой задачи, реализующий интерфейс <xref:Microsoft.Extensions.Hosting.IHostedService>. Этот раздел содержит три примера размещенных служб:

* Фоновая задача, которая выполняется по таймеру.
* Размещенная служба, которая активирует [службу с заданной областью](xref:fundamentals/dependency-injection#service-lifetimes). Служба с заданной областью может использовать [внедрение зависимостей](xref:fundamentals/dependency-injection).
* Очередь фоновых задач, которые выполняются последовательно.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([как скачивать](xref:index#how-to-download-a-sample))

Пример приложения предоставляется в двух версиях:

* Веб-узел &ndash; удобно использовать для размещения веб-приложений. Пример кода, приведенный в этой статье, относится к этой версии примера приложения. Дополнительные сведения см. в статье о [веб-узле](xref:fundamentals/host/web-host).
* Универсальный узел &ndash; — новая возможность в ASP.NET Core 2.1. Дополнительные сведения см. в статье об [универсальном узле](xref:fundamentals/host/generic-host).

## <a name="worker-service-template"></a>Шаблон службы рабочей роли

Шаблон службы рабочей роли ASP.NET Core может служить отправной точкой для написания длительно выполняющихся приложений служб. Чтобы использовать шаблон в качестве основы для приложения размещенных служб, выполните указанные ниже действия.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Создайте новый проект.
1. Выберите **Новое веб-приложение ASP.NET Core**. Выберите **Далее**.
1. В поле **Имя проекта** укажите имя проекта или оставьте имя по умолчанию. Выберите **Создать**.
1. В диалоговом окне **Создание веб-приложения ASP.NET Core** убедитесь в том, что выбраны платформы **.NET Core** и **ASP.NET Core 3.0**.
1. Выберите шаблон **Служба рабочей роли**. Выберите **Создать**.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

1. Создайте новый проект.
1. В разделе **.NET Core** на боковой панели выберите **Приложение**.
1. В разделе **ASP.NET Core** выберите **Рабочая роль**. Выберите **Далее**.
1. Для параметра **Требуемая версия .NET Framework** выберите **.NET Core 3.0**. Выберите **Далее**.
1. Введите имя в поле **Имя проекта**. Выберите **Создать**.

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

Используйте шаблон службы рабочей роли (`worker`) с командой [dotnet new](/dotnet/core/tools/dotnet-new) из командной оболочки. В приведенном ниже примере создается приложение службы рабочей роли с именем `ContosoWorker`. Папка для приложения `ContosoWorker` создается автоматически при выполнении команды.

```dotnetcli
dotnet new worker -o ContosoWorker
```

---

## <a name="package"></a>Пакет

Ссылка на пакет [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) добавляется неявно для приложений ASP.NET Core.

## <a name="ihostedservice-interface"></a>Интерфейс IHostedService

Интерфейс <xref:Microsoft.Extensions.Hosting.IHostedService> определяет два метода для объектов, которые управляются узлом:

* [StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` содержит логику для запуска фоновой задачи. *Первым* вызывается `StartAsync`:

  * Настраивается конвейер обработки запросов приложения (`Startup.Configure`).
  * Запускается сервер и активируется [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*).

  Поведение по умолчанию можно изменить таким образом, чтобы `StartAsync` размещенной службы выполнялся после настройки конвейера приложения и вызова `ApplicationStarted`. Чтобы изменить поведение по умолчанию, добавьте размещенную службу (`VideosWatcher` в следующем примере) после вызова `ConfigureWebHostDefaults`:

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

* [StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; запускается, когда происходит нормальное завершение работы узла. `StopAsync` содержит логику для завершения фоновой задачи. Реализуйте <xref:System.IDisposable> и [методы завершения (деструкторы)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) для освобождения неуправляемых ресурсов.

  Токен отмены использует заданное по умолчанию 5-секундное время ожидания, указывающее, что процесс завершения работы больше не должен быть нормальным. При запросе отмены происходит следующее:

  * должны быть прерваны все оставшиеся фоновые операции, выполняемые приложением;
  * должны быть незамедлительно возвращены все методы, вызываемые в `StopAsync`.

  Однако после запроса отмены выполнение задач не прекращается &mdash; вызывающий объект ожидает завершения всех задач.

  Если приложение завершает работу неожиданно (например, при сбое процесса приложения), `StopAsync` может не вызываться. Поэтому вызов методов или выполнение операций в `StopAsync` может быть невозможным.

  Чтобы увеличить время ожидания завершения работы по умолчанию (пять секунд), установите следующие значения:

  * <xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> при использовании универсального узла. Дополнительные сведения можно найти по адресу: <xref:fundamentals/host/generic-host#shutdown-timeout>.
  * Параметр конфигурации узла для времени ожидания завершения работы при использовании веб-узла. Дополнительные сведения можно найти по адресу: <xref:fundamentals/host/web-host#shutdown-timeout>.

Размещенная служба активируется при запуске приложения и нормально завершает работу при завершении работы приложения. Если во время выполнения задачи в фоновом режиме возникает ошибка, необходимо вызвать `Dispose`, даже если `StopAsync` не вызывается.

## <a name="backgroundservice"></a>BackgroundService

`BackgroundService` — это базовый класс для реализации долго выполняющегося интерфейса <xref:Microsoft.Extensions.Hosting.IHostedService>. `BackgroundService` определяет два метода для фоновых операций:

* При запуске <xref:Microsoft.Extensions.Hosting.IHostedService> вызывается `ExecuteAsync(CancellationToken stoppingToken)` &ndash; `ExecuteAsync`. Реализация должна возвращать значение `Task`, представляющее время существования длительно выполняемых операций. `stoppingToken` активируется при вызове [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*).
* `StopAsync(CancellationToken stoppingToken)` &ndash; `StopAsync` запускается, когда происходит нормальное завершение работы узла приложения. `stoppingToken` указывает, что процесс завершения работы больше не должен быть корректным.

## <a name="timed-background-tasks"></a>Фоновые задачи с заданным временем

Для фоновых задач с заданным временем используется класс [System.Threading.Timer](xref:System.Threading.Timer). Таймер запускает метод `DoWork` задачи. Таймер отключается методом `StopAsync` и удаляется при удалении контейнера службы методом `Dispose`:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

Служба регистрируется в `IHostBuilder.ConfigureServices` (*Program.cs*) с использованием метода расширения `AddHostedService`:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Использование службы с заданной областью в фоновой задаче

Чтобы использовать [службы с заданной областью](xref:fundamentals/dependency-injection#service-lifetimes) в `BackgroundService`, создайте область. Для размещенной службы по умолчанию не создается область.

Служба фоновой задачи с заданной областью содержит логику фоновой задачи. В следующем примере:

* Служба является асинхронной. Метод `DoWork` возвращает значение `Task`. В демонстрационных целях в методе `DoWork` ожидается задержка в десять секунд.
* В службу вставляется <xref:Microsoft.Extensions.Logging.ILogger>.

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

Размещенная служба создает область для разрешения службы фоновой задачи с заданной областью, чтобы вызвать ее метод `DoWork`: `DoWork` возвращает объект `Task`, ожидаемый в `ExecuteAsync`:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

Службы регистрируются в `IHostBuilder.ConfigureServices` (*Program.cs*). Размещенная служба регистрируется с использованием метода расширения `AddHostedService`:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>Фоновые задачи в очереди

Очередь фоновых задач основывается на методе <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> в .NET 4.x ([предварительно запланировано встроить в ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

В следующем примере `QueueHostedService`:

* Метод `BackgroundProcessing` возвращает объект `Task`, ожидаемый в `ExecuteAsync`:
* Фоновые задачи в очереди выводятся из очереди и выполняются в `BackgroundProcessing`:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28,39-40,44)]

Служба `MonitorLoop` обрабатывает задачи постановки в очередь для размещенной службы при выборе на устройстве ввода ключа `w`:

* В службу `MonitorLoop` внедряется `IBackgroundTaskQueue`.
* `IBackgroundTaskQueue.QueueBackgroundWorkItem` вызывается для постановки рабочего элемента в очередь:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

Службы регистрируются в `IHostBuilder.ConfigureServices` (*Program.cs*). Размещенная служба регистрируется с использованием метода расширения `AddHostedService`:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

В ASP.NET Core фоновые задачи реализуются как *размещенные службы*. Размещенная служба — это класс с логикой фоновой задачи, реализующий интерфейс <xref:Microsoft.Extensions.Hosting.IHostedService>. Этот раздел содержит три примера размещенных служб:

* Фоновая задача, которая выполняется по таймеру.
* Размещенная служба, которая активирует [службу с заданной областью](xref:fundamentals/dependency-injection#service-lifetimes). Служба с заданной областью может использовать [внедрение зависимостей](xref:fundamentals/dependency-injection).
* Очередь фоновых задач, которые выполняются последовательно.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([как скачивать](xref:index#how-to-download-a-sample))

Пример приложения предоставляется в двух версиях:

* Веб-узел &ndash; удобно использовать для размещения веб-приложений. Пример кода, приведенный в этой статье, относится к этой версии примера приложения. Дополнительные сведения см. в статье о [веб-узле](xref:fundamentals/host/web-host).
* Универсальный узел &ndash; — новая возможность в ASP.NET Core 2.1. Дополнительные сведения см. в статье об [универсальном узле](xref:fundamentals/host/generic-host).

## <a name="package"></a>Пакет

Добавьте ссылку на [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) или добавьте ссылку на пакет в пакет [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting).

## <a name="ihostedservice-interface"></a>Интерфейс IHostedService

Размещенные службы реализуют интерфейс <xref:Microsoft.Extensions.Hosting.IHostedService>. Этот интерфейс определяет два метода для объектов, которые управляются узлом:

* [StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` содержит логику для запуска фоновой задачи. При использовании [Web Host](xref:fundamentals/host/web-host) `StartAsync` вызывается после запуска сервера и активации [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*). При использовании [универсального узла](xref:fundamentals/host/generic-host) `StartAsync` вызывается до активации `ApplicationStarted`.

* [StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; запускается, когда происходит нормальное завершение работы узла. `StopAsync` содержит логику для завершения фоновой задачи. Реализуйте <xref:System.IDisposable> и [методы завершения (деструкторы)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) для освобождения неуправляемых ресурсов.

  Токен отмены использует заданное по умолчанию 5-секундное время ожидания, указывающее, что процесс завершения работы больше не должен быть нормальным. При запросе отмены происходит следующее:

  * должны быть прерваны все оставшиеся фоновые операции, выполняемые приложением;
  * должны быть незамедлительно возвращены все методы, вызываемые в `StopAsync`.

  Однако после запроса отмены выполнение задач не прекращается &mdash; вызывающий объект ожидает завершения всех задач.

  Если приложение завершает работу неожиданно (например, при сбое процесса приложения), `StopAsync` может не вызываться. Поэтому вызов методов или выполнение операций в `StopAsync` может быть невозможным.

  Чтобы увеличить время ожидания завершения работы по умолчанию (пять секунд), установите следующие значения:

  * <xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> при использовании универсального узла. Дополнительные сведения можно найти по адресу: <xref:fundamentals/host/generic-host#shutdown-timeout>.
  * Параметр конфигурации узла для времени ожидания завершения работы при использовании веб-узла. Дополнительные сведения можно найти по адресу: <xref:fundamentals/host/web-host#shutdown-timeout>.

Размещенная служба активируется при запуске приложения и нормально завершает работу при завершении работы приложения. Если во время выполнения задачи в фоновом режиме возникает ошибка, необходимо вызвать `Dispose`, даже если `StopAsync` не вызывается.

## <a name="timed-background-tasks"></a>Фоновые задачи с заданным временем

Для фоновых задач с заданным временем используется класс [System.Threading.Timer](xref:System.Threading.Timer). Таймер запускает метод `DoWork` задачи. Таймер отключается методом `StopAsync` и удаляется при удалении контейнера службы методом `Dispose`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

Служба зарегистрирована в `Startup.ConfigureServices` с методом расширения `AddHostedService`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Использование службы с заданной областью в фоновой задаче

Чтобы использовать [службы с заданной областью](xref:fundamentals/dependency-injection#service-lifetimes) в `IHostedService`, создайте область. Для размещенной службы по умолчанию не создается область.

Служба фоновой задачи с заданной областью содержит логику фоновой задачи. В следующем примере в службу вставляется <xref:Microsoft.Extensions.Logging.ILogger>:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

Размещенная служба создает область для разрешения службы фоновой задачи с заданной областью, чтобы вызвать ее метод `DoWork`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

Службы регистрируются в `Startup.ConfigureServices`. Реализация `IHostedService` зарегистрирована с методом расширения `AddHostedService`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>Фоновые задачи в очереди

Очередь фоновых задач основывается на методе <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> в .NET 4.x ([предварительно запланировано встроить в ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

В `QueueHostedService` фоновые задачи в очереди из очереди выводятся из очереди и выполняются в качестве <xref:Microsoft.Extensions.Hosting.BackgroundService> — базового класса для реализации длительного выполнения `IHostedService`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

Службы регистрируются в `Startup.ConfigureServices`. Реализация `IHostedService` зарегистрирована с методом расширения `AddHostedService`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

В классе модели страницы индексов:

* `IBackgroundTaskQueue` вставляется в конструктор и присваивается `Queue`.
* <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> вставляется и присваивается `_serviceScopeFactory`. Фабрика используется для создания экземпляров <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, который используется для создания служб с заданной областью. Для использования `AppDbContext` приложения создается область ([служба с заданной областью](xref:fundamentals/dependency-injection#service-lifetimes)) для записи данных из базы данных в `IBackgroundTaskQueue` (отдельная служба).

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

Если нажать кнопку **Добавить задачу** на странице индексов, выполняется метод `OnPostAddTask`. `QueueBackgroundWorkItem` вызывается для постановки рабочего элемента в очередь:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Реализация фоновых задач в микрослужбах с помощью IHostedService и класса BackgroundService](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
