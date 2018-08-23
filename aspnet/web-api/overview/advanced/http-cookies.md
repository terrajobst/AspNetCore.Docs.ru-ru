---
uid: web-api/overview/advanced/http-cookies
title: Файлы cookie HTTP в веб-API ASP.NET | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: riande
ms.date: 09/17/2012
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 61e0c47efdd92a3a0b329930aeec757b446eb9b8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838236"
---
<a name="http-cookies-in-aspnet-web-api"></a>Файлы cookie HTTP в веб-API ASP.NET
====================
по [Майк Уоссон](https://github.com/MikeWasson)

В этом разделе описывается, как отправлять и получать файлы cookie HTTP в веб-API.

## <a name="background-on-http-cookies"></a>Фон с помощью файлов cookie HTTP

В этом разделе дается краткий обзор реализации файлы cookie на уровне HTTP. Дополнительные сведения см. в [RFC 6265](http://tools.ietf.org/html/rfc6265).

Файл cookie — это часть данных, которые сервер отправляет HTTP-ответов. Клиент (при необходимости) сохраняет файл cookie и возвращает его при запросах subsequet. Это позволяет использовать общие состояния клиента и сервера. Чтобы задать файл cookie, сервер включает заголовок Set-Cookie в ответе. Формат файла cookie — это пара имя значение, с дополнительными атрибутами. Пример:

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

Ниже приведен пример с атрибутами:

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

Чтобы вернуть файл cookie на сервер, клиент включает заголовок Cookie в последующих запросах.

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

HTTP-ответ может включать несколько заголовков Set-Cookie.

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

Клиент возвращает несколько файлов cookie, используя один заголовок файла Cookie.

[!code-console[Main](http-cookies/samples/sample5.cmd)]

Объема и продолжительности файла cookie управляются следующие атрибуты в заголовке Set-Cookie:

- **Домен**: сообщает клиенту, какой домен должен получать куки-файл. Например если домен — «example.com», клиент возвращает файл cookie в каждом поддомен example.com. Если не указан, домен — на сервере-источнике.
- **Путь**: ограничивает куки-файл по указанному пути в пределах домена. Если не указан, используется путь к URI запроса.
- **Срок действия истекает**: задает дату окончания срока действия файла cookie. Клиент удаляет файл cookie, когда истечет срок его действия.
- **Max-Age**: задает максимальное время существования файлов cookie. Клиент удаляет файл cookie, когда будет достигнуто максимальное время существования.

Если оба `Expires` и `Max-Age` заданы, `Max-Age` имеет более высокий приоритет. Если не задан, клиент удаляет файл cookie после завершения текущего сеанса. (Точное значение «сеанс» определяется агентом пользователя.)

Однако имейте в виду, что клиенты могут игнорировать файлы cookie. Например пользователь может отключить файлы cookie для соблюдения конфиденциальности. Клиенты могут удалять файлы cookie, до истечения срока действия или ограничить количество хранящихся файлов cookie. По соображениям конфиденциальности клиенты отклоняют часто файлы cookie «third party», где домен соответствует на сервере-источнике. Короче говоря сервер не следует полагаться на получение, на которые он задает файлы cookie.

## <a name="cookies-in-web-api"></a>Файлы cookie в веб-API

Чтобы добавить маркер в HTTP-ответ, создайте **CookieHeaderValue** экземпляр, представляющий файл cookie. Затем вызовите **AddCookies** метод расширения, который определен в **System.Net.Http. HttpResponseHeadersExtensions** класс, чтобы добавить файл cookie.

Например следующий код добавляет файл cookie в действие контроллера:

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

Обратите внимание, что **AddCookies** принимает массив **CookieHeaderValue** экземпляров.

Чтобы извлечь файлы cookie запроса клиента, вызовите **GetCookies** метод:

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

Объект **CookieHeaderValue** содержит коллекцию **CookieState** экземпляров. Каждый **CookieState** представляет один файл cookie. Метод индексатор для получения **CookieState** по имени, как показано.

## <a name="structured-cookie-data"></a>Файл Cookie структурированных данных

Многие браузеры ограничить количество файлов cookie, они будут храниться&#8212;как общее количество, так и номер каждого домена. Таким образом можно попробовать установить структурированных данных в единый файл cookie, а не задавать несколько файлов cookie.

> [!NOTE]
> RFC 6265 не определяет структуру данных файлов cookie.


С помощью **CookieHeaderValue** класса, можно передать список пар имя значение для данных файлов cookie. Эти пары "имя значение" кодируются в виде URL-адреса формы данных в заголовке Set-Cookie:

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

Приведенный выше код создает следующий заголовок Set-Cookie:

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

**CookieState** класс предоставляет метод индексатора для считывания вложенных значений из файла cookie в сообщении запроса:

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a>Пример: Задание и получение файлов cookie в обработчик сообщений

В предыдущих примерах показано, как использовать файлы cookie из контроллера веб-API. Другой вариант — использовать [обработчиков сообщений](http-message-handlers.md). Ранее в конвейере контроллеры вызываются обработчики сообщений. Обработчик сообщений можно считать файлы cookie из запроса, прежде чем они достигнут контроллера или добавьте файлы cookie в ответ после контроллер создает ответ.

![](http-cookies/_static/image2.png)

В следующем коде показано обработчик сообщений для создания идентификаторов сеансов. Идентификатор сеанса хранится в файле cookie. Обработчик проверяет запрос файл cookie сеанса. Если запрос не содержит файл cookie, обработчик создает идентификатор нового сеанса. В любом случае обработчик сохраняет идентификатор сеанса в **HttpRequestMessage.Properties** контейнер свойств. Он также добавляет файл cookie сеанса для HTTP-ответа.

Эта реализация не проверяет, что идентификатор сеанса от клиента, фактически был выдан сервером. Не используйте его как форму проверки подлинности! Точка примера — Показать куки-файлы HTTP.

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

Контроллер можно получить идентификатор сеанса из **HttpRequestMessage.Properties** контейнер свойств.

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
