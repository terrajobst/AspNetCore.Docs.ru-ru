---
uid: web-api/overview/advanced/http-cookies
title: Файлы cookie HTTP в ASP.NET Web API | Документы Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/17/2012
ms.topic: article
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 363ca975cf75b635b766a53eeda87cf957eed60c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
---
<a name="http-cookies-in-aspnet-web-api"></a>Файлы cookie HTTP в веб-API ASP.NET
====================
по [Mike Wasson](https://github.com/MikeWasson)

Описывается, как отправлять и получать файлы cookie HTTP в веб-API.

## <a name="background-on-http-cookies"></a>Основные сведения о файлы cookie HTTP

В этом разделе содержится краткий обзор о реализации файлы cookie на уровне HTTP. Дополнительные сведения, обратитесь к [RFC 6265](http://tools.ietf.org/html/rfc6265).

Файл cookie — это часть данных, сервер отправляет в ответ HTTP. Клиент (необязательно) сохраняет файл cookie и возвращает его на subsequet запросы. Это позволяет совместно использовать состояние клиента и сервера. Чтобы задать файл cookie, сервер включает заголовок Set-Cookie в ответе. Формат файла cookie — это пара имя значение с дополнительными атрибутами. Пример:

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

Ниже приведен пример с атрибутами:

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

Чтобы вернуть куки-файл на сервер, клиент включает заголовок файла Cookie в последующих запросах.

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

HTTP-ответа, может включать несколько заголовков Set-Cookie.

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

Клиент возвращает несколько файлов cookie, с помощью одного заголовка файла Cookie.

[!code-console[Main](http-cookies/samples/sample5.cmd)]

Область и длительность файла cookie, определяются следующие атрибуты в заголовок Set-Cookie:

- **Домен**: сообщает клиенту, какой домен должен получить куки-файл. Например если домен «example.com», клиент возвращает куки-файл каждый поддомен example.com. Если не указан, указан домен исходного сервера.
- **Путь**: ограничивает куки-файл по указанному пути в пределах домена. Если не указан, используется путь запроса URI.
- **Срок действия истекает**: задает дату окончания срока действия файла cookie. Клиент удаляет файл cookie, после истечения срока его действия.
- **Max-Age**: задает максимальное время существования файлов cookie. Клиент удаляет файл cookie, при достижении максимального возраста.

Если оба `Expires` и `Max-Age` заданы, `Max-Age` имеет более высокий приоритет. Если не задан ни один клиент удаляет файл cookie при завершении текущего сеанса. (Точное значение «сеанс» определяется агентом пользователя.)

Однако имейте в виду, что клиенты могут игнорировать файлы cookie. Например пользователь может отключить файлы cookie для соблюдения конфиденциальности. Клиенты могут удалять файлы cookie, до истечения срока действия или ограничить количество хранящихся файлов cookie. По соображениям конфиденциальности часто отклонять куки-файлы «third party», где домен не совпадает с исходного сервера. Иными словами сервер не следует полагаться на получение файлов cookie, которые он задает.

## <a name="cookies-in-web-api"></a>Файлы cookie в веб-API

Чтобы добавить файл cookie HTTP-ответа, создать **CookieHeaderValue** экземпляр, представляющий файл cookie. Затем вызовите **AddCookies** метод расширения, который определен в **System.Net.Http. HttpResponseHeadersExtensions** класса, чтобы добавить файл cookie.

Например следующий код добавляет файл cookie в пределах действия контроллера:

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

Обратите внимание, что **AddCookies** принимает массив **CookieHeaderValue** экземпляров.

Чтобы извлечь файлы cookie из запроса клиента, вызовите **GetCookies** метод:

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

Объект **CookieHeaderValue** содержит коллекцию **CookieState** экземпляров. Каждый **CookieState** представляет один файл cookie. Метод индексатор для получения **CookieState** по имени, как показано.

## <a name="structured-cookie-data"></a>Cookie структурированных данных

Большинство браузеров ограничить количество файлов cookie, они будут храниться&#8212;общее число и число в домене. Таким образом его можно использовать для размещения структурированных данных в одном файле cookie, вместо задания нескольких файлов cookie.

> [!NOTE]
> RFC 6265 не определяет структуру данных файлов cookie.


С помощью **CookieHeaderValue** класса, можно передать список пар имя значение для данных файлов cookie. Эти пары "имя значение", кодируются как данные формы, закодированные в URL-адрес в заголовке Set-Cookie:

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

Предыдущий код выводит следующий заголовок Set-Cookie:

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

**CookieState** класс предоставляет метод индексатора для чтения вложенные значения из куки-файла в сообщении запроса:

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a>Пример: Задание и получение файлов cookie в обработчике сообщений

В предыдущих примерах показано, как использовать файлы cookie из контроллера веб-API. Другой вариант — использовать [обработчиков сообщений](http-message-handlers.md). Обработчики сообщений вызываются ранее в конвейере контроллеры. Обработчик сообщений можно считывать файлы cookie из запроса, прежде чем запрос достигнет контроллер или добавьте файлы cookie в ответ после контроллера формирует ответ.

![](http-cookies/_static/image2.png)

В следующем коде показано создание идентификаторов сеансов обработчик сообщений. Идентификатор сеанса хранится в файле cookie. Обработчик проверяет запрос файл cookie сеанса. Если запрос не содержит файл cookie, обработчик создает идентификатор нового сеанса. В любом случае обработчик сохраняет идентификатор сеанса в **HttpRequestMessage.Properties** контейнер свойств. Он также добавляет файл cookie сеанса HTTP-ответа.

Эта реализация не выполняет проверку, сервер фактически выдан идентификатор сеанса от клиента. Не используйте его как один из видов проверки подлинности! Точка примера является демонстрация управления файла cookie HTTP.

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

Контроллер можно получить идентификатор сеанса из **HttpRequestMessage.Properties** контейнер свойств.

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
