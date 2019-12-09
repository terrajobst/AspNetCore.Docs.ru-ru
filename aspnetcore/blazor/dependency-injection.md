---
title: ASP.NET Core внедрения зависимостей Blazor
author: guardrex
description: Узнайте, как Blazor приложения могут внедрять службы в компоненты.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
uid: blazor/dependency-injection
ms.openlocfilehash: aad6cfee500b5cb502470f6a4a7cb5756df09dc4
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/09/2019
ms.locfileid: "74943788"
---
# <a name="aspnet-core-opno-locblazor-dependency-injection"></a><span data-ttu-id="601b5-103">ASP.NET Core внедрения зависимостей Blazor</span><span class="sxs-lookup"><span data-stu-id="601b5-103">ASP.NET Core Blazor dependency injection</span></span>

<span data-ttu-id="601b5-104">По [Раинер стропек](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="601b5-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor<span data-ttu-id="601b5-105"> поддерживает [внедрение зависимостей (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="601b5-105"> supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="601b5-106">Приложения могут использовать встроенные службы, внедряя их в компоненты.</span><span class="sxs-lookup"><span data-stu-id="601b5-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="601b5-107">Приложения также могут определять и регистрировать пользовательские службы и предоставлять доступ к ним в рамках всего приложения с помощью ondi.</span><span class="sxs-lookup"><span data-stu-id="601b5-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

<span data-ttu-id="601b5-108">DI — это методика доступа к службам, настроенным в центральном расположении.</span><span class="sxs-lookup"><span data-stu-id="601b5-108">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="601b5-109">Это может быть полезно в Blazor приложениях для:</span><span class="sxs-lookup"><span data-stu-id="601b5-109">This can be useful in Blazor apps to:</span></span>

* <span data-ttu-id="601b5-110">Совместное использование одного экземпляра класса службы во множестве компонентов, называемом *одноэлементной* службой.</span><span class="sxs-lookup"><span data-stu-id="601b5-110">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="601b5-111">Отделение компонентов от конкретных классов служб с помощью абстракций ссылок.</span><span class="sxs-lookup"><span data-stu-id="601b5-111">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="601b5-112">Например, рассмотрим интерфейс `IDataAccess` для доступа к данным в приложении.</span><span class="sxs-lookup"><span data-stu-id="601b5-112">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="601b5-113">Интерфейс реализуется конкретным классом `DataAccess` и регистрируется как служба в контейнере службы приложения.</span><span class="sxs-lookup"><span data-stu-id="601b5-113">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="601b5-114">Когда компонент использует метод DI для получения реализации `IDataAccess`, этот компонент не связан с конкретным типом.</span><span class="sxs-lookup"><span data-stu-id="601b5-114">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="601b5-115">Реализация может быть переключена, возможно, для реализации макета в модульных тестах.</span><span class="sxs-lookup"><span data-stu-id="601b5-115">The implementation can be swapped, perhaps for a mock implementation in unit tests.</span></span>

## <a name="default-services"></a><span data-ttu-id="601b5-116">Службы по умолчанию</span><span class="sxs-lookup"><span data-stu-id="601b5-116">Default services</span></span>

<span data-ttu-id="601b5-117">Службы по умолчанию автоматически добавляются в коллекцию служб приложения.</span><span class="sxs-lookup"><span data-stu-id="601b5-117">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="601b5-118">Service</span><span class="sxs-lookup"><span data-stu-id="601b5-118">Service</span></span> | <span data-ttu-id="601b5-119">Время существования</span><span class="sxs-lookup"><span data-stu-id="601b5-119">Lifetime</span></span> | <span data-ttu-id="601b5-120">Описание</span><span class="sxs-lookup"><span data-stu-id="601b5-120">Description</span></span> |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="601b5-121">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="601b5-121">Singleton</span></span> | <span data-ttu-id="601b5-122">Предоставляет методы для отправки HTTP-запросов и получения HTTP-ответов от ресурса, идентифицируемого по универсальному коду ресурса (URI).</span><span class="sxs-lookup"><span data-stu-id="601b5-122">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI.</span></span><br><br><span data-ttu-id="601b5-123">Экземпляр `HttpClient` в приложении Blazor сборки использует браузер для обработки HTTP-трафика в фоновом режиме.</span><span class="sxs-lookup"><span data-stu-id="601b5-123">The instance of `HttpClient` in a Blazor WebAssembly app uses the browser for handling the HTTP traffic in the background.</span></span><br><br><span data-ttu-id="601b5-124">Серверные приложения Blazor не включают `HttpClient`, настроенные как службы, по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="601b5-124">Blazor Server apps don't include an `HttpClient` configured as a service by default.</span></span> <span data-ttu-id="601b5-125">Предоставьте `HttpClient` для Blazor серверного приложения.</span><span class="sxs-lookup"><span data-stu-id="601b5-125">Provide an `HttpClient` to a Blazor Server app.</span></span><br><br><span data-ttu-id="601b5-126">Для получения дополнительной информации см. <xref:blazor/call-web-api>.</span><span class="sxs-lookup"><span data-stu-id="601b5-126">For more information, see <xref:blazor/call-web-api>.</span></span> |
| `IJSRuntime` | <span data-ttu-id="601b5-127">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="601b5-127">Singleton</span></span> | <span data-ttu-id="601b5-128">Представляет экземпляр среды выполнения JavaScript, в которой отправляются вызовы JavaScript.</span><span class="sxs-lookup"><span data-stu-id="601b5-128">Represents an instance of a JavaScript runtime where JavaScript calls are dispatched.</span></span> <span data-ttu-id="601b5-129">Для получения дополнительной информации см. <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="601b5-129">For more information, see <xref:blazor/javascript-interop>.</span></span> |
| `NavigationManager` | <span data-ttu-id="601b5-130">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="601b5-130">Singleton</span></span> | <span data-ttu-id="601b5-131">Содержит вспомогательные методы для работы с URI и состоянием навигации.</span><span class="sxs-lookup"><span data-stu-id="601b5-131">Contains helpers for working with URIs and navigation state.</span></span> <span data-ttu-id="601b5-132">Дополнительные сведения см. в разделе вспомогательные функции для [URI и состояний навигации](xref:blazor/routing#uri-and-navigation-state-helpers).</span><span class="sxs-lookup"><span data-stu-id="601b5-132">For more information, see [URI and navigation state helpers](xref:blazor/routing#uri-and-navigation-state-helpers).</span></span> |

<span data-ttu-id="601b5-133">Пользовательский поставщик услуг не предоставляет службы по умолчанию, перечисленные в таблице автоматически.</span><span class="sxs-lookup"><span data-stu-id="601b5-133">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="601b5-134">Если вы используете пользовательский поставщик услуг и требуете какой-либо из служб, показанных в таблице, добавьте необходимые службы к новому поставщику услуг.</span><span class="sxs-lookup"><span data-stu-id="601b5-134">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="add-services-to-an-app"></a><span data-ttu-id="601b5-135">Добавление служб в приложение</span><span class="sxs-lookup"><span data-stu-id="601b5-135">Add services to an app</span></span>

<span data-ttu-id="601b5-136">После создания нового приложения изучите метод `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="601b5-136">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="601b5-137">Методу `ConfigureServices` передается <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, который представляет собой список объектов дескриптора службы (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span><span class="sxs-lookup"><span data-stu-id="601b5-137">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="601b5-138">Службы добавляются путем предоставления дескрипторов служб коллекции служб.</span><span class="sxs-lookup"><span data-stu-id="601b5-138">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="601b5-139">В следующем примере демонстрируется понятие с интерфейсом `IDataAccess` и его конкретной реализацией `DataAccess`.</span><span class="sxs-lookup"><span data-stu-id="601b5-139">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

<span data-ttu-id="601b5-140">Службы можно настроить с учетом времени существования, приведенного в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="601b5-140">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="601b5-141">Время существования</span><span class="sxs-lookup"><span data-stu-id="601b5-141">Lifetime</span></span> | <span data-ttu-id="601b5-142">Описание</span><span class="sxs-lookup"><span data-stu-id="601b5-142">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Blazor<span data-ttu-id="601b5-143"> приложения веб-сборки в настоящее время не имеют концепции областей DI.</span><span class="sxs-lookup"><span data-stu-id="601b5-143"> WebAssembly apps don't currently have a concept of DI scopes.</span></span> <span data-ttu-id="601b5-144">`Scoped`-зарегистрированные службы ведут себя так же, как `Singleton` Services.</span><span class="sxs-lookup"><span data-stu-id="601b5-144">`Scoped`-registered services behave like `Singleton` services.</span></span> <span data-ttu-id="601b5-145">Однако модель размещения сервера Blazor поддерживает время существования `Scoped`.</span><span class="sxs-lookup"><span data-stu-id="601b5-145">However, the Blazor Server hosting model supports the `Scoped` lifetime.</span></span> <span data-ttu-id="601b5-146">В Blazor Server Apps регистрация службы с заданной областью ограничивается *подключением*.</span><span class="sxs-lookup"><span data-stu-id="601b5-146">In Blazor Server apps, a scoped service registration is scoped to the *connection*.</span></span> <span data-ttu-id="601b5-147">По этой причине использование служб с заданной областью предпочтительно для служб, которые должны быть ограничены текущим пользователем, даже если текущим намерением является запуск на стороне клиента в браузере.</span><span class="sxs-lookup"><span data-stu-id="601b5-147">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="601b5-148">DI создает *один экземпляр* службы.</span><span class="sxs-lookup"><span data-stu-id="601b5-148">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="601b5-149">Все компоненты, для которых необходима служба `Singleton`, получают экземпляр той же службы.</span><span class="sxs-lookup"><span data-stu-id="601b5-149">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="601b5-150">Каждый раз, когда компонент получает экземпляр службы `Transient` из контейнера службы, он получает *новый экземпляр* службы.</span><span class="sxs-lookup"><span data-stu-id="601b5-150">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |

<span data-ttu-id="601b5-151">Система DI основана на системе DI в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="601b5-151">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="601b5-152">Для получения дополнительной информации см. <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="601b5-152">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="601b5-153">Запрос службы в компоненте</span><span class="sxs-lookup"><span data-stu-id="601b5-153">Request a service in a component</span></span>

<span data-ttu-id="601b5-154">После добавления служб в коллекцию служб внедрите службы в компоненты, используя директиву [\@вставить](xref:mvc/views/razor#inject) Razor.</span><span class="sxs-lookup"><span data-stu-id="601b5-154">After services are added to the service collection, inject the services into the components using the [\@inject](xref:mvc/views/razor#inject) Razor directive.</span></span> <span data-ttu-id="601b5-155">`@inject` имеет два параметра:</span><span class="sxs-lookup"><span data-stu-id="601b5-155">`@inject` has two parameters:</span></span>

* <span data-ttu-id="601b5-156">Введите &ndash; тип службы для вставки.</span><span class="sxs-lookup"><span data-stu-id="601b5-156">Type &ndash; The type of the service to inject.</span></span>
* <span data-ttu-id="601b5-157">Свойство &ndash; имя свойства, получающего внедренную службу приложений.</span><span class="sxs-lookup"><span data-stu-id="601b5-157">Property &ndash; The name of the property receiving the injected app service.</span></span> <span data-ttu-id="601b5-158">Свойство не требуется создавать вручную.</span><span class="sxs-lookup"><span data-stu-id="601b5-158">The property doesn't require manual creation.</span></span> <span data-ttu-id="601b5-159">Компилятор создает свойство.</span><span class="sxs-lookup"><span data-stu-id="601b5-159">The compiler creates the property.</span></span>

<span data-ttu-id="601b5-160">Для получения дополнительной информации см. <xref:mvc/views/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="601b5-160">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="601b5-161">Используйте несколько инструкций `@inject` для внедрения различных служб.</span><span class="sxs-lookup"><span data-stu-id="601b5-161">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="601b5-162">В следующем примере показано, как использовать `@inject`.</span><span class="sxs-lookup"><span data-stu-id="601b5-162">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="601b5-163">Служба, реализующая `Services.IDataAccess`, внедряется в `DataRepository`свойств компонента.</span><span class="sxs-lookup"><span data-stu-id="601b5-163">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="601b5-164">Обратите внимание, что код использует только абстракцию `IDataAccess`:</span><span class="sxs-lookup"><span data-stu-id="601b5-164">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-razor[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

<span data-ttu-id="601b5-165">На внутреннем уровне создаваемое свойство (`DataRepository`) использует атрибут `InjectAttribute`.</span><span class="sxs-lookup"><span data-stu-id="601b5-165">Internally, the generated property (`DataRepository`) uses the `InjectAttribute` attribute.</span></span> <span data-ttu-id="601b5-166">Как правило, этот атрибут не используется напрямую.</span><span class="sxs-lookup"><span data-stu-id="601b5-166">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="601b5-167">Если базовый класс необходим для компонентов, а для базового класса также требуются обязательные свойства, добавьте `InjectAttribute`вручную:</span><span class="sxs-lookup"><span data-stu-id="601b5-167">If a base class is required for components and injected properties are also required for the base class, manually add the `InjectAttribute`:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="601b5-168">В компонентах, производных от базового класса, директива `@inject` не требуется.</span><span class="sxs-lookup"><span data-stu-id="601b5-168">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="601b5-169">`InjectAttribute` базового класса достаточно:</span><span class="sxs-lookup"><span data-stu-id="601b5-169">The `InjectAttribute` of the base class is sufficient:</span></span>

```razor
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a><span data-ttu-id="601b5-170">Использование ondi в службах</span><span class="sxs-lookup"><span data-stu-id="601b5-170">Use DI in services</span></span>

<span data-ttu-id="601b5-171">Для сложных служб могут потребоваться дополнительные службы.</span><span class="sxs-lookup"><span data-stu-id="601b5-171">Complex services might require additional services.</span></span> <span data-ttu-id="601b5-172">В предыдущем примере `DataAccess` может потребоваться служба `HttpClient` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="601b5-172">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="601b5-173">`@inject` (или `InjectAttribute`) недоступен для использования в службах.</span><span class="sxs-lookup"><span data-stu-id="601b5-173">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="601b5-174">Вместо этого следует использовать *внедрение конструктора* .</span><span class="sxs-lookup"><span data-stu-id="601b5-174">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="601b5-175">Необходимые службы добавляются путем добавления параметров в конструктор службы.</span><span class="sxs-lookup"><span data-stu-id="601b5-175">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="601b5-176">Когда DI создает службу, она распознает необходимые службы в конструкторе и предоставляет их соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="601b5-176">When DI creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

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

<span data-ttu-id="601b5-177">Необходимые условия для внедрения конструктора:</span><span class="sxs-lookup"><span data-stu-id="601b5-177">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="601b5-178">Должен существовать один конструктор, аргументы которого могут быть выполнены методом DI.</span><span class="sxs-lookup"><span data-stu-id="601b5-178">One constructor must exist whose arguments can all be fulfilled by DI.</span></span> <span data-ttu-id="601b5-179">Дополнительные параметры, не охваченные DI, разрешены, если они указывают значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="601b5-179">Additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="601b5-180">Применимый конструктор должен быть *открытым*.</span><span class="sxs-lookup"><span data-stu-id="601b5-180">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="601b5-181">Должен существовать один подходящий конструктор.</span><span class="sxs-lookup"><span data-stu-id="601b5-181">One applicable constructor must exist.</span></span> <span data-ttu-id="601b5-182">В случае неоднозначности DI выдает исключение.</span><span class="sxs-lookup"><span data-stu-id="601b5-182">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="utility-base-component-classes-to-manage-a-di-scope"></a><span data-ttu-id="601b5-183">Классы базовых компонентов служебной программы для управления областью DI</span><span class="sxs-lookup"><span data-stu-id="601b5-183">Utility base component classes to manage a DI scope</span></span>

<span data-ttu-id="601b5-184">В ASP.NET Core приложениях службы с областью действия обычно ограничены текущим запросом.</span><span class="sxs-lookup"><span data-stu-id="601b5-184">In ASP.NET Core apps, scoped services are typically scoped to the current request.</span></span> <span data-ttu-id="601b5-185">По завершении запроса все неограниченные или временные службы удаляются системой DI.</span><span class="sxs-lookup"><span data-stu-id="601b5-185">After the request completes, any scoped or transient services are disposed by the DI system.</span></span> <span data-ttu-id="601b5-186">В Blazor серверных приложений область запроса длится на время клиентского соединения, что может привести к тому, что временные и ограниченные службы будут работать намного дольше, чем ожидалось.</span><span class="sxs-lookup"><span data-stu-id="601b5-186">In Blazor Server apps, the request scope lasts for the duration of the client connection, which can result in transient and scoped services living much longer than expected.</span></span>

<span data-ttu-id="601b5-187">Чтобы ограничить время существования компонента службами, можно использовать базовые классы `OwningComponentBase` и `OwningComponentBase<TService>`.</span><span class="sxs-lookup"><span data-stu-id="601b5-187">To scope services to the lifetime of a component, can use the `OwningComponentBase` and `OwningComponentBase<TService>` base classes.</span></span> <span data-ttu-id="601b5-188">Эти базовые классы предоставляют `ScopedServices` свойство типа `IServiceProvider`, разрешающее службы, областью действия которых является компонент.</span><span class="sxs-lookup"><span data-stu-id="601b5-188">These base classes expose a `ScopedServices` property of type `IServiceProvider` that resolve services that are scoped to the lifetime of the component.</span></span> <span data-ttu-id="601b5-189">Чтобы создать компонент, наследующий от базового класса в Razor, используйте директиву `@inherits`.</span><span class="sxs-lookup"><span data-stu-id="601b5-189">To author a component that inherits from a base class in Razor, use the `@inherits` directive.</span></span>

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
> <span data-ttu-id="601b5-190">Службы, внедренные в компонент с помощью `@inject` или `InjectAttribute`, не создаются в области компонента и привязаны к области запроса.</span><span class="sxs-lookup"><span data-stu-id="601b5-190">Services injected into the component using `@inject` or the `InjectAttribute` aren't created in the component's scope and are tied to the request scope.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="601b5-191">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="601b5-191">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
