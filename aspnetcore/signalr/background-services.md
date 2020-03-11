---
title: SignalR ASP.NET Core узла в фоновых службах
author: bradygaster
description: Узнайте, как отправить сообщения SignalR клиентам из классов .NET Core BackgroundService.
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/background-services
ms.openlocfilehash: 86319cc93febab18c29e2fb6366cef0d025943ba
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78651586"
---
# <a name="host-aspnet-core-opno-locsignalr-in-background-services"></a>SignalR ASP.NET Core узла в фоновых службах

По [Брейди Гастер](https://twitter.com/bradygaster)

В этой статье приводятся рекомендации по следующим вопросам.

* Размещение концентраторов SignalR с помощью фонового рабочего процесса, размещенного в ASP.NET Core.
* Отправка сообщений на подключенные клиенты из [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).NET Core.

[Просмотр или скачивание образца кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(Загрузка)](xref:index#how-to-download-a-sample)

## <a name="enable-opno-locsignalr-in-startup"></a>Включение SignalR при запуске

::: moniker range=">= aspnetcore-3.0"

Размещение ASP.NET Core SignalR концентраторов в контексте фонового рабочего процесса идентично размещению концентратора в ASP.NET Core веб-приложении. В методе `Startup.ConfigureServices` вызов `services.AddSignalR` добавляет необходимые службы к слою ASP.NET Core внедрения зависимостей (DI) для поддержки SignalR. В `Startup.Configure`метод `MapHub` вызывается в обратном вызове `UseEndpoints` для подключения конечных точек концентратора в конвейере запросов ASP.NET Core.

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSignalR();
        services.AddHostedService<Worker>();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }

        app.UseRouting();
        app.UseEndpoints(endpoints =>
        {
            endpoints.MapHub<ClockHub>("/hubs/clock");
        });
    }
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Размещение ASP.NET Core SignalR концентраторов в контексте фонового рабочего процесса идентично размещению концентратора в ASP.NET Core веб-приложении. В методе `Startup.ConfigureServices` вызов `services.AddSignalR` добавляет необходимые службы к слою ASP.NET Core внедрения зависимостей (DI) для поддержки SignalR. В `Startup.Configure`вызывается метод `UseSignalR` для подключения конечных точек концентратора в конвейере запросов ASP.NET Core.

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

::: moniker-end

В предыдущем примере класс `ClockHub` реализует класс `Hub<T>` для создания строго типизированного концентратора. `ClockHub` был настроен в классе `Startup` для реагирования на запросы на конечной точке `/hubs/clock`.

Дополнительные сведения о строго типизированных концентраторах см. [в статье Использование концентраторов в SignalR для ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).

> [!NOTE]
> Эта функциональность не ограничена классом [\<>](xref:Microsoft.AspNetCore.SignalR.Hub`1) . Любой класс, наследующий от [концентратора](xref:Microsoft.AspNetCore.SignalR.Hub), например [динамичуб](xref:Microsoft.AspNetCore.SignalR.DynamicHub), также будет работать.

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

Интерфейс, используемый строго типизированным `ClockHub`, является интерфейсом `IClock`.

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-opno-locsignalr-hub-from-a-background-service"></a>Вызов центра SignalR из фоновой службы

Во время запуска класс `Worker`, `BackgroundService`, включается с помощью `AddHostedService`.

```csharp
services.AddHostedService<Worker>();
```

Так как SignalR также включена на этапе `Startup`, в котором каждый концентратор подключен к отдельной конечной точке в конвейере HTTP-запросов ASP.NET Core, каждый концентратор представляется `IHubContext<T>` на сервере. Используя функции DI ASP.NET Core, другие классы, созданные на уровне размещения, такие как классы `BackgroundService`, классы контроллеров MVC или модели страниц Razor, могут получать ссылки на серверные концентраторы, принимая экземпляры `IHubContext<ClockHub, IClock>` во время создания.

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

Так как метод `ExecuteAsync` вызывается итеративно в фоновой службе, текущая дата и время сервера отправляются подключенным клиентам с помощью `ClockHub`.

## <a name="react-to-opno-locsignalr-events-with-background-services"></a>Реагирование на события SignalR с помощью фоновых служб

Как и в одностраничном приложении, использующем клиент JavaScript для SignalR или классического приложения .NET, может выполнять с помощью <xref:signalr/dotnet-client>, `BackgroundService` или `IHostedService` можно также использовать для подключения к концентраторам SignalR и реагирования на события.

Класс `ClockHubClient` реализует интерфейс `IClock` и интерфейс `IHostedService`. Таким образом, его можно включить во время `Startup` для непрерывного выполнения и реагирования на события концентратора с сервера.

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

Во время инициализации `ClockHubClient` создает экземпляр `HubConnection` и включает метод `IClock.ShowTime` в качестве обработчика для события `ShowTime` концентратора.

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

В реализации `IHostedService.StartAsync` `HubConnection` запускается асинхронно.

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

Во время выполнения метода `IHostedService.StopAsync` `HubConnection` удаляется асинхронно.

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Начало работы](xref:tutorials/signalr)
* [Центры](xref:signalr/hubs)
* [Публикация в Azure](xref:signalr/publish-to-azure-web-app)
* [Строго типизированные концентраторы](xref:signalr/hubs#strongly-typed-hubs)
