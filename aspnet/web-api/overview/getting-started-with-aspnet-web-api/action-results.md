---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Действие приводит к веб-API 2 | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/03/2014
ms.topic: article
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: 7726829ac9eba339ff3ac1c94c86132cb1090783
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395533"
---
<a name="action-results-in-web-api-2"></a>Результаты действий в веб-API 2
====================
по [Майк Уоссон](https://github.com/MikeWasson)

Здесь описывается, как веб-API ASP.NET преобразует возвращаемое значение из действия контроллера в ответное сообщение HTTP.

Действие контроллера веб-API может возвращать одно из следующих:

1. void
2. **HttpResponseMessage**
3. **IHttpActionResult**
4. Какое-либо иное

В зависимости от их возвращается, веб-API использует другой механизм для создания HTTP-ответа.

| Тип возвращаемого значения | Как веб-API создает ответ |
| --- | --- |
| void | Возвращает пустой 204 (нет содержимого) |
| **HttpResponseMessage** | Преобразуйте непосредственно в сообщение ответа HTTP. |
| **IHttpActionResult** | Вызовите **ExecuteAsync** для создания **HttpResponseMessage**, затем преобразовать сообщение ответа HTTP. |
| Другой тип | Записи сериализованного возвращаемое значение в тексте ответа; возвращать ответ 200 (ОК). |

В остальной части этого раздела описаны все параметры более подробно.

## <a name="void"></a>void

Если возвращаемый тип — `void`, веб-API просто возвращает пустой ответ HTTP с кодом состояния 204 (нет содержимого).

Пример контроллера:

[!code-csharp[Main](action-results/samples/sample1.cs)]

HTTP-ответа:

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a>HttpResponseMessage

Если действие возвращает [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), веб-API преобразует возвращаемое значение непосредственно в ответное сообщение HTTP с помощью свойств **HttpResponseMessage** объекта для заполнения ответ.

Этот параметр обеспечивает большую контроля над ответного сообщения. Например следующее действие контроллера задает заголовок Cache-Control.

[!code-csharp[Main](action-results/samples/sample3.cs)]

Ответ

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

Если передать модель домена, чтобы **CreateResponse** метод, веб-API использует [форматирования мультимедиа](../formats-and-model-binding/media-formatters.md) для записи сериализованной модели в тексте ответа.

[!code-csharp[Main](action-results/samples/sample5.cs)]

Веб-API использует заголовок Accept в запросе для выбора модуля форматирования. Дополнительные сведения см. в разделе [согласование содержимого](../formats-and-model-binding/content-negotiation.md).

## <a name="ihttpactionresult"></a>IHttpActionResult

**IHttpActionResult** в веб-API 2 был представлен интерфейс. По сути, он определяет **HttpResponseMessage** фабрики. Ниже приведены некоторые преимущества использования **IHttpActionResult** интерфейса:

- Упрощает [модульное тестирование](../testing-and-debugging/unit-testing-controllers-in-web-api.md) контроллеров.
- Перемещает общую логику для создания HTTP-ответов в отдельные классы.
- Делает цель более понятным, действие контроллера, скрывая низкоуровневые сведения о создании ответа.

**IHttpActionResult** содержит один метод, **ExecuteAsync**, которая асинхронно создает **HttpResponseMessage** экземпляра.

[!code-csharp[Main](action-results/samples/sample6.cs)]

Возвращает действие контроллера **IHttpActionResult**, вызывает веб-API **ExecuteAsync** метод для создания **HttpResponseMessage**. То она преобразует **HttpResponseMessage** в ответное сообщение HTTP.

Ниже приведен простой реализация из **IHttpActionResult** , создающий формат ответа:

[!code-csharp[Main](action-results/samples/sample7.cs)]

Пример действия контроллера:

[!code-csharp[Main](action-results/samples/sample8.cs)]

Ответ

[!code-console[Main](action-results/samples/sample9.cmd)]

Более часто, вы воспользуетесь **IHttpActionResult** реализации, определенных в **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** пространства имен. **ApiController** класс определяет вспомогательные методы, которые возвращают результаты этих встроенных действий.

В следующем примере, если запрос не совпадает с Идентификатором существующего продукта, контроллер вызывает [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) для создания ответа 404 (не найдено). В противном случае вызывает контроллер [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), который создает ответ 200 (ОК), содержащий продукта.

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a>Другие типы возвращаемого значения

Для всех других типов возвращаемого значения, веб-API использует [форматирования мультимедиа](../formats-and-model-binding/media-formatters.md) для сериализации возвращаемого значения. Веб-API записывает сериализованное значение в тексте ответа. Код состояния ответа: 200 (ОК).

[!code-csharp[Main](action-results/samples/sample11.cs)]

Недостаток этого подхода является то, что нельзя напрямую возвращать код ошибки, например 404. Тем не менее, можно создавать **HttpResponseException** для кодов ошибок. Дополнительные сведения см. в разделе [обработка исключений в веб-API ASP.NET](../error-handling/exception-handling.md).

Веб-API использует заголовок Accept в запросе для выбора модуля форматирования. Дополнительные сведения см. в разделе [согласование содержимого](../formats-and-model-binding/content-negotiation.md).

Пример запроса

[!code-console[Main](action-results/samples/sample12.cmd)]

Пример ответа:

[!code-console[Main](action-results/samples/sample13.cmd)]
