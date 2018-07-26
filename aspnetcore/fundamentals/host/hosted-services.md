---
title: Фоновые задачи с размещенными службами в ASP.NET Core
author: guardrex
description: Узнайте, как реализовать фоновые задачи с размещенными службами в ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 087ff4e1e169e1a1f76e93d4993441e47bafc945
ms.sourcegitcommit: 7097dba14d5b858e82758ee031ac62dbe3611339
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/19/2018
ms.locfileid: "39138601"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a>Фоновые задачи с размещенными службами в ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

В ASP.NET Core фоновые задачи реализуются как *размещенные службы*. Размещенная служба — это класс с логикой фоновой задачи, реализующий интерфейс [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice). Этот раздел содержит три примера размещенных служб:

* Фоновая задача, которая выполняется по таймеру.
* Размещенная служба, которая активирует службу с заданной областью. Служба с заданной областью может использовать внедрение зависимостей.
* Очередь фоновых задач, которые выполняются последовательно.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

Пример приложения предоставляется в двух версиях:

* Веб-узел &ndash; удобно использовать для размещения веб-приложений. Пример кода, приведенный в этой статье, относится к этой версии примера приложения. Дополнительные сведения см. в статье о [веб-узле](xref:fundamentals/host/web-host).
* Универсальный узел &ndash; — новая возможность в ASP.NET Core 2.1. Дополнительные сведения см. в статье об [универсальном узле](xref:fundamentals/host/generic-host).

## <a name="ihostedservice-interface"></a>Интерфейс IHostedService

Размещенные службы реализуют интерфейс [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice). Этот интерфейс определяет два метода для объектов, которые управляются узлом:

* [StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) — вызывается после запуска сервера и активации [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted). `StartAsync` содержит логику для запуска фоновой задачи.

* [StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) — запускается, когда происходит нормальное завершение работы узла. `StopAsync` содержит логику для завершения фоновой задачи и удаления неуправляемых ресурсов. Если приложение завершает работу неожиданно (например, при сбое процесса приложения), `StopAsync` может не вызываться.

Размещенная служба активируется при запуске приложения и корректно завершает работу при завершении работы приложения. Если реализуется [IDisposable](/dotnet/api/system.idisposable), ресурсы можно удалить при удалении контейнера службы. Если во время выполнения задачи в фоновом режиме возникает ошибка, необходимо вызвать `Dispose`, даже если `StopAsync` не вызывается.

## <a name="timed-background-tasks"></a>Фоновые задачи с заданным временем

Для фоновых задач с заданным временем используется класс [System.Threading.Timer](/dotnet/api/system.threading.timer). Таймер запускает метод `DoWork` задачи. Таймер отключается методом `StopAsync` и удаляется при удалении контейнера службы методом `Dispose`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

Служба зарегистрирована в `Startup.ConfigureServices` с методом расширения `AddHostedService`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Использование службы с заданной областью в фоновой задаче

Чтобы использовать службы с заданной областью в `IHostedService`, создайте область. Для размещенной службы по умолчанию не создается область.

Служба фоновой задачи с заданной областью содержит логику фоновой задачи. В следующем примере в службу вставляется [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger):

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

Размещенная служба создает область для разрешения службы фоновой задачи с заданной областью, чтобы вызвать ее метод `DoWork`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

Службы регистрируются в `Startup.ConfigureServices`. Реализация `IHostedService` зарегистрирована с методом расширения `AddHostedService`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>Фоновые задачи в очереди

Очередь фоновых задач основывается на методе [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) в .NET 4.x ([предварительно запланирована встройка в ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

В `QueueHostedService` фоновые задачи в очереди из очереди выводятся из очереди и выполняются в качестве [BackgroundService](/dotnet/api/microsoft.extensions.hosting.backgroundservice) — базового класса для реализации длительного выполнения `IHostedService`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=16,20)]

Службы регистрируются в `Startup.ConfigureServices`. Реализация `IHostedService` зарегистрирована с методом расширения `AddHostedService`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

В классе модели страницы индексов `IBackgroundTaskQueue` вставляется в конструктор и присваивается `Queue`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

Если нажать кнопку **Добавить задачу** на странице индексов, выполняется метод `OnPostAddTask`. `QueueBackgroundWorkItem` вызывается для постановки рабочего элемента в очередь:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Реализация фоновых задач в микрослужбах с помощью IHostedService и класса BackgroundService](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [System.Threading.Timer](/dotnet/api/system.threading.timer)
