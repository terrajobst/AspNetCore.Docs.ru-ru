---
title: Внедрение зависимостей в ASP.NET Core
author: guardrex
description: Сведения о том, как ASP.NET Core реализует внедрение зависимостей и как его использовать.
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2018
uid: fundamentals/dependency-injection
ms.openlocfilehash: 861370dc689e2420838f639ea0b1fb8f73927e16
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342423"
---
# <a name="dependency-injection-in-aspnet-core"></a>Внедрение зависимостей в ASP.NET Core

Авторы: [Стив Смит](https://ardalis.com/) (Steve Smith), [Скотт Эдди](https://scottaddie.com) (Scott Addie) и [Люк Латам](https://github.com/guardrex) (Luke Latham)

ASP.NET Core поддерживает проектирование программного обеспечения с возможностью внедрения зависимостей. При таком подходе достигается [инверсия управления](https://deviq.com/inversion-of-control/) между классами и их зависимостями.

Дополнительные сведения о внедрении зависимостей в контроллерах MVC см. в статье <xref:mvc/controllers/dependency-injection>.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="overview-of-dependency-injection"></a>Общие сведения о внедрении зависимостей

*Зависимость* — это любой объект, который требуется другому объекту. Рассмотрим класс `MyDependency` с методом `WriteMessage`, от которого зависят другие классы в приложении.

```csharp
public class MyDependency
{
    public MyDependency()
    {
    }

    public Task WriteMessage(string message)
    {
        Console.WriteLine(
            $"MyDependency.WriteMessage called. Message: {message}");

        return Task.FromResult(0);
    }
}
```

::: moniker range=">= aspnetcore-2.1"

Чтобы сделать метод `WriteMessage` доступным какому-то классу, можно создать экземпляр класса `MyDependency`. Класс `MyDependency` выступает зависимостью класса `IndexModel`.

```csharp
public class IndexModel : PageModel
{
    MyDependency _dependency = new MyDependency();

    public async Task OnGetAsync()
    {
        await _myDependency.WriteMessage(
            "IndexModel.OnGetAsync created this message.");
    }
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

Чтобы сделать метод `WriteMessage` доступным какому-то классу, можно создать экземпляр класса `MyDependency`. Класс `MyDependency` выступает зависимостью класса `HomeController`.

```csharp
public class HomeController : Controller
{
    MyDependency _dependency = new MyDependency();

    public async Task<IActionResult> Index()
    {
        await _myDependency.WriteMessage(
            "HomeController.Index created this message.");

        return View();
    }
}
```

::: moniker-end

Этот класс создает экземпляр `MyDependency` и напрямую зависит от него. Зависимости в коде (как в предыдущем примере) представляют собой определенную проблему. Их следует избегать по следующим причинам:

* Чтобы заменить `MyDependency` другой реализацией, класс необходимо изменить.
* Если у `MyDependency` есть зависимости, их конфигурацию должен выполнять класс. В больших проектах, когда от `MyDependency` зависят многие классы, код конфигурации растягивается по всему приложению.
* Такая реализация плохо подходит для модульных тестов. В приложении нужно использовать имитацию или заглушку в виде класса `MyDependency`, что при таком подходе невозможно.

Внедрение зависимостей устраняет эти проблемы следующим образом:

* Используется интерфейс для абстрагирования реализации зависимостей.
* Зависимость регистрируется в контейнере служб. ASP.NET Core предоставляет встроенный контейнер служб, [IServiceProvider](/dotnet/api/system.iserviceprovider). Службы регистрируются в приложении в методе `Startup.ConfigureServices`.
* Служба *внедряется* в конструктор класса там, где он используется. Платформа берет на себя создание экземпляра зависимости и его удаление, когда он больше не нужен.

В [примере приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) интерфейс `IMyDependency` определяет метод, который служба предоставляет приложению.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

Этот интерфейс реализуется конкретным типом, `MyDependency`.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

`MyDependency` запрашивает [ILogger&lt;TCategoryName&gt;](/dotnet/api/microsoft.extensions.logging.ilogger-1) в своем конструкторе. Использование цепочки внедрений зависимостей не является чем-то необычным. Каждая запрашиваемая зависимость запрашивает собственные зависимости. Контейнер разрешает зависимости в графе и возвращает полностью разрешенную службу. Весь набор зависимостей, которые нужно разрешить, обычно называют *деревом зависимостей*, *графом зависимостей* или *графом объектов*.

`IMyDependency` и `ILogger<TCategoryName>` должны быть зарегистрированы в контейнере служб. `IMyDependency` регистрируется в `Startup.ConfigureServices`. Службу `ILogger<TCategoryName>` регистрирует инфраструктура абстракций ведения журналов, поэтому такая служба является [платформенной](#framework-provided-services), что означает, что ее по умолчанию регистрирует платформа.

В примере приложения служба `IMyDependency` зарегистрирована с конкретным типом `MyDependency`. Регистрация регулирует время существования службы согласно времени существования одного запроса. Подробнее о [времени существования служб](#service-lifetimes) мы поговорим далее в этой статье.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=11)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

> [!NOTE]
> Каждый метод расширения `services.Add<ServiceName>` добавляет (а потенциально и настраивает) службы. Например, `services.AddMvc()` добавляет службы, которые нужны для Razor Pages и MVC. Рекомендуем придерживаться такого подхода в приложениях. Поместите методы расширения в пространство имен [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection), чтобы инкапсулировать группы зарегистрированных служб.

Если конструктору службы требуется примитив, например `string`, его можно внедрить с помощью [конфигурации](xref:fundamentals/configuration/index) и [шаблона параметров](xref:fundamentals/configuration/options).

```csharp
public class MyDependency : IMyDependency
{
    public MyDependency(IConfiguration config)
    {
        var myStringValue = config["MyStringKey"];

        // Use myStringValue
    }

    ...
}
```

Экземпляр службы запрашивается через конструктор класса, в котором эта служба используется и назначается скрытому полю. Поле используется во всем классе для доступа к службе (по мере необходимости).

В примере приложения запрашивается экземпляр `IMyDependency`, который затем используется для вызова метода службы `WriteMessage`.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Controllers/MyDependencyController.cs?name=snippet1&highlight=3,5-8,13-14)]

::: moniker-end

## <a name="framework-provided-services"></a>Платформенные службы

Метод `Startup.ConfigureServices` отвечает за определение служб, которые использует приложение, включая такие компоненты, как Entity Framework Core и ASP.NET Core MVC. Изначально `IServiceCollection`, предоставленный `ConfigureServices`, имеет следующие определенные службы (в зависимости от [настройки узла](xref:fundamentals/host/index)):

| Тип службы | Время существования |
| ------------ | -------- |
| [Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory](/dotnet/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | Временно |
| [Microsoft.AspNetCore.Hosting.IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | Один раз |
| [Microsoft.AspNetCore.Hosting.IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) | Один раз |
| [Microsoft.AspNetCore.Hosting.IStartup](/dotnet/api/microsoft.aspnetcore.hosting.istartup) | Один раз |
| [Microsoft.AspNetCore.Hosting.IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) | Временно |
| [Microsoft.AspNetCore.Hosting.Server.IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) | Один раз |
| [Microsoft.AspNetCore.Http.IHttpContextFactory](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextfactory) | Временно |
| [Microsoft.Extensions.Logging.ILogger&lt;T&gt;](/dotnet/api/microsoft.extensions.logging.ilogger) | Один раз |
| [Microsoft.Extensions.Logging.ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) | Один раз |
| [Microsoft.Extensions.ObjectPool.ObjectPoolProvider](/dotnet/api/microsoft.extensions.objectpool.objectpoolprovider) | Один раз |
| [Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) | Временно |
| [Microsoft.Extensions.Options.IOptions&lt;T&gt;](/dotnet/api/microsoft.extensions.options.ioptions-1) | Один раз |
| [System.Diagnostics.DiagnosticSource](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | Один раз |
| [System.Diagnostics.DiagnosticListener](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | Один раз |

Если зарегистрировать службу (включая ее зависимые службы, если нужно) можно, используя метод расширения коллекции служб, для регистрации всех служб, необходимых этой службе, рекомендуется использовать один метод расширения `Add<ServiceName>`. Ниже приведен пример того, как добавить дополнительные службы в контейнер с помощью методов расширения [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity) и [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc).

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddMvc();
}
```

Дополнительные сведения см. в документации по API в статье о [классе ServiceCollection](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollection).

## <a name="service-lifetimes"></a>Время существования служб

Для каждой зарегистрированной службы выбирайте подходящее время существования. Для служб ASP.NET можно настроить следующие параметры времени существования:

**Временная**

Временные службы времени существования создаются при каждом их запросе. Это время существования лучше всего подходит для простых служб без отслеживания состояния.

**Регулируемая**

Службы с регулируемым временем существования создаются один раз для одного запроса.

> [!WARNING]
> При использовании такой службы в ПО промежуточного слоя внедрите ее в метод `Invoke` или `InvokeAsync`. Не внедряйте службу через внедрение конструктора, поскольку в таком случае служба будет вести себя как одноэлементный объект. Дополнительные сведения см. в разделе <xref:fundamentals/middleware/index>.

**Одинарная**

Одинарные службы создаются при первом запросе (или при выполнении `ConfigureServices`, когда экземпляр указан во время регистрации службы) и существуют в единственном экземпляре. Созданный экземпляр используют все последующие запросы. Если в приложении нужно использовать одинарные службы, рекомендуется разрешить контейнеру служб управлять временем их существования. Реализовывать конструктивный шаблон с одинарными службами и предоставлять пользовательский код для управления временем существования объекта в классе категорически не рекомендуется.

> [!WARNING]
> Разрешать регулируемую службу из одиночной нельзя. При обработке последующих запросов это может вызвать неправильное состояние службы.

### <a name="constructor-injection-behavior"></a>Поведение при внедрении через конструктор

Службы можно разрешать двумя методами:

* `IServiceProvider`
* [ActivatorUtilities.](/dotnet/api/microsoft.extensions.dependencyinjection.activatorutilities) Позволяет создавать объекты без регистрации службы в контейнере внедрения зависимостей. `ActivatorUtilities` используется с абстракциями пользовательского уровня, например с вспомогательными функциями для тегов, контроллерами MVC, концентраторами SignalR и связывателями моделей.

Конструкторы могут принимать аргументы, которые не предоставляются внедрением зависимостей, но эти аргументы должны назначать значения по умолчанию.

Когда разрешение служб выполняется через `IServiceProvider` или `ActivatorUtilities`, для внедрения через конструктор требуется *открытый* конструктор.

Когда разрешение служб выполняется через `ActivatorUtilities`, для внедрения через конструктор требуется наличие только одного соответствующего конструктора. Перегрузки конструктора поддерживаются, но может существовать всего одна перегрузка, все аргументы которой могут быть обработаны с помощью внедрения зависимостей.

## <a name="entity-framework-contexts"></a>Контексты Entity Framework

Контексты Entity Framework нужно добавлять в контейнер служб с использованием регулируемого времени существования. Это делается автоматически с помощью вызова метода [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) при регистрации контекста базы данных. Службы, использующие контекст базы данных, также должны иметь регулируемое время существования.

## <a name="lifetime-and-registration-options"></a>Параметры времени существования и регистрации

Чтобы продемонстрировать различия между указанными вариантами времени существования и регистрации, рассмотрим интерфейсы, представляющие задачи в виде операции с уникальным идентификатором `OperationId`. В зависимости от того, как время существования службы операции настроено для этих интерфейсов, при запросе из класса контейнер предоставляет тот же или другой экземпляр службы.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

Интерфейсы реализованы в классе `Operation`. Если идентификатор GUID не предоставлен, конструктор `Operation` создает его.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

Регистрируется служба `OperationService`, которая зависит от каждого из других типов `Operation`. Когда служба `OperationService` запрашивается через внедрение зависимостей, она получает либо новый экземпляр каждой службы, либо экземпляр уже существующей службы в зависимости от времени существования зависимой службы.

* Если при запросе создаются временные службы, `OperationsId` службы `IOperationTransient` отличается от `OperationsId` службы `OperationService`. `OperationService` получает новый экземпляр класса `IOperationTransient`. Новый экземпляр возвращает другой идентификатор `OperationsId`.
* Если при запросе создаются регулируемые службы, идентификатор `OperationsId` службы `IOperationScoped` будет таким же, как для службы `OperationService` в запросе. В разных запросах обе службы используют разные значения `OperationsId`.
* Если одинарные службы и службы с одинарным экземпляром создаются один раз и используются во всех запросах и службах, идентификатор `OperationsId` будет одинаковым во всех запросах служб.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

В `Startup.ConfigureServices` каждый тип добавляется в контейнер согласно его времени существования.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=12-15,18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

::: moniker-end

Служба `IOperationSingletonInstance` использует определенный экземпляр с известным идентификатором `Guid.Empty`. Понятно, когда этот тип используется (его GUID — все нули).

::: moniker range=">= aspnetcore-2.1"

В примере приложения показано время существования объектов в пределах одного и нескольких запросов. В примере приложения `IndexModel` запрашивает каждый вид типа `IOperation`, а также `OperationService`. После этого на странице отображаются все значения `OperationId` службы и класса модели страниц в виде назначенных свойств.

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

В примере приложения показано время существования объектов в пределах одного и нескольких запросов. Образец приложения содержит контроллер `OperationsController`, который запрашивает каждый вид типа `IOperation`, а также `OperationService`. Чтобы отобразить значения `OperationId` службы, действие `Index` помещает службы в `ViewBag`.

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Controllers/OperationsController.cs?name=snippet1)]

::: moniker-end

Результаты двух запросов будут выглядеть так:

**Первый запрос**

Операции контроллера:

Transient: d233e165-f417-469b-a866-1cf1935d2518  
Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19  
Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Instance: 00000000-0000-0000-0000-000000000000

Операции `OperationService`:

Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64  
Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19  
Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Instance: 00000000-0000-0000-0000-000000000000

**Второй запрос**

Операции контроллера:

Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0  
Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4  
Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Instance: 00000000-0000-0000-0000-000000000000

Операции `OperationService`:

Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf  
Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4  
Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Instance: 00000000-0000-0000-0000-000000000000

Проанализируйте, какие значения `OperationId` изменяются в пределах одного и нескольких запросов.

* *Временные* объекты всегда разные. Обратите внимание, что временное значение `OperationId` отличается в первом и втором запросе, а также в обеих операциях `OperationService`. Каждой службе и каждому запросу предоставляется новый экземпляр.
* *Регулируемые* объекты остаются неизменными в пределах одного запроса, но в разных запросах используются разные объекты.
* *Одинарные* объекты остаются неизменными для всех объектов и запросов независимо от того, предоставляется ли в `ConfigureServices` экземпляр `Operation`.

## <a name="call-services-from-main"></a>Вызов служб из функции main

Создайте [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) с [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) для разрешения регулируемой службы в области приложения. Этот способ позволит получить доступ к регулируемой службе при запуске для выполнения задач по инициализации. В следующем примере показано, как получить контекст для `MyScopedService` в `Program.Main`:

```csharp
public static void Main(string[] args)
{
    var host = CreateWebHostBuilder(args).Build();

    using (var serviceScope = host.Services.CreateScope())
    {
        var services = serviceScope.ServiceProvider;

        try
        {
            var serviceContext = services.GetRequiredService<MyScopedService>();
            // Use the context here
        }
        catch (Exception ex)
        {
            var logger = services.GetRequiredService<ILogger<Program>>();
            logger.LogError(ex, "An error occurred.");
        }
    }

    host.Run();
}
```

## <a name="scope-validation"></a>Проверка регулировки

::: moniker range=">= aspnetcore-2.0"

Когда приложение выполняется в среде разработки, поставщик службы по умолчанию проверяет следующее:

* Регулируемые службы не разрешаются из корневого поставщика службы ни прямо, ни косвенно.
* Регулируемые службы не внедряются в одинарные объекты ни прямо, ни косвенно.

::: moniker-end

Корневой поставщик службы создается при вызове [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider). Время существования корневого поставщика службы соответствует времени существования приложения или сервера — поставщик запускается с приложением и удаляется, когда приложение завершает работу.

Регулируемые службы удаляются создавшим их контейнером. Если регулируемая служба создается в корневом контейнере, время существования службы повышается до уровня одинарного объекта, поскольку она удаляется только корневым контейнером при завершении работы приложения или сервера. Проверка регулировки службы перехватывает эти ситуации при вызове `BuildServiceProvider`.

Дополнительные сведения см. в разделе <xref:fundamentals/host/web-host#scope-validation>.

## <a name="request-services"></a>Службы запросов

Службы, доступные в пределах запроса ASP.NET Core из `HttpContext`, предоставляются через коллекцию [HttpContext.RequestServices](/dotnet/api/microsoft.aspnetcore.http.httpcontext.requestservices).

Службы запросов — это все настроенные и запрашиваемые в приложении службы. Когда объекты указывают зависимости, они удовлетворяются за счет типов из `RequestServices`, а не из `ApplicationServices`.

Как правило, использовать эти свойства в приложении напрямую не следует. Вместо этого запрашивайте требуемые для классов типы через конструкторы классов и разрешите платформе внедрять зависимости. Таким образом вы получите классы, которые легче тестировать (см. статью [Тестирование, отладка и устранение неполадок в ASP.NET Core](xref:test/index)).

> [!NOTE]
> Предпочтительнее запрашивать зависимости в качестве параметров конструктора, а не обращаться к коллекции `RequestServices`.

## <a name="design-services-for-dependency-injection"></a>Проектирование служб для внедрения зависимостей

Придерживайтесь следующих рекомендаций:

* Проектируйте службы так, чтобы для получения зависимостей они использовали внедрение зависимостей.
* Избегайте вызовов статических методов или методов с отслеживанием состояния ([статическое сцепление](https://deviq.com/static-cling/)).
* Избегайте прямого создания экземпляров зависимых классов внутри служб. Прямое создание экземпляров обязывает использовать в коде определенную реализацию.

Соблюдение [принципов SOLID для объектно-ориентированного проектирования](https://deviq.com/solid/) поможет сделать программные классы небольшими, хорошо организованными и удобными в тестировании.

Если класс имеет слишком много внедренных зависимостей, обычно это указывает на то, что у класса слишком много задач и он не соответствует [принципу единственной обязанности](https://deviq.com/single-responsibility-principle/). Попробуйте выполнить рефакторинг класса и перенести часть его обязанностей в новый класс. Помните, что в классах модели страниц Razor Pages и классах контроллера MVC должны преимущественно выполняться задачи, связанные с пользовательским интерфейсом. Бизнес-правила и реализация доступа к данным должны обрабатываться в классах, которые предназначены для [этих целей](https://deviq.com/separation-of-concerns/).

### <a name="disposal-of-services"></a>Удаление служб

Контейнер вызывает `Dispose` для создаваемых им типов `IDisposable`. Если экземпляр добавлен в контейнер с помощью пользовательского кода, он не будет удален автоматически.

```csharp
// Services that implement IDisposable:
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}
public class Service3 : IDisposable {}

public interface ISomeService {}
public class SomeServiceImplementation : ISomeService, IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    // The container creates the following instances and disposes them automatically:
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<ISomeService>(sp => new SomeServiceImplementation());

    // The container doesn't create the following instances, so it doesn't dispose of
    // the instances automatically:
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

::: moniker range="= aspnetcore-1.0"

> [!NOTE]
> В ASP.NET Core 1.0 контейнер вызывает удаление для *всех* объектов `IDisposable`, включая те, которые он не создавал.

::: moniker-end

## <a name="default-service-container-replacement"></a>Замена стандартного контейнера служб

Встроенный контейнер служб предназначен для удовлетворения базовых потребностей платформы и большинства основанных на ней клиентских приложений. Однако разработчики могут заменить его на предпочитаемый ими контейнер. Метод `Startup.ConfigureServices` обычно возвращает `void`. Если его сигнатуру изменить так, чтобы он возвращал [IServiceProvider](/dotnet/api/system.iserviceprovider), вы сможете настроить и возвращать другой контейнер. Для .NET доступно множество контейнеров с инверсией управления. В следующем примере используется контейнер [Autofac](https://autofac.org/):

1. Установите соответствующие пакеты контейнера:

    * [Autofac](https://www.nuget.org/packages/Autofac/);
    * [Autofac.Extensions.DependencyInjection](https://www.nuget.org/packages/Autofac.Extensions.DependencyInjection/).

2. Настройте контейнер в `Startup.ConfigureServices` и возвратите `IServiceProvider`.

    ```csharp
    public IServiceProvider ConfigureServices(IServiceCollection services)
    {
        services.AddMvc();
        // Add other framework services

        // Add Autofac
        var containerBuilder = new ContainerBuilder();
        containerBuilder.RegisterModule<DefaultModule>();
        containerBuilder.Populate(services);
        var container = containerBuilder.Build();
        return new AutofacServiceProvider(container);
    }
    ```

    Чтобы использовать сторонний контейнер, метод `Startup.ConfigureServices` должен возвращать `IServiceProvider`.

3. Настройте Autofac в `DefaultModule`.

    ```csharp
    public class DefaultModule : Module
    {
        protected override void Load(ContainerBuilder builder)
        {
            builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
        }
    }
    ```

В среде выполнения Autofac используется для разрешения типов и внедрения зависимостей. Дополнительные сведения об использовании Autofac с ASP.NET Core см. в [документации по Autofac](https://docs.autofac.org/en/latest/integration/aspnetcore.html).

### <a name="thread-safety"></a>Потокобезопасность

Одинарные службы должны быть потокобезопасными. Если одинарная служба имеет зависимость от временной службы, с учетом характера использования одинарной службой к этой временной службе также может предъявляться требование потокобезопасности.

Фабричный метод одной службы, например второй аргумент для [AddSingleton&lt;TService&gt;(IServiceCollection, Func&lt;IServiceProvider,TService&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addsingleton#Microsoft_Extensions_DependencyInjection_ServiceCollectionServiceExtensions_AddSingleton__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Func_System_IServiceProvider___0__), не обязательно должен быть потокобезопасным. Как и конструктор типов (`static`), он гарантированно будет вызываться один раз одним потоком.

## <a name="recommendations"></a>Рекомендации

При работе с внедрением зависимостей соблюдайте следующие рекомендации:

* Не храните данные и конфигурацию непосредственно в контейнере служб. Например, обычно не следует добавлять корзину пользователя в контейнер служб. Конфигурация должна использовать [шаблон параметров](xref:fundamentals/configuration/options). Аналогичным образом, избегайте объектов "хранения данных", которые служат лишь для доступа к некоторому другому объекту. По возможности лучше запросить фактический элемент через внедрение зависимостей.

* Не используйте статический доступ к службам (например, не используйте везде [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices)).

* Не используйте шаблон обнаружения службы (например, [IServiceProvider.GetService](/dotnet/api/system.iserviceprovider.getservice)).

* Не используйте статический доступ к `HttpContext` (например, [IHttpContextAccessor.HttpContext](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor.httpcontext)).

Как и с любыми рекомендациями, у вас могут возникнуть ситуации, когда нужно отступить от одного из правил. Такие исключения случаются редко. Главным образом они связаны с особенностями самой платформы.

Внедрение зависимостей является *альтернативой* для шаблонов доступа к статическим или глобальным объектам. Вы не сможете воспользоваться преимуществами внедрения зависимостей, если будете сочетать его с доступом к статическим объектам.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:fundamentals/repository-pattern>
* <xref:fundamentals/startup>
* <xref:test/index>
* <xref:fundamentals/middleware/extensibility>
* [Написание чистого кода в ASP.NET Core с внедрением зависимостей (MSDN)](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [Проектирование приложения на основе контейнеров. Вступление. К чему относится контейнер?](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)
* [Принцип явных зависимостей](https://deviq.com/explicit-dependencies-principle/)
* [Контейнеры с инверсией управления и шаблон внедрения зависимостей (Мартин Фаулер)](https://www.martinfowler.com/articles/injection.html)
* [Ключевое слово New — связующий элемент (связывание кода с определенной реализацией)](https://ardalis.com/new-is-glue)
