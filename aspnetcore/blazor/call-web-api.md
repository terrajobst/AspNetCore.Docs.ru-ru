---
title: Вызов веб-API из ASP.NET Core Блазор
author: guardrex
description: Узнайте, как вызывать веб-API из приложения Блазор с помощью вспомогательных функций JSON, включая создание запросов на общий доступ к ресурсам в разных источниках (CORS).
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/call-web-api
ms.openlocfilehash: 23131ac0357b722e7f229fcfe5dab8590cf34739
ms.sourcegitcommit: e5a74f882c14eaa0e5639ff082355e130559ba83
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/20/2019
ms.locfileid: "71168045"
---
# <a name="call-a-web-api-from-aspnet-core-blazor"></a>Вызов веб-API из ASP.NET Core Блазор

[Люк ЛаСаМ](https://github.com/guardrex) и [Даниэль Roth)](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Блазорные приложения вызывают веб-API с помощью предварительно настроенной `HttpClient` службы. Запросы на создание, которые могут включать параметры [API-интерфейса получения](https://developer.mozilla.org/docs/Web/API/Fetch_API) JavaScript, с помощью вспомогательных <xref:System.Net.Http.HttpRequestMessage>функций JSON блазор или с.

Серверные приложения блазор вызывают веб- <xref:System.Net.Http.HttpClient> API с помощью экземпляров <xref:System.Net.Http.IHttpClientFactory>, обычно созданных с помощью. Дополнительные сведения см. в разделе <xref:fundamentals/http-requests>.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([как скачивать](xref:index#how-to-download-a-sample))

Примеры Блазор см. в следующих компонентах примера приложения:

* Вызов веб-API (*pages/каллвебапи. Razor*)
* Тестер HTTP-запросов (*Components/хттпрекуесттестер. Razor*)

## <a name="httpclient-and-json-helpers"></a>HttpClient и вспомогательные методы JSON

В приложениях Блазор веб-сборки [HttpClient](xref:fundamentals/http-requests) доступна в виде предварительно настроенной службы для выполнения запросов к серверу-источнику. Чтобы использовать `HttpClient` вспомогательные методы JSON, добавьте ссылку на пакет в `Microsoft.AspNetCore.Blazor.HttpClient`. `HttpClient`Кроме того, вспомогательные методы JSON также используются для вызова конечных точек веб-API сторонних производителей. `HttpClient`реализуется с помощью [API выборки](https://developer.mozilla.org/docs/Web/API/Fetch_API) в браузере и подчиняется его ограничениям, включая принудительное применение той же политики происхождения.

Базовый адрес клиента устанавливается в адрес исходного сервера. `HttpClient` Вставьте экземпляр `@inject` с помощью директивы:

```cshtml
@using System.Net.Http
@inject HttpClient Http
```

В следующих примерах веб-API TODO обрабатывает операции создания, чтения, обновления и удаления (CRUD). Примеры основаны на `TodoItem` классе, который хранит:

* Идентификатор (`Id`, `long`) &ndash; уникальный идентификатор элемента.
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

* `GetJsonAsync`&ndash; Отправляет запрос HTTP GET и анализирует текст ответа JSON для создания объекта.

  В следующем коде `_todoItems` компонент отображается с помощью компонента. Метод активируется при завершении подготовки компонента к просмотру ([онинитиализедасинк).](xref:blazor/components#lifecycle-methods) `GetTodoItems` Полный пример см. в примере приложения.

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/todo");
  }
  ```

* `PostJsonAsync`&ndash; Отправляет запрос HTTP POST, включая содержимое в кодировке JSON, и анализирует текст ответа JSON для создания объекта.

  В приведенном ниже коде `_newItemName` предоставляется привязанным элементом компонента. Метод активируется путем `<button>` выбора элемента. `AddItem` Полный пример см. в примере приложения.

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

* `PutJsonAsync`&ndash; Отправляет запрос HTTP-размещения, включая содержимое в кодировке JSON.

  В следующем коде `_editItem` значения для `Name` и `IsCompleted` предоставляются связанными элементами компонента. Элемент `Id` задается, когда элемент выбирается в другой части пользовательского интерфейса и `EditItem` вызывается. Метод активируется путем выбора элемента Save `<button>`. `SaveItem` Полный пример см. в примере приложения.

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

<xref:System.Net.Http>включает дополнительные методы расширения для отправки HTTP-запросов и получения HTTP-ответов. [HttpClient. DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) используется для отправки запроса HTTP DELETE в веб-интерфейс API.

В следующем коде элемент Delete `<button>` `DeleteItem` вызывает метод. Связанный `<input>` элемент`id` предоставляет удаляемый элемент. Полный пример см. в примере приложения.

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

## <a name="cross-origin-resource-sharing-cors"></a>Общий доступ к ресурсам между источниками (CORS)

Безопасность в браузере предотвращает запросы веб-страницы к другому домену, отличному от того, который обслуживает веб-страницу. Это ограничение называется *политикой того же происхождения*. Политика того же источника не позволит вредоносному сайту считывать конфиденциальные данные с другого сайта. Чтобы сделать запросы от браузера к конечной точке с другим источником, *Конечная точка* должна включить [общий доступ к ресурсам в разных источниках (CORS)](https://www.w3.org/TR/cors/).

Пример приложения демонстрирует использование CORS в компоненте Call Web API (*pages/каллвебапи. Razor*).

Сведения о том, как разрешить другим сайтам выполнять запросы общего доступа к ресурсам на основе источника (CORS <xref:security/cors>) в приложение, см. в разделе.

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a>HttpClient и HttpRequestMessage с параметрами запроса API FETCH

При выполнении для сборки в блазор приложении [HttpClient](xref:fundamentals/http-requests) <xref:System.Net.Http.HttpRequestMessage> используйте для настройки запросов. Например, можно указать URI запроса, метод HTTP и все нужные заголовки запроса.

Укажите параметры запроса для базового [API выборки](https://developer.mozilla.org/docs/Web/API/Fetch_API) JavaScript, `WebAssemblyHttpMessageHandler.FetchArgs` используя свойство в запросе. Как показано в следующем примере, `credentials` свойству присвоено любое из следующих значений:

* `FetchCredentialsOption.Include`("include") &ndash; Сообщает браузеру о необходимости отправки учетных данных (например, файлов cookie или заголовков проверки подлинности HTTP) даже для запросов между источниками. Допускается, только если политика CORS настроена для разрешения учетных данных.
* `FetchCredentialsOption.Omit`("пропустить") &ndash; Рекомендует браузеру никогда не передавать учетные данные (например, файлы cookie или заголовки проверки подлинности HTTP).
* `FetchCredentialsOption.SameOrigin`("один источник") &ndash; Рекомендует браузеру передавать учетные данные (например, файлы cookie или заголовки проверки подлинности HTTP) только в том случае, если целевой URL-адрес находится в том же источнике, что и вызывающее приложение.

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

Дополнительные сведения о параметрах API FETCH см. [в разделе веб-документы MDN: Виндоворворкерглобалскопе. fetch ():P араметерс](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).

При отправке учетных данных (файлов cookie или заголовков авторизации `Authorization` ) в запросах CORS этот заголовок должен быть разрешен политикой CORS.

Следующая политика включает в себя настройку для:

* Источники запроса (`http://localhost:5000`, `https://localhost:5001`).
* Любой метод (глагол).
* `Content-Type`и `Authorization` заголовки. Чтобы разрешить пользовательский заголовок (например, `x-custom-header`), выведите список заголовков при вызове метода. <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>
* Учетные данные, которые`credentials` `include`задаются кодом JavaScript на стороне клиента (свойство имеет значение).

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

Дополнительные сведения см. в <xref:security/cors> разделе и компоненте тестера HTTP-запросов примера приложения (*Components/хттпрекуесттестер. Razor*).

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/http-requests>
* [Общий доступ к ресурсам в разных источниках (CORS) в консорциуме W3C](https://www.w3.org/TR/cors/)
