---
title: Инициирование HTTP-запросов
author: stevejgordon
description: Сведения об использовании интерфейса IHttpClientFactory для управления логическими экземплярами HttpClient в ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 07/23/2018
uid: fundamentals/http-requests
ms.openlocfilehash: 87424eaea499ba7ece1e5ef88649fcbb2e297635
ms.sourcegitcommit: 516d0645c35ea784a3ae807be087ae70446a46ee
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/27/2018
ms.locfileid: "39320659"
---
# <a name="initiate-http-requests"></a><span data-ttu-id="3a247-103">Инициирование HTTP-запросов</span><span class="sxs-lookup"><span data-stu-id="3a247-103">Initiate HTTP requests</span></span>

<span data-ttu-id="3a247-104">Авторы: [Гленн Кондрон (Glenn Condron)](https://github.com/glennc), [Райан Новак (Ryan Nowak)](https://github.com/rynowak) и [Стив Гордон (Steve Gordon)](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="3a247-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="3a247-105">[IHttpClientFactory](/dotnet/api/system.net.http.ihttpclientfactory) можно зарегистрировать и использовать для настройки и создания экземпляров [HttpClient](/dotnet/api/system.net.http.httpclient) в приложении.</span><span class="sxs-lookup"><span data-stu-id="3a247-105">An [IHttpClientFactory](/dotnet/api/system.net.http.ihttpclientfactory) can be registered and used to configure and create [HttpClient](/dotnet/api/system.net.http.httpclient) instances in an app.</span></span> <span data-ttu-id="3a247-106">Так вы получите следующие преимущества:</span><span class="sxs-lookup"><span data-stu-id="3a247-106">It offers the following benefits:</span></span>

* <span data-ttu-id="3a247-107">Центральное расположение для именования и настройки логических экземпляров `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="3a247-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="3a247-108">Например, можно зарегистрировать и использовать клиент github для доступа к GitHub.</span><span class="sxs-lookup"><span data-stu-id="3a247-108">For example, a "github" client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="3a247-109">Можно зарегистрировать клиент по умолчанию для других целей.</span><span class="sxs-lookup"><span data-stu-id="3a247-109">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="3a247-110">Кодификация концепции исходящего ПО промежуточного слоя путем делегирования обработчиков в `HttpClient` и предоставление расширений для ПО промежуточного слоя на основе Polly для использования этой возможности.</span><span class="sxs-lookup"><span data-stu-id="3a247-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="3a247-111">Управление созданием пулов и временем существования базовых экземпляров `HttpClientMessageHandler` с целью избежать обычных проблем с DNS, которые возникают при управлении временем существования `HttpClient` вручную.</span><span class="sxs-lookup"><span data-stu-id="3a247-111">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="3a247-112">Настройка параметров ведения журнала (через `ILogger`) для всех запросов, отправленных через клиентов, созданных фабрикой.</span><span class="sxs-lookup"><span data-stu-id="3a247-112">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="3a247-113">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3a247-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3a247-114">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="3a247-114">Prerequisites</span></span>

<span data-ttu-id="3a247-115">Для проектов, предназначенных для .NET Framework, необходимо установить пакет NuGet [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/).</span><span class="sxs-lookup"><span data-stu-id="3a247-115">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="3a247-116">Пакет `Microsoft.Extensions.Http` уже включен в проекты, предназначенные для .NET Core и ссылающиеся на [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="3a247-116">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="3a247-117">Принципы использования</span><span class="sxs-lookup"><span data-stu-id="3a247-117">Consumption patterns</span></span>

<span data-ttu-id="3a247-118">Существует несколько способов использования `IHttpClientFactory` в приложении:</span><span class="sxs-lookup"><span data-stu-id="3a247-118">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="3a247-119">Основное использование</span><span class="sxs-lookup"><span data-stu-id="3a247-119">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="3a247-120">Именованные клиенты</span><span class="sxs-lookup"><span data-stu-id="3a247-120">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="3a247-121">Типизированные клиенты</span><span class="sxs-lookup"><span data-stu-id="3a247-121">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="3a247-122">Созданные клиенты</span><span class="sxs-lookup"><span data-stu-id="3a247-122">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="3a247-123">Все способы равноценны.</span><span class="sxs-lookup"><span data-stu-id="3a247-123">None of them are strictly superior to another.</span></span> <span data-ttu-id="3a247-124">Оптимальный подход зависит от ограничений приложения.</span><span class="sxs-lookup"><span data-stu-id="3a247-124">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="3a247-125">Основное использование</span><span class="sxs-lookup"><span data-stu-id="3a247-125">Basic usage</span></span>

<span data-ttu-id="3a247-126">`IHttpClientFactory` можно зарегистрировать путем вызова метода расширения `AddHttpClient` в `IServiceCollection` внутри метода `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3a247-126">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet1)]

<span data-ttu-id="3a247-127">После регистрации код может принимать `IHttpClientFactory` в любом месте, куда можно внедрить службу с помощью [внедрения зависимостей](xref:fundamentals/dependency-injection) (DI).</span><span class="sxs-lookup"><span data-stu-id="3a247-127">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="3a247-128">`IHttpClientFactory` можно использовать для создания экземпляра `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="3a247-128">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,20)]

<span data-ttu-id="3a247-129">Подобное использование `IHttpClientFactory` — это отличный способ рефакторинга существующего приложения.</span><span class="sxs-lookup"><span data-stu-id="3a247-129">Using `IHttpClientFactory` in this fashion is a great way to refactor an existing app.</span></span> <span data-ttu-id="3a247-130">Он не оказывает влияния на использование `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="3a247-130">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="3a247-131">Там, где в данный момент создаются экземпляры `HttpClient`, используйте вызов к `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="3a247-131">In places where `HttpClient` instances are currently created, replace those occurrences with a call to `CreateClient`.</span></span>

### <a name="named-clients"></a><span data-ttu-id="3a247-132">Именованные клиенты</span><span class="sxs-lookup"><span data-stu-id="3a247-132">Named clients</span></span>

<span data-ttu-id="3a247-133">Если в приложении требуется несколько различных способов использования `HttpClient`, каждый со своей конфигурацией, можно использовать **именованных клиентов**.</span><span class="sxs-lookup"><span data-stu-id="3a247-133">If an app requires multiple distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="3a247-134">Конфигурацию для именованного клиента `HttpClient` можно указать во время регистрации в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3a247-134">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet2)]

<span data-ttu-id="3a247-135">В приведенном выше коде вызывается клиент `AddHttpClient`, предоставляющий имя "github".</span><span class="sxs-lookup"><span data-stu-id="3a247-135">In the preceding code, `AddHttpClient` is called, providing the name "github".</span></span> <span data-ttu-id="3a247-136">У клиента есть некоторые настройки по умолчанию &mdash; а именно: базовый адрес и два заголовка, необходимые для работы с API GitHub.</span><span class="sxs-lookup"><span data-stu-id="3a247-136">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="3a247-137">При каждом вызове `CreateClient` создается новый экземпляр `HttpClient` и вызывается действие конфигурации.</span><span class="sxs-lookup"><span data-stu-id="3a247-137">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="3a247-138">Для использования именованного клиента можно передать строковый параметр в `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="3a247-138">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="3a247-139">Укажите имя создаваемого клиента:</span><span class="sxs-lookup"><span data-stu-id="3a247-139">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=20)]

<span data-ttu-id="3a247-140">В приведенном выше коде в запросе не требуется указывать имя узла.</span><span class="sxs-lookup"><span data-stu-id="3a247-140">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="3a247-141">Достаточно передать только путь, так как используется базовый адрес, заданный для клиента.</span><span class="sxs-lookup"><span data-stu-id="3a247-141">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="3a247-142">Типизированные клиенты</span><span class="sxs-lookup"><span data-stu-id="3a247-142">Typed clients</span></span>

<span data-ttu-id="3a247-143">Типизированные клиенты предоставляют те же возможности, что и именованные клиенты, без необходимости использовать строки в качестве ключей.</span><span class="sxs-lookup"><span data-stu-id="3a247-143">Typed clients provide the same capabilities as named clients without the need to use strings as keys.</span></span> <span data-ttu-id="3a247-144">Метод типизированных клиентов помогает IntelliSense и компилятору при использовании клиентов.</span><span class="sxs-lookup"><span data-stu-id="3a247-144">The typed client approach provides IntelliSense and compiler help when consuming clients.</span></span> <span data-ttu-id="3a247-145">Они предоставляют единое расположение для настройки и взаимодействия с конкретным клиентом `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="3a247-145">They provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="3a247-146">Например, для конечной точки серверной части можно использовать один типизированный клиент, который будет содержать всю логику работы с этой конечной точкой.</span><span class="sxs-lookup"><span data-stu-id="3a247-146">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span> <span data-ttu-id="3a247-147">Еще одно преимущество — работа с внедрением зависимостей и возможность вставки в нужное место в приложении.</span><span class="sxs-lookup"><span data-stu-id="3a247-147">Another advantage is that they work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="3a247-148">Типизированный клиент принимает параметр `HttpClient` в конструкторе:</span><span class="sxs-lookup"><span data-stu-id="3a247-148">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="3a247-149">В приведенном выше коде конфигурация перемещается в типизированный клиент.</span><span class="sxs-lookup"><span data-stu-id="3a247-149">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="3a247-150">Объект `HttpClient` предоставляется в виде открытого свойства.</span><span class="sxs-lookup"><span data-stu-id="3a247-150">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="3a247-151">Можно определить связанные с API методы, которые предоставляют функциональные возможности `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="3a247-151">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="3a247-152">Метод `GetAspNetDocsIssues` инкапсулирует код, необходимый для запроса и анализа последнего открытого выпуска из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="3a247-152">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="3a247-153">Для регистрации типизированного клиента можно использовать универсальный метод расширения `AddHttpClient` в `Startup.ConfigureServices`, указав класс типизированного клиента:</span><span class="sxs-lookup"><span data-stu-id="3a247-153">To register a typed client, the generic `AddHttpClient` extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet3)]

<span data-ttu-id="3a247-154">Типизированный клиент регистрируется во внедрении зависимостей как временный.</span><span class="sxs-lookup"><span data-stu-id="3a247-154">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="3a247-155">Типизированный клиент можно внедрить и использовать напрямую:</span><span class="sxs-lookup"><span data-stu-id="3a247-155">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="3a247-156">При желании конфигурацию для типизированного клиента можно указать во время регистрации в `Startup.ConfigureServices`, а не в конструкторе типизированного клиента:</span><span class="sxs-lookup"><span data-stu-id="3a247-156">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet4)]

<span data-ttu-id="3a247-157">Можно полностью инкапсулировать `HttpClient` внутри типизированного клиента.</span><span class="sxs-lookup"><span data-stu-id="3a247-157">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="3a247-158">Вместо предоставления его как свойства можно использовать открытые методы для внутреннего вызова экземпляра `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="3a247-158">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/GitHub/RepoService.cs?name=snippet1&highlight=3)]

<span data-ttu-id="3a247-159">В приведенном выше коде `HttpClient` хранится как закрытое поле.</span><span class="sxs-lookup"><span data-stu-id="3a247-159">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="3a247-160">Любой доступ для совершения внешних вызовов осуществляется через метод `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="3a247-160">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="3a247-161">Созданные клиенты</span><span class="sxs-lookup"><span data-stu-id="3a247-161">Generated clients</span></span>

<span data-ttu-id="3a247-162">`IHttpClientFactory` можно использовать в сочетании с другими библиотеками сторонних разработчиков, например [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="3a247-162">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="3a247-163">Refit — это библиотека REST для .NET.</span><span class="sxs-lookup"><span data-stu-id="3a247-163">Refit is a REST library for .NET.</span></span> <span data-ttu-id="3a247-164">Она преобразует REST API в динамические интерфейсы.</span><span class="sxs-lookup"><span data-stu-id="3a247-164">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="3a247-165">Реализация интерфейса формируется динамически с помощью `RestService` с использованием `HttpClient` для совершения внешних вызовов HTTP.</span><span class="sxs-lookup"><span data-stu-id="3a247-165">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="3a247-166">Для представления внешнего API и его ответа определяются интерфейс и ответ:</span><span class="sxs-lookup"><span data-stu-id="3a247-166">An interface and a reply are defined to represent the external API and its response:</span></span>

```csharp
public interface IHelloClient
{
    [Get("/helloworld")]
    Task<Reply> GetMessageAsync();
}

public class Reply
{
    public string Message { get; set; }
}
```

<span data-ttu-id="3a247-167">Можно добавить типизированный клиент, используя Refit для создания реализации:</span><span class="sxs-lookup"><span data-stu-id="3a247-167">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("http://localhost:5000");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddMvc();
}
```

<span data-ttu-id="3a247-168">При необходимости можно использовать определенный интерфейс с реализацией, предоставленной внедрением зависимостей и Refit:</span><span class="sxs-lookup"><span data-stu-id="3a247-168">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

```csharp
[ApiController]
public class ValuesController : ControllerBase
{
    private readonly IHelloClient _client;

    public ValuesController(IHelloClient client)
    {
        _client = client;
    }

    [HttpGet("/")]
    public async Task<ActionResult<Reply>> Index()
    {
        return await _client.GetMessageAsync();
    }
}
```

## <a name="outgoing-request-middleware"></a><span data-ttu-id="3a247-169">ПО промежуточного слоя для исходящих запросов</span><span class="sxs-lookup"><span data-stu-id="3a247-169">Outgoing request middleware</span></span>

<span data-ttu-id="3a247-170">В `HttpClient` уже существует концепция делегирования обработчиков, которые можно связать друг с другом для исходящих HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="3a247-170">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="3a247-171">Класс `IHttpClientFactory` упрощает определение обработчиков для применения к каждому именованному клиенту.</span><span class="sxs-lookup"><span data-stu-id="3a247-171">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="3a247-172">Он поддерживает регистрацию и объединение в цепочки нескольких обработчиков для создания конвейера ПО промежуточного слоя для исходящих запросов.</span><span class="sxs-lookup"><span data-stu-id="3a247-172">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="3a247-173">Каждый из этих обработчиков может выполнять работу до и после исходящего запроса.</span><span class="sxs-lookup"><span data-stu-id="3a247-173">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="3a247-174">Этот шаблон похож на входящий конвейер ПО промежуточного слоя в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3a247-174">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="3a247-175">Шаблон предоставляет механизм управления сквозной функциональностью HTTP-запросов, включая кэширование, обработку ошибок, сериализацию и ведение журнала.</span><span class="sxs-lookup"><span data-stu-id="3a247-175">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="3a247-176">Чтобы создать обработчик, необходимо определить класс, производный от `DelegatingHandler`.</span><span class="sxs-lookup"><span data-stu-id="3a247-176">To create a handler, define a class deriving from `DelegatingHandler`.</span></span> <span data-ttu-id="3a247-177">Переопределите метод `SendAsync` для выполнения кода до передачи запросов следующему обработчику в конвейере:</span><span class="sxs-lookup"><span data-stu-id="3a247-177">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[Main](http-requests/samples/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="3a247-178">В предыдущем коде определяется базовый обработчик.</span><span class="sxs-lookup"><span data-stu-id="3a247-178">The preceding code defines a basic handler.</span></span> <span data-ttu-id="3a247-179">Он проверяет включение в запрос заголовка X-API-KEY.</span><span class="sxs-lookup"><span data-stu-id="3a247-179">It checks to see if an X-API-KEY header has been included on the request.</span></span> <span data-ttu-id="3a247-180">Если заголовок отсутствует, он может избежать вызовов HTTP и вернуть подходящий ответ.</span><span class="sxs-lookup"><span data-stu-id="3a247-180">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="3a247-181">Во время регистрации можно добавить один или несколько обработчиков в конфигурацию для `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="3a247-181">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="3a247-182">Эта задача выполняется через методы расширения в `IHttpClientBuilder`.</span><span class="sxs-lookup"><span data-stu-id="3a247-182">This task is accomplished via extension methods on the `IHttpClientBuilder`.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet5)]

<span data-ttu-id="3a247-183">В приведенном выше коде `ValidateHeaderHandler` регистрируется с помощью внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="3a247-183">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="3a247-184">Обработчик **должен** регистрироваться во внедрении зависимостей как временный.</span><span class="sxs-lookup"><span data-stu-id="3a247-184">The handler **must** be registered in DI as transient.</span></span> <span data-ttu-id="3a247-185">После регистрации можно вызвать `AddHttpMessageHandler`, передав тип обработчика.</span><span class="sxs-lookup"><span data-stu-id="3a247-185">Once registered, `AddHttpMessageHandler` can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="3a247-186">Можно зарегистрировать несколько обработчиков в порядке, в котором они должны выполняться.</span><span class="sxs-lookup"><span data-stu-id="3a247-186">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="3a247-187">Каждый обработчик содержит следующий обработчик, пока последний `HttpClientHandler` не выполнит запрос:</span><span class="sxs-lookup"><span data-stu-id="3a247-187">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet6)]

## <a name="use-polly-based-handlers"></a><span data-ttu-id="3a247-188">Использование обработчиков на основе Polly</span><span class="sxs-lookup"><span data-stu-id="3a247-188">Use Polly-based handlers</span></span>

<span data-ttu-id="3a247-189">`IHttpClientFactory` интегрируется с популярной библиотекой сторонних разработчиков под названием [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="3a247-189">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="3a247-190">Polly — это комплексная библиотека, обеспечивающая отказоустойчивость и обработку временных сбоев в .NET.</span><span class="sxs-lookup"><span data-stu-id="3a247-190">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="3a247-191">Она позволяет разработчикам выражать политики, например политику повтора, размыкателя цепи, времени ожидания, изоляции отсеков и отката, более эффективным и потокобезопасным образом.</span><span class="sxs-lookup"><span data-stu-id="3a247-191">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="3a247-192">Для использования политик Polly с настроенными экземплярами `HttpClient` предоставляются методы расширения.</span><span class="sxs-lookup"><span data-stu-id="3a247-192">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="3a247-193">Расширения Polly доступны в пакете NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/).</span><span class="sxs-lookup"><span data-stu-id="3a247-193">The Polly extensions are available in the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="3a247-194">Этот пакет не входит в состав [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="3a247-194">This package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="3a247-195">Для использования расширений в проект необходимо включить явную ссылку `<PackageReference />`.</span><span class="sxs-lookup"><span data-stu-id="3a247-195">To use the extensions, an explicit `<PackageReference />` should be included in the project.</span></span>

[!code-csharp[](http-requests/samples/HttpClientFactorySample.csproj?highlight=9)]

<span data-ttu-id="3a247-196">После восстановления этого пакета будут доступны методы расширения для поддержки добавления обработчиков на основе Polly в клиенты.</span><span class="sxs-lookup"><span data-stu-id="3a247-196">After restoring this package, extension methods are available to support adding Polly-based handlers to clients.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="3a247-197">Обработка временных сбоев</span><span class="sxs-lookup"><span data-stu-id="3a247-197">Handle transient faults</span></span>

<span data-ttu-id="3a247-198">Наиболее распространенные сбои, возникающие при совершении внешних вызовов HTTP, будут носить временный характер.</span><span class="sxs-lookup"><span data-stu-id="3a247-198">The most common faults you may expect to occur when making external HTTP calls will be transient.</span></span> <span data-ttu-id="3a247-199">Используется удобный метод расширения `AddTransientHttpErrorPolicy`, который позволяет определить политику для обработки временных ошибок.</span><span class="sxs-lookup"><span data-stu-id="3a247-199">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="3a247-200">Политики, заданные с помощью этого метода расширения, обрабатывают `HttpRequestException`, ответы HTTP 5xx и ответы HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="3a247-200">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="3a247-201">Расширение `AddTransientHttpErrorPolicy` может быть использовано в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3a247-201">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="3a247-202">Данное расширение предоставляет доступ к объекту `PolicyBuilder`, настроенному для обработки ошибок, представляющих возможный временный сбой:</span><span class="sxs-lookup"><span data-stu-id="3a247-202">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet7)]

<span data-ttu-id="3a247-203">В приведенном выше коде определена политика `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="3a247-203">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="3a247-204">Неудачные запросы повторяются до трех раз с задержкой 600 мс между попытками.</span><span class="sxs-lookup"><span data-stu-id="3a247-204">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="3a247-205">Динамический выбор политик</span><span class="sxs-lookup"><span data-stu-id="3a247-205">Dynamically select policies</span></span>

<span data-ttu-id="3a247-206">Существуют дополнительные методы расширения, которые можно использовать для добавления обработчиков на основе Polly.</span><span class="sxs-lookup"><span data-stu-id="3a247-206">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="3a247-207">Одним из таких расширений является `AddPolicyHandler` с несколькими перегрузками.</span><span class="sxs-lookup"><span data-stu-id="3a247-207">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="3a247-208">Одна перегрузка разрешает проверку запроса для определения необходимой политики:</span><span class="sxs-lookup"><span data-stu-id="3a247-208">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet8)]

<span data-ttu-id="3a247-209">В приведенном выше коде для запроса GET время ожидания составляет 10 секунд.</span><span class="sxs-lookup"><span data-stu-id="3a247-209">In the preceding code, if the outgoing request is a GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="3a247-210">Для остальных методов HTTP время ожидания — 30 секунд.</span><span class="sxs-lookup"><span data-stu-id="3a247-210">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="3a247-211">Добавление нескольких обработчиков Polly</span><span class="sxs-lookup"><span data-stu-id="3a247-211">Add multiple Polly handlers</span></span>

<span data-ttu-id="3a247-212">Вложение политик Polly для расширения функциональных возможностей — это распространенная практика:</span><span class="sxs-lookup"><span data-stu-id="3a247-212">It is common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet9)]

<span data-ttu-id="3a247-213">В приведенном выше примере добавляются два обработчика.</span><span class="sxs-lookup"><span data-stu-id="3a247-213">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="3a247-214">Первый использует расширение `AddTransientHttpErrorPolicy`, чтобы добавить политику повтора.</span><span class="sxs-lookup"><span data-stu-id="3a247-214">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="3a247-215">Неудачные запросы выполняются повторно до трех раз.</span><span class="sxs-lookup"><span data-stu-id="3a247-215">Failed requests are retried up to three times.</span></span> <span data-ttu-id="3a247-216">Второй вызов к `AddTransientHttpErrorPolicy` добавляет политику размыкателя цепи.</span><span class="sxs-lookup"><span data-stu-id="3a247-216">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="3a247-217">Дополнительные внешние запросы блокируются в течение 30 секунд в случае пяти неудачных попыток подряд.</span><span class="sxs-lookup"><span data-stu-id="3a247-217">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="3a247-218">Политики размыкателя цепи отслеживают состояние.</span><span class="sxs-lookup"><span data-stu-id="3a247-218">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="3a247-219">Все вызовы через этот клиент имеют одинаковое состояние цепи.</span><span class="sxs-lookup"><span data-stu-id="3a247-219">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="3a247-220">Добавление политик из реестра Polly</span><span class="sxs-lookup"><span data-stu-id="3a247-220">Add policies from the Polly registry</span></span>

<span data-ttu-id="3a247-221">Подход к управлению регулярно используемыми политиками заключается в их однократном определении и регистрации с помощью `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="3a247-221">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="3a247-222">Предоставляется метод расширения, разрешающий добавление обработчика с помощью политики из реестра:</span><span class="sxs-lookup"><span data-stu-id="3a247-222">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet10)]

<span data-ttu-id="3a247-223">В приведенном выше коде PolicyRegistry добавляется в `ServiceCollection`, и с его помощью регистрируется две политики.</span><span class="sxs-lookup"><span data-stu-id="3a247-223">In the preceding code, a PolicyRegistry is added to the `ServiceCollection` and two policies are registered with it.</span></span> <span data-ttu-id="3a247-224">Чтобы использовать политику из реестра, применяется метод `AddPolicyHandlerFromRegistry`, который передает имя необходимой политики.</span><span class="sxs-lookup"><span data-stu-id="3a247-224">In order to use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="3a247-225">Дополнительные сведения об интеграции `IHttpClientFactory` и Polly см. на [вики-сайте Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="3a247-225">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="3a247-226">Управление HttpClient и временем существования</span><span class="sxs-lookup"><span data-stu-id="3a247-226">HttpClient and lifetime management</span></span>

<span data-ttu-id="3a247-227">При каждом вызове `CreateClient` в `IHttpClientFactory` возвращается новый экземпляр `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="3a247-227">Each time `CreateClient` is called on the `IHttpClientFactory`, a new instance of a `HttpClient` is returned.</span></span> <span data-ttu-id="3a247-228">Для каждого именованного клиента будет использоваться отдельный `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="3a247-228">There will be a `HttpMessageHandler` per named client.</span></span> <span data-ttu-id="3a247-229">`IHttpClientFactory` будет объединять в пул все экземпляры `HttpMessageHandler`, созданные фабрикой, чтобы уменьшить потребление ресурсов.</span><span class="sxs-lookup"><span data-stu-id="3a247-229">`IHttpClientFactory` will pool the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="3a247-230">Экземпляр `HttpMessageHandler` можно использовать повторно из пула при создании нового экземпляра `HttpClient`, если его время существования еще не истекло.</span><span class="sxs-lookup"><span data-stu-id="3a247-230">A `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span> 

<span data-ttu-id="3a247-231">Создание пулов обработчиков желательно, поскольку каждый обработчик обычно управляет собственными базовыми HTTP-подключениями. Создание лишних обработчиков может привести к задержке подключения.</span><span class="sxs-lookup"><span data-stu-id="3a247-231">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections; creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="3a247-232">Некоторые обработчики поддерживают подключения открытыми в течение неопределенного периода, что может помешать обработчику отреагировать на изменения DNS.</span><span class="sxs-lookup"><span data-stu-id="3a247-232">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="3a247-233">Время существования обработчика по умолчанию — две минуты.</span><span class="sxs-lookup"><span data-stu-id="3a247-233">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="3a247-234">Значение по умолчанию можно переопределить для каждого именованного клиента.</span><span class="sxs-lookup"><span data-stu-id="3a247-234">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="3a247-235">Чтобы переопределить это значение, вызовите `SetHandlerLifetime` в `IHttpClientBuilder`, который возвращается при создании клиента:</span><span class="sxs-lookup"><span data-stu-id="3a247-235">To override it, call `SetHandlerLifetime` on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet11)]

## <a name="logging"></a><span data-ttu-id="3a247-236">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="3a247-236">Logging</span></span>

<span data-ttu-id="3a247-237">Клиенты, созданные через `IHttpClientFactory`, записывают сообщения журнала для всех запросов.</span><span class="sxs-lookup"><span data-stu-id="3a247-237">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="3a247-238">Необходимо установить соответствующий уровень информации в конфигурации ведения журнала для просмотра сообщений журнала по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3a247-238">You'll need to enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="3a247-239">Дополнительное ведение журнала, например запись заголовков запросов, включено только на уровне трассировки.</span><span class="sxs-lookup"><span data-stu-id="3a247-239">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="3a247-240">Категория журнала для каждого клиента включает в себя имя клиента.</span><span class="sxs-lookup"><span data-stu-id="3a247-240">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="3a247-241">Клиент с именем MyNamedClient, например, записывает в журнал сообщения с категорией `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="3a247-241">A client named "MyNamedClient", for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="3a247-242">Сообщения с суффиксом LogicalHandler возникают на внешней стороне конвейера обработчиков запросов.</span><span class="sxs-lookup"><span data-stu-id="3a247-242">Messages with the suffix of "LogicalHandler" occur on the outside of request handler pipeline.</span></span> <span data-ttu-id="3a247-243">Во время запроса сообщения записываются в журнал до обработки запроса другими обработчиками в конвейере.</span><span class="sxs-lookup"><span data-stu-id="3a247-243">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="3a247-244">Во время ответа сообщения записываются в журнал после получения ответа другими обработчиками в конвейере.</span><span class="sxs-lookup"><span data-stu-id="3a247-244">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="3a247-245">Ведение журнала также происходит внутри конвейера обработчиков запросов.</span><span class="sxs-lookup"><span data-stu-id="3a247-245">Logging also occurs on the inside of the request handler pipeline.</span></span> <span data-ttu-id="3a247-246">В примере MyNamedClient эти сообщения вносятся в журнал по категории журнала `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="3a247-246">In the case of the "MyNamedClient" example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="3a247-247">Во время запроса это происходит после выполнения всех обработчиков и непосредственно перед отправкой запроса по сети.</span><span class="sxs-lookup"><span data-stu-id="3a247-247">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="3a247-248">Во время ответа в журнале записывается состояние ответа перед его передачей обратно по конвейеру обработчиков.</span><span class="sxs-lookup"><span data-stu-id="3a247-248">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="3a247-249">Включение ведения журнала на внешней и внутренней стороне конвейера позволяет выполнять проверку изменений, внесенных другими обработчиками конвейера.</span><span class="sxs-lookup"><span data-stu-id="3a247-249">Enabling logging on the outside and inside of the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="3a247-250">Сюда входят, например, изменения заголовков запросов или кода состояния ответов.</span><span class="sxs-lookup"><span data-stu-id="3a247-250">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="3a247-251">Включение имени клиента в категорию журнала позволяет фильтровать журналы по именованным клиентам при необходимости.</span><span class="sxs-lookup"><span data-stu-id="3a247-251">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="3a247-252">Настройка HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="3a247-252">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="3a247-253">Иногда необходимо контролировать конфигурацию внутреннего обработчика `HttpMessageHandler`, используемого клиентом.</span><span class="sxs-lookup"><span data-stu-id="3a247-253">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="3a247-254">При добавлении именованного или типизированного клиента возвращается `IHttpClientBuilder`.</span><span class="sxs-lookup"><span data-stu-id="3a247-254">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="3a247-255">Для определения делегата можно использовать метод расширения `ConfigurePrimaryHttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="3a247-255">The `ConfigurePrimaryHttpMessageHandler` extension method can be used to define a delegate.</span></span> <span data-ttu-id="3a247-256">Делегат используется для создания и настройки основного обработчика `HttpMessageHandler`, используемого этим клиентом:</span><span class="sxs-lookup"><span data-stu-id="3a247-256">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet12)]
