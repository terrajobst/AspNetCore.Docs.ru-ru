---
title: ASP.NET Core внедрения зависимостей Blazor
author: guardrex
description: Узнайте, как Blazor приложения могут внедрять службы в компоненты.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2020
no-loc:
- Blazor
- SignalR
uid: blazor/dependency-injection
ms.openlocfilehash: 859fd484fc00104575f176fa7d3bf752895475a0
ms.sourcegitcommit: c81ef12a1b6e6ac838e5e07042717cf492e6635b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2020
ms.locfileid: "76885503"
---
# <a name="aspnet-core-blazor-dependency-injection"></a>ASP.NET Core внедрения зависимостей Блазор

По [Раинер стропек](https://www.timecockpit.com)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Блазор поддерживает [внедрение зависимостей (DI)](xref:fundamentals/dependency-injection). Приложения могут использовать встроенные службы, внедряя их в компоненты. Приложения также могут определять и регистрировать пользовательские службы и предоставлять доступ к ним в рамках всего приложения с помощью ondi.

DI — это методика доступа к службам, настроенным в центральном расположении. Это может быть полезно в приложениях Блазор для:

* Совместное использование одного экземпляра класса службы во множестве компонентов, называемом *одноэлементной* службой.
* Отделение компонентов от конкретных классов служб с помощью абстракций ссылок. Например, рассмотрим интерфейс `IDataAccess` для доступа к данным в приложении. Интерфейс реализуется конкретным классом `DataAccess` и регистрируется как служба в контейнере службы приложения. Когда компонент использует метод DI для получения реализации `IDataAccess`, этот компонент не связан с конкретным типом. Реализация может быть переключена, возможно, для реализации макета в модульных тестах.

## <a name="default-services"></a>Службы по умолчанию

Службы по умолчанию автоматически добавляются в коллекцию служб приложения.

| Service | Время существования | Описание |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | Одноэлементный | Предоставляет методы для отправки HTTP-запросов и получения HTTP-ответов от ресурса, идентифицируемого по универсальному коду ресурса (URI).<br><br>Экземпляр `HttpClient` в приложении Блазор веб-сборки использует браузер для обработки HTTP-трафика в фоновом режиме.<br><br>Серверные приложения блазор не включают `HttpClient`, настроенные как службы по умолчанию. Предоставьте `HttpClient` серверному приложению Блазор.<br><br>Для получения дополнительной информации см. <xref:blazor/call-web-api>. |
| `IJSRuntime` | Singleton (Блазор-сборка)<br>С заданной областью (Блазор Server) | Представляет экземпляр среды выполнения JavaScript, в которой отправляются вызовы JavaScript. Для получения дополнительной информации см. <xref:blazor/javascript-interop>. |
| `NavigationManager` | Singleton (Блазор-сборка)<br>С заданной областью (Блазор Server) | Содержит вспомогательные методы для работы с URI и состоянием навигации. Дополнительные сведения см. в разделе вспомогательные функции для [URI и состояний навигации](xref:blazor/routing#uri-and-navigation-state-helpers). |

Пользовательский поставщик услуг не предоставляет службы по умолчанию, перечисленные в таблице автоматически. Если вы используете пользовательский поставщик услуг и требуете какой-либо из служб, показанных в таблице, добавьте необходимые службы к новому поставщику услуг.

## <a name="add-services-to-an-app"></a>Добавление служб в приложение

### <a name="blazor-webassembly"></a>Blazor WebAssembly

Настройте службы для коллекции служб приложения в методе `Main` *Program.CS*. В следующем примере `MyDependency`ная реализация зарегистрирована для `IMyDependency`:

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<IMyDependency, MyDependency>();
        builder.RootComponents.Add<App>("app");

        await builder.Build().RunAsync();
    }
}
```

После сборки узла доступ к службам можно получить из корневой области DI до подготовки к просмотру каких-либо компонентов. Это может быть полезно для запуска логики инициализации перед отрисовкой содержимого:

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<WeatherService>();
        builder.RootComponents.Add<App>("app");

        var host = builder.Build();

        var weatherService = host.Services.GetRequiredService<WeatherService>();
        await weatherService.InitializeWeatherAsync();

        await host.RunAsync();
    }
}
```

Узел также предоставляет центральный экземпляр конфигурации для приложения. Основываясь на предыдущем примере, URL-адрес службы погоды передается из источника конфигурации по умолчанию (например, *appSettings. JSON*) в `InitializeWeatherAsync`:

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<WeatherService>();
        builder.RootComponents.Add<App>("app");

        var host = builder.Build();

        var weatherService = host.Services.GetRequiredService<WeatherService>();
        await weatherService.InitializeWeatherAsync(
            host.Configuration["WeatherServiceUrl"]);

        await host.RunAsync();
    }
}
```

### <a name="blazor-server"></a>Blazor Server

После создания нового приложения изучите метод `Startup.ConfigureServices`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

Методу `ConfigureServices` передается <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, который представляет собой список объектов дескриптора службы (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>). Службы добавляются путем предоставления дескрипторов служб коллекции служб. В следующем примере демонстрируется понятие с интерфейсом `IDataAccess` и его конкретной реализацией `DataAccess`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

### <a name="service-lifetime"></a>Время существования службы

Службы можно настроить с учетом времени существования, приведенного в следующей таблице.

| Время существования | Описание |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Blazor приложения веб-сборки в настоящее время не имеют концепции областей DI. `Scoped`-зарегистрированные службы ведут себя так же, как `Singleton` Services. Однако модель размещения сервера Blazor поддерживает время существования `Scoped`. В Blazor Server Apps регистрация службы с заданной областью ограничивается *подключением*. По этой причине использование служб с заданной областью предпочтительно для служб, которые должны быть ограничены текущим пользователем, даже если текущим намерением является запуск на стороне клиента в браузере. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | DI создает *один экземпляр* службы. Все компоненты, для которых необходима служба `Singleton`, получают экземпляр той же службы. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | Каждый раз, когда компонент получает экземпляр службы `Transient` из контейнера службы, он получает *новый экземпляр* службы. |

Система DI основана на системе DI в ASP.NET Core. Для получения дополнительной информации см. <xref:fundamentals/dependency-injection>.

## <a name="request-a-service-in-a-component"></a>Запрос службы в компоненте

После добавления служб в коллекцию служб внедрите службы в компоненты, используя директиву [\@вставить](xref:mvc/views/razor#inject) Razor. `@inject` имеет два параметра:

* Введите &ndash; тип службы для вставки.
* Свойство &ndash; имя свойства, получающего внедренную службу приложений. Свойство не требуется создавать вручную. Компилятор создает свойство.

Для получения дополнительной информации см. <xref:mvc/views/dependency-injection>.

Используйте несколько инструкций `@inject` для внедрения различных служб.

В следующем примере показано, как использовать `@inject`. Служба, реализующая `Services.IDataAccess`, внедряется в `DataRepository`свойств компонента. Обратите внимание, что код использует только абстракцию `IDataAccess`:

[!code-razor[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

На внутреннем уровне создаваемое свойство (`DataRepository`) использует атрибут `InjectAttribute`. Как правило, этот атрибут не используется напрямую. Если базовый класс необходим для компонентов, а для базового класса также требуются обязательные свойства, добавьте `InjectAttribute`вручную:

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

В компонентах, производных от базового класса, директива `@inject` не требуется. `InjectAttribute` базового класса достаточно:

```razor
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a>Использование ondi в службах

Для сложных служб могут потребоваться дополнительные службы. В предыдущем примере `DataAccess` может потребоваться служба `HttpClient` по умолчанию. `@inject` (или `InjectAttribute`) недоступен для использования в службах. Вместо этого следует использовать *внедрение конструктора* . Необходимые службы добавляются путем добавления параметров в конструктор службы. Когда DI создает службу, она распознает необходимые службы в конструкторе и предоставляет их соответствующим образом.

```csharp
public class DataAccess : IDataAccess
{
    // The constructor receives an HttpClient via dependency
    // injection. HttpClient is a default service.
    public DataAccess(HttpClient client)
    {
        ...
    }
}
```

Необходимые условия для внедрения конструктора:

* Должен существовать один конструктор, аргументы которого могут быть выполнены методом DI. Дополнительные параметры, не охваченные DI, разрешены, если они указывают значения по умолчанию.
* Применимый конструктор должен быть *открытым*.
* Должен существовать один подходящий конструктор. В случае неоднозначности DI выдает исключение.

## <a name="utility-base-component-classes-to-manage-a-di-scope"></a>Классы базовых компонентов служебной программы для управления областью DI

В ASP.NET Core приложениях службы с областью действия обычно ограничены текущим запросом. По завершении запроса все неограниченные или временные службы удаляются системой DI. В Blazor серверных приложений область запроса длится на время клиентского соединения, что может привести к тому, что временные и ограниченные службы будут работать намного дольше, чем ожидалось.

Чтобы ограничить время существования компонента службами, можно использовать базовые классы `OwningComponentBase` и `OwningComponentBase<TService>`. Эти базовые классы предоставляют `ScopedServices` свойство типа `IServiceProvider`, разрешающее службы, областью действия которых является компонент. Чтобы создать компонент, наследующий от базового класса в Razor, используйте директиву `@inherits`.

```razor
@page "/users"
@attribute [Authorize]
@inherits OwningComponentBase<Data.ApplicationDbContext>

<h1>Users (@Service.Users.Count())</h1>
<ul>
    @foreach (var user in Service.Users)
    {
        <li>@user.UserName</li>
    }
</ul>
```

> [!NOTE]
> Службы, внедренные в компонент с помощью `@inject` или `InjectAttribute`, не создаются в области компонента и привязаны к области запроса.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
