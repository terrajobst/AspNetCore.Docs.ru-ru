---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Модульное тестирование контроллеров в ASP.NET Web API 2 | Документация Майкрософт
author: MikeWasson
description: В этом разделе описывается определенных приемов контроллеры модульного тестирования в веб-API 2. Перед прочтением этого раздела, может потребоваться ознакомьтесь с руководством единицы...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/11/2014
ms.topic: article
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 1a3cfa1962a5f914fd2393088bec4424f6453d07
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389387"
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a>Модульное тестирование контроллеров в ASP.NET Web API 2
====================
по [Майк Уоссон](https://github.com/MikeWasson)

> В этом разделе описывается определенных приемов контроллеры модульного тестирования в веб-API 2. Перед прочтением этого раздела, ознакомьтесь с руководством может потребоваться [модульного тестирования ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), который показывает, как добавить в решение проект модульного теста.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемые в этом руководстве
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Веб-API 2
> - [MOQ](https://github.com/Moq) 4.5.30

> [!NOTE]
> Я использовал Moq, но тот же принцип применим для любой платформы макетирования. MOQ 4.5.30 (и более поздних версий) поддерживает Visual Studio 2017, Roslyn и .NET 4.5 и более поздних версий.

Распространенный подход в модульных тестах — &quot;упорядочить act утверждение&quot;:

- Упорядочить: Настройте все необходимые компоненты для выполнения тестов.
- ACT: Выполните тест.
- Assert: Убедитесь, что тест успешно.

На этапе компоновки будут часто использовать макет или заглушки объектов. Сводит к минимуму число зависимостей, чтобы тест предназначен для тестирования одну вещь.

Вот некоторые действия, что следует подвергать модульному тестированию в контроллерах веб-API.

- Это действие возвращает правильный тип ответа.
- Недопустимые параметры возврата ответа исправить ошибку.
- Действие вызывает правильный метод на уровне репозитория или службы.
- Если ответ содержит модель предметной области, проверьте тип модели.

Ниже перечислены некоторые общие действия для тестирования, но конкретная обработка зависит от реализации контроллера. В частности, она заметным отличиям ли возвращать действий контроллера **HttpResponseMessage** или **IHttpActionResult**. Дополнительные сведения об этих типах результат, см. в разделе [результатов действий в веб-Api 2](../getting-started-with-aspnet-web-api/action-results.md).

## <a name="testing-actions-that-return-httpresponsemessage"></a>Тестирование действий, которые возвращают HttpResponseMessage

Ниже приведен пример контроллера возвращаемого значения которой действия **HttpResponseMessage**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

Обратите внимание, что контроллер использует внедрение зависимостей для внедрения `IProductRepository`. В результате контроллер эффективность тестирования, так как можно внедрить макет репозитория. Следующий модульный тест проверяет, что `Get` метод записи `Product` в текст ответа. Предполагается, что `repository` является макет `IProductRepository`.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

Очень важно задать **запроса** и **конфигурации** на контроллере. В противном случае тест завершится ошибкой с **ArgumentNullException** или **InvalidOperationException**.

## <a name="testing-link-generation"></a>Тестирование компоновки

`Post` Вызовы методов **UrlHelper.Link** для создания ссылок в ответе. Для этого требуется настройка немного сложнее в модульном тесте:

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

**UrlHelper** класс должен запроса URL-адрес и данные маршрута, поэтому тест должен задать значения для них. Другой вариант — макет или заглушки **UrlHelper**. При таком подходе вы замените значение по умолчанию [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) с версией макет или заглушки, которая возвращает значение.

Давайте перепишем тестирования с помощью [Moq](https://github.com/Moq) framework. Установить `Moq` пакет NuGet в проекте теста.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

В этой версии не требуется настроить все данные маршрута, так как макет **UrlHelper** возвращает строковую константу.


## <a name="testing-actions-that-return-ihttpactionresult"></a>Тестирование действий, которые возвращают IHttpActionResult

В веб-API 2, может возвращать действие контроллера **IHttpActionResult**, который является аналогом **ActionResult** в ASP.NET MVC. **IHttpActionResult** интерфейс определяет шаблон команды для создания ответов HTTP. Вместо непосредственно создания ответа, возвращает ли контроллер **IHttpActionResult**. Позже, конвейер вызывает **IHttpActionResult** для создания ответа. Такой подход упрощает написание модульных тестов, так как вы можете пропустить массу установки, которая необходима для **HttpResponseMessage**.

Ниже приведен пример контроллера возвращаемого значения которой действия **IHttpActionResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

В этом примере показаны некоторые распространенные шаблоны с помощью **IHttpActionResult**. Давайте посмотрим, как модульное тестирование их.

### <a name="action-returns-200-ok-with-a-response-body"></a>Действие возвращает 200 (ОК) с телом ответа

`Get` Вызовы методов `Ok(product)` при обнаружении продукта. В модульном тесте, убедитесь, что возвращаемый тип — **OkNegotiatedContentResult** и возвращаемый продукт имеет идентификатор справа.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

Обратите внимание на то, что модульный тест не выполняет результат действия. Можно предположить, что результат действия правильно создает HTTP-ответа. (Вот почему платформа веб-API имеет свой собственный модульные тесты!)

### <a name="action-returns-404-not-found"></a>Действие возвращает ошибку 404 (не найдено)

`Get` Вызовы методов `NotFound()` Если продукт не найден. В этом случае модульный тест только проверки, если возвращаемый тип — **NotFoundResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a>Действие возвращает 200 (ОК) без текста ответа

`Delete` Вызовы методов `Ok()` возвращать пустой ответ HTTP 200. Как в предыдущем примере, модульный тест проверяет тип возвращаемого значения, в этом случае **OkResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a>Действие возвращает код 201 (создано), заголовок расположения

`Post` Вызовы методов `CreatedAtRoute` чтобы возвратить ответ HTTP 201 с URI в заголовке Location. В модульном тесте убедитесь, что действие задает правильные значения маршрутизации.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a>Действие возвращает другой 2xx с телом ответа

`Put` Вызовы методов `Content` чтобы возвратить ответ HTTP 202 (принято) с телом ответа. Этот случай похож на возвращение 200 (ОК), но модульный тест следует также проверить код состояния.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Макетирование Entity Framework при модульном тестировании ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- [Написание тестов для службы веб-API ASP.NET](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (запись блога по Youssef Moussaoui).
- [Отладка ASP.NET Web API с помощью отладчика маршрута](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
