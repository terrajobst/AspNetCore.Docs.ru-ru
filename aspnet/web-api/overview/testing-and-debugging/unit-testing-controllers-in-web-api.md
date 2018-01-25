---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: "Модульное тестирование контроллеров в ASP.NET Web API 2 | Документы Microsoft"
author: MikeWasson
description: "В этом разделе описываются некоторые конкретные методы модульного тестирования контроллеров в веб-API 2. Перед прочтением этого раздела, можно прочитать учебник единицы..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/11/2014
ms.topic: article
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: bda5148a4c1553d70f3173de66371fbb8576e83f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a>Модульное тестирование контроллеров в ASP.NET Web API 2
====================
по [Mike Wasson](https://github.com/MikeWasson)

> В этом разделе описываются некоторые конкретные методы модульного тестирования контроллеров в веб-API 2. Перед прочтением этого раздела, может потребоваться чтение учебника [веб-тестирования ASP.NET единицы API 2](unit-testing-with-aspnet-web-api.md), показано, как добавить в решение проект модульного теста.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемая в этом учебнике
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Веб-API 2
> - [Заказа](https://github.com/Moq) 4.5.30

> [!NOTE]
> Я использовал заказа, но тот же принцип применим к любой платформа имитации. Заказа 4.5.30 (и более поздние версии) поддерживает 2017 г. для Visual Studio, Roslyn .NET 4.5 и более поздние версии.

Общий шаблон в модульные тесты — &quot;упорядочить act-assert&quot;:

- Упорядочить: Настройте все необходимые компоненты для выполнения тестов.
- ACT: Выполнения теста.
- Assert: Убедитесь, что тест успешно.

На шаге упорядочить часто используемые макетов или объектов заглушки. Сводит к минимуму число зависимостей, поэтому теста основное внимание уделено тестирование одно.

Ниже приведены некоторые аспекты, которые следует модульного теста, в контроллерах веб-API.

- Действие возвращает правильный тип ответа.
- Недопустимые параметры возврата ответа исправить ошибку.
- Действие вызывает правильный метод слоя репозитория или службы.
- Если ответ включает модель домена, проверьте тип модели.

Вот некоторые общие задачи, чтобы проверить, но конкретная обработка зависит от реализации контроллера. В частности, он оказывает серьезное влияние ли возвращать ваши действия контроллера **HttpResponseMessage** или **IHttpActionResult**. Дополнительные сведения об этих типов результата см. в разделе [результаты действий в веб-Api 2](../getting-started-with-aspnet-web-api/action-results.md).

## <a name="testing-actions-that-return-httpresponsemessage"></a>Действия тестирования, которые возвращают HttpResponseMessage

Ниже приведен пример контроллера которого возврат действия **HttpResponseMessage**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

Обратите внимание, контроллер внедрения зависимостей используется для вставки `IProductRepository`. Делает более тестируемым контроллером так, как можно внедрить макет репозитория. Следующий модульный тест проверяет, что `Get` метод записи `Product` в тексте ответа. Предполагается, что `repository` является макетирование `IProductRepository`.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

Очень важно задать **запроса** и **конфигурации** на контроллере. В противном случае тест завершится с **ArgumentNullException** или **InvalidOperationException**.

## <a name="testing-link-generation"></a>Тестирование компоновки

`Post` Вызовы метода **UrlHelper.Link** для создания ссылок в ответе. Это требует немного дополнительной настройки в модульном тесте:

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

**UrlHelper** класс должен запроса URL-адрес и данные маршрута, поэтому тест должен задать значения для этих. Другой вариант — макетов или заглушки **UrlHelper**. При таком подходе, замените значение по умолчанию [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) с версией макетов или заглушки, которая возвращает фиксированное значение.

Давайте перепишите тестирования с помощью [заказа](https://github.com/Moq) framework. Установить `Moq` пакет NuGet в тестовом проекте.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

В этой версии не требуется настроить все данные о маршруте, так как макетов **UrlHelper** возвращает строковую константу.


## <a name="testing-actions-that-return-ihttpactionresult"></a>Действия тестирования, которые возвращают IHttpActionResult

В веб-API 2 может возвращать действие контроллера **IHttpActionResult**, который является аналогом **ActionResult** в ASP.NET MVC. **IHttpActionResult** интерфейс определяет шаблон команды для создания ответов HTTP. Вместо того чтобы создавать ответ непосредственно, возвращает ли контроллер **IHttpActionResult**. Позже, вызывает конвейер **IHttpActionResult** для создания контракта. Этот подход упрощает написание модульных тестов, из-за большой объем, необходимый для установки можно пропустить **HttpResponseMessage**.

Ниже приведен пример контроллера которого возврат действия **IHttpActionResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

В этом примере показаны некоторые распространенные шаблоны с помощью **IHttpActionResult**. Давайте посмотрим, как для модульного тестирования их.

### <a name="action-returns-200-ok-with-a-response-body"></a>Действие возвращает 200 (ОК) с текстом ответа

`Get` Вызовы метода `Ok(product)` найденные продукта. В рамках модульного теста, убедитесь, что возвращаемый тип — **OkNegotiatedContentResult** и возвращаемый продукт имеет идентификатор справа.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

Обратите внимание, что модульный тест не выполняет результат действия. Можно предположить, что результат действия правильно создает HTTP-ответа. (Вот почему платформа веб-API имеет свой собственный модульные тесты!)

### <a name="action-returns-404-not-found"></a>Действие возвращает 404 (не найдено)

`Get` Вызовы метода `NotFound()` Если продукта не найден. В этом случае модульный тест проверяет, точно так же, если возвращаемый тип — **NotFoundResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a>Действие возвращает 200 (ОК) без тела ответа

`Delete` Вызовы метода `Ok()` для возврата пустой ответ HTTP 200. Как и в предыдущем примере модульный тест проверяет возвращаемый тип, в этом случае **OkResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a>Действие возвращает 201 (создано) с заголовок расположения

`Post` Вызовы метода `CreatedAtRoute` для возврата ответа HTTP 201 с URI в заголовке Location. В рамках модульного теста убедитесь, что действие задает правильные значения маршрутизации.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a>Действие возвращает другой 2xx с текстом ответа

`Put` Вызовы метода `Content` для возврата ответа HTTP 202 (принято) с текстом ответа. Этот случай похож на возвращение 200 (ОК), но модульного теста необходимо также проверить код состояния.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Макетирование Entity Framework при модульного тестирования ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- [Написание тестов для службы веб-API ASP.NET](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (запись блога по Youssef Moussaoui).
- [Отладка ASP.NET Web API с отладчиком маршрута](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
