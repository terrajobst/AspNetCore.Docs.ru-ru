---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: "Действие приводит к веб-API 2 | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/03/2014
ms.topic: article
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: d0db5c6d45020861d7295ab1db989caee525fff9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="action-results-in-web-api-2"></a>Результаты действий в веб-API 2
====================
по [Mike Wasson](https://github.com/MikeWasson)

В этом разделе описывается, возвращаемое значение веб-API ASP.NET преобразование из действия контроллера в сообщение ответа HTTP.

Действие контроллера Web API может возвращать одно из следующих значений:

1. void
2. **HttpResponseMessage**
3. **IHttpActionResult**
4. Другим типом

В зависимости от того, какой из этих возвращается, веб-API использует другой механизм для создания HTTP-ответа.

| Тип возвращаемого значения | Создание веб-API ответа |
| --- | --- |
| void | Возвращает пустой 204 (нет содержимого) |
| **HttpResponseMessage** | Преобразуйте непосредственно в сообщение ответа HTTP. |
| **IHttpActionResult** | Вызовите **ExecuteAsync** для создания **HttpResponseMessage**, затем преобразовать сообщение ответа HTTP. |
| Другой тип | Записи сериализованного возвращаемого значения в тексте ответа; Возвращает 200 (ОК). |

В остальной части этого раздела описаны все параметры более подробно.

## <a name="void"></a>void

Если тип возвращаемого значения является `void`, веб-API просто возвращает пустой ответ HTTP с кодом состояния 204 (нет содержимого).

Пример контроллера:

[!code-csharp[Main](action-results/samples/sample1.cs)]

HTTP-ответа:

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a>HttpResponseMessage

Если действие возвращает [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), веб-API преобразует возвращаемое значение непосредственно в сообщение ответа HTTP с помощью свойств **HttpResponseMessage** объекта для заполнения ответ.

Этот параметр обеспечивает большую контроля над ответное сообщение. Например следующее действие контроллера задает заголовок Cache-Control.

[!code-csharp[Main](action-results/samples/sample3.cs)]

Ответ

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

Если модель домена, чтобы передать **CreateResponse** метод, использует веб-API [форматирования мультимедиа](../formats-and-model-binding/media-formatters.md) для записи сериализованной модели в тексте ответа.

[!code-csharp[Main](action-results/samples/sample5.cs)]

Для выбора модуля форматирования веб-API использует заголовок Accept в запросе. Дополнительные сведения см. в разделе [согласования содержимого](../formats-and-model-binding/content-negotiation.md).

## <a name="ihttpactionresult"></a>IHttpActionResult

**IHttpActionResult** в веб-API 2 был представлен интерфейс. По существу, он определяет **HttpResponseMessage** фабрики. Ниже приведены некоторые преимущества использования **IHttpActionResult** интерфейс:

- Упрощает [модульное тестирование](../testing-and-debugging/unit-testing-controllers-in-web-api.md) контроллеров.
- Перемещает общую логику для создания HTTP-ответов в отдельные классы.
- Делает назначение яснее, действия контроллера, скрывая сведения низкого уровня построения ответа.

**IHttpActionResult** содержит только один метод **ExecuteAsync**, которая асинхронно создает **HttpResponseMessage** экземпляра.

[!code-csharp[Main](action-results/samples/sample6.cs)]

Если действие контроллера возвращает **IHttpActionResult**, вызывает веб-API **ExecuteAsync** метод для создания **HttpResponseMessage**. Затем он преобразует **HttpResponseMessage** в сообщение ответа HTTP.

Ниже приведен простой реализация из **IHttpActionResult** , создающего формат ответа:

[!code-csharp[Main](action-results/samples/sample7.cs)]

Пример действия контроллера:

[!code-csharp[Main](action-results/samples/sample8.cs)]

Ответ

[!code-console[Main](action-results/samples/sample9.cmd)]

Как правило, используется **IHttpActionResult** реализации, определенные в  **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)**  пространства имен. **ApiController** класс определяет вспомогательные методы, которые возвращают результаты этих встроенных действий.

В следующем примере, если запрос не соответствует существующей код продукта, контроллер вызывает [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) для создания ответа 404 (не найдено). В противном случае вызывает контроллер [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), который создает ответ 200 (ОК), который содержит результат.

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a>Другие возвращаемые типы

Для всех других типов возврата, использует веб-API [форматирования мультимедиа](../formats-and-model-binding/media-formatters.md) для сериализации возвращаемое значение. Сериализованное значение записывает веб-API в тексте ответа. Код состояния ответа: 200 (ОК).

[!code-csharp[Main](action-results/samples/sample11.cs)]

Недостатком этого подхода является, не может непосредственно возвращают код ошибки, такие как 404. Однако можно создать исключение **HttpResponseException** коды ошибок. Дополнительные сведения см. в разделе [обработка исключений в веб-API ASP.NET](../error-handling/exception-handling.md).

Для выбора модуля форматирования веб-API использует заголовок Accept в запросе. Дополнительные сведения см. в разделе [согласования содержимого](../formats-and-model-binding/content-negotiation.md).

Пример запроса

[!code-console[Main](action-results/samples/sample12.cmd)]

Пример ответа:

[!code-console[Main](action-results/samples/sample13.cmd)]
