---
title: Внедрение зависимостей компонентов Razor
author: guardrex
description: См. в разделе, как Blazor и Razor компоненты приложения могут использовать службы, вставляя их в компоненты.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/27/2019
uid: razor-components/dependency-injection
ms.openlocfilehash: 40aec2e3a5032039c7d921f67d7d333b03c07fb1
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2019
ms.locfileid: "59515611"
---
# <a name="razor-components-dependency-injection"></a><span data-ttu-id="58038-103">Внедрение зависимостей компонентов Razor</span><span class="sxs-lookup"><span data-stu-id="58038-103">Razor Components dependency injection</span></span>

<span data-ttu-id="58038-104">По [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="58038-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="58038-105">Компоненты Razor поддерживает [внедрения зависимостей (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="58038-105">Razor Components supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="58038-106">Приложения могут использовать встроенные службы, вставляя их в компоненты.</span><span class="sxs-lookup"><span data-stu-id="58038-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="58038-107">Приложения можно определить и зарегистрировать пользовательские службы и сделать их доступными на протяжении всего приложения посредством внедрения Зависимостей.</span><span class="sxs-lookup"><span data-stu-id="58038-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="58038-108">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="58038-108">Dependency injection</span></span>

<span data-ttu-id="58038-109">Внедрение Зависимостей — это способ для доступа к службам, настроенным в центральном расположении.</span><span class="sxs-lookup"><span data-stu-id="58038-109">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="58038-110">Это может быть полезно в приложениях Razor компоненты:</span><span class="sxs-lookup"><span data-stu-id="58038-110">This can be useful in Razor Components apps to:</span></span>

* <span data-ttu-id="58038-111">Используют общий экземпляр класса службы через множества компонентов, известный как *одноэлементный* службы.</span><span class="sxs-lookup"><span data-stu-id="58038-111">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="58038-112">Отделять компоненты от классов конкретные службы с помощью ссылки абстракции.</span><span class="sxs-lookup"><span data-stu-id="58038-112">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="58038-113">Например, рассмотрим интерфейс `IDataAccess` для доступа к данным в приложении.</span><span class="sxs-lookup"><span data-stu-id="58038-113">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="58038-114">Интерфейс реализуется устойчивый `DataAccess` класса и зарегистрирован как служба в контейнере служб приложения.</span><span class="sxs-lookup"><span data-stu-id="58038-114">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="58038-115">Когда компонент использует внедрения Зависимостей для получения `IDataAccess` реализации, компонент не связан с конкретным типом.</span><span class="sxs-lookup"><span data-stu-id="58038-115">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="58038-116">Реализацию можно переключать, возможно для реализации макетов в модульных тестах.</span><span class="sxs-lookup"><span data-stu-id="58038-116">The implementation can be swapped, perhaps to a mock implementation in unit tests.</span></span>

<span data-ttu-id="58038-117">Дополнительные сведения см. в разделе <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="58038-117">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="add-services-to-di"></a><span data-ttu-id="58038-118">Добавление служб для внедрения Зависимостей</span><span class="sxs-lookup"><span data-stu-id="58038-118">Add services to DI</span></span>

<span data-ttu-id="58038-119">После создания нового приложения, изучите `Startup.ConfigureServices` метод:</span><span class="sxs-lookup"><span data-stu-id="58038-119">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="58038-120">`ConfigureServices` Методу передается <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, являющимся списком объектов дескриптора службы (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span><span class="sxs-lookup"><span data-stu-id="58038-120">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="58038-121">Службы добавляются, предоставляя дескрипторов службы для службы коллекции.</span><span class="sxs-lookup"><span data-stu-id="58038-121">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="58038-122">В следующем примере демонстрируется концепция с `IDataAccess` интерфейс и его конкретную реализацию `DataAccess`:</span><span class="sxs-lookup"><span data-stu-id="58038-122">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

<span data-ttu-id="58038-123">Службы могут быть настроены с временем существования, показано в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="58038-123">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="58038-124">Время существования</span><span class="sxs-lookup"><span data-stu-id="58038-124">Lifetime</span></span> | <span data-ttu-id="58038-125">Описание</span><span class="sxs-lookup"><span data-stu-id="58038-125">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="58038-126">Создает DI *единственных* службы.</span><span class="sxs-lookup"><span data-stu-id="58038-126">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="58038-127">Все компоненты, требующие `Singleton` служба получает экземпляр той же службе.</span><span class="sxs-lookup"><span data-stu-id="58038-127">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="58038-128">Каждый раз, когда компонент получает экземпляр `Transient` службы из контейнера служб, он получает *новый экземпляр* службы.</span><span class="sxs-lookup"><span data-stu-id="58038-128">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | <span data-ttu-id="58038-129">В настоящее время понятие областей внедрения Зависимостей не имеет Blazor на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="58038-129">Client-side Blazor doesn't currently have the concept of DI scopes.</span></span> <span data-ttu-id="58038-130">`Scoped` ведет себя как `Singleton`.</span><span class="sxs-lookup"><span data-stu-id="58038-130">`Scoped` behaves like `Singleton`.</span></span> <span data-ttu-id="58038-131">Тем не менее, ASP.NET Core Razor компоненты поддерживают `Scoped` времени существования.</span><span class="sxs-lookup"><span data-stu-id="58038-131">However, ASP.NET Core Razor Components support the `Scoped` lifetime.</span></span> <span data-ttu-id="58038-132">В компоненте Razor регистрация службы с заданной областью, ограничиваются соединения.</span><span class="sxs-lookup"><span data-stu-id="58038-132">In a Razor Component, a scoped service registration is scoped to the connection.</span></span> <span data-ttu-id="58038-133">По этой причине использование ограниченных служб является предпочтительным для служб, которые должны быть привязаны к текущему пользователю, даже если текущий предполагается выполняемых на стороне клиента в браузере.</span><span class="sxs-lookup"><span data-stu-id="58038-133">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |

<span data-ttu-id="58038-134">DI система основана на системе внедрения Зависимостей в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="58038-134">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="58038-135">Дополнительные сведения см. в разделе <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="58038-135">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="default-services"></a><span data-ttu-id="58038-136">Службы по умолчанию</span><span class="sxs-lookup"><span data-stu-id="58038-136">Default services</span></span>

<span data-ttu-id="58038-137">Службы по умолчанию автоматически добавляются к коллекции службы приложений.</span><span class="sxs-lookup"><span data-stu-id="58038-137">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="58038-138">Служба</span><span class="sxs-lookup"><span data-stu-id="58038-138">Service</span></span> | <span data-ttu-id="58038-139">Описание</span><span class="sxs-lookup"><span data-stu-id="58038-139">Description</span></span> |
| ------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="58038-140">Предоставляет методы для отправки HTTP-запросов и получения HTTP-ответов от ресурса с заданным URI (единственного экземпляра).</span><span class="sxs-lookup"><span data-stu-id="58038-140">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI (singleton).</span></span> <span data-ttu-id="58038-141">Обратите внимание, что этот экземпляр `HttpClient` использует браузер для обработки трафика HTTP в фоновом режиме.</span><span class="sxs-lookup"><span data-stu-id="58038-141">Note that this instance of `HttpClient` uses the browser for handling the HTTP traffic in the background.</span></span> <span data-ttu-id="58038-142">[HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) автоматически присваивается базовый префикс URI приложения.</span><span class="sxs-lookup"><span data-stu-id="58038-142">[HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) is automatically set to the base URI prefix of the app.</span></span> <span data-ttu-id="58038-143">`HttpClient` предоставляется только для приложений Blazor на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="58038-143">`HttpClient` is only provided to client-side Blazor apps.</span></span> |
| `IJSRuntime` | <span data-ttu-id="58038-144">Представляет экземпляр среды выполнения JavaScript, на который может быть отправлен вызовов.</span><span class="sxs-lookup"><span data-stu-id="58038-144">Represents an instance of a JavaScript runtime to which calls may be dispatched.</span></span> <span data-ttu-id="58038-145">Дополнительные сведения см. в разделе <xref:razor-components/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="58038-145">For more information, see <xref:razor-components/javascript-interop>.</span></span> |
| `IUriHelper` | <span data-ttu-id="58038-146">Содержит вспомогательные методы для работы с состоянием URI и навигации (единственного экземпляра).</span><span class="sxs-lookup"><span data-stu-id="58038-146">Contains helpers for working with URIs and navigation state (singleton).</span></span> <span data-ttu-id="58038-147">`IUriHelper` предоставляется Blazor и Razor компоненты приложения.</span><span class="sxs-lookup"><span data-stu-id="58038-147">`IUriHelper` is provided to both Blazor and Razor Components apps.</span></span> |

<span data-ttu-id="58038-148">Это можно использовать пользовательскую службу поставщика вместо поставщика службы по умолчанию, которые добавлены с помощью шаблона по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="58038-148">It's possible to use a custom service provider instead of the default service provider added by the default template.</span></span> <span data-ttu-id="58038-149">Поставщик пользовательская служба не предоставляет автоматически службы по умолчанию, перечисленных в таблице.</span><span class="sxs-lookup"><span data-stu-id="58038-149">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="58038-150">Если вы используете поставщик пользовательских служб и требовать каких-либо служб, приведенных в таблице, добавьте необходимые службы нового поставщика служб.</span><span class="sxs-lookup"><span data-stu-id="58038-150">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="58038-151">Запрос службы в компоненте</span><span class="sxs-lookup"><span data-stu-id="58038-151">Request a service in a component</span></span>

<span data-ttu-id="58038-152">После добавления в коллекцию службы служб внедрить службы в шаблоны Razor компонентов с помощью [ @inject ](xref:mvc/views/razor#section-4) директивы Razor.</span><span class="sxs-lookup"><span data-stu-id="58038-152">After services are added to the service collection, inject the services into the components' Razor templates using the [@inject](xref:mvc/views/razor#section-4) Razor directive.</span></span> <span data-ttu-id="58038-153">`@inject` имеет два параметра:</span><span class="sxs-lookup"><span data-stu-id="58038-153">`@inject` has two parameters:</span></span>

* <span data-ttu-id="58038-154">Имя типа: Тип службы для вставки.</span><span class="sxs-lookup"><span data-stu-id="58038-154">Type name: The type of the service to inject.</span></span>
* <span data-ttu-id="58038-155">Имя свойства: Имя свойства, получение внедренного приложения службы.</span><span class="sxs-lookup"><span data-stu-id="58038-155">Property name: The name of the property receiving the injected app service.</span></span> <span data-ttu-id="58038-156">Обратите внимание на то, что свойство не требует создания вручную.</span><span class="sxs-lookup"><span data-stu-id="58038-156">Note that the property doesn't require manual creation.</span></span> <span data-ttu-id="58038-157">Компилятор создает свойства.</span><span class="sxs-lookup"><span data-stu-id="58038-157">The compiler creates the property.</span></span>

<span data-ttu-id="58038-158">Дополнительные сведения см. в разделе <xref:mvc/views/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="58038-158">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="58038-159">Используйте несколько `@inject` инструкции для добавления различных служб.</span><span class="sxs-lookup"><span data-stu-id="58038-159">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="58038-160">В следующем примере показано, как использовать `@inject`.</span><span class="sxs-lookup"><span data-stu-id="58038-160">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="58038-161">Реализация службы `Services.IDataAccess` вставляется в свойство компонента `DataRepository`.</span><span class="sxs-lookup"><span data-stu-id="58038-161">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="58038-162">Обратите внимание на то, как код использует только `IDataAccess` абстракции:</span><span class="sxs-lookup"><span data-stu-id="58038-162">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.cshtml?highlight=2-3,23)]

<span data-ttu-id="58038-163">На внутреннем уровне созданное свойство (`DataRepository`) снабжен `InjectAttribute` атрибута.</span><span class="sxs-lookup"><span data-stu-id="58038-163">Internally, the generated property (`DataRepository`) is decorated with the `InjectAttribute` attribute.</span></span> <span data-ttu-id="58038-164">Как правило этот атрибут не используется напрямую.</span><span class="sxs-lookup"><span data-stu-id="58038-164">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="58038-165">Если базовый класс является обязательным для компонентов и внедренного свойства также требуются для базового класса, `InjectAttribute` можно добавлять вручную:</span><span class="sxs-lookup"><span data-stu-id="58038-165">If a base class is required for components and injected properties are also required for the base class, `InjectAttribute` can be manually added:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // Dependency injection works even if using the
    // InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="58038-166">В компонентах, производный от базового класса `@inject` директива не является обязательным.</span><span class="sxs-lookup"><span data-stu-id="58038-166">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="58038-167">`InjectAttribute` Базового класса достаточно:</span><span class="sxs-lookup"><span data-stu-id="58038-167">The `InjectAttribute` of the base class is sufficient:</span></span>

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="dependency-injection-in-services"></a><span data-ttu-id="58038-168">Внедрение зависимостей в службах</span><span class="sxs-lookup"><span data-stu-id="58038-168">Dependency injection in services</span></span>

<span data-ttu-id="58038-169">Сложные служб требуются дополнительные службы.</span><span class="sxs-lookup"><span data-stu-id="58038-169">Complex services might require additional services.</span></span> <span data-ttu-id="58038-170">В предыдущем примере `DataAccess` может потребоваться `HttpClient` службу по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="58038-170">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="58038-171">`@inject` (или `InjectAttribute`) недоступен для использования в службах.</span><span class="sxs-lookup"><span data-stu-id="58038-171">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="58038-172">*Внедрение через конструктор* следует использовать.</span><span class="sxs-lookup"><span data-stu-id="58038-172">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="58038-173">Путем добавления параметров в конструкторе службы добавляются необходимые службы.</span><span class="sxs-lookup"><span data-stu-id="58038-173">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="58038-174">Когда внедрение зависимостей создает службу, он распознает служб ему необходим в конструкторе и предоставляет их соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="58038-174">When dependency injection creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

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

<span data-ttu-id="58038-175">Предварительные требования для внедрения через конструктор:</span><span class="sxs-lookup"><span data-stu-id="58038-175">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="58038-176">Должен быть хотя бы один конструктор, аргументы которых все выполнения может использоваться внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="58038-176">There must be one constructor whose arguments can all be fulfilled by dependency injection.</span></span> <span data-ttu-id="58038-177">Обратите внимание, что дополнительные параметры, не охваченных DI разрешены в том случае, если они задают значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="58038-177">Note that additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="58038-178">Соответствующий конструктор должен быть *открытый*.</span><span class="sxs-lookup"><span data-stu-id="58038-178">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="58038-179">Должно существовать только один соответствующий конструктор.</span><span class="sxs-lookup"><span data-stu-id="58038-179">There must only be one applicable constructor.</span></span> <span data-ttu-id="58038-180">В случае неоднозначности DI вызывает исключение.</span><span class="sxs-lookup"><span data-stu-id="58038-180">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="58038-181">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="58038-181">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
