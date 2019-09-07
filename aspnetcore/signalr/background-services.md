---
title: Ведущие ASP.NET Core SignalR в фоновых службах
author: bradygaster
description: Узнайте, как отправить сообщения клиентам SignalR из классов .NET Core BackgroundService.
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/04/2019
uid: signalr/background-services
ms.openlocfilehash: 23a53f33a03ce3b76cf6846c3f214a6cad055209
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773938"
---
# <a name="host-aspnet-core-signalr-in-background-services"></a>Ведущие ASP.NET Core SignalR в фоновых службах

По [Брейди Гастер](https://twitter.com/bradygaster)

В этой статье приводятся рекомендации по следующим вопросам.

* Размещение концентраторов SignalR с помощью фонового рабочего процесса, размещенного в ASP.NET Core.
* Отправка сообщений на подключенные клиенты из [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).NET Core.

[Просмотр или скачивание образца кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(как скачать)](xref:index#how-to-download-a-sample)

## <a name="wire-up-signalr-during-startup"></a>Подсоединить SignalR во время запуска

::: moniker range=">= aspnetcore-3.0"

Размещение концентраторов SignalR ASP.NET Core в контексте фонового рабочего процесса идентично размещению центра в веб-приложении ASP.NET Core. В методе вызов `services.AddSignalR` добавляет необходимые службы к слою ASP.NET Core внедрения зависимостей (DI) для поддержки SignalR. `Startup.ConfigureServices` В `Startup.Configure` методвызывается`MapHub` в обратномвызове,чтобыподключитьконечныеточкиконцентраторавконвейерезапросовASP.NETCore.`UseEndpoints`

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

Размещение концентраторов SignalR ASP.NET Core в контексте фонового рабочего процесса идентично размещению центра в веб-приложении ASP.NET Core. В методе вызов `services.AddSignalR` добавляет необходимые службы к слою ASP.NET Core внедрения зависимостей (DI) для поддержки SignalR. `Startup.ConfigureServices` В `Startup.Configure` `UseSignalR` метод вызывается, чтобы подключить конечные точки концентратора в конвейере запросов ASP.NET Core.

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

::: moniker-end

В предыдущем примере `ClockHub` класс `Hub<T>` реализует класс для создания строго типизированного концентратора. Объект `ClockHub` настроен `/hubs/clock`в классе для реагирования на запросы в конечной точке. `Startup`

Дополнительные сведения о строго типизированных концентраторах см. в статье [Использование концентраторов в SignalR для ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).

> [!NOTE]
> Эта функция не ограничена классом [> концентратора\<](xref:Microsoft.AspNetCore.SignalR.Hub`1) . Любой класс, наследующий от [концентратора](xref:Microsoft.AspNetCore.SignalR.Hub), например [динамичуб](xref:Microsoft.AspNetCore.SignalR.DynamicHub), также будет работать.

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

Интерфейс, используемый строго типизированным `ClockHub` объектом, `IClock` является интерфейсом.

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-signalr-hub-from-a-background-service"></a>Вызов концентратора SignalR из фоновой службы

Во время запуска `Worker` класс `BackgroundService`() поддается с помощью `AddHostedService`.

```csharp
services.AddHostedService<Worker>();
```

Так как SignalR также `Startup` работает на этапе, в котором каждый концентратор подключен к отдельной конечной точке в конвейере HTTP-запросов ASP.NET Core, каждый концентратор `IHubContext<T>` представляется на сервере. Используя функции внедрения ASP.NET Core, другие классы, созданные на уровне размещения, такие как `BackgroundService` классы, классы контроллеров MVC или модели страниц Razor, могут получать ссылки на серверные концентраторы, принимая `IHubContext<ClockHub, IClock>` экземпляры во время создания.

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

Так как `ClockHub`метод вызывается итеративно в фоновой службе, текущая дата и время сервера отправляются подключенным клиентам с помощью. `ExecuteAsync`

## <a name="react-to-signalr-events-with-background-services"></a>Реагирование на события SignalR с помощью фоновых служб

Как и приложение с одним страничным приложением, использующее клиент JavaScript для SignalR или классическое приложение .NET, может использовать <xref:signalr/dotnet-client>с помощью `BackgroundService` , `IHostedService` а также для подключения к концентраторам SignalR и реагирования на события.

Класс реализует `IClock` интерфейс и`IHostedService` интерфейс. `ClockHubClient` Таким образом, она может быть подключена `Startup` во время непрерывного выполнения и реагировать на события концентратора с сервера.

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

Во время инициализации объект `ClockHubClient` создает экземпляр `HubConnection` класса и подсоединяет `IClock.ShowTime` `ShowTime` метод к нему в качестве обработчика для события концентратора.

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

`IHostedService.StartAsync` В реализации `HubConnection` запускается асинхронно.

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

Во время выполнения `HubConnection`методаобъект уничтожается асинхронно. `IHostedService.StopAsync`

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Начало работы](xref:tutorials/signalr)
* [Центры](xref:signalr/hubs)
* [Публикация в Azure](xref:signalr/publish-to-azure-web-app)
* [Строго типизированные концентраторы](xref:signalr/hubs#strongly-typed-hubs)
