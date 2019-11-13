---
title: ASP.NET Core внедрения зависимостей Blazor
author: guardrex
description: Узнайте, как Blazor приложения могут внедрять службы в компоненты.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
no-loc:
- Blazor
uid: blazor/dependency-injection
ms.openlocfilehash: a39d913636afc55ac9d831de923ba7ae8db1216b
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963078"
---
# <a name="aspnet-core-opno-locblazor-dependency-injection"></a>ASP.NET Core внедрения зависимостей Blazor

По [Раинер стропек](https://www.timecockpit.com)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor поддерживает [внедрение зависимостей (DI)](xref:fundamentals/dependency-injection). Приложения могут использовать встроенные службы, внедряя их в компоненты. Приложения также могут определять и регистрировать пользовательские службы и предоставлять доступ к ним в рамках всего приложения с помощью ondi.

DI — это методика доступа к службам, настроенным в центральном расположении. Это может быть полезно в Blazor приложениях для:

* Совместное использование одного экземпляра класса службы во множестве компонентов, называемом *одноэлементной* службой.
* Отделение компонентов от конкретных классов служб с помощью абстракций ссылок. Например, рассмотрим интерфейс `IDataAccess` для доступа к данным в приложении. Интерфейс реализуется конкретным классом `DataAccess` и регистрируется как служба в контейнере службы приложения. Когда компонент использует метод DI для получения реализации `IDataAccess`, компонент не связан с конкретным типом. Реализация может быть переключена, возможно, для реализации макета в модульных тестах.

## <a name="default-services"></a>Службы по умолчанию

Службы по умолчанию автоматически добавляются в коллекцию служб приложения.

| Служба | Время существования | Описание |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | Одноэлементный | Предоставляет методы для отправки HTTP-запросов и получения HTTP-ответов от ресурса, идентифицируемого по универсальному коду ресурса (URI). Обратите внимание, что этот экземпляр `HttpClient` использует браузер для обработки HTTP-трафика в фоновом режиме. [HttpClient. BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) автоматически устанавливается в качестве базового префикса URI приложения. Для получения дополнительной информации см. <xref:blazor/call-web-api>. |
| `IJSRuntime` | Одноэлементный | Представляет экземпляр среды выполнения JavaScript, в которой отправляются вызовы JavaScript. Для получения дополнительной информации см. <xref:blazor/javascript-interop>. |
| `NavigationManager` | Одноэлементный | Содержит вспомогательные методы для работы с URI и состоянием навигации. Дополнительные сведения см. в разделе вспомогательные функции для [URI и состояний навигации](xref:blazor/routing#uri-and-navigation-state-helpers). |

Пользовательский поставщик услуг не предоставляет службы по умолчанию, перечисленные в таблице автоматически. Если вы используете пользовательский поставщик услуг и требуете какой-либо из служб, показанных в таблице, добавьте необходимые службы к новому поставщику услуг.

## <a name="add-services-to-an-app"></a>Добавление служб в приложение

После создания нового приложения изучите метод `Startup.ConfigureServices`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

Методу `ConfigureServices` передается <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, который представляет собой список объектов дескриптора службы (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>). Службы добавляются путем предоставления дескрипторов служб коллекции служб. В следующем примере показана концепция с интерфейсом `IDataAccess` и его конкретной реализацией `DataAccess`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

Службы можно настроить с учетом времени существования, приведенного в следующей таблице.

| Время существования | Описание |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Blazor приложения веб-сборки в настоящее время не имеют концепции областей DI. `Scoped`-зарегистрированные службы ведут себя так же, как `Singleton` Services. Однако модель размещения сервера Blazor поддерживает время существования `Scoped`. В Blazor Server Apps регистрация службы с заданной областью ограничивается *подключением*. По этой причине использование служб с заданной областью предпочтительно для служб, которые должны быть ограничены текущим пользователем, даже если текущим намерением является запуск на стороне клиента в браузере. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | DI создает *один экземпляр* службы. Все компоненты, для которых необходима служба `Singleton`, получают экземпляр той же службы. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | Каждый раз, когда компонент получает экземпляр службы `Transient` из контейнера службы, он получает *новый экземпляр* службы. |

Система DI основана на системе DI в ASP.NET Core. Для получения дополнительной информации см. <xref:fundamentals/dependency-injection>.

## <a name="request-a-service-in-a-component"></a>Запрос службы в компоненте

После добавления служб в коллекцию служб внедрите службы в компоненты с помощью директивы Razor [\@inject](xref:mvc/views/razor#inject) . `@inject` имеет два параметра:

* Введите &ndash; тип службы для вставки.
* Свойство &ndash; имя свойства, получающего внедренную службу приложений. Свойство не требуется создавать вручную. Компилятор создает свойство.

Для получения дополнительной информации см. <xref:mvc/views/dependency-injection>.

Используйте несколько инструкций `@inject` для внедрения различных служб.

В следующем примере показано, как использовать `@inject`. Служба, реализующая `Services.IDataAccess`, внедряется в свойство компонента `DataRepository`. Обратите внимание, что код использует только абстракцию `IDataAccess`:

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

На внутреннем уровне созданное свойство (`DataRepository`) дополнено атрибутом `InjectAttribute`. Как правило, этот атрибут не используется напрямую. Если базовый класс необходим для компонентов, а для базового класса также требуются обязательные свойства, вручную добавьте `InjectAttribute`:

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

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a>Использование ondi в службах

Для сложных служб могут потребоваться дополнительные службы. В предыдущем примере для `DataAccess` может потребоваться служба по умолчанию `HttpClient`. `@inject` (или `InjectAttribute`) недоступен для использования в службах. Вместо этого следует использовать *внедрение конструктора* . Необходимые службы добавляются путем добавления параметров в конструктор службы. Когда DI создает службу, она распознает необходимые службы в конструкторе и предоставляет их соответствующим образом.

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

Чтобы ограничить время существования компонента службами, можно использовать базовые классы `OwningComponentBase` и `OwningComponentBase<TService>`. Эти базовые классы предоставляют свойство `ScopedServices` типа `IServiceProvider`, которое разрешает службы, областью действия которых является компонент. Чтобы создать компонент, наследующий от базового класса в Razor, используйте директиву `@inherits`.

```cshtml
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
