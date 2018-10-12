---
title: Внедрение зависимостей в ASP.NET Core
author: guardrex
description: Сведения о том, как ASP.NET Core реализует внедрение зависимостей и как его использовать.
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2018
uid: fundamentals/dependency-injection
ms.openlocfilehash: 50986eeb4c5c8b06c739ee9f860665b877853d78
ms.sourcegitcommit: 517bb1366da2a28b0014e384fa379755c21b47d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/26/2018
ms.locfileid: "47230195"
---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="e77c5-103">Внедрение зависимостей в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e77c5-103">Dependency injection in ASP.NET Core</span></span>

<span data-ttu-id="e77c5-104">Авторы: [Стив Смит](https://ardalis.com/) (Steve Smith), [Скотт Эдди](https://scottaddie.com) (Scott Addie) и [Люк Латам](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="e77c5-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e77c5-105">ASP.NET Core поддерживает проектирование программного обеспечения с возможностью внедрения зависимостей. При таком подходе достигается [инверсия управления](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) между классами и их зависимостями.</span><span class="sxs-lookup"><span data-stu-id="e77c5-105">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) between classes and their dependencies.</span></span>

<span data-ttu-id="e77c5-106">Дополнительные сведения о внедрении зависимостей в контроллерах MVC см. в статье <xref:mvc/controllers/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="e77c5-106">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="e77c5-107">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e77c5-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="e77c5-108">Общие сведения о внедрении зависимостей</span><span class="sxs-lookup"><span data-stu-id="e77c5-108">Overview of dependency injection</span></span>

<span data-ttu-id="e77c5-109">*Зависимость* — это любой объект, который требуется другому объекту.</span><span class="sxs-lookup"><span data-stu-id="e77c5-109">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="e77c5-110">Рассмотрим класс `MyDependency` с методом `WriteMessage`, от которого зависят другие классы в приложении.</span><span class="sxs-lookup"><span data-stu-id="e77c5-110">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

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

<span data-ttu-id="e77c5-111">Чтобы сделать метод `WriteMessage` доступным какому-то классу, можно создать экземпляр класса `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="e77c5-111">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="e77c5-112">Класс `MyDependency` выступает зависимостью класса `IndexModel`.</span><span class="sxs-lookup"><span data-stu-id="e77c5-112">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

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

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="e77c5-113">Чтобы сделать метод `WriteMessage` доступным какому-то классу, можно создать экземпляр класса `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="e77c5-113">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="e77c5-114">Класс `MyDependency` выступает зависимостью класса `HomeController`.</span><span class="sxs-lookup"><span data-stu-id="e77c5-114">The `MyDependency` class is a dependency of the `HomeController` class:</span></span>

```csharp
public class HomeController : Controller
{
    MyDependency _dependency = new MyDependency();

    public async Task<IActionResult> Index()
    {
        await _dependency.WriteMessage(
            "HomeController.Index created this message.");

        return View();
    }
}
```

::: moniker-end

<span data-ttu-id="e77c5-115">Этот класс создает экземпляр `MyDependency` и напрямую зависит от него.</span><span class="sxs-lookup"><span data-stu-id="e77c5-115">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="e77c5-116">Зависимости в коде (как в предыдущем примере) представляют собой определенную проблему. Их следует избегать по следующим причинам:</span><span class="sxs-lookup"><span data-stu-id="e77c5-116">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="e77c5-117">Чтобы заменить `MyDependency` другой реализацией, класс необходимо изменить.</span><span class="sxs-lookup"><span data-stu-id="e77c5-117">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="e77c5-118">Если у `MyDependency` есть зависимости, их конфигурацию должен выполнять класс.</span><span class="sxs-lookup"><span data-stu-id="e77c5-118">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="e77c5-119">В больших проектах, когда от `MyDependency` зависят многие классы, код конфигурации растягивается по всему приложению.</span><span class="sxs-lookup"><span data-stu-id="e77c5-119">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="e77c5-120">Такая реализация плохо подходит для модульных тестов.</span><span class="sxs-lookup"><span data-stu-id="e77c5-120">This implementation is difficult to unit test.</span></span> <span data-ttu-id="e77c5-121">В приложении нужно использовать имитацию или заглушку в виде класса `MyDependency`, что при таком подходе невозможно.</span><span class="sxs-lookup"><span data-stu-id="e77c5-121">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="e77c5-122">Внедрение зависимостей устраняет эти проблемы следующим образом:</span><span class="sxs-lookup"><span data-stu-id="e77c5-122">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="e77c5-123">Используется интерфейс для абстрагирования реализации зависимостей.</span><span class="sxs-lookup"><span data-stu-id="e77c5-123">The use of an interface to abstract the dependency implementation.</span></span>
* <span data-ttu-id="e77c5-124">Зависимость регистрируется в контейнере служб.</span><span class="sxs-lookup"><span data-stu-id="e77c5-124">Registration of the dependency in a service container.</span></span> <span data-ttu-id="e77c5-125">ASP.NET Core предоставляет встроенный контейнер служб, [IServiceProvider](/dotnet/api/system.iserviceprovider).</span><span class="sxs-lookup"><span data-stu-id="e77c5-125">ASP.NET Core provides a built-in service container, [IServiceProvider](/dotnet/api/system.iserviceprovider).</span></span> <span data-ttu-id="e77c5-126">Службы регистрируются в приложении в методе `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e77c5-126">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="e77c5-127">Служба *внедряется* в конструктор класса там, где он используется.</span><span class="sxs-lookup"><span data-stu-id="e77c5-127">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="e77c5-128">Платформа берет на себя создание экземпляра зависимости и его удаление, когда он больше не нужен.</span><span class="sxs-lookup"><span data-stu-id="e77c5-128">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="e77c5-129">В [примере приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) интерфейс `IMyDependency` определяет метод, который служба предоставляет приложению.</span><span class="sxs-lookup"><span data-stu-id="e77c5-129">In the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e77c5-130">Этот интерфейс реализуется конкретным типом, `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="e77c5-130">This interface is implemented by a concrete type, `MyDependency`:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e77c5-131">`MyDependency` запрашивает [ILogger&lt;TCategoryName&gt;](/dotnet/api/microsoft.extensions.logging.ilogger-1) в своем конструкторе.</span><span class="sxs-lookup"><span data-stu-id="e77c5-131">`MyDependency` requests an [ILogger&lt;TCategoryName&gt;](/dotnet/api/microsoft.extensions.logging.ilogger-1) in its constructor.</span></span> <span data-ttu-id="e77c5-132">Использование цепочки внедрений зависимостей не является чем-то необычным.</span><span class="sxs-lookup"><span data-stu-id="e77c5-132">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="e77c5-133">Каждая запрашиваемая зависимость запрашивает собственные зависимости.</span><span class="sxs-lookup"><span data-stu-id="e77c5-133">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="e77c5-134">Контейнер разрешает зависимости в графе и возвращает полностью разрешенную службу.</span><span class="sxs-lookup"><span data-stu-id="e77c5-134">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="e77c5-135">Весь набор зависимостей, которые нужно разрешить, обычно называют *деревом зависимостей*, *графом зависимостей* или *графом объектов*.</span><span class="sxs-lookup"><span data-stu-id="e77c5-135">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="e77c5-136">`IMyDependency` и `ILogger<TCategoryName>` должны быть зарегистрированы в контейнере служб.</span><span class="sxs-lookup"><span data-stu-id="e77c5-136">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="e77c5-137">`IMyDependency` регистрируется в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e77c5-137">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="e77c5-138">Службу `ILogger<TCategoryName>` регистрирует инфраструктура абстракций ведения журналов, поэтому такая служба является [платформенной](#framework-provided-services), что означает, что ее по умолчанию регистрирует платформа.</span><span class="sxs-lookup"><span data-stu-id="e77c5-138">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="e77c5-139">В примере приложения служба `IMyDependency` зарегистрирована с конкретным типом `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="e77c5-139">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="e77c5-140">Регистрация регулирует время существования службы согласно времени существования одного запроса.</span><span class="sxs-lookup"><span data-stu-id="e77c5-140">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="e77c5-141">Подробнее о [времени существования служб](#service-lifetimes) мы поговорим далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="e77c5-141">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=11)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="e77c5-142">Каждый метод расширения `services.Add{SERVICE_NAME}` добавляет (а потенциально и настраивает) службы.</span><span class="sxs-lookup"><span data-stu-id="e77c5-142">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="e77c5-143">Например, `services.AddMvc()` добавляет службы, которые нужны для Razor Pages и MVC.</span><span class="sxs-lookup"><span data-stu-id="e77c5-143">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="e77c5-144">Рекомендуем придерживаться такого подхода в приложениях.</span><span class="sxs-lookup"><span data-stu-id="e77c5-144">We recommended that apps follow this convention.</span></span> <span data-ttu-id="e77c5-145">Поместите методы расширения в пространство имен [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection), чтобы инкапсулировать группы зарегистрированных служб.</span><span class="sxs-lookup"><span data-stu-id="e77c5-145">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="e77c5-146">Если конструктору службы требуется примитив, например `string`, его можно внедрить с помощью [конфигурации](xref:fundamentals/configuration/index) и [шаблона параметров](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="e77c5-146">If the service's constructor requires a primitive, such as a `string`, the primitive can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

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

<span data-ttu-id="e77c5-147">Экземпляр службы запрашивается через конструктор класса, в котором эта служба используется и назначается скрытому полю.</span><span class="sxs-lookup"><span data-stu-id="e77c5-147">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="e77c5-148">Поле используется во всем классе для доступа к службе (по мере необходимости).</span><span class="sxs-lookup"><span data-stu-id="e77c5-148">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="e77c5-149">В примере приложения запрашивается экземпляр `IMyDependency`, который затем используется для вызова метода службы `WriteMessage`.</span><span class="sxs-lookup"><span data-stu-id="e77c5-149">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Controllers/MyDependencyController.cs?name=snippet1&highlight=3,5-8,13-14)]

::: moniker-end

## <a name="framework-provided-services"></a><span data-ttu-id="e77c5-150">Платформенные службы</span><span class="sxs-lookup"><span data-stu-id="e77c5-150">Framework-provided services</span></span>

<span data-ttu-id="e77c5-151">Метод `Startup.ConfigureServices` отвечает за определение служб, которые использует приложение, включая такие компоненты, как Entity Framework Core и ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="e77c5-151">The `Startup.ConfigureServices` method is responsible for defining the services the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="e77c5-152">Изначально `IServiceCollection`, предоставленный `ConfigureServices`, имеет следующие определенные службы (в зависимости от [настройки узла](xref:fundamentals/host/index)):</span><span class="sxs-lookup"><span data-stu-id="e77c5-152">Initially, the `IServiceCollection` provided to `ConfigureServices` has the following services defined (depending on [how the host was configured](xref:fundamentals/host/index)):</span></span>

| <span data-ttu-id="e77c5-153">Тип службы</span><span class="sxs-lookup"><span data-stu-id="e77c5-153">Service Type</span></span> | <span data-ttu-id="e77c5-154">Время существования</span><span class="sxs-lookup"><span data-stu-id="e77c5-154">Lifetime</span></span> |
| ------------ | -------- |
| [<span data-ttu-id="e77c5-155">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span><span class="sxs-lookup"><span data-stu-id="e77c5-155">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | <span data-ttu-id="e77c5-156">Временный</span><span class="sxs-lookup"><span data-stu-id="e77c5-156">Transient</span></span> |
| [<span data-ttu-id="e77c5-157">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="e77c5-157">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | <span data-ttu-id="e77c5-158">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="e77c5-158">Singleton</span></span> |
| [<span data-ttu-id="e77c5-159">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="e77c5-159">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) | <span data-ttu-id="e77c5-160">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="e77c5-160">Singleton</span></span> |
| [<span data-ttu-id="e77c5-161">Microsoft.AspNetCore.Hosting.IStartup</span><span class="sxs-lookup"><span data-stu-id="e77c5-161">Microsoft.AspNetCore.Hosting.IStartup</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.istartup) | <span data-ttu-id="e77c5-162">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="e77c5-162">Singleton</span></span> |
| [<span data-ttu-id="e77c5-163">Microsoft.AspNetCore.Hosting.IStartupFilter</span><span class="sxs-lookup"><span data-stu-id="e77c5-163">Microsoft.AspNetCore.Hosting.IStartupFilter</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) | <span data-ttu-id="e77c5-164">Временный</span><span class="sxs-lookup"><span data-stu-id="e77c5-164">Transient</span></span> |
| [<span data-ttu-id="e77c5-165">Microsoft.AspNetCore.Hosting.Server.IServer</span><span class="sxs-lookup"><span data-stu-id="e77c5-165">Microsoft.AspNetCore.Hosting.Server.IServer</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) | <span data-ttu-id="e77c5-166">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="e77c5-166">Singleton</span></span> |
| [<span data-ttu-id="e77c5-167">Microsoft.AspNetCore.Http.IHttpContextFactory</span><span class="sxs-lookup"><span data-stu-id="e77c5-167">Microsoft.AspNetCore.Http.IHttpContextFactory</span></span>](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextfactory) | <span data-ttu-id="e77c5-168">Временный</span><span class="sxs-lookup"><span data-stu-id="e77c5-168">Transient</span></span> |
| [<span data-ttu-id="e77c5-169">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="e77c5-169">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.logging.ilogger) | <span data-ttu-id="e77c5-170">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="e77c5-170">Singleton</span></span> |
| [<span data-ttu-id="e77c5-171">Microsoft.Extensions.Logging.ILoggerFactory</span><span class="sxs-lookup"><span data-stu-id="e77c5-171">Microsoft.Extensions.Logging.ILoggerFactory</span></span>](/dotnet/api/microsoft.extensions.logging.iloggerfactory) | <span data-ttu-id="e77c5-172">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="e77c5-172">Singleton</span></span> |
| [<span data-ttu-id="e77c5-173">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span><span class="sxs-lookup"><span data-stu-id="e77c5-173">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span></span>](/dotnet/api/microsoft.extensions.objectpool.objectpoolprovider) | <span data-ttu-id="e77c5-174">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="e77c5-174">Singleton</span></span> |
| [<span data-ttu-id="e77c5-175">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="e77c5-175">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) | <span data-ttu-id="e77c5-176">Временный</span><span class="sxs-lookup"><span data-stu-id="e77c5-176">Transient</span></span> |
| [<span data-ttu-id="e77c5-177">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="e77c5-177">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.options.ioptions-1) | <span data-ttu-id="e77c5-178">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="e77c5-178">Singleton</span></span> |
| [<span data-ttu-id="e77c5-179">System.Diagnostics.DiagnosticSource</span><span class="sxs-lookup"><span data-stu-id="e77c5-179">System.Diagnostics.DiagnosticSource</span></span>](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | <span data-ttu-id="e77c5-180">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="e77c5-180">Singleton</span></span> |
| [<span data-ttu-id="e77c5-181">System.Diagnostics.DiagnosticListener</span><span class="sxs-lookup"><span data-stu-id="e77c5-181">System.Diagnostics.DiagnosticListener</span></span>](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | <span data-ttu-id="e77c5-182">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="e77c5-182">Singleton</span></span> |

<span data-ttu-id="e77c5-183">Если зарегистрировать службу (включая ее зависимые службы, если нужно) можно, используя метод расширения коллекции служб, для регистрации всех служб, необходимых этой службе, рекомендуется использовать один метод расширения `Add{SERVICE_NAME}`.</span><span class="sxs-lookup"><span data-stu-id="e77c5-183">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="e77c5-184">Ниже приведен пример того, как добавить дополнительные службы в контейнер с помощью методов расширения [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity) и [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc).</span><span class="sxs-lookup"><span data-stu-id="e77c5-184">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity), and [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc):</span></span>

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

<span data-ttu-id="e77c5-185">Дополнительные сведения см. в документации по API в статье о [классе ServiceCollection](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollection).</span><span class="sxs-lookup"><span data-stu-id="e77c5-185">For more information, see the [ServiceCollection Class](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollection) in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="e77c5-186">Время существования служб</span><span class="sxs-lookup"><span data-stu-id="e77c5-186">Service lifetimes</span></span>

<span data-ttu-id="e77c5-187">Для каждой зарегистрированной службы выбирайте подходящее время существования.</span><span class="sxs-lookup"><span data-stu-id="e77c5-187">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="e77c5-188">Для служб ASP.NET можно настроить следующие параметры времени существования:</span><span class="sxs-lookup"><span data-stu-id="e77c5-188">ASP.NET Core services can be configured with the following lifetimes:</span></span>

<span data-ttu-id="e77c5-189">**Временный**</span><span class="sxs-lookup"><span data-stu-id="e77c5-189">**Transient**</span></span>

<span data-ttu-id="e77c5-190">Временные службы времени существования создаются при каждом их запросе.</span><span class="sxs-lookup"><span data-stu-id="e77c5-190">Transient lifetime services are created each time they're requested.</span></span> <span data-ttu-id="e77c5-191">Это время существования лучше всего подходит для простых служб без отслеживания состояния.</span><span class="sxs-lookup"><span data-stu-id="e77c5-191">This lifetime works best for lightweight, stateless services.</span></span>

<span data-ttu-id="e77c5-192">**Ограниченный областью**</span><span class="sxs-lookup"><span data-stu-id="e77c5-192">**Scoped**</span></span>

<span data-ttu-id="e77c5-193">Службы времени существования с заданной областью создаются один раз для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="e77c5-193">Scoped lifetime services are created once per request.</span></span>

> [!WARNING]
> <span data-ttu-id="e77c5-194">При использовании такой службы в ПО промежуточного слоя внедрите ее в метод `Invoke` или `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="e77c5-194">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="e77c5-195">Не внедряйте службу через внедрение конструктора, поскольку в таком случае служба будет вести себя как одноэлементный объект.</span><span class="sxs-lookup"><span data-stu-id="e77c5-195">Don't inject via constructor injection because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="e77c5-196">Дополнительные сведения см. в разделе <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="e77c5-196">For more information, see <xref:fundamentals/middleware/index>.</span></span>

<span data-ttu-id="e77c5-197">**Одноэлементные**</span><span class="sxs-lookup"><span data-stu-id="e77c5-197">**Singleton**</span></span>

<span data-ttu-id="e77c5-198">Одинарные службы создаются при первом запросе (или при выполнении `ConfigureServices`, когда экземпляр указан во время регистрации службы) и существуют в единственном экземпляре.</span><span class="sxs-lookup"><span data-stu-id="e77c5-198">Singleton lifetime services are created the first time they're requested (or when `ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="e77c5-199">Созданный экземпляр используют все последующие запросы.</span><span class="sxs-lookup"><span data-stu-id="e77c5-199">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="e77c5-200">Если в приложении нужно использовать одинарные службы, рекомендуется разрешить контейнеру служб управлять временем их существования.</span><span class="sxs-lookup"><span data-stu-id="e77c5-200">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="e77c5-201">Реализовывать конструктивный шаблон с одинарными службами и предоставлять пользовательский код для управления временем существования объекта в классе категорически не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="e77c5-201">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="e77c5-202">Разрешать регулируемую службу из одиночной нельзя.</span><span class="sxs-lookup"><span data-stu-id="e77c5-202">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="e77c5-203">При обработке последующих запросов это может вызвать неправильное состояние службы.</span><span class="sxs-lookup"><span data-stu-id="e77c5-203">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

### <a name="constructor-injection-behavior"></a><span data-ttu-id="e77c5-204">Поведение внедрения через конструктор</span><span class="sxs-lookup"><span data-stu-id="e77c5-204">Constructor injection behavior</span></span>

<span data-ttu-id="e77c5-205">Службы можно разрешать двумя методами:</span><span class="sxs-lookup"><span data-stu-id="e77c5-205">Services can be resolved by two mechanisms:</span></span>

* `IServiceProvider`
* <span data-ttu-id="e77c5-206">[ActivatorUtilities.](/dotnet/api/microsoft.extensions.dependencyinjection.activatorutilities) Позволяет создавать объекты без регистрации службы в контейнере внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="e77c5-206">[ActivatorUtilities](/dotnet/api/microsoft.extensions.dependencyinjection.activatorutilities) &ndash; Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="e77c5-207">`ActivatorUtilities` используется с абстракциями пользовательского уровня, например со вспомогательными функциями для тегов, контроллерами MVC, и связывателями моделей.</span><span class="sxs-lookup"><span data-stu-id="e77c5-207">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, and model binders.</span></span>

<span data-ttu-id="e77c5-208">Конструкторы могут принимать аргументы, которые не предоставляются внедрением зависимостей, но эти аргументы должны назначать значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e77c5-208">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="e77c5-209">Когда разрешение служб выполняется через `IServiceProvider` или `ActivatorUtilities`, для внедрения через конструктор требуется *открытый* конструктор.</span><span class="sxs-lookup"><span data-stu-id="e77c5-209">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, constructor injection requires a *public* constructor.</span></span>

<span data-ttu-id="e77c5-210">Когда разрешение служб выполняется через `ActivatorUtilities`, для внедрения через конструктор требуется наличие только одного соответствующего конструктора.</span><span class="sxs-lookup"><span data-stu-id="e77c5-210">When services are resolved by `ActivatorUtilities`, constructor injection requires that only one applicable constructor exist.</span></span> <span data-ttu-id="e77c5-211">Перегрузки конструктора поддерживаются, но может существовать всего одна перегрузка, все аргументы которой могут быть обработаны с помощью внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="e77c5-211">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="e77c5-212">Контексты Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e77c5-212">Entity Framework contexts</span></span>

<span data-ttu-id="e77c5-213">Контексты Entity Framework нужно добавлять в контейнер служб с использованием регулируемого времени существования.</span><span class="sxs-lookup"><span data-stu-id="e77c5-213">Entity Framework contexts should be added to the service container using the scoped lifetime.</span></span> <span data-ttu-id="e77c5-214">Это делается автоматически с помощью вызова метода [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) при регистрации контекста базы данных.</span><span class="sxs-lookup"><span data-stu-id="e77c5-214">This is handled automatically with a call to the [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) method when registering the database context.</span></span> <span data-ttu-id="e77c5-215">Службы, использующие контекст базы данных, также должны иметь регулируемое время существования.</span><span class="sxs-lookup"><span data-stu-id="e77c5-215">Services that use the database context should also use the scoped lifetime.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="e77c5-216">Параметры времени существования и регистрации</span><span class="sxs-lookup"><span data-stu-id="e77c5-216">Lifetime and registration options</span></span>

<span data-ttu-id="e77c5-217">Чтобы продемонстрировать различия между указанными вариантами времени существования и регистрации, рассмотрим интерфейсы, представляющие задачи в виде операции с уникальным идентификатором `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="e77c5-217">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="e77c5-218">В зависимости от того, как время существования службы операции настроено для этих интерфейсов, при запросе из класса контейнер предоставляет тот же или другой экземпляр службы.</span><span class="sxs-lookup"><span data-stu-id="e77c5-218">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e77c5-219">Интерфейсы реализованы в классе `Operation`.</span><span class="sxs-lookup"><span data-stu-id="e77c5-219">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="e77c5-220">Если идентификатор GUID не предоставлен, конструктор `Operation` создает его.</span><span class="sxs-lookup"><span data-stu-id="e77c5-220">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e77c5-221">Регистрируется служба `OperationService`, которая зависит от каждого из других типов `Operation`.</span><span class="sxs-lookup"><span data-stu-id="e77c5-221">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="e77c5-222">Когда служба `OperationService` запрашивается через внедрение зависимостей, она получает либо новый экземпляр каждой службы, либо экземпляр уже существующей службы в зависимости от времени существования зависимой службы.</span><span class="sxs-lookup"><span data-stu-id="e77c5-222">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="e77c5-223">Если при запросе создаются временные службы, `OperationsId` службы `IOperationTransient` отличается от `OperationsId` службы `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="e77c5-223">If transient services are created when requested, the `OperationsId` of the `IOperationTransient` service is different than the `OperationsId` of the `OperationService`.</span></span> <span data-ttu-id="e77c5-224">`OperationService` получает новый экземпляр класса `IOperationTransient`.</span><span class="sxs-lookup"><span data-stu-id="e77c5-224">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="e77c5-225">Новый экземпляр возвращает другой идентификатор `OperationsId`.</span><span class="sxs-lookup"><span data-stu-id="e77c5-225">The new instance yields a different `OperationsId`.</span></span>
* <span data-ttu-id="e77c5-226">Если при запросе создаются регулируемые службы, идентификатор `OperationsId` службы `IOperationScoped` будет таким же, как для службы `OperationService` в запросе.</span><span class="sxs-lookup"><span data-stu-id="e77c5-226">If scoped services are created per request, the `OperationsId` of the `IOperationScoped` service is the same as that of `OperationService` within a request.</span></span> <span data-ttu-id="e77c5-227">В разных запросах обе службы используют разные значения `OperationsId`.</span><span class="sxs-lookup"><span data-stu-id="e77c5-227">Across requests, both services share a different `OperationsId` value.</span></span>
* <span data-ttu-id="e77c5-228">Если одинарные службы и службы с одинарным экземпляром создаются один раз и используются во всех запросах и службах, идентификатор `OperationsId` будет одинаковым во всех запросах служб.</span><span class="sxs-lookup"><span data-stu-id="e77c5-228">If singleton and singleton-instance services are created once and used across all requests and all services, the `OperationsId` is constant across all service requests.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e77c5-229">В `Startup.ConfigureServices` каждый тип добавляется в контейнер согласно его времени существования.</span><span class="sxs-lookup"><span data-stu-id="e77c5-229">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=12-15,18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

::: moniker-end

<span data-ttu-id="e77c5-230">Служба `IOperationSingletonInstance` использует определенный экземпляр с известным идентификатором `Guid.Empty`.</span><span class="sxs-lookup"><span data-stu-id="e77c5-230">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="e77c5-231">Понятно, когда этот тип используется (его GUID — все нули).</span><span class="sxs-lookup"><span data-stu-id="e77c5-231">It's clear when this type is in use (its GUID is all zeroes).</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="e77c5-232">В примере приложения показано время существования объектов в пределах одного и нескольких запросов.</span><span class="sxs-lookup"><span data-stu-id="e77c5-232">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="e77c5-233">В примере приложения `IndexModel` запрашивает каждый вид типа `IOperation`, а также `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="e77c5-233">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="e77c5-234">После этого на странице отображаются все значения `OperationId` службы и класса модели страниц в виде назначенных свойств.</span><span class="sxs-lookup"><span data-stu-id="e77c5-234">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="e77c5-235">В примере приложения показано время существования объектов в пределах одного и нескольких запросов.</span><span class="sxs-lookup"><span data-stu-id="e77c5-235">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="e77c5-236">Образец приложения содержит контроллер `OperationsController`, который запрашивает каждый вид типа `IOperation`, а также `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="e77c5-236">The sample app includes an `OperationsController` that requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="e77c5-237">Чтобы отобразить значения `OperationId` службы, действие `Index` помещает службы в `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="e77c5-237">The `Index` action sets the services into the `ViewBag` for display of the service's `OperationId` values:</span></span>

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Controllers/OperationsController.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e77c5-238">Результаты двух запросов будут выглядеть так:</span><span class="sxs-lookup"><span data-stu-id="e77c5-238">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="e77c5-239">**Первый запрос**</span><span class="sxs-lookup"><span data-stu-id="e77c5-239">**First request:**</span></span>

<span data-ttu-id="e77c5-240">Операции контроллера:</span><span class="sxs-lookup"><span data-stu-id="e77c5-240">Controller operations:</span></span>

<span data-ttu-id="e77c5-241">Transient: d233e165-f417-469b-a866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="e77c5-241">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="e77c5-242">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="e77c5-242">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="e77c5-243">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="e77c5-243">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="e77c5-244">Instance: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="e77c5-244">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="e77c5-245">Операции `OperationService`:</span><span class="sxs-lookup"><span data-stu-id="e77c5-245">`OperationService` operations:</span></span>

<span data-ttu-id="e77c5-246">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="e77c5-246">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="e77c5-247">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="e77c5-247">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="e77c5-248">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="e77c5-248">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="e77c5-249">Instance: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="e77c5-249">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="e77c5-250">**Второй запрос**</span><span class="sxs-lookup"><span data-stu-id="e77c5-250">**Second request:**</span></span>

<span data-ttu-id="e77c5-251">Операции контроллера:</span><span class="sxs-lookup"><span data-stu-id="e77c5-251">Controller operations:</span></span>

<span data-ttu-id="e77c5-252">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="e77c5-252">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="e77c5-253">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="e77c5-253">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="e77c5-254">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="e77c5-254">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="e77c5-255">Instance: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="e77c5-255">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="e77c5-256">Операции `OperationService`:</span><span class="sxs-lookup"><span data-stu-id="e77c5-256">`OperationService` operations:</span></span>

<span data-ttu-id="e77c5-257">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="e77c5-257">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="e77c5-258">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="e77c5-258">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="e77c5-259">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="e77c5-259">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="e77c5-260">Instance: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="e77c5-260">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="e77c5-261">Проанализируйте, какие значения `OperationId` изменяются в пределах одного и нескольких запросов.</span><span class="sxs-lookup"><span data-stu-id="e77c5-261">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="e77c5-262">*Временные* объекты всегда разные.</span><span class="sxs-lookup"><span data-stu-id="e77c5-262">*Transient* objects are always different.</span></span> <span data-ttu-id="e77c5-263">Обратите внимание, что временное значение `OperationId` отличается в первом и втором запросе, а также в обеих операциях `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="e77c5-263">Note that the transient `OperationId` value for both the first and second requests are different for both `OperationService` operations and across requests.</span></span> <span data-ttu-id="e77c5-264">Каждой службе и каждому запросу предоставляется новый экземпляр.</span><span class="sxs-lookup"><span data-stu-id="e77c5-264">A new instance is provided to each service and request.</span></span>
* <span data-ttu-id="e77c5-265">*Регулируемые* объекты остаются неизменными в пределах одного запроса, но в разных запросах используются разные объекты.</span><span class="sxs-lookup"><span data-stu-id="e77c5-265">*Scoped* objects are the same within a request but different across requests.</span></span>
* <span data-ttu-id="e77c5-266">*Одинарные* объекты остаются неизменными для всех объектов и запросов независимо от того, предоставляется ли в `ConfigureServices` экземпляр `Operation`.</span><span class="sxs-lookup"><span data-stu-id="e77c5-266">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="e77c5-267">Вызов служб из функции main</span><span class="sxs-lookup"><span data-stu-id="e77c5-267">Call services from main</span></span>

<span data-ttu-id="e77c5-268">Создайте [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) с [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) для разрешения службы с заданной областью в области приложения.</span><span class="sxs-lookup"><span data-stu-id="e77c5-268">Create an [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) with [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="e77c5-269">Этот способ позволит получить доступ к службе с заданной областью при запуске для выполнения задач по инициализации.</span><span class="sxs-lookup"><span data-stu-id="e77c5-269">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="e77c5-270">В следующем примере показано, как получить контекст для `MyScopedService` в `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="e77c5-270">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="e77c5-271">Проверка области</span><span class="sxs-lookup"><span data-stu-id="e77c5-271">Scope validation</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e77c5-272">Когда приложение выполняется в среде разработки, поставщик службы по умолчанию проверяет следующее:</span><span class="sxs-lookup"><span data-stu-id="e77c5-272">When the app is running in the Development environment, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="e77c5-273">Службы с заданной областью не разрешаются из корневого поставщика службы, прямо или косвенно.</span><span class="sxs-lookup"><span data-stu-id="e77c5-273">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="e77c5-274">Службы с заданной областью не вводятся в одноэлементные объекты, прямо или косвенно.</span><span class="sxs-lookup"><span data-stu-id="e77c5-274">Scoped services aren't directly or indirectly injected into singletons.</span></span>

::: moniker-end

<span data-ttu-id="e77c5-275">Корневой поставщик службы создается при вызове [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider).</span><span class="sxs-lookup"><span data-stu-id="e77c5-275">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="e77c5-276">Время существования корневого поставщика службы соответствует времени существования приложения или сервера — поставщик запускается с приложением и удаляется, когда приложение завершает работу.</span><span class="sxs-lookup"><span data-stu-id="e77c5-276">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="e77c5-277">Службы с заданной областью удаляются создавшим их контейнером.</span><span class="sxs-lookup"><span data-stu-id="e77c5-277">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="e77c5-278">Если служба с заданной областью создается в корневом контейнере, время существования службы повышается до уровня одноэлементного объекта, поскольку она удаляется только корневым контейнером при завершении работы приложения или сервера.</span><span class="sxs-lookup"><span data-stu-id="e77c5-278">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="e77c5-279">Проверка областей службы перехватывает эти ситуации при вызове `BuildServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="e77c5-279">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="e77c5-280">Дополнительные сведения см. в разделе <xref:fundamentals/host/web-host#scope-validation>.</span><span class="sxs-lookup"><span data-stu-id="e77c5-280">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="e77c5-281">Службы запросов</span><span class="sxs-lookup"><span data-stu-id="e77c5-281">Request Services</span></span>

<span data-ttu-id="e77c5-282">Службы, доступные в пределах запроса ASP.NET Core из `HttpContext`, предоставляются через коллекцию [HttpContext.RequestServices](/dotnet/api/microsoft.aspnetcore.http.httpcontext.requestservices).</span><span class="sxs-lookup"><span data-stu-id="e77c5-282">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](/dotnet/api/microsoft.aspnetcore.http.httpcontext.requestservices) collection.</span></span>

<span data-ttu-id="e77c5-283">Службы запросов — это все настроенные и запрашиваемые в приложении службы.</span><span class="sxs-lookup"><span data-stu-id="e77c5-283">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="e77c5-284">Когда объекты указывают зависимости, они удовлетворяются за счет типов из `RequestServices`, а не из `ApplicationServices`.</span><span class="sxs-lookup"><span data-stu-id="e77c5-284">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="e77c5-285">Как правило, использовать эти свойства в приложении напрямую не следует.</span><span class="sxs-lookup"><span data-stu-id="e77c5-285">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="e77c5-286">Вместо этого запрашивайте требуемые для классов типы через конструкторы классов и разрешите платформе внедрять зависимости.</span><span class="sxs-lookup"><span data-stu-id="e77c5-286">Instead, request the types that classes require via class constructors and allow the framework inject the dependencies.</span></span> <span data-ttu-id="e77c5-287">Таким образом вы получите классы, которые легче тестировать (см. статью [Тестирование, отладка и устранение неполадок в ASP.NET Core](xref:test/index)).</span><span class="sxs-lookup"><span data-stu-id="e77c5-287">This yields classes that are easier to test (see the [Test and debug](xref:test/index) topics).</span></span>

> [!NOTE]
> <span data-ttu-id="e77c5-288">Предпочтительнее запрашивать зависимости в качестве параметров конструктора, а не обращаться к коллекции `RequestServices`.</span><span class="sxs-lookup"><span data-stu-id="e77c5-288">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="e77c5-289">Проектирование служб для внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="e77c5-289">Design services for dependency injection</span></span>

<span data-ttu-id="e77c5-290">Придерживайтесь следующих рекомендаций:</span><span class="sxs-lookup"><span data-stu-id="e77c5-290">Best practices are to:</span></span>

* <span data-ttu-id="e77c5-291">Проектируйте службы так, чтобы для получения зависимостей они использовали внедрение зависимостей.</span><span class="sxs-lookup"><span data-stu-id="e77c5-291">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="e77c5-292">Избегайте вызовов статических методов или методов с отслеживанием состояния ([статическое сцепление](https://deviq.com/static-cling/)).</span><span class="sxs-lookup"><span data-stu-id="e77c5-292">Avoid stateful, static method calls (a practice known as [static cling](https://deviq.com/static-cling/)).</span></span>
* <span data-ttu-id="e77c5-293">Избегайте прямого создания экземпляров зависимых классов внутри служб.</span><span class="sxs-lookup"><span data-stu-id="e77c5-293">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="e77c5-294">Прямое создание экземпляров обязывает использовать в коде определенную реализацию.</span><span class="sxs-lookup"><span data-stu-id="e77c5-294">Direct instantiation couples the code to a particular implementation.</span></span>

<span data-ttu-id="e77c5-295">Соблюдение [принципов SOLID для объектно-ориентированного проектирования](https://deviq.com/solid/) поможет сделать программные классы небольшими, хорошо организованными и удобными в тестировании.</span><span class="sxs-lookup"><span data-stu-id="e77c5-295">By following the [SOLID Principles of Object Oriented Design](https://deviq.com/solid/), app classes naturally tend to be small, well-factored, and easily tested.</span></span>

<span data-ttu-id="e77c5-296">Если класс имеет слишком много внедренных зависимостей, обычно это указывает на то, что у класса слишком много задач и он не соответствует [принципу единственной обязанности](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span><span class="sxs-lookup"><span data-stu-id="e77c5-296">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span></span> <span data-ttu-id="e77c5-297">Попробуйте выполнить рефакторинг класса и перенести часть его обязанностей в новый класс.</span><span class="sxs-lookup"><span data-stu-id="e77c5-297">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="e77c5-298">Помните, что в классах модели страниц Razor Pages и классах контроллера MVC должны преимущественно выполняться задачи, связанные с пользовательским интерфейсом.</span><span class="sxs-lookup"><span data-stu-id="e77c5-298">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="e77c5-299">Бизнес-правила и реализация доступа к данным должны обрабатываться в классах, которые предназначены для [этих целей](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span><span class="sxs-lookup"><span data-stu-id="e77c5-299">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="e77c5-300">Удаление служб</span><span class="sxs-lookup"><span data-stu-id="e77c5-300">Disposal of services</span></span>

<span data-ttu-id="e77c5-301">Контейнер вызывает `Dispose` для создаваемых им типов `IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="e77c5-301">The container calls `Dispose` for the `IDisposable` types it creates.</span></span> <span data-ttu-id="e77c5-302">Если экземпляр добавлен в контейнер с помощью пользовательского кода, он не будет удален автоматически.</span><span class="sxs-lookup"><span data-stu-id="e77c5-302">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

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
> <span data-ttu-id="e77c5-303">В ASP.NET Core 1.0 контейнер вызывает удаление для *всех* объектов `IDisposable`, включая те, которые он не создавал.</span><span class="sxs-lookup"><span data-stu-id="e77c5-303">In ASP.NET Core 1.0, the container calls dispose on *all* `IDisposable` objects, including those it didn't create.</span></span>

::: moniker-end

## <a name="default-service-container-replacement"></a><span data-ttu-id="e77c5-304">Замена стандартного контейнера служб</span><span class="sxs-lookup"><span data-stu-id="e77c5-304">Default service container replacement</span></span>

<span data-ttu-id="e77c5-305">Встроенный контейнер служб предназначен для удовлетворения базовых потребностей платформы и большинства клиентских приложений.</span><span class="sxs-lookup"><span data-stu-id="e77c5-305">The built-in service container is meant to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="e77c5-306">Мы рекомендуем использовать встроенный контейнер, если только не требуется конкретная функциональная возможность, которую он не поддерживает.</span><span class="sxs-lookup"><span data-stu-id="e77c5-306">We recommend using the built-in container unless you need a specific feature that it doesn't support.</span></span> <span data-ttu-id="e77c5-307">Некоторые функции, поддерживаемые сторонними контейнерами, отсутствуют во встроенном контейнере:</span><span class="sxs-lookup"><span data-stu-id="e77c5-307">Some of the features supported in 3rd party containers not found in the built-in container:</span></span>

* <span data-ttu-id="e77c5-308">Внедрение свойств</span><span class="sxs-lookup"><span data-stu-id="e77c5-308">Property injection</span></span>
* <span data-ttu-id="e77c5-309">Внедрение по имени</span><span class="sxs-lookup"><span data-stu-id="e77c5-309">Injection based on name</span></span>
* <span data-ttu-id="e77c5-310">Дочерние контейнеры</span><span class="sxs-lookup"><span data-stu-id="e77c5-310">Child containers</span></span>
* <span data-ttu-id="e77c5-311">Настраиваемое управление временем существования</span><span class="sxs-lookup"><span data-stu-id="e77c5-311">Custom lifetime management</span></span>
* <span data-ttu-id="e77c5-312">`Func<T>` поддерживает отложенную инициализацию</span><span class="sxs-lookup"><span data-stu-id="e77c5-312">`Func<T>` support for lazy initialization</span></span>

<span data-ttu-id="e77c5-313">Список некоторых контейнеров, которые поддерживают адаптеры, см. в разделе [Файл readme.md внедрения зависимостей](https://github.com/aspnet/DependencyInjection#using-other-containers-with-microsoftextensionsdependencyinjection).</span><span class="sxs-lookup"><span data-stu-id="e77c5-313">See the [Dependency Injection readme.md file](https://github.com/aspnet/DependencyInjection#using-other-containers-with-microsoftextensionsdependencyinjection) for a list of some of the containers that support adapters.</span></span>

<span data-ttu-id="e77c5-314">В следующем примере встроенный контейнер заменяется на [Autofac](https://autofac.org/):</span><span class="sxs-lookup"><span data-stu-id="e77c5-314">The following sample replaces the built-in container with [Autofac](https://autofac.org/):</span></span>

* <span data-ttu-id="e77c5-315">Установите соответствующие пакеты контейнера:</span><span class="sxs-lookup"><span data-stu-id="e77c5-315">Install the appropriate container package(s):</span></span>

    * <span data-ttu-id="e77c5-316">[Autofac](https://www.nuget.org/packages/Autofac/);</span><span class="sxs-lookup"><span data-stu-id="e77c5-316">[Autofac](https://www.nuget.org/packages/Autofac/)</span></span>
    * <span data-ttu-id="e77c5-317">[Autofac.Extensions.DependencyInjection](https://www.nuget.org/packages/Autofac.Extensions.DependencyInjection/).</span><span class="sxs-lookup"><span data-stu-id="e77c5-317">[Autofac.Extensions.DependencyInjection](https://www.nuget.org/packages/Autofac.Extensions.DependencyInjection/)</span></span>

* <span data-ttu-id="e77c5-318">Настройте контейнер в `Startup.ConfigureServices` и возвратите `IServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="e77c5-318">Configure the container in `Startup.ConfigureServices` and return an `IServiceProvider`:</span></span>

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

    <span data-ttu-id="e77c5-319">Чтобы использовать сторонний контейнер, метод `Startup.ConfigureServices` должен возвращать `IServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="e77c5-319">To use a 3rd party container, `Startup.ConfigureServices` must return `IServiceProvider`.</span></span>

* <span data-ttu-id="e77c5-320">Настройте Autofac в `DefaultModule`.</span><span class="sxs-lookup"><span data-stu-id="e77c5-320">Configure Autofac in `DefaultModule`:</span></span>

    ```csharp
    public class DefaultModule : Module
    {
        protected override void Load(ContainerBuilder builder)
        {
            builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
        }
    }
    ```

<span data-ttu-id="e77c5-321">В среде выполнения Autofac используется для разрешения типов и внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="e77c5-321">At runtime, Autofac is used to resolve types and inject dependencies.</span></span> <span data-ttu-id="e77c5-322">Дополнительные сведения об использовании Autofac с ASP.NET Core см. в [документации по Autofac](https://docs.autofac.org/en/latest/integration/aspnetcore.html).</span><span class="sxs-lookup"><span data-stu-id="e77c5-322">To learn more about using Autofac with ASP.NET Core, see the [Autofac documentation](https://docs.autofac.org/en/latest/integration/aspnetcore.html).</span></span>

### <a name="thread-safety"></a><span data-ttu-id="e77c5-323">Потокобезопасность</span><span class="sxs-lookup"><span data-stu-id="e77c5-323">Thread safety</span></span>

<span data-ttu-id="e77c5-324">Одноэлементные службы должны быть потокобезопасными.</span><span class="sxs-lookup"><span data-stu-id="e77c5-324">Singleton services need to be thread safe.</span></span> <span data-ttu-id="e77c5-325">Если одноэлементная служба имеет зависимость от временной службы, с учетом характера использования одноэлементной службой к этой временной службе также может предъявляться требование потокобезопасности.</span><span class="sxs-lookup"><span data-stu-id="e77c5-325">If a singleton service has a dependency on a transient service, the transient service may also need to be thread safe depending how it's used by the singleton.</span></span>

<span data-ttu-id="e77c5-326">Фабричный метод одной службы, например второй аргумент для [AddSingleton&lt;TService&gt;(IServiceCollection, Func&lt;IServiceProvider,TService&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addsingleton#Microsoft_Extensions_DependencyInjection_ServiceCollectionServiceExtensions_AddSingleton__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Func_System_IServiceProvider___0__), не обязательно должен быть потокобезопасным.</span><span class="sxs-lookup"><span data-stu-id="e77c5-326">The factory method of single service, such as the second argument to [AddSingleton&lt;TService&gt;(IServiceCollection, Func&lt;IServiceProvider,TService&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addsingleton#Microsoft_Extensions_DependencyInjection_ServiceCollectionServiceExtensions_AddSingleton__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Func_System_IServiceProvider___0__), doesn't need to be thread-safe.</span></span> <span data-ttu-id="e77c5-327">Как и конструктор типов (`static`), он гарантированно будет вызываться один раз одним потоком.</span><span class="sxs-lookup"><span data-stu-id="e77c5-327">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="e77c5-328">Рекомендации</span><span class="sxs-lookup"><span data-stu-id="e77c5-328">Recommendations</span></span>

<span data-ttu-id="e77c5-329">При работе с внедрением зависимостей соблюдайте следующие рекомендации:</span><span class="sxs-lookup"><span data-stu-id="e77c5-329">When working with dependency injection, keep the following recommendations in mind:</span></span>

* <span data-ttu-id="e77c5-330">Не храните данные и конфигурацию непосредственно в контейнере служб.</span><span class="sxs-lookup"><span data-stu-id="e77c5-330">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="e77c5-331">Например, обычно не следует добавлять корзину пользователя в контейнер служб.</span><span class="sxs-lookup"><span data-stu-id="e77c5-331">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="e77c5-332">Конфигурация должна использовать [шаблон параметров](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="e77c5-332">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="e77c5-333">Аналогичным образом, избегайте объектов "хранения данных", которые служат лишь для доступа к некоторому другому объекту.</span><span class="sxs-lookup"><span data-stu-id="e77c5-333">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="e77c5-334">По возможности лучше запросить фактический элемент через внедрение зависимостей.</span><span class="sxs-lookup"><span data-stu-id="e77c5-334">It's better to request the actual item via dependency injection, if possible.</span></span>

* <span data-ttu-id="e77c5-335">Не используйте статический доступ к службам (например, не используйте везде [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices)).</span><span class="sxs-lookup"><span data-stu-id="e77c5-335">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices) for use elsewhere).</span></span>

* <span data-ttu-id="e77c5-336">Не используйте шаблон обнаружения службы (например, [IServiceProvider.GetService](/dotnet/api/system.iserviceprovider.getservice)).</span><span class="sxs-lookup"><span data-stu-id="e77c5-336">Avoid using the service locator pattern (for example, [IServiceProvider.GetService](/dotnet/api/system.iserviceprovider.getservice)).</span></span>

* <span data-ttu-id="e77c5-337">Не используйте статический доступ к `HttpContext` (например, [IHttpContextAccessor.HttpContext](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor.httpcontext)).</span><span class="sxs-lookup"><span data-stu-id="e77c5-337">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor.httpcontext)).</span></span>

<span data-ttu-id="e77c5-338">Как и с любыми рекомендациями, у вас могут возникнуть ситуации, когда нужно отступить от одного из правил.</span><span class="sxs-lookup"><span data-stu-id="e77c5-338">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="e77c5-339">Такие исключения случаются редко. Главным образом они связаны с особенностями самой платформы.</span><span class="sxs-lookup"><span data-stu-id="e77c5-339">Exceptions are rare&mdash;mostly special cases within the framework itself.</span></span>

<span data-ttu-id="e77c5-340">Внедрение зависимостей является *альтернативой* для шаблонов доступа к статическим или глобальным объектам.</span><span class="sxs-lookup"><span data-stu-id="e77c5-340">Dependency injection is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="e77c5-341">Вы не сможете воспользоваться преимуществами внедрения зависимостей, если будете сочетать его с доступом к статическим объектам.</span><span class="sxs-lookup"><span data-stu-id="e77c5-341">You may not be able to realize the benefits of dependency injection if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e77c5-342">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="e77c5-342">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:fundamentals/repository-pattern>
* <xref:fundamentals/startup>
* <xref:test/index>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="e77c5-343">Написание чистого кода в ASP.NET Core с внедрением зависимостей (MSDN)</span><span class="sxs-lookup"><span data-stu-id="e77c5-343">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="e77c5-344">Проектирование приложения на основе контейнеров. Вступление. К чему относится контейнер?</span><span class="sxs-lookup"><span data-stu-id="e77c5-344">Container-Managed Application Design, Prelude: Where does the Container Belong?</span></span>](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)
* [<span data-ttu-id="e77c5-345">Принцип явных зависимостей</span><span class="sxs-lookup"><span data-stu-id="e77c5-345">Explicit Dependencies Principle</span></span>](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* [<span data-ttu-id="e77c5-346">Контейнеры с инверсией управления и шаблон внедрения зависимостей (Мартин Фаулер)</span><span class="sxs-lookup"><span data-stu-id="e77c5-346">Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)</span></span>](https://www.martinfowler.com/articles/injection.html)
* [<span data-ttu-id="e77c5-347">Ключевое слово New — связующий элемент (связывание кода с определенной реализацией)</span><span class="sxs-lookup"><span data-stu-id="e77c5-347">New is Glue ("gluing" code to a particular implementation)</span></span>](https://ardalis.com/new-is-glue)
* <span data-ttu-id="e77c5-348">[How to register a service with multiple interfaces in ASP.NET Core DI](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/) (Регистрация службы с несколькими интерфейсами с помощью внедрения зависимостей ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="e77c5-348">[How to register a service with multiple interfaces in ASP.NET Core DI](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)</span></span>
