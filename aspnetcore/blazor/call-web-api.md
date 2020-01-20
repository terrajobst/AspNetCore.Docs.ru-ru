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
# <a name="call-a-web-api-from-aspnet-core-opno-locblazor"></a>Вызов веб-API из ASP.NET Core Blazor

[Люк ЛаСаМ](https://github.com/guardrex), [Даниэль Roth)](https://github.com/danroth27)и самое [Нела Круз](https://github.com/juandelacruz23)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[Blazor приложения сборки](xref:blazor/hosting-models#blazor-webassembly) вызывают веб-API с помощью предварительно настроенной службы `HttpClient`. Запросы на создание, которые могут включать параметры [API-интерфейса получения](https://developer.mozilla.org/docs/Web/API/Fetch_API) JavaScript, с помощью Blazor вспомогательных функций JSON или <xref:System.Net.Http.HttpRequestMessage>.

[Blazor серверные](xref:blazor/hosting-models#blazor-server) приложения вызывают веб-API с помощью экземпляров <xref:System.Net.Http.HttpClient>, обычно созданных с помощью <xref:System.Net.Http.IHttpClientFactory>. Для получения дополнительной информации см. <xref:fundamentals/http-requests>.

[Просмотрите или Скачайте образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([как скачать](xref:index#how-to-download-a-sample)) &ndash; выберите приложение *блазорвебассемблисампле* .

См. следующие компоненты в примере приложения *блазорвебассемблисампле* :

* Вызов веб-API (*pages/каллвебапи. Razor*)
* Тестер HTTP-запросов (*Components/хттпрекуесттестер. Razor*)

## <a name="packages"></a>пакеты,

Сослаться на *экспериментальный* [Microsoft. AspNetCore.Blazor. HttpClient](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.HttpClient/) пакет NuGet в файле проекта. `Microsoft.AspNetCore.Blazor.HttpClient` основан на `HttpClient` и [System. Text. JSON](https://www.nuget.org/packages/System.Text.Json/).

Чтобы использовать стабильный API, используйте пакет [Microsoft. AspNet. WebApi. Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) , использующий [Newtonsoft. JSON](https://www.nuget.org/packages/Newtonsoft.Json/)/[JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm). Использование стабильного API в `Microsoft.AspNet.WebApi.Client` не предоставляет вспомогательные методы JSON, описанные в этом разделе, которые являются уникальными для экспериментального `Microsoft.AspNetCore.Blazor.HttpClient` пакета.

## <a name="httpclient-and-json-helpers"></a>HttpClient и вспомогательные методы JSON

В Blazor приложении [HttpClient](xref:fundamentals/http-requests) доступна как предварительно настроенная служба для выполнения запросов к серверу-источнику.

По умолчанию серверное приложение Blazor не включает службу `HttpClient`. Предоставьте `HttpClient` приложению с помощью [инфраструктуры фабрики HttpClient](xref:fundamentals/http-requests).

`HttpClient` и вспомогательные методы JSON также используются для вызова конечных точек веб-API сторонних производителей. `HttpClient` реализуется с помощью [API-интерфейса выборки](https://developer.mozilla.org/docs/Web/API/Fetch_API) браузера и подчиняется его ограничениям, включая принудительное применение той же политики происхождения.

Базовый адрес клиента устанавливается в адрес исходного сервера. Внедрить `HttpClient` экземпляр с помощью директивы `@inject`:

```razor
@using System.Net.Http
@inject HttpClient Http
```

В следующих примерах веб-API TODO обрабатывает операции создания, чтения, обновления и удаления (CRUD). Примеры основаны на классе `TodoItem`, в котором хранится:

* ID (`Id`, `long`) &ndash; уникальный идентификатор элемента.
* Имя (`Name`, `string`) &ndash; имя элемента.
* Состояние (`IsComplete`, `bool`) &ndash; указывает, что элемент TODO завершен.

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

Вспомогательные методы JSON отправляют запросы в URI (веб-API в следующих примерах) и обрабатывают ответ:

* `GetJsonAsync` &ndash; отправляет запрос HTTP GET и анализирует текст ответа JSON для создания объекта.

  В следующем коде `_todoItems` отображаются компонентом. Метод `GetTodoItems` активируется при завершении подготовки компонента к просмотру ([онинитиализедасинк](xref:blazor/lifecycle#component-initialization-methods)). Полный пример см. в примере приложения.

  ```razor
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/TodoItems");
  }
  ```

* `PostJsonAsync` &ndash; отправляет запрос HTTP POST, включая содержимое в кодировке JSON, и анализирует текст ответа JSON для создания объекта.

  В следующем коде `_newItemName` предоставляется связанным элементом компонента. Метод `AddItem` активируется путем выбора элемента `<button>`. Полный пример см. в примере приложения.

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

* `PutJsonAsync` &ndash; отправляет запрос HTTP POST, включая содержимое в кодировке JSON.

  В следующем коде `_editItem` значения для `Name` и `IsCompleted` предоставляются связанными элементами компонента. `Id` элемента задается, когда элемент выбирается в другой части пользовательского интерфейса и вызывается `EditItem`. Метод `SaveItem` активируется путем выбора элемента сохранить `<button>`. Полный пример см. в примере приложения.

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

<xref:System.Net.Http> включает дополнительные методы расширения для отправки HTTP-запросов и получения HTTP-ответов. [HttpClient. DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) используется для отправки запроса HTTP DELETE в веб-интерфейс API.

В следующем коде элемент Delete `<button>` вызывает метод `DeleteItem`. Связанный элемент `<input>` предоставляет `id` удаляемого элемента. Полный пример см. в примере приложения.

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

## <a name="cross-origin-resource-sharing-cors"></a>Общий доступ к ресурсам между источниками (CORS)

Безопасность в браузере предотвращает запросы веб-страницы к другому домену, отличному от того, который обслуживает веб-страницу. Это ограничение называется *политикой того же происхождения*. Политика того же источника не позволит вредоносному сайту считывать конфиденциальные данные с другого сайта. Чтобы сделать запросы от браузера к конечной точке с другим источником, *Конечная точка* должна включить [общий доступ к ресурсам в разных источниках (CORS)](https://www.w3.org/TR/cors/).

[Пример приложенияBlazor веб-сборки (блазорвебассемблисампле)](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ДЕМОНСТРИРУЕТ использование CORS в компоненте Call Web API (*pages/каллвебапи. Razor*).

Сведения о том, как разрешить другим сайтам выполнять запросы на общий доступ к ресурсам на основе источника (CORS) в приложении, см. в разделе <xref:security/cors>.

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a>HttpClient и HttpRequestMessage с параметрами запроса API FETCH

При запуске в сборке в Blazor приложении сборки используйте [HttpClient](xref:fundamentals/http-requests) и <xref:System.Net.Http.HttpRequestMessage> для настройки запросов. Например, можно указать URI запроса, метод HTTP и все нужные заголовки запроса.

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

Дополнительные сведения о возможностях API-получения см. в разделе [MDN Web документация: виндоворворкерглобалскопе. fetch ():P араметерс](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).

При отправке учетных данных (файлов cookie или заголовков авторизации) в запросах CORS этот заголовок `Authorization` должен быть разрешен политикой CORS.

Следующая политика включает в себя настройку для:

* Источники запроса (`http://localhost:5000`, `https://localhost:5001`).
* Любой метод (глагол).
* заголовки `Content-Type` и `Authorization`. Чтобы разрешить пользовательский заголовок (например, `x-custom-header`), выведите список заголовков при вызове <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.
* Учетные данные, установленные клиентским кодом JavaScript (для свойства`credentials` задано значение `include`).

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

Дополнительные сведения см. в разделе <xref:security/cors> и компонент-инженер по запросу HTTP примера приложения (*Components/хттпрекуесттестер. Razor*).

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/http-requests>
* <xref:security/enforcing-ssl>
* [Конфигурация конечной точки HTTPS Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [Общий доступ к ресурсам в разных источниках (CORS) в консорциуме W3C](https://www.w3.org/TR/cors/)
