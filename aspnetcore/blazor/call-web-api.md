---
title: Вызов веб-API из ASP.NET Core Блазор
author: guardrex
description: Узнайте, как вызывать веб-API из приложения Блазор с помощью вспомогательных функций JSON, включая создание запросов на общий доступ к ресурсам в разных источниках (CORS).
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/call-web-api
ms.openlocfilehash: 152a2d5ac9a4325592ca414e9ea5e70c947d079f
ms.sourcegitcommit: 092061c4f6ef46ed2165fa84de6273d3786fb97e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2019
ms.locfileid: "70963697"
---
# <a name="call-a-web-api-from-aspnet-core-blazor"></a><span data-ttu-id="f4240-103">Вызов веб-API из ASP.NET Core Блазор</span><span class="sxs-lookup"><span data-stu-id="f4240-103">Call a web API from ASP.NET Core Blazor</span></span>

<span data-ttu-id="f4240-104">[Люк ЛаСаМ](https://github.com/guardrex) и [Даниэль Roth)](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="f4240-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="f4240-105">Блазорные приложения вызывают веб-API с помощью предварительно настроенной `HttpClient` службы.</span><span class="sxs-lookup"><span data-stu-id="f4240-105">Blazor WebAssembly apps call web APIs using a preconfigured `HttpClient` service.</span></span> <span data-ttu-id="f4240-106">Запросы на создание, которые могут включать параметры [API-интерфейса получения](https://developer.mozilla.org/docs/Web/API/Fetch_API) JavaScript, с помощью вспомогательных <xref:System.Net.Http.HttpRequestMessage>функций JSON блазор или с.</span><span class="sxs-lookup"><span data-stu-id="f4240-106">Compose requests, which can include JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) options, using Blazor JSON helpers or with <xref:System.Net.Http.HttpRequestMessage>.</span></span>

<span data-ttu-id="f4240-107">Серверные приложения блазор вызывают веб- <xref:System.Net.Http.HttpClient> API с помощью экземпляров <xref:System.Net.Http.IHttpClientFactory>, обычно созданных с помощью.</span><span class="sxs-lookup"><span data-stu-id="f4240-107">Blazor Server apps call web APIs using <xref:System.Net.Http.HttpClient> instances typically created using <xref:System.Net.Http.IHttpClientFactory>.</span></span> <span data-ttu-id="f4240-108">Дополнительные сведения см. в разделе <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="f4240-108">For more information, see <xref:fundamentals/http-requests>.</span></span>

<span data-ttu-id="f4240-109">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f4240-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="f4240-110">Примеры Блазор см. в следующих компонентах примера приложения:</span><span class="sxs-lookup"><span data-stu-id="f4240-110">For Blazor WebAssembly examples, see the following components in the sample app:</span></span>

* <span data-ttu-id="f4240-111">Вызов веб-API (*pages/каллвебапи. Razor*)</span><span class="sxs-lookup"><span data-stu-id="f4240-111">Call Web API (*Pages/CallWebAPI.razor*)</span></span>
* <span data-ttu-id="f4240-112">Тестер HTTP-запросов (*Components/хттпрекуесттестер. Razor*)</span><span class="sxs-lookup"><span data-stu-id="f4240-112">HTTP Request Tester (*Components/HTTPRequestTester.razor*)</span></span>

## <a name="httpclient-and-json-helpers"></a><span data-ttu-id="f4240-113">HttpClient и вспомогательные методы JSON</span><span class="sxs-lookup"><span data-stu-id="f4240-113">HttpClient and JSON helpers</span></span>

<span data-ttu-id="f4240-114">В приложениях Блазор веб-сборки [HttpClient](xref:fundamentals/http-requests) доступна в виде предварительно настроенной службы для выполнения запросов к серверу-источнику.</span><span class="sxs-lookup"><span data-stu-id="f4240-114">In Blazor WebAssembly apps, [HttpClient](xref:fundamentals/http-requests) is available as a preconfigured service for making requests back to the origin server.</span></span> <span data-ttu-id="f4240-115">Чтобы использовать `HttpClient` вспомогательные методы JSON, добавьте ссылку на пакет в `Microsoft.AspNetCore.Blazor.HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f4240-115">To use `HttpClient` JSON helpers, add a package reference to `Microsoft.AspNetCore.Blazor.HttpClient`.</span></span> <span data-ttu-id="f4240-116">`HttpClient`Кроме того, вспомогательные методы JSON также используются для вызова конечных точек веб-API сторонних производителей.</span><span class="sxs-lookup"><span data-stu-id="f4240-116">`HttpClient` and JSON helpers are also used to call third-party web API endpoints.</span></span> <span data-ttu-id="f4240-117">`HttpClient`реализуется с помощью [API выборки](https://developer.mozilla.org/docs/Web/API/Fetch_API) в браузере и подчиняется его ограничениям, включая принудительное применение той же политики происхождения.</span><span class="sxs-lookup"><span data-stu-id="f4240-117">`HttpClient` is implemented using the browser [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) and is subject to its limitations, including enforcement of the same origin policy.</span></span>

<span data-ttu-id="f4240-118">Базовый адрес клиента устанавливается в адрес исходного сервера.</span><span class="sxs-lookup"><span data-stu-id="f4240-118">The client's base address is set to the originating server's address.</span></span> <span data-ttu-id="f4240-119">`HttpClient` Вставьте экземпляр `@inject` с помощью директивы:</span><span class="sxs-lookup"><span data-stu-id="f4240-119">Inject an `HttpClient` instance using the `@inject` directive:</span></span>

```cshtml
@using System.Net.Http
@inject HttpClient Http
```

<span data-ttu-id="f4240-120">В следующих примерах веб-API TODO обрабатывает операции создания, чтения, обновления и удаления (CRUD).</span><span class="sxs-lookup"><span data-stu-id="f4240-120">In the following examples, a Todo web API processes create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="f4240-121">Примеры основаны на `TodoItem` классе, который хранит:</span><span class="sxs-lookup"><span data-stu-id="f4240-121">The examples are based on a `TodoItem` class that stores the:</span></span>

* <span data-ttu-id="f4240-122">Идентификатор (`Id`, `long`) &ndash; уникальный идентификатор элемента.</span><span class="sxs-lookup"><span data-stu-id="f4240-122">ID (`Id`, `long`) &ndash; Unique ID of the item.</span></span>
* <span data-ttu-id="f4240-123">Имя (`Name`, `string`) &ndash; имя элемента.</span><span class="sxs-lookup"><span data-stu-id="f4240-123">Name (`Name`, `string`) &ndash; Name of the item.</span></span>
* <span data-ttu-id="f4240-124">Состояние (`IsComplete`, `bool`) &ndash; указывает, что элемент TODO завершен.</span><span class="sxs-lookup"><span data-stu-id="f4240-124">Status (`IsComplete`, `bool`) &ndash; Indication if the Todo item is finished.</span></span>

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

<span data-ttu-id="f4240-125">Вспомогательные методы JSON отправляют запросы в URI (веб-API в следующих примерах) и обрабатывают ответ:</span><span class="sxs-lookup"><span data-stu-id="f4240-125">JSON helper methods send requests to a URI (a web API in the following examples) and process the response:</span></span>

* <span data-ttu-id="f4240-126">`GetJsonAsync`&ndash; Отправляет запрос HTTP GET и анализирует текст ответа JSON для создания объекта.</span><span class="sxs-lookup"><span data-stu-id="f4240-126">`GetJsonAsync` &ndash; Sends an HTTP GET request and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="f4240-127">В следующем коде `_todoItems` компонент отображается с помощью компонента.</span><span class="sxs-lookup"><span data-stu-id="f4240-127">In the following code, the `_todoItems` are displayed by the component.</span></span> <span data-ttu-id="f4240-128">Метод активируется при завершении подготовки компонента к просмотру ([онинитиализедасинк).](xref:blazor/components#lifecycle-methods) `GetTodoItems`</span><span class="sxs-lookup"><span data-stu-id="f4240-128">The `GetTodoItems` method is triggered when the component is finished rendering ([OnInitializedAsync](xref:blazor/components#lifecycle-methods)).</span></span> <span data-ttu-id="f4240-129">Полный пример см. в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="f4240-129">See the sample app for a complete example.</span></span>

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/todo");
  }
  ```

* <span data-ttu-id="f4240-130">`PostJsonAsync`&ndash; Отправляет запрос HTTP POST, включая содержимое в кодировке JSON, и анализирует текст ответа JSON для создания объекта.</span><span class="sxs-lookup"><span data-stu-id="f4240-130">`PostJsonAsync` &ndash; Sends an HTTP POST request, including JSON-encoded content, and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="f4240-131">В приведенном ниже коде `_newItemName` предоставляется привязанным элементом компонента.</span><span class="sxs-lookup"><span data-stu-id="f4240-131">In the following code, `_newItemName` is provided by a bound element of the component.</span></span> <span data-ttu-id="f4240-132">Метод активируется путем `<button>` выбора элемента. `AddItem`</span><span class="sxs-lookup"><span data-stu-id="f4240-132">The `AddItem` method is triggered by selecting a `<button>` element.</span></span> <span data-ttu-id="f4240-133">Полный пример см. в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="f4240-133">See the sample app for a complete example.</span></span>

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  <input @bind="_newItemName" placeholder="New Todo Item" />
  <button @onclick="@AddItem">Add</button>

  @code {
      private string _newItemName;

      private async Task AddItem()
      {
          var addItem = new TodoItem { Name = _newItemName, IsComplete = false };
          await Http.PostJsonAsync("api/todo", addItem);
      }
  }
  ```

* <span data-ttu-id="f4240-134">`PutJsonAsync`&ndash; Отправляет запрос HTTP-размещения, включая содержимое в кодировке JSON.</span><span class="sxs-lookup"><span data-stu-id="f4240-134">`PutJsonAsync` &ndash; Sends an HTTP PUT request, including JSON-encoded content.</span></span>

  <span data-ttu-id="f4240-135">В следующем коде `_editItem` значения для `Name` и `IsCompleted` предоставляются связанными элементами компонента.</span><span class="sxs-lookup"><span data-stu-id="f4240-135">In the following code, `_editItem` values for `Name` and `IsCompleted` are provided by bound elements of the component.</span></span> <span data-ttu-id="f4240-136">Элемент `Id` задается, когда элемент выбирается в другой части пользовательского интерфейса и `EditItem` вызывается.</span><span class="sxs-lookup"><span data-stu-id="f4240-136">The item's `Id` is set when the item is selected in another part of the UI and `EditItem` is called.</span></span> <span data-ttu-id="f4240-137">Метод активируется путем выбора элемента Save `<button>`. `SaveItem`</span><span class="sxs-lookup"><span data-stu-id="f4240-137">The `SaveItem` method is triggered by selecting the Save `<button>` element.</span></span> <span data-ttu-id="f4240-138">Полный пример см. в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="f4240-138">See the sample app for a complete example.</span></span>

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  <input type="checkbox" @bind="_editItem.IsComplete" />
  <input @bind="_editItem.Name" />
  <button @onclick="@SaveItem">Save</button>

  @code {
      private TodoItem _editItem = new TodoItem();

      private void EditItem(long id)
      {
          var editItem = _todoItems.Single(i => i.Id == id);
          _editItem = new TodoItem { Id = editItem.Id, Name = editItem.Name, 
              IsComplete = editItem.IsComplete };
      }

      private async Task SaveItem() =>
          await Http.PutJsonAsync($"api/todo/{_editItem.Id}, _editItem);
  }
  ```

<span data-ttu-id="f4240-139"><xref:System.Net.Http>включает дополнительные методы расширения для отправки HTTP-запросов и получения HTTP-ответов.</span><span class="sxs-lookup"><span data-stu-id="f4240-139"><xref:System.Net.Http> includes additional extension methods for sending HTTP requests and receiving HTTP responses.</span></span> <span data-ttu-id="f4240-140">[HttpClient. DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) используется для отправки запроса HTTP DELETE в веб-интерфейс API.</span><span class="sxs-lookup"><span data-stu-id="f4240-140">[HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) is used to send an HTTP DELETE request to a web API.</span></span>

<span data-ttu-id="f4240-141">В следующем коде элемент Delete `<button>` `DeleteItem` вызывает метод.</span><span class="sxs-lookup"><span data-stu-id="f4240-141">In the following code, the Delete `<button>` element calls the `DeleteItem` method.</span></span> <span data-ttu-id="f4240-142">Связанный `<input>` элемент`id` предоставляет удаляемый элемент.</span><span class="sxs-lookup"><span data-stu-id="f4240-142">The bound `<input>` element supplies the `id` of the item to delete.</span></span> <span data-ttu-id="f4240-143">Полный пример см. в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="f4240-143">See the sample app for a complete example.</span></span>

```cshtml
@using System.Net.Http
@inject HttpClient Http

<input @bind="_id" />
<button @onclick="@DeleteItem">Delete</button>

@code {
    private long _id;

    private async Task DeleteItem() =>
        await Http.DeleteAsync($"api/todo/{_id}");
}
```

## <a name="cross-origin-resource-sharing-cors"></a><span data-ttu-id="f4240-144">Общий доступ к ресурсам между источниками (CORS)</span><span class="sxs-lookup"><span data-stu-id="f4240-144">Cross-origin resource sharing (CORS)</span></span>

<span data-ttu-id="f4240-145">Безопасность в браузере предотвращает запросы веб-страницы к другому домену, отличному от того, который обслуживает веб-страницу.</span><span class="sxs-lookup"><span data-stu-id="f4240-145">Browser security prevents a webpage from making requests to a different domain than the one that served the webpage.</span></span> <span data-ttu-id="f4240-146">Это ограничение называется *политикой того же происхождения*.</span><span class="sxs-lookup"><span data-stu-id="f4240-146">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="f4240-147">Политика того же источника не позволит вредоносному сайту считывать конфиденциальные данные с другого сайта.</span><span class="sxs-lookup"><span data-stu-id="f4240-147">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="f4240-148">Чтобы сделать запросы от браузера к конечной точке с другим источником, *Конечная точка* должна включить [общий доступ к ресурсам в разных источниках (CORS)](https://www.w3.org/TR/cors/).</span><span class="sxs-lookup"><span data-stu-id="f4240-148">To make requests from the browser to an endpoint with a different origin, the *endpoint* must enable [cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/).</span></span>

<span data-ttu-id="f4240-149">Пример приложения демонстрирует использование CORS в компоненте Call Web API (*pages/каллвебапи. Razor*).</span><span class="sxs-lookup"><span data-stu-id="f4240-149">The sample app demonstrates the use of CORS in the Call Web API component (*Pages/CallWebAPI.razor*).</span></span>

<span data-ttu-id="f4240-150">Сведения о том, как разрешить другим сайтам выполнять запросы общего доступа к ресурсам на основе источника (CORS <xref:security/cors>) в приложение, см. в разделе.</span><span class="sxs-lookup"><span data-stu-id="f4240-150">To allow other sites to make cross-origin resource sharing (CORS) requests to your app, see <xref:security/cors>.</span></span>

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a><span data-ttu-id="f4240-151">HttpClient и HttpRequestMessage с параметрами запроса API FETCH</span><span class="sxs-lookup"><span data-stu-id="f4240-151">HttpClient and HttpRequestMessage with Fetch API request options</span></span>

<span data-ttu-id="f4240-152">При выполнении для сборки в блазор приложении [HttpClient](xref:fundamentals/http-requests) <xref:System.Net.Http.HttpRequestMessage> используйте для настройки запросов.</span><span class="sxs-lookup"><span data-stu-id="f4240-152">When running on WebAssembly in a Blazor WebAssembly app, use [HttpClient](xref:fundamentals/http-requests) and <xref:System.Net.Http.HttpRequestMessage> to customize requests.</span></span> <span data-ttu-id="f4240-153">Например, можно указать URI запроса, метод HTTP и все нужные заголовки запроса.</span><span class="sxs-lookup"><span data-stu-id="f4240-153">For example, you can specify the request URI, HTTP method, and any desired request headers.</span></span>

<span data-ttu-id="f4240-154">Укажите параметры запроса для базового [API выборки](https://developer.mozilla.org/docs/Web/API/Fetch_API) JavaScript, `WebAssemblyHttpMessageHandler.FetchArgs` используя свойство в запросе.</span><span class="sxs-lookup"><span data-stu-id="f4240-154">Supply request options to the underlying JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) using the `WebAssemblyHttpMessageHandler.FetchArgs` property on the request.</span></span> <span data-ttu-id="f4240-155">Как показано в следующем примере, `credentials` свойству присвоено любое из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="f4240-155">As shown in the following example, the `credentials` property is set to any of the following values:</span></span>

* <span data-ttu-id="f4240-156">`FetchCredentialsOption.Include`("include") &ndash; Сообщает браузеру о необходимости отправки учетных данных (например, файлов cookie или заголовков проверки подлинности HTTP) даже для запросов между источниками.</span><span class="sxs-lookup"><span data-stu-id="f4240-156">`FetchCredentialsOption.Include` ("include") &ndash; Advises the browser to send credentials (such as cookies or HTTP authentication headers) even for cross-origin requests.</span></span> <span data-ttu-id="f4240-157">Допускается, только если политика CORS настроена для разрешения учетных данных.</span><span class="sxs-lookup"><span data-stu-id="f4240-157">Only allowed when the CORS policy is configured to allow credentials.</span></span>
* <span data-ttu-id="f4240-158">`FetchCredentialsOption.Omit`("пропустить") &ndash; Рекомендует браузеру никогда не передавать учетные данные (например, файлы cookie или заголовки проверки подлинности HTTP).</span><span class="sxs-lookup"><span data-stu-id="f4240-158">`FetchCredentialsOption.Omit` ("omit") &ndash; Advises the browser never to send credentials (such as cookies or HTTP auth headers).</span></span>
* <span data-ttu-id="f4240-159">`FetchCredentialsOption.SameOrigin`("один источник") &ndash; Рекомендует браузеру передавать учетные данные (например, файлы cookie или заголовки проверки подлинности HTTP) только в том случае, если целевой URL-адрес находится в том же источнике, что и вызывающее приложение.</span><span class="sxs-lookup"><span data-stu-id="f4240-159">`FetchCredentialsOption.SameOrigin` ("same-origin") &ndash; Advises the browser to send credentials (such as cookies or HTTP auth headers) only if the target URL is on the same origin as the calling application.</span></span>

```cshtml
@using System.Net.Http
@using System.Net.Http.Headers
@inject HttpClient Http

@code {
    private async Task PostRequest()
    {
        Http.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Bearer", "{OAUTH TOKEN}");

        var requestMessage = new HttpRequestMessage()
        {
            Method = new HttpMethod("POST"),
            RequestUri = new Uri("https://localhost:10000/api/todo"),
            Content = 
                new StringContent(
                    @"{""name"":""A New Todo Item"",""isComplete"":false}")
        };

        requestMessage.Content.Headers.ContentType = 
            new System.Net.Http.Headers.MediaTypeHeaderValue(
                "application/json");

        requestMessage.Content.Headers.TryAddWithoutValidation(
            "x-custom-header", "value");
        
        requestMessage.Properties[WebAssemblyHttpMessageHandler.FetchArgs] = new
        { 
            credentials = FetchCredentialsOption.Include
        };

        var response = await Http.SendAsync(requestMessage);
        var responseStatusCode = response.StatusCode;
        var responseBody = await response.Content.ReadAsStringAsync();
    }
}
```

<span data-ttu-id="f4240-160">Дополнительные сведения о параметрах API FETCH см. [в разделе веб-документы MDN: Виндоворворкерглобалскопе. fetch ():P араметерс](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span><span class="sxs-lookup"><span data-stu-id="f4240-160">For more information on Fetch API options, see [MDN web docs: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span></span>

<span data-ttu-id="f4240-161">При отправке учетных данных (файлов cookie или заголовков авторизации `Authorization` ) в запросах CORS этот заголовок должен быть разрешен политикой CORS.</span><span class="sxs-lookup"><span data-stu-id="f4240-161">When sending credentials (authorization cookies/headers) on CORS requests, the `Authorization` header must be allowed by the CORS policy.</span></span>

<span data-ttu-id="f4240-162">Следующая политика включает в себя настройку для:</span><span class="sxs-lookup"><span data-stu-id="f4240-162">The following policy includes configuration for:</span></span>

* <span data-ttu-id="f4240-163">Источники запроса (`http://localhost:5000`, `https://localhost:5001`).</span><span class="sxs-lookup"><span data-stu-id="f4240-163">Request origins (`http://localhost:5000`, `https://localhost:5001`).</span></span>
* <span data-ttu-id="f4240-164">Любой метод (глагол).</span><span class="sxs-lookup"><span data-stu-id="f4240-164">Any method (verb).</span></span>
* <span data-ttu-id="f4240-165">`Content-Type`и `Authorization` заголовки.</span><span class="sxs-lookup"><span data-stu-id="f4240-165">`Content-Type` and `Authorization` headers.</span></span> <span data-ttu-id="f4240-166">Чтобы разрешить пользовательский заголовок (например, `x-custom-header`), выведите список заголовков при вызове метода. <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*></span><span class="sxs-lookup"><span data-stu-id="f4240-166">To allow a custom header (for example, `x-custom-header`), list the header when calling <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span></span>
* <span data-ttu-id="f4240-167">Учетные данные, которые`credentials` `include`задаются кодом JavaScript на стороне клиента (свойство имеет значение).</span><span class="sxs-lookup"><span data-stu-id="f4240-167">Credentials set by client-side JavaScript code (`credentials` property set to `include`).</span></span>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

<span data-ttu-id="f4240-168">Дополнительные сведения см. в <xref:security/cors> разделе и компоненте тестера HTTP-запросов примера приложения (*Components/хттпрекуесттестер. Razor*).</span><span class="sxs-lookup"><span data-stu-id="f4240-168">For more information, see <xref:security/cors> and the sample app's HTTP Request Tester component (*Components/HTTPRequestTester.razor*).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f4240-169">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="f4240-169">Additional resources</span></span>

* <xref:fundamentals/http-requests>
* [<span data-ttu-id="f4240-170">Общий доступ к ресурсам в разных источниках (CORS) в консорциуме W3C</span><span class="sxs-lookup"><span data-stu-id="f4240-170">Cross Origin Resource Sharing (CORS) at W3C</span></span>](https://www.w3.org/TR/cors/)
