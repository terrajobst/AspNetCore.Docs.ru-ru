---
title: Выполнения HTTP-запросов с помощью IHttpClientFactory в ASP.NET Core
author: stevejgordon
description: Сведения об использовании интерфейса IHttpClientFactory для управления логическими экземплярами HttpClient в ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 10/27/2019
uid: fundamentals/http-requests
ms.openlocfilehash: a963833acfa12889c8ae3dac443962682e1cb931
ms.sourcegitcommit: 032113208bb55ecfb2faeb6d3e9ea44eea827950
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/31/2019
ms.locfileid: "73190581"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="f0740-103">Выполнения HTTP-запросов с помощью IHttpClientFactory в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f0740-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f0740-104">Авторы: [Гленн Кондрон (Glenn Condron)](https://github.com/glennc), [Райан Новак (Ryan Nowak)](https://github.com/rynowak), [Стив Гордон (Steve Gordon)](https://github.com/stevejgordon), [Рик Андерсон (Rick Anderson)](https://twitter.com/RickAndMSFT) и [Кирк Ларкин (Kirk Larkin)](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="f0740-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak),  [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="f0740-105"><xref:System.Net.Http.IHttpClientFactory> можно зарегистрировать и использовать для настройки и создания экземпляров <xref:System.Net.Http.HttpClient> в приложении.</span><span class="sxs-lookup"><span data-stu-id="f0740-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="f0740-106">`IHttpClientFactory` предоставляет следующие преимущества:</span><span class="sxs-lookup"><span data-stu-id="f0740-106">`IHttpClientFactory` offers the following benefits:</span></span>

* <span data-ttu-id="f0740-107">Центральное расположение для именования и настройки логических экземпляров `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f0740-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="f0740-108">Например, можно зарегистрировать и настроить клиент *github* для доступа к [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="f0740-108">For example, a client named  *github* could be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="f0740-109">Можно зарегистрировать клиент по умолчанию для общего доступа.</span><span class="sxs-lookup"><span data-stu-id="f0740-109">A default client can be registered for general access.</span></span>
* <span data-ttu-id="f0740-110">Кодификация концепции исходящего ПО промежуточного слоя путем делегирования обработчиков в `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f0740-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient`.</span></span> <span data-ttu-id="f0740-111">Предоставление расширений для ПО промежуточного слоя на основе Polly для делегирования обработчиков в `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f0740-111">Provides extensions for Polly-based middleware to take advantage of delegating handlers in `HttpClient`.</span></span>
* <span data-ttu-id="f0740-112">Управление созданием пулов и временем существования базовых экземпляров `HttpClientMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="f0740-112">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances.</span></span> <span data-ttu-id="f0740-113">Автоматическое управление позволяет избежать обычных проблем со службой доменных имен (DNS), которые возникают при управлении временем существования `HttpClient` вручную.</span><span class="sxs-lookup"><span data-stu-id="f0740-113">Automatic management avoids common DNS (Domain Name System) problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="f0740-114">Настройка параметров ведения журнала (через `ILogger`) для всех запросов, отправленных через клиентов, созданных фабрикой.</span><span class="sxs-lookup"><span data-stu-id="f0740-114">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="f0740-115">[Просмотреть или скачать пример кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="f0740-115">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="f0740-116">Пример кода в этой версии раздела использует <xref:System.Text.Json> для десериализации содержимого JSON, возвращаемого в ответах HTTP.</span><span class="sxs-lookup"><span data-stu-id="f0740-116">The sample code in this topic version uses <xref:System.Text.Json> to deserialize JSON content returned in HTTP responses.</span></span> <span data-ttu-id="f0740-117">Для примеров, использующих `Json.NET` и `ReadAsAsync<T>`, воспользуйтесь средством выбора версии, чтобы выбрать версию 2.x этого раздела.</span><span class="sxs-lookup"><span data-stu-id="f0740-117">For samples that use `Json.NET` and `ReadAsAsync<T>`, use the version selector to select a 2.x version of this topic.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="f0740-118">Принципы использования</span><span class="sxs-lookup"><span data-stu-id="f0740-118">Consumption patterns</span></span>

<span data-ttu-id="f0740-119">Существует несколько способов использования `IHttpClientFactory` в приложении:</span><span class="sxs-lookup"><span data-stu-id="f0740-119">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="f0740-120">Основное использование</span><span class="sxs-lookup"><span data-stu-id="f0740-120">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="f0740-121">Именованные клиенты</span><span class="sxs-lookup"><span data-stu-id="f0740-121">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="f0740-122">Типизированные клиенты</span><span class="sxs-lookup"><span data-stu-id="f0740-122">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="f0740-123">Созданные клиенты</span><span class="sxs-lookup"><span data-stu-id="f0740-123">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="f0740-124">Оптимальный подход зависит от требований приложения.</span><span class="sxs-lookup"><span data-stu-id="f0740-124">The best approach depends upon the app's requirements.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="f0740-125">Основное использование</span><span class="sxs-lookup"><span data-stu-id="f0740-125">Basic usage</span></span>

<span data-ttu-id="f0740-126">`IHttpClientFactory` можно зарегистрировать, вызвав `AddHttpClient`:</span><span class="sxs-lookup"><span data-stu-id="f0740-126">`IHttpClientFactory` can be registered by calling `AddHttpClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="f0740-127">`IHttpClientFactory` можно запросить с помощью [внедрения зависимостей (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="f0740-127">An `IHttpClientFactory` can be requested using [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="f0740-128">Следующий код использует `IHttpClientFactory` для создания экземпляра `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="f0740-128">The following code uses `IHttpClientFactory` to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="f0740-129">Подобное использование `IHttpClientFactory` — это хороший способ рефакторинга имеющегося приложения.</span><span class="sxs-lookup"><span data-stu-id="f0740-129">Using `IHttpClientFactory` like in the preceding example is a good way to refactor an existing app.</span></span> <span data-ttu-id="f0740-130">Он не оказывает влияния на использование `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f0740-130">It has no impact on how `HttpClient` is used.</span></span> <span data-ttu-id="f0740-131">Там, где в существующем приложении создаются экземпляры `HttpClient`, используйте вызовы к <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="f0740-131">In places where `HttpClient` instances are created in an existing app, replace those occurrences with calls to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="f0740-132">Именованные клиенты</span><span class="sxs-lookup"><span data-stu-id="f0740-132">Named clients</span></span>

<span data-ttu-id="f0740-133">Именованные клиенты являются хорошим выбором в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="f0740-133">Named clients are a good choice when:</span></span>

* <span data-ttu-id="f0740-134">Приложение требует много отдельных использований `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f0740-134">The app requires many distinct uses of `HttpClient`.</span></span>
* <span data-ttu-id="f0740-135">Многие `HttpClient` имеют другую конфигурацию.</span><span class="sxs-lookup"><span data-stu-id="f0740-135">Many `HttpClient`s have different configuration.</span></span>

<span data-ttu-id="f0740-136">Конфигурацию для именованного клиента `HttpClient` можно указать во время регистрации в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f0740-136">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="f0740-137">В приведенном выше коде клиент регистрируется с:</span><span class="sxs-lookup"><span data-stu-id="f0740-137">In the preceding code the client is configured with:</span></span>

* <span data-ttu-id="f0740-138">базовым адресом `https://api.github.com/`;</span><span class="sxs-lookup"><span data-stu-id="f0740-138">The base address `https://api.github.com/`.</span></span>
* <span data-ttu-id="f0740-139">двумя заголовками, необходимыми для работы с API GitHub.</span><span class="sxs-lookup"><span data-stu-id="f0740-139">Two headers required to work with the GitHub API.</span></span>

#### <a name="createclient"></a><span data-ttu-id="f0740-140">CreateClient</span><span class="sxs-lookup"><span data-stu-id="f0740-140">CreateClient</span></span>

<span data-ttu-id="f0740-141">При каждом вызове <xref:System.Net.Http.IHttpClientFactory.CreateClient*>:</span><span class="sxs-lookup"><span data-stu-id="f0740-141">Each time <xref:System.Net.Http.IHttpClientFactory.CreateClient*> is called:</span></span>

* <span data-ttu-id="f0740-142">создается новый экземпляр `HttpClient`;</span><span class="sxs-lookup"><span data-stu-id="f0740-142">A new instance of `HttpClient` is created.</span></span>
* <span data-ttu-id="f0740-143">вызывается действие настройки.</span><span class="sxs-lookup"><span data-stu-id="f0740-143">The configuration action is called.</span></span>

<span data-ttu-id="f0740-144">Чтобы создать именованный клиент, передайте его имя в `CreateClient`:</span><span class="sxs-lookup"><span data-stu-id="f0740-144">To create a named client, pass its name into `CreateClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="f0740-145">В приведенном выше коде в запросе не требуется указывать имя узла.</span><span class="sxs-lookup"><span data-stu-id="f0740-145">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="f0740-146">Достаточно передать только путь, так как используется базовый адрес, заданный для клиента.</span><span class="sxs-lookup"><span data-stu-id="f0740-146">The code can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="f0740-147">Типизированные клиенты</span><span class="sxs-lookup"><span data-stu-id="f0740-147">Typed clients</span></span>

<span data-ttu-id="f0740-148">Типизированные клиенты:</span><span class="sxs-lookup"><span data-stu-id="f0740-148">Typed clients:</span></span>

* <span data-ttu-id="f0740-149">предоставляют те же возможности, что и именованные клиенты, без необходимости использовать строки в качестве ключей.</span><span class="sxs-lookup"><span data-stu-id="f0740-149">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="f0740-150">Это помогает IntelliSense и компилятору при использовании клиентов.</span><span class="sxs-lookup"><span data-stu-id="f0740-150">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="f0740-151">Они предоставляют единое расположение для настройки и взаимодействия с конкретным клиентом `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f0740-151">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="f0740-152">Например, один типизированный клиент можно использовать:</span><span class="sxs-lookup"><span data-stu-id="f0740-152">For example, a single typed client might be used:</span></span>
  * <span data-ttu-id="f0740-153">для одной серверной конечной точки;</span><span class="sxs-lookup"><span data-stu-id="f0740-153">For a single backend endpoint.</span></span>
  * <span data-ttu-id="f0740-154">для инкапсуляции всей логики, связанной с конечной точкой.</span><span class="sxs-lookup"><span data-stu-id="f0740-154">To encapsulate all logic dealing with the endpoint.</span></span>
* <span data-ttu-id="f0740-155">Поддерживаются работа с внедрением зависимостей и возможность вставки в нужное место в приложении.</span><span class="sxs-lookup"><span data-stu-id="f0740-155">Work with DI and can be injected where required in the app.</span></span>

<span data-ttu-id="f0740-156">Типизированный клиент принимает параметр `HttpClient` в конструкторе:</span><span class="sxs-lookup"><span data-stu-id="f0740-156">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="f0740-157">В приведенном выше коде:</span><span class="sxs-lookup"><span data-stu-id="f0740-157">In the preceding code:</span></span>

* <span data-ttu-id="f0740-158">Конфигурация перемещается в типизированный клиент.</span><span class="sxs-lookup"><span data-stu-id="f0740-158">The configuration is moved into the typed client.</span></span>
* <span data-ttu-id="f0740-159">Объект `HttpClient` предоставляется в виде открытого свойства.</span><span class="sxs-lookup"><span data-stu-id="f0740-159">The `HttpClient` object is exposed as a public property.</span></span>

<span data-ttu-id="f0740-160">Можно создать связанные с API методы, которые предоставляют функциональные возможности `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f0740-160">API-specific methods can be created that expose `HttpClient` functionality.</span></span> <span data-ttu-id="f0740-161">Например, метод `GetAspNetDocsIssues` инкапсулирует код для получения открытых вопросов.</span><span class="sxs-lookup"><span data-stu-id="f0740-161">For example, the `GetAspNetDocsIssues` method encapsulates code to retrieve open issues.</span></span>

<span data-ttu-id="f0740-162">Следующий код вызывает <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> в `Startup.ConfigureServices` для регистрации класса типизированного клиента:</span><span class="sxs-lookup"><span data-stu-id="f0740-162">The following code calls <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` to register a typed client class:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="f0740-163">Типизированный клиент регистрируется во внедрении зависимостей как временный.</span><span class="sxs-lookup"><span data-stu-id="f0740-163">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="f0740-164">Типизированный клиент можно внедрить и использовать напрямую:</span><span class="sxs-lookup"><span data-stu-id="f0740-164">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="f0740-165">Конфигурацию для типизированного клиента можно указать во время регистрации в `Startup.ConfigureServices`, а не в конструкторе типизированного клиента:</span><span class="sxs-lookup"><span data-stu-id="f0740-165">The configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="f0740-166">`HttpClient` может быть инкапсулирован в типизированном клиенте.</span><span class="sxs-lookup"><span data-stu-id="f0740-166">The `HttpClient` can be encapsulated within a typed client.</span></span> <span data-ttu-id="f0740-167">Вместо предоставления его как свойства определите метод для внутреннего вызова экземпляра `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="f0740-167">Rather than exposing it as a property, define a method which calls the `HttpClient` instance internally:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="f0740-168">В приведенном выше коде `HttpClient` хранится в закрытом поле.</span><span class="sxs-lookup"><span data-stu-id="f0740-168">In the preceding code, the `HttpClient` is stored in a private field.</span></span> <span data-ttu-id="f0740-169">Доступ к `HttpClient` осуществляется с помощью общедоступного метода `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="f0740-169">Access to the `HttpClient` is by the public `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="f0740-170">Созданные клиенты</span><span class="sxs-lookup"><span data-stu-id="f0740-170">Generated clients</span></span>

<span data-ttu-id="f0740-171">`IHttpClientFactory` можно использовать в сочетании с библиотеками сторонних разработчиков, например [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="f0740-171">`IHttpClientFactory` can be used in combination with third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="f0740-172">Refit — это библиотека REST для .NET.</span><span class="sxs-lookup"><span data-stu-id="f0740-172">Refit is a REST library for .NET.</span></span> <span data-ttu-id="f0740-173">Она преобразует REST API в динамические интерфейсы.</span><span class="sxs-lookup"><span data-stu-id="f0740-173">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="f0740-174">Реализация интерфейса формируется динамически с помощью `RestService` с использованием `HttpClient` для совершения внешних вызовов HTTP.</span><span class="sxs-lookup"><span data-stu-id="f0740-174">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="f0740-175">Для представления внешнего API и его ответа определяются интерфейс и ответ:</span><span class="sxs-lookup"><span data-stu-id="f0740-175">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="f0740-176">Можно добавить типизированный клиент, используя Refit для создания реализации:</span><span class="sxs-lookup"><span data-stu-id="f0740-176">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("http://localhost:5000");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddControllers();
}
```

<span data-ttu-id="f0740-177">При необходимости можно использовать определенный интерфейс с реализацией, предоставленной внедрением зависимостей и Refit:</span><span class="sxs-lookup"><span data-stu-id="f0740-177">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="f0740-178">ПО промежуточного слоя для исходящих запросов</span><span class="sxs-lookup"><span data-stu-id="f0740-178">Outgoing request middleware</span></span>

<span data-ttu-id="f0740-179">В `HttpClient` существует концепция делегирования обработчиков, которые можно связать друг с другом для исходящих HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="f0740-179">`HttpClient` has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="f0740-180">`IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="f0740-180">`IHttpClientFactory`:</span></span>

* <span data-ttu-id="f0740-181">Упрощает определение обработчиков для применения к каждому именованному клиенту.</span><span class="sxs-lookup"><span data-stu-id="f0740-181">Simplifies defining the handlers to apply for each named client.</span></span>
* <span data-ttu-id="f0740-182">Поддерживает регистрацию и объединение в цепочки нескольких обработчиков для создания конвейера ПО промежуточного слоя для исходящих запросов.</span><span class="sxs-lookup"><span data-stu-id="f0740-182">Supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="f0740-183">Каждый из этих обработчиков может выполнять работу до и после исходящего запроса.</span><span class="sxs-lookup"><span data-stu-id="f0740-183">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="f0740-184">Этот шаблон:</span><span class="sxs-lookup"><span data-stu-id="f0740-184">This pattern:</span></span>

  * <span data-ttu-id="f0740-185">похож на входящий конвейер ПО промежуточного слоя в ASP.NET Core;</span><span class="sxs-lookup"><span data-stu-id="f0740-185">Is similar to the inbound middleware pipeline in ASP.NET Core.</span></span>
  * <span data-ttu-id="f0740-186">предоставляет механизм управления сквозной функциональностью HTTP-запросов, включая:</span><span class="sxs-lookup"><span data-stu-id="f0740-186">Provides a mechanism to manage cross-cutting concerns around HTTP requests, such as:</span></span>

    * <span data-ttu-id="f0740-187">кэширование</span><span class="sxs-lookup"><span data-stu-id="f0740-187">caching</span></span>
    * <span data-ttu-id="f0740-188">обработку ошибок</span><span class="sxs-lookup"><span data-stu-id="f0740-188">error handling</span></span>
    * <span data-ttu-id="f0740-189">сериализацию</span><span class="sxs-lookup"><span data-stu-id="f0740-189">serialization</span></span>
    * <span data-ttu-id="f0740-190">Ведение журналов</span><span class="sxs-lookup"><span data-stu-id="f0740-190">logging</span></span>

<span data-ttu-id="f0740-191">Чтобы создать делегированный обработчик, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="f0740-191">To create a delegating handler:</span></span>

* <span data-ttu-id="f0740-192">Создайте объект, производный от <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="f0740-192">Derive from <xref:System.Net.Http.DelegatingHandler>.</span></span>
* <span data-ttu-id="f0740-193">Переопределите метод <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span><span class="sxs-lookup"><span data-stu-id="f0740-193">Override <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span></span> <span data-ttu-id="f0740-194">Выполните код до передачи запроса следующему обработчику в конвейере:</span><span class="sxs-lookup"><span data-stu-id="f0740-194">Execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="f0740-195">Предыдущий код проверяет, находится ли заголовок `X-API-KEY` в запросе.</span><span class="sxs-lookup"><span data-stu-id="f0740-195">The preceding code checks if the `X-API-KEY` header is in the request.</span></span> <span data-ttu-id="f0740-196">Если `X-API-KEY` отсутствует, возвращается <xref:System.Net.HttpStatusCode.BadRequest>.</span><span class="sxs-lookup"><span data-stu-id="f0740-196">If `X-API-KEY` is missing, <xref:System.Net.HttpStatusCode.BadRequest> is returned.</span></span>

<span data-ttu-id="f0740-197">Можно добавить сразу несколько обработчиков в конфигурацию для `HttpClient` с использованием <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span><span class="sxs-lookup"><span data-stu-id="f0740-197">More than one handler can be added to the configuration for a `HttpClient` with <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup2.cs?name=snippet1)]

<span data-ttu-id="f0740-198">В приведенном выше коде `ValidateHeaderHandler` регистрируется с помощью внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="f0740-198">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="f0740-199">`IHttpClientFactory` создает отдельную область внедрения зависимостей для каждого обработчика.</span><span class="sxs-lookup"><span data-stu-id="f0740-199">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="f0740-200">Обработчики могут зависеть от служб из любой области.</span><span class="sxs-lookup"><span data-stu-id="f0740-200">Handlers can depend upon services of any scope.</span></span> <span data-ttu-id="f0740-201">Службы, которые зависят от обработчиков, удаляются при удалении обработчика.</span><span class="sxs-lookup"><span data-stu-id="f0740-201">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="f0740-202">После регистрации можно вызвать <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, передав тип обработчика.</span><span class="sxs-lookup"><span data-stu-id="f0740-202">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="f0740-203">Можно зарегистрировать несколько обработчиков в порядке, в котором они должны выполняться.</span><span class="sxs-lookup"><span data-stu-id="f0740-203">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="f0740-204">Каждый обработчик содержит следующий обработчик, пока последний `HttpClientHandler` не выполнит запрос:</span><span class="sxs-lookup"><span data-stu-id="f0740-204">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="f0740-205">Используйте один из следующих методов для предоставления общего доступа к состоянию отдельных запросов с помощью обработчиков сообщений:</span><span class="sxs-lookup"><span data-stu-id="f0740-205">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="f0740-206">Передайте данные в обработчик с помощью [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span><span class="sxs-lookup"><span data-stu-id="f0740-206">Pass data into the handler using [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span></span>
* <span data-ttu-id="f0740-207">Используйте <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> для доступа к текущему запросу.</span><span class="sxs-lookup"><span data-stu-id="f0740-207">Use <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> to access the current request.</span></span>
* <span data-ttu-id="f0740-208">Создайте пользовательский объект хранилища <xref:System.Threading.AsyncLocal`1> для передачи данных.</span><span class="sxs-lookup"><span data-stu-id="f0740-208">Create a custom <xref:System.Threading.AsyncLocal`1> storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="f0740-209">Использование обработчиков на основе Polly</span><span class="sxs-lookup"><span data-stu-id="f0740-209">Use Polly-based handlers</span></span>

<span data-ttu-id="f0740-210">`IHttpClientFactory` интегрируется с библиотекой сторонних разработчиков [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="f0740-210">`IHttpClientFactory` integrates with the third-party library [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="f0740-211">Polly — это комплексная библиотека, обеспечивающая отказоустойчивость и обработку временных сбоев в .NET.</span><span class="sxs-lookup"><span data-stu-id="f0740-211">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="f0740-212">Она позволяет разработчикам выражать политики, например политику повтора, размыкателя цепи, времени ожидания, изоляции отсеков и отката, более эффективным и потокобезопасным образом.</span><span class="sxs-lookup"><span data-stu-id="f0740-212">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="f0740-213">Для использования политик Polly с настроенными экземплярами `HttpClient` предоставляются методы расширения.</span><span class="sxs-lookup"><span data-stu-id="f0740-213">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="f0740-214">Расширения Polly поддерживают добавление обработчиков на основе Polly клиентам.</span><span class="sxs-lookup"><span data-stu-id="f0740-214">The Polly extensions support adding Polly-based handlers to clients.</span></span> <span data-ttu-id="f0740-215">Polly нужен пакет NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/).</span><span class="sxs-lookup"><span data-stu-id="f0740-215">Polly requires the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="f0740-216">Обработка временных сбоев</span><span class="sxs-lookup"><span data-stu-id="f0740-216">Handle transient faults</span></span>

<span data-ttu-id="f0740-217">Чаще всего ошибки происходят, когда внешние вызовы HTTP являются временными.</span><span class="sxs-lookup"><span data-stu-id="f0740-217">Faults typically occur when external HTTP calls are transient.</span></span> <span data-ttu-id="f0740-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> позволяет определить политику для обработки временных ошибок.</span><span class="sxs-lookup"><span data-stu-id="f0740-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="f0740-219">Политики, настроенные с помощью `AddTransientHttpErrorPolicy`, обрабатывают следующие ответы:</span><span class="sxs-lookup"><span data-stu-id="f0740-219">Policies configured with `AddTransientHttpErrorPolicy` handle the following responses:</span></span>

* <xref:System.Net.Http.HttpRequestException>
* <span data-ttu-id="f0740-220">HTTP 5xx</span><span class="sxs-lookup"><span data-stu-id="f0740-220">HTTP 5xx</span></span>
* <span data-ttu-id="f0740-221">HTTP 408</span><span class="sxs-lookup"><span data-stu-id="f0740-221">HTTP 408</span></span>

<span data-ttu-id="f0740-222">`AddTransientHttpErrorPolicy` предоставляет доступ к объекту `PolicyBuilder`, настроенному для обработки ошибок, представляющих возможный временный сбой:</span><span class="sxs-lookup"><span data-stu-id="f0740-222">`AddTransientHttpErrorPolicy` provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup3.cs?name=snippet1)]

<span data-ttu-id="f0740-223">В приведенном выше коде определена политика `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="f0740-223">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="f0740-224">Неудачные запросы повторяются до трех раз с задержкой 600 мс между попытками.</span><span class="sxs-lookup"><span data-stu-id="f0740-224">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="f0740-225">Динамический выбор политик</span><span class="sxs-lookup"><span data-stu-id="f0740-225">Dynamically select policies</span></span>

<span data-ttu-id="f0740-226">Предоставляются методы расширения для добавления обработчиков на основе Polly, например <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span><span class="sxs-lookup"><span data-stu-id="f0740-226">Extension methods are provided to add Polly-based handlers, for example, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span></span> <span data-ttu-id="f0740-227">Следующая перегрузка `AddPolicyHandler` проверяет запрос для определения применимой политики:</span><span class="sxs-lookup"><span data-stu-id="f0740-227">The following `AddPolicyHandler` overload inspects the request to decide which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="f0740-228">Если в приведенном выше коде исходящий запрос является запросом HTTP GET, применяется время ожидания 10 секунд.</span><span class="sxs-lookup"><span data-stu-id="f0740-228">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="f0740-229">Для остальных методов HTTP время ожидания — 30 секунд.</span><span class="sxs-lookup"><span data-stu-id="f0740-229">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="f0740-230">Добавление нескольких обработчиков Polly</span><span class="sxs-lookup"><span data-stu-id="f0740-230">Add multiple Polly handlers</span></span>

<span data-ttu-id="f0740-231">Общепринятой практикой является вложение политик Polly:</span><span class="sxs-lookup"><span data-stu-id="f0740-231">It's common to nest Polly policies:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="f0740-232">В предшествующем примере:</span><span class="sxs-lookup"><span data-stu-id="f0740-232">In the preceding example:</span></span>

* <span data-ttu-id="f0740-233">Добавляются два обработчика.</span><span class="sxs-lookup"><span data-stu-id="f0740-233">Two handlers are added.</span></span>
* <span data-ttu-id="f0740-234">Первый обработчик использует <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*>, чтобы добавить политику повтора.</span><span class="sxs-lookup"><span data-stu-id="f0740-234">The first handler uses <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> to add a retry policy.</span></span> <span data-ttu-id="f0740-235">Неудачные запросы выполняются повторно до трех раз.</span><span class="sxs-lookup"><span data-stu-id="f0740-235">Failed requests are retried up to three times.</span></span>
* <span data-ttu-id="f0740-236">Второй вызов `AddTransientHttpErrorPolicy` добавляет политику размыкателя цепи.</span><span class="sxs-lookup"><span data-stu-id="f0740-236">The second `AddTransientHttpErrorPolicy` call adds a circuit breaker policy.</span></span> <span data-ttu-id="f0740-237">Дополнительные внешние запросы блокируются в течение 30 секунд в случае пяти неудачных попыток подряд.</span><span class="sxs-lookup"><span data-stu-id="f0740-237">Further external requests are blocked for 30 seconds if 5 failed attempts occur sequentially.</span></span> <span data-ttu-id="f0740-238">Политики размыкателя цепи отслеживают состояние.</span><span class="sxs-lookup"><span data-stu-id="f0740-238">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="f0740-239">Все вызовы через этот клиент имеют одинаковое состояние цепи.</span><span class="sxs-lookup"><span data-stu-id="f0740-239">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="f0740-240">Добавление политик из реестра Polly</span><span class="sxs-lookup"><span data-stu-id="f0740-240">Add policies from the Polly registry</span></span>

<span data-ttu-id="f0740-241">Подход к управлению регулярно используемыми политиками заключается в их однократном определении и регистрации с помощью `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="f0740-241">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span>

<span data-ttu-id="f0740-242">В приведенном ниже коде выполняется следующее:</span><span class="sxs-lookup"><span data-stu-id="f0740-242">In the following code:</span></span>

* <span data-ttu-id="f0740-243">Добавляются политики regular и long.</span><span class="sxs-lookup"><span data-stu-id="f0740-243">The "regular" and "long" polices are added.</span></span>
* <span data-ttu-id="f0740-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*> добавляет политики regular и long из реестра.</span><span class="sxs-lookup"><span data-stu-id="f0740-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>  adds the "regular" and "long" policies from the registry.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup4.cs?name=snippet1)]

<span data-ttu-id="f0740-245">Дополнительные сведения о `IHttpClientFactory` и интеграции Polly см. на [вики-сайте Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="f0740-245">For more information on `IHttpClientFactory` and Polly integrations, see the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="f0740-246">Управление HttpClient и временем существования</span><span class="sxs-lookup"><span data-stu-id="f0740-246">HttpClient and lifetime management</span></span>

<span data-ttu-id="f0740-247">При каждом вызове `CreateClient` в `IHttpClientFactory` возвращается новый экземпляр `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f0740-247">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="f0740-248"><xref:System.Net.Http.HttpMessageHandler> создается для каждого именованного клиента.</span><span class="sxs-lookup"><span data-stu-id="f0740-248">An <xref:System.Net.Http.HttpMessageHandler> is created per named client.</span></span> <span data-ttu-id="f0740-249">Фабрика обеспечивает управление временем существования экземпляров `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="f0740-249">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="f0740-250">`IHttpClientFactory` объединяет в пул все экземпляры `HttpMessageHandler`, созданные фабрикой, чтобы уменьшить потребление ресурсов.</span><span class="sxs-lookup"><span data-stu-id="f0740-250">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="f0740-251">Экземпляр `HttpMessageHandler` можно использовать повторно из пула при создании экземпляра `HttpClient`, если его время существования еще не истекло.</span><span class="sxs-lookup"><span data-stu-id="f0740-251">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="f0740-252">Создавать пулы обработчиков желательно, так как каждый обработчик обычно управляет собственными базовыми HTTP-подключениями.</span><span class="sxs-lookup"><span data-stu-id="f0740-252">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="f0740-253">Создание лишних обработчиков может привести к задержке подключения.</span><span class="sxs-lookup"><span data-stu-id="f0740-253">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="f0740-254">Некоторые обработчики поддерживают подключения открытыми в течение неопределенного периода, что может помешать обработчику отреагировать на изменения службы доменных имен (DNS).</span><span class="sxs-lookup"><span data-stu-id="f0740-254">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS (Domain Name System) changes.</span></span>

<span data-ttu-id="f0740-255">Время существования обработчика по умолчанию — две минуты.</span><span class="sxs-lookup"><span data-stu-id="f0740-255">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="f0740-256">Значение по умолчанию можно переопределить для каждого именованного клиента:</span><span class="sxs-lookup"><span data-stu-id="f0740-256">The default value can be overridden on a per named client basis:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup5.cs?name=snippet1)]

<span data-ttu-id="f0740-257">Экземпляры `HttpClient` обычно можно рассматривать как объекты .NET, **не** требующие освобождения.</span><span class="sxs-lookup"><span data-stu-id="f0740-257">`HttpClient` instances can generally be treated as .NET objects **not** requiring disposal.</span></span> <span data-ttu-id="f0740-258">Высвобождение отменяет исходящие запросы и гарантирует, что указанный экземпляр `HttpClient` не может использоваться после вызова <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="f0740-258">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="f0740-259">`IHttpClientFactory` отслеживает и высвобождает ресурсы, используемые экземплярами `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f0740-259">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span>

<span data-ttu-id="f0740-260">До появления `IHttpClientFactory` один экземпляр `HttpClient` часто сохраняли в активном состоянии в течение длительного времени.</span><span class="sxs-lookup"><span data-stu-id="f0740-260">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="f0740-261">После перехода на `IHttpClientFactory` это уже не нужно.</span><span class="sxs-lookup"><span data-stu-id="f0740-261">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

## <a name="logging"></a><span data-ttu-id="f0740-262">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="f0740-262">Logging</span></span>

<span data-ttu-id="f0740-263">Клиенты, созданные через `IHttpClientFactory`, записывают сообщения журнала для всех запросов.</span><span class="sxs-lookup"><span data-stu-id="f0740-263">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="f0740-264">Установите соответствующий уровень информации в конфигурации ведения журнала, чтобы просматривать сообщения журнала по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f0740-264">Enable the appropriate information level in the logging configuration to see the default log messages.</span></span> <span data-ttu-id="f0740-265">Дополнительное ведение журнала, например запись заголовков запросов, включено только на уровне трассировки.</span><span class="sxs-lookup"><span data-stu-id="f0740-265">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="f0740-266">Категория журнала для каждого клиента включает в себя имя клиента.</span><span class="sxs-lookup"><span data-stu-id="f0740-266">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="f0740-267">Клиент с именем *MyNamedClient*, например, записывает в журнал сообщения с категорией "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span><span class="sxs-lookup"><span data-stu-id="f0740-267">A client named *MyNamedClient*, for example, logs messages with a category of "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span></span> <span data-ttu-id="f0740-268">Сообщения с суффиксом *LogicalHandler* создаются за пределами конвейера обработчиков запросов.</span><span class="sxs-lookup"><span data-stu-id="f0740-268">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="f0740-269">Во время запроса сообщения записываются в журнал до обработки запроса другими обработчиками в конвейере.</span><span class="sxs-lookup"><span data-stu-id="f0740-269">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="f0740-270">Во время ответа сообщения записываются в журнал после получения ответа другими обработчиками в конвейере.</span><span class="sxs-lookup"><span data-stu-id="f0740-270">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="f0740-271">Кроме того, журнал ведется в конвейере обработчиков запросов.</span><span class="sxs-lookup"><span data-stu-id="f0740-271">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="f0740-272">В примере *MyNamedClient* эти сообщения регистрируются с категорией журнала "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span><span class="sxs-lookup"><span data-stu-id="f0740-272">In the *MyNamedClient* example, those messages are logged with the log category "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span></span> <span data-ttu-id="f0740-273">Во время запроса это происходит после выполнения всех обработчиков и непосредственно перед отправкой запроса.</span><span class="sxs-lookup"><span data-stu-id="f0740-273">For the request, this occurs after all other handlers have run and immediately before the request is sent.</span></span> <span data-ttu-id="f0740-274">Во время ответа в журнале записывается состояние ответа перед его передачей обратно по конвейеру обработчиков.</span><span class="sxs-lookup"><span data-stu-id="f0740-274">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="f0740-275">Включив ведение журнала в конвейере и за его пределами, можно выполнять проверку изменений, внесенных другими обработчиками конвейера.</span><span class="sxs-lookup"><span data-stu-id="f0740-275">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="f0740-276">Сюда входят изменения заголовков запросов или кода состояния ответов.</span><span class="sxs-lookup"><span data-stu-id="f0740-276">This may include changes to request headers or to the response status code.</span></span>

<span data-ttu-id="f0740-277">Включение имени клиента в категорию журнала позволяет фильтровать журналы по именованным клиентам.</span><span class="sxs-lookup"><span data-stu-id="f0740-277">Including the name of the client in the log category enables log filtering for specific named clients.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="f0740-278">Настройка HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="f0740-278">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="f0740-279">Иногда необходимо контролировать конфигурацию внутреннего обработчика `HttpMessageHandler`, используемого клиентом.</span><span class="sxs-lookup"><span data-stu-id="f0740-279">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="f0740-280">При добавлении именованного или типизированного клиента возвращается `IHttpClientBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f0740-280">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="f0740-281">Для определения делегата можно использовать метод расширения <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>.</span><span class="sxs-lookup"><span data-stu-id="f0740-281">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="f0740-282">Делегат используется для создания и настройки основного обработчика `HttpMessageHandler`, используемого этим клиентом:</span><span class="sxs-lookup"><span data-stu-id="f0740-282">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup6.cs?name=snippet1)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="f0740-283">Использование IHttpClientFactory в консольном приложении</span><span class="sxs-lookup"><span data-stu-id="f0740-283">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="f0740-284">В консольном приложении добавьте в проект следующие ссылки на пакеты:</span><span class="sxs-lookup"><span data-stu-id="f0740-284">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="f0740-285">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="f0740-285">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="f0740-286">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="f0740-286">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="f0740-287">В следующем примере:</span><span class="sxs-lookup"><span data-stu-id="f0740-287">In the following example:</span></span>

* <span data-ttu-id="f0740-288"><xref:System.Net.Http.IHttpClientFactory> регистрируется в контейнере службы [универсального узла](xref:fundamentals/host/generic-host):</span><span class="sxs-lookup"><span data-stu-id="f0740-288"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="f0740-289">`MyService` создает экземпляр фабрики клиента из службы, который используется для создания `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f0740-289">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="f0740-290">`HttpClient` используется для получения веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="f0740-290">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="f0740-291">`Main` создает область для выполнения метода `GetPage` службы и вывода первых 500 символов содержимого веб-страницы на консоль.</span><span class="sxs-lookup"><span data-stu-id="f0740-291">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="f0740-292">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="f0740-292">Additional resources</span></span>

* [<span data-ttu-id="f0740-293">Использование HttpClientFactory для реализации устойчивых HTTP-запросов</span><span class="sxs-lookup"><span data-stu-id="f0740-293">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="f0740-294">Реализация повторных попыток вызова HTTP с экспоненциальной задержкой с помощью HttpClientFactory и политик Polly</span><span class="sxs-lookup"><span data-stu-id="f0740-294">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="f0740-295">Реализация шаблона размыкателя цепи</span><span class="sxs-lookup"><span data-stu-id="f0740-295">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="f0740-296">Авторы: [Гленн Кондрон (Glenn Condron)](https://github.com/glennc), [Райан Новак (Ryan Nowak)](https://github.com/rynowak) и [Стив Гордон (Steve Gordon)](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="f0740-296">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="f0740-297"><xref:System.Net.Http.IHttpClientFactory> можно зарегистрировать и использовать для настройки и создания экземпляров <xref:System.Net.Http.HttpClient> в приложении.</span><span class="sxs-lookup"><span data-stu-id="f0740-297">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="f0740-298">Так вы получите следующие преимущества:</span><span class="sxs-lookup"><span data-stu-id="f0740-298">It offers the following benefits:</span></span>

* <span data-ttu-id="f0740-299">Центральное расположение для именования и настройки логических экземпляров `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f0740-299">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="f0740-300">Например, можно зарегистрировать и настроить клиент *github* для доступа к [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="f0740-300">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="f0740-301">Можно зарегистрировать клиент по умолчанию для других целей.</span><span class="sxs-lookup"><span data-stu-id="f0740-301">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="f0740-302">Кодификация концепции исходящего ПО промежуточного слоя путем делегирования обработчиков в `HttpClient` и предоставление расширений для ПО промежуточного слоя на основе Polly для использования этой возможности.</span><span class="sxs-lookup"><span data-stu-id="f0740-302">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="f0740-303">Управление созданием пулов и временем существования базовых экземпляров `HttpClientMessageHandler` с целью избежать обычных проблем с DNS, которые возникают при управлении временем существования `HttpClient` вручную.</span><span class="sxs-lookup"><span data-stu-id="f0740-303">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="f0740-304">Настройка параметров ведения журнала (через `ILogger`) для всех запросов, отправленных через клиентов, созданных фабрикой.</span><span class="sxs-lookup"><span data-stu-id="f0740-304">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="f0740-305">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f0740-305">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="f0740-306">Принципы использования</span><span class="sxs-lookup"><span data-stu-id="f0740-306">Consumption patterns</span></span>

<span data-ttu-id="f0740-307">Существует несколько способов использования `IHttpClientFactory` в приложении:</span><span class="sxs-lookup"><span data-stu-id="f0740-307">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="f0740-308">Основное использование</span><span class="sxs-lookup"><span data-stu-id="f0740-308">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="f0740-309">Именованные клиенты</span><span class="sxs-lookup"><span data-stu-id="f0740-309">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="f0740-310">Типизированные клиенты</span><span class="sxs-lookup"><span data-stu-id="f0740-310">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="f0740-311">Созданные клиенты</span><span class="sxs-lookup"><span data-stu-id="f0740-311">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="f0740-312">Все способы равноценны.</span><span class="sxs-lookup"><span data-stu-id="f0740-312">None of them are strictly superior to another.</span></span> <span data-ttu-id="f0740-313">Оптимальный подход зависит от ограничений приложения.</span><span class="sxs-lookup"><span data-stu-id="f0740-313">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="f0740-314">Основное использование</span><span class="sxs-lookup"><span data-stu-id="f0740-314">Basic usage</span></span>

<span data-ttu-id="f0740-315">`IHttpClientFactory` можно зарегистрировать путем вызова метода расширения `AddHttpClient` в `IServiceCollection` внутри метода `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f0740-315">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="f0740-316">После регистрации код может принимать `IHttpClientFactory` в любом месте, куда можно внедрить службу с помощью [внедрения зависимостей](xref:fundamentals/dependency-injection) (DI).</span><span class="sxs-lookup"><span data-stu-id="f0740-316">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="f0740-317">`IHttpClientFactory` можно использовать для создания экземпляра `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="f0740-317">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="f0740-318">Подобное использование `IHttpClientFactory` — это отличный способ рефакторинга имеющегося приложения.</span><span class="sxs-lookup"><span data-stu-id="f0740-318">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="f0740-319">Он не оказывает влияния на использование `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f0740-319">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="f0740-320">Там, где в данный момент создаются экземпляры `HttpClient`, используйте вызов к <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="f0740-320">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="f0740-321">Именованные клиенты</span><span class="sxs-lookup"><span data-stu-id="f0740-321">Named clients</span></span>

<span data-ttu-id="f0740-322">Если для приложения предполагаются разные способы использования `HttpClient`, каждый со своей конфигурацией, можно применять **именованные клиенты**.</span><span class="sxs-lookup"><span data-stu-id="f0740-322">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="f0740-323">Конфигурацию для именованного клиента `HttpClient` можно указать во время регистрации в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f0740-323">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="f0740-324">В приведенном выше коде вызывается клиент `AddHttpClient`, предоставляющий имя *github*.</span><span class="sxs-lookup"><span data-stu-id="f0740-324">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="f0740-325">У клиента есть некоторые настройки по умолчанию &mdash; а именно: базовый адрес и два заголовка, необходимые для работы с API GitHub.</span><span class="sxs-lookup"><span data-stu-id="f0740-325">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="f0740-326">При каждом вызове `CreateClient` создается новый экземпляр `HttpClient` и вызывается действие конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f0740-326">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="f0740-327">Для использования именованного клиента можно передать строковый параметр в `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="f0740-327">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="f0740-328">Укажите имя создаваемого клиента:</span><span class="sxs-lookup"><span data-stu-id="f0740-328">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="f0740-329">В приведенном выше коде в запросе не требуется указывать имя узла.</span><span class="sxs-lookup"><span data-stu-id="f0740-329">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="f0740-330">Достаточно передать только путь, так как используется базовый адрес, заданный для клиента.</span><span class="sxs-lookup"><span data-stu-id="f0740-330">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="f0740-331">Типизированные клиенты</span><span class="sxs-lookup"><span data-stu-id="f0740-331">Typed clients</span></span>

<span data-ttu-id="f0740-332">Типизированные клиенты:</span><span class="sxs-lookup"><span data-stu-id="f0740-332">Typed clients:</span></span>

* <span data-ttu-id="f0740-333">предоставляют те же возможности, что и именованные клиенты, без необходимости использовать строки в качестве ключей.</span><span class="sxs-lookup"><span data-stu-id="f0740-333">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="f0740-334">Это помогает IntelliSense и компилятору при использовании клиентов.</span><span class="sxs-lookup"><span data-stu-id="f0740-334">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="f0740-335">Они предоставляют единое расположение для настройки и взаимодействия с конкретным клиентом `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f0740-335">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="f0740-336">Например, для конечной точки серверной части можно использовать один типизированный клиент, который будет содержать всю логику работы с этой конечной точкой.</span><span class="sxs-lookup"><span data-stu-id="f0740-336">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="f0740-337">Поддерживаются работа с внедрением зависимостей и возможность вставки в нужное место в приложении.</span><span class="sxs-lookup"><span data-stu-id="f0740-337">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="f0740-338">Типизированный клиент принимает параметр `HttpClient` в конструкторе:</span><span class="sxs-lookup"><span data-stu-id="f0740-338">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="f0740-339">В приведенном выше коде конфигурация перемещается в типизированный клиент.</span><span class="sxs-lookup"><span data-stu-id="f0740-339">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="f0740-340">Объект `HttpClient` предоставляется в виде открытого свойства.</span><span class="sxs-lookup"><span data-stu-id="f0740-340">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="f0740-341">Можно определить связанные с API методы, которые предоставляют функциональные возможности `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f0740-341">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="f0740-342">Метод `GetAspNetDocsIssues` инкапсулирует код, необходимый для запроса и анализа последнего открытого выпуска из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="f0740-342">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="f0740-343">Для регистрации типизированного клиента можно использовать универсальный метод расширения <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> в `Startup.ConfigureServices`, указав класс типизированного клиента:</span><span class="sxs-lookup"><span data-stu-id="f0740-343">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="f0740-344">Типизированный клиент регистрируется во внедрении зависимостей как временный.</span><span class="sxs-lookup"><span data-stu-id="f0740-344">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="f0740-345">Типизированный клиент можно внедрить и использовать напрямую:</span><span class="sxs-lookup"><span data-stu-id="f0740-345">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="f0740-346">При желании конфигурацию для типизированного клиента можно указать во время регистрации в `Startup.ConfigureServices`, а не в конструкторе типизированного клиента:</span><span class="sxs-lookup"><span data-stu-id="f0740-346">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="f0740-347">Можно полностью инкапсулировать `HttpClient` внутри типизированного клиента.</span><span class="sxs-lookup"><span data-stu-id="f0740-347">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="f0740-348">Вместо предоставления его как свойства можно использовать открытые методы для внутреннего вызова экземпляра `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f0740-348">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="f0740-349">В приведенном выше коде `HttpClient` хранится как закрытое поле.</span><span class="sxs-lookup"><span data-stu-id="f0740-349">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="f0740-350">Любой доступ для совершения внешних вызовов осуществляется через метод `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="f0740-350">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="f0740-351">Созданные клиенты</span><span class="sxs-lookup"><span data-stu-id="f0740-351">Generated clients</span></span>

<span data-ttu-id="f0740-352">`IHttpClientFactory` можно использовать в сочетании с другими библиотеками сторонних разработчиков, например [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="f0740-352">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="f0740-353">Refit — это библиотека REST для .NET.</span><span class="sxs-lookup"><span data-stu-id="f0740-353">Refit is a REST library for .NET.</span></span> <span data-ttu-id="f0740-354">Она преобразует REST API в динамические интерфейсы.</span><span class="sxs-lookup"><span data-stu-id="f0740-354">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="f0740-355">Реализация интерфейса формируется динамически с помощью `RestService` с использованием `HttpClient` для совершения внешних вызовов HTTP.</span><span class="sxs-lookup"><span data-stu-id="f0740-355">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="f0740-356">Для представления внешнего API и его ответа определяются интерфейс и ответ:</span><span class="sxs-lookup"><span data-stu-id="f0740-356">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="f0740-357">Можно добавить типизированный клиент, используя Refit для создания реализации:</span><span class="sxs-lookup"><span data-stu-id="f0740-357">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("https://localhost:5001");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddMvc();
}
```

<span data-ttu-id="f0740-358">При необходимости можно использовать определенный интерфейс с реализацией, предоставленной внедрением зависимостей и Refit:</span><span class="sxs-lookup"><span data-stu-id="f0740-358">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="f0740-359">ПО промежуточного слоя для исходящих запросов</span><span class="sxs-lookup"><span data-stu-id="f0740-359">Outgoing request middleware</span></span>

<span data-ttu-id="f0740-360">В `HttpClient` уже существует концепция делегирования обработчиков, которые можно связать друг с другом для исходящих HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="f0740-360">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="f0740-361">Класс `IHttpClientFactory` упрощает определение обработчиков для применения к каждому именованному клиенту.</span><span class="sxs-lookup"><span data-stu-id="f0740-361">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="f0740-362">Он поддерживает регистрацию и объединение в цепочки нескольких обработчиков для создания конвейера ПО промежуточного слоя для исходящих запросов.</span><span class="sxs-lookup"><span data-stu-id="f0740-362">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="f0740-363">Каждый из этих обработчиков может выполнять работу до и после исходящего запроса.</span><span class="sxs-lookup"><span data-stu-id="f0740-363">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="f0740-364">Этот шаблон похож на входящий конвейер ПО промежуточного слоя в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f0740-364">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="f0740-365">Шаблон предоставляет механизм управления сквозной функциональностью HTTP-запросов, включая кэширование, обработку ошибок, сериализацию и ведение журнала.</span><span class="sxs-lookup"><span data-stu-id="f0740-365">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="f0740-366">Чтобы создать обработчик, необходимо определить класс, производный от <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="f0740-366">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="f0740-367">Переопределите метод `SendAsync` для выполнения кода до передачи запросов следующему обработчику в конвейере:</span><span class="sxs-lookup"><span data-stu-id="f0740-367">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="f0740-368">В предыдущем коде определяется базовый обработчик.</span><span class="sxs-lookup"><span data-stu-id="f0740-368">The preceding code defines a basic handler.</span></span> <span data-ttu-id="f0740-369">Он проверяет, включен ли в запрос заголовок `X-API-KEY`.</span><span class="sxs-lookup"><span data-stu-id="f0740-369">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="f0740-370">Если заголовок отсутствует, он может избежать вызовов HTTP и вернуть подходящий ответ.</span><span class="sxs-lookup"><span data-stu-id="f0740-370">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="f0740-371">Во время регистрации можно добавить один или несколько обработчиков в конфигурацию для `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f0740-371">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="f0740-372">Эта задача выполняется через методы расширения в <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="f0740-372">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="f0740-373">В приведенном выше коде `ValidateHeaderHandler` регистрируется с помощью внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="f0740-373">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="f0740-374">`IHttpClientFactory` создает отдельную область внедрения зависимостей для каждого обработчика.</span><span class="sxs-lookup"><span data-stu-id="f0740-374">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="f0740-375">Обработчики могут зависеть от служб из любой области.</span><span class="sxs-lookup"><span data-stu-id="f0740-375">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="f0740-376">Службы, которые зависят от обработчиков, удаляются при удалении обработчика.</span><span class="sxs-lookup"><span data-stu-id="f0740-376">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="f0740-377">После регистрации можно вызвать <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, передав тип обработчика.</span><span class="sxs-lookup"><span data-stu-id="f0740-377">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="f0740-378">Можно зарегистрировать несколько обработчиков в порядке, в котором они должны выполняться.</span><span class="sxs-lookup"><span data-stu-id="f0740-378">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="f0740-379">Каждый обработчик содержит следующий обработчик, пока последний `HttpClientHandler` не выполнит запрос:</span><span class="sxs-lookup"><span data-stu-id="f0740-379">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="f0740-380">Используйте один из следующих методов для предоставления общего доступа к состоянию отдельных запросов с помощью обработчиков сообщений:</span><span class="sxs-lookup"><span data-stu-id="f0740-380">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="f0740-381">Передайте данные в обработчик с помощью `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="f0740-381">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="f0740-382">Используйте `IHttpContextAccessor` для доступа к текущему запросу.</span><span class="sxs-lookup"><span data-stu-id="f0740-382">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="f0740-383">Создайте пользовательский объект хранилища `AsyncLocal` для передачи данных.</span><span class="sxs-lookup"><span data-stu-id="f0740-383">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="f0740-384">Использование обработчиков на основе Polly</span><span class="sxs-lookup"><span data-stu-id="f0740-384">Use Polly-based handlers</span></span>

<span data-ttu-id="f0740-385">`IHttpClientFactory` интегрируется с популярной библиотекой сторонних разработчиков под названием [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="f0740-385">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="f0740-386">Polly — это комплексная библиотека, обеспечивающая отказоустойчивость и обработку временных сбоев в .NET.</span><span class="sxs-lookup"><span data-stu-id="f0740-386">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="f0740-387">Она позволяет разработчикам выражать политики, например политику повтора, размыкателя цепи, времени ожидания, изоляции отсеков и отката, более эффективным и потокобезопасным образом.</span><span class="sxs-lookup"><span data-stu-id="f0740-387">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="f0740-388">Для использования политик Polly с настроенными экземплярами `HttpClient` предоставляются методы расширения.</span><span class="sxs-lookup"><span data-stu-id="f0740-388">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="f0740-389">Расширения Polly:</span><span class="sxs-lookup"><span data-stu-id="f0740-389">The Polly extensions:</span></span>

* <span data-ttu-id="f0740-390">Поддерживает добавление обработчиков на основе Polly клиентам.</span><span class="sxs-lookup"><span data-stu-id="f0740-390">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="f0740-391">Можно использовать после установки пакета NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/).</span><span class="sxs-lookup"><span data-stu-id="f0740-391">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="f0740-392">Пакет не включен в общую платформу ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f0740-392">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="f0740-393">Обработка временных сбоев</span><span class="sxs-lookup"><span data-stu-id="f0740-393">Handle transient faults</span></span>

<span data-ttu-id="f0740-394">Чаще всего ошибки происходят, когда внешние вызовы HTTP являются временными.</span><span class="sxs-lookup"><span data-stu-id="f0740-394">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="f0740-395">Используется удобный метод расширения `AddTransientHttpErrorPolicy`, который позволяет определить политику для обработки временных ошибок.</span><span class="sxs-lookup"><span data-stu-id="f0740-395">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="f0740-396">Политики, заданные с помощью этого метода расширения, обрабатывают `HttpRequestException`, ответы HTTP 5xx и ответы HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="f0740-396">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="f0740-397">Расширение `AddTransientHttpErrorPolicy` может быть использовано в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f0740-397">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="f0740-398">Данное расширение предоставляет доступ к объекту `PolicyBuilder`, настроенному для обработки ошибок, представляющих возможный временный сбой:</span><span class="sxs-lookup"><span data-stu-id="f0740-398">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="f0740-399">В приведенном выше коде определена политика `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="f0740-399">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="f0740-400">Неудачные запросы повторяются до трех раз с задержкой 600 мс между попытками.</span><span class="sxs-lookup"><span data-stu-id="f0740-400">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="f0740-401">Динамический выбор политик</span><span class="sxs-lookup"><span data-stu-id="f0740-401">Dynamically select policies</span></span>

<span data-ttu-id="f0740-402">Существуют дополнительные методы расширения, которые можно использовать для добавления обработчиков на основе Polly.</span><span class="sxs-lookup"><span data-stu-id="f0740-402">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="f0740-403">Одним из таких расширений является `AddPolicyHandler` с несколькими перегрузками.</span><span class="sxs-lookup"><span data-stu-id="f0740-403">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="f0740-404">Одна перегрузка разрешает проверку запроса для определения необходимой политики:</span><span class="sxs-lookup"><span data-stu-id="f0740-404">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="f0740-405">Если в приведенном выше коде исходящий запрос является запросом HTTP GET, применяется время ожидания 10 секунд.</span><span class="sxs-lookup"><span data-stu-id="f0740-405">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="f0740-406">Для остальных методов HTTP время ожидания — 30 секунд.</span><span class="sxs-lookup"><span data-stu-id="f0740-406">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="f0740-407">Добавление нескольких обработчиков Polly</span><span class="sxs-lookup"><span data-stu-id="f0740-407">Add multiple Polly handlers</span></span>

<span data-ttu-id="f0740-408">Общепринятой практикой является вложение политик Polly для предоставления расширенной функциональности:</span><span class="sxs-lookup"><span data-stu-id="f0740-408">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="f0740-409">В приведенном выше примере добавляются два обработчика.</span><span class="sxs-lookup"><span data-stu-id="f0740-409">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="f0740-410">Первый использует расширение `AddTransientHttpErrorPolicy`, чтобы добавить политику повтора.</span><span class="sxs-lookup"><span data-stu-id="f0740-410">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="f0740-411">Неудачные запросы выполняются повторно до трех раз.</span><span class="sxs-lookup"><span data-stu-id="f0740-411">Failed requests are retried up to three times.</span></span> <span data-ttu-id="f0740-412">Второй вызов к `AddTransientHttpErrorPolicy` добавляет политику размыкателя цепи.</span><span class="sxs-lookup"><span data-stu-id="f0740-412">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="f0740-413">Дополнительные внешние запросы блокируются в течение 30 секунд в случае пяти неудачных попыток подряд.</span><span class="sxs-lookup"><span data-stu-id="f0740-413">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="f0740-414">Политики размыкателя цепи отслеживают состояние.</span><span class="sxs-lookup"><span data-stu-id="f0740-414">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="f0740-415">Все вызовы через этот клиент имеют одинаковое состояние цепи.</span><span class="sxs-lookup"><span data-stu-id="f0740-415">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="f0740-416">Добавление политик из реестра Polly</span><span class="sxs-lookup"><span data-stu-id="f0740-416">Add policies from the Polly registry</span></span>

<span data-ttu-id="f0740-417">Подход к управлению регулярно используемыми политиками заключается в их однократном определении и регистрации с помощью `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="f0740-417">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="f0740-418">Предоставляется метод расширения, разрешающий добавление обработчика с помощью политики из реестра:</span><span class="sxs-lookup"><span data-stu-id="f0740-418">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="f0740-419">В приведенном выше коде, когда `PolicyRegistry` добавляется в `ServiceCollection`, регистрируются две политики.</span><span class="sxs-lookup"><span data-stu-id="f0740-419">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="f0740-420">Чтобы использовать политику из реестра, применяется метод `AddPolicyHandlerFromRegistry`, который передает имя необходимой политики.</span><span class="sxs-lookup"><span data-stu-id="f0740-420">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="f0740-421">Дополнительные сведения об интеграции `IHttpClientFactory` и Polly см. на [вики-сайте Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="f0740-421">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="f0740-422">Управление HttpClient и временем существования</span><span class="sxs-lookup"><span data-stu-id="f0740-422">HttpClient and lifetime management</span></span>

<span data-ttu-id="f0740-423">При каждом вызове `CreateClient` в `IHttpClientFactory` возвращается новый экземпляр `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f0740-423">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="f0740-424">Для каждого названного клиента существует <xref:System.Net.Http.HttpMessageHandler>.</span><span class="sxs-lookup"><span data-stu-id="f0740-424">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="f0740-425">Фабрика обеспечивает управление временем существования экземпляров `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="f0740-425">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="f0740-426">`IHttpClientFactory` объединяет в пул все экземпляры `HttpMessageHandler`, созданные фабрикой, чтобы уменьшить потребление ресурсов.</span><span class="sxs-lookup"><span data-stu-id="f0740-426">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="f0740-427">Экземпляр `HttpMessageHandler` можно использовать повторно из пула при создании экземпляра `HttpClient`, если его время существования еще не истекло.</span><span class="sxs-lookup"><span data-stu-id="f0740-427">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="f0740-428">Создавать пулы обработчиков желательно, так как каждый обработчик обычно управляет собственными базовыми HTTP-подключениями.</span><span class="sxs-lookup"><span data-stu-id="f0740-428">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="f0740-429">Создание лишних обработчиков может привести к задержке подключения.</span><span class="sxs-lookup"><span data-stu-id="f0740-429">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="f0740-430">Некоторые обработчики поддерживают подключения открытыми в течение неопределенного периода, что может помешать обработчику отреагировать на изменения DNS.</span><span class="sxs-lookup"><span data-stu-id="f0740-430">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="f0740-431">Время существования обработчика по умолчанию — две минуты.</span><span class="sxs-lookup"><span data-stu-id="f0740-431">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="f0740-432">Значение по умолчанию можно переопределить для каждого именованного клиента.</span><span class="sxs-lookup"><span data-stu-id="f0740-432">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="f0740-433">Чтобы переопределить это значение, вызовите <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> в `IHttpClientBuilder`, который возвращается при создании клиента:</span><span class="sxs-lookup"><span data-stu-id="f0740-433">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="f0740-434">Высвобождать клиент не требуется.</span><span class="sxs-lookup"><span data-stu-id="f0740-434">Disposal of the client isn't required.</span></span> <span data-ttu-id="f0740-435">Высвобождение отменяет исходящие запросы и гарантирует, что указанный экземпляр `HttpClient` не может использоваться после вызова <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="f0740-435">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="f0740-436">`IHttpClientFactory` отслеживает и высвобождает ресурсы, используемые экземплярами `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f0740-436">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="f0740-437">Экземпляры `HttpClient` обычно можно рассматривать как объекты .NET, не требующие высвобождения.</span><span class="sxs-lookup"><span data-stu-id="f0740-437">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="f0740-438">До появления `IHttpClientFactory` один экземпляр `HttpClient` часто сохраняли в активном состоянии в течение длительного времени.</span><span class="sxs-lookup"><span data-stu-id="f0740-438">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="f0740-439">После перехода на `IHttpClientFactory` это уже не нужно.</span><span class="sxs-lookup"><span data-stu-id="f0740-439">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

## <a name="logging"></a><span data-ttu-id="f0740-440">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="f0740-440">Logging</span></span>

<span data-ttu-id="f0740-441">Клиенты, созданные через `IHttpClientFactory`, записывают сообщения журнала для всех запросов.</span><span class="sxs-lookup"><span data-stu-id="f0740-441">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="f0740-442">Установите соответствующий уровень информации в конфигурации ведения журнала, чтобы просматривать сообщения журнала по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f0740-442">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="f0740-443">Дополнительное ведение журнала, например запись заголовков запросов, включено только на уровне трассировки.</span><span class="sxs-lookup"><span data-stu-id="f0740-443">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="f0740-444">Категория журнала для каждого клиента включает в себя имя клиента.</span><span class="sxs-lookup"><span data-stu-id="f0740-444">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="f0740-445">Клиент с именем *MyNamedClient*, например, записывает в журнал сообщения с категорией `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="f0740-445">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="f0740-446">Сообщения с суффиксом *LogicalHandler* создаются за пределами конвейера обработчиков запросов.</span><span class="sxs-lookup"><span data-stu-id="f0740-446">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="f0740-447">Во время запроса сообщения записываются в журнал до обработки запроса другими обработчиками в конвейере.</span><span class="sxs-lookup"><span data-stu-id="f0740-447">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="f0740-448">Во время ответа сообщения записываются в журнал после получения ответа другими обработчиками в конвейере.</span><span class="sxs-lookup"><span data-stu-id="f0740-448">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="f0740-449">Кроме того, журнал ведется в конвейере обработчиков запросов.</span><span class="sxs-lookup"><span data-stu-id="f0740-449">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="f0740-450">В примере *MyNamedClient* эти сообщения вносятся в журнал по категории журнала `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="f0740-450">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="f0740-451">Во время запроса это происходит после выполнения всех обработчиков и непосредственно перед отправкой запроса по сети.</span><span class="sxs-lookup"><span data-stu-id="f0740-451">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="f0740-452">Во время ответа в журнале записывается состояние ответа перед его передачей обратно по конвейеру обработчиков.</span><span class="sxs-lookup"><span data-stu-id="f0740-452">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="f0740-453">Включив ведение журнала в конвейере и за его пределами, можно выполнять проверку изменений, внесенных другими обработчиками конвейера.</span><span class="sxs-lookup"><span data-stu-id="f0740-453">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="f0740-454">Сюда входят, например, изменения заголовков запросов или кода состояния ответов.</span><span class="sxs-lookup"><span data-stu-id="f0740-454">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="f0740-455">Включение имени клиента в категорию журнала позволяет фильтровать журналы по именованным клиентам при необходимости.</span><span class="sxs-lookup"><span data-stu-id="f0740-455">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="f0740-456">Настройка HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="f0740-456">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="f0740-457">Иногда необходимо контролировать конфигурацию внутреннего обработчика `HttpMessageHandler`, используемого клиентом.</span><span class="sxs-lookup"><span data-stu-id="f0740-457">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="f0740-458">При добавлении именованного или типизированного клиента возвращается `IHttpClientBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f0740-458">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="f0740-459">Для определения делегата можно использовать метод расширения <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>.</span><span class="sxs-lookup"><span data-stu-id="f0740-459">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="f0740-460">Делегат используется для создания и настройки основного обработчика `HttpMessageHandler`, используемого этим клиентом:</span><span class="sxs-lookup"><span data-stu-id="f0740-460">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="f0740-461">Использование IHttpClientFactory в консольном приложении</span><span class="sxs-lookup"><span data-stu-id="f0740-461">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="f0740-462">В консольном приложении добавьте в проект следующие ссылки на пакеты:</span><span class="sxs-lookup"><span data-stu-id="f0740-462">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="f0740-463">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="f0740-463">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="f0740-464">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="f0740-464">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="f0740-465">В следующем примере:</span><span class="sxs-lookup"><span data-stu-id="f0740-465">In the following example:</span></span>

* <span data-ttu-id="f0740-466"><xref:System.Net.Http.IHttpClientFactory> регистрируется в контейнере службы [универсального узла](xref:fundamentals/host/generic-host):</span><span class="sxs-lookup"><span data-stu-id="f0740-466"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="f0740-467">`MyService` создает экземпляр фабрики клиента из службы, который используется для создания `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f0740-467">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="f0740-468">`HttpClient` используется для получения веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="f0740-468">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="f0740-469">`Main` создает область для выполнения метода `GetPage` службы и вывода первых 500 символов содержимого веб-страницы на консоль.</span><span class="sxs-lookup"><span data-stu-id="f0740-469">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="f0740-470">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="f0740-470">Additional resources</span></span>

* [<span data-ttu-id="f0740-471">Использование HttpClientFactory для реализации устойчивых HTTP-запросов</span><span class="sxs-lookup"><span data-stu-id="f0740-471">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="f0740-472">Реализация повторных попыток вызова HTTP с экспоненциальной задержкой с помощью HttpClientFactory и политик Polly</span><span class="sxs-lookup"><span data-stu-id="f0740-472">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="f0740-473">Реализация шаблона размыкателя цепи</span><span class="sxs-lookup"><span data-stu-id="f0740-473">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="<= aspnetcore-2.1"

<span data-ttu-id="f0740-474">Авторы: [Гленн Кондрон (Glenn Condron)](https://github.com/glennc), [Райан Новак (Ryan Nowak)](https://github.com/rynowak) и [Стив Гордон (Steve Gordon)](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="f0740-474">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="f0740-475"><xref:System.Net.Http.IHttpClientFactory> можно зарегистрировать и использовать для настройки и создания экземпляров <xref:System.Net.Http.HttpClient> в приложении.</span><span class="sxs-lookup"><span data-stu-id="f0740-475">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="f0740-476">Так вы получите следующие преимущества:</span><span class="sxs-lookup"><span data-stu-id="f0740-476">It offers the following benefits:</span></span>

* <span data-ttu-id="f0740-477">Центральное расположение для именования и настройки логических экземпляров `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f0740-477">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="f0740-478">Например, можно зарегистрировать и настроить клиент *github* для доступа к [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="f0740-478">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="f0740-479">Можно зарегистрировать клиент по умолчанию для других целей.</span><span class="sxs-lookup"><span data-stu-id="f0740-479">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="f0740-480">Кодификация концепции исходящего ПО промежуточного слоя путем делегирования обработчиков в `HttpClient` и предоставление расширений для ПО промежуточного слоя на основе Polly для использования этой возможности.</span><span class="sxs-lookup"><span data-stu-id="f0740-480">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="f0740-481">Управление созданием пулов и временем существования базовых экземпляров `HttpClientMessageHandler` с целью избежать обычных проблем с DNS, которые возникают при управлении временем существования `HttpClient` вручную.</span><span class="sxs-lookup"><span data-stu-id="f0740-481">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="f0740-482">Настройка параметров ведения журнала (через `ILogger`) для всех запросов, отправленных через клиентов, созданных фабрикой.</span><span class="sxs-lookup"><span data-stu-id="f0740-482">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="f0740-483">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f0740-483">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f0740-484">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="f0740-484">Prerequisites</span></span>

<span data-ttu-id="f0740-485">Для проектов, предназначенных для .NET Framework, необходимо установить пакет NuGet [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/).</span><span class="sxs-lookup"><span data-stu-id="f0740-485">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="f0740-486">Пакет `Microsoft.Extensions.Http` уже включен в проекты, предназначенные для .NET Core и ссылающиеся на [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="f0740-486">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="f0740-487">Принципы использования</span><span class="sxs-lookup"><span data-stu-id="f0740-487">Consumption patterns</span></span>

<span data-ttu-id="f0740-488">Существует несколько способов использования `IHttpClientFactory` в приложении:</span><span class="sxs-lookup"><span data-stu-id="f0740-488">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="f0740-489">Основное использование</span><span class="sxs-lookup"><span data-stu-id="f0740-489">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="f0740-490">Именованные клиенты</span><span class="sxs-lookup"><span data-stu-id="f0740-490">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="f0740-491">Типизированные клиенты</span><span class="sxs-lookup"><span data-stu-id="f0740-491">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="f0740-492">Созданные клиенты</span><span class="sxs-lookup"><span data-stu-id="f0740-492">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="f0740-493">Все способы равноценны.</span><span class="sxs-lookup"><span data-stu-id="f0740-493">None of them are strictly superior to another.</span></span> <span data-ttu-id="f0740-494">Оптимальный подход зависит от ограничений приложения.</span><span class="sxs-lookup"><span data-stu-id="f0740-494">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="f0740-495">Основное использование</span><span class="sxs-lookup"><span data-stu-id="f0740-495">Basic usage</span></span>

<span data-ttu-id="f0740-496">`IHttpClientFactory` можно зарегистрировать путем вызова метода расширения `AddHttpClient` в `IServiceCollection` внутри метода `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f0740-496">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="f0740-497">После регистрации код может принимать `IHttpClientFactory` в любом месте, куда можно внедрить службу с помощью [внедрения зависимостей](xref:fundamentals/dependency-injection) (DI).</span><span class="sxs-lookup"><span data-stu-id="f0740-497">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="f0740-498">`IHttpClientFactory` можно использовать для создания экземпляра `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="f0740-498">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="f0740-499">Подобное использование `IHttpClientFactory` — это отличный способ рефакторинга имеющегося приложения.</span><span class="sxs-lookup"><span data-stu-id="f0740-499">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="f0740-500">Он не оказывает влияния на использование `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f0740-500">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="f0740-501">Там, где в данный момент создаются экземпляры `HttpClient`, используйте вызов к <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="f0740-501">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="f0740-502">Именованные клиенты</span><span class="sxs-lookup"><span data-stu-id="f0740-502">Named clients</span></span>

<span data-ttu-id="f0740-503">Если для приложения предполагаются разные способы использования `HttpClient`, каждый со своей конфигурацией, можно применять **именованные клиенты**.</span><span class="sxs-lookup"><span data-stu-id="f0740-503">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="f0740-504">Конфигурацию для именованного клиента `HttpClient` можно указать во время регистрации в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f0740-504">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="f0740-505">В приведенном выше коде вызывается клиент `AddHttpClient`, предоставляющий имя *github*.</span><span class="sxs-lookup"><span data-stu-id="f0740-505">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="f0740-506">У клиента есть некоторые настройки по умолчанию &mdash; а именно: базовый адрес и два заголовка, необходимые для работы с API GitHub.</span><span class="sxs-lookup"><span data-stu-id="f0740-506">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="f0740-507">При каждом вызове `CreateClient` создается новый экземпляр `HttpClient` и вызывается действие конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f0740-507">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="f0740-508">Для использования именованного клиента можно передать строковый параметр в `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="f0740-508">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="f0740-509">Укажите имя создаваемого клиента:</span><span class="sxs-lookup"><span data-stu-id="f0740-509">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="f0740-510">В приведенном выше коде в запросе не требуется указывать имя узла.</span><span class="sxs-lookup"><span data-stu-id="f0740-510">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="f0740-511">Достаточно передать только путь, так как используется базовый адрес, заданный для клиента.</span><span class="sxs-lookup"><span data-stu-id="f0740-511">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="f0740-512">Типизированные клиенты</span><span class="sxs-lookup"><span data-stu-id="f0740-512">Typed clients</span></span>

<span data-ttu-id="f0740-513">Типизированные клиенты:</span><span class="sxs-lookup"><span data-stu-id="f0740-513">Typed clients:</span></span>

* <span data-ttu-id="f0740-514">предоставляют те же возможности, что и именованные клиенты, без необходимости использовать строки в качестве ключей.</span><span class="sxs-lookup"><span data-stu-id="f0740-514">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="f0740-515">Это помогает IntelliSense и компилятору при использовании клиентов.</span><span class="sxs-lookup"><span data-stu-id="f0740-515">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="f0740-516">Они предоставляют единое расположение для настройки и взаимодействия с конкретным клиентом `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f0740-516">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="f0740-517">Например, для конечной точки серверной части можно использовать один типизированный клиент, который будет содержать всю логику работы с этой конечной точкой.</span><span class="sxs-lookup"><span data-stu-id="f0740-517">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="f0740-518">Поддерживаются работа с внедрением зависимостей и возможность вставки в нужное место в приложении.</span><span class="sxs-lookup"><span data-stu-id="f0740-518">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="f0740-519">Типизированный клиент принимает параметр `HttpClient` в конструкторе:</span><span class="sxs-lookup"><span data-stu-id="f0740-519">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="f0740-520">В приведенном выше коде конфигурация перемещается в типизированный клиент.</span><span class="sxs-lookup"><span data-stu-id="f0740-520">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="f0740-521">Объект `HttpClient` предоставляется в виде открытого свойства.</span><span class="sxs-lookup"><span data-stu-id="f0740-521">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="f0740-522">Можно определить связанные с API методы, которые предоставляют функциональные возможности `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f0740-522">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="f0740-523">Метод `GetAspNetDocsIssues` инкапсулирует код, необходимый для запроса и анализа последнего открытого выпуска из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="f0740-523">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="f0740-524">Для регистрации типизированного клиента можно использовать универсальный метод расширения <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> в `Startup.ConfigureServices`, указав класс типизированного клиента:</span><span class="sxs-lookup"><span data-stu-id="f0740-524">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="f0740-525">Типизированный клиент регистрируется во внедрении зависимостей как временный.</span><span class="sxs-lookup"><span data-stu-id="f0740-525">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="f0740-526">Типизированный клиент можно внедрить и использовать напрямую:</span><span class="sxs-lookup"><span data-stu-id="f0740-526">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="f0740-527">При желании конфигурацию для типизированного клиента можно указать во время регистрации в `Startup.ConfigureServices`, а не в конструкторе типизированного клиента:</span><span class="sxs-lookup"><span data-stu-id="f0740-527">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="f0740-528">Можно полностью инкапсулировать `HttpClient` внутри типизированного клиента.</span><span class="sxs-lookup"><span data-stu-id="f0740-528">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="f0740-529">Вместо предоставления его как свойства можно использовать открытые методы для внутреннего вызова экземпляра `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f0740-529">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="f0740-530">В приведенном выше коде `HttpClient` хранится как закрытое поле.</span><span class="sxs-lookup"><span data-stu-id="f0740-530">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="f0740-531">Любой доступ для совершения внешних вызовов осуществляется через метод `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="f0740-531">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="f0740-532">Созданные клиенты</span><span class="sxs-lookup"><span data-stu-id="f0740-532">Generated clients</span></span>

<span data-ttu-id="f0740-533">`IHttpClientFactory` можно использовать в сочетании с другими библиотеками сторонних разработчиков, например [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="f0740-533">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="f0740-534">Refit — это библиотека REST для .NET.</span><span class="sxs-lookup"><span data-stu-id="f0740-534">Refit is a REST library for .NET.</span></span> <span data-ttu-id="f0740-535">Она преобразует REST API в динамические интерфейсы.</span><span class="sxs-lookup"><span data-stu-id="f0740-535">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="f0740-536">Реализация интерфейса формируется динамически с помощью `RestService` с использованием `HttpClient` для совершения внешних вызовов HTTP.</span><span class="sxs-lookup"><span data-stu-id="f0740-536">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="f0740-537">Для представления внешнего API и его ответа определяются интерфейс и ответ:</span><span class="sxs-lookup"><span data-stu-id="f0740-537">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="f0740-538">Можно добавить типизированный клиент, используя Refit для создания реализации:</span><span class="sxs-lookup"><span data-stu-id="f0740-538">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="f0740-539">При необходимости можно использовать определенный интерфейс с реализацией, предоставленной внедрением зависимостей и Refit:</span><span class="sxs-lookup"><span data-stu-id="f0740-539">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="f0740-540">ПО промежуточного слоя для исходящих запросов</span><span class="sxs-lookup"><span data-stu-id="f0740-540">Outgoing request middleware</span></span>

<span data-ttu-id="f0740-541">В `HttpClient` уже существует концепция делегирования обработчиков, которые можно связать друг с другом для исходящих HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="f0740-541">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="f0740-542">Класс `IHttpClientFactory` упрощает определение обработчиков для применения к каждому именованному клиенту.</span><span class="sxs-lookup"><span data-stu-id="f0740-542">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="f0740-543">Он поддерживает регистрацию и объединение в цепочки нескольких обработчиков для создания конвейера ПО промежуточного слоя для исходящих запросов.</span><span class="sxs-lookup"><span data-stu-id="f0740-543">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="f0740-544">Каждый из этих обработчиков может выполнять работу до и после исходящего запроса.</span><span class="sxs-lookup"><span data-stu-id="f0740-544">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="f0740-545">Этот шаблон похож на входящий конвейер ПО промежуточного слоя в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f0740-545">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="f0740-546">Шаблон предоставляет механизм управления сквозной функциональностью HTTP-запросов, включая кэширование, обработку ошибок, сериализацию и ведение журнала.</span><span class="sxs-lookup"><span data-stu-id="f0740-546">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="f0740-547">Чтобы создать обработчик, необходимо определить класс, производный от <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="f0740-547">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="f0740-548">Переопределите метод `SendAsync` для выполнения кода до передачи запросов следующему обработчику в конвейере:</span><span class="sxs-lookup"><span data-stu-id="f0740-548">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="f0740-549">В предыдущем коде определяется базовый обработчик.</span><span class="sxs-lookup"><span data-stu-id="f0740-549">The preceding code defines a basic handler.</span></span> <span data-ttu-id="f0740-550">Он проверяет, включен ли в запрос заголовок `X-API-KEY`.</span><span class="sxs-lookup"><span data-stu-id="f0740-550">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="f0740-551">Если заголовок отсутствует, он может избежать вызовов HTTP и вернуть подходящий ответ.</span><span class="sxs-lookup"><span data-stu-id="f0740-551">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="f0740-552">Во время регистрации можно добавить один или несколько обработчиков в конфигурацию для `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f0740-552">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="f0740-553">Эта задача выполняется через методы расширения в <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="f0740-553">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="f0740-554">В приведенном выше коде `ValidateHeaderHandler` регистрируется с помощью внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="f0740-554">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="f0740-555">Обработчик **должен** регистрироваться во внедрении зависимостей как временная служба, а не ограниченная.</span><span class="sxs-lookup"><span data-stu-id="f0740-555">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="f0740-556">Если обработчик зарегистрирован в качестве службы с областью действия и все службы, от которых зависит этот обработчик, освобождаются:</span><span class="sxs-lookup"><span data-stu-id="f0740-556">If the handler is registered as a scoped service and any services that the handler depends upon are disposable:</span></span>

* <span data-ttu-id="f0740-557">Службы обработчика могли быть удалены, прежде чем обработчик вышел из области действия.</span><span class="sxs-lookup"><span data-stu-id="f0740-557">The handler's services could be disposed before the handler goes out of scope.</span></span>
* <span data-ttu-id="f0740-558">Освобожденные службы обработчика приводят к его сбою.</span><span class="sxs-lookup"><span data-stu-id="f0740-558">The disposed handler services causes the handler to fail.</span></span>

<span data-ttu-id="f0740-559">После регистрации можно вызвать <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, передав тип обработчика.</span><span class="sxs-lookup"><span data-stu-id="f0740-559">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

<span data-ttu-id="f0740-560">Можно зарегистрировать несколько обработчиков в порядке, в котором они должны выполняться.</span><span class="sxs-lookup"><span data-stu-id="f0740-560">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="f0740-561">Каждый обработчик содержит следующий обработчик, пока последний `HttpClientHandler` не выполнит запрос:</span><span class="sxs-lookup"><span data-stu-id="f0740-561">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="f0740-562">Используйте один из следующих методов для предоставления общего доступа к состоянию отдельных запросов с помощью обработчиков сообщений:</span><span class="sxs-lookup"><span data-stu-id="f0740-562">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="f0740-563">Передайте данные в обработчик с помощью `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="f0740-563">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="f0740-564">Используйте `IHttpContextAccessor` для доступа к текущему запросу.</span><span class="sxs-lookup"><span data-stu-id="f0740-564">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="f0740-565">Создайте пользовательский объект хранилища `AsyncLocal` для передачи данных.</span><span class="sxs-lookup"><span data-stu-id="f0740-565">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="f0740-566">Использование обработчиков на основе Polly</span><span class="sxs-lookup"><span data-stu-id="f0740-566">Use Polly-based handlers</span></span>

<span data-ttu-id="f0740-567">`IHttpClientFactory` интегрируется с популярной библиотекой сторонних разработчиков под названием [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="f0740-567">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="f0740-568">Polly — это комплексная библиотека, обеспечивающая отказоустойчивость и обработку временных сбоев в .NET.</span><span class="sxs-lookup"><span data-stu-id="f0740-568">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="f0740-569">Она позволяет разработчикам выражать политики, например политику повтора, размыкателя цепи, времени ожидания, изоляции отсеков и отката, более эффективным и потокобезопасным образом.</span><span class="sxs-lookup"><span data-stu-id="f0740-569">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="f0740-570">Для использования политик Polly с настроенными экземплярами `HttpClient` предоставляются методы расширения.</span><span class="sxs-lookup"><span data-stu-id="f0740-570">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="f0740-571">Расширения Polly:</span><span class="sxs-lookup"><span data-stu-id="f0740-571">The Polly extensions:</span></span>

* <span data-ttu-id="f0740-572">Поддерживает добавление обработчиков на основе Polly клиентам.</span><span class="sxs-lookup"><span data-stu-id="f0740-572">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="f0740-573">Можно использовать после установки пакета NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/).</span><span class="sxs-lookup"><span data-stu-id="f0740-573">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="f0740-574">Пакет не включен в общую платформу ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f0740-574">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="f0740-575">Обработка временных сбоев</span><span class="sxs-lookup"><span data-stu-id="f0740-575">Handle transient faults</span></span>

<span data-ttu-id="f0740-576">Чаще всего ошибки происходят, когда внешние вызовы HTTP являются временными.</span><span class="sxs-lookup"><span data-stu-id="f0740-576">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="f0740-577">Используется удобный метод расширения `AddTransientHttpErrorPolicy`, который позволяет определить политику для обработки временных ошибок.</span><span class="sxs-lookup"><span data-stu-id="f0740-577">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="f0740-578">Политики, заданные с помощью этого метода расширения, обрабатывают `HttpRequestException`, ответы HTTP 5xx и ответы HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="f0740-578">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="f0740-579">Расширение `AddTransientHttpErrorPolicy` может быть использовано в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f0740-579">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="f0740-580">Данное расширение предоставляет доступ к объекту `PolicyBuilder`, настроенному для обработки ошибок, представляющих возможный временный сбой:</span><span class="sxs-lookup"><span data-stu-id="f0740-580">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="f0740-581">В приведенном выше коде определена политика `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="f0740-581">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="f0740-582">Неудачные запросы повторяются до трех раз с задержкой 600 мс между попытками.</span><span class="sxs-lookup"><span data-stu-id="f0740-582">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="f0740-583">Динамический выбор политик</span><span class="sxs-lookup"><span data-stu-id="f0740-583">Dynamically select policies</span></span>

<span data-ttu-id="f0740-584">Существуют дополнительные методы расширения, которые можно использовать для добавления обработчиков на основе Polly.</span><span class="sxs-lookup"><span data-stu-id="f0740-584">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="f0740-585">Одним из таких расширений является `AddPolicyHandler` с несколькими перегрузками.</span><span class="sxs-lookup"><span data-stu-id="f0740-585">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="f0740-586">Одна перегрузка разрешает проверку запроса для определения необходимой политики:</span><span class="sxs-lookup"><span data-stu-id="f0740-586">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="f0740-587">Если в приведенном выше коде исходящий запрос является запросом HTTP GET, применяется время ожидания 10 секунд.</span><span class="sxs-lookup"><span data-stu-id="f0740-587">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="f0740-588">Для остальных методов HTTP время ожидания — 30 секунд.</span><span class="sxs-lookup"><span data-stu-id="f0740-588">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="f0740-589">Добавление нескольких обработчиков Polly</span><span class="sxs-lookup"><span data-stu-id="f0740-589">Add multiple Polly handlers</span></span>

<span data-ttu-id="f0740-590">Общепринятой практикой является вложение политик Polly для предоставления расширенной функциональности:</span><span class="sxs-lookup"><span data-stu-id="f0740-590">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="f0740-591">В приведенном выше примере добавляются два обработчика.</span><span class="sxs-lookup"><span data-stu-id="f0740-591">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="f0740-592">Первый использует расширение `AddTransientHttpErrorPolicy`, чтобы добавить политику повтора.</span><span class="sxs-lookup"><span data-stu-id="f0740-592">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="f0740-593">Неудачные запросы выполняются повторно до трех раз.</span><span class="sxs-lookup"><span data-stu-id="f0740-593">Failed requests are retried up to three times.</span></span> <span data-ttu-id="f0740-594">Второй вызов к `AddTransientHttpErrorPolicy` добавляет политику размыкателя цепи.</span><span class="sxs-lookup"><span data-stu-id="f0740-594">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="f0740-595">Дополнительные внешние запросы блокируются в течение 30 секунд в случае пяти неудачных попыток подряд.</span><span class="sxs-lookup"><span data-stu-id="f0740-595">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="f0740-596">Политики размыкателя цепи отслеживают состояние.</span><span class="sxs-lookup"><span data-stu-id="f0740-596">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="f0740-597">Все вызовы через этот клиент имеют одинаковое состояние цепи.</span><span class="sxs-lookup"><span data-stu-id="f0740-597">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="f0740-598">Добавление политик из реестра Polly</span><span class="sxs-lookup"><span data-stu-id="f0740-598">Add policies from the Polly registry</span></span>

<span data-ttu-id="f0740-599">Подход к управлению регулярно используемыми политиками заключается в их однократном определении и регистрации с помощью `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="f0740-599">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="f0740-600">Предоставляется метод расширения, разрешающий добавление обработчика с помощью политики из реестра:</span><span class="sxs-lookup"><span data-stu-id="f0740-600">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="f0740-601">В приведенном выше коде, когда `PolicyRegistry` добавляется в `ServiceCollection`, регистрируются две политики.</span><span class="sxs-lookup"><span data-stu-id="f0740-601">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="f0740-602">Чтобы использовать политику из реестра, применяется метод `AddPolicyHandlerFromRegistry`, который передает имя необходимой политики.</span><span class="sxs-lookup"><span data-stu-id="f0740-602">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="f0740-603">Дополнительные сведения об интеграции `IHttpClientFactory` и Polly см. на [вики-сайте Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="f0740-603">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="f0740-604">Управление HttpClient и временем существования</span><span class="sxs-lookup"><span data-stu-id="f0740-604">HttpClient and lifetime management</span></span>

<span data-ttu-id="f0740-605">При каждом вызове `CreateClient` в `IHttpClientFactory` возвращается новый экземпляр `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f0740-605">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="f0740-606">Для каждого названного клиента существует <xref:System.Net.Http.HttpMessageHandler>.</span><span class="sxs-lookup"><span data-stu-id="f0740-606">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="f0740-607">Фабрика обеспечивает управление временем существования экземпляров `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="f0740-607">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="f0740-608">`IHttpClientFactory` объединяет в пул все экземпляры `HttpMessageHandler`, созданные фабрикой, чтобы уменьшить потребление ресурсов.</span><span class="sxs-lookup"><span data-stu-id="f0740-608">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="f0740-609">Экземпляр `HttpMessageHandler` можно использовать повторно из пула при создании экземпляра `HttpClient`, если его время существования еще не истекло.</span><span class="sxs-lookup"><span data-stu-id="f0740-609">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="f0740-610">Создавать пулы обработчиков желательно, так как каждый обработчик обычно управляет собственными базовыми HTTP-подключениями.</span><span class="sxs-lookup"><span data-stu-id="f0740-610">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="f0740-611">Создание лишних обработчиков может привести к задержке подключения.</span><span class="sxs-lookup"><span data-stu-id="f0740-611">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="f0740-612">Некоторые обработчики поддерживают подключения открытыми в течение неопределенного периода, что может помешать обработчику отреагировать на изменения DNS.</span><span class="sxs-lookup"><span data-stu-id="f0740-612">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="f0740-613">Время существования обработчика по умолчанию — две минуты.</span><span class="sxs-lookup"><span data-stu-id="f0740-613">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="f0740-614">Значение по умолчанию можно переопределить для каждого именованного клиента.</span><span class="sxs-lookup"><span data-stu-id="f0740-614">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="f0740-615">Чтобы переопределить это значение, вызовите <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> в `IHttpClientBuilder`, который возвращается при создании клиента:</span><span class="sxs-lookup"><span data-stu-id="f0740-615">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="f0740-616">Высвобождать клиент не требуется.</span><span class="sxs-lookup"><span data-stu-id="f0740-616">Disposal of the client isn't required.</span></span> <span data-ttu-id="f0740-617">Высвобождение отменяет исходящие запросы и гарантирует, что указанный экземпляр `HttpClient` не может использоваться после вызова <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="f0740-617">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="f0740-618">`IHttpClientFactory` отслеживает и высвобождает ресурсы, используемые экземплярами `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f0740-618">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="f0740-619">Экземпляры `HttpClient` обычно можно рассматривать как объекты .NET, не требующие высвобождения.</span><span class="sxs-lookup"><span data-stu-id="f0740-619">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="f0740-620">До появления `IHttpClientFactory` один экземпляр `HttpClient` часто сохраняли в активном состоянии в течение длительного времени.</span><span class="sxs-lookup"><span data-stu-id="f0740-620">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="f0740-621">После перехода на `IHttpClientFactory` это уже не нужно.</span><span class="sxs-lookup"><span data-stu-id="f0740-621">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

## <a name="logging"></a><span data-ttu-id="f0740-622">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="f0740-622">Logging</span></span>

<span data-ttu-id="f0740-623">Клиенты, созданные через `IHttpClientFactory`, записывают сообщения журнала для всех запросов.</span><span class="sxs-lookup"><span data-stu-id="f0740-623">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="f0740-624">Установите соответствующий уровень информации в конфигурации ведения журнала, чтобы просматривать сообщения журнала по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f0740-624">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="f0740-625">Дополнительное ведение журнала, например запись заголовков запросов, включено только на уровне трассировки.</span><span class="sxs-lookup"><span data-stu-id="f0740-625">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="f0740-626">Категория журнала для каждого клиента включает в себя имя клиента.</span><span class="sxs-lookup"><span data-stu-id="f0740-626">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="f0740-627">Клиент с именем *MyNamedClient*, например, записывает в журнал сообщения с категорией `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="f0740-627">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="f0740-628">Сообщения с суффиксом *LogicalHandler* создаются за пределами конвейера обработчиков запросов.</span><span class="sxs-lookup"><span data-stu-id="f0740-628">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="f0740-629">Во время запроса сообщения записываются в журнал до обработки запроса другими обработчиками в конвейере.</span><span class="sxs-lookup"><span data-stu-id="f0740-629">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="f0740-630">Во время ответа сообщения записываются в журнал после получения ответа другими обработчиками в конвейере.</span><span class="sxs-lookup"><span data-stu-id="f0740-630">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="f0740-631">Кроме того, журнал ведется в конвейере обработчиков запросов.</span><span class="sxs-lookup"><span data-stu-id="f0740-631">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="f0740-632">В примере *MyNamedClient* эти сообщения вносятся в журнал по категории журнала `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="f0740-632">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="f0740-633">Во время запроса это происходит после выполнения всех обработчиков и непосредственно перед отправкой запроса по сети.</span><span class="sxs-lookup"><span data-stu-id="f0740-633">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="f0740-634">Во время ответа в журнале записывается состояние ответа перед его передачей обратно по конвейеру обработчиков.</span><span class="sxs-lookup"><span data-stu-id="f0740-634">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="f0740-635">Включив ведение журнала в конвейере и за его пределами, можно выполнять проверку изменений, внесенных другими обработчиками конвейера.</span><span class="sxs-lookup"><span data-stu-id="f0740-635">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="f0740-636">Сюда входят, например, изменения заголовков запросов или кода состояния ответов.</span><span class="sxs-lookup"><span data-stu-id="f0740-636">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="f0740-637">Включение имени клиента в категорию журнала позволяет фильтровать журналы по именованным клиентам при необходимости.</span><span class="sxs-lookup"><span data-stu-id="f0740-637">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="f0740-638">Настройка HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="f0740-638">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="f0740-639">Иногда необходимо контролировать конфигурацию внутреннего обработчика `HttpMessageHandler`, используемого клиентом.</span><span class="sxs-lookup"><span data-stu-id="f0740-639">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="f0740-640">При добавлении именованного или типизированного клиента возвращается `IHttpClientBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f0740-640">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="f0740-641">Для определения делегата можно использовать метод расширения <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>.</span><span class="sxs-lookup"><span data-stu-id="f0740-641">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="f0740-642">Делегат используется для создания и настройки основного обработчика `HttpMessageHandler`, используемого этим клиентом:</span><span class="sxs-lookup"><span data-stu-id="f0740-642">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="f0740-643">Использование IHttpClientFactory в консольном приложении</span><span class="sxs-lookup"><span data-stu-id="f0740-643">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="f0740-644">В консольном приложении добавьте в проект следующие ссылки на пакеты:</span><span class="sxs-lookup"><span data-stu-id="f0740-644">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="f0740-645">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="f0740-645">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="f0740-646">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="f0740-646">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="f0740-647">В следующем примере:</span><span class="sxs-lookup"><span data-stu-id="f0740-647">In the following example:</span></span>

* <span data-ttu-id="f0740-648"><xref:System.Net.Http.IHttpClientFactory> регистрируется в контейнере службы [универсального узла](xref:fundamentals/host/generic-host):</span><span class="sxs-lookup"><span data-stu-id="f0740-648"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="f0740-649">`MyService` создает экземпляр фабрики клиента из службы, который используется для создания `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f0740-649">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="f0740-650">`HttpClient` используется для получения веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="f0740-650">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="f0740-651">`Main` создает область для выполнения метода `GetPage` службы и вывода первых 500 символов содержимого веб-страницы на консоль.</span><span class="sxs-lookup"><span data-stu-id="f0740-651">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="f0740-652">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="f0740-652">Additional resources</span></span>

* [<span data-ttu-id="f0740-653">Использование HttpClientFactory для реализации устойчивых HTTP-запросов</span><span class="sxs-lookup"><span data-stu-id="f0740-653">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="f0740-654">Реализация повторных попыток вызова HTTP с экспоненциальной задержкой с помощью HttpClientFactory и политик Polly</span><span class="sxs-lookup"><span data-stu-id="f0740-654">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="f0740-655">Реализация шаблона размыкателя цепи</span><span class="sxs-lookup"><span data-stu-id="f0740-655">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end
