---
title: Внедрение зависимостей Blazor в ASP.NET Core
author: guardrex
description: Сведения о том, как приложения Blazor могут внедрять службы в компоненты.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/20/2020
no-loc:
- Blazor
- SignalR
uid: blazor/dependency-injection
ms.openlocfilehash: 4cdde9ee8c9fd9adf00894a067d32965b180e5ec
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2020
ms.locfileid: "78646696"
---
# <a name="aspnet-core-blazor-dependency-injection"></a>Внедрение зависимостей Blazor в ASP.NET Core

Авторы: [Райнер Стропек](https://www.timecockpit.com) (Rainer Stropek) и [Майк Роусос](https://github.com/mjrousos) (Mike Rousos)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor поддерживает [внедрение зависимостей](xref:fundamentals/dependency-injection). Приложения могут использовать встроенные службы, внедряя их в компоненты. Приложения также могут определять и регистрировать пользовательские службы и предоставлять к ним доступ в рамках всего приложения с помощью внедрения зависимостей.

Внедрение зависимостей — это методика доступа к службам, настроенным в центральном расположении. Она может пригодиться в приложениях Blazor для следующих задач:

* Совместное использование одного экземпляра класса службы во множестве компонентов, называемого *одноэлементной* службой.
* Отделение компонентов от конкретных классов служб с помощью абстракций ссылок. Например, рассмотрим интерфейс `IDataAccess` для доступа к данным в приложении. Этот интерфейс реализуется конкретным классом `DataAccess` и регистрируется в качестве службы в контейнере службы приложения. Когда компонент использует внедрение зависимостей для получения реализации `IDataAccess`, этот компонент не связывается с конкретным типом. Реализация может быть переключена, например, для реализации макета в модульных тестах.

## <a name="default-services"></a>Службы по умолчанию

Службы по умолчанию автоматически добавляются в коллекцию служб приложения.

| Служба | Срок действия | Description |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | Одноэлементный | Предоставляет методы для отправки HTTP-запросов и получения HTTP-ответов от ресурса с заданным URI.<br><br>Экземпляр `HttpClient` в приложении Blazor WebAssembly использует браузер для обработки HTTP-трафика в фоновом режиме.<br><br>Приложения Blazor Server не включают `HttpClient`, по умолчанию настроенный в качестве службы. Предоставьте `HttpClient` приложению Blazor Server.<br><br>Дополнительные сведения см. в разделе <xref:blazor/call-web-api>. |
| `IJSRuntime` | Одноэлементный (Blazor WebAssembly)<br>С заданной областью (Blazor Server) | Представляет экземпляр среды выполнения JavaScript, в которую отправляются вызовы JavaScript. Дополнительные сведения см. в разделе <xref:blazor/call-javascript-from-dotnet>. |
| `NavigationManager` | Одноэлементный (Blazor WebAssembly)<br>С заданной областью (Blazor Server) | Содержит вспомогательные методы для работы с URI и состоянием навигации. Дополнительные сведения см. в разделе [URI и вспомогательные инструменты состояния навигации](xref:blazor/routing#uri-and-navigation-state-helpers). |

Пользовательский поставщик услуг не предоставляет перечисленные в таблице службы по умолчанию автоматически. Если вы используете пользовательский поставщик услуг и нуждаетесь в какой-либо из служб, указанных в таблице, добавьте необходимые службы в новый поставщик услуг.

## <a name="add-services-to-an-app"></a>Добавление служб в приложение

### <a name="blazor-webassembly"></a>Blazor WebAssembly

Настройте службы для коллекции служб приложения в методе `Main` файла *Program.cs*. В следующем примере реализация `MyDependency` зарегистрирована для `IMyDependency`:

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

После сборки узла доступ к службам можно получить из корневой области внедрения зависимостей до отрисовки каких-либо компонентов. Это может быть удобно для запуска логики инициализации перед отрисовкой содержимого:

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

Узел также предоставляет центральный экземпляр конфигурации для приложения. Основываясь на предыдущем примере, URL-адрес службы погоды передается из источника конфигурации по умолчанию (например, *appsettings.json*) в `InitializeWeatherAsync`:

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

После создания приложения изучите метод `Startup.ConfigureServices`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

В метод `ConfigureServices` передается <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, представляющий собой список объектов дескриптора службы (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>). Службы добавляются путем предоставления дескрипторов служб в коллекцию служб. В следующем примере показана концепция с интерфейсом `IDataAccess` и его конкретной реализацией `DataAccess`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

### <a name="service-lifetime"></a>Время существования службы

Для служб можно настроить параметры времени существования, указанные в следующей таблице.

| Срок действия | Description |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Сейчас в приложениях Blazor WebAssembly концепция областей внедрения зависимостей отсутствует. Службы, зарегистрированные как `Scoped`, работают аналогично службам `Singleton`. Однако модель размещения сервера Blazor поддерживает время существования `Scoped`. В приложениях Blazor Server регистрация службы с заданной областью ограничивается *соединением*. По этой причине использование служб с заданной областью предпочтительно для служб, которые должны быть ограничены текущим пользователем, даже если текущим намерением является запуск на стороне клиента в браузере. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | Система внедрения зависимостей создает *один экземпляр* службы. Все компоненты, для которых необходима служба `Singleton`, получают экземпляр той же службы. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | Каждый раз, когда компонент получает экземпляр службы `Transient` из контейнера службы, он получает *новый экземпляр* этой службы. |

Система внедрения зависимостей основана на системе внедрения зависимостей в ASP.NET Core. Дополнительные сведения см. в разделе <xref:fundamentals/dependency-injection>.

## <a name="request-a-service-in-a-component"></a>Запрос службы в компоненте

После добавления служб в коллекцию служб внедрите их в компоненты, используя директиву [\@inject](xref:mvc/views/razor#inject) Razor. `@inject` имеет два параметра:

* Тип &ndash; тип внедряемой службы.
* Свойство &ndash; имя свойства, получающего внедренную службу приложений. Свойство не требуется создавать вручную. Его создает компилятор.

Дополнительные сведения см. в разделе <xref:mvc/views/dependency-injection>.

Используйте несколько инструкций `@inject` для внедрения различных служб.

В следующем примере показано, как использовать `@inject`. Служба, реализующая `Services.IDataAccess`, внедряется в свойство `DataRepository` компонента. Обратите внимание, что код использует только абстракцию `IDataAccess`:

[!code-razor[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

На внутреннем уровне создаваемое свойство (`DataRepository`) использует атрибут `InjectAttribute`. Как правило, этот атрибут не используется напрямую. Если базовый класс необходим для компонентов, а для базового класса также требуются обязательные свойства, добавьте `InjectAttribute` вручную:

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

## <a name="use-di-in-services"></a>Использование внедрения зависимостей в службах

Для сложных служб могут потребоваться дополнительные службы. В предыдущем примере для `DataAccess` может потребоваться служба `HttpClient` по умолчанию. `@inject` (или `InjectAttribute`) невозможно использовать в службах. Вместо этого следует использовать *внедрения конструктора*. Необходимые службы добавляются путем добавления параметров в конструктор службы. Когда система внедрения зависимостей создает службу, она распознает необходимые службы в конструкторе и предоставляет их соответствующим образом.

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

* Должен существовать один конструктор, аргументы которого могут использоваться системой внедрения зависимостей. Дополнительные параметры, не охваченные системой внедрения зависимостей, разрешены, если они указывают значения по умолчанию.
* Применимый конструктор должен быть *общедоступным*.
* Должен существовать один подходящий конструктор. В случае неоднозначности система внедрения зависимостей выдает исключение.

## <a name="utility-base-component-classes-to-manage-a-di-scope"></a>Служебные классы базовых компонентов служебной программы для управления областью внедрения зависимостей

В приложениях ASP.NET Core службы с заданной областью обычно ограничены текущим запросом. По завершении запроса все временные службы или службы с заданной областью удаляются системой внедрения зависимостей. В приложениях Blazor Server область запроса существует на протяжении всего клиентского соединения, что может привести к тому, что временные службы или службы с заданной областью будут работать намного дольше, чем ожидалось. В приложениях Blazor WebAssembly службы, зарегистрированные с ограниченным временем существования, рассматриваются как одноэлементные, поэтому они существуют дольше, чем службы с заданной областью в типичных приложениях ASP.NET Core.

Подход, ограничивающий время существования службы в приложениях Blazor, заключается в использовании типа `OwningComponentBase`. `OwningComponentBase` является абстрактным типом, производным от `ComponentBase`, который создает область внедрения зависимостей, соответствующую времени существования компонента. Используя эту область, можно использовать службы внедрения зависимостей с ограниченным временем существования, заставляя их существовать так же долго, как и компонент. При удалении компонента также удаляются и службы из поставщика служб с заданной областью действия этого компонента. Это может быть полезно для служб, которые:

* требуется повторно использовать в компоненте, так как временное существование не подходит;
* не должны совместно использоваться компонентами, так как одноэлементное время существования не подходит.

Доступны две версии типа `OwningComponentBase`:

* `OwningComponentBase` является абстрактной высвобождаемой дочерней версией типа `ComponentBase` с защищенным свойством `ScopedServices` типа `IServiceProvider`. Этот поставщик можно использовать для разрешения служб, ограниченных временем существования компонента.

  Службы внедрения зависимостей, внедренные в компонент с помощью `@inject` или `InjectAttribute` (`[Inject]`), не создаются в области компонента. Чтобы использовать область компонента, необходимо разрешить службы с помощью `ScopedServices.GetRequiredService` или `ScopedServices.GetService`. Все службы, разрешенные с помощью поставщика `ScopedServices`, имеют свои зависимости из этой же области.

  ```razor
  @page "/preferences"
  @using Microsoft.Extensions.DependencyInjection
  @inherits OwningComponentBase

  <h1>User (@UserService.Name)</h1>

  <ul>
      @foreach (var setting in SettingService.GetSettings())
      {
          <li>@setting.SettingName: @setting.SettingValue</li>
      }
  </ul>

  @code {
      private IUserService UserService { get; set; }
      private ISettingService SettingService { get; set; }

      protected override void OnInitialized()
      {
          UserService = ScopedServices.GetRequiredService<IUserService>();
          SettingService = ScopedServices.GetRequiredService<ISettingService>();
      }
  }
  ```

* `OwningComponentBase<T>` является производным от `OwningComponentBase` и добавляет свойство `Service`, которое возвращает экземпляр `T` из поставщика внедрения зависимостей с заданной областью. Этот тип удобен для доступа к службам с заданной областью без использования экземпляра `IServiceProvider` при наличии одной основной службы, которую приложение запрашивает из контейнера внедрения зависимостей с использованием области компонента. Свойство `ScopedServices` доступно, поэтому при необходимости приложение может получить службы других типов.

  ```razor
  @page "/users"
  @attribute [Authorize]
  @inherits OwningComponentBase<AppDbContext>

  <h1>Users (@Service.Users.Count())</h1>

  <ul>
      @foreach (var user in Service.Users)
      {
          <li>@user.UserName</li>
      }
  </ul>
  ```

## <a name="use-of-entity-framework-dbcontext-from-di"></a>Использование DbContext Entity Framework из системы внедрения зависимостей

Одним из распространенных типов службы, извлекаемым из системы внедрения зависимостей в веб-приложениях, являются объекты `DbContext` Entity Framework (EF). Регистрация служб EF с помощью `IServiceCollection.AddDbContext` добавляет `DbContext` в качестве службы с заданной областью по умолчанию. Регистрация в качестве службы с заданной областью может привести к проблемам в приложениях Blazor, так как это приводит к длительному существованию экземпляров `DbContext` и их совместному использованию в приложении. `DbContext` не является потокобезопасным и не должен использоваться параллельно.

В зависимости от приложения использование `OwningComponentBase` для ограничения области `DbContext` одним компонентом *может* решить эту проблему. Если компонент не использует `DbContext` параллельно, достаточно создать производный компонент от `OwningComponentBase` и извлечь `DbContext` из `ScopedServices`, так как это гарантирует следующее:

* Отдельные компоненты не используют `DbContext` совместно.
* `DbContext` существует столько же, сколько и зависящий от него компонент.

Если отдельный компонент может использовать `DbContext` параллельно (например, каждый раз, когда пользователь нажимает кнопку), даже использование `OwningComponentBase` не позволяет избежать проблем с параллельными операциями EF. В этом случае используйте разные `DbContext` для каждой логической операции EF. Вы можете использовать один из следующих подходов:

* Создайте `DbContext` напрямую с использованием `DbContextOptions<TContext>` в качестве аргумента, который можно извлечь из системы внедрения зависимостей и является потокобезопасным.

    ```razor
    @page "/example"
    @inject DbContextOptions<AppDbContext> DbContextOptions

    <ul>
        @foreach (var item in _data)
        {
            <li>@item</li>
        }
    </ul>

    <button @onclick="LoadData">Load Data</button>

    @code {
        private List<string> _data = new List<string>();

        private async Task LoadData()
        {
            _data = await GetAsync();
            StateHasChanged();
        }

        public async Task<List<string>> GetAsync()
        {
            using (var context = new AppDbContext(DbContextOptions))
            {
                return await context.Products.Select(p => p.Name).ToListAsync();
            }
        }
    }
    ```

* Зарегистрируйте `DbContext` в контейнере службы с временным существованием:
  * При регистрации контекста используйте `ServiceLifetime.Transient`. Метод расширения `AddDbContext` принимает два необязательных параметра типа `ServiceLifetime`. Чтобы использовать этот подход, только параметр `contextLifetime` должен иметь значение `ServiceLifetime.Transient`. `optionsLifetime` может иметь значение по умолчанию `ServiceLifetime.Scoped`.

    ```csharp
    services.AddDbContext<AppDbContext>(options =>
         options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")),
         ServiceLifetime.Transient);
    ```  

  * Временный объект `DbContext` можно внедрить как обычный (с помощью `@inject`) в компоненты, которые не будут выполнять несколько операций EF параллельно. Те компоненты, которые могут выполнять несколько операций EF одновременно, могут запрашивать отдельные объекты `DbContext` для каждой параллельной операции с помощью `IServiceProvider.GetRequiredService`.

    ```razor
    @page "/example"
    @using Microsoft.Extensions.DependencyInjection
    @inject IServiceProvider ServiceProvider

    <ul>
        @foreach (var item in _data)
        {
            <li>@item</li>
        }
    </ul>

    <button @onclick="LoadData">Load Data</button>

    @code {
        private List<string> _data = new List<string>();

        private async Task LoadData()
        {
            _data = await GetAsync();
            StateHasChanged();
        }

        public async Task<List<string>> GetAsync()
        {
            using (var context = ServiceProvider.GetRequiredService<AppDbContext>())
            {
                return await context.Products.Select(p => p.Name).ToListAsync();
            }
        }
    }
    ```

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
