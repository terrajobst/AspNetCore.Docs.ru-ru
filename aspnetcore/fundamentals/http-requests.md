---
title: Выполнения HTTP-запросов с помощью IHttpClientFactory в ASP.NET Core
author: stevejgordon
description: Сведения об использовании интерфейса IHttpClientFactory для управления логическими экземплярами HttpClient в ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 08/01/2019
uid: fundamentals/http-requests
ms.openlocfilehash: bcf2a2eaf6910222d274c38bac343c92fab9cb5b
ms.sourcegitcommit: b5e63714afc26e94be49a92619586df5189ed93a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739538"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="22d06-103">Выполнения HTTP-запросов с помощью IHttpClientFactory в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="22d06-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

<span data-ttu-id="22d06-104">Авторы: [Гленн Кондрон (Glenn Condron)](https://github.com/glennc), [Райан Новак (Ryan Nowak)](https://github.com/rynowak) и [Стив Гордон (Steve Gordon)](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="22d06-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="22d06-105"><xref:System.Net.Http.IHttpClientFactory> можно зарегистрировать и использовать для настройки и создания экземпляров <xref:System.Net.Http.HttpClient> в приложении.</span><span class="sxs-lookup"><span data-stu-id="22d06-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="22d06-106">Так вы получите следующие преимущества:</span><span class="sxs-lookup"><span data-stu-id="22d06-106">It offers the following benefits:</span></span>

* <span data-ttu-id="22d06-107">Центральное расположение для именования и настройки логических экземпляров `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="22d06-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="22d06-108">Например, можно зарегистрировать и настроить клиент *github* для доступа к [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="22d06-108">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="22d06-109">Можно зарегистрировать клиент по умолчанию для других целей.</span><span class="sxs-lookup"><span data-stu-id="22d06-109">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="22d06-110">Кодификация концепции исходящего ПО промежуточного слоя путем делегирования обработчиков в `HttpClient` и предоставление расширений для ПО промежуточного слоя на основе Polly для использования этой возможности.</span><span class="sxs-lookup"><span data-stu-id="22d06-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="22d06-111">Управление созданием пулов и временем существования базовых экземпляров `HttpClientMessageHandler` с целью избежать обычных проблем с DNS, которые возникают при управлении временем существования `HttpClient` вручную.</span><span class="sxs-lookup"><span data-stu-id="22d06-111">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="22d06-112">Настройка параметров ведения журнала (через `ILogger`) для всех запросов, отправленных через клиентов, созданных фабрикой.</span><span class="sxs-lookup"><span data-stu-id="22d06-112">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="22d06-113">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="22d06-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker range="<= aspnetcore-2.2"

## <a name="prerequisites"></a><span data-ttu-id="22d06-114">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="22d06-114">Prerequisites</span></span>

<span data-ttu-id="22d06-115">Для проектов, предназначенных для .NET Framework, необходимо установить пакет NuGet [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/).</span><span class="sxs-lookup"><span data-stu-id="22d06-115">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="22d06-116">Пакет `Microsoft.Extensions.Http` уже включен в проекты, предназначенные для .NET Core и ссылающиеся на [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="22d06-116">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

::: moniker-end

## <a name="consumption-patterns"></a><span data-ttu-id="22d06-117">Принципы использования</span><span class="sxs-lookup"><span data-stu-id="22d06-117">Consumption patterns</span></span>

<span data-ttu-id="22d06-118">Существует несколько способов использования `IHttpClientFactory` в приложении:</span><span class="sxs-lookup"><span data-stu-id="22d06-118">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="22d06-119">Основное использование</span><span class="sxs-lookup"><span data-stu-id="22d06-119">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="22d06-120">Именованные клиенты</span><span class="sxs-lookup"><span data-stu-id="22d06-120">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="22d06-121">Типизированные клиенты</span><span class="sxs-lookup"><span data-stu-id="22d06-121">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="22d06-122">Созданные клиенты</span><span class="sxs-lookup"><span data-stu-id="22d06-122">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="22d06-123">Все способы равноценны.</span><span class="sxs-lookup"><span data-stu-id="22d06-123">None of them are strictly superior to another.</span></span> <span data-ttu-id="22d06-124">Оптимальный подход зависит от ограничений приложения.</span><span class="sxs-lookup"><span data-stu-id="22d06-124">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="22d06-125">Основное использование</span><span class="sxs-lookup"><span data-stu-id="22d06-125">Basic usage</span></span>

<span data-ttu-id="22d06-126">`IHttpClientFactory` можно зарегистрировать путем вызова метода расширения `AddHttpClient` в `IServiceCollection` внутри метода `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="22d06-126">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="22d06-127">После регистрации код может принимать `IHttpClientFactory` в любом месте, куда можно внедрить службу с помощью [внедрения зависимостей](xref:fundamentals/dependency-injection) (DI).</span><span class="sxs-lookup"><span data-stu-id="22d06-127">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="22d06-128">`IHttpClientFactory` можно использовать для создания экземпляра `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="22d06-128">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="22d06-129">Подобное использование `IHttpClientFactory` — это отличный способ рефакторинга имеющегося приложения.</span><span class="sxs-lookup"><span data-stu-id="22d06-129">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="22d06-130">Он не оказывает влияния на использование `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="22d06-130">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="22d06-131">Там, где в данный момент создаются экземпляры `HttpClient`, используйте вызов к <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="22d06-131">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="22d06-132">Именованные клиенты</span><span class="sxs-lookup"><span data-stu-id="22d06-132">Named clients</span></span>

<span data-ttu-id="22d06-133">Если для приложения предполагаются разные способы использования `HttpClient`, каждый со своей конфигурацией, можно применять **именованные клиенты**.</span><span class="sxs-lookup"><span data-stu-id="22d06-133">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="22d06-134">Конфигурацию для именованного клиента `HttpClient` можно указать во время регистрации в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="22d06-134">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="22d06-135">В приведенном выше коде вызывается клиент `AddHttpClient`, предоставляющий имя *github*.</span><span class="sxs-lookup"><span data-stu-id="22d06-135">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="22d06-136">У клиента есть некоторые настройки по умолчанию &mdash; а именно: базовый адрес и два заголовка, необходимые для работы с API GitHub.</span><span class="sxs-lookup"><span data-stu-id="22d06-136">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="22d06-137">При каждом вызове `CreateClient` создается новый экземпляр `HttpClient` и вызывается действие конфигурации.</span><span class="sxs-lookup"><span data-stu-id="22d06-137">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="22d06-138">Для использования именованного клиента можно передать строковый параметр в `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="22d06-138">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="22d06-139">Укажите имя создаваемого клиента:</span><span class="sxs-lookup"><span data-stu-id="22d06-139">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="22d06-140">В приведенном выше коде в запросе не требуется указывать имя узла.</span><span class="sxs-lookup"><span data-stu-id="22d06-140">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="22d06-141">Достаточно передать только путь, так как используется базовый адрес, заданный для клиента.</span><span class="sxs-lookup"><span data-stu-id="22d06-141">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="22d06-142">Типизированные клиенты</span><span class="sxs-lookup"><span data-stu-id="22d06-142">Typed clients</span></span>

<span data-ttu-id="22d06-143">Типизированные клиенты:</span><span class="sxs-lookup"><span data-stu-id="22d06-143">Typed clients:</span></span>

* <span data-ttu-id="22d06-144">предоставляют те же возможности, что и именованные клиенты, без необходимости использовать строки в качестве ключей.</span><span class="sxs-lookup"><span data-stu-id="22d06-144">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="22d06-145">Это помогает IntelliSense и компилятору при использовании клиентов.</span><span class="sxs-lookup"><span data-stu-id="22d06-145">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="22d06-146">Они предоставляют единое расположение для настройки и взаимодействия с конкретным клиентом `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="22d06-146">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="22d06-147">Например, для конечной точки серверной части можно использовать один типизированный клиент, который будет содержать всю логику работы с этой конечной точкой.</span><span class="sxs-lookup"><span data-stu-id="22d06-147">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="22d06-148">Поддерживаются работа с внедрением зависимостей и возможность вставки в нужное место в приложении.</span><span class="sxs-lookup"><span data-stu-id="22d06-148">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="22d06-149">Типизированный клиент принимает параметр `HttpClient` в конструкторе:</span><span class="sxs-lookup"><span data-stu-id="22d06-149">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="22d06-150">В приведенном выше коде конфигурация перемещается в типизированный клиент.</span><span class="sxs-lookup"><span data-stu-id="22d06-150">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="22d06-151">Объект `HttpClient` предоставляется в виде открытого свойства.</span><span class="sxs-lookup"><span data-stu-id="22d06-151">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="22d06-152">Можно определить связанные с API методы, которые предоставляют функциональные возможности `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="22d06-152">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="22d06-153">Метод `GetAspNetDocsIssues` инкапсулирует код, необходимый для запроса и анализа последнего открытого выпуска из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="22d06-153">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="22d06-154">Для регистрации типизированного клиента можно использовать универсальный метод расширения <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> в `Startup.ConfigureServices`, указав класс типизированного клиента:</span><span class="sxs-lookup"><span data-stu-id="22d06-154">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="22d06-155">Типизированный клиент регистрируется во внедрении зависимостей как временный.</span><span class="sxs-lookup"><span data-stu-id="22d06-155">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="22d06-156">Типизированный клиент можно внедрить и использовать напрямую:</span><span class="sxs-lookup"><span data-stu-id="22d06-156">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="22d06-157">При желании конфигурацию для типизированного клиента можно указать во время регистрации в `Startup.ConfigureServices`, а не в конструкторе типизированного клиента:</span><span class="sxs-lookup"><span data-stu-id="22d06-157">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="22d06-158">Можно полностью инкапсулировать `HttpClient` внутри типизированного клиента.</span><span class="sxs-lookup"><span data-stu-id="22d06-158">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="22d06-159">Вместо предоставления его как свойства можно использовать открытые методы для внутреннего вызова экземпляра `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="22d06-159">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="22d06-160">В приведенном выше коде `HttpClient` хранится как закрытое поле.</span><span class="sxs-lookup"><span data-stu-id="22d06-160">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="22d06-161">Любой доступ для совершения внешних вызовов осуществляется через метод `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="22d06-161">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="22d06-162">Созданные клиенты</span><span class="sxs-lookup"><span data-stu-id="22d06-162">Generated clients</span></span>

<span data-ttu-id="22d06-163">`IHttpClientFactory` можно использовать в сочетании с другими библиотеками сторонних разработчиков, например [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="22d06-163">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="22d06-164">Refit — это библиотека REST для .NET.</span><span class="sxs-lookup"><span data-stu-id="22d06-164">Refit is a REST library for .NET.</span></span> <span data-ttu-id="22d06-165">Она преобразует REST API в динамические интерфейсы.</span><span class="sxs-lookup"><span data-stu-id="22d06-165">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="22d06-166">Реализация интерфейса формируется динамически с помощью `RestService` с использованием `HttpClient` для совершения внешних вызовов HTTP.</span><span class="sxs-lookup"><span data-stu-id="22d06-166">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="22d06-167">Для представления внешнего API и его ответа определяются интерфейс и ответ:</span><span class="sxs-lookup"><span data-stu-id="22d06-167">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="22d06-168">Можно добавить типизированный клиент, используя Refit для создания реализации:</span><span class="sxs-lookup"><span data-stu-id="22d06-168">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="22d06-169">При необходимости можно использовать определенный интерфейс с реализацией, предоставленной внедрением зависимостей и Refit:</span><span class="sxs-lookup"><span data-stu-id="22d06-169">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="22d06-170">ПО промежуточного слоя для исходящих запросов</span><span class="sxs-lookup"><span data-stu-id="22d06-170">Outgoing request middleware</span></span>

<span data-ttu-id="22d06-171">В `HttpClient` уже существует концепция делегирования обработчиков, которые можно связать друг с другом для исходящих HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="22d06-171">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="22d06-172">Класс `IHttpClientFactory` упрощает определение обработчиков для применения к каждому именованному клиенту.</span><span class="sxs-lookup"><span data-stu-id="22d06-172">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="22d06-173">Он поддерживает регистрацию и объединение в цепочки нескольких обработчиков для создания конвейера ПО промежуточного слоя для исходящих запросов.</span><span class="sxs-lookup"><span data-stu-id="22d06-173">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="22d06-174">Каждый из этих обработчиков может выполнять работу до и после исходящего запроса.</span><span class="sxs-lookup"><span data-stu-id="22d06-174">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="22d06-175">Этот шаблон похож на входящий конвейер ПО промежуточного слоя в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="22d06-175">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="22d06-176">Шаблон предоставляет механизм управления сквозной функциональностью HTTP-запросов, включая кэширование, обработку ошибок, сериализацию и ведение журнала.</span><span class="sxs-lookup"><span data-stu-id="22d06-176">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="22d06-177">Чтобы создать обработчик, необходимо определить класс, производный от <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="22d06-177">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="22d06-178">Переопределите метод `SendAsync` для выполнения кода до передачи запросов следующему обработчику в конвейере:</span><span class="sxs-lookup"><span data-stu-id="22d06-178">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="22d06-179">В предыдущем коде определяется базовый обработчик.</span><span class="sxs-lookup"><span data-stu-id="22d06-179">The preceding code defines a basic handler.</span></span> <span data-ttu-id="22d06-180">Он проверяет, включен ли в запрос заголовок `X-API-KEY`.</span><span class="sxs-lookup"><span data-stu-id="22d06-180">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="22d06-181">Если заголовок отсутствует, он может избежать вызовов HTTP и вернуть подходящий ответ.</span><span class="sxs-lookup"><span data-stu-id="22d06-181">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="22d06-182">Во время регистрации можно добавить один или несколько обработчиков в конфигурацию для `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="22d06-182">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="22d06-183">Эта задача выполняется через методы расширения в <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="22d06-183">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="22d06-184">В приведенном выше коде `ValidateHeaderHandler` регистрируется с помощью внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="22d06-184">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="22d06-185">`IHttpClientFactory` создает отдельную область внедрения зависимостей для каждого обработчика.</span><span class="sxs-lookup"><span data-stu-id="22d06-185">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="22d06-186">Обработчики могут зависеть от служб из любой области.</span><span class="sxs-lookup"><span data-stu-id="22d06-186">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="22d06-187">Службы, которые зависят от обработчиков, удаляются при удалении обработчика.</span><span class="sxs-lookup"><span data-stu-id="22d06-187">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="22d06-188">После регистрации можно вызвать <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, передав тип обработчика.</span><span class="sxs-lookup"><span data-stu-id="22d06-188">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="22d06-189">В приведенном выше коде `ValidateHeaderHandler` регистрируется с помощью внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="22d06-189">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="22d06-190">Обработчик **должен** регистрироваться во внедрении зависимостей как временная служба, а не ограниченная.</span><span class="sxs-lookup"><span data-stu-id="22d06-190">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="22d06-191">Если обработчик зарегистрирован в качестве службы с областью действия и все службы, от которых зависит этот обработчик, освобождаются:</span><span class="sxs-lookup"><span data-stu-id="22d06-191">If the handler is registered as a scoped service and any services that the handler depends upon are disposable:</span></span>

* <span data-ttu-id="22d06-192">Службы обработчика могли быть удалены, прежде чем обработчик вышел из области действия.</span><span class="sxs-lookup"><span data-stu-id="22d06-192">The handler's services could be disposed before the handler goes out of scope.</span></span>
* <span data-ttu-id="22d06-193">Освобожденные службы обработчика приводят к его сбою.</span><span class="sxs-lookup"><span data-stu-id="22d06-193">The disposed handler services causes the handler to fail.</span></span>

<span data-ttu-id="22d06-194">После регистрации можно вызвать <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, передав тип обработчика.</span><span class="sxs-lookup"><span data-stu-id="22d06-194">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

::: moniker-end

<span data-ttu-id="22d06-195">Можно зарегистрировать несколько обработчиков в порядке, в котором они должны выполняться.</span><span class="sxs-lookup"><span data-stu-id="22d06-195">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="22d06-196">Каждый обработчик содержит следующий обработчик, пока последний `HttpClientHandler` не выполнит запрос:</span><span class="sxs-lookup"><span data-stu-id="22d06-196">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="22d06-197">Используйте один из следующих методов для предоставления общего доступа к состоянию отдельных запросов с помощью обработчиков сообщений:</span><span class="sxs-lookup"><span data-stu-id="22d06-197">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="22d06-198">Передайте данные в обработчик с помощью `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="22d06-198">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="22d06-199">Используйте `IHttpContextAccessor` для доступа к текущему запросу.</span><span class="sxs-lookup"><span data-stu-id="22d06-199">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="22d06-200">Создайте пользовательский объект хранилища `AsyncLocal` для передачи данных.</span><span class="sxs-lookup"><span data-stu-id="22d06-200">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="22d06-201">Использование обработчиков на основе Polly</span><span class="sxs-lookup"><span data-stu-id="22d06-201">Use Polly-based handlers</span></span>

<span data-ttu-id="22d06-202">`IHttpClientFactory` интегрируется с популярной библиотекой сторонних разработчиков под названием [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="22d06-202">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="22d06-203">Polly — это комплексная библиотека, обеспечивающая отказоустойчивость и обработку временных сбоев в .NET.</span><span class="sxs-lookup"><span data-stu-id="22d06-203">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="22d06-204">Она позволяет разработчикам выражать политики, например политику повтора, размыкателя цепи, времени ожидания, изоляции отсеков и отката, более эффективным и потокобезопасным образом.</span><span class="sxs-lookup"><span data-stu-id="22d06-204">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="22d06-205">Для использования политик Polly с настроенными экземплярами `HttpClient` предоставляются методы расширения.</span><span class="sxs-lookup"><span data-stu-id="22d06-205">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="22d06-206">Расширения Polly:</span><span class="sxs-lookup"><span data-stu-id="22d06-206">The Polly extensions:</span></span>

* <span data-ttu-id="22d06-207">Поддерживает добавление обработчиков на основе Polly клиентам.</span><span class="sxs-lookup"><span data-stu-id="22d06-207">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="22d06-208">Можно использовать после установки пакета NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/).</span><span class="sxs-lookup"><span data-stu-id="22d06-208">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="22d06-209">Пакет не включен в общую платформу ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="22d06-209">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="22d06-210">Обработка временных сбоев</span><span class="sxs-lookup"><span data-stu-id="22d06-210">Handle transient faults</span></span>

<span data-ttu-id="22d06-211">Чаще всего ошибки происходят, когда внешние вызовы HTTP являются временными.</span><span class="sxs-lookup"><span data-stu-id="22d06-211">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="22d06-212">Используется удобный метод расширения `AddTransientHttpErrorPolicy`, который позволяет определить политику для обработки временных ошибок.</span><span class="sxs-lookup"><span data-stu-id="22d06-212">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="22d06-213">Политики, заданные с помощью этого метода расширения, обрабатывают `HttpRequestException`, ответы HTTP 5xx и ответы HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="22d06-213">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="22d06-214">Расширение `AddTransientHttpErrorPolicy` может быть использовано в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="22d06-214">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="22d06-215">Данное расширение предоставляет доступ к объекту `PolicyBuilder`, настроенному для обработки ошибок, представляющих возможный временный сбой:</span><span class="sxs-lookup"><span data-stu-id="22d06-215">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="22d06-216">В приведенном выше коде определена политика `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="22d06-216">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="22d06-217">Неудачные запросы повторяются до трех раз с задержкой 600 мс между попытками.</span><span class="sxs-lookup"><span data-stu-id="22d06-217">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="22d06-218">Динамический выбор политик</span><span class="sxs-lookup"><span data-stu-id="22d06-218">Dynamically select policies</span></span>

<span data-ttu-id="22d06-219">Существуют дополнительные методы расширения, которые можно использовать для добавления обработчиков на основе Polly.</span><span class="sxs-lookup"><span data-stu-id="22d06-219">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="22d06-220">Одним из таких расширений является `AddPolicyHandler` с несколькими перегрузками.</span><span class="sxs-lookup"><span data-stu-id="22d06-220">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="22d06-221">Одна перегрузка разрешает проверку запроса для определения необходимой политики:</span><span class="sxs-lookup"><span data-stu-id="22d06-221">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="22d06-222">Если в приведенном выше коде исходящий запрос является запросом HTTP GET, применяется время ожидания 10 секунд.</span><span class="sxs-lookup"><span data-stu-id="22d06-222">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="22d06-223">Для остальных методов HTTP время ожидания — 30 секунд.</span><span class="sxs-lookup"><span data-stu-id="22d06-223">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="22d06-224">Добавление нескольких обработчиков Polly</span><span class="sxs-lookup"><span data-stu-id="22d06-224">Add multiple Polly handlers</span></span>

<span data-ttu-id="22d06-225">Общепринятой практикой является вложение политик Polly для предоставления расширенной функциональности:</span><span class="sxs-lookup"><span data-stu-id="22d06-225">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="22d06-226">В приведенном выше примере добавляются два обработчика.</span><span class="sxs-lookup"><span data-stu-id="22d06-226">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="22d06-227">Первый использует расширение `AddTransientHttpErrorPolicy`, чтобы добавить политику повтора.</span><span class="sxs-lookup"><span data-stu-id="22d06-227">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="22d06-228">Неудачные запросы выполняются повторно до трех раз.</span><span class="sxs-lookup"><span data-stu-id="22d06-228">Failed requests are retried up to three times.</span></span> <span data-ttu-id="22d06-229">Второй вызов к `AddTransientHttpErrorPolicy` добавляет политику размыкателя цепи.</span><span class="sxs-lookup"><span data-stu-id="22d06-229">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="22d06-230">Дополнительные внешние запросы блокируются в течение 30 секунд в случае пяти неудачных попыток подряд.</span><span class="sxs-lookup"><span data-stu-id="22d06-230">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="22d06-231">Политики размыкателя цепи отслеживают состояние.</span><span class="sxs-lookup"><span data-stu-id="22d06-231">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="22d06-232">Все вызовы через этот клиент имеют одинаковое состояние цепи.</span><span class="sxs-lookup"><span data-stu-id="22d06-232">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="22d06-233">Добавление политик из реестра Polly</span><span class="sxs-lookup"><span data-stu-id="22d06-233">Add policies from the Polly registry</span></span>

<span data-ttu-id="22d06-234">Подход к управлению регулярно используемыми политиками заключается в их однократном определении и регистрации с помощью `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="22d06-234">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="22d06-235">Предоставляется метод расширения, разрешающий добавление обработчика с помощью политики из реестра:</span><span class="sxs-lookup"><span data-stu-id="22d06-235">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="22d06-236">В приведенном выше коде, когда `PolicyRegistry` добавляется в `ServiceCollection`, регистрируются две политики.</span><span class="sxs-lookup"><span data-stu-id="22d06-236">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="22d06-237">Чтобы использовать политику из реестра, применяется метод `AddPolicyHandlerFromRegistry`, который передает имя необходимой политики.</span><span class="sxs-lookup"><span data-stu-id="22d06-237">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="22d06-238">Дополнительные сведения об интеграции `IHttpClientFactory` и Polly см. на [вики-сайте Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="22d06-238">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="22d06-239">Управление HttpClient и временем существования</span><span class="sxs-lookup"><span data-stu-id="22d06-239">HttpClient and lifetime management</span></span>

<span data-ttu-id="22d06-240">При каждом вызове `CreateClient` в `IHttpClientFactory` возвращается новый экземпляр `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="22d06-240">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="22d06-241">Для каждого названного клиента существует <xref:System.Net.Http.HttpMessageHandler>.</span><span class="sxs-lookup"><span data-stu-id="22d06-241">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="22d06-242">Фабрика обеспечивает управление временем существования экземпляров `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="22d06-242">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="22d06-243">`IHttpClientFactory` объединяет в пул все экземпляры `HttpMessageHandler`, созданные фабрикой, чтобы уменьшить потребление ресурсов.</span><span class="sxs-lookup"><span data-stu-id="22d06-243">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="22d06-244">Экземпляр `HttpMessageHandler` можно использовать повторно из пула при создании экземпляра `HttpClient`, если его время существования еще не истекло.</span><span class="sxs-lookup"><span data-stu-id="22d06-244">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="22d06-245">Создавать пулы обработчиков желательно, так как каждый обработчик обычно управляет собственными базовыми HTTP-подключениями.</span><span class="sxs-lookup"><span data-stu-id="22d06-245">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="22d06-246">Создание лишних обработчиков может привести к задержке подключения.</span><span class="sxs-lookup"><span data-stu-id="22d06-246">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="22d06-247">Некоторые обработчики поддерживают подключения открытыми в течение неопределенного периода, что может помешать обработчику отреагировать на изменения DNS.</span><span class="sxs-lookup"><span data-stu-id="22d06-247">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="22d06-248">Время существования обработчика по умолчанию — две минуты.</span><span class="sxs-lookup"><span data-stu-id="22d06-248">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="22d06-249">Значение по умолчанию можно переопределить для каждого именованного клиента.</span><span class="sxs-lookup"><span data-stu-id="22d06-249">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="22d06-250">Чтобы переопределить это значение, вызовите <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> в `IHttpClientBuilder`, который возвращается при создании клиента:</span><span class="sxs-lookup"><span data-stu-id="22d06-250">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="22d06-251">Высвобождать клиент не требуется.</span><span class="sxs-lookup"><span data-stu-id="22d06-251">Disposal of the client isn't required.</span></span> <span data-ttu-id="22d06-252">Высвобождение отменяет исходящие запросы и гарантирует, что указанный экземпляр `HttpClient` не может использоваться после вызова <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="22d06-252">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="22d06-253">`IHttpClientFactory` отслеживает и высвобождает ресурсы, используемые экземплярами `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="22d06-253">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="22d06-254">Экземпляры `HttpClient` обычно можно рассматривать как объекты .NET, не требующие высвобождения.</span><span class="sxs-lookup"><span data-stu-id="22d06-254">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="22d06-255">До появления `IHttpClientFactory` один экземпляр `HttpClient` часто сохраняли в активном состоянии в течение длительного времени.</span><span class="sxs-lookup"><span data-stu-id="22d06-255">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="22d06-256">После перехода на `IHttpClientFactory` это уже не нужно.</span><span class="sxs-lookup"><span data-stu-id="22d06-256">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

## <a name="logging"></a><span data-ttu-id="22d06-257">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="22d06-257">Logging</span></span>

<span data-ttu-id="22d06-258">Клиенты, созданные через `IHttpClientFactory`, записывают сообщения журнала для всех запросов.</span><span class="sxs-lookup"><span data-stu-id="22d06-258">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="22d06-259">Установите соответствующий уровень информации в конфигурации ведения журнала, чтобы просматривать сообщения журнала по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="22d06-259">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="22d06-260">Дополнительное ведение журнала, например запись заголовков запросов, включено только на уровне трассировки.</span><span class="sxs-lookup"><span data-stu-id="22d06-260">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="22d06-261">Категория журнала для каждого клиента включает в себя имя клиента.</span><span class="sxs-lookup"><span data-stu-id="22d06-261">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="22d06-262">Клиент с именем *MyNamedClient*, например, записывает в журнал сообщения с категорией `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="22d06-262">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="22d06-263">Сообщения с суффиксом *LogicalHandler* создаются за пределами конвейера обработчиков запросов.</span><span class="sxs-lookup"><span data-stu-id="22d06-263">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="22d06-264">Во время запроса сообщения записываются в журнал до обработки запроса другими обработчиками в конвейере.</span><span class="sxs-lookup"><span data-stu-id="22d06-264">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="22d06-265">Во время ответа сообщения записываются в журнал после получения ответа другими обработчиками в конвейере.</span><span class="sxs-lookup"><span data-stu-id="22d06-265">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="22d06-266">Кроме того, журнал ведется в конвейере обработчиков запросов.</span><span class="sxs-lookup"><span data-stu-id="22d06-266">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="22d06-267">В примере *MyNamedClient* эти сообщения вносятся в журнал по категории журнала `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="22d06-267">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="22d06-268">Во время запроса это происходит после выполнения всех обработчиков и непосредственно перед отправкой запроса по сети.</span><span class="sxs-lookup"><span data-stu-id="22d06-268">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="22d06-269">Во время ответа в журнале записывается состояние ответа перед его передачей обратно по конвейеру обработчиков.</span><span class="sxs-lookup"><span data-stu-id="22d06-269">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="22d06-270">Включив ведение журнала в конвейере и за его пределами, можно выполнять проверку изменений, внесенных другими обработчиками конвейера.</span><span class="sxs-lookup"><span data-stu-id="22d06-270">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="22d06-271">Сюда входят, например, изменения заголовков запросов или кода состояния ответов.</span><span class="sxs-lookup"><span data-stu-id="22d06-271">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="22d06-272">Включение имени клиента в категорию журнала позволяет фильтровать журналы по именованным клиентам при необходимости.</span><span class="sxs-lookup"><span data-stu-id="22d06-272">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="22d06-273">Настройка HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="22d06-273">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="22d06-274">Иногда необходимо контролировать конфигурацию внутреннего обработчика `HttpMessageHandler`, используемого клиентом.</span><span class="sxs-lookup"><span data-stu-id="22d06-274">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="22d06-275">При добавлении именованного или типизированного клиента возвращается `IHttpClientBuilder`.</span><span class="sxs-lookup"><span data-stu-id="22d06-275">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="22d06-276">Для определения делегата можно использовать метод расширения <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>.</span><span class="sxs-lookup"><span data-stu-id="22d06-276">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="22d06-277">Делегат используется для создания и настройки основного обработчика `HttpMessageHandler`, используемого этим клиентом:</span><span class="sxs-lookup"><span data-stu-id="22d06-277">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="22d06-278">Использование IHttpClientFactory в консольном приложении</span><span class="sxs-lookup"><span data-stu-id="22d06-278">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="22d06-279">В консольном приложении добавьте в проект следующие ссылки на пакеты:</span><span class="sxs-lookup"><span data-stu-id="22d06-279">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="22d06-280">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="22d06-280">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="22d06-281">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="22d06-281">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="22d06-282">В следующем примере:</span><span class="sxs-lookup"><span data-stu-id="22d06-282">In the following example:</span></span>

* <span data-ttu-id="22d06-283"><xref:System.Net.Http.IHttpClientFactory> регистрируется в контейнере службы [универсального узла](xref:fundamentals/host/generic-host):</span><span class="sxs-lookup"><span data-stu-id="22d06-283"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="22d06-284">`MyService` создает экземпляр фабрики клиента из службы, который используется для создания `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="22d06-284">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="22d06-285">`HttpClient` используется для получения веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="22d06-285">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="22d06-286">`Main` создает область для выполнения метода `GetPage` службы и вывода первых 500 символов содержимого веб-страницы на консоль.</span><span class="sxs-lookup"><span data-stu-id="22d06-286">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="22d06-287">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="22d06-287">Additional resources</span></span>

* [<span data-ttu-id="22d06-288">Использование HttpClientFactory для реализации устойчивых HTTP-запросов</span><span class="sxs-lookup"><span data-stu-id="22d06-288">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="22d06-289">Реализация повторных попыток вызова HTTP с экспоненциальной задержкой с помощью HttpClientFactory и политик Polly</span><span class="sxs-lookup"><span data-stu-id="22d06-289">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="22d06-290">Реализация шаблона размыкателя цепи</span><span class="sxs-lookup"><span data-stu-id="22d06-290">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)
