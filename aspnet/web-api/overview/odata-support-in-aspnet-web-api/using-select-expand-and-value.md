---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: Использование $select, $expand и $value в ASP.NET Web API 2 OData | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/11/2013
ms.topic: article
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: f7e70bd76668f2af9215d57ee1cc9e7d41948c67
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384768"
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a>Использование $select, $expand и $value в ASP.NET Web API 2 OData
====================
по [Майк Уоссон](https://github.com/MikeWasson)

Веб-API 2 добавлена поддержка $expand, $select и $value в OData. Эти параметры позволяют клиенту управлять представление, которое возвращается с сервера.

- **$expand** вызывает связанные сущности быть указаны в ответе.
- **$select** выбирает подмножество свойств для включения в ответ.
- **$value** получает необработанное значение свойства.

## <a name="example-schema"></a>Пример схемы

В этой статье я буду использовать службы OData, определяющий трех сущностей: продукта, поставщика и категории. Каждый продукт состоит из одной категории и одного поставщика.

![](using-select-expand-and-value/_static/image1.png)

Ниже приведены классы C#, которые определяют модели сущностей.

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

Обратите внимание, что `Product` класс определяет свойства навигации для `Supplier` и `Category`. `Category` Класс определяет свойство навигации для продуктов в каждой категории.

Чтобы создать конечную точку OData для этой схемы, использовать формирование шаблонов Visual Studio 2013, как описано в разделе [Создание конечной точки OData в веб-API ASP.NET](odata-v3/creating-an-odata-endpoint.md). Добавьте отдельные контроллеры для продукта, категорию и поставщика.

## <a name="enabling-expand-and-select"></a>Включение $разверните и $select

В Visual Studio 2013 формирование шаблонов веб-API OData создает контроллер, автоматически поддерживает $expand и $select. Для справки ниже приведены, разверните узел требования для поддержки $ и $select в контроллере.

Для коллекций, контроллер `Get` метод должен возвращать **IQueryable**.

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

Для одной сущности, возвращаемого **— SingleResult&lt;T&gt;**, где T — **IQueryable** , содержащая более одной сущности.

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

Кроме того, снабдить вашего `Get` методы с **[Queryable]** атрибута, как показано в предыдущих фрагментах кода. Кроме того, вызвать **EnableQuerySupport** на **HttpConfiguration** объекта во время запуска. (Дополнительные сведения см. в разделе [включения параметров запроса OData](supporting-odata-query-options.md#enable).)

## <a name="using-expand"></a>Разверните узел с помощью $

При запросе OData сущность или коллекцию, ответ по умолчанию не включает связанных сущностей. Например вот ответ по умолчанию для набора сущностей категории:

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

Как вы видите, ответ не включает все продукты, несмотря на то, что сущность «Категория» содержит ссылку перехода продуктов. Тем не менее, клиент может использовать $разверните, чтобы получить список продуктов для каждой категории. Параметр expand $ переходит в строке запроса:

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

Теперь сервер будет включать продукты для каждой категории, встроенные с категориями. Ниже приведен полезных данных ответа.

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

Обратите внимание на то, что каждая запись в массиве «значение» со списком продуктов.

$Expand параметр принимает разделенный запятыми список свойств навигации для развертывания. Следующий запрос расширяет категорию и поставщика для продукта.

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

Ниже приведен текст ответа:

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

Вы можете развернуть более одного уровня свойства навигации. Следующий пример включает все продукты для категории, а также поставщика для каждого продукта.

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

Ниже приведен текст ответа:

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

По умолчанию веб-API ограничивает максимальное расширения глубина до 2. Не позволяет клиенту отправлять сложных запросов, например `$expand=Orders/OrderDetails/Product/Supplier/Region`, который может быть неэффективным для запроса и создание больших ответов. Чтобы переопределить значение по умолчанию, задайте **MaxExpansionDepth** свойство **[Queryable]** атрибута.

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

Дополнительные сведения о $разверните параметр, см. в разделе [системный параметр запроса ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) в официальной документации OData.

## <a name="using-select"></a>Использование $select

Параметр $select определяет подмножество свойств для включения в тексте ответа. Например чтобы получить только название и цену каждого продукта, используйте следующий запрос:

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

Ниже приведен текст ответа:

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

Вы можете объединить $select и $expand в одном запросе. Не забудьте включить расширенное свойство в параметра $select. Например следующий запрос возвращает имя продукта и поставщика.

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

Ниже приведен текст ответа:

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

Можно также выбрать свойства, используемые в расширенное свойство. Следующий запрос расширяет продуктов и выбирает имя категории, а также название продукта.

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

Ниже приведен текст ответа:

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

Дополнительные сведения о параметра $select, см. в разделе [параметр системного запроса выбора ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) в официальной документации OData.

## <a name="getting-individual-properties-of-an-entity-value"></a>Получение отдельных свойств сущности ($value)

Существует два способа для клиента OData для получения отдельного свойства из сущности. Клиента можно получить значение в формате OData или получить необработанное значение свойства.

Следующий запрос возвращает свойство в формате OData.

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

Ниже приведен пример ответа в формате JSON:

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

Чтобы получить необработанное значение свойства, добавьте $value к URI.

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

Вот ответ. Обратите внимание на то, что тип содержимого «text/plain», не является JSON.

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

Поддерживает эти запросы в контроллере OData, добавьте метод с именем `GetProperty`, где `Property` — это имя свойства. Например, будет иметь имя метода для получения имени свойства `GetName`. Метод должен вернуть значение этого свойства:

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
