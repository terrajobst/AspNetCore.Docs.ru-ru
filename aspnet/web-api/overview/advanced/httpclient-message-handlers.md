---
uid: web-api/overview/advanced/httpclient-message-handlers
title: Обработчики сообщений HttpClient в веб-API ASP.NET | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 10/01/2012
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: db9edcf4fb31e967c3d4e7635f96c68829aec97d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831376"
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a>Обработчики сообщений HttpClient в веб-API ASP.NET
====================
по [Майк Уоссон](https://github.com/MikeWasson)

Объект *обработчик сообщений* — это класс, который получает HTTP-запрос и возвращает ответ HTTP.

Как правило ряд обработчиков сообщений соединяются друг с другом. Первый обработчик получает запрос HTTP, обрабатывает и предоставляет запрос к следующий обработчик. Рано или поздно ответа создается и возвращается в цепочке. Такая модель называется *делегирование* обработчика.

![](httpclient-message-handlers/_static/image1.png)

На стороне клиента **HttpClient** класс использует обработчик сообщений для обработки запросов. Обработчик по умолчанию является **HttpClientHandler**, который отправляет запрос по сети и получает ответ от сервера. Вы можете вставить обработчики пользовательских сообщений в конвейер клиента:

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> Веб-API ASP.NET также использует обработчики сообщений на стороне сервера. Дополнительные сведения см. в разделе [обработчиков сообщений HTTP](http-message-handlers.md).


## <a name="custom-message-handlers"></a>Обработчики пользовательских сообщений

Чтобы написать обработчик пользовательского сообщения, являются производными от **System.Net.Http.DelegatingHandler** и переопределить **SendAsync** метод. Ниже представлена подпись метода:

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

Этот метод принимает **HttpRequestMessage** как входных данных и асинхронно возвращает **HttpResponseMessage**. Типичной реализации выполняет следующие функции:

1. Процесс сообщения запроса.
2. Вызовите `base.SendAsync` для отправки запроса на внутренний обработчик.
3. Внутренний обработчик возвращает ответное сообщение. (Этот шаг выполняется асинхронно).
4. Обработать ответ и вернуть его вызывающей стороне.

В следующем примере обработчик сообщений, который добавляет пользовательский заголовок исходящего запроса:

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

Вызов `base.SendAsync` является асинхронным. Если обработчик не выполняет никакой работы после этого вызова, используйте **await** ключевое слово для возобновления выполнения после завершения работы метода. Пример обработчика, который регистрирует коды ошибок. Ведение журнала, сам не очень интересно, но примере показано, как получить в ответ внутри обработчика.

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a>Добавление обработчиков сообщений в конвейер, клиент

Добавление пользовательских обработчиков для **HttpClient**, использовать **HttpClientFactory.Create** метод:

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

Обработчики сообщений вызываются в порядке, который можно передать их в **создать** метод. Так как обработчики являются вложенными, ответное сообщение перемещается в обратном направлении. Последним обработчиком является первым ответного сообщения.
