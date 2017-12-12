---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: "Включение запросов независимо от источника в ASP.NET Web API 2 | Документы Microsoft"
author: MikeWasson
description: "Показано, как поддержка общего доступа к ресурсам независимо от источника (CORS) в веб-API ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/15/2014
ms.topic: article
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 453ad29ff4f10f9660f3aa8bab358519b4cfd48b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="enabling-cross-origin-requests-in-aspnet-web-api-2"></a>Включение запросов независимо от источника в ASP.NET Web API 2
====================
по [Mike Wasson](https://github.com/MikeWasson)

> Безопасность обозревателя предотвращает внесение запросы AJAX в другой домен на веб-странице. Это ограничение называется *политика одного источника*и предотвращает чтение конфиденциальных данных с другого сайта вредоносный сайт. Однако иногда может потребоваться разрешить другие сайты вызвать ваш веб-API.
> 
> [Кросс-общий доступ к ресурсам источника](http://www.w3.org/TR/cors/) (CORS) — это стандарт W3C, которая позволяет серверу ослабить политика одного источника. С помощью CORS, сервер можно явно разрешить некоторые запросы независимо от источника при отклонении другим пользователям. CORS является более безопасным и более гибким, чем ранее методов, например [JSONP](http://en.wikipedia.org/wiki/JSONP). Этого учебника показано, как включить CORS в приложении веб-API.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемая в этом учебнике
> 
> 
> - [Visual Studio 2013 с обновлением 2](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Веб-API 2.2


<a id="intro"></a>
## <a name="introduction"></a>Вступление

Этот учебник демонстрирует поддержку CORS в веб-API ASP.NET. Мы начнем с создания двух проектов ASP.NET — один вызываемой» веб-служба», где размещается контроллер веб-API, и другие вызываемой «WebClient», который вызывает веб-службы. Так как два приложения размещены в разных доменах, AJAX-запросом из веб-клиента для веб-службы — это запрос независимо от источника.

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a>Что такое «Того же происхождения»?

Два URL-адреса имеют того же источника, если они имеют одинаковые схемы, узлов и портов. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Эти два URL-адреса имеют того же источника:

- `http://example.com/foo.html`
- `http://example.com/bar.html`

Эти URL-адреса имеют различные источники, чем предыдущий два:

- `http://example.net`-Другой домен
- `http://example.com:9000/foo.html`-Другой порт
- `https://example.com/foo.html`-Другой схемы
- `http://www.example.com/foo.html`-Другой поддомен

> [!NOTE]
> При сравнении источников Internet Explorer не учитывает порт.


<a id="create-webapi-project"></a>
## <a name="create-the-webservice-project"></a>Создание проекта веб-службы

> [!NOTE]
> В этом разделе предполагается, что вы уже умеете создавать проекты веб-API. См. в противном случае [Приступая к работе с веб-API ASP.NET](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).


Запустите Visual Studio и создайте новый **веб-приложение ASP.NET** проекта. Выберите **пустой** шаблона проекта. В разделе «Добавление папок и основные ссылки для» выберите **веб-API** флажок. При необходимости выберите параметр «Разместить в облаке», чтобы развернуть приложение в Mircosoft Azure. Корпорация Майкрософт предлагает бесплатные услуг хостинга до 10 веб-сайтов в [освободить пробной учетной записи Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

[![](enabling-cross-origin-requests-in-web-api/_static/image3.png)](enabling-cross-origin-requests-in-web-api/_static/image2.png)

Добавить контроллер веб-API с именем `TestController` следующим кодом:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

Можно запустить приложение локально или развернуть в Azure. (Снимки экрана, в этом учебнике, я развертывается в веб-приложениях службы приложений Azure.) Чтобы проверить работоспособность веб-API, перейдите в папку `http://hostname/api/test/`, где *hostname* — это домен, в котором приложение было развернуто. Вы должны увидеть текст ответа, &quot;получить: тестовое сообщение&quot;.

![](enabling-cross-origin-requests-in-web-api/_static/image4.png)

<a id="create-client"></a>
## <a name="create-the-webclient-project"></a>Создание проекта веб-клиента

Создайте другой проект веб-приложения ASP.NET и выберите **MVC** шаблона проекта. При необходимости установите **изменить аутентификацию** > **без проверки подлинности**. Не требуется проверка подлинности для этого учебника.

[![](enabling-cross-origin-requests-in-web-api/_static/image6.png)](enabling-cross-origin-requests-in-web-api/_static/image5.png)

В обозревателе решений откройте файл Views/Home/Index.cshtml. Замените код в этот файл следующее:

[!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

Для *serviceUrl* переменной, используйте URI-адрес веб-службы приложения. Сейчас WebClient приложение запускается локально или опубликовать его в другой веб-сайт.

Нажав кнопку «Попробовать» отправляет запрос AJAX в приложение веб-службы с помощью метода HTTP, перечисленные в раскрывающемся списке (GET, POST или PUT). Это позволяет проверять различные запросы независимо от источника. В данный момент, приложения веб-службы не поддерживают CORS, поэтому при нажатии кнопки, возникнет ошибка.

![](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> Если посмотреть HTTP-трафика в средстве, например [Fiddler](http://www.telerik.com/fiddler), вы увидите, что браузер отправляет запрос GET и запрос выполнен успешно, но вызов AJAX возвращает ошибку. Важно понимать, что политика одного источника не запрещает браузера из *отправки* запроса. Вместо этого он предотвращает приложения просматривать *ответ*.


![](enabling-cross-origin-requests-in-web-api/_static/image8.png)

<a id="enable-cors"></a>
## <a name="enable-cors"></a>Включить CORS

Теперь давайте включить CORS в приложении веб-службы. Во-первых добавьте пакет CORS NuGet. В Visual Studio из **средства** последовательно выберите пункты **диспетчер пакетов библиотеки**, а затем выберите **консоль диспетчера пакетов**. В окне консоли диспетчера пакетов введите следующую команду:

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

Эта команда устанавливает последнюю версию пакета и обновляет все зависимости, в том числе основных библиотек веб-API. Пользователь - флаг версии для конкретной версии. CORS пакету требуется веб-API 2.0 или более поздней версии.

Откройте файл приложения\_Start/WebApiConfig.cs. Добавьте следующий код в **WebApiConfig.Register** метод.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

Добавьте **[EnableCors]** атрибут `TestController` класса:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

Для *источников* параметра, используйте URI, где WebClient приложение было развернуто. Это позволит запросы независимо от источника из веб-клиента, при этом по-прежнему Запрет всех других запросов между доменами. Позднее, будут описаны параметры для **[EnableCors]** более подробно.

Не содержать косую черту в конце *источников* URL-адрес.

Повторно разверните обновленное приложение веб-службы. Обновить веб-клиента не требуется. Теперь запрос AJAX из веб-клиента должны выполняться успешно. Методы GET, PUT и POST все разрешены.

![](enabling-cross-origin-requests-in-web-api/_static/image9.png)

<a id="how-it-works"></a>
## <a name="how-cors-works"></a>Как работает CORS

В этом разделе описывается, что происходит в запрос CORS, на уровне сообщений HTTP. Очень важно понять, как работает CORS, и можно настроить **[EnableCors]** атрибут правильно и устранения неполадок, если не все работает должным образом.

Спецификация CORS представлены несколько заголовки HTTP, которые позволяют запросы независимо от источника. Если браузер поддерживает CORS, он устанавливает эти заголовки автоматически для запросов независимо от источника. не требуется никаких действий в коде JavaScript.

Ниже приведен пример запроса независимо от источника. Заголовок «Источник» предоставляет домену узла, который выполняет запрос.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

Если сервер разрешает запрос, он устанавливает заголовка Access-Control-Allow-Origin. Значение этого заголовка соответствует заголовку источника либо является использование подстановочного знака «\*», это значит, что разрешены любые источники.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

Если ответ содержит Access-Control-Allow-Origin заголовок, происходит сбой запроса AJAX. В частности браузер блокирует запрос. Даже если сервер возвращает успешный ответ, браузер не делает ответ доступной клиентскому приложению.

**Предварительные запросы**

Для некоторых запросов CORS браузер отправляет запрос на дополнительные, называется «Предварительный запрос,» перед отправкой самого запроса для ресурса.

Браузер может пропустить Предварительный запрос, если выполняются следующие условия:

- Метод запроса является GET, HEAD или POST, *и*
- Приложение не устанавливает все заголовки запросов, отличные от Accept, Accept-Language, Content-Language, Content-Type или последнего-событие-ID, *и*
- Заголовок Content-Type (если задать) является одним из следующих: 

    - application/x-www-form-urlencoded
    - данные multipart/формы
    - text/plain.

Правило о заголовках запроса применяется к заголовки, которые приложение задает путем вызова **setRequestHeader** на **XMLHttpRequest** объекта. (Спецификации CORS вызывает эти «автор запроса заголовки»). Правило не применяется к заголовкам *браузера* можно задать, например User-Agent, узлу или Content-Length.

Ниже приведен пример Предварительный запрос:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

Предварительный запрос с помощью метода HTTP OPTIONS. Он включает два особых заголовков:

- Access-Control-Request-Method: HTTP метод, который будет использоваться для самого запроса.
- Access-Control-Request-Headers: Список заголовков запроса, *приложения* задать для самого запроса. (Опять же, это не включает заголовки, которые задает браузера.)

Ниже приведен пример ответа, при условии, что сервер разрешает запрос:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

Ответ включает и-методы управления доступом — разрешить заголовок, который содержит список допустимых методов и при необходимости заголовок Access-Control-разрешить-Headers, в котором перечислены разрешенные заголовки. Если Предварительный запрос завершается успешно, браузер отправляет сам запрос, как описано выше.

<a id="scope"></a>
## <a name="scope-rules-for-enablecors"></a>Правила областей для [EnableCors]

Можно включить CORS каждого действия каждого контроллера или глобально для всех контроллеров веб-API в приложении.

**Каждого действия**

Чтобы включить CORS для одного действия, установите **[EnableCors]** атрибута в методе действия. В следующем примере включается CORS для `GetItem` только метод.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

**На каждый контроллер**

Если задать **[EnableCors]** класса контроллера, оно применяется ко всем действиям в контроллере. Чтобы отключить CORS для действия, добавьте **[DisableCors]** атрибут действия. В следующем примере включается CORS для каждого метода, за исключением `PutItem`.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

**Глобально**

Чтобы включить CORS для всех контроллеров веб-API в приложении, необходимо передать **EnableCorsAttribute** экземпляр **EnableCors** метод:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

Если задать атрибут на более чем одну область, является порядок приоритета:

1. Действие
2. Контроллер
3. Global

<a id="allowed-origins"></a>
## <a name="set-the-allowed-origins"></a>Задайте разрешенные источники

*Источников* параметр **[EnableCors]** атрибут указывает, какие источники, которые получают доступ к ресурсу. Значение — список разрешенных источников с разделителями запятыми.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

Можно также использовать использование подстановочного знака "\*" разрешать запросы от любого источников.

Внимательно рассмотрите перед предоставлением запросов из любого источника. Он означает, что практически любой веб-сайта можно вносить вызовы AJAX для веб-API.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

<a id="allowed-methods"></a>
## <a name="set-the-allowed-http-methods"></a>Набор разрешенных HTTP-методы

*Методы* параметр **[EnableCors]** атрибут задает HTTP-методы, получают доступ к ресурсу. Чтобы разрешить все методы, используйте подстановочный знак значение «\*». В следующем примере пользователь запросов GET и POST.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

<a id="allowed-request-headers"></a>
## <a name="set-the-allowed-request-headers"></a>Задать заголовки запросов

Ранее я описал как Предварительный запрос может включать заголовок Access-Control-Request-Headers список заголовков HTTP, установленный приложением (так называемого «author заголовки запроса»). *Заголовки* параметр **[EnableCors]** атрибут указывает, какие заголовки запроса автор разрешены. Чтобы разрешить все заголовки, задайте *заголовки* для «\*». Чтобы белый список специальные заголовки, установите *заголовки* в список с разделителями запятыми допустимых заголовков:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

Однако браузеры не полностью соответствуют в установке Access-Control-Request-Headers. Например в настоящее время включает Chrome «источник»; Хотя FireFox не включает стандартные заголовки, такие как «Accept», даже в том случае, когда приложение устанавливает их в скрипт.

Если задать *заголовки* на что-либо, отличное от «\*», следует включать по крайней мере «принять,» «content-type» и «источник», а также любые пользовательские заголовки, которые требуется поддерживать.

<a id="allowed-response-headers"></a>
## <a name="set-the-allowed-response-headers"></a>Задать заголовки ответа разрешенных

По умолчанию браузер не предоставляет все заголовки ответа для приложения. Заголовки ответа, которые доступны по умолчанию являются:

- Cache-Control
- Content-Language
- Тип содержимого
- Срок действия истекает
- Дата последнего изменения
- Директивы pragma

Спецификация CORS вызывает эти [заголовки ответа на простой](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header). Чтобы настроить другие заголовки, доступны для приложения, установить *exposedHeaders* параметр **[EnableCors]**.

В следующем примере, контроллер `Get` метод задает пользовательский заголовок с именем «X пользовательский заголовок». По умолчанию браузер не будет предоставлять этот заголовок в запросе независимо от источника. Чтобы сделать доступным заголовок, включают «X пользовательский заголовок» в *exposedHeaders*.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

<a id="credentials"></a>
## <a name="passing-credentials-in-cross-origin-requests"></a>Передача учетных данных в запросы независимо от источника

Учетные данные, требующие особых действий в запрос CORS. По умолчанию браузер не отправляет никаких учетных данных с помощью запроса независимо от источника. Учетные данные включают файлы cookie, а также схемы проверки подлинности HTTP. Для отправки учетных данных с помощью запроса независимо от источника, клиент должен указать **XMLHttpRequest.withCredentials** значение true.

С помощью **XMLHttpRequest** напрямую:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

В jQuery:

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

Кроме того сервер необходимо разрешить учетные данные. Чтобы разрешить независимо от источника учетные данные в веб-API, задайте **SupportsCredentials** свойство значение true в **[EnableCors]** атрибута:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

Если это свойство имеет значение true, HTTP-ответ будет содержать заголовок доступа-элемент управления-Allow-Credentials. Этот заголовок предписывает браузеру, поддерживает ли сервер учетные данные для запроса независимо от источника.

Если браузер отправляет учетные данные, но ответа не содержит допустимый заголовок доступа-элемент управления-Allow-Credentials, браузер не предоставляет приложению ответ и происходит сбой запроса AJAX.

Должны быть очень осторожными параметр **SupportsCredentials** значение true, так как это означает, что веб-сайт в другом домене может отправлять учетные данные вошедшего в систему пользователя веб-API от имени пользователя без оповещения пользователя. Спецификация CORS указывается этот параметр *источников* для &quot; \* &quot; недопустим при **SupportsCredentials** имеет значение true.

<a id="cors-policy-providers"></a>
## <a name="custom-cors-policy-providers"></a>Поставщики пользовательских CORS политики

**[EnableCors]** атрибут реализует **ICorsPolicyProvider** интерфейса. Можно предоставить собственную реализацию, создав класс, производный от **атрибута** и реализует **ICorsProlicyProvider**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

Теперь можно применить атрибут, в любом месте, что нужно будет только разместить **[EnableCors]**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

Например пользовательский поставщик политики CORS удалось считать параметры из файла конфигурации.

В качестве альтернативы, используя атрибуты, можно зарегистрировать **ICorsPolicyProviderFactory** объект, который создает **ICorsPolicyProvider** объектов.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

Чтобы задать **ICorsPolicyProviderFactory**, вызовите **SetCorsPolicyProviderFactory** метод расширения во время запуска, как показано ниже:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

<a id="browser-support"></a>
## <a name="browser-support"></a>Поддержка браузеров

Пакет Web API CORS не серверные технологии. В браузере пользователя необходимо также поддерживают CORS. К счастью, все основные обозреватели текущие версии включают [поддержка CORS](http://caniuse.com/cors).

Internet Explorer 8 и Internet Explorer 9 имеют частичная поддержка CORS, вместо прежних версий XDomainRequest, который обеспечивает объект XMLHttpRequest. Дополнительные сведения см. в разделе [XDomainRequest, который обеспечивает - ограничения, ограничения и способы решения проблем](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).
