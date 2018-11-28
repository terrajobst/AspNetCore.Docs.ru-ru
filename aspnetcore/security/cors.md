---
title: Включение запросов о происхождении (CORS) в ASP.NET Core
author: rick-anderson
description: Узнайте, как CORS в качестве стандарта для предоставления или отклонения запросов независимо от источника в приложении ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2018
uid: security/cors
ms.openlocfilehash: f0e01cfa618184d8a3b19c06212dc3914183a2e4
ms.sourcegitcommit: e7fafb153b9de7595c2558a0133f8d1c33a3bddb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/28/2018
ms.locfileid: "52458547"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>Включение запросов о происхождении (CORS) в ASP.NET Core

По [Майк Уоссон](https://github.com/mikewasson), [Шейн Бойер](https://twitter.com/spboyer), и [том Дайкстра](https://github.com/tdykstra)

Безопасность обозревателя предотвращает веб-странице запросов в другой домен, отличного от того, который обслуживал веб-страницы. Это ограничение называется *политика одного источника*. Политика одного источника предотвращает чтение конфиденциальных данных с другого сайта вредоносный сайт. В некоторых случаях может потребоваться разрешить другие сайты выполнять запросы независимо от источника к приложению.

[Кросс-Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS) — это стандарт консорциума W3C, позволяющий серверу смягчить ограничения политики одного источника. С помощью CORS сервер может явным образом разрешить некоторые запросы независимо от источника а другие — отклонять. CORS — более безопасное и более гибким, чем предыдущие технологии, такие как [JSONP](https://wikipedia.org/wiki/JSONP). В этом разделе показано, как включить поддержку CORS в приложении ASP.NET Core.

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

> [!NOTE]
> Internet Explorer не считает порт, при сравнении источников.

## <a name="register-cors-services"></a>Регистрация службы CORS

::: moniker range=">= aspnetcore-2.1"

Справочник по [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) или добавьте ссылку на пакет [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) пакета.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Справочник по [метапакет Microsoft.AspNetCore.All](xref:fundamentals/metapackage) или добавьте ссылку на пакет [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) пакета.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Добавьте ссылку на пакет [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) пакета.

::: moniker-end

Вызовите <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> в `Startup.ConfigureServices` добавлять службы CORS в контейнере служб приложения:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors&highlight=3)]

## <a name="enable-cors"></a>Включение CORS

После регистрации службы CORS, используйте один из следующих подходов описывается включение CORS в приложении ASP.NET Core.

* [По промежуточного слоя CORS](#enable-cors-with-cors-middleware) &ndash; политики CORS применяются глобально для приложения с помощью по промежуточного слоя.
* [CORS в MVC](#enable-cors-in-mvc) &ndash; CORS для применения политик на действие или на контроллере. По промежуточного слоя CORS не используется.

### <a name="enable-cors-with-cors-middleware"></a>Включение CORS с помощью по промежуточного слоя CORS

По промежуточного слоя CORS обрабатывает запросы независимо от источника к приложению. Чтобы включить по промежуточного слоя CORS в конвейер обработки запросов, вызовите <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> метод расширения в `Startup.Configure`.

По промежуточного слоя CORS должен предшествовать любой определены конечные точки в приложении место для поддержки запросов о происхождении (например, перед вызовом `UseMvc` для по промежуточного слоя MVC и Razor Pages).

Объект *политики независимо от источника* могут быть указаны при добавлении по промежуточного слоя CORS с помощью <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> класса. Существует два подхода для определения политики CORS:

* Вызовите `UseCors` с лямбда-выражения:

  [!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

  Лямбда-выражение принимает <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> объекта. [Параметры конфигурации](#cors-policy-options), такие как `WithOrigins`, описаны далее в этом разделе. В приведенном выше примере политика позволяет запросов о происхождении из `https://example.com` и другие источники.

  URL-адрес должен быть указан без косой чертой (`/`). Если URL-адрес заканчивается `/`, сравнение возвращает `false` и возвращается без заголовка.

  `CorsPolicyBuilder` имеет текучего API, поэтому можно объединять в цепочку вызовов методов:

  [!code-csharp[](cors/sample/CorsExample3/Startup.cs?highlight=2-3&range=29-32)]

* Определите один или несколько именованных политик CORS и выберите политику по имени во время выполнения. В следующем примере добавляется определяемую пользователем политику CORS с именем *AllowSpecificOrigin*. Чтобы выбрать политику, передайте имя в `UseCors`:

  [!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin&highlight=5-6,21)]

### <a name="enable-cors-in-mvc"></a>Включение CORS в MVC

Также можно использовать MVC для применения определенных политик CORS на действие или на контроллере. При использовании MVC для включения CORS, используются зарегистрированные службы CORS. По промежуточного слоя CORS не используется.

### <a name="per-action"></a>Каждого действия

Чтобы указать политику CORS для определенного действия, добавьте [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) атрибут к действию. Укажите имя политики.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction&highlight=2)]

### <a name="per-controller"></a>Для контроллера

Чтобы указать политику CORS для определенного контроллера, добавьте [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) атрибут в класс контроллера. Укажите имя политики.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController&highlight=2)]

Ниже приведен порядок приоритета.

1. action
1. контроллер

### <a name="disable-cors"></a>Отключить CORS

Чтобы отключить CORS для контроллера или действия, используйте [ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) атрибут:

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction&highlight=2)]

## <a name="cors-policy-options"></a>Параметры политики CORS

В этом разделе описываются различные параметры, которые можно задать в политику CORS. <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> Метод вызывается в `Startup.ConfigureServices`.

* [Задайте разрешенные источники](#set-the-allowed-origins)
* [Задайте разрешенные методы HTTP](#set-the-allowed-http-methods)
* [Задать заголовки запросов](#set-the-allowed-request-headers)
* [Задайте заголовки ответа, предоставляемого](#set-the-exposed-response-headers)
* [Учетные данные в запросов о происхождении](#credentials-in-cross-origin-requests)
* [Задайте срок действия предварительного](#set-the-preflight-expiration-time)

Для некоторых параметров, может оказаться удобным для чтения [работает как CORS](#how-cors-works) разделе сначала.

### <a name="set-the-allowed-origins"></a>Задайте разрешенные источники

По промежуточного слоя CORS в ASP.NET Core MVC имеет несколько способов, чтобы указать разрешенные источники:

* <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*> &ndash; Позволяет указать один или несколько URL-адреса. URL-адрес может включать схему, имя узла и порт без включения данных пути. Например, `https://example.com`. URL-адрес должен быть указан без косой чертой (`/`).

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=20-25&highlight=4-5)]

* <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Разрешает запросы CORS из всех источников с любой схемой (`http` или `https`).

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=29-33&highlight=4)]

  Тщательно обдумайте прежде чем разрешить запросы из любого источника. Разрешение запросов из любого источника означает, что *любой веб-сайт* могут выполнять запросы независимо от источника к приложению.

  ::: moniker range=">= aspnetcore-2.2"

  > [!NOTE]
  > Указание `AllowAnyOrigin` и `AllowCredentials` является небезопасной конфигурацией и может привести к подделки межсайтовых запросов. CORS, служба возвращает недопустимый ответ CORS приложения настраивается с помощью обоих методов.

  ::: moniker-end

  ::: moniker range="< aspnetcore-2.2"

  > [!NOTE]
  > Указание `AllowAnyOrigin` и `AllowCredentials` является небезопасной конфигурацией и может привести к подделки межсайтовых запросов. Можно указать точный список источников, если клиент должен авторизоваться сам доступ к ресурсам сервера.

  ::: moniker-end

  Этот параметр влияет на предварительные запросы и `Access-Control-Allow-Origin` заголовка. Дополнительные сведения см. в разделе [перед запуском выявила запросы](#preflight-requests) раздел.

::: moniker range=">= aspnetcore-2.0"

* <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Наборы <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> свойство быть функцией, которая позволяет источники, которые можно соответствовать домену настроенных с подстановочными знаками, при оценке, если разрешено источника политики.

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-104&highlight=4)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a>Задайте разрешенные методы HTTP

Чтобы разрешить все методы HTTP, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=46-51&highlight=5)]

Этот параметр влияет на предварительные запросы и `Access-Control-Allow-Methods` заголовка. Дополнительные сведения см. в разделе [перед запуском выявила запросы](#preflight-requests) раздел.

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

По умолчанию браузер не предоставляет все заголовки ответа для приложения. Дополнительные сведения см. в разделе [W3C независимо от источника общий доступ к ресурсам (терминология): простой заголовок ответа](https://www.w3.org/TR/cors/#simple-response-header).

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

В jQuery:

```jQuery
$.ajax({
  type: 'get',
  url: 'https://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

Кроме того сервер необходимо разрешить учетные данные. Чтобы разрешить учетные данные от источника, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

Ответ HTTP содержит `Access-Control-Allow-Credentials` заголовок, который указывает обозревателю, учетные данные для запроса независимо от источника, поддерживает ли сервер.

Если браузер отправляет учетные данные, но ответ не содержит допустимый `Access-Control-Allow-Credentials` заголовок, браузер не предоставляет приложению ответ, и независимо от источника запрос завершится ошибкой.

Будьте внимательны, если разрешить передачу учетных данных независимо от источника. Веб-сайт в другом домене можно отправить учетные данные выполнившего вход пользователя в приложение от имени пользователя без уведомления пользователя.

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

## <a name="how-cors-works"></a>Как работает CORS

В этом разделе описывается, что происходит в запрос CORS на уровне сообщений HTTP. Важно понять, как работает CORS, чтобы политика CORS можно настроить правильно и отладки при возникновении непредвиденному поведению.

Спецификация CORS представляет ряд новых заголовков HTTP, которые позволяют запросы независимо от источника. Если браузер поддерживает CORS, он устанавливает эти заголовки для запросов о происхождении автоматически. Пользовательский код JavaScript не обязательно для включения CORS.

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

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Кросс-совместного использования ресурсов (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
