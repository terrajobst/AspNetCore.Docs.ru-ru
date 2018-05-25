---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: Развернуть с помощью $select, $ и $value в ASP.NET Web API 2 OData | Документы Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/11/2013
ms.topic: article
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: f229cdbd8850a787dd3585e0640e8e66f6109331
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a>Развернуть с помощью $select, $ и $value в ASP.NET Web API 2 OData
====================
по [Mike Wasson](https://github.com/MikeWasson)

Веб-API 2 добавляет поддержку для $разверните $select и возможности $value OData. Эти параметры позволяют контролировать представление, он получает от сервера клиенту.

- **$expand** вызывает связанные сущности быть указаны в ответе.
- **$select** выбирает подмножество свойств для включения в ответ.
- **$value** получает необработанное значение свойства.

## <a name="example-schema"></a>Пример схемы

В этой статье я буду использовать службы OData, определяющий три сущности: продукта, поставщика и категории. Каждый продукт состоит из одной категории и одного поставщика.

![](using-select-expand-and-value/_static/image1.png)

Ниже приведены классов C#, которые определяют модели сущностей.

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

Обратите внимание, что `Product` класс определяет свойства навигации для `Supplier` и `Category`. `Category` Класс определяет свойство навигации для продуктов в каждой категории.

Создание конечной точки OData для этой схемы, использовать формирование шаблонов Visual Studio 2013, как описано в [Создание конечной точки OData в веб-API ASP.NET](odata-v3/creating-an-odata-endpoint.md). Добавьте отдельные контроллеры для продукта, категорию и поставщика.

## <a name="enabling-expand-and-select"></a>Включение $разверните и $select

В Visual Studio 2013 формирование шаблонов OData веб-API будет создан контроллер, автоматически поддерживает $expand и $select. Справочник, имеются требования к поддержке $разверните и $select в контроллере.

Для коллекций, контроллер `Get` метод должен возвращать **IQueryable**.

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

Для одной сущности возвращают **SingleResult&lt;T&gt;**, где T — **IQueryable** , содержащая более одной сущности.

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

Кроме того, оформление вашей `Get` методы с **[Queryable]** атрибута, как показано в предыдущем фрагменты кода. Кроме того, вызвать **EnableQuerySupport** на **HttpConfiguration** объекта во время запуска. (Дополнительные сведения см. в разделе [включения параметров запроса OData](supporting-odata-query-options.md#enable).)

## <a name="using-expand"></a>Разверните узел с помощью $

При запросе OData сущность или коллекцию ответа по умолчанию не содержит связанных сущностей. Например здесь является ответом по умолчанию для набора сущностей категории:

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

Как видите, ответ не содержит какие-либо продукты хотя сущности «Категория» имеется ссылка навигации продуктов. Тем не менее, клиент может использовать $разверните, чтобы получить список продуктов для каждой категории. $Разверните параметр переходит в строке запроса:

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

Теперь сервер будет включать продукты для каждой категории встроенных категорий. Вот полезные данные ответа.

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

Обратите внимание, что каждая запись в массиве «значение» содержит список продуктов.

$Разверните параметр принимает разделенный запятыми список свойств навигации для развертывания. Следующий запрос расширяет категории и поставщика для продукта.

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

Ниже приведен текст ответа:

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

Вы можете развернуть более одного уровня свойства навигации. Следующий пример содержит все продукты для категории, а также поставщик для каждого продукта.

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

Ниже приведен текст ответа:

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

По умолчанию веб-API ограничивает максимальное глубину 2. Который предотвращает отправку сложных запросов, например клиент `$expand=Orders/OrderDetails/Product/Supplier/Region`, который может быть неэффективным для запроса и создания большие ответы. Чтобы переопределить значение по умолчанию, задайте **MaxExpansionDepth** свойство **[Queryable]** атрибута.

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

Дополнительные сведения о $разверните параметр, см. в разделе [параметр системного запроса ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) в официальной документации OData.

## <a name="using-select"></a>С помощью $select

Параметр $select определяет подмножество свойств для включения в тексте ответа. Например чтобы получить только имя и цены каждого продукта, используйте следующий запрос:

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

Ниже приведен текст ответа:

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

Можно объединять $select и $expand в одном запросе. Убедитесь, что для включения расширенного свойства в параметр $select. Например следующий запрос возвращает имя продукта и поставщика.

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

Ниже приведен текст ответа:

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

Можно также выбрать свойства в расширенное свойство. Следующий запрос расширяет продуктов и выбирает имя категории, а также название продукта.

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

Ниже приведен текст ответа:

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

Дополнительные сведения о параметре $select см. в разделе [параметр системного запроса ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) в официальной документации OData.

## <a name="getting-individual-properties-of-an-entity-value"></a>Получение отдельных свойств сущности ($value)

Существует два способа для клиента для получения отдельного свойства из сущности OData. Клиента можно получить значение в формате OData или получить необработанное значение свойства.

Следующий запрос возвращает свойство в формате OData.

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

Ниже приведен пример ответа в формате JSON:

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

Чтобы получить необработанное значение свойства, добавление $value URI:

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

Ниже представлен ответ. Обратите внимание, что тип содержимого «text/plain» не является JSON.

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

Для поддержки этих запросов в контроллер OData, добавьте метод с именем `GetProperty`, где `Property` имя свойства. Например, метод, чтобы получить значение свойства Name будет называться `GetName`. Метод должен возвращать значение этого свойства:

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
