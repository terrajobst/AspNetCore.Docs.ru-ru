---
title: Внедрение зависимостей в ASP.NET Core
author: guardrex
description: Сведения о том, как ASP.NET Core реализует внедрение зависимостей и как его использовать.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/05/2019
uid: fundamentals/dependency-injection
ms.openlocfilehash: c46e7322e86c2836a15bd0720995a8634bb185be
ms.sourcegitcommit: 897d4abff58505dae86b2947c5fe3d1b80d927f3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73634013"
---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="6642e-103">Внедрение зависимостей в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6642e-103">Dependency injection in ASP.NET Core</span></span>

<span data-ttu-id="6642e-104">Авторы: [Стив Смит](https://ardalis.com/) (Steve Smith), [Скотт Эдди](https://scottaddie.com) (Scott Addie) и [Люк Латам](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="6642e-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6642e-105">ASP.NET Core поддерживает проектирование программного обеспечения с возможностью внедрения зависимостей. При таком подходе достигается [инверсия управления](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) между классами и их зависимостями.</span><span class="sxs-lookup"><span data-stu-id="6642e-105">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) between classes and their dependencies.</span></span>

<span data-ttu-id="6642e-106">Дополнительные сведения о внедрении зависимостей в контроллерах MVC см. в статье <xref:mvc/controllers/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="6642e-106">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="6642e-107">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6642e-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="6642e-108">Общие сведения о внедрении зависимостей</span><span class="sxs-lookup"><span data-stu-id="6642e-108">Overview of dependency injection</span></span>

<span data-ttu-id="6642e-109">*Зависимость* — это любой объект, который требуется другому объекту.</span><span class="sxs-lookup"><span data-stu-id="6642e-109">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="6642e-110">Рассмотрим класс `MyDependency` с методом `WriteMessage`, от которого зависят другие классы в приложении.</span><span class="sxs-lookup"><span data-stu-id="6642e-110">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

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

<span data-ttu-id="6642e-111">Чтобы сделать метод `WriteMessage` доступным какому-то классу, можно создать экземпляр класса `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="6642e-111">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="6642e-112">Класс `MyDependency` выступает зависимостью класса `IndexModel`.</span><span class="sxs-lookup"><span data-stu-id="6642e-112">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

```csharp
public class IndexModel : PageModel
{
    MyDependency _dependency = new MyDependency();

    public async Task OnGetAsync()
    {
        await _dependency.WriteMessage(
            "IndexModel.OnGetAsync created this message.");
    }
}
```

<span data-ttu-id="6642e-113">Этот класс создает экземпляр `MyDependency` и напрямую зависит от него.</span><span class="sxs-lookup"><span data-stu-id="6642e-113">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="6642e-114">Зависимости в коде (как в предыдущем примере) представляют собой определенную проблему. Их следует избегать по следующим причинам:</span><span class="sxs-lookup"><span data-stu-id="6642e-114">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="6642e-115">Чтобы заменить `MyDependency` другой реализацией, класс необходимо изменить.</span><span class="sxs-lookup"><span data-stu-id="6642e-115">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="6642e-116">Если у `MyDependency` есть зависимости, их конфигурацию должен выполнять класс.</span><span class="sxs-lookup"><span data-stu-id="6642e-116">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="6642e-117">В больших проектах, когда от `MyDependency` зависят многие классы, код конфигурации растягивается по всему приложению.</span><span class="sxs-lookup"><span data-stu-id="6642e-117">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="6642e-118">Такая реализация плохо подходит для модульных тестов.</span><span class="sxs-lookup"><span data-stu-id="6642e-118">This implementation is difficult to unit test.</span></span> <span data-ttu-id="6642e-119">В приложении нужно использовать имитацию или заглушку в виде класса `MyDependency`, что при таком подходе невозможно.</span><span class="sxs-lookup"><span data-stu-id="6642e-119">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="6642e-120">Внедрение зависимостей устраняет эти проблемы следующим образом:</span><span class="sxs-lookup"><span data-stu-id="6642e-120">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="6642e-121">Используется интерфейс или базовый класс для абстрагирования реализации зависимостей.</span><span class="sxs-lookup"><span data-stu-id="6642e-121">The use of an interface or base class to abstract the dependency implementation.</span></span>
* <span data-ttu-id="6642e-122">Зависимость регистрируется в контейнере служб.</span><span class="sxs-lookup"><span data-stu-id="6642e-122">Registration of the dependency in a service container.</span></span> <span data-ttu-id="6642e-123">ASP.NET Core предоставляет встроенный контейнер служб, <xref:System.IServiceProvider>.</span><span class="sxs-lookup"><span data-stu-id="6642e-123">ASP.NET Core provides a built-in service container, <xref:System.IServiceProvider>.</span></span> <span data-ttu-id="6642e-124">Службы регистрируются в приложении в методе `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6642e-124">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="6642e-125">Служба *внедряется* в конструктор класса там, где он используется.</span><span class="sxs-lookup"><span data-stu-id="6642e-125">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="6642e-126">Платформа берет на себя создание экземпляра зависимости и его удаление, когда он больше не нужен.</span><span class="sxs-lookup"><span data-stu-id="6642e-126">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="6642e-127">В [примере приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) интерфейс `IMyDependency` определяет метод, который служба предоставляет приложению.</span><span class="sxs-lookup"><span data-stu-id="6642e-127">In the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6642e-128">Этот интерфейс реализуется конкретным типом, `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="6642e-128">This interface is implemented by a concrete type, `MyDependency`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6642e-129">`MyDependency` запрашивает <xref:Microsoft.Extensions.Logging.ILogger`1> в своем конструкторе.</span><span class="sxs-lookup"><span data-stu-id="6642e-129">`MyDependency` requests an <xref:Microsoft.Extensions.Logging.ILogger`1> in its constructor.</span></span> <span data-ttu-id="6642e-130">Использование цепочки внедрений зависимостей не является чем-то необычным.</span><span class="sxs-lookup"><span data-stu-id="6642e-130">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="6642e-131">Каждая запрашиваемая зависимость запрашивает собственные зависимости.</span><span class="sxs-lookup"><span data-stu-id="6642e-131">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="6642e-132">Контейнер разрешает зависимости в графе и возвращает полностью разрешенную службу.</span><span class="sxs-lookup"><span data-stu-id="6642e-132">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="6642e-133">Весь набор зависимостей, которые нужно разрешить, обычно называют *деревом зависимостей*, *графом зависимостей* или *графом объектов*.</span><span class="sxs-lookup"><span data-stu-id="6642e-133">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="6642e-134">`IMyDependency` и `ILogger<TCategoryName>` должны быть зарегистрированы в контейнере служб.</span><span class="sxs-lookup"><span data-stu-id="6642e-134">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="6642e-135">`IMyDependency` регистрируется в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6642e-135">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="6642e-136">Службу `ILogger<TCategoryName>` регистрирует инфраструктура абстракций ведения журналов, поэтому такая служба является [платформенной](#framework-provided-services), что означает, что ее по умолчанию регистрирует платформа.</span><span class="sxs-lookup"><span data-stu-id="6642e-136">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="6642e-137">Контейнер разрешает `ILogger<TCategoryName>`, используя преимущества [(универсальных) открытых типов](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), что устраняет необходимость регистрации каждого [(универсального) сконструированного типа](/dotnet/csharp/language-reference/language-specification/types#constructed-types).</span><span class="sxs-lookup"><span data-stu-id="6642e-137">The container resolves `ILogger<TCategoryName>` by taking advantage of [(generic) open types](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminating the need to register every [(generic) constructed type](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span></span>

```csharp
services.AddSingleton(typeof(ILogger<T>), typeof(Logger<T>));
```

<span data-ttu-id="6642e-138">В примере приложения служба `IMyDependency` зарегистрирована с конкретным типом `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="6642e-138">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="6642e-139">Регистрация регулирует время существования службы согласно времени существования одного запроса.</span><span class="sxs-lookup"><span data-stu-id="6642e-139">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="6642e-140">Подробнее о [времени существования служб](#service-lifetimes) мы поговорим далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="6642e-140">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="6642e-141">Каждый метод расширения `services.Add{SERVICE_NAME}` добавляет (а потенциально и настраивает) службы.</span><span class="sxs-lookup"><span data-stu-id="6642e-141">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="6642e-142">Например, `services.AddMvc()` добавляет службы, которые нужны для Razor Pages и MVC.</span><span class="sxs-lookup"><span data-stu-id="6642e-142">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="6642e-143">Рекомендуем придерживаться такого подхода в приложениях.</span><span class="sxs-lookup"><span data-stu-id="6642e-143">We recommended that apps follow this convention.</span></span> <span data-ttu-id="6642e-144">Поместите методы расширения в пространство имен [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection), чтобы инкапсулировать группы зарегистрированных служб.</span><span class="sxs-lookup"><span data-stu-id="6642e-144">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="6642e-145">Если конструктору службы требуется [встроенный тип](/dotnet/csharp/language-reference/keywords/built-in-types-table), например `string`, его можно внедрить с помощью [конфигурации](xref:fundamentals/configuration/index) или [шаблона параметров](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="6642e-145">If the service's constructor requires a [built in type](/dotnet/csharp/language-reference/keywords/built-in-types-table), such as a `string`, the type can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

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

<span data-ttu-id="6642e-146">Экземпляр службы запрашивается через конструктор класса, в котором эта служба используется и назначается скрытому полю.</span><span class="sxs-lookup"><span data-stu-id="6642e-146">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="6642e-147">Поле используется во всем классе для доступа к службе (по мере необходимости).</span><span class="sxs-lookup"><span data-stu-id="6642e-147">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="6642e-148">В примере приложения запрашивается экземпляр `IMyDependency`, который затем используется для вызова метода службы `WriteMessage`.</span><span class="sxs-lookup"><span data-stu-id="6642e-148">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

::: moniker-end

## <a name="services-injected-into-startup"></a><span data-ttu-id="6642e-149">Службы, внедренные в конструктор Startup</span><span class="sxs-lookup"><span data-stu-id="6642e-149">Services injected into Startup</span></span>

<span data-ttu-id="6642e-150">При использовании универсального узла (<xref:Microsoft.Extensions.Hosting.IHostBuilder>) в конструктор `Startup` могут внедряться только следующие типы служб:</span><span class="sxs-lookup"><span data-stu-id="6642e-150">Only the following service types can be injected into the `Startup` constructor when using the Generic Host (<xref:Microsoft.Extensions.Hosting.IHostBuilder>):</span></span>

* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

<span data-ttu-id="6642e-151">Службы можно внедрить в `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="6642e-151">Services can be injected into `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> options)
{
    ...
}
```

<span data-ttu-id="6642e-152">Дополнительные сведения можно найти по адресу: <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="6642e-152">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="6642e-153">Платформенные службы</span><span class="sxs-lookup"><span data-stu-id="6642e-153">Framework-provided services</span></span>

<span data-ttu-id="6642e-154">Метод `Startup.ConfigureServices` отвечает за определение служб, которые использует приложение, включая такие компоненты, как Entity Framework Core и ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="6642e-154">The `Startup.ConfigureServices` method is responsible for defining the services that the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="6642e-155">Изначально коллекция `IServiceCollection`, предоставленная для `ConfigureServices`, содержит определенные платформой службы (в зависимости от [настройки узла](xref:fundamentals/index#host)).</span><span class="sxs-lookup"><span data-stu-id="6642e-155">Initially, the `IServiceCollection` provided to `ConfigureServices` has services defined by the framework depending on [how the host was configured](xref:fundamentals/index#host).</span></span> <span data-ttu-id="6642e-156">Приложение, основанное на шаблоне ASP.NET Core, часто содержит сотни служб, зарегистрированных платформой.</span><span class="sxs-lookup"><span data-stu-id="6642e-156">It's not uncommon for an app based on an ASP.NET Core template to have hundreds of services registered by the framework.</span></span> <span data-ttu-id="6642e-157">В следующей таблице перечислены некоторые примеры зарегистрированных платформой служб.</span><span class="sxs-lookup"><span data-stu-id="6642e-157">A small sample of framework-registered services is listed in the following table.</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="6642e-158">Тип службы</span><span class="sxs-lookup"><span data-stu-id="6642e-158">Service Type</span></span> | <span data-ttu-id="6642e-159">Время существования</span><span class="sxs-lookup"><span data-stu-id="6642e-159">Lifetime</span></span> |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | <span data-ttu-id="6642e-160">Временный</span><span class="sxs-lookup"><span data-stu-id="6642e-160">Transient</span></span> |
| `IHostApplicationLifetime` | <span data-ttu-id="6642e-161">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="6642e-161">Singleton</span></span> |
| `IWebHostEnvironment` | <span data-ttu-id="6642e-162">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="6642e-162">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | <span data-ttu-id="6642e-163">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="6642e-163">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | <span data-ttu-id="6642e-164">Временный</span><span class="sxs-lookup"><span data-stu-id="6642e-164">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | <span data-ttu-id="6642e-165">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="6642e-165">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | <span data-ttu-id="6642e-166">Временный</span><span class="sxs-lookup"><span data-stu-id="6642e-166">Transient</span></span> |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | <span data-ttu-id="6642e-167">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="6642e-167">Singleton</span></span> |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | <span data-ttu-id="6642e-168">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="6642e-168">Singleton</span></span> |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | <span data-ttu-id="6642e-169">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="6642e-169">Singleton</span></span> |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | <span data-ttu-id="6642e-170">Временный</span><span class="sxs-lookup"><span data-stu-id="6642e-170">Transient</span></span> |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | <span data-ttu-id="6642e-171">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="6642e-171">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | <span data-ttu-id="6642e-172">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="6642e-172">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | <span data-ttu-id="6642e-173">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="6642e-173">Singleton</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="6642e-174">Тип службы</span><span class="sxs-lookup"><span data-stu-id="6642e-174">Service Type</span></span> | <span data-ttu-id="6642e-175">Время существования</span><span class="sxs-lookup"><span data-stu-id="6642e-175">Lifetime</span></span> |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | <span data-ttu-id="6642e-176">Временный</span><span class="sxs-lookup"><span data-stu-id="6642e-176">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime?displayProperty=fullName> | <span data-ttu-id="6642e-177">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="6642e-177">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment?displayProperty=fullName> | <span data-ttu-id="6642e-178">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="6642e-178">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | <span data-ttu-id="6642e-179">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="6642e-179">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | <span data-ttu-id="6642e-180">Временный</span><span class="sxs-lookup"><span data-stu-id="6642e-180">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | <span data-ttu-id="6642e-181">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="6642e-181">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | <span data-ttu-id="6642e-182">Временный</span><span class="sxs-lookup"><span data-stu-id="6642e-182">Transient</span></span> |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | <span data-ttu-id="6642e-183">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="6642e-183">Singleton</span></span> |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | <span data-ttu-id="6642e-184">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="6642e-184">Singleton</span></span> |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | <span data-ttu-id="6642e-185">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="6642e-185">Singleton</span></span> |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | <span data-ttu-id="6642e-186">Временный</span><span class="sxs-lookup"><span data-stu-id="6642e-186">Transient</span></span> |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | <span data-ttu-id="6642e-187">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="6642e-187">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | <span data-ttu-id="6642e-188">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="6642e-188">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | <span data-ttu-id="6642e-189">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="6642e-189">Singleton</span></span> |

::: moniker-end

## <a name="register-additional-services-with-extension-methods"></a><span data-ttu-id="6642e-190">Регистрация дополнительных служб с помощью методов расширения</span><span class="sxs-lookup"><span data-stu-id="6642e-190">Register additional services with extension methods</span></span>

<span data-ttu-id="6642e-191">Если зарегистрировать службу (включая ее зависимые службы, если нужно) можно, используя метод расширения коллекции служб, для регистрации всех служб, необходимых этой службе, рекомендуется использовать один метод расширения `Add{SERVICE_NAME}`.</span><span class="sxs-lookup"><span data-stu-id="6642e-191">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="6642e-192">Ниже приведен пример того, как добавить дополнительные службы в контейнер с помощью методов расширения [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) и <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>:</span><span class="sxs-lookup"><span data-stu-id="6642e-192">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) and <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    ...
}
```

<span data-ttu-id="6642e-193">Дополнительные сведения см. в разделе о классе <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> в документации по API-интерфейсам.</span><span class="sxs-lookup"><span data-stu-id="6642e-193">For more information, see the <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> class in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="6642e-194">Время существования служб</span><span class="sxs-lookup"><span data-stu-id="6642e-194">Service lifetimes</span></span>

<span data-ttu-id="6642e-195">Для каждой зарегистрированной службы выбирайте подходящее время существования.</span><span class="sxs-lookup"><span data-stu-id="6642e-195">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="6642e-196">Для служб ASP.NET можно настроить следующие параметры времени существования:</span><span class="sxs-lookup"><span data-stu-id="6642e-196">ASP.NET Core services can be configured with the following lifetimes:</span></span>

### <a name="transient"></a><span data-ttu-id="6642e-197">Временный</span><span class="sxs-lookup"><span data-stu-id="6642e-197">Transient</span></span>

<span data-ttu-id="6642e-198">Временные службы времени существования (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) создаются при каждом их запросе из контейнера служб.</span><span class="sxs-lookup"><span data-stu-id="6642e-198">Transient lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) are created each time they're requested from the service container.</span></span> <span data-ttu-id="6642e-199">Это время существования лучше всего подходит для простых служб без отслеживания состояния.</span><span class="sxs-lookup"><span data-stu-id="6642e-199">This lifetime works best for lightweight, stateless services.</span></span>

### <a name="scoped"></a><span data-ttu-id="6642e-200">Область действия</span><span class="sxs-lookup"><span data-stu-id="6642e-200">Scoped</span></span>

<span data-ttu-id="6642e-201">Службы времени существования с заданной областью (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) создаются один раз для каждого клиентского запроса (подключения).</span><span class="sxs-lookup"><span data-stu-id="6642e-201">Scoped lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) are created once per client request (connection).</span></span>

> [!WARNING]
> <span data-ttu-id="6642e-202">При использовании такой службы в ПО промежуточного слоя внедрите ее в метод `Invoke` или `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="6642e-202">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="6642e-203">Не внедряйте службу через внедрение конструктора, поскольку в таком случае служба будет вести себя как одноэлементный объект.</span><span class="sxs-lookup"><span data-stu-id="6642e-203">Don't inject via constructor injection because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="6642e-204">Дополнительные сведения можно найти по адресу: <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span><span class="sxs-lookup"><span data-stu-id="6642e-204">For more information, see <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span></span>

### <a name="singleton"></a><span data-ttu-id="6642e-205">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="6642e-205">Singleton</span></span>

<span data-ttu-id="6642e-206">Отдельные службы времени существования (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) создаются при первом запросе (или при выполнении `Startup.ConfigureServices`, когда экземпляр указан во время регистрации службы).</span><span class="sxs-lookup"><span data-stu-id="6642e-206">Singleton lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) are created the first time they're requested (or when `Startup.ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="6642e-207">Созданный экземпляр используют все последующие запросы.</span><span class="sxs-lookup"><span data-stu-id="6642e-207">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="6642e-208">Если в приложении нужно использовать одинарные службы, рекомендуется разрешить контейнеру служб управлять временем их существования.</span><span class="sxs-lookup"><span data-stu-id="6642e-208">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="6642e-209">Реализовывать конструктивный шаблон с одинарными службами и предоставлять пользовательский код для управления временем существования объекта в классе категорически не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="6642e-209">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="6642e-210">Разрешать регулируемую службу из одиночной нельзя.</span><span class="sxs-lookup"><span data-stu-id="6642e-210">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="6642e-211">При обработке последующих запросов это может вызвать неправильное состояние службы.</span><span class="sxs-lookup"><span data-stu-id="6642e-211">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

## <a name="service-registration-methods"></a><span data-ttu-id="6642e-212">Методы регистрации службы</span><span class="sxs-lookup"><span data-stu-id="6642e-212">Service registration methods</span></span>

<span data-ttu-id="6642e-213">Методы расширения регистрации службы предлагают перегрузки, которые полезны в определенных сценариях.</span><span class="sxs-lookup"><span data-stu-id="6642e-213">Service registration extension methods offer overloads that are useful in specific scenarios.</span></span>

| <span data-ttu-id="6642e-214">Метод</span><span class="sxs-lookup"><span data-stu-id="6642e-214">Method</span></span> | <span data-ttu-id="6642e-215">Автоматический</span><span class="sxs-lookup"><span data-stu-id="6642e-215">Automatic</span></span><br><span data-ttu-id="6642e-216">object</span><span class="sxs-lookup"><span data-stu-id="6642e-216">object</span></span><br><span data-ttu-id="6642e-217">удаление</span><span class="sxs-lookup"><span data-stu-id="6642e-217">disposal</span></span> | <span data-ttu-id="6642e-218">Несколько</span><span class="sxs-lookup"><span data-stu-id="6642e-218">Multiple</span></span><br><span data-ttu-id="6642e-219">реализации</span><span class="sxs-lookup"><span data-stu-id="6642e-219">implementations</span></span> | <span data-ttu-id="6642e-220">Передача аргументов</span><span class="sxs-lookup"><span data-stu-id="6642e-220">Pass args</span></span> |
| ------ | :-----------------------------: | :-------------------------: | :-------: |
| `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`<br><span data-ttu-id="6642e-221">Пример.</span><span class="sxs-lookup"><span data-stu-id="6642e-221">Example:</span></span><br>`services.AddSingleton<IMyDep, MyDep>();` | <span data-ttu-id="6642e-222">Yes</span><span class="sxs-lookup"><span data-stu-id="6642e-222">Yes</span></span> | <span data-ttu-id="6642e-223">Да</span><span class="sxs-lookup"><span data-stu-id="6642e-223">Yes</span></span> | <span data-ttu-id="6642e-224">Нет</span><span class="sxs-lookup"><span data-stu-id="6642e-224">No</span></span> |
| `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`<br><span data-ttu-id="6642e-225">Примеры</span><span class="sxs-lookup"><span data-stu-id="6642e-225">Examples:</span></span><br>`services.AddSingleton<IMyDep>(sp => new MyDep());`<br>`services.AddSingleton<IMyDep>(sp => new MyDep("A string!"));` | <span data-ttu-id="6642e-226">Yes</span><span class="sxs-lookup"><span data-stu-id="6642e-226">Yes</span></span> | <span data-ttu-id="6642e-227">Да</span><span class="sxs-lookup"><span data-stu-id="6642e-227">Yes</span></span> | <span data-ttu-id="6642e-228">Yes</span><span class="sxs-lookup"><span data-stu-id="6642e-228">Yes</span></span> |
| `Add{LIFETIME}<{IMPLEMENTATION}>()`<br><span data-ttu-id="6642e-229">Пример.</span><span class="sxs-lookup"><span data-stu-id="6642e-229">Example:</span></span><br>`services.AddSingleton<MyDep>();` | <span data-ttu-id="6642e-230">Yes</span><span class="sxs-lookup"><span data-stu-id="6642e-230">Yes</span></span> | <span data-ttu-id="6642e-231">Нет</span><span class="sxs-lookup"><span data-stu-id="6642e-231">No</span></span> | <span data-ttu-id="6642e-232">Нет</span><span class="sxs-lookup"><span data-stu-id="6642e-232">No</span></span> |
| `AddSingleton<{SERVICE}>(new {IMPLEMENTATION})`<br><span data-ttu-id="6642e-233">Примеры</span><span class="sxs-lookup"><span data-stu-id="6642e-233">Examples:</span></span><br>`services.AddSingleton<IMyDep>(new MyDep());`<br>`services.AddSingleton<IMyDep>(new MyDep("A string!"));` | <span data-ttu-id="6642e-234">Нет</span><span class="sxs-lookup"><span data-stu-id="6642e-234">No</span></span> | <span data-ttu-id="6642e-235">Да</span><span class="sxs-lookup"><span data-stu-id="6642e-235">Yes</span></span> | <span data-ttu-id="6642e-236">Yes</span><span class="sxs-lookup"><span data-stu-id="6642e-236">Yes</span></span> |
| `AddSingleton(new {IMPLEMENTATION})`<br><span data-ttu-id="6642e-237">Примеры</span><span class="sxs-lookup"><span data-stu-id="6642e-237">Examples:</span></span><br>`services.AddSingleton(new MyDep());`<br>`services.AddSingleton(new MyDep("A string!"));` | <span data-ttu-id="6642e-238">Нет</span><span class="sxs-lookup"><span data-stu-id="6642e-238">No</span></span> | <span data-ttu-id="6642e-239">Нет</span><span class="sxs-lookup"><span data-stu-id="6642e-239">No</span></span> | <span data-ttu-id="6642e-240">Yes</span><span class="sxs-lookup"><span data-stu-id="6642e-240">Yes</span></span> |

<span data-ttu-id="6642e-241">Дополнительные сведения об удалении типа см. в разделе [Удаление служб](#disposal-of-services).</span><span class="sxs-lookup"><span data-stu-id="6642e-241">For more information on type disposal, see the [Disposal of services](#disposal-of-services) section.</span></span> <span data-ttu-id="6642e-242">Распространенный сценарий для нескольких реализаций — [макеты типов для тестирования](xref:test/integration-tests#inject-mock-services).</span><span class="sxs-lookup"><span data-stu-id="6642e-242">A common scenario for multiple implementations is [mocking types for testing](xref:test/integration-tests#inject-mock-services).</span></span>

<span data-ttu-id="6642e-243">Методы `TryAdd{LIFETIME}` регистрируют службу только в том случае, если еще не зарегистрирована реализация.</span><span class="sxs-lookup"><span data-stu-id="6642e-243">`TryAdd{LIFETIME}` methods only register the service if there isn't already an implementation registered.</span></span>

<span data-ttu-id="6642e-244">В следующем примере первая строка регистрирует `MyDependency` для `IMyDependency`.</span><span class="sxs-lookup"><span data-stu-id="6642e-244">In the following example, the first line registers `MyDependency` for `IMyDependency`.</span></span> <span data-ttu-id="6642e-245">Вторая строка ничего не делает, поскольку у `IMyDependency` уже есть зарегистрированная реализация:</span><span class="sxs-lookup"><span data-stu-id="6642e-245">The second line has no effect because `IMyDependency` already has a registered implementation:</span></span>

```csharp
services.AddSingleton<IMyDependency, MyDependency>();
// The following line has no effect:
services.TryAddSingleton<IMyDependency, DifferentDependency>();
```

<span data-ttu-id="6642e-246">Дополнительные сведения можно найти в разделе</span><span class="sxs-lookup"><span data-stu-id="6642e-246">For more information, see:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAdd*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddTransient*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddScoped*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddSingleton*>

<span data-ttu-id="6642e-247">Методы [TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) регистрируют службу только в том случае, если еще не существует реализации *того же типа*.</span><span class="sxs-lookup"><span data-stu-id="6642e-247">[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) methods only register the service if there isn't already an implementation *of the same type*.</span></span> <span data-ttu-id="6642e-248">Несколько служб разрешается через `IEnumerable<{SERVICE}>`.</span><span class="sxs-lookup"><span data-stu-id="6642e-248">Multiple services are resolved via `IEnumerable<{SERVICE}>`.</span></span> <span data-ttu-id="6642e-249">При регистрации служб разработчик хочет добавить экземпляр только в том случае, если экземпляр такого типа еще не был добавлен.</span><span class="sxs-lookup"><span data-stu-id="6642e-249">When registering services, the developer only wants to add an instance if one of the same type hasn't already been added.</span></span> <span data-ttu-id="6642e-250">Как правило, этот метод используется авторами библиотек, чтобы избежать регистрации двух копий экземпляра в контейнере.</span><span class="sxs-lookup"><span data-stu-id="6642e-250">Generally, this method is used by library authors to avoid registering two copies of an instance in the container.</span></span>

<span data-ttu-id="6642e-251">В следующем примере первая строка регистрирует `MyDep` для `IMyDep1`.</span><span class="sxs-lookup"><span data-stu-id="6642e-251">In the following example, the first line registers `MyDep` for `IMyDep1`.</span></span> <span data-ttu-id="6642e-252">Вторая строка регистрирует `MyDep` для `IMyDep2`.</span><span class="sxs-lookup"><span data-stu-id="6642e-252">The second line registers `MyDep` for `IMyDep2`.</span></span> <span data-ttu-id="6642e-253">Третья строка ничего не делает, поскольку у `IMyDep1` уже есть зарегистрированная реализация `MyDep`:</span><span class="sxs-lookup"><span data-stu-id="6642e-253">The third line has no effect because `IMyDep1` already has a registered implementation of `MyDep`:</span></span>

```csharp
public interface IMyDep1 {}
public interface IMyDep2 {}

public class MyDep : IMyDep1, IMyDep2 {}

services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep2, MyDep>());
// Two registrations of MyDep for IMyDep1 is avoided by the following line:
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
```

### <a name="constructor-injection-behavior"></a><span data-ttu-id="6642e-254">Поведение внедрения через конструктор</span><span class="sxs-lookup"><span data-stu-id="6642e-254">Constructor injection behavior</span></span>

<span data-ttu-id="6642e-255">Службы можно разрешать двумя методами:</span><span class="sxs-lookup"><span data-stu-id="6642e-255">Services can be resolved by two mechanisms:</span></span>

* <xref:System.IServiceProvider>
* <span data-ttu-id="6642e-256"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; Позволяет создавать объекты без регистрации службы в контейнере внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="6642e-256"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="6642e-257">`ActivatorUtilities` используется с абстракциями пользовательского уровня, например со вспомогательными функциями для тегов, контроллерами MVC, и связывателями моделей.</span><span class="sxs-lookup"><span data-stu-id="6642e-257">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, and model binders.</span></span>

<span data-ttu-id="6642e-258">Конструкторы могут принимать аргументы, которые не предоставляются внедрением зависимостей, но эти аргументы должны назначать значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6642e-258">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="6642e-259">Когда разрешение служб выполняется через `IServiceProvider` или `ActivatorUtilities`, для внедрения через конструктор требуется *открытый* конструктор.</span><span class="sxs-lookup"><span data-stu-id="6642e-259">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, constructor injection requires a *public* constructor.</span></span>

<span data-ttu-id="6642e-260">Когда разрешение служб выполняется через `ActivatorUtilities`, для внедрения с помощью конструктора требуется наличие только одного соответствующего конструктора.</span><span class="sxs-lookup"><span data-stu-id="6642e-260">When services are resolved by `ActivatorUtilities`, constructor injection requires that only one applicable constructor exists.</span></span> <span data-ttu-id="6642e-261">Перегрузки конструктора поддерживаются, но может существовать всего одна перегрузка, все аргументы которой могут быть обработаны с помощью внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="6642e-261">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="6642e-262">Контексты Entity Framework</span><span class="sxs-lookup"><span data-stu-id="6642e-262">Entity Framework contexts</span></span>

<span data-ttu-id="6642e-263">Контексты Entity Framework обычно добавляются в контейнер службы с помощью [времени существования с заданной областью](#service-lifetimes), поскольку операции базы данных в веб-приложении обычно относятся к области клиентского запроса.</span><span class="sxs-lookup"><span data-stu-id="6642e-263">Entity Framework contexts are usually added to the service container using the [scoped lifetime](#service-lifetimes) because web app database operations are normally scoped to the client request.</span></span> <span data-ttu-id="6642e-264">Время существования по умолчанию имеет заданную область, если время существования не указано в перегрузке [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) при регистрации контекста базы данных.</span><span class="sxs-lookup"><span data-stu-id="6642e-264">The default lifetime is scoped if a lifetime isn't specified by an [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) overload when registering the database context.</span></span> <span data-ttu-id="6642e-265">Службы данного времени существования не должны использовать контекст базы данных с временем существования короче, чем у службы.</span><span class="sxs-lookup"><span data-stu-id="6642e-265">Services of a given lifetime shouldn't use a database context with a shorter lifetime than the service.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="6642e-266">Параметры времени существования и регистрации</span><span class="sxs-lookup"><span data-stu-id="6642e-266">Lifetime and registration options</span></span>

<span data-ttu-id="6642e-267">Чтобы продемонстрировать различия между указанными вариантами времени существования и регистрации, рассмотрим интерфейсы, представляющие задачи в виде операции с уникальным идентификатором `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="6642e-267">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="6642e-268">В зависимости от того, как время существования службы операции настроено для этих интерфейсов, при запросе из класса контейнер предоставляет тот же или другой экземпляр службы.</span><span class="sxs-lookup"><span data-stu-id="6642e-268">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6642e-269">Интерфейсы реализованы в классе `Operation`.</span><span class="sxs-lookup"><span data-stu-id="6642e-269">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="6642e-270">Если идентификатор GUID не предоставлен, конструктор `Operation` создает его.</span><span class="sxs-lookup"><span data-stu-id="6642e-270">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6642e-271">Регистрируется служба `OperationService`, которая зависит от каждого из других типов `Operation`.</span><span class="sxs-lookup"><span data-stu-id="6642e-271">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="6642e-272">Когда служба `OperationService` запрашивается через внедрение зависимостей, она получает либо новый экземпляр каждой службы, либо экземпляр уже существующей службы в зависимости от времени существования зависимой службы.</span><span class="sxs-lookup"><span data-stu-id="6642e-272">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="6642e-273">Если при запросе из контейнера создаются временные службы, `OperationId` службы `IOperationTransient` отличается от `OperationId` службы `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="6642e-273">When transient services are created when requested from the container, the `OperationId` of the `IOperationTransient` service is different than the `OperationId` of the `OperationService`.</span></span> <span data-ttu-id="6642e-274">`OperationService` получает новый экземпляр класса `IOperationTransient`.</span><span class="sxs-lookup"><span data-stu-id="6642e-274">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="6642e-275">Новый экземпляр возвращает другой идентификатор `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="6642e-275">The new instance yields a different `OperationId`.</span></span>
* <span data-ttu-id="6642e-276">Если при каждом клиентском запросе создаются регулируемые службы, идентификатор `OperationId` службы `IOperationScoped` будет таким же, как для службы `OperationService` в клиентском запросе.</span><span class="sxs-lookup"><span data-stu-id="6642e-276">When scoped services are created per client request, the `OperationId` of the `IOperationScoped` service is the same as that of `OperationService` within a client request.</span></span> <span data-ttu-id="6642e-277">В разных клиентских запросах обе службы используют разные значения `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="6642e-277">Across client requests, both services share a different `OperationId` value.</span></span>
* <span data-ttu-id="6642e-278">Если одинарные службы и службы с одинарным экземпляром создаются один раз и используются во всех клиентских запросах и службах, идентификатор `OperationId` будет одинаковым во всех запросах служб.</span><span class="sxs-lookup"><span data-stu-id="6642e-278">When singleton and singleton-instance services are created once and used across all client requests and all services, the `OperationId` is constant across all service requests.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6642e-279">В `Startup.ConfigureServices` каждый тип добавляется в контейнер согласно его времени существования.</span><span class="sxs-lookup"><span data-stu-id="6642e-279">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

::: moniker-end

<span data-ttu-id="6642e-280">Служба `IOperationSingletonInstance` использует определенный экземпляр с известным идентификатором `Guid.Empty`.</span><span class="sxs-lookup"><span data-stu-id="6642e-280">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="6642e-281">Понятно, когда этот тип используется (его GUID — все нули).</span><span class="sxs-lookup"><span data-stu-id="6642e-281">It's clear when this type is in use (its GUID is all zeroes).</span></span>

<span data-ttu-id="6642e-282">В примере приложения показано время существования объектов в пределах одного и нескольких запросов.</span><span class="sxs-lookup"><span data-stu-id="6642e-282">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="6642e-283">В примере приложения `IndexModel` запрашивает каждый вид типа `IOperation`, а также `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="6642e-283">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="6642e-284">После этого на странице отображаются все значения `OperationId` службы и класса модели страниц в виде назначенных свойств.</span><span class="sxs-lookup"><span data-stu-id="6642e-284">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

::: moniker-end

<span data-ttu-id="6642e-285">Результаты двух запросов будут выглядеть так:</span><span class="sxs-lookup"><span data-stu-id="6642e-285">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="6642e-286">**Первый запрос**</span><span class="sxs-lookup"><span data-stu-id="6642e-286">**First request:**</span></span>

<span data-ttu-id="6642e-287">Операции контроллера:</span><span class="sxs-lookup"><span data-stu-id="6642e-287">Controller operations:</span></span>

<span data-ttu-id="6642e-288">Transient: d233e165-f417-469b-a866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="6642e-288">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="6642e-289">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="6642e-289">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="6642e-290">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="6642e-290">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="6642e-291">Instance: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="6642e-291">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="6642e-292">Операции `OperationService`:</span><span class="sxs-lookup"><span data-stu-id="6642e-292">`OperationService` operations:</span></span>

<span data-ttu-id="6642e-293">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="6642e-293">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="6642e-294">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="6642e-294">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="6642e-295">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="6642e-295">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="6642e-296">Instance: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="6642e-296">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="6642e-297">**Второй запрос**</span><span class="sxs-lookup"><span data-stu-id="6642e-297">**Second request:**</span></span>

<span data-ttu-id="6642e-298">Операции контроллера:</span><span class="sxs-lookup"><span data-stu-id="6642e-298">Controller operations:</span></span>

<span data-ttu-id="6642e-299">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="6642e-299">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="6642e-300">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="6642e-300">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="6642e-301">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="6642e-301">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="6642e-302">Instance: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="6642e-302">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="6642e-303">Операции `OperationService`:</span><span class="sxs-lookup"><span data-stu-id="6642e-303">`OperationService` operations:</span></span>

<span data-ttu-id="6642e-304">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="6642e-304">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="6642e-305">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="6642e-305">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="6642e-306">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="6642e-306">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="6642e-307">Instance: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="6642e-307">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="6642e-308">Проанализируйте, какие значения `OperationId` изменяются в пределах одного и нескольких запросов.</span><span class="sxs-lookup"><span data-stu-id="6642e-308">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="6642e-309">*Временные* объекты всегда разные.</span><span class="sxs-lookup"><span data-stu-id="6642e-309">*Transient* objects are always different.</span></span> <span data-ttu-id="6642e-310">Временное значение `OperationId` отличается в первом и втором клиентских запросах, а также в обеих операциях `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="6642e-310">The transient `OperationId` value for both the first and second client requests are different for both `OperationService` operations and across client requests.</span></span> <span data-ttu-id="6642e-311">Каждому запросу службы и каждому клиентскому запросу предоставляется новый экземпляр.</span><span class="sxs-lookup"><span data-stu-id="6642e-311">A new instance is provided to each service request and client request.</span></span>
* <span data-ttu-id="6642e-312">*Ограниченные по области* объекты одинаковы в рамках клиентского запроса, но различаются для разных запросов.</span><span class="sxs-lookup"><span data-stu-id="6642e-312">*Scoped* objects are the same within a client request but different across client requests.</span></span>
* <span data-ttu-id="6642e-313">*Одинарные* объекты остаются неизменными для всех объектов и запросов независимо от того, предоставляется ли в `Startup.ConfigureServices` экземпляр `Operation`.</span><span class="sxs-lookup"><span data-stu-id="6642e-313">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `Startup.ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="6642e-314">Вызов служб из функции main</span><span class="sxs-lookup"><span data-stu-id="6642e-314">Call services from main</span></span>

<span data-ttu-id="6642e-315">Создайте <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> с [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) для разрешения службы с заданной областью в области приложения.</span><span class="sxs-lookup"><span data-stu-id="6642e-315">Create an <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> with [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="6642e-316">Этот способ позволит получить доступ к службе с заданной областью при запуске для выполнения задач по инициализации.</span><span class="sxs-lookup"><span data-stu-id="6642e-316">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="6642e-317">В следующем примере показано, как получить контекст для `MyScopedService` в `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="6642e-317">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

::: moniker range=">= aspnetcore-3.0"

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

public class Program
{
    public static async Task Main(string[] args)
    {
        var host = CreateHostBuilder(args).Build();

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
    
        await host.RunAsync();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStartup<Startup>();
            });
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Logging;

public class Program
{
    public static async Task Main(string[] args)
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
    
        await host.RunAsync();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>();
}
```

::: moniker-end

## <a name="scope-validation"></a><span data-ttu-id="6642e-318">Проверка области</span><span class="sxs-lookup"><span data-stu-id="6642e-318">Scope validation</span></span>

<span data-ttu-id="6642e-319">Когда приложение выполняется в среде разработки, поставщик службы по умолчанию проверяет следующее:</span><span class="sxs-lookup"><span data-stu-id="6642e-319">When the app is running in the Development environment, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="6642e-320">Службы с заданной областью не разрешаются из корневого поставщика службы, прямо или косвенно.</span><span class="sxs-lookup"><span data-stu-id="6642e-320">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="6642e-321">Службы с заданной областью не вводятся в одноэлементные объекты, прямо или косвенно.</span><span class="sxs-lookup"><span data-stu-id="6642e-321">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="6642e-322">Корневой поставщик службы создается при вызове <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*>.</span><span class="sxs-lookup"><span data-stu-id="6642e-322">The root service provider is created when <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> is called.</span></span> <span data-ttu-id="6642e-323">Время существования корневого поставщика службы соответствует времени существования приложения или сервера — поставщик запускается с приложением и удаляется, когда приложение завершает работу.</span><span class="sxs-lookup"><span data-stu-id="6642e-323">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="6642e-324">Службы с заданной областью удаляются создавшим их контейнером.</span><span class="sxs-lookup"><span data-stu-id="6642e-324">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="6642e-325">Если служба с заданной областью создается в корневом контейнере, время существования службы повышается до уровня одноэлементного объекта, поскольку она удаляется только корневым контейнером при завершении работы приложения или сервера.</span><span class="sxs-lookup"><span data-stu-id="6642e-325">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="6642e-326">Проверка областей службы перехватывает эти ситуации при вызове `BuildServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="6642e-326">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="6642e-327">Дополнительные сведения можно найти по адресу: <xref:fundamentals/host/web-host#scope-validation>.</span><span class="sxs-lookup"><span data-stu-id="6642e-327">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="6642e-328">Службы запросов</span><span class="sxs-lookup"><span data-stu-id="6642e-328">Request Services</span></span>

<span data-ttu-id="6642e-329">Службы, доступные в пределах запроса ASP.NET Core из `HttpContext`, предоставляются через коллекцию [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices).</span><span class="sxs-lookup"><span data-stu-id="6642e-329">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) collection.</span></span>

<span data-ttu-id="6642e-330">Службы запросов — это все настроенные и запрашиваемые в приложении службы.</span><span class="sxs-lookup"><span data-stu-id="6642e-330">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="6642e-331">Когда объекты указывают зависимости, они удовлетворяются за счет типов из `RequestServices`, а не из `ApplicationServices`.</span><span class="sxs-lookup"><span data-stu-id="6642e-331">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="6642e-332">Как правило, использовать эти свойства в приложении напрямую не следует.</span><span class="sxs-lookup"><span data-stu-id="6642e-332">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="6642e-333">Вместо этого запрашивайте требуемые для классов типы через конструкторы классов и разрешите платформе внедрять зависимости.</span><span class="sxs-lookup"><span data-stu-id="6642e-333">Instead, request the types that classes require via class constructors and allow the framework inject the dependencies.</span></span> <span data-ttu-id="6642e-334">Этот процесс создает классы, которые легче тестировать.</span><span class="sxs-lookup"><span data-stu-id="6642e-334">This yields classes that are easier to test.</span></span>

> [!NOTE]
> <span data-ttu-id="6642e-335">Предпочтительнее запрашивать зависимости в качестве параметров конструктора, а не обращаться к коллекции `RequestServices`.</span><span class="sxs-lookup"><span data-stu-id="6642e-335">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="6642e-336">Проектирование служб для внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="6642e-336">Design services for dependency injection</span></span>

<span data-ttu-id="6642e-337">Придерживайтесь следующих рекомендаций:</span><span class="sxs-lookup"><span data-stu-id="6642e-337">Best practices are to:</span></span>

* <span data-ttu-id="6642e-338">Проектируйте службы так, чтобы для получения зависимостей они использовали внедрение зависимостей.</span><span class="sxs-lookup"><span data-stu-id="6642e-338">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="6642e-339">Избегайте статических классов и членов с отслеживанием состояния.</span><span class="sxs-lookup"><span data-stu-id="6642e-339">Avoid stateful, static classes and members.</span></span> <span data-ttu-id="6642e-340">Вместо этого следует проектировать приложения для использования отдельных служб, что позволяет избежать создания глобального состояния.</span><span class="sxs-lookup"><span data-stu-id="6642e-340">Design apps to use singleton services instead, which avoid creating global state.</span></span>
* <span data-ttu-id="6642e-341">Избегайте прямого создания экземпляров зависимых классов внутри служб.</span><span class="sxs-lookup"><span data-stu-id="6642e-341">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="6642e-342">Прямое создание экземпляров обязывает использовать в коде определенную реализацию.</span><span class="sxs-lookup"><span data-stu-id="6642e-342">Direct instantiation couples the code to a particular implementation.</span></span>
* <span data-ttu-id="6642e-343">Сделайте классы приложения небольшими, хорошо организованными и удобными в тестировании.</span><span class="sxs-lookup"><span data-stu-id="6642e-343">Make app classes small, well-factored, and easily tested.</span></span>

<span data-ttu-id="6642e-344">Если класс имеет слишком много внедренных зависимостей, обычно это указывает на то, что у класса слишком много задач и он не соответствует [принципу единственной обязанности](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span><span class="sxs-lookup"><span data-stu-id="6642e-344">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span></span> <span data-ttu-id="6642e-345">Попробуйте выполнить рефакторинг класса и перенести часть его обязанностей в новый класс.</span><span class="sxs-lookup"><span data-stu-id="6642e-345">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="6642e-346">Помните, что в классах модели страниц Razor Pages и классах контроллера MVC должны преимущественно выполняться задачи, связанные с пользовательским интерфейсом.</span><span class="sxs-lookup"><span data-stu-id="6642e-346">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="6642e-347">Бизнес-правила и реализация доступа к данным должны обрабатываться в классах, которые предназначены для [этих целей](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span><span class="sxs-lookup"><span data-stu-id="6642e-347">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="6642e-348">Удаление служб</span><span class="sxs-lookup"><span data-stu-id="6642e-348">Disposal of services</span></span>

<span data-ttu-id="6642e-349">Контейнер вызывает <xref:System.IDisposable.Dispose*> для создаваемых им типов <xref:System.IDisposable>.</span><span class="sxs-lookup"><span data-stu-id="6642e-349">The container calls <xref:System.IDisposable.Dispose*> for the <xref:System.IDisposable> types it creates.</span></span> <span data-ttu-id="6642e-350">Если экземпляр добавлен в контейнер с помощью пользовательского кода, он не будет удален автоматически.</span><span class="sxs-lookup"><span data-stu-id="6642e-350">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

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

## <a name="default-service-container-replacement"></a><span data-ttu-id="6642e-351">Замена стандартного контейнера служб</span><span class="sxs-lookup"><span data-stu-id="6642e-351">Default service container replacement</span></span>

<span data-ttu-id="6642e-352">Встроенный контейнер служб предназначен для удовлетворения потребностей платформы и большинства клиентских приложений.</span><span class="sxs-lookup"><span data-stu-id="6642e-352">The built-in service container is designed to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="6642e-353">Мы рекомендуем использовать встроенный контейнер, если только не требуется конкретная функциональная возможность, которую он не поддерживает, например:</span><span class="sxs-lookup"><span data-stu-id="6642e-353">We recommend using the built-in container unless you need a specific feature that the built-in container doesn't support, such as:</span></span>

* <span data-ttu-id="6642e-354">Внедрение свойств</span><span class="sxs-lookup"><span data-stu-id="6642e-354">Property injection</span></span>
* <span data-ttu-id="6642e-355">Внедрение по имени</span><span class="sxs-lookup"><span data-stu-id="6642e-355">Injection based on name</span></span>
* <span data-ttu-id="6642e-356">Дочерние контейнеры</span><span class="sxs-lookup"><span data-stu-id="6642e-356">Child containers</span></span>
* <span data-ttu-id="6642e-357">Настраиваемое управление временем существования</span><span class="sxs-lookup"><span data-stu-id="6642e-357">Custom lifetime management</span></span>
* <span data-ttu-id="6642e-358">`Func<T>` поддерживает отложенную инициализацию</span><span class="sxs-lookup"><span data-stu-id="6642e-358">`Func<T>` support for lazy initialization</span></span>

<span data-ttu-id="6642e-359">С приложениями ASP.NET Core можно использовать следующие сторонние контейнеры:</span><span class="sxs-lookup"><span data-stu-id="6642e-359">The following 3rd party containers can be used with ASP.NET Core apps:</span></span>

* <span data-ttu-id="6642e-360">[Autofac](https://autofac.readthedocs.io/en/latest/integration/aspnetcore.html);</span><span class="sxs-lookup"><span data-stu-id="6642e-360">[Autofac](https://autofac.readthedocs.io/en/latest/integration/aspnetcore.html)</span></span>
* <span data-ttu-id="6642e-361">[DryIoc](https://www.nuget.org/packages/DryIoc.Microsoft.DependencyInjection);</span><span class="sxs-lookup"><span data-stu-id="6642e-361">[DryIoc](https://www.nuget.org/packages/DryIoc.Microsoft.DependencyInjection)</span></span>
* <span data-ttu-id="6642e-362">[Grace](https://www.nuget.org/packages/Grace.DependencyInjection.Extensions);</span><span class="sxs-lookup"><span data-stu-id="6642e-362">[Grace](https://www.nuget.org/packages/Grace.DependencyInjection.Extensions)</span></span>
* <span data-ttu-id="6642e-363">[LightInject](https://github.com/seesharper/LightInject.Microsoft.DependencyInjection);</span><span class="sxs-lookup"><span data-stu-id="6642e-363">[LightInject](https://github.com/seesharper/LightInject.Microsoft.DependencyInjection)</span></span>
* <span data-ttu-id="6642e-364">[Lamar](https://jasperfx.github.io/lamar/);</span><span class="sxs-lookup"><span data-stu-id="6642e-364">[Lamar](https://jasperfx.github.io/lamar/)</span></span>
* <span data-ttu-id="6642e-365">[Stashbox](https://github.com/z4kn4fein/stashbox-extensions-dependencyinjection);</span><span class="sxs-lookup"><span data-stu-id="6642e-365">[Stashbox](https://github.com/z4kn4fein/stashbox-extensions-dependencyinjection)</span></span>
* [<span data-ttu-id="6642e-366">Unity</span><span class="sxs-lookup"><span data-stu-id="6642e-366">Unity</span></span>](https://www.nuget.org/packages/Unity.Microsoft.DependencyInjection)

### <a name="thread-safety"></a><span data-ttu-id="6642e-367">Потокобезопасность</span><span class="sxs-lookup"><span data-stu-id="6642e-367">Thread safety</span></span>

<span data-ttu-id="6642e-368">Создавайте потокобезопасные одноэлементные службы.</span><span class="sxs-lookup"><span data-stu-id="6642e-368">Create thread-safe singleton services.</span></span> <span data-ttu-id="6642e-369">Если одноэлементная служба имеет зависимость от временной службы, с учетом характера использования одноэлементной службой к этой временной службе также может предъявляться требование потокобезопасности.</span><span class="sxs-lookup"><span data-stu-id="6642e-369">If a singleton service has a dependency on a transient service, the transient service may also require thread safety depending how it's used by the singleton.</span></span>

<span data-ttu-id="6642e-370">Фабричный метод одной службы, например второй аргумент для [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), не обязательно должен быть потокобезопасным.</span><span class="sxs-lookup"><span data-stu-id="6642e-370">The factory method of single service, such as the second argument to [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), doesn't need to be thread-safe.</span></span> <span data-ttu-id="6642e-371">Как и конструктор типов (`static`), он гарантированно будет вызываться один раз одним потоком.</span><span class="sxs-lookup"><span data-stu-id="6642e-371">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="6642e-372">Рекомендации</span><span class="sxs-lookup"><span data-stu-id="6642e-372">Recommendations</span></span>

* <span data-ttu-id="6642e-373">Разрешение служб на основе `async/await` и `Task` не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="6642e-373">`async/await` and `Task` based service resolution is not supported.</span></span> <span data-ttu-id="6642e-374">C# не поддерживает асинхронные конструкторы, поэтому рекомендуем использовать асинхронные методы после асинхронного разрешения службы.</span><span class="sxs-lookup"><span data-stu-id="6642e-374">C# does not support asynchronous constructors; therefore, the recommended pattern is to use asynchronous methods after synchronously resolving the service.</span></span>

* <span data-ttu-id="6642e-375">Не храните данные и конфигурацию непосредственно в контейнере служб.</span><span class="sxs-lookup"><span data-stu-id="6642e-375">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="6642e-376">Например, обычно не следует добавлять корзину пользователя в контейнер служб.</span><span class="sxs-lookup"><span data-stu-id="6642e-376">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="6642e-377">Конфигурация должна использовать [шаблон параметров](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="6642e-377">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="6642e-378">Аналогичным образом, избегайте объектов "хранения данных", которые служат лишь для доступа к некоторому другому объекту.</span><span class="sxs-lookup"><span data-stu-id="6642e-378">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="6642e-379">Лучше запросить фактический элемент через внедрение зависимостей.</span><span class="sxs-lookup"><span data-stu-id="6642e-379">It's better to request the actual item via DI.</span></span>

* <span data-ttu-id="6642e-380">Не используйте статический доступ к службам (например, не используйте везде [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices)).</span><span class="sxs-lookup"><span data-stu-id="6642e-380">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) for use elsewhere).</span></span>

* <span data-ttu-id="6642e-381">Старайтесь не использовать *схему указателя служб*.</span><span class="sxs-lookup"><span data-stu-id="6642e-381">Avoid using the *service locator pattern*.</span></span> <span data-ttu-id="6642e-382">Например, не вызывайте <xref:System.IServiceProvider.GetService*> для получения экземпляра службы, когда можно использовать внедрение зависимостей:</span><span class="sxs-lookup"><span data-stu-id="6642e-382">For example, don't invoke <xref:System.IServiceProvider.GetService*> to obtain a service instance when you can use DI instead:</span></span>

  <span data-ttu-id="6642e-383">**Неправильно**:</span><span class="sxs-lookup"><span data-stu-id="6642e-383">**Incorrect:**</span></span>

  ```csharp
  public class MyClass()
  {
      public void MyMethod()
      {
          var optionsMonitor = 
              _services.GetService<IOptionsMonitor<MyOptions>>();
          var option = optionsMonitor.CurrentValue.Option;

          ...
      }
  }
  ```

  <span data-ttu-id="6642e-384">**Правильно**:</span><span class="sxs-lookup"><span data-stu-id="6642e-384">**Correct**:</span></span>

  ```csharp
  public class MyClass
  {
      private readonly IOptionsMonitor<MyOptions> _optionsMonitor;

      public MyClass(IOptionsMonitor<MyOptions> optionsMonitor)
      {
          _optionsMonitor = optionsMonitor;
      }

      public void MyMethod()
      {
          var option = _optionsMonitor.CurrentValue.Option;

          ...
      }
  }
  ```

* <span data-ttu-id="6642e-385">Другой вариант указателя службы, позволяющий избежать этого, — внедрение фабрики, которая разрешает зависимости во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="6642e-385">Another service locator variation to avoid is injecting a factory that resolves dependencies at runtime.</span></span> <span data-ttu-id="6642e-386">Оба метода смешивают стратегии [инверсии управления](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion).</span><span class="sxs-lookup"><span data-stu-id="6642e-386">Both of these practices mix [Inversion of Control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategies.</span></span>

* <span data-ttu-id="6642e-387">Не используйте статический доступ к `HttpContext` (например, [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span><span class="sxs-lookup"><span data-stu-id="6642e-387">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span></span>

<span data-ttu-id="6642e-388">Как и с любыми рекомендациями, у вас могут возникнуть ситуации, когда нужно отступить от одного из правил.</span><span class="sxs-lookup"><span data-stu-id="6642e-388">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="6642e-389">Такие исключения случаются редко. Главным образом они связаны с особенностями самой платформы.</span><span class="sxs-lookup"><span data-stu-id="6642e-389">Exceptions are rare&mdash;mostly special cases within the framework itself.</span></span>

<span data-ttu-id="6642e-390">Внедрение зависимостей является *альтернативой* для шаблонов доступа к статическим или глобальным объектам.</span><span class="sxs-lookup"><span data-stu-id="6642e-390">DI is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="6642e-391">Вы не сможете воспользоваться преимуществами внедрения зависимостей, если будете сочетать его с доступом к статическим объектам.</span><span class="sxs-lookup"><span data-stu-id="6642e-391">You may not be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6642e-392">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="6642e-392">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:blazor/dependency-injection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="6642e-393">Написание чистого кода в ASP.NET Core с внедрением зависимостей (MSDN)</span><span class="sxs-lookup"><span data-stu-id="6642e-393">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="6642e-394">Принцип явных зависимостей</span><span class="sxs-lookup"><span data-stu-id="6642e-394">Explicit Dependencies Principle</span></span>](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* [<span data-ttu-id="6642e-395">Контейнеры с инверсией управления и шаблон внедрения зависимостей (Мартин Фаулер)</span><span class="sxs-lookup"><span data-stu-id="6642e-395">Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)</span></span>](https://www.martinfowler.com/articles/injection.html)
* <span data-ttu-id="6642e-396">[How to register a service with multiple interfaces in ASP.NET Core DI](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/) (Регистрация службы с несколькими интерфейсами с помощью внедрения зависимостей ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="6642e-396">[How to register a service with multiple interfaces in ASP.NET Core DI](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)</span></span>
