---
title: Включение запросов о происхождении (CORS) в ASP.NET Core
author: rick-anderson
description: Узнайте, как CORS в качестве стандарта для предоставления или отклонения запросов независимо от источника в приложении ASP.NET Core.
ms.author: riande
ms.date: 08/17/2018
uid: security/cors
ms.openlocfilehash: 0dbb7933c76bb0d1d0cab519ea08c6c8f0ebedfd
ms.sourcegitcommit: 64c2ca86fff445944b155635918126165ee0f8aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/18/2018
ms.locfileid: "41838936"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>Включение запросов о происхождении (CORS) в ASP.NET Core

По [Майк Уоссон](https://github.com/mikewasson), [Шейн Бойер](https://twitter.com/spboyer), и [том Дайкстра](https://github.com/tdykstra)

Безопасность обозревателя запрещает отправку запросов AJAX в другой домен веб-страницы. Это ограничение называется *политика одного источника*и предотвращает чтение конфиденциальных данных с другого сайта вредоносный сайт. Тем не менее иногда вам может потребоваться разрешить другие сайты, которые выполняют запросы независимо от источника к веб-API.

[Кросс-Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) — это стандарт консорциума W3C, позволяющий серверу смягчить ограничения политики одного источника. С помощью CORS сервер может явным образом разрешить некоторые запросы независимо от источника а другие — отклонять. CORS — более безопасное и более гибким, чем предыдущие технологии, такие как [JSONP](https://wikipedia.org/wiki/JSONP). В этом разделе показано, как включить поддержку CORS в приложении ASP.NET Core.

## <a name="what-is-same-origin"></a>Что такое «того же происхождения»?

Два URL-адреса иметь того же происхождения, если они имеют одинаковые схемы, узлов и порты. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Эти два URL-адреса у того же происхождения:

* `http://example.com/foo.html`

* `http://example.com/bar.html`

Эти URL-адреса имеют различное происхождение по сравнению с предыдущим два:

* `http://example.net` -Другой домен

* `http://www.example.com/foo.html` -Другой поддомен

* `https://example.com/foo.html` -Другую схему

* `http://example.com:9000/foo.html` -Другой порт

> [!NOTE]
> Internet Explorer не считает порт, при сравнении источников.

## <a name="enable-cors"></a>Включение CORS

::: moniker range="<= aspnetcore-1.1"

Чтобы настроить CORS для приложения добавьте `Microsoft.AspNetCore.Cors` пакета в проект.

::: moniker-end

Вызовите [AddCors](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) в `Startup.ConfigureServices`:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a>Включение CORS с по промежуточного слоя

Чтобы включить CORS, добавьте по промежуточного слоя CORS в конвейер запросов с помощью `UseCors` метода расширения. По промежуточного слоя CORS должен предшествовать любой определены конечные точки в приложении место для поддержки запросов о происхождении (например, предшествующий вызову `UseMvc`).

Можно указать политику независимо от источника, при добавлении по промежуточного слоя CORS с помощью [CorsPolicyBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) класса. Это можно сделать двумя способами. Первый способ — вызвать `UseCors` с лямбда-выражения:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

**Примечание:** URL-адрес должен быть указан без косой чертой (`/`). Если URL-адрес заканчивается `/`, сравнение вернет `false` и будет возвращаться без заголовка.

Лямбда-выражение принимает `CorsPolicyBuilder` объекта. Вы найдете список [параметры конфигурации](#cors-policy-options) далее в этом разделе. В этом примере политика позволяет запросов о происхождении из `http://example.com` и другие источники.

CorsPolicyBuilder имеет текучего API, поэтому можно объединять в цепочку вызовов методов:

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

Второй подход заключается в том, чтобы определить один или несколько именованных политик CORS, а затем выбрать политику по имени во время выполнения.

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

Этот пример добавляет политику CORS, с именем «AllowSpecificOrigin». Чтобы выбрать политику, передайте имя в `UseCors`.

## <a name="enabling-cors-in-mvc"></a>Включение CORS в MVC

Также можно использовать MVC для применения определенных CORS каждого действия, отдельного контроллера или глобально для всех контроллеров. При использовании MVC для включения CORS используются те же службы CORS, но не по промежуточного слоя CORS.

### <a name="per-action"></a>Каждого действия

Чтобы указать политику CORS для определенного действия добавьте `[EnableCors]` атрибут к действию. Укажите имя политики.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a>Для контроллера

Чтобы указать политику CORS для определенного контроллера добавьте `[EnableCors]` атрибут в класс контроллера. Укажите имя политики.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a>Глобально

Вы можете включить CORS глобально для всех контроллеров, добавив `CorsAuthorizationFilterFactory` фильтр в глобальную коллекцию фильтров:

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

Очередность: действия, контроллера, глобальные. Политики на уровне действия имеют приоритет над политиками уровня контроллера, и политики на уровне контроллера имеют приоритет над глобальные политики.

### <a name="disable-cors"></a>Отключить CORS

Чтобы отключить CORS для контроллера или действия, используйте `[DisableCors]` атрибута.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a>Параметры политики CORS

В этом разделе описываются различные параметры, которые можно задать в политику CORS.

* [Задайте разрешенные источники](#set-the-allowed-origins)

* [Задайте разрешенные методы HTTP](#set-the-allowed-http-methods)

* [Задать заголовки запросов](#set-the-allowed-request-headers)

* [Задайте заголовки ответа, предоставляемого](#set-the-exposed-response-headers)

* [Учетные данные в запросов о происхождении](#credentials-in-cross-origin-requests)

* [Задайте срок действия предварительного](#set-the-preflight-expiration-time)

Для некоторых параметров, может оказаться удобным для чтения [работает как CORS](#how-cors-works) первого.

### <a name="set-the-allowed-origins"></a>Задайте разрешенные источники

Чтобы разрешить один или несколько определенных источников:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

Для разрешения всех источников:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

Тщательно обдумайте прежде чем разрешить запросы из любого источника. Это означает, что буквально любой веб-сайт можно вызовы AJAX к вашему API.

### <a name="set-the-allowed-http-methods"></a>Задайте разрешенные методы HTTP

Чтобы разрешить все методы HTTP:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

Это влияет на предварительных запросов и заголовка Access-Control-Allow-Methods.

### <a name="set-the-allowed-request-headers"></a>Задать заголовки запросов

Предварительный запрос CORS может включать заголовок Access-Control-Request-Headers, список заголовков HTTP, установленный приложением (так называемого «author заголовки запроса»).

В список разрешений определенные заголовки:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

Чтобы разрешить все создавать заголовки запроса:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

Браузеры не полностью соответствуют в установке Access-Control-Request-Headers. Если задать заголовки на что-либо отличное от «*», следует включать по крайней мере «принять,» «content-type» и «началом координат», а также любые пользовательские заголовки, которые требуется поддерживать.

### <a name="set-the-exposed-response-headers"></a>Задайте заголовки ответа, предоставляемого

По умолчанию браузер не предоставляет все заголовки ответа для приложения. (См. в разделе [ http://www.w3.org/TR/cors/#simple-response-header ](http://www.w3.org/TR/cors/#simple-response-header).) Заголовки ответа, которые доступны по умолчанию являются:

* Cache-Control

* Content-Language

* Content-Type

* Срок действия истекает

* Дата последнего изменения

* Директивы pragma

Спецификация CORS вызывает эти *заголовки ответа на простой*. Чтобы сделать доступными для приложения другие заголовки:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a>Учетные данные в запросов о происхождении

Учетные данные, требующие особых действий в запрос CORS. По умолчанию браузер не отправляет никаких учетных данных с помощью запроса независимо от источника. Учетные данные содержат файлы cookie, а также схемы проверки подлинности HTTP. Для отправки учетных данных с помощью запроса независимо от источника, клиент должен указать XMLHttpRequest.withCredentials значение true.

Непосредственное использование XMLHttpRequest:

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

В jQuery:

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

Кроме того сервер необходимо разрешить учетные данные. Чтобы разрешить учетные данные от источника:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

Теперь HTTP-ответа будет включать заголовок доступа-элемент управления-Allow-Credentials, который указывает обозревателю, учетные данные для запроса независимо от источника, поддерживает ли сервер.

Если браузер отправляет учетные данные, но ответ не содержит допустимый заголовка Access-элемент управления-Allow-Credentials, браузер не возвращают ответ в приложение, и сбоя запроса AJAX.

Будьте внимательны, если разрешить передачу учетных данных независимо от источника. Веб-сайт в другом домене может отправлять учетные данные вошедшего в систему пользователя приложения от имени пользователя без уведомления пользователя. Спецификация CORS также указывает, что параметр источники, которые можно `"*"` (все источники) является недопустимым при `Access-Control-Allow-Credentials` заголовок отсутствует.

### <a name="set-the-preflight-expiration-time"></a>Задайте срок действия предварительного

Заголовок доступа-элемент управления-Max-Age указывает, как долго можно кэшировать ответ на Предварительный запрос. Чтобы задать этот заголовок:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a>Как работает CORS

В этом разделе описывается, что происходит в запрос CORS на уровне сообщений HTTP. Важно понять, как работает CORS, чтобы политика CORS можно настроить правильно и отладки при возникновении непредвиденному поведению.

Спецификация CORS представляет ряд новых заголовков HTTP, которые позволяют запросы независимо от источника. Если браузер поддерживает CORS, он устанавливает эти заголовки для запросов о происхождении автоматически. Пользовательский код JavaScript не обязательно для включения CORS.

Вот пример запроса независимо от источника. `Origin` Заголовок предоставляет домена сайта, который выполняет запрос:

```
GET http://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: http://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: http://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

Если сервер разрешает запрос, он задает заголовка Access-Control-Allow-Origin в ответе. Значение этого заголовка соответствует заголовку источника из запроса, либо значение подстановочный знак «*», это значит, что допускается любого источника:

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

Если ответ не содержит заголовка Access-Control-Allow-Origin, сбоя запроса AJAX. В частности браузер запрещает запрос. Даже если сервер возвращает успешный ответ, браузер не предоставить ответ клиентскому приложению.

### <a name="preflight-requests"></a>Предварительные запросы

Для некоторых запросов CORS браузер посылает дополнительный запрос, называется «Предварительный запрос,» перед отправкой самого запроса для ресурса. Браузер можно пропустить Предварительный запрос, если выполняются следующие условия:

* Метод запроса — GET, HEAD или POST, и

* Приложение не устанавливает все заголовки запроса, отличные от Accept, Accept-Language, Content-Language, Content-Type или последнего-событие-ID, и

* Заголовок Content-Type (если задать) является одним из следующих:

  * application/x-www-form-urlencoded

  * данные multipart/формы

  * text/plain

Правило о заголовках запроса применяется к заголовки, которые приложение задает путем вызова setRequestHeader для объекта XMLHttpRequest. (Спецификации CORS вызывает эти «заголовки запроса автора»). Правило не применяется к заголовки, которые можно установить браузер, например User-Agent, узла или Content-Length.

Ниже приведен пример Предварительный запрос:

```
OPTIONS http://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: http://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

Возможность предварительного запроса используется метод HTTP OPTIONS. Он включает два специальных заголовков:

* Access-Control-Request-Method: Метод HTTP, будет использоваться для самого запроса.

* Access-Control-Request-Headers: Список заголовков запросов, устанавливающие приложение на самого запроса. (Опять же, это не включает заголовки, которые задает браузера.)

Ниже приведен пример ответа, при условии, что сервер разрешает запрос.

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

Ответ содержит заголовок Access-Control-Allow-Methods, в которой перечислены разрешенные методы и при необходимости заголовок Access-Control-разрешить-Headers, в которой перечислены разрешенные заголовки. Если Предварительный запрос завершается успешно, браузер отправляет фактический запрос, как описано выше.
