---
title: Вызов веб-API из ASP.NET Core Blazor
author: guardrex
description: Узнайте, как вызывать веб-API из Blazor приложения с помощью вспомогательных средств JSON, включая создание запросов на общий доступ к ресурсам в разных источниках (CORS).
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/call-web-api
ms.openlocfilehash: 66605f38a6fcaedebc92b0946dca1e5f28b593c6
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/17/2020
ms.locfileid: "76160071"
---
# <a name="call-a-web-api-from-aspnet-core-opno-locblazor"></a><span data-ttu-id="128d6-103">Вызов веб-API из ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="128d6-103">Call a web API from ASP.NET Core Blazor</span></span>

<span data-ttu-id="128d6-104">[Люк ЛаСаМ](https://github.com/guardrex), [Даниэль Roth)](https://github.com/danroth27)и самое [Нела Круз](https://github.com/juandelacruz23)</span><span class="sxs-lookup"><span data-stu-id="128d6-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Juan De la Cruz](https://github.com/juandelacruz23)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="128d6-105">[Blazor приложения сборки](xref:blazor/hosting-models#blazor-webassembly) вызывают веб-API с помощью предварительно настроенной службы `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="128d6-105">[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) apps call web APIs using a preconfigured `HttpClient` service.</span></span> <span data-ttu-id="128d6-106">Запросы на создание, которые могут включать параметры [API-интерфейса получения](https://developer.mozilla.org/docs/Web/API/Fetch_API) JavaScript, с помощью Blazor вспомогательных функций JSON или <xref:System.Net.Http.HttpRequestMessage>.</span><span class="sxs-lookup"><span data-stu-id="128d6-106">Compose requests, which can include JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) options, using Blazor JSON helpers or with <xref:System.Net.Http.HttpRequestMessage>.</span></span>

<span data-ttu-id="128d6-107">[Blazor серверные](xref:blazor/hosting-models#blazor-server) приложения вызывают веб-API с помощью экземпляров <xref:System.Net.Http.HttpClient>, обычно созданных с помощью <xref:System.Net.Http.IHttpClientFactory>.</span><span class="sxs-lookup"><span data-stu-id="128d6-107">[Blazor Server](xref:blazor/hosting-models#blazor-server) apps call web APIs using <xref:System.Net.Http.HttpClient> instances typically created using <xref:System.Net.Http.IHttpClientFactory>.</span></span> <span data-ttu-id="128d6-108">Для получения дополнительной информации см. <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="128d6-108">For more information, see <xref:fundamentals/http-requests>.</span></span>

<span data-ttu-id="128d6-109">[Просмотрите или Скачайте образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([как скачать](xref:index#how-to-download-a-sample)) &ndash; выберите приложение *блазорвебассемблисампле* .</span><span class="sxs-lookup"><span data-stu-id="128d6-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample)) &ndash; Select the *BlazorWebAssemblySample* app.</span></span>

<span data-ttu-id="128d6-110">См. следующие компоненты в примере приложения *блазорвебассемблисампле* :</span><span class="sxs-lookup"><span data-stu-id="128d6-110">See the following components in the *BlazorWebAssemblySample* sample app:</span></span>

* <span data-ttu-id="128d6-111">Вызов веб-API (*pages/каллвебапи. Razor*)</span><span class="sxs-lookup"><span data-stu-id="128d6-111">Call Web API (*Pages/CallWebAPI.razor*)</span></span>
* <span data-ttu-id="128d6-112">Тестер HTTP-запросов (*Components/хттпрекуесттестер. Razor*)</span><span class="sxs-lookup"><span data-stu-id="128d6-112">HTTP Request Tester (*Components/HTTPRequestTester.razor*)</span></span>

## <a name="packages"></a><span data-ttu-id="128d6-113">пакеты,</span><span class="sxs-lookup"><span data-stu-id="128d6-113">Packages</span></span>

<span data-ttu-id="128d6-114">Сослаться на *экспериментальный* [Microsoft. AspNetCore.Blazor. HttpClient](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.HttpClient/) пакет NuGet в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="128d6-114">Reference the *experimental* [Microsoft.AspNetCore.Blazor.HttpClient](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.HttpClient/) NuGet package in the project file.</span></span> <span data-ttu-id="128d6-115">`Microsoft.AspNetCore.Blazor.HttpClient` основан на `HttpClient` и [System. Text. JSON](https://www.nuget.org/packages/System.Text.Json/).</span><span class="sxs-lookup"><span data-stu-id="128d6-115">`Microsoft.AspNetCore.Blazor.HttpClient` is based on `HttpClient` and [System.Text.Json](https://www.nuget.org/packages/System.Text.Json/).</span></span>

<span data-ttu-id="128d6-116">Чтобы использовать стабильный API, используйте пакет [Microsoft. AspNet. WebApi. Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) , использующий [Newtonsoft. JSON](https://www.nuget.org/packages/Newtonsoft.Json/)/[JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="128d6-116">To use a stable API, use the [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) package, which uses [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/)/[Json.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span> <span data-ttu-id="128d6-117">Использование стабильного API в `Microsoft.AspNet.WebApi.Client` не предоставляет вспомогательные методы JSON, описанные в этом разделе, которые являются уникальными для экспериментального `Microsoft.AspNetCore.Blazor.HttpClient` пакета.</span><span class="sxs-lookup"><span data-stu-id="128d6-117">Using the stable API in `Microsoft.AspNet.WebApi.Client` doesn't provide the JSON helpers described in this topic, which are unique to the experimental `Microsoft.AspNetCore.Blazor.HttpClient` package.</span></span>

## <a name="httpclient-and-json-helpers"></a><span data-ttu-id="128d6-118">HttpClient и вспомогательные методы JSON</span><span class="sxs-lookup"><span data-stu-id="128d6-118">HttpClient and JSON helpers</span></span>

<span data-ttu-id="128d6-119">В Blazor приложении [HttpClient](xref:fundamentals/http-requests) доступна как предварительно настроенная служба для выполнения запросов к серверу-источнику.</span><span class="sxs-lookup"><span data-stu-id="128d6-119">In a Blazor WebAssembly app, [HttpClient](xref:fundamentals/http-requests) is available as a preconfigured service for making requests back to the origin server.</span></span>

<span data-ttu-id="128d6-120">По умолчанию серверное приложение Blazor не включает службу `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="128d6-120">A Blazor Server app doesn't include an `HttpClient` service by default.</span></span> <span data-ttu-id="128d6-121">Предоставьте `HttpClient` приложению с помощью [инфраструктуры фабрики HttpClient](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="128d6-121">Provide an `HttpClient` to the app using the [HttpClient factory infrastructure](xref:fundamentals/http-requests).</span></span>

<span data-ttu-id="128d6-122">`HttpClient` и вспомогательные методы JSON также используются для вызова конечных точек веб-API сторонних производителей.</span><span class="sxs-lookup"><span data-stu-id="128d6-122">`HttpClient` and JSON helpers are also used to call third-party web API endpoints.</span></span> <span data-ttu-id="128d6-123">`HttpClient` реализуется с помощью [API-интерфейса выборки](https://developer.mozilla.org/docs/Web/API/Fetch_API) браузера и подчиняется его ограничениям, включая принудительное применение той же политики происхождения.</span><span class="sxs-lookup"><span data-stu-id="128d6-123">`HttpClient` is implemented using the browser [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) and is subject to its limitations, including enforcement of the same origin policy.</span></span>

<span data-ttu-id="128d6-124">Базовый адрес клиента устанавливается в адрес исходного сервера.</span><span class="sxs-lookup"><span data-stu-id="128d6-124">The client's base address is set to the originating server's address.</span></span> <span data-ttu-id="128d6-125">Внедрить `HttpClient` экземпляр с помощью директивы `@inject`:</span><span class="sxs-lookup"><span data-stu-id="128d6-125">Inject an `HttpClient` instance using the `@inject` directive:</span></span>

```razor
@using System.Net.Http
@inject HttpClient Http
```

<span data-ttu-id="128d6-126">В следующих примерах веб-API TODO обрабатывает операции создания, чтения, обновления и удаления (CRUD).</span><span class="sxs-lookup"><span data-stu-id="128d6-126">In the following examples, a Todo web API processes create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="128d6-127">Примеры основаны на классе `TodoItem`, в котором хранится:</span><span class="sxs-lookup"><span data-stu-id="128d6-127">The examples are based on a `TodoItem` class that stores the:</span></span>

* <span data-ttu-id="128d6-128">ID (`Id`, `long`) &ndash; уникальный идентификатор элемента.</span><span class="sxs-lookup"><span data-stu-id="128d6-128">ID (`Id`, `long`) &ndash; Unique ID of the item.</span></span>
* <span data-ttu-id="128d6-129">Имя (`Name`, `string`) &ndash; имя элемента.</span><span class="sxs-lookup"><span data-stu-id="128d6-129">Name (`Name`, `string`) &ndash; Name of the item.</span></span>
* <span data-ttu-id="128d6-130">Состояние (`IsComplete`, `bool`) &ndash; указывает, что элемент TODO завершен.</span><span class="sxs-lookup"><span data-stu-id="128d6-130">Status (`IsComplete`, `bool`) &ndash; Indication if the Todo item is finished.</span></span>

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

<span data-ttu-id="128d6-131">Вспомогательные методы JSON отправляют запросы в URI (веб-API в следующих примерах) и обрабатывают ответ:</span><span class="sxs-lookup"><span data-stu-id="128d6-131">JSON helper methods send requests to a URI (a web API in the following examples) and process the response:</span></span>

* <span data-ttu-id="128d6-132">`GetJsonAsync` &ndash; отправляет запрос HTTP GET и анализирует текст ответа JSON для создания объекта.</span><span class="sxs-lookup"><span data-stu-id="128d6-132">`GetJsonAsync` &ndash; Sends an HTTP GET request and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="128d6-133">В следующем коде `_todoItems` отображаются компонентом.</span><span class="sxs-lookup"><span data-stu-id="128d6-133">In the following code, the `_todoItems` are displayed by the component.</span></span> <span data-ttu-id="128d6-134">Метод `GetTodoItems` активируется при завершении подготовки компонента к просмотру ([онинитиализедасинк](xref:blazor/lifecycle#component-initialization-methods)).</span><span class="sxs-lookup"><span data-stu-id="128d6-134">The `GetTodoItems` method is triggered when the component is finished rendering ([OnInitializedAsync](xref:blazor/lifecycle#component-initialization-methods)).</span></span> <span data-ttu-id="128d6-135">Полный пример см. в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="128d6-135">See the sample app for a complete example.</span></span>

  ```razor
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/TodoItems");
  }
  ```

* <span data-ttu-id="128d6-136">`PostJsonAsync` &ndash; отправляет запрос HTTP POST, включая содержимое в кодировке JSON, и анализирует текст ответа JSON для создания объекта.</span><span class="sxs-lookup"><span data-stu-id="128d6-136">`PostJsonAsync` &ndash; Sends an HTTP POST request, including JSON-encoded content, and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="128d6-137">В следующем коде `_newItemName` предоставляется связанным элементом компонента.</span><span class="sxs-lookup"><span data-stu-id="128d6-137">In the following code, `_newItemName` is provided by a bound element of the component.</span></span> <span data-ttu-id="128d6-138">Метод `AddItem` активируется путем выбора элемента `<button>`.</span><span class="sxs-lookup"><span data-stu-id="128d6-138">The `AddItem` method is triggered by selecting a `<button>` element.</span></span> <span data-ttu-id="128d6-139">Полный пример см. в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="128d6-139">See the sample app for a complete example.</span></span>

  ```razor
  @using System.Net.Http
  @inject HttpClient Http

  <input @bind="_newItemName" placeholder="New Todo Item" />
  <button @onclick="@AddItem">Add</button>

  @code {
      private string _newItemName;

      private async Task AddItem()
      {
          var addItem = new TodoItem { Name = _newItemName, IsComplete = false };
          await Http.PostJsonAsync("api/TodoItems", addItem);
      }
  }
  ```

* <span data-ttu-id="128d6-140">`PutJsonAsync` &ndash; отправляет запрос HTTP POST, включая содержимое в кодировке JSON.</span><span class="sxs-lookup"><span data-stu-id="128d6-140">`PutJsonAsync` &ndash; Sends an HTTP PUT request, including JSON-encoded content.</span></span>

  <span data-ttu-id="128d6-141">В следующем коде `_editItem` значения для `Name` и `IsCompleted` предоставляются связанными элементами компонента.</span><span class="sxs-lookup"><span data-stu-id="128d6-141">In the following code, `_editItem` values for `Name` and `IsCompleted` are provided by bound elements of the component.</span></span> <span data-ttu-id="128d6-142">`Id` элемента задается, когда элемент выбирается в другой части пользовательского интерфейса и вызывается `EditItem`.</span><span class="sxs-lookup"><span data-stu-id="128d6-142">The item's `Id` is set when the item is selected in another part of the UI and `EditItem` is called.</span></span> <span data-ttu-id="128d6-143">Метод `SaveItem` активируется путем выбора элемента сохранить `<button>`.</span><span class="sxs-lookup"><span data-stu-id="128d6-143">The `SaveItem` method is triggered by selecting the Save `<button>` element.</span></span> <span data-ttu-id="128d6-144">Полный пример см. в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="128d6-144">See the sample app for a complete example.</span></span>

  ```razor
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
          await Http.PutJsonAsync($"api/TodoItems/{_editItem.Id}, _editItem);
  }
  ```

<span data-ttu-id="128d6-145"><xref:System.Net.Http> включает дополнительные методы расширения для отправки HTTP-запросов и получения HTTP-ответов.</span><span class="sxs-lookup"><span data-stu-id="128d6-145"><xref:System.Net.Http> includes additional extension methods for sending HTTP requests and receiving HTTP responses.</span></span> <span data-ttu-id="128d6-146">[HttpClient. DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) используется для отправки запроса HTTP DELETE в веб-интерфейс API.</span><span class="sxs-lookup"><span data-stu-id="128d6-146">[HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) is used to send an HTTP DELETE request to a web API.</span></span>

<span data-ttu-id="128d6-147">В следующем коде элемент Delete `<button>` вызывает метод `DeleteItem`.</span><span class="sxs-lookup"><span data-stu-id="128d6-147">In the following code, the Delete `<button>` element calls the `DeleteItem` method.</span></span> <span data-ttu-id="128d6-148">Связанный элемент `<input>` предоставляет `id` удаляемого элемента.</span><span class="sxs-lookup"><span data-stu-id="128d6-148">The bound `<input>` element supplies the `id` of the item to delete.</span></span> <span data-ttu-id="128d6-149">Полный пример см. в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="128d6-149">See the sample app for a complete example.</span></span>

```razor
@using System.Net.Http
@inject HttpClient Http

<input @bind="_id" />
<button @onclick="@DeleteItem">Delete</button>

@code {
    private long _id;

    private async Task DeleteItem() =>
        await Http.DeleteAsync($"api/TodoItems/{_id}");
}
```

## <a name="cross-origin-resource-sharing-cors"></a><span data-ttu-id="128d6-150">Общий доступ к ресурсам между источниками (CORS)</span><span class="sxs-lookup"><span data-stu-id="128d6-150">Cross-origin resource sharing (CORS)</span></span>

<span data-ttu-id="128d6-151">Безопасность в браузере предотвращает запросы веб-страницы к другому домену, отличному от того, который обслуживает веб-страницу.</span><span class="sxs-lookup"><span data-stu-id="128d6-151">Browser security prevents a webpage from making requests to a different domain than the one that served the webpage.</span></span> <span data-ttu-id="128d6-152">Это ограничение называется *политикой того же происхождения*.</span><span class="sxs-lookup"><span data-stu-id="128d6-152">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="128d6-153">Политика того же источника не позволит вредоносному сайту считывать конфиденциальные данные с другого сайта.</span><span class="sxs-lookup"><span data-stu-id="128d6-153">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="128d6-154">Чтобы сделать запросы от браузера к конечной точке с другим источником, *Конечная точка* должна включить [общий доступ к ресурсам в разных источниках (CORS)](https://www.w3.org/TR/cors/).</span><span class="sxs-lookup"><span data-stu-id="128d6-154">To make requests from the browser to an endpoint with a different origin, the *endpoint* must enable [cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/).</span></span>

<span data-ttu-id="128d6-155">[Пример приложенияBlazor веб-сборки (блазорвебассемблисампле)](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ДЕМОНСТРИРУЕТ использование CORS в компоненте Call Web API (*pages/каллвебапи. Razor*).</span><span class="sxs-lookup"><span data-stu-id="128d6-155">The [Blazor WebAssembly sample app (BlazorWebAssemblySample)](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) demonstrates the use of CORS in the Call Web API component (*Pages/CallWebAPI.razor*).</span></span>

<span data-ttu-id="128d6-156">Сведения о том, как разрешить другим сайтам выполнять запросы на общий доступ к ресурсам на основе источника (CORS) в приложении, см. в разделе <xref:security/cors>.</span><span class="sxs-lookup"><span data-stu-id="128d6-156">To allow other sites to make cross-origin resource sharing (CORS) requests to your app, see <xref:security/cors>.</span></span>

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a><span data-ttu-id="128d6-157">HttpClient и HttpRequestMessage с параметрами запроса API FETCH</span><span class="sxs-lookup"><span data-stu-id="128d6-157">HttpClient and HttpRequestMessage with Fetch API request options</span></span>

<span data-ttu-id="128d6-158">При запуске в сборке в Blazor приложении сборки используйте [HttpClient](xref:fundamentals/http-requests) и <xref:System.Net.Http.HttpRequestMessage> для настройки запросов.</span><span class="sxs-lookup"><span data-stu-id="128d6-158">When running on WebAssembly in a Blazor WebAssembly app, use [HttpClient](xref:fundamentals/http-requests) and <xref:System.Net.Http.HttpRequestMessage> to customize requests.</span></span> <span data-ttu-id="128d6-159">Например, можно указать URI запроса, метод HTTP и все нужные заголовки запроса.</span><span class="sxs-lookup"><span data-stu-id="128d6-159">For example, you can specify the request URI, HTTP method, and any desired request headers.</span></span>

```razor
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
            RequestUri = new Uri("https://localhost:10000/api/TodoItems"),
            Content = 
                new StringContent(
                    @"{""name"":""A New Todo Item"",""isComplete"":false}")
        };

        requestMessage.Content.Headers.ContentType = 
            new System.Net.Http.Headers.MediaTypeHeaderValue(
                "application/json");

        requestMessage.Content.Headers.TryAddWithoutValidation(
            "x-custom-header", "value");

        var response = await Http.SendAsync(requestMessage);
        var responseStatusCode = response.StatusCode;
        var responseBody = await response.Content.ReadAsStringAsync();
    }
}
```

<span data-ttu-id="128d6-160">Дополнительные сведения о возможностях API-получения см. в разделе [MDN Web документация: виндоворворкерглобалскопе. fetch ():P араметерс](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span><span class="sxs-lookup"><span data-stu-id="128d6-160">For more information on Fetch API options, see [MDN web docs: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span></span>

<span data-ttu-id="128d6-161">При отправке учетных данных (файлов cookie или заголовков авторизации) в запросах CORS этот заголовок `Authorization` должен быть разрешен политикой CORS.</span><span class="sxs-lookup"><span data-stu-id="128d6-161">When sending credentials (authorization cookies/headers) on CORS requests, the `Authorization` header must be allowed by the CORS policy.</span></span>

<span data-ttu-id="128d6-162">Следующая политика включает в себя настройку для:</span><span class="sxs-lookup"><span data-stu-id="128d6-162">The following policy includes configuration for:</span></span>

* <span data-ttu-id="128d6-163">Источники запроса (`http://localhost:5000`, `https://localhost:5001`).</span><span class="sxs-lookup"><span data-stu-id="128d6-163">Request origins (`http://localhost:5000`, `https://localhost:5001`).</span></span>
* <span data-ttu-id="128d6-164">Любой метод (глагол).</span><span class="sxs-lookup"><span data-stu-id="128d6-164">Any method (verb).</span></span>
* <span data-ttu-id="128d6-165">заголовки `Content-Type` и `Authorization`.</span><span class="sxs-lookup"><span data-stu-id="128d6-165">`Content-Type` and `Authorization` headers.</span></span> <span data-ttu-id="128d6-166">Чтобы разрешить пользовательский заголовок (например, `x-custom-header`), выведите список заголовков при вызове <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span><span class="sxs-lookup"><span data-stu-id="128d6-166">To allow a custom header (for example, `x-custom-header`), list the header when calling <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span></span>
* <span data-ttu-id="128d6-167">Учетные данные, установленные клиентским кодом JavaScript (для свойства`credentials` задано значение `include`).</span><span class="sxs-lookup"><span data-stu-id="128d6-167">Credentials set by client-side JavaScript code (`credentials` property set to `include`).</span></span>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

<span data-ttu-id="128d6-168">Дополнительные сведения см. в разделе <xref:security/cors> и компонент-инженер по запросу HTTP примера приложения (*Components/хттпрекуесттестер. Razor*).</span><span class="sxs-lookup"><span data-stu-id="128d6-168">For more information, see <xref:security/cors> and the sample app's HTTP Request Tester component (*Components/HTTPRequestTester.razor*).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="128d6-169">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="128d6-169">Additional resources</span></span>

* <xref:fundamentals/http-requests>
* <xref:security/enforcing-ssl>
* [<span data-ttu-id="128d6-170">Конфигурация конечной точки HTTPS Kestrel</span><span class="sxs-lookup"><span data-stu-id="128d6-170">Kestrel HTTPS endpoint configuration</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [<span data-ttu-id="128d6-171">Общий доступ к ресурсам в разных источниках (CORS) в консорциуме W3C</span><span class="sxs-lookup"><span data-stu-id="128d6-171">Cross Origin Resource Sharing (CORS) at W3C</span></span>](https://www.w3.org/TR/cors/)
