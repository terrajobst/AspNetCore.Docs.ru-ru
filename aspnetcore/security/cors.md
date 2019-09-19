---
title: Включение запросов между источниками (CORS) в ASP.NET Core
author: rick-anderson
description: Узнайте, как использовать CORS в качестве стандарта для разрешения или отклонения запросов между источниками в ASP.NET Core приложении.
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: security/cors
ms.openlocfilehash: a34b77ad799a00707048c923b82b48774ce91682
ms.sourcegitcommit: b1e480e1736b0fe0e4d8dce4a4cf5c8e47fc2101
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/19/2019
ms.locfileid: "71108077"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>Включение запросов между источниками (CORS) в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

В этой статье показано, как включить CORS в приложении ASP.NET Core.

Безопасность в браузере предотвращает выполнение веб-страницей запросов к домену, отличному от того, который обслуживает веб-страницу. Это ограничение называется *политикой того же происхождения*. Политика того же источника не позволит вредоносному сайту считывать конфиденциальные данные с другого сайта. Иногда может потребоваться разрешить другим сайтам выполнять запросы между источниками в приложении. Дополнительные сведения см. в [статье Mozilla CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

[Совместное использование ресурсов в разных источниках](https://www.w3.org/TR/cors/) (CORS):

* — Это стандарт консорциума W3C, позволяющий серверу ослабить политику того же источника.
* **Не** является функцией безопасности, CORS ослабляет безопасность. Интерфейс API не обеспечивает безопасную поддержку CORS. Дополнительные сведения см. в разделе [как работает CORS](#how-cors).
* Позволяет серверу явно разрешать некоторые запросы между источниками и отклонять другие.
* Является более безопасным и более гибким, чем более ранние методики, например [JSONP](/dotnet/framework/wcf/samples/jsonp).

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="same-origin"></a>Тот же источник

Два URL-адреса имеют одинаковый источник, если они имеют идентичные схемы, узлы и порты ([RFC 6454](https://tools.ietf.org/html/rfc6454)).

Эти два URL-адреса имеют один и тот же источник:

* `https://example.com/foo.html`
* `https://example.com/bar.html`

Эти URL-адреса имеют разные источники, чем предыдущие два URL-адреса:

* `https://example.net`&ndash; Другой домен
* `https://www.example.com/foo.html`&ndash; Другой поддомен
* `http://example.com/foo.html`&ndash; Другая схема
* `https://example.com:9000/foo.html`&ndash; Другой порт

При сравнении источников Internet Explorer не учитывает порт.

## <a name="cors-with-named-policy-and-middleware"></a>CORS с именованной политикой и по промежуточного слоя

По промежуточного слоя CORS обрабатывает запросы между источниками. Следующий код включает CORS для всего приложения с указанным источником:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

Предыдущий код:

* Задает имя политики "\_мялловспеЦификоригинс". Имя политики является произвольным.
* Вызывает метод <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> расширения, который включает CORS.
* Вызывает <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> [лямбда-выражение](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). Лямбда-выражение принимает <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> объект. [Параметры конфигурации](#cors-policy-options), такие как `WithOrigins`, описаны далее в этой статье.

Вызов <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> метода добавляет службы CORS в контейнер службы приложения:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

Дополнительные сведения см. в разделе [Параметры политики CORS](#cpo) в этом документе.

Метод <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> может привести к цепочке методов, как показано в следующем коде:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

Примечание. URL-адрес **не** должен содержать косую черту`/`(). Если URL-адрес завершается `/`с, то сравнение `false` возвращает значение и заголовок не возвращается.

::: moniker range=">= aspnetcore-3.0"

Следующий код применяет политики CORS ко всем конечным точкам приложений через по промежуточного слоя CORS:
```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // Preceding code ommitted.
    app.UseRouting();

    app.UseCors();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });

    // Following code ommited.
}
```

> [!WARNING]
> При маршрутизации конечных точек по промежуточного слоя CORS должно быть настроено для выполнения `UseRouting` между `UseEndpoints`вызовами функций и. Неправильная конфигурация приведет к тому, что по промежуточного слоя будет работать правильно.

::: moniker-end

::: moniker range="<= aspnetcore-2.2"
Следующий код применяет политики CORS ко всем конечным точкам приложений через по промежуточного слоя CORS:
```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseHsts();
    }

    app.UseCors();

    app.UseHttpsRedirection();
    app.UseMvc();
}
```
Примечание. `UseCors` необходимо вызвать до `UseMvc`.

::: moniker-end

См. раздел [Включение CORS в Razor Pages, контроллерах и методах действий](#ecors) для применения политики CORS на уровне страницы, контроллера или действия.

Инструкции по тестированию приведенного выше кода см. в разделе [тестирование CORS](#test) .

<a name="ecors"></a>

::: moniker range=">= aspnetcore-3.0"

## <a name="enable-cors-with-endpoint-routing"></a>Включение CORS с маршрутизацией конечных точек

При маршрутизации конечных точек CORS можно включить для каждой конечной точки с помощью `RequireCors` набора методов расширения.

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapGet("/echo", async context => context.Response.WriteAsync("echo"))
    .RequireCors("policy-name");
});

```

Кроме того, CORS можно также включить для всех контроллеров:

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapControllers().RequireCors("policy-name");
});
```
::: moniker-end

## <a name="enable-cors-with-attributes"></a>Включение CORS с помощью атрибутов

Атрибут [EnableCors&rbrack;предоставляет альтернативу глобальному применению CORS. &lbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) `[EnableCors]` Атрибут включает CORS для выбранных конечных точек, а не всех конечных точек.

Используйте `[EnableCors]` , чтобы указать политику по умолчанию и `[EnableCors("{Policy String}")]` указать политику.

`[EnableCors]` Атрибут может быть применен к:

* Страница Razor`PageModel`
* Контроллер
* Метод действия контроллера

С `[EnableCors]` атрибутом можно применить различные политики для контроллера, модели страницы или действия. `[EnableCors]` Если атрибут применяется к контроллеру, модели страницы или методу действия, а CORS включен по промежуточного слоя, применяются обе политики. Мы советуем использовать сочетание политик. Используйте атрибут `[EnableCors]` или по промежуточного слоя, а не оба в одном приложении.

Следующий код применяет к каждому методу другую политику:

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

Следующий код создает политику CORS по умолчанию и политику с именем `"AnotherPolicy"`:

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a>Отключение CORS

Атрибут [дисаблекорс&rbrack;отключает CORS для контроллера, модели страницы или действия. &lbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute)

<a name="cpo"></a>

## <a name="cors-policy-options"></a>Параметры политики CORS

В этом разделе описаны различные параметры, которые можно задать в политике CORS:

* [Установка разрешенных источников](#set-the-allowed-origins)
* [Задание допустимых методов HTTP](#set-the-allowed-http-methods)
* [Задание разрешенных заголовков запроса](#set-the-allowed-request-headers)
* [Задание предоставленных заголовков ответа](#set-the-exposed-response-headers)
* [Учетные данные в запросах между источниками](#credentials-in-cross-origin-requests)
* [Задать срок действия предпечатного срока](#set-the-preflight-expiration-time)

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*>метод вызывается `Startup.ConfigureServices`в. Для некоторых параметров может оказаться полезным сначала ознакомиться с разделом " [работа CORS](#how-cors) ".

## <a name="set-the-allowed-origins"></a>Установка разрешенных источников

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>Разрешает запросы CORS из всех источников с любой схемой (`http` или `https`). &ndash; `AllowAnyOrigin`не является безопасным, так как *любой веб-сайт* может выполнять запросы между источниками в приложение.

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> Указание `AllowAnyOrigin` и`AllowCredentials` является небезопасной конфигурацией и может привести к подделке межсайтовых запросов. Служба CORS возвращает недопустимый ответ CORS, если приложение настроено с обоими методами.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> Указание `AllowAnyOrigin` и`AllowCredentials` является небезопасной конфигурацией и может привести к подделке межсайтовых запросов. Для безопасного приложения укажите точный список источников, если клиент должен авторизоваться для доступа к ресурсам сервера.

::: moniker-end

`AllowAnyOrigin`влияет на предпечатные запросы `Access-Control-Allow-Origin` и заголовок. Дополнительные сведения см. в разделе [предпечатные запросы](#preflight-requests) .

::: moniker range=">= aspnetcore-2.0"

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*>&ndash; Задает свойство политики как функцию, которая позволяет источникам соответствовать настроенному домену с подстановочными знаками при оценке того, разрешен ли источник. <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-104&highlight=4)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a>Задание допустимых методов HTTP

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

* Разрешает любой метод HTTP:
* Влияет на предпечатные запросы `Access-Control-Allow-Methods` и заголовок. Дополнительные сведения см. в разделе [предпечатные запросы](#preflight-requests) .

### <a name="set-the-allowed-request-headers"></a>Задание разрешенных заголовков запроса

Чтобы разрешить отправку конкретных заголовков в запрос CORS, именуемый *заголовком запроса на создание*, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> и укажите разрешенные заголовки:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Чтобы разрешить все заголовки запроса автора, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

Этот параметр влияет на предпечатные запросы `Access-Control-Request-Headers` и заголовок. Дополнительные сведения см. в разделе [предпечатные запросы](#preflight-requests) .

::: moniker range=">= aspnetcore-2.2"

Политика по промежуточного слоя CORS соответствует определенным заголовкам `WithHeaders` , указанным в параметре, только если `Access-Control-Request-Headers` отправляемые заголовки в точности совпадают `WithHeaders`с заголовками, указанными в.

Например, рассмотрим приложение, настроенное следующим образом:

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

По промежуточного слоя CORS отклоняет Предпечатный запрос со следующим заголовком запроса, так как `Content-Language` ([хеадернамес. контентлангуаже](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) нет в списке в: `WithHeaders`

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

Приложение возвращает ответ *ок 200* , но не ОТПРАВЛЯЕТ заголовки CORS обратно. Поэтому браузер не пытается выполнить запрос между источниками.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

По промежуточного слоя CORS всегда позволяет отправлять `Access-Control-Request-Headers` четыре заголовка в, независимо от значений, настроенных в корсполици. Headers. Этот список заголовков включает:

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

Например, рассмотрим приложение, настроенное следующим образом:

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

По промежуточного слоя CORS успешно реагирует на предварительный запрос со следующим заголовком `Content-Language` запроса, поскольку всегда имеет значение список разрешений:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a>Задание предоставленных заголовков ответа

По умолчанию браузер не предоставляет все заголовки ответа приложению. Дополнительные сведения см. в [разделе Общий доступ к ресурсам между источниками W3C (терминология). Простой заголовок](https://www.w3.org/TR/cors/#simple-response-header)ответа.

По умолчанию доступны заголовки ответов:

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

Спецификация CORS вызывает эти заголовки *простых заголовков ответа*. Чтобы сделать другие заголовки доступными для приложения, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a>Учетные данные в запросах между источниками

Учетные данные требует специальной обработки в запросе CORS. По умолчанию браузер не отправляет учетные данные с запросом между источниками. Учетные данные включают файлы cookie и схемы проверки подлинности HTTP. Чтобы отправить учетные данные с запросом между источниками, клиент должен `XMLHttpRequest.withCredentials` установить `true`в значение.

Использование `XMLHttpRequest` напрямую:

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

Использование jQuery:

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

Использование [API выборки](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

Сервер должен разрешать учетные данные. Чтобы разрешить учетные данные для разных источников <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>, вызовите:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

HTTP-ответ содержит `Access-Control-Allow-Credentials` заголовок, который сообщает браузеру, что сервер разрешает учетные данные для запроса между источниками.

Если браузер отправляет учетные данные, но ответ не включает допустимый `Access-Control-Allow-Credentials` заголовок, браузер не предоставляет ответ на приложение, и запрос на перекрестное происхождение завершается сбоем.

Разрешение учетных данных между источниками является угрозой безопасности. Веб-сайт в другом домене может отправить учетные данные пользователя, выполнившего вход, в приложение от имени пользователя без ведома пользователя. <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

В спецификации CORS также указывается, что `"*"` `Access-Control-Allow-Credentials` при наличии заголовка недопустимыми являются исходные значения (все источники).

### <a name="preflight-requests"></a>Предпечатные запросы

Для некоторых запросов CORS браузер отправляет дополнительный запрос перед выполнением фактического запроса. Этот запрос называется *предпечатным запросом*. Браузер может пропустить Предпечатный запрос, если выполняются следующие условия.

* Метод запроса — GET, HEAD или POST.
* Приложение не устанавливает заголовки `Accept`запроса, кроме, `Accept-Language`, `Content-Language`, `Content-Type`или `Last-Event-ID`.
* `Content-Type` Заголовок, если он задан, имеет одно из следующих значений:
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

Правило для заголовков запросов, заданных для запроса клиента, применяется к заголовкам, которые `setRequestHeader` устанавливаются `XMLHttpRequest` приложением путем вызова для объекта. Спецификация CORS вызывает заголовки *запроса автора*заголовков. Правило не применяется к заголовкам, которые может задать браузер, например `User-Agent`, `Host`или `Content-Length`.

Ниже приведен пример предпечатного запроса.

```
OPTIONS https://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: https://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

Запрос перед рейсом использует метод HTTP OPTIONS. Он включает два специальных заголовка:

* `Access-Control-Request-Method`: Метод HTTP, который будет использоваться для фактического запроса.
* `Access-Control-Request-Headers`: Список заголовков запросов, которые приложение устанавливает на фактическом запросе. Как упоминалось ранее, в него не входят заголовки, заданных браузером `User-Agent`, например.

Предпечатный запрос CORS может включать `Access-Control-Request-Headers` заголовок, указывающий серверу на заголовки, отправляемые с фактическим запросом.

Чтобы разрешить определенные заголовки, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Чтобы разрешить все заголовки запроса автора, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

Браузеры не полностью согласуются с тем `Access-Control-Request-Headers`, как они заданы. Если для заголовков задано значение `"*"` , отличное от <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>(или использование), следует включить `Accept`как `Content-Type`минимум, `Origin`, и, а также любые настраиваемые заголовки, которые требуется поддерживать.

Ниже приведен пример ответа на Предпечатный запрос (при условии, что сервер разрешает запрос):

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

Ответ содержит `Access-Control-Allow-Methods` заголовок со списком допустимых методов и, при необходимости `Access-Control-Allow-Headers` , заголовок, в котором перечислены разрешенные заголовки. Если Предпечатный запрос выполнен, браузер отправляет фактический запрос.

Если Предпечатный запрос отклонен, приложение возвращает ответ *ок 200* , но не ОТПРАВЛЯЕТ заголовки CORS обратно. Поэтому браузер не пытается выполнить запрос между источниками.

### <a name="set-the-preflight-expiration-time"></a>Задать срок действия предпечатного срока

`Access-Control-Max-Age` Заголовок указывает время, в течение которого может быть кэширован ответ на Предпечатный запрос. Чтобы задать этот заголовок, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a>Принцип работы CORS

В этом разделе описывается, что происходит в запросе [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) на уровне HTTP-сообщений.

* CORS **не** является функцией безопасности. CORS — это стандарт консорциума W3C, позволяющий серверу ослабить политику того же источника.
  * Например, вредоносный субъект может использовать для веб [-узла предотвращение межсайтовых сценариев (XSS)](xref:security/cross-site-scripting) и выполнить межсайтовый запрос к сайту с поддержкой CORS, чтобы украсть информацию.
* Интерфейс API не обеспечивает безопасную работу, разрешая CORS.
  * Для применения CORS требуется клиент (браузер). Сервер выполняет запрос и возвращает ответ. это клиент, который возвращает сообщение об ошибке и блокирует ответ. Например, в любом из следующих средств будет отображаться ответ сервера:
    * [Fiddler](https://www.telerik.com/fiddler)
    * [Postman](https://www.getpostman.com/)
    * [HttpClient .NET](/dotnet/csharp/tutorials/console-webapiclient)
    * Веб-браузер, введя URL-адрес в адресной строке.
* Сервер может позволить обозревателям выполнять запросы API [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) или [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) , которые в противном случае будут запрещены.
  * Браузеры (без CORS) не могут выполнять запросы между источниками. Перед CORS использовалась технология [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) для обхода этого ограничения. JSONP не использует XHR, он использует `<script>` тег для получения ответа. Скрипты могут загружаться в разных источниках.

В [спецификации CORS](https://www.w3.org/TR/cors/) появились несколько новых HTTP-заголовков, которые позволяют выполнять запросы между источниками. Если браузер поддерживает CORS, эти заголовки автоматически устанавливаются для запросов между источниками. Для включения CORS не требуется пользовательский код JavaScript.

Ниже приведен пример запроса между источниками. `Origin` Заголовок предоставляет домен сайта, выполняющего запрос:

```
GET https://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: https://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: https://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

Если сервер разрешает запрос, он устанавливает `Access-Control-Allow-Origin` заголовок в ответе. Значение этого заголовка либо соответствует `Origin` заголовку запроса, либо является подстановочным значением `"*"`, что означает, что любой источник разрешен:

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

Если ответ не содержит `Access-Control-Allow-Origin` заголовок, запрос на перекрестное происхождение завершается с ошибкой. В частности, браузер не разрешает запрос. Даже если сервер возвращает успешный ответ, браузер не делает ответ доступным клиентскому приложению.

<a name="test"></a>

## <a name="test-cors"></a>Тестирование CORS

Тестирование CORS:

1. [Создайте проект API](xref:tutorials/first-web-api). Кроме того, можно [загрузить пример](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).
1. Включите CORS с помощью одного из подходов, описанных в этом документе. Например:

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > `WithOrigins("https://localhost:<port>");`следует использовать только для тестирования примера приложения, аналогичного [образцу кода для скачивания](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).

1. Создайте проект веб-приложения (Razor Pages или MVC). В примере используется Razor Pages. Вы можете создать веб-приложение в том же решении, что и проект API.
1. Добавьте выделенный ниже код в файл *index. cshtml* :

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. В приведенном выше коде замените `url: 'https://<web app>.azurewebsites.net/api/values/1',` URL-адресом развернутого приложения.
1. Разверните проект API. Например, выполните [развертывание в Azure](xref:host-and-deploy/azure-apps/index).
1. Запустите приложение Razor Pages или MVC на рабочем столе и нажмите кнопку **Test (тест** ). Используйте средства F12 для просмотра сообщений об ошибках.
1. Удалите источник localhost из `WithOrigins` и разверните приложение. Кроме того, можно запустить клиентское приложение с другим портом. Например, запустите из Visual Studio.
1. Протестируйте клиентское приложение. Ошибки CORS возвращают ошибку, но сообщение об ошибке недоступно для JavaScript. Чтобы просмотреть ошибку, используйте вкладку консоль в средствах F12. В зависимости от браузера вы получаете ошибку (в консоли средств F12), как показано ниже:

   * Использование Microsoft ребр:

     **SEC7120: [CORS] не удалось `https://localhost:44375` найти `https://localhost:44375` источник в заголовке ответа Access-Control-Allow-Origin для ресурса-источника в`https://webapi.azurewebsites.net/api/values/1`**

   * Использование Chrome:

     **Доступ к XMLHttpRequest `https://webapi.azurewebsites.net/api/values/1` из источника `https://localhost:44375` заблокирован политикой CORS: В запрошенном ресурсе отсутствует заголовок "Access-Control-Allow-Origin".**

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Общий доступ к ресурсам между источниками (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
