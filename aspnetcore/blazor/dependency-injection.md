---
title: ASP.NET Core внедрения зависимостей Блазор
author: guardrex
description: Узнайте, как приложения Блазор могут внедрять службы в компоненты.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/06/2019
uid: blazor/dependency-injection
ms.openlocfilehash: 0b48cd0cbe14d2b07627f56ab78611bbd3209fa1
ms.sourcegitcommit: 43c6335b5859282f64d66a7696c5935a2bcdf966
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/07/2019
ms.locfileid: "70800397"
---
# <a name="aspnet-core-blazor-dependency-injection"></a><span data-ttu-id="d4389-103">ASP.NET Core внедрения зависимостей Блазор</span><span class="sxs-lookup"><span data-stu-id="d4389-103">ASP.NET Core Blazor dependency injection</span></span>

<span data-ttu-id="d4389-104">По [Раинер стропек](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="d4389-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="d4389-105">Блазор поддерживает [внедрение зависимостей (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d4389-105">Blazor supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d4389-106">Приложения могут использовать встроенные службы, внедряя их в компоненты.</span><span class="sxs-lookup"><span data-stu-id="d4389-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="d4389-107">Приложения также могут определять и регистрировать пользовательские службы и предоставлять доступ к ним в рамках всего приложения с помощью ondi.</span><span class="sxs-lookup"><span data-stu-id="d4389-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

<span data-ttu-id="d4389-108">DI — это методика доступа к службам, настроенным в центральном расположении.</span><span class="sxs-lookup"><span data-stu-id="d4389-108">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="d4389-109">Это может быть полезно в приложениях Блазор для:</span><span class="sxs-lookup"><span data-stu-id="d4389-109">This can be useful in Blazor apps to:</span></span>

* <span data-ttu-id="d4389-110">Совместное использование одного экземпляра класса службы во множестве компонентов, называемом *одноэлементной* службой.</span><span class="sxs-lookup"><span data-stu-id="d4389-110">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="d4389-111">Отделение компонентов от конкретных классов служб с помощью абстракций ссылок.</span><span class="sxs-lookup"><span data-stu-id="d4389-111">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="d4389-112">Например, рассмотрим интерфейс `IDataAccess` для доступа к данным в приложении.</span><span class="sxs-lookup"><span data-stu-id="d4389-112">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="d4389-113">Интерфейс реализуется конкретным `DataAccess` классом и регистрируется как служба в контейнере службы приложения.</span><span class="sxs-lookup"><span data-stu-id="d4389-113">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="d4389-114">Когда компонент использует метод di для получения `IDataAccess` реализации, этот компонент не связан с конкретным типом.</span><span class="sxs-lookup"><span data-stu-id="d4389-114">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="d4389-115">Реализация может быть переключена, возможно, для реализации макета в модульных тестах.</span><span class="sxs-lookup"><span data-stu-id="d4389-115">The implementation can be swapped, perhaps for a mock implementation in unit tests.</span></span>

## <a name="default-services"></a><span data-ttu-id="d4389-116">Службы по умолчанию</span><span class="sxs-lookup"><span data-stu-id="d4389-116">Default services</span></span>

<span data-ttu-id="d4389-117">Службы по умолчанию автоматически добавляются в коллекцию служб приложения.</span><span class="sxs-lookup"><span data-stu-id="d4389-117">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="d4389-118">Служба</span><span class="sxs-lookup"><span data-stu-id="d4389-118">Service</span></span> | <span data-ttu-id="d4389-119">Время существования</span><span class="sxs-lookup"><span data-stu-id="d4389-119">Lifetime</span></span> | <span data-ttu-id="d4389-120">Описание</span><span class="sxs-lookup"><span data-stu-id="d4389-120">Description</span></span> |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="d4389-121">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="d4389-121">Singleton</span></span> | <span data-ttu-id="d4389-122">Предоставляет методы для отправки HTTP-запросов и получения HTTP-ответов от ресурса, идентифицируемого по универсальному коду ресурса (URI).</span><span class="sxs-lookup"><span data-stu-id="d4389-122">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI.</span></span> <span data-ttu-id="d4389-123">Обратите внимание, что `HttpClient` этот экземпляр использует браузер для обработки HTTP-трафика в фоновом режиме.</span><span class="sxs-lookup"><span data-stu-id="d4389-123">Note that this instance of `HttpClient` uses the browser for handling the HTTP traffic in the background.</span></span> <span data-ttu-id="d4389-124">[HttpClient. BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) автоматически устанавливается в качестве базового префикса URI приложения.</span><span class="sxs-lookup"><span data-stu-id="d4389-124">[HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) is automatically set to the base URI prefix of the app.</span></span> <span data-ttu-id="d4389-125">Дополнительные сведения см. в разделе <xref:blazor/call-web-api>.</span><span class="sxs-lookup"><span data-stu-id="d4389-125">For more information, see <xref:blazor/call-web-api>.</span></span> |
| `IJSRuntime` | <span data-ttu-id="d4389-126">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="d4389-126">Singleton</span></span> | <span data-ttu-id="d4389-127">Представляет экземпляр среды выполнения JavaScript, в которой отправляются вызовы JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d4389-127">Represents an instance of a JavaScript runtime where JavaScript calls are dispatched.</span></span> <span data-ttu-id="d4389-128">Дополнительные сведения см. в разделе <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="d4389-128">For more information, see <xref:blazor/javascript-interop>.</span></span> |
| `NavigationManager` | <span data-ttu-id="d4389-129">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="d4389-129">Singleton</span></span> | <span data-ttu-id="d4389-130">Содержит вспомогательные методы для работы с URI и состоянием навигации.</span><span class="sxs-lookup"><span data-stu-id="d4389-130">Contains helpers for working with URIs and navigation state.</span></span> <span data-ttu-id="d4389-131">Дополнительные сведения см. в разделе вспомогательные функции для [URI и состояний навигации](xref:blazor/routing#uri-and-navigation-state-helpers).</span><span class="sxs-lookup"><span data-stu-id="d4389-131">For more information, see [URI and navigation state helpers](xref:blazor/routing#uri-and-navigation-state-helpers).</span></span> |

<span data-ttu-id="d4389-132">Пользовательский поставщик услуг не предоставляет службы по умолчанию, перечисленные в таблице автоматически.</span><span class="sxs-lookup"><span data-stu-id="d4389-132">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="d4389-133">Если вы используете пользовательский поставщик услуг и требуете какой-либо из служб, показанных в таблице, добавьте необходимые службы к новому поставщику услуг.</span><span class="sxs-lookup"><span data-stu-id="d4389-133">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="add-services-to-an-app"></a><span data-ttu-id="d4389-134">Добавление служб в приложение</span><span class="sxs-lookup"><span data-stu-id="d4389-134">Add services to an app</span></span>

<span data-ttu-id="d4389-135">После создания нового приложения изучите `Startup.ConfigureServices` метод:</span><span class="sxs-lookup"><span data-stu-id="d4389-135">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="d4389-136">Методу передается <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>объект, который представляет собой список объектов дескриптора службы<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>(). `ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="d4389-136">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="d4389-137">Службы добавляются путем предоставления дескрипторов служб коллекции служб.</span><span class="sxs-lookup"><span data-stu-id="d4389-137">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="d4389-138">В следующем примере показана концепция с `IDataAccess` интерфейсом и его конкретной реализацией: `DataAccess`</span><span class="sxs-lookup"><span data-stu-id="d4389-138">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

<span data-ttu-id="d4389-139">Службы можно настроить с учетом времени существования, приведенного в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="d4389-139">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="d4389-140">Время существования</span><span class="sxs-lookup"><span data-stu-id="d4389-140">Lifetime</span></span> | <span data-ttu-id="d4389-141">Описание</span><span class="sxs-lookup"><span data-stu-id="d4389-141">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | <span data-ttu-id="d4389-142">В настоящее время приложения веб-сборки блазор не имеют концепции областей DI.</span><span class="sxs-lookup"><span data-stu-id="d4389-142">Blazor WebAssembly apps don't currently have a concept of DI scopes.</span></span> <span data-ttu-id="d4389-143">`Scoped`— зарегистрированные службы ведут `Singleton` себя как службы.</span><span class="sxs-lookup"><span data-stu-id="d4389-143">`Scoped`-registered services behave like `Singleton` services.</span></span> <span data-ttu-id="d4389-144">Однако модель размещения на стороне сервера поддерживает `Scoped` время существования.</span><span class="sxs-lookup"><span data-stu-id="d4389-144">However, the server-side hosting model supports the `Scoped` lifetime.</span></span> <span data-ttu-id="d4389-145">В приложениях Блазор Server регистрация службы с заданной областью ограничивается *соединением*.</span><span class="sxs-lookup"><span data-stu-id="d4389-145">In Blazor Server apps, a scoped service registration is scoped to the *connection*.</span></span> <span data-ttu-id="d4389-146">По этой причине использование служб с заданной областью предпочтительно для служб, которые должны быть ограничены текущим пользователем, даже если текущим намерением является запуск на стороне клиента в браузере.</span><span class="sxs-lookup"><span data-stu-id="d4389-146">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="d4389-147">DI создает *один экземпляр* службы.</span><span class="sxs-lookup"><span data-stu-id="d4389-147">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="d4389-148">Все компоненты, которым `Singleton` необходима служба, получают экземпляр той же службы.</span><span class="sxs-lookup"><span data-stu-id="d4389-148">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="d4389-149">Каждый раз, когда компонент получает экземпляр `Transient` службы из контейнера службы, он получает *новый экземпляр* службы.</span><span class="sxs-lookup"><span data-stu-id="d4389-149">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |

<span data-ttu-id="d4389-150">Система DI основана на системе DI в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d4389-150">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="d4389-151">Дополнительные сведения см. в разделе <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="d4389-151">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="d4389-152">Запрос службы в компоненте</span><span class="sxs-lookup"><span data-stu-id="d4389-152">Request a service in a component</span></span>

<span data-ttu-id="d4389-153">После добавления служб в коллекцию служб вставьте их в компоненты с помощью [ \@](xref:mvc/views/razor#inject) директивы вставки Razor.</span><span class="sxs-lookup"><span data-stu-id="d4389-153">After services are added to the service collection, inject the services into the components using the [\@inject](xref:mvc/views/razor#inject) Razor directive.</span></span> <span data-ttu-id="d4389-154">`@inject`имеет два параметра:</span><span class="sxs-lookup"><span data-stu-id="d4389-154">`@inject` has two parameters:</span></span>

* <span data-ttu-id="d4389-155">Введите &ndash; тип службы для вставки.</span><span class="sxs-lookup"><span data-stu-id="d4389-155">Type &ndash; The type of the service to inject.</span></span>
* <span data-ttu-id="d4389-156">Свойство &ndash; имя свойства, получающего внедренную службу приложений.</span><span class="sxs-lookup"><span data-stu-id="d4389-156">Property &ndash; The name of the property receiving the injected app service.</span></span> <span data-ttu-id="d4389-157">Свойство не требуется создавать вручную.</span><span class="sxs-lookup"><span data-stu-id="d4389-157">The property doesn't require manual creation.</span></span> <span data-ttu-id="d4389-158">Компилятор создает свойство.</span><span class="sxs-lookup"><span data-stu-id="d4389-158">The compiler creates the property.</span></span>

<span data-ttu-id="d4389-159">Дополнительные сведения см. в разделе <xref:mvc/views/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="d4389-159">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="d4389-160">Используйте несколько `@inject` инструкций для внедрения различных служб.</span><span class="sxs-lookup"><span data-stu-id="d4389-160">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="d4389-161">В следующем примере показано, как использовать `@inject`.</span><span class="sxs-lookup"><span data-stu-id="d4389-161">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="d4389-162">Реализация `Services.IDataAccess` службы внедряется в свойство `DataRepository`компонента.</span><span class="sxs-lookup"><span data-stu-id="d4389-162">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="d4389-163">Обратите внимание, что код использует `IDataAccess` только абстракцию:</span><span class="sxs-lookup"><span data-stu-id="d4389-163">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

<span data-ttu-id="d4389-164">На внутреннем уровне созданное свойство`DataRepository`() снабжено `InjectAttribute` атрибутом.</span><span class="sxs-lookup"><span data-stu-id="d4389-164">Internally, the generated property (`DataRepository`) is decorated with the `InjectAttribute` attribute.</span></span> <span data-ttu-id="d4389-165">Как правило, этот атрибут не используется напрямую.</span><span class="sxs-lookup"><span data-stu-id="d4389-165">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="d4389-166">Если базовый класс необходим для компонентов, а для базового класса также требуются подставляемые свойства, вручную добавьте `InjectAttribute`:</span><span class="sxs-lookup"><span data-stu-id="d4389-166">If a base class is required for components and injected properties are also required for the base class, manually add the `InjectAttribute`:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="d4389-167">В компонентах, производных от базового класса `@inject` , директива не требуется.</span><span class="sxs-lookup"><span data-stu-id="d4389-167">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="d4389-168">`InjectAttribute` Для базового класса достаточно:</span><span class="sxs-lookup"><span data-stu-id="d4389-168">The `InjectAttribute` of the base class is sufficient:</span></span>

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a><span data-ttu-id="d4389-169">Использование ondi в службах</span><span class="sxs-lookup"><span data-stu-id="d4389-169">Use DI in services</span></span>

<span data-ttu-id="d4389-170">Для сложных служб могут потребоваться дополнительные службы.</span><span class="sxs-lookup"><span data-stu-id="d4389-170">Complex services might require additional services.</span></span> <span data-ttu-id="d4389-171">В предыдущем примере `DataAccess` может `HttpClient` потребоваться служба по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d4389-171">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="d4389-172">`@inject`(или `InjectAttribute`) недоступен для использования в службах.</span><span class="sxs-lookup"><span data-stu-id="d4389-172">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="d4389-173">Вместо этого следует использовать *внедрение конструктора* .</span><span class="sxs-lookup"><span data-stu-id="d4389-173">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="d4389-174">Необходимые службы добавляются путем добавления параметров в конструктор службы.</span><span class="sxs-lookup"><span data-stu-id="d4389-174">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="d4389-175">Когда DI создает службу, она распознает необходимые службы в конструкторе и предоставляет их соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="d4389-175">When DI creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

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

<span data-ttu-id="d4389-176">Необходимые условия для внедрения конструктора:</span><span class="sxs-lookup"><span data-stu-id="d4389-176">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="d4389-177">Должен существовать один конструктор, аргументы которого могут быть выполнены методом DI.</span><span class="sxs-lookup"><span data-stu-id="d4389-177">One constructor must exist whose arguments can all be fulfilled by DI.</span></span> <span data-ttu-id="d4389-178">Дополнительные параметры, не охваченные DI, разрешены, если они указывают значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d4389-178">Additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="d4389-179">Применимый конструктор должен быть *открытым*.</span><span class="sxs-lookup"><span data-stu-id="d4389-179">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="d4389-180">Должен существовать один подходящий конструктор.</span><span class="sxs-lookup"><span data-stu-id="d4389-180">One applicable constructor must exist.</span></span> <span data-ttu-id="d4389-181">В случае неоднозначности DI выдает исключение.</span><span class="sxs-lookup"><span data-stu-id="d4389-181">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="utility-base-component-classes-to-manage-a-di-scope"></a><span data-ttu-id="d4389-182">Классы базовых компонентов служебной программы для управления областью DI</span><span class="sxs-lookup"><span data-stu-id="d4389-182">Utility base component classes to manage a DI scope</span></span>

<span data-ttu-id="d4389-183">В ASP.NET Core приложениях службы с областью действия обычно ограничены текущим запросом.</span><span class="sxs-lookup"><span data-stu-id="d4389-183">In ASP.NET Core apps, scoped services are typically scoped to the current request.</span></span> <span data-ttu-id="d4389-184">По завершении запроса все неограниченные или временные службы удаляются системой DI.</span><span class="sxs-lookup"><span data-stu-id="d4389-184">After the request completes, any scoped or transient services are disposed by the DI system.</span></span> <span data-ttu-id="d4389-185">В приложениях Блазор Server область запроса длится на время клиентского соединения, что может привести к тому, что временные и ограниченные службы будут работать намного дольше, чем ожидалось.</span><span class="sxs-lookup"><span data-stu-id="d4389-185">In Blazor Server apps, the request scope lasts for the duration of the client connection, which can result in transient and scoped services living much longer than expected.</span></span>

<span data-ttu-id="d4389-186">Чтобы ограничить время существования компонента службами, можно использовать `OwningComponentBase` базовые классы и. `OwningComponentBase<TService>`</span><span class="sxs-lookup"><span data-stu-id="d4389-186">To scope services to the lifetime of a component, can use the `OwningComponentBase` and `OwningComponentBase<TService>` base classes.</span></span> <span data-ttu-id="d4389-187">Эти базовые классы предоставляют `ScopedServices` свойство типа `IServiceProvider` , разрешающее службы, областью действия которых является время существования компонента.</span><span class="sxs-lookup"><span data-stu-id="d4389-187">These base classes expose a `ScopedServices` property of type `IServiceProvider` that resolve services that are scoped to the lifetime of the component.</span></span> <span data-ttu-id="d4389-188">Чтобы создать компонент, наследующий от базового класса в Razor, используйте `@inherits` директиву.</span><span class="sxs-lookup"><span data-stu-id="d4389-188">To author a component that inherits from a base class in Razor, use the `@inherits` directive.</span></span>

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
> <span data-ttu-id="d4389-189">Службы, внедренные в компонент с `@inject` помощью или `InjectAttribute` , не создаются в области компонента и привязаны к области запроса.</span><span class="sxs-lookup"><span data-stu-id="d4389-189">Services injected into the component using `@inject` or the `InjectAttribute` aren't created in the component's scope and are tied to the request scope.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d4389-190">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="d4389-190">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
