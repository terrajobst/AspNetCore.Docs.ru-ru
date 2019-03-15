---
title: Включение запросов о происхождении (CORS) в ASP.NET Core
author: rick-anderson
description: Узнайте, как CORS в качестве стандарта для предоставления или отклонения запросов независимо от источника в приложении ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/27/2019
uid: security/cors
ms.openlocfilehash: 6be8b4da1642a9eff021371c229a17071d6e9bfb
ms.sourcegitcommit: d913bca90373c07f89b1d1df01af5fc01fc908ef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/14/2019
ms.locfileid: "57978475"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>Включение запросов о происхождении (CORS) в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

В этой статье показано, как включить поддержку CORS в приложении ASP.NET Core.

Безопасность обозревателя предотвращает веб-странице запросов в другой домен, отличного от того, который обслуживал веб-страницы. Это ограничение называется *политика одного источника*. Политика одного источника предотвращает чтение конфиденциальных данных с другого сайта вредоносный сайт. В некоторых случаях может потребоваться разрешить другие сайты выполнять запросы независимо от источника к приложению. Дополнительные сведения см. в разделе [Mozilla CORS статье](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

[CROSS Origin общий доступ к ресурсам](https://www.w3.org/TR/cors/) (CORS):

* Такое W3C, позволяющий серверу смягчить ограничения политики одного источника "стандартный".
* — **Не** средство безопасности, а CORS снижает безопасность. API не является более безопасным, позволяя CORS. Дополнительные сведения см. в разделе [работает как CORS](#how-cors).
* Позволяет серверу явным образом разрешить некоторые запросы независимо от источника, а другие — отклонять.
* Является более безопасным и более гибким, чем предыдущие технологии, такие как [JSONP](/dotnet/framework/wcf/samples/jsonp).

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/2.2-stage-samples) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="same-origin"></a>Того же происхождения

Два URL-адреса иметь того же происхождения, если они имеют одинаковые схемы, узлов и порты ([RFC 6454](https://tools.ietf.org/html/rfc6454)).

Эти два URL-адреса у того же происхождения:

* `https://example.com/foo.html`
* `https://example.com/bar.html`

Эти URL-адреса имеют различное происхождение, чем предыдущие два URL-адреса:

* `https://example.net` &ndash; Другой домен
* `https://www.example.com/foo.html` &ndash; Другой поддомен
* `http://example.com/foo.html` &ndash; Другой схемы
* `https://example.com:9000/foo.html` &ndash; Другой порт

Internet Explorer не считает порт, при сравнении источников.

## <a name="cors-with-named-policy-and-middleware"></a>С помощью именованной политики и по промежуточного слоя

По промежуточного слоя CORS обрабатывает запросы независимо от источника. Следующий код включает доступ CORS для всего приложения с помощью указанного источника:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

Предыдущий код:

* Задает имя политики "\_myAllowSpecificOrigins». Имя политики является произвольным.
* Вызовы <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> метод расширения, который позволяет ядер.
* Вызовы <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> с [лямбда-выражение](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). Лямбда-выражение принимает <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> объекта. [Параметры конфигурации](#cors-policy-options), такие как `WithOrigins`, описаны далее в этой статье.

<xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> Вызов метода добавляет CORS службы в контейнер службы приложения:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

Дополнительные сведения см. в разделе [параметры политики CORS](#cpo) в этом документе.

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> Метода можно объединять в цепочку методы, как показано в следующем коде:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

Следующий выделенный код применяет политики CORS ко всем конечным точкам приложений с помощью по промежуточного слоя CORS:

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

См. в разделе [включить CORS в Razor Pages, контроллеры и методы действий](#ecors) для применения политики CORS на уровне страницы или контроллеру или действию.

Примечание.

* `UseCors` должен вызываться перед `UseMvc`.
* URL-адрес должен **не** содержать косую черту (`/`). Если URL-адрес заканчивается `/`, сравнение возвращает `false` и возвращается без заголовка.

См. в разделе [CORS теста](#test) инструкции по тестированию приведенный выше код.

<a name="ecors"></a>

## <a name="enable-cors-with-attributes"></a>Включение CORS с помощью атрибутов

[ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) атрибута представляет собой альтернативу глобально применение CORS. `[EnableCors]` Атрибута включает доступ CORS для выбранных конечных точек, а не всех конечных точек.

Используйте `[EnableCors]` для задания политики по умолчанию и `[EnableCors("{Policy String}")]` задание политики.

`[EnableCors]` Атрибут может применяться для:

* Страница Razor `PageModel`
* Контроллер
* Метод действия контроллера

Разные политики можно применить к контроллеру или странице модели/действию с `[EnableCors]` атрибута. Когда `[EnableCors]` атрибут применяется к методу контроллеров/страницы модель и действие и включен CORS в по промежуточного слоя, обе политики применяются. Мы не рекомендуем объединение политик. Используйте `[EnableCors]` атрибута или по промежуточного слоя, не одновременно в одном приложении.

В следующем коде применяется другую политику к каждому методу:

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

Следующий код создает политику CORS по умолчанию и политику с именем `"AnotherPolicy"`:

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a>Отключить CORS

[ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) атрибут отключает CORS для контроллера/страницы модели/действие.

<a name="cpo"></a>

## <a name="cors-policy-options"></a>Параметры политики CORS

В этом разделе описываются различные параметры, которые могут устанавливаться в политике CORS:

* [Задайте разрешенные источники](#set-the-allowed-origins)
* [Задайте разрешенные методы HTTP](#set-the-allowed-http-methods)
* [Задать заголовки запросов](#set-the-allowed-request-headers)
* [Задайте заголовки ответа, предоставляемого](#set-the-exposed-response-headers)
* [Учетные данные в запросов о происхождении](#credentials-in-cross-origin-requests)
* [Задайте срок действия предварительного](#set-the-preflight-expiration-time)

 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> вызывается в `Startup.ConfigureServices`. Для некоторых параметров, может оказаться удобным для чтения [работает как CORS](#how-cors) разделе сначала.

## <a name="set-the-allowed-origins"></a>Задайте разрешенные источники

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Разрешает запросы CORS из всех источников с любой схемой (`http` или `https`). `AllowAnyOrigin` не защищен, поскольку *любой веб-сайт* могут выполнять запросы независимо от источника к приложению.

  ::: moniker range=">= aspnetcore-2.2"

  > [!NOTE]
  > Указание `AllowAnyOrigin` и `AllowCredentials` является небезопасной конфигурацией и может привести к подделки межсайтовых запросов. CORS, служба возвращает недопустимый ответ CORS приложения настраивается с помощью обоих методов.

  ::: moniker-end

  ::: moniker range="< aspnetcore-2.2"

  > [!NOTE]
  > Указание `AllowAnyOrigin` и `AllowCredentials` является небезопасной конфигурацией и может привести к подделки межсайтовых запросов. Для безопасного приложения укажите точный список источников, если клиент должен авторизоваться сам доступ к ресурсам сервера.

  ::: moniker-end

<!-- REVIEW required
I changed from
Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration. **This** setting affects preflight requests and the ...
to
**`AllowAnyOrigin`** affects preflight requests and the

to remove the ambiguous **This**. 
-->

  `AllowAnyOrigin` влияет на запросы перед запуском выявила и `Access-Control-Allow-Origin` заголовка. Дополнительные сведения см. в разделе [перед запуском выявила запросы](#preflight-requests) раздел.

::: moniker range=">= aspnetcore-2.0"

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Наборы <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> свойство быть функцией, которая позволяет источников для сопоставления домена с подстановочным знаком настроенных при оценке, если разрешено источника политики.

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-104&highlight=4)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a>Задайте разрешенные методы HTTP

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

* Разрешает любой метод HTTP:
* Влияет на запросы перед запуском выявила и `Access-Control-Allow-Methods` заголовка. Дополнительные сведения см. в разделе [перед запуском выявила запросы](#preflight-requests) раздел.

### <a name="set-the-allowed-request-headers"></a>Задать заголовки запросов

Чтобы разрешить специальные заголовки, которые будут отправляться в запрос CORS с именем *создания заголовков запроса*, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> и укажите разрешенные заголовки:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Чтобы разрешить все создавать заголовки запроса, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

Этот параметр влияет на предварительные запросы и `Access-Control-Request-Headers` заголовка. Дополнительные сведения см. в разделе [перед запуском выявила запросы](#preflight-requests) раздел.

::: moniker range=">= aspnetcore-2.2"

Политики соответствия по промежуточного слоя CORS специальные заголовки, заданные `WithHeaders` , возможна только при отправке заголовков `Access-Control-Request-Headers` точно соответствовать заголовки, перечисленным в `WithHeaders`.

Например рассмотрим приложение, настроить следующим образом:

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

По промежуточного слоя CORS отклоняет Предварительный запрос следующий заголовок запроса, так как `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) не отображается в `WithHeaders`:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

Возвращает приложение *200 ОК* ответа, но не отправляет обратно заголовки CORS. Таким образом браузер не делает запрос независимо от источника.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

По промежуточного слоя CORS позволяет всегда четыре заголовки в `Access-Control-Request-Headers` отправляемых независимо от значения, заданные в CorsPolicy.Headers. Этот список заголовков входят:

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

Например рассмотрим приложение, настроить следующим образом:

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

По промежуточного слоя CORS успешно отвечает на Предварительный запрос следующий заголовок запроса, так как `Content-Language` всегда имеет список разрешенных:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a>Задайте заголовки ответа, предоставляемого

По умолчанию браузер не предоставляет все заголовки ответа для приложения. Дополнительные сведения см. в разделе [W3C независимо от источника общий доступ к ресурсам (терминология): Заголовок ответа на простой](https://www.w3.org/TR/cors/#simple-response-header).

Заголовки ответа, которые доступны по умолчанию являются:

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

Спецификация CORS вызывает эти заголовки *заголовки ответа на простой*. Чтобы сделать доступным для приложения другие заголовки, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a>Учетные данные в запросов о происхождении

Учетные данные, требующие особых действий в запрос CORS. По умолчанию браузер не отправляет учетные данные с помощью запроса независимо от источника. Учетные данные содержат файлы cookie и схемы проверки подлинности HTTP. Для отправки учетных данных с помощью запроса независимо от источника, клиент должен указать `XMLHttpRequest.withCredentials` для `true`.

С помощью `XMLHttpRequest` напрямую:

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

С помощью jQuery:

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

С помощью [выборки API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

Server должен разрешать учетные данные. Чтобы разрешить учетные данные от источника, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

Ответ HTTP содержит `Access-Control-Allow-Credentials` заголовок, который указывает обозревателю, учетные данные для запроса независимо от источника, поддерживает ли сервер.

Если браузер отправляет учетные данные, но ответ не содержит допустимый `Access-Control-Allow-Credentials` заголовок, браузер не предоставляет приложению ответ, и независимо от источника запрос завершится ошибкой.

Учетные данные от источника является угрозу безопасности. Веб-сайт в другом домене можно отправить учетные данные выполнившего вход пользователя в приложение от имени пользователя без уведомления пользователя. <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

Спецификация CORS также указывает, что параметр источники, которые можно `"*"` (все источники) является недопустимым при `Access-Control-Allow-Credentials` заголовок отсутствует.

### <a name="preflight-requests"></a>Предварительные запросы

Для некоторых запросов CORS браузер посылает дополнительный запрос перед внесением самого запроса. Этот запрос называется *Предварительный запрос*. Браузер можно пропустить Предварительный запрос, если выполняются следующие условия:

* Метод запроса — GET, HEAD или POST.
* Приложение не задает заголовки запроса, отличное от `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, или `Last-Event-ID`.
* `Content-Type` Заголовка, если задано, имеет одно из следующих значений:
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

Набора правил на заголовки запроса для запроса клиента применяется к заголовки, которые приложение задает путем вызова `setRequestHeader` на `XMLHttpRequest` объекта. Спецификация CORS вызывает эти заголовки *создания заголовков запроса*. Правило не применяется к заголовкам, можно задать браузер, такие как `User-Agent`, `Host`, или `Content-Length`.

Ниже приведен пример Предварительный запрос:

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

Возможность предварительного запроса используется метод HTTP OPTIONS. Он включает два специальных заголовков:

* `Access-Control-Request-Method`: Метод HTTP, который будет использоваться для самого запроса.
* `Access-Control-Request-Headers`: Список заголовков запросов, которые приложение задает для самого запроса. Как уже говорилось ранее, не включая заголовки, которые задает браузер, такие как `User-Agent`.

Предварительный запрос CORS может включать `Access-Control-Request-Headers` заголовок, который указывает серверу, заголовки, которые отправляются в самом запросе.

Чтобы разрешить специальные заголовки, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Чтобы разрешить все создавать заголовки запроса, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

Браузеры не полностью согласованные, в том, как они заданы `Access-Control-Request-Headers`. Если задать заголовки на что-либо отличное от `"*"` (или используйте <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), должен включать по крайней мере `Accept`, `Content-Type`, и `Origin`, а также любые пользовательские заголовки, которые требуется поддерживать.

Ниже приведен пример ответа на Предварительный запрос, (при условии, что сервер разрешает запрос).

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

Ответ включает в себя `Access-Control-Allow-Methods` заголовка, в которой перечислены разрешенные методы и при необходимости `Access-Control-Allow-Headers` заголовок, в котором перечислены разрешенные заголовки. Если Предварительный запрос завершается успешно, браузер отправляет фактический запрос.

Если Предварительный запрос отклонен, приложение возвращает *200 ОК* ответа, но не отправляет обратно заголовки CORS. Таким образом браузер не делает запрос независимо от источника.

### <a name="set-the-preflight-expiration-time"></a>Задайте срок действия предварительного

`Access-Control-Max-Age` Заголовок указывает, как долго можно кэшировать ответ на Предварительный запрос. Чтобы задать этот заголовок, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a>Как работает CORS

В этом разделе описывается, что происходит в [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) запрос на уровне сообщений HTTP.

* CORS — **не** средство безопасности. CORS — это стандартный W3C, позволяющий серверу смягчить ограничения политики одного источника.
  * Например, использовать вредоносного субъекта [сценариев предотвращения между сайтами (XSS)](xref:security/cross-site-scripting) на веб-узле и выполнения запросов между сайтами с сайтом включен механизм CORS для кражи информации.
* API не является более безопасным, позволяя CORS.
  * Именно на клиент (браузер) принудительно CORS. Сервер выполняет запрос и возвращает ответ, клиент, который возвращает ошибку и блоки ответ. Например любой из следующих средств отображения ответ сервера:
     * [Fiddler](https://www.telerik.com/fiddler)
     * [Postman](https://www.getpostman.com/)
     * [.NET HttpClient](/dotnet/csharp/tutorials/console-webapiclient)
     * Веб-браузер, введя URL-адрес в адресной строке.
* Это способ для сервера разрешить браузеры выполнения независимо от источника [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) или [выборки API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) запроса, в противном случае может быть запрещено.
  * Запросов о происхождении обозревателей (CORS) не поддерживается. Прежде чем CORS [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) было использовано, чтобы обойти это ограничение. JSONP не использует XHR, он использует `<script>` тег для получения ответа. Сценарии могут быть загружена независимо от источника.

[Спецификация CORS]() появился ряд новых заголовков HTTP, которые позволяют запросы независимо от источника. Если браузер поддерживает CORS, он устанавливает эти заголовки для запросов о происхождении автоматически. Пользовательский код JavaScript не обязательно для включения CORS.

Ниже приведен пример запроса независимо от источника. `Origin` Заголовок предоставляет домена сайта, который выполняет запрос:

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

Если сервер разрешает запрос, он задает `Access-Control-Allow-Origin` заголовок ответа. Значение этого заголовка либо соответствует `Origin` заголовок из запроса или подстановочное значение `"*"`, это значит, что допускается любого источника:

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

Если ответ не включает `Access-Control-Allow-Origin` заголовок, происходит сбой запроса независимо от источника. В частности браузер запрещает запрос. Даже если сервер возвращает успешный ответ, браузер не предоставления ответа в клиентское приложение.

<a name="test"></a>

## <a name="test-cors"></a>Тестирование CORS

Тестирование CORS:

1. [Создание проекта API](xref:tutorials/first-web-api). Кроме того, вы можете [скачать пример](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cors/sample/Cors).
1. Включите CORS, с помощью одного из методов в этом документе. Пример:

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]
  
  > [!WARNING]
  > `WithOrigins("https://localhost:<port>");` следует использовать только для тестирования примера приложения, аналогичную [скачать образец кода](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/cors/sample/Cors).

1. Создайте проект веб-приложения (Razor Pages или MVC). В примере используется Razor Pages. Можно создать веб-приложения в том же решении, что и проект API.
1. Добавьте следующий выделенный код, который *Index.cshtml* файла:

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. В приведенном выше коде замените `url: 'https://<web app>.azurewebsites.net/api/values/1',` с URL-адрес развернутого приложения.
1. Разверните проект API. Например [развертывание в Azure](xref:host-and-deploy/azure-apps/index).
1. Запустите приложение Razor Pages или MVC с рабочего стола и щелкнуть **теста** кнопки. Используйте инструменты F12 для просмотра сообщений об ошибках.
1. Удаление из начала координат localhost `WithOrigins` и развернуть приложение. Кроме того запустите клиентское приложение с другим портом. Например запуск из Visual Studio.
1. Тестирование с помощью клиентского приложения. Сбои CORS сообщение об ошибке, но сообщение об ошибке недоступно в код JavaScript. Используйте вкладку консоли в инструментах F12, чтобы просмотреть ошибку. В зависимости от браузера отобразится сообщение об ошибке (в консоли инструменты F12) следующего вида:

  * С помощью Microsoft Edge:

    **SEC7120: [CORS] источник `https://localhost:44375` не обнаружил `https://localhost:44375` в заголовке ответа Access-Control-Allow-Origin для ресурса независимо от источника `https://webapi.azurewebsites.net/api/values/1`**

  * С помощью Chrome:

    **Доступ к XMLHttpRequest в `https://webapi.azurewebsites.net/api/values/1` от начала `https://localhost:44375` была заблокирована политикой CORS: Заголовок «Access-Control-Allow-Origin» отсутствует для запрашиваемого ресурса.**

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Кросс-совместного использования ресурсов (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
