---
title: Вызов веб-API из ASP.NET Core Blazor
author: guardrex
description: Узнайте, как вызывать веб-API из Blazor приложения с помощью вспомогательных средств JSON, включая создание запросов на общий доступ к ресурсам в разных источниках (CORS).
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/22/2020
no-loc:
- Blazor
- SignalR
uid: blazor/call-web-api
ms.openlocfilehash: 345fb6962e3376c22551eb7914c70c89cb7100d5
ms.sourcegitcommit: d2ba66023884f0dca115ff010bd98d5ed6459283
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2020
ms.locfileid: "77213279"
---
# <a name="call-a-web-api-from-aspnet-core-opno-locblazor"></a><span data-ttu-id="e3cbb-103">Вызов веб-API из ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="e3cbb-103">Call a web API from ASP.NET Core Blazor</span></span>

<span data-ttu-id="e3cbb-104">[Люк ЛаСаМ](https://github.com/guardrex), [Даниэль Roth)](https://github.com/danroth27)и самое [Нела Круз](https://github.com/juandelacruz23)</span><span class="sxs-lookup"><span data-stu-id="e3cbb-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Juan De la Cruz](https://github.com/juandelacruz23)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="e3cbb-105">[Blazor приложения сборки](xref:blazor/hosting-models#blazor-webassembly) вызывают веб-API с помощью предварительно настроенной службы `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-105">[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) apps call web APIs using a preconfigured `HttpClient` service.</span></span> <span data-ttu-id="e3cbb-106">Запросы на создание, которые могут включать параметры [API-интерфейса получения](https://developer.mozilla.org/docs/Web/API/Fetch_API) JavaScript, с помощью Blazor вспомогательных функций JSON или <xref:System.Net.Http.HttpRequestMessage>.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-106">Compose requests, which can include JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) options, using Blazor JSON helpers or with <xref:System.Net.Http.HttpRequestMessage>.</span></span> <span data-ttu-id="e3cbb-107">Служба `HttpClient` в Blazor приложениях веб-сборки посвящена выполнению запросов к серверу происхождения.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-107">The `HttpClient` service in Blazor WebAssembly apps is focused on making requests back to the server of origin.</span></span> <span data-ttu-id="e3cbb-108">Рекомендации в этом разделе относятся только к Blazorным приложениям веб – сборки.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-108">The guidance in this topic only pertains to Blazor WebAssembly apps.</span></span>

<span data-ttu-id="e3cbb-109">[Blazor серверные](xref:blazor/hosting-models#blazor-server) приложения вызывают веб-API с помощью <xref:System.Net.Http.HttpClient> экземпляров, обычно созданных с помощью <xref:System.Net.Http.IHttpClientFactory>.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-109">[Blazor Server](xref:blazor/hosting-models#blazor-server) apps call web APIs using <xref:System.Net.Http.HttpClient> instances, typically created using <xref:System.Net.Http.IHttpClientFactory>.</span></span> <span data-ttu-id="e3cbb-110">Рекомендации в этом разделе не относятся к Blazorным серверным приложениям.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-110">The guidance in this topic doesn't pertain to Blazor Server apps.</span></span> <span data-ttu-id="e3cbb-111">При разработке приложений Blazor Server следуйте указаниям в <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-111">When developing Blazor Server apps, follow the guidance in <xref:fundamentals/http-requests>.</span></span>

<span data-ttu-id="e3cbb-112">[Просмотрите или Скачайте образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([как скачать](xref:index#how-to-download-a-sample)) &ndash; выберите приложение *блазорвебассемблисампле* .</span><span class="sxs-lookup"><span data-stu-id="e3cbb-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample)) &ndash; Select the *BlazorWebAssemblySample* app.</span></span>

<span data-ttu-id="e3cbb-113">См. следующие компоненты в примере приложения *блазорвебассемблисампле* :</span><span class="sxs-lookup"><span data-stu-id="e3cbb-113">See the following components in the *BlazorWebAssemblySample* sample app:</span></span>

* <span data-ttu-id="e3cbb-114">Вызов веб-API (*pages/каллвебапи. Razor*)</span><span class="sxs-lookup"><span data-stu-id="e3cbb-114">Call Web API (*Pages/CallWebAPI.razor*)</span></span>
* <span data-ttu-id="e3cbb-115">Тестер HTTP-запросов (*Components/хттпрекуесттестер. Razor*)</span><span class="sxs-lookup"><span data-stu-id="e3cbb-115">HTTP Request Tester (*Components/HTTPRequestTester.razor*)</span></span>

## <a name="packages"></a><span data-ttu-id="e3cbb-116">Пакеты</span><span class="sxs-lookup"><span data-stu-id="e3cbb-116">Packages</span></span>

<span data-ttu-id="e3cbb-117">Сослаться на *экспериментальный* [Microsoft.AspNetCore.Blazor.HttpClient](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.HttpClient/) пакет NuGet в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-117">Reference the *experimental* [Microsoft.AspNetCore.Blazor.HttpClient](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.HttpClient/) NuGet package in the project file.</span></span> <span data-ttu-id="e3cbb-118">`Microsoft.AspNetCore.Blazor.HttpClient` основан на `HttpClient` и [System. Text. JSON](https://www.nuget.org/packages/System.Text.Json/).</span><span class="sxs-lookup"><span data-stu-id="e3cbb-118">`Microsoft.AspNetCore.Blazor.HttpClient` is based on `HttpClient` and [System.Text.Json](https://www.nuget.org/packages/System.Text.Json/).</span></span>

<span data-ttu-id="e3cbb-119">Чтобы использовать стабильный API, используйте пакет [Microsoft. AspNet. WebApi. Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) , использующий [Newtonsoft. JSON](https://www.nuget.org/packages/Newtonsoft.Json/)/[JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="e3cbb-119">To use a stable API, use the [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) package, which uses [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/)/[Json.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span> <span data-ttu-id="e3cbb-120">Использование стабильного API в `Microsoft.AspNet.WebApi.Client` не предоставляет вспомогательные методы JSON, описанные в этом разделе, которые являются уникальными для экспериментального `Microsoft.AspNetCore.Blazor.HttpClient` пакета.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-120">Using the stable API in `Microsoft.AspNet.WebApi.Client` doesn't provide the JSON helpers described in this topic, which are unique to the experimental `Microsoft.AspNetCore.Blazor.HttpClient` package.</span></span>

## <a name="httpclient-and-json-helpers"></a><span data-ttu-id="e3cbb-121">HttpClient и вспомогательные методы JSON</span><span class="sxs-lookup"><span data-stu-id="e3cbb-121">HttpClient and JSON helpers</span></span>

<span data-ttu-id="e3cbb-122">В Blazor приложении [HttpClient](xref:fundamentals/http-requests) доступна как предварительно настроенная служба для выполнения запросов к серверу-источнику.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-122">In a Blazor WebAssembly app, [HttpClient](xref:fundamentals/http-requests) is available as a preconfigured service for making requests back to the origin server.</span></span>

<span data-ttu-id="e3cbb-123">По умолчанию серверное приложение Blazor не включает службу `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-123">A Blazor Server app doesn't include an `HttpClient` service by default.</span></span> <span data-ttu-id="e3cbb-124">Предоставьте `HttpClient` приложению с помощью [инфраструктуры фабрики HttpClient](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="e3cbb-124">Provide an `HttpClient` to the app using the [HttpClient factory infrastructure](xref:fundamentals/http-requests).</span></span>

<span data-ttu-id="e3cbb-125">`HttpClient` и вспомогательные методы JSON также используются для вызова конечных точек веб-API сторонних производителей.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-125">`HttpClient` and JSON helpers are also used to call third-party web API endpoints.</span></span> <span data-ttu-id="e3cbb-126">`HttpClient` реализуется с помощью [API-интерфейса выборки](https://developer.mozilla.org/docs/Web/API/Fetch_API) браузера и подчиняется его ограничениям, включая принудительное применение той же политики происхождения.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-126">`HttpClient` is implemented using the browser [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) and is subject to its limitations, including enforcement of the same origin policy.</span></span>

<span data-ttu-id="e3cbb-127">Базовый адрес клиента устанавливается в адрес исходного сервера.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-127">The client's base address is set to the originating server's address.</span></span> <span data-ttu-id="e3cbb-128">Внедрить `HttpClient` экземпляр с помощью директивы `@inject`:</span><span class="sxs-lookup"><span data-stu-id="e3cbb-128">Inject an `HttpClient` instance using the `@inject` directive:</span></span>

```razor
@using System.Net.Http
@inject HttpClient Http
```

<span data-ttu-id="e3cbb-129">В следующих примерах веб-API TODO обрабатывает операции создания, чтения, обновления и удаления (CRUD).</span><span class="sxs-lookup"><span data-stu-id="e3cbb-129">In the following examples, a Todo web API processes create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="e3cbb-130">Примеры основаны на классе `TodoItem`, в котором хранится:</span><span class="sxs-lookup"><span data-stu-id="e3cbb-130">The examples are based on a `TodoItem` class that stores the:</span></span>

* <span data-ttu-id="e3cbb-131">ID (`Id`, `long`) &ndash; уникальный идентификатор элемента.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-131">ID (`Id`, `long`) &ndash; Unique ID of the item.</span></span>
* <span data-ttu-id="e3cbb-132">Имя (`Name`, `string`) &ndash; имя элемента.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-132">Name (`Name`, `string`) &ndash; Name of the item.</span></span>
* <span data-ttu-id="e3cbb-133">Состояние (`IsComplete`, `bool`) &ndash; указывает, что элемент TODO завершен.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-133">Status (`IsComplete`, `bool`) &ndash; Indication if the Todo item is finished.</span></span>

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

<span data-ttu-id="e3cbb-134">Вспомогательные методы JSON отправляют запросы в URI (веб-API в следующих примерах) и обрабатывают ответ:</span><span class="sxs-lookup"><span data-stu-id="e3cbb-134">JSON helper methods send requests to a URI (a web API in the following examples) and process the response:</span></span>

* <span data-ttu-id="e3cbb-135">`GetJsonAsync` &ndash; отправляет запрос HTTP GET и анализирует текст ответа JSON для создания объекта.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-135">`GetJsonAsync` &ndash; Sends an HTTP GET request and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="e3cbb-136">В следующем коде `_todoItems` отображаются компонентом.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-136">In the following code, the `_todoItems` are displayed by the component.</span></span> <span data-ttu-id="e3cbb-137">Метод `GetTodoItems` активируется при завершении подготовки компонента к просмотру ([онинитиализедасинк](xref:blazor/lifecycle#component-initialization-methods)).</span><span class="sxs-lookup"><span data-stu-id="e3cbb-137">The `GetTodoItems` method is triggered when the component is finished rendering ([OnInitializedAsync](xref:blazor/lifecycle#component-initialization-methods)).</span></span> <span data-ttu-id="e3cbb-138">Полный пример см. в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-138">See the sample app for a complete example.</span></span>

  ```razor
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/TodoItems");
  }
  ```

* <span data-ttu-id="e3cbb-139">`PostJsonAsync` &ndash; отправляет запрос HTTP POST, включая содержимое в кодировке JSON, и анализирует текст ответа JSON для создания объекта.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-139">`PostJsonAsync` &ndash; Sends an HTTP POST request, including JSON-encoded content, and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="e3cbb-140">В следующем коде `_newItemName` предоставляется связанным элементом компонента.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-140">In the following code, `_newItemName` is provided by a bound element of the component.</span></span> <span data-ttu-id="e3cbb-141">Метод `AddItem` активируется путем выбора элемента `<button>`.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-141">The `AddItem` method is triggered by selecting a `<button>` element.</span></span> <span data-ttu-id="e3cbb-142">Полный пример см. в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-142">See the sample app for a complete example.</span></span>

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

* <span data-ttu-id="e3cbb-143">`PutJsonAsync` &ndash; отправляет запрос HTTP POST, включая содержимое в кодировке JSON.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-143">`PutJsonAsync` &ndash; Sends an HTTP PUT request, including JSON-encoded content.</span></span>

  <span data-ttu-id="e3cbb-144">В следующем коде `_editItem` значения для `Name` и `IsCompleted` предоставляются связанными элементами компонента.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-144">In the following code, `_editItem` values for `Name` and `IsCompleted` are provided by bound elements of the component.</span></span> <span data-ttu-id="e3cbb-145">`Id` элемента задается, когда элемент выбирается в другой части пользовательского интерфейса и вызывается `EditItem`.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-145">The item's `Id` is set when the item is selected in another part of the UI and `EditItem` is called.</span></span> <span data-ttu-id="e3cbb-146">Метод `SaveItem` активируется путем выбора элемента сохранить `<button>`.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-146">The `SaveItem` method is triggered by selecting the Save `<button>` element.</span></span> <span data-ttu-id="e3cbb-147">Полный пример см. в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-147">See the sample app for a complete example.</span></span>

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

<span data-ttu-id="e3cbb-148"><xref:System.Net.Http> включает дополнительные методы расширения для отправки HTTP-запросов и получения HTTP-ответов.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-148"><xref:System.Net.Http> includes additional extension methods for sending HTTP requests and receiving HTTP responses.</span></span> <span data-ttu-id="e3cbb-149">[HttpClient. DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) используется для отправки запроса HTTP DELETE в веб-интерфейс API.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-149">[HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) is used to send an HTTP DELETE request to a web API.</span></span>

<span data-ttu-id="e3cbb-150">В следующем коде элемент Delete `<button>` вызывает метод `DeleteItem`.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-150">In the following code, the Delete `<button>` element calls the `DeleteItem` method.</span></span> <span data-ttu-id="e3cbb-151">Связанный элемент `<input>` предоставляет `id` удаляемого элемента.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-151">The bound `<input>` element supplies the `id` of the item to delete.</span></span> <span data-ttu-id="e3cbb-152">Полный пример см. в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-152">See the sample app for a complete example.</span></span>

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

## <a name="cross-origin-resource-sharing-cors"></a><span data-ttu-id="e3cbb-153">Общий доступ к ресурсам между источниками (CORS)</span><span class="sxs-lookup"><span data-stu-id="e3cbb-153">Cross-origin resource sharing (CORS)</span></span>

<span data-ttu-id="e3cbb-154">Безопасность в браузере предотвращает запросы веб-страницы к другому домену, отличному от того, который обслуживает веб-страницу.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-154">Browser security prevents a webpage from making requests to a different domain than the one that served the webpage.</span></span> <span data-ttu-id="e3cbb-155">Это ограничение называется *политикой того же происхождения*.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-155">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="e3cbb-156">Политика того же источника не позволит вредоносному сайту считывать конфиденциальные данные с другого сайта.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-156">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="e3cbb-157">Чтобы сделать запросы от браузера к конечной точке с другим источником, *Конечная точка* должна включить [общий доступ к ресурсам в разных источниках (CORS)](https://www.w3.org/TR/cors/).</span><span class="sxs-lookup"><span data-stu-id="e3cbb-157">To make requests from the browser to an endpoint with a different origin, the *endpoint* must enable [cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/).</span></span>

<span data-ttu-id="e3cbb-158">[Пример приложенияBlazor веб-сборки (блазорвебассемблисампле)](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ДЕМОНСТРИРУЕТ использование CORS в компоненте Call Web API (*pages/каллвебапи. Razor*).</span><span class="sxs-lookup"><span data-stu-id="e3cbb-158">The [Blazor WebAssembly sample app (BlazorWebAssemblySample)](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) demonstrates the use of CORS in the Call Web API component (*Pages/CallWebAPI.razor*).</span></span>

<span data-ttu-id="e3cbb-159">Сведения о том, как разрешить другим сайтам выполнять запросы на общий доступ к ресурсам на основе источника (CORS) в приложении, см. в разделе <xref:security/cors>.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-159">To allow other sites to make cross-origin resource sharing (CORS) requests to your app, see <xref:security/cors>.</span></span>

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a><span data-ttu-id="e3cbb-160">HttpClient и HttpRequestMessage с параметрами запроса API FETCH</span><span class="sxs-lookup"><span data-stu-id="e3cbb-160">HttpClient and HttpRequestMessage with Fetch API request options</span></span>

<span data-ttu-id="e3cbb-161">При запуске в сборке в Blazor приложении сборки используйте [HttpClient](xref:fundamentals/http-requests) и <xref:System.Net.Http.HttpRequestMessage> для настройки запросов.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-161">When running on WebAssembly in a Blazor WebAssembly app, use [HttpClient](xref:fundamentals/http-requests) and <xref:System.Net.Http.HttpRequestMessage> to customize requests.</span></span> <span data-ttu-id="e3cbb-162">Например, можно указать URI запроса, метод HTTP и все нужные заголовки запроса.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-162">For example, you can specify the request URI, HTTP method, and any desired request headers.</span></span>

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

<span data-ttu-id="e3cbb-163">Дополнительные сведения о возможностях API-получения см. в разделе [MDN Web документация: виндоворворкерглобалскопе. fetch ():P араметерс](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span><span class="sxs-lookup"><span data-stu-id="e3cbb-163">For more information on Fetch API options, see [MDN web docs: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span></span>

<span data-ttu-id="e3cbb-164">При отправке учетных данных (файлов cookie или заголовков авторизации) в запросах CORS этот заголовок `Authorization` должен быть разрешен политикой CORS.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-164">When sending credentials (authorization cookies/headers) on CORS requests, the `Authorization` header must be allowed by the CORS policy.</span></span>

<span data-ttu-id="e3cbb-165">Следующая политика включает в себя настройку для:</span><span class="sxs-lookup"><span data-stu-id="e3cbb-165">The following policy includes configuration for:</span></span>

* <span data-ttu-id="e3cbb-166">Источники запроса (`http://localhost:5000`, `https://localhost:5001`).</span><span class="sxs-lookup"><span data-stu-id="e3cbb-166">Request origins (`http://localhost:5000`, `https://localhost:5001`).</span></span>
* <span data-ttu-id="e3cbb-167">Любой метод (глагол).</span><span class="sxs-lookup"><span data-stu-id="e3cbb-167">Any method (verb).</span></span>
* <span data-ttu-id="e3cbb-168">заголовки `Content-Type` и `Authorization`.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-168">`Content-Type` and `Authorization` headers.</span></span> <span data-ttu-id="e3cbb-169">Чтобы разрешить пользовательский заголовок (например, `x-custom-header`), выведите список заголовков при вызове <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span><span class="sxs-lookup"><span data-stu-id="e3cbb-169">To allow a custom header (for example, `x-custom-header`), list the header when calling <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span></span>
* <span data-ttu-id="e3cbb-170">Учетные данные, установленные клиентским кодом JavaScript (для свойства`credentials` задано значение `include`).</span><span class="sxs-lookup"><span data-stu-id="e3cbb-170">Credentials set by client-side JavaScript code (`credentials` property set to `include`).</span></span>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

<span data-ttu-id="e3cbb-171">Дополнительные сведения см. в разделе <xref:security/cors> и компонент-инженер по запросу HTTP примера приложения (*Components/хттпрекуесттестер. Razor*).</span><span class="sxs-lookup"><span data-stu-id="e3cbb-171">For more information, see <xref:security/cors> and the sample app's HTTP Request Tester component (*Components/HTTPRequestTester.razor*).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e3cbb-172">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="e3cbb-172">Additional resources</span></span>

* <xref:fundamentals/http-requests>
* <xref:security/enforcing-ssl>
* [<span data-ttu-id="e3cbb-173">Конфигурация конечной точки HTTPS Kestrel</span><span class="sxs-lookup"><span data-stu-id="e3cbb-173">Kestrel HTTPS endpoint configuration</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [<span data-ttu-id="e3cbb-174">Общий доступ к ресурсам в разных источниках (CORS) в консорциуме W3C</span><span class="sxs-lookup"><span data-stu-id="e3cbb-174">Cross Origin Resource Sharing (CORS) at W3C</span></span>](https://www.w3.org/TR/cors/)
