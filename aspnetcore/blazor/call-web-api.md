---
title: Вызов веб-API из ASP.NET Core Blazor
author: guardrex
description: Узнайте, как вызывать веб-API из приложения Blazor с помощью вспомогательных методов JSON, включая создание запросов на общий доступ к ресурсам независимо от источника (CORS).
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/22/2020
no-loc:
- Blazor
- SignalR
uid: blazor/call-web-api
ms.openlocfilehash: e6996f0e6731b05038d0a9329152b8afd5f6796d
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78647734"
---
# <a name="call-a-web-api-from-aspnet-core-opno-locblazor"></a><span data-ttu-id="f587a-103">Вызов веб-API из ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="f587a-103">Call a web API from ASP.NET Core Blazor</span></span>

<span data-ttu-id="f587a-104">Авторы: [Люк Латэм (Luke Latham)](https://github.com/guardrex), [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Хуан Де ла Круз (Juan De la Cruz)](https://github.com/juandelacruz23)</span><span class="sxs-lookup"><span data-stu-id="f587a-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Juan De la Cruz](https://github.com/juandelacruz23)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="f587a-105">[Приложения Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) вызывают веб-API с помощью предварительно настроенной службы `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f587a-105">[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) apps call web APIs using a preconfigured `HttpClient` service.</span></span> <span data-ttu-id="f587a-106">Составляйте запросы, которые могут включать параметры JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API), используя вспомогательные методы JSON Blazor или с помощью <xref:System.Net.Http.HttpRequestMessage>.</span><span class="sxs-lookup"><span data-stu-id="f587a-106">Compose requests, which can include JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) options, using Blazor JSON helpers or with <xref:System.Net.Http.HttpRequestMessage>.</span></span> <span data-ttu-id="f587a-107">Служба `HttpClient` в приложениях Blazor WebAssembly направляет запросы обратно к серверу-источнику.</span><span class="sxs-lookup"><span data-stu-id="f587a-107">The `HttpClient` service in Blazor WebAssembly apps is focused on making requests back to the server of origin.</span></span> <span data-ttu-id="f587a-108">Рекомендации в этом разделе относятся только к приложениям Blazor WebAssembly.</span><span class="sxs-lookup"><span data-stu-id="f587a-108">The guidance in this topic only pertains to Blazor WebAssembly apps.</span></span>

<span data-ttu-id="f587a-109">[Приложения Blazor Server](xref:blazor/hosting-models#blazor-server) вызывают веб-API с помощью экземпляров <xref:System.Net.Http.HttpClient>, обычно созданных с помощью <xref:System.Net.Http.IHttpClientFactory>.</span><span class="sxs-lookup"><span data-stu-id="f587a-109">[Blazor Server](xref:blazor/hosting-models#blazor-server) apps call web APIs using <xref:System.Net.Http.HttpClient> instances, typically created using <xref:System.Net.Http.IHttpClientFactory>.</span></span> <span data-ttu-id="f587a-110">Рекомендации в этом разделе не относятся к приложениям Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="f587a-110">The guidance in this topic doesn't pertain to Blazor Server apps.</span></span> <span data-ttu-id="f587a-111">При разработке приложений Blazor Server следуйте указаниям в <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="f587a-111">When developing Blazor Server apps, follow the guidance in <xref:fundamentals/http-requests>.</span></span>

<span data-ttu-id="f587a-112">[Просмотрите или загрузите образец кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([сведения о загрузке](xref:index#how-to-download-a-sample)) &ndash; Выберите приложение *BlazorWebAssemblySample*.</span><span class="sxs-lookup"><span data-stu-id="f587a-112">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample)) &ndash; Select the *BlazorWebAssemblySample* app.</span></span>

<span data-ttu-id="f587a-113">См. следующие компоненты в примере приложения *BlazorWebAssemblySample*:</span><span class="sxs-lookup"><span data-stu-id="f587a-113">See the following components in the *BlazorWebAssemblySample* sample app:</span></span>

* <span data-ttu-id="f587a-114">Вызов веб-API (*Pages/CallWebAPI.razor*)</span><span class="sxs-lookup"><span data-stu-id="f587a-114">Call Web API (*Pages/CallWebAPI.razor*)</span></span>
* <span data-ttu-id="f587a-115">Тестер HTTP-запросов (*Components/HTTPRequestTester.razor*)</span><span class="sxs-lookup"><span data-stu-id="f587a-115">HTTP Request Tester (*Components/HTTPRequestTester.razor*)</span></span>

## <a name="packages"></a><span data-ttu-id="f587a-116">Пакеты</span><span class="sxs-lookup"><span data-stu-id="f587a-116">Packages</span></span>

<span data-ttu-id="f587a-117">Используйте *экспериментальный* пакет NuGet [Microsoft.AspNetCore.Blazor.HttpClient](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.HttpClient/) в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="f587a-117">Reference the *experimental* [Microsoft.AspNetCore.Blazor.HttpClient](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.HttpClient/) NuGet package in the project file.</span></span> <span data-ttu-id="f587a-118">`Microsoft.AspNetCore.Blazor.HttpClient` основывается на `HttpClient` и [System.Text.Json](https://www.nuget.org/packages/System.Text.Json/).</span><span class="sxs-lookup"><span data-stu-id="f587a-118">`Microsoft.AspNetCore.Blazor.HttpClient` is based on `HttpClient` and [System.Text.Json](https://www.nuget.org/packages/System.Text.Json/).</span></span>

<span data-ttu-id="f587a-119">Чтобы использовать стабильный API, используйте пакет [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/), который использует [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/)/[Json.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="f587a-119">To use a stable API, use the [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) package, which uses [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/)/[Json.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span> <span data-ttu-id="f587a-120">Использование стабильного API в `Microsoft.AspNet.WebApi.Client` не предоставляет вспомогательные методы JSON, описанные в этом разделе, которые являются уникальными для экспериментального пакета `Microsoft.AspNetCore.Blazor.HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f587a-120">Using the stable API in `Microsoft.AspNet.WebApi.Client` doesn't provide the JSON helpers described in this topic, which are unique to the experimental `Microsoft.AspNetCore.Blazor.HttpClient` package.</span></span>

## <a name="httpclient-and-json-helpers"></a><span data-ttu-id="f587a-121">HttpClient и вспомогательные методы JSON</span><span class="sxs-lookup"><span data-stu-id="f587a-121">HttpClient and JSON helpers</span></span>

<span data-ttu-id="f587a-122">В приложении Blazor WebAssembly [HttpClient](xref:fundamentals/http-requests) доступен в качестве предварительно настроенной службы для отправки запросов обратно к серверу-источнику.</span><span class="sxs-lookup"><span data-stu-id="f587a-122">In a Blazor WebAssembly app, [HttpClient](xref:fundamentals/http-requests) is available as a preconfigured service for making requests back to the origin server.</span></span>

<span data-ttu-id="f587a-123">Приложение Blazor Server не включает службу `HttpClient` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f587a-123">A Blazor Server app doesn't include an `HttpClient` service by default.</span></span> <span data-ttu-id="f587a-124">Предоставьте `HttpClient` для приложения с помощью [инфраструктуры фабрики HttpClient](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="f587a-124">Provide an `HttpClient` to the app using the [HttpClient factory infrastructure](xref:fundamentals/http-requests).</span></span>

<span data-ttu-id="f587a-125">`HttpClient` и вспомогательные методы JSON также используются для вызова сторонних конечных точек веб-API.</span><span class="sxs-lookup"><span data-stu-id="f587a-125">`HttpClient` and JSON helpers are also used to call third-party web API endpoints.</span></span> <span data-ttu-id="f587a-126">`HttpClient` реализуется с помощью [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) браузера и зависит от его ограничений, включая принудительное применение той же политики источника.</span><span class="sxs-lookup"><span data-stu-id="f587a-126">`HttpClient` is implemented using the browser [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) and is subject to its limitations, including enforcement of the same origin policy.</span></span>

<span data-ttu-id="f587a-127">Базовый адрес клиента устанавливается как адрес сервера-источника.</span><span class="sxs-lookup"><span data-stu-id="f587a-127">The client's base address is set to the originating server's address.</span></span> <span data-ttu-id="f587a-128">Внедрите экземпляр `HttpClient` с помощью директивы `@inject`:</span><span class="sxs-lookup"><span data-stu-id="f587a-128">Inject an `HttpClient` instance using the `@inject` directive:</span></span>

```razor
@using System.Net.Http
@inject HttpClient Http
```

<span data-ttu-id="f587a-129">В следующих примерах веб-API Todo обрабатывает операции создания, чтения, обновления и удаления (CRUD).</span><span class="sxs-lookup"><span data-stu-id="f587a-129">In the following examples, a Todo web API processes create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="f587a-130">Примеры основаны на классе `TodoItem`, в котором хранится:</span><span class="sxs-lookup"><span data-stu-id="f587a-130">The examples are based on a `TodoItem` class that stores the:</span></span>

* <span data-ttu-id="f587a-131">Идентификатор (`Id`, `long`) — уникальный идентификатор элемента.</span><span class="sxs-lookup"><span data-stu-id="f587a-131">ID (`Id`, `long`) &ndash; Unique ID of the item.</span></span>
* <span data-ttu-id="f587a-132">Имя (`Name`, `string`) — имя элемента.</span><span class="sxs-lookup"><span data-stu-id="f587a-132">Name (`Name`, `string`) &ndash; Name of the item.</span></span>
* <span data-ttu-id="f587a-133">Состояние (`IsComplete`, `bool`) — указание того, завершен ли элемент Todo.</span><span class="sxs-lookup"><span data-stu-id="f587a-133">Status (`IsComplete`, `bool`) &ndash; Indication if the Todo item is finished.</span></span>

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

<span data-ttu-id="f587a-134">Вспомогательные методы JSON отправляют запросы к URI (веб-API в следующих примерах) и обрабатывают ответ:</span><span class="sxs-lookup"><span data-stu-id="f587a-134">JSON helper methods send requests to a URI (a web API in the following examples) and process the response:</span></span>

* <span data-ttu-id="f587a-135">`GetJsonAsync` &ndash; Отправляет запрос HTTP GET и анализирует текст ответа JSON для создания объекта.</span><span class="sxs-lookup"><span data-stu-id="f587a-135">`GetJsonAsync` &ndash; Sends an HTTP GET request and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="f587a-136">В следующем коде `_todoItems` отображаются компонентом.</span><span class="sxs-lookup"><span data-stu-id="f587a-136">In the following code, the `_todoItems` are displayed by the component.</span></span> <span data-ttu-id="f587a-137">Метод `GetTodoItems` срабатывает после завершения подготовки компонента к просмотру ([OnInitializedAsync](xref:blazor/lifecycle#component-initialization-methods)).</span><span class="sxs-lookup"><span data-stu-id="f587a-137">The `GetTodoItems` method is triggered when the component is finished rendering ([OnInitializedAsync](xref:blazor/lifecycle#component-initialization-methods)).</span></span> <span data-ttu-id="f587a-138">Полный пример см. в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="f587a-138">See the sample app for a complete example.</span></span>

  ```razor
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/TodoItems");
  }
  ```

* <span data-ttu-id="f587a-139">`PostJsonAsync` &ndash; Отправляет запрос HTTP POST, включая содержимое в кодировке JSON, и анализирует текст ответа JSON для создания объекта.</span><span class="sxs-lookup"><span data-stu-id="f587a-139">`PostJsonAsync` &ndash; Sends an HTTP POST request, including JSON-encoded content, and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="f587a-140">В следующем коде `_newItemName` предоставляется связанным элементом компонента.</span><span class="sxs-lookup"><span data-stu-id="f587a-140">In the following code, `_newItemName` is provided by a bound element of the component.</span></span> <span data-ttu-id="f587a-141">Метод `AddItem` активируется путем выбора элемента `<button>`.</span><span class="sxs-lookup"><span data-stu-id="f587a-141">The `AddItem` method is triggered by selecting a `<button>` element.</span></span> <span data-ttu-id="f587a-142">Полный пример см. в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="f587a-142">See the sample app for a complete example.</span></span>

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

* <span data-ttu-id="f587a-143">`PutJsonAsync` &ndash; Отправляет запрос HTTP PUT, включая содержимое в кодировке JSON.</span><span class="sxs-lookup"><span data-stu-id="f587a-143">`PutJsonAsync` &ndash; Sends an HTTP PUT request, including JSON-encoded content.</span></span>

  <span data-ttu-id="f587a-144">В следующем коде значения `_editItem` для `Name` и `IsCompleted` предоставляются связанными элементами компонента.</span><span class="sxs-lookup"><span data-stu-id="f587a-144">In the following code, `_editItem` values for `Name` and `IsCompleted` are provided by bound elements of the component.</span></span> <span data-ttu-id="f587a-145">Элемент `Id` задается, когда элемент выбирается в другой части пользовательского интерфейса и вызывается `EditItem`.</span><span class="sxs-lookup"><span data-stu-id="f587a-145">The item's `Id` is set when the item is selected in another part of the UI and `EditItem` is called.</span></span> <span data-ttu-id="f587a-146">Метод `SaveItem` активируется путем выбора элемента `<button>` "Save".</span><span class="sxs-lookup"><span data-stu-id="f587a-146">The `SaveItem` method is triggered by selecting the Save `<button>` element.</span></span> <span data-ttu-id="f587a-147">Полный пример см. в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="f587a-147">See the sample app for a complete example.</span></span>

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

<span data-ttu-id="f587a-148"><xref:System.Net.Http> включает дополнительные методы расширения для отправки HTTP-запросов и получения HTTP-ответов.</span><span class="sxs-lookup"><span data-stu-id="f587a-148"><xref:System.Net.Http> includes additional extension methods for sending HTTP requests and receiving HTTP responses.</span></span> <span data-ttu-id="f587a-149">[HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) используется для отправки запроса HTTP DELETE в веб-интерфейс API.</span><span class="sxs-lookup"><span data-stu-id="f587a-149">[HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) is used to send an HTTP DELETE request to a web API.</span></span>

<span data-ttu-id="f587a-150">В следующем коде элемент `<button>` "Delete" вызывает метод `DeleteItem`.</span><span class="sxs-lookup"><span data-stu-id="f587a-150">In the following code, the Delete `<button>` element calls the `DeleteItem` method.</span></span> <span data-ttu-id="f587a-151">Связанный элемент `<input>` предоставляет `id` удаляемого элемента.</span><span class="sxs-lookup"><span data-stu-id="f587a-151">The bound `<input>` element supplies the `id` of the item to delete.</span></span> <span data-ttu-id="f587a-152">Полный пример см. в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="f587a-152">See the sample app for a complete example.</span></span>

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

## <a name="cross-origin-resource-sharing-cors"></a><span data-ttu-id="f587a-153">Общий доступ к ресурсам независимо от источника (CORS)</span><span class="sxs-lookup"><span data-stu-id="f587a-153">Cross-origin resource sharing (CORS)</span></span>

<span data-ttu-id="f587a-154">Безопасность в браузере предотвращает запросы веб-страницы к другому домену, отличному от того, который обслуживает веб-страницу.</span><span class="sxs-lookup"><span data-stu-id="f587a-154">Browser security prevents a webpage from making requests to a different domain than the one that served the webpage.</span></span> <span data-ttu-id="f587a-155">Это ограничение называется *политика одного источника*.</span><span class="sxs-lookup"><span data-stu-id="f587a-155">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="f587a-156">Эта политика предотвращает чтение вредоносным сайтом конфиденциальных данных с другого сайта.</span><span class="sxs-lookup"><span data-stu-id="f587a-156">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="f587a-157">Для выполнения запросов из браузера к конечной точке с другим источником *конечная точка* должна включать [общий доступ к ресурсам независимо от источника (CORS)](https://www.w3.org/TR/cors/).</span><span class="sxs-lookup"><span data-stu-id="f587a-157">To make requests from the browser to an endpoint with a different origin, the *endpoint* must enable [cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/).</span></span>

<span data-ttu-id="f587a-158">В [примере приложения Blazor WebAssembly (BlazorWebAssemblySample)](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) показано использование CORS в компоненте вызова веб-API (*Pages/CallWebAPI.razor*).</span><span class="sxs-lookup"><span data-stu-id="f587a-158">The [Blazor WebAssembly sample app (BlazorWebAssemblySample)](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) demonstrates the use of CORS in the Call Web API component (*Pages/CallWebAPI.razor*).</span></span>

<span data-ttu-id="f587a-159">Сведения о том, как разрешить другим сайтам выполнять запросы на общий доступ к ресурсам независимо от источника (CORS) к приложению, см. в разделе <xref:security/cors>.</span><span class="sxs-lookup"><span data-stu-id="f587a-159">To allow other sites to make cross-origin resource sharing (CORS) requests to your app, see <xref:security/cors>.</span></span>

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a><span data-ttu-id="f587a-160">HttpClient и HttpRequestMessage с параметрами запроса Fetch API</span><span class="sxs-lookup"><span data-stu-id="f587a-160">HttpClient and HttpRequestMessage with Fetch API request options</span></span>

<span data-ttu-id="f587a-161">При выполнении в WebAssembly в приложении Blazor WebAssembly используйте [HttpClient](xref:fundamentals/http-requests) и <xref:System.Net.Http.HttpRequestMessage> для настройки запросов.</span><span class="sxs-lookup"><span data-stu-id="f587a-161">When running on WebAssembly in a Blazor WebAssembly app, use [HttpClient](xref:fundamentals/http-requests) and <xref:System.Net.Http.HttpRequestMessage> to customize requests.</span></span> <span data-ttu-id="f587a-162">Например, можно указать URI запроса, метод HTTP и все нужные заголовки запроса.</span><span class="sxs-lookup"><span data-stu-id="f587a-162">For example, you can specify the request URI, HTTP method, and any desired request headers.</span></span>

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

<span data-ttu-id="f587a-163">Дополнительные сведения о возможностях Fetch API см. в разделе [Веб-документы MDN: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span><span class="sxs-lookup"><span data-stu-id="f587a-163">For more information on Fetch API options, see [MDN web docs: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span></span>

<span data-ttu-id="f587a-164">При отправке учетных данных (файлов cookie или заголовков авторизации) в запросах CORS заголовок `Authorization` должен быть разрешен политикой CORS.</span><span class="sxs-lookup"><span data-stu-id="f587a-164">When sending credentials (authorization cookies/headers) on CORS requests, the `Authorization` header must be allowed by the CORS policy.</span></span>

<span data-ttu-id="f587a-165">Следующая политика включает в себя настройку для следующих элементов:</span><span class="sxs-lookup"><span data-stu-id="f587a-165">The following policy includes configuration for:</span></span>

* <span data-ttu-id="f587a-166">Источники запроса (`http://localhost:5000`, `https://localhost:5001`).</span><span class="sxs-lookup"><span data-stu-id="f587a-166">Request origins (`http://localhost:5000`, `https://localhost:5001`).</span></span>
* <span data-ttu-id="f587a-167">Любой метод (команда).</span><span class="sxs-lookup"><span data-stu-id="f587a-167">Any method (verb).</span></span>
* <span data-ttu-id="f587a-168">Заголовки `Content-Type` и `Authorization`.</span><span class="sxs-lookup"><span data-stu-id="f587a-168">`Content-Type` and `Authorization` headers.</span></span> <span data-ttu-id="f587a-169">Чтобы разрешить пользовательский заголовок (например, `x-custom-header`), выведите список заголовков при вызове <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span><span class="sxs-lookup"><span data-stu-id="f587a-169">To allow a custom header (for example, `x-custom-header`), list the header when calling <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span></span>
* <span data-ttu-id="f587a-170">Учетные данные, установленные клиентским кодом JavaScript (для свойства `credentials` задано значение `include`).</span><span class="sxs-lookup"><span data-stu-id="f587a-170">Credentials set by client-side JavaScript code (`credentials` property set to `include`).</span></span>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

<span data-ttu-id="f587a-171">Дополнительные сведения см. в разделе <xref:security/cors> и компоненте "Тестер HTTP-запросов" примера приложения (*Components/HTTPRequestTester.razor*).</span><span class="sxs-lookup"><span data-stu-id="f587a-171">For more information, see <xref:security/cors> and the sample app's HTTP Request Tester component (*Components/HTTPRequestTester.razor*).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f587a-172">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="f587a-172">Additional resources</span></span>

* <xref:fundamentals/http-requests>
* <xref:security/enforcing-ssl>
* [<span data-ttu-id="f587a-173">Конфигурация конечных точек HTTPS Kestrel</span><span class="sxs-lookup"><span data-stu-id="f587a-173">Kestrel HTTPS endpoint configuration</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [<span data-ttu-id="f587a-174">Общий доступ к ресурсам независимо от источника (CORS) в W3C</span><span class="sxs-lookup"><span data-stu-id="f587a-174">Cross Origin Resource Sharing (CORS) at W3C</span></span>](https://www.w3.org/TR/cors/)
