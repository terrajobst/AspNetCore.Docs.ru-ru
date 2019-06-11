---
title: Фоновые задачи с размещенными службами в ASP.NET Core
author: guardrex
description: Узнайте, как реализовать фоновые задачи с размещенными службами в ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/03/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 2dbb1a84a380ab06a4be7ecf628799a070afc9e3
ms.sourcegitcommit: 5dd2ce9709c9e41142771e652d1a4bd0b5248cec
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2019
ms.locfileid: "66692512"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a>Фоновые задачи с размещенными службами в ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

В ASP.NET Core фоновые задачи реализуются как *размещенные службы*. Размещенная служба — это класс с логикой фоновой задачи, реализующий интерфейс <xref:Microsoft.Extensions.Hosting.IHostedService>. Этот раздел содержит три примера размещенных служб:

* Фоновая задача, которая выполняется по таймеру.
* Размещенная служба, которая активирует [службу с заданной областью](xref:fundamentals/dependency-injection#service-lifetimes). Служба с заданной областью может использовать внедрение зависимостей.
* Очередь фоновых задач, которые выполняются последовательно.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([как скачивать](xref:index#how-to-download-a-sample))

Пример приложения предоставляется в двух версиях:

* Веб-узел &ndash; удобно использовать для размещения веб-приложений. Пример кода, приведенный в этой статье, относится к этой версии примера приложения. Дополнительные сведения см. в статье о [веб-узле](xref:fundamentals/host/web-host).
* Универсальный узел &ndash; — новая возможность в ASP.NET Core 2.1. Дополнительные сведения см. в статье об [универсальном узле](xref:fundamentals/host/generic-host).

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a>Шаблон службы рабочей роли

Шаблон службы рабочей роли ASP.NET Core может служить отправной точкой для написания длительно выполняющихся приложений служб. Чтобы использовать шаблон в качестве основы для приложения размещенных служб, выполните указанные ниже действия.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Создайте новый проект.
1. Выберите **Новое веб-приложение ASP.NET Core**. Выберите **Далее**.
1. В поле **Имя проекта** укажите имя проекта или оставьте имя по умолчанию. Выберите **Создать**.
1. В диалоговом окне **Создание веб-приложения ASP.NET Core** убедитесь в том, что выбраны платформы **.NET Core** и **ASP.NET Core 3.0**.
1. Выберите шаблон **Служба рабочей роли**. Выберите **Создать**.

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[Visual Studio Code или .NET Core CLI](#tab/visual-studio-code+netcore-cli)

Используйте шаблон службы рабочей роли (`worker`) с командой [dotnet new](/dotnet/core/tools/dotnet-new) из командной оболочки. В приведенном ниже примере создается приложение службы рабочей роли с именем `ContosoWorkerService`. Папка для приложения `ContosoWorkerService` создается автоматически при выполнении команды.

```console
dotnet new worker -o ContosoWorkerService
```

---

::: moniker-end

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

  * <xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> при использовании универсального узла. Для получения дополнительной информации см. <xref:fundamentals/host/generic-host#shutdown-timeout>.
  * Параметр конфигурации узла для времени ожидания завершения работы при использовании веб-узла. Для получения дополнительной информации см. <xref:fundamentals/host/web-host#shutdown-timeout>.

Размещенная служба активируется при запуске приложения и нормально завершает работу при завершении работы приложения. Если во время выполнения задачи в фоновом режиме возникает ошибка, необходимо вызвать `Dispose`, даже если `StopAsync` не вызывается.

## <a name="timed-background-tasks"></a>Фоновые задачи с заданным временем

Для фоновых задач с заданным временем используется класс [System.Threading.Timer](xref:System.Threading.Timer). Таймер запускает метод `DoWork` задачи. Таймер отключается методом `StopAsync` и удаляется при удалении контейнера службы методом `Dispose`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

Служба зарегистрирована в `Startup.ConfigureServices` с методом расширения `AddHostedService`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Использование службы с заданной областью в фоновой задаче

Чтобы использовать [службы с заданной областью](xref:fundamentals/dependency-injection#service-lifetimes) в `IHostedService`, создайте область. Для размещенной службы по умолчанию не создается область.

Служба фоновой задачи с заданной областью содержит логику фоновой задачи. В следующем примере в службу вставляется <xref:Microsoft.Extensions.Logging.ILogger>:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

Размещенная служба создает область для разрешения службы фоновой задачи с заданной областью, чтобы вызвать ее метод `DoWork`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

Службы регистрируются в `Startup.ConfigureServices`. Реализация `IHostedService` зарегистрирована с методом расширения `AddHostedService`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>Фоновые задачи в очереди

Очередь фоновых задач основывается на методе <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> в .NET 4.x ([предварительно запланировано встроить в ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

В `QueueHostedService` фоновые задачи в очереди из очереди выводятся из очереди и выполняются в качестве <xref:Microsoft.Extensions.Hosting.BackgroundService> — базового класса для реализации длительного выполнения `IHostedService`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

Службы регистрируются в `Startup.ConfigureServices`. Реализация `IHostedService` зарегистрирована с методом расширения `AddHostedService`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

В классе модели страницы индексов:

* `IBackgroundTaskQueue` вставляется в конструктор и присваивается `Queue`.
* <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> вставляется и присваивается `_serviceScopeFactory`. Фабрика используется для создания экземпляров <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, который используется для создания служб с заданной областью. Для использования `AppDbContext` приложения создается область ([служба с заданной областью](xref:fundamentals/dependency-injection#service-lifetimes)) для записи данных из базы данных в `IBackgroundTaskQueue` (отдельная служба).

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

Если нажать кнопку **Добавить задачу** на странице индексов, выполняется метод `OnPostAddTask`. `QueueBackgroundWorkItem` вызывается для постановки рабочего элемента в очередь:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Реализация фоновых задач в микрослужбах с помощью IHostedService и класса BackgroundService](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
