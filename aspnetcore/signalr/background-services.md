---
title: Узел ASP.NET Core SignalR в фоновых служб
author: bradygaster
description: Узнайте, как для отправки сообщений SignalR клиентам из классов .NET Core BackgroundService.
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/04/2019
uid: signalr/background-services
ms.openlocfilehash: b359bd7f6b0667aeb8d9c8f5eb450637b1347b19
ms.sourcegitcommit: e418cb9cddeb3de06fa0cb4fdb5529da03ff6d63
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/05/2019
ms.locfileid: "55739674"
---
# <a name="host-aspnet-core-signalr-in-background-services"></a>Узел ASP.NET Core SignalR в фоновых служб

По [Brady Gaster](https://twitter.com/bradygaster)

Эта статья содержит рекомендации для:

* Размещение концентраторы SignalR с помощью фона рабочего процесса, размещенного с помощью ASP.NET Core.
* Отправка сообщений подключенных клиентов из в .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(способ загрузки)](xref:index#how-to-download-a-sample)

## <a name="wire-up-signalr-during-startup"></a>Подключения SignalR во время запуска

Размещение ASP.NET Core SignalR концентраторов в контексте фоновый рабочий процесс идентичен размещение концентратора в веб-приложение ASP.NET Core. В `Startup.ConfigureServices` метода, вызывая метод `services.AddSignalR` добавляет необходимые службы на уровень внедрения зависимостей ASP.NET Core (DI) для поддержки SignalR. В `Startup.Configure`, `UseSignalR` вызывается метод пишем конечные концентратора в конвейере запросов ASP.NET Core.

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

В приведенном выше примере `ClockHub` класс реализует `Hub<T>` класс для создания строго типизированных центра. `ClockHub` Был настроен в `Startup` класс отвечать на запросы в конечной точке `/hubs/clock`.

Дополнительные сведения о строго типизированных концентраторах см. в разделе [используют концентраторы в SignalR для ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).

> [!NOTE]
> Эта функция не ограничивается [концентратора\<T >](xref:Microsoft.AspNetCore.SignalR.Hub`1) класса. Любой класс, наследуемый от [концентратора](xref:Microsoft.AspNetCore.SignalR.Hub), такие как [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), также будет работать.

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

Интерфейс, через строго типизированные `ClockHub` является `IClock` интерфейс.

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-signalr-hub-from-a-background-service"></a>Вызов из фоновую службу концентратора SignalR

Во время запуска `Worker` класс, `BackgroundService`, построенная с помощью `AddHostedService`.

```csharp
services.AddHostedService<Worker>();
```

Поскольку SignalR также уже будет устроено во время `Startup` этап, в которой каждый центр будет подключен к отдельной конечной точки в ASP.NET Core конвейер HTTP-запросов, представлены в каждом центре `IHubContext<T>` на сервере. С помощью ASP.NET Core DI функции, другие классы, создается путем размещения слоя, например `BackgroundService` классы, классы контроллера MVC или модели страницы Razor, можно получить ссылки для концентраторов на стороне сервера, принимая экземпляров `IHubContext<ClockHub, IClock>` во время построения.

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

Как `ExecuteAsync` метод вызывается многократно в фоновой службы, текущую дату и время сервера отправляются к подключенным клиентам, использующим `ClockHub`.

## <a name="react-to-signalr-events-with-background-services"></a>Реагирование на события SignalR в работе фоновых служб

Как одностраничное приложение с помощью клиента JavaScript для SignalR или классическое приложение .NET можно сделать с помощью <xref:signalr/dotnet-client>, `BackgroundService` или `IHostedService` реализации может также использоваться для подключения к концентраторов SignalR и реагировать на события.

`ClockHubClient` Класс реализует оба `IClock` интерфейс и `IHostedService` интерфейс. Таким образом, его можно регистрировать во время `Startup` в непрерывном режиме и реагировать на события концентратора с сервера. 

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

Во время инициализации `ClockHubClient` создает экземпляр класса `HubConnection` и настраивающий `IClock.ShowTime` метода как обработчика для центра `ShowTime` событий.

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

В `IHostedService.StartAsync` реализации `HubConnection` запускается в асинхронном режиме.

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

Во время `IHostedService.StopAsync` метода `HubConnection` удаляется асинхронно.

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Начало работы](xref:tutorials/signalr)
* [Центры](xref:signalr/hubs)
* [Публикация в Azure](xref:signalr/publish-to-azure-web-app)
* [Строго типизированные концентраторов](xref:signalr/hubs#strongly-typed-hubs)
