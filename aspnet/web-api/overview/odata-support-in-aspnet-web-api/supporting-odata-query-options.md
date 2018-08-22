---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: Поддержка параметров запроса OData в веб-API 2 ASP.NET | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: riande
ms.date: 02/04/2013
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 8745183125c9dd1dcc7cb0e146367a893bdb0170
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835449"
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a>Поддержка параметров запроса OData в веб-API 2 ASP.NET
====================
по [Майк Уоссон](https://github.com/MikeWasson)

OData определяет параметры, которые можно использовать для изменения запроса OData. Клиент отправляет эти параметры в строке запроса URI запроса. Например чтобы отсортировать результаты, клиент использует параметр $orderby:

`http://localhost/Products?$orderby=Name`

Спецификации протокола OData вызывает эти параметры *параметры запроса*. Вы можете включить параметры запроса OData для любого контроллера веб-API в вашем проекте &#8212; контроллера не должны быть конечной точкой OData. Это позволяет легко добавить функции, такие как фильтрация и сортировка в любое приложение веб-API.

Прежде чем включать параметры запроса, см. в статье в разделе [рекомендации по безопасности OData](odata-security-guidance.md).

- [Включение параметров запроса OData](#enable)
- [Примеры запросов](#examples)
- [Server-Driven Paging](#server-paging)
- [Ограничение параметры запроса](#limiting_query_options)
- [Вызов непосредственно параметры запроса](#ODataQueryOptions)
- [Проверка запроса](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a>Включение параметров запроса OData

Веб-API поддерживает следующие параметры запроса OData:

| Параметр | Описание: |
| --- | --- |
| $expand | При развертывании встроенного связанных сущностей. |
| $filter | Фильтрует результаты, на основе логического условия. |
| $inlinecount | Указывает, что сервер для включения общее число соответствующих сущностей в ответе. (Полезно для серверное разбиение по страницам). |
| $orderby | Сортирует результаты. |
| $select | Выбирает, какие свойства будут включены в ответе. |
| $skip | Пропускает первые n результатов. |
| $top | Возвращает только первые n результатов. |

Чтобы использовать параметры запроса OData, необходимо включить их явным образом. Можно включить глобально для всего приложения или включить их для определенных контроллеров и определенные действия.

Чтобы включить параметры запроса OData глобально, вызовите **EnableQuerySupport** на **HttpConfiguration** класса во время запуска:

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

**EnableQuerySupport** метод включает параметры запроса для любого действия контроллера, который возвращает **IQueryable** типа. Если вы не хотите параметры запросов, которые включены для всего приложения, вы можете включить их для действий определенного контроллера, добавив **[Queryable]** атрибут к методу действия.

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a>Примеры запросов

В этом разделе показаны типы запросов, которые возможно использует параметры запроса OData. Конкретные сведения о параметрах запроса см. в документации по OData на [www.odata.org](http://www.odata.org/).

Сведения о $ разверните узел и $select, см. в разделе [использование $select, $expand и $value в OData веб-API ASP.NET](using-select-expand-and-value.md).

**Разбиение на страницы клиента**

Для больших наборов сущностей клиент может потребоваться ограничить количество результатов. Например клиент может показывать 10 записей одновременно, со ссылками «Далее» для получения следующей страницы результатов. Чтобы сделать это, клиент использует параметры $top и $skip.

`http://localhost/Products?$top=10&$skip=20`

Параметр $top дает максимальное количество возвращаемых записей, а параметр $skip дает число пропускаемых записей. Предыдущий пример извлекает записи 21-30.

**Фильтрация**

Параметр $filter позволяет клиенту фильтрации результатов путем применения логического выражения. Выражения фильтра весьма существенны; они включают логические операторы и арифметические операторы, строковые функции и функции даты.

| Возвратить все продукты с категорией, равным «Toys». | `http://localhost/Products?$filter=Category` EQ «Toys» |
| --- | --- |
| Возвратить все продукты с ценой меньше 10. | `http://localhost/Products?$filter=Price` lt 10 |
| Логические операторы: возвратить все продукты где цена > = 5 и цена < = 15. | `http://localhost/Products?$filter=Price` GE 5 и цена le 15 |
| Строковые функции: возвратить все продукты с «zz» в имени. | `http://localhost/Products?$filter=substringof('zz',Name)` |
| Функции даты: возвратить все продукты с ReleaseDate за 2005. | `http://localhost/Products?$filter=year(ReleaseDate)` gt 2005 |

**Сортировка**

Чтобы отсортировать результаты, используйте фильтр $orderby.

| Сортировка по цене. | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| Сортировка по цене в убывающем порядке (от большего к меньшему). | `http://localhost/Products?$orderby=Price desc` |
| Сортировать по категориям, а затем отсортировать по цене в убывающем порядке в пределах их категорий. | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a>Server-Driven Paging

Если база данных содержит миллионы записей, вы не хотите отправлять их все в одной полезной нагрузке. Чтобы избежать этого, сервер может ограничить количество записей, отправляемых в одном ответе. Чтобы включить разбиение на страницы, задайте **PageSize** свойство в **Queryable** атрибута. Значение — это максимальное число возвращаемых записей.

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

Если ваш контроллер возвращает формат OData, текст ответа будет содержать ссылку на следующую страницу данных:

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

Клиент может использовать эту ссылку для получения следующей страницы. Чтобы узнать, общее число записей в результирующем наборе, клиент может задать параметра запроса $inlinecount со значением «allpages».

`http://localhost/Products?$inlinecount=allpages`

Значение «allpages» предписывает серверу включал полный счетчик в ответе:

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> Ссылки следующей страницы и встроенное количество требуется формат OData. Причина в том, что OData определяет специальные поля в тексте ответа для размещения ссылки и count.


Для форматов не OData, можно по-прежнему поддерживает счетчик ссылок и в строке следующей страницы, заключив результаты запроса в **PageResult&lt;T&gt;**  объекта. Однако он требует немного больше кода. Пример:

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

Ниже приведен пример ответа JSON:

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a>Ограничение параметры запроса

Параметры запроса предоставьте клиенту массу контроль над запроса, который выполняется на сервере. В некоторых случаях может потребоваться ограничить доступные параметры для повышения производительности и безопасности. **[Queryable]** атрибут некоторые встроенные свойства для этого. Ниже приводятся некоторые примеры.

Разрешить только $skip, $top, для поддержки разбиения по страницам и ничего более:

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

Разрешить упорядочение только с определенными свойствами запретить сортировку по свойствам, которые не индексируются в базе данных:

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

Разрешить использование логической функции «eq», но не другие логические функции:

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

Не разрешать все арифметические операторы:

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

Вы можете ограничить параметры глобально, создав **QueryableAttribute** экземпляра и передается командлету **EnableQuerySupport** функции:

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a>Вызов непосредственно параметры запроса

Вместо использования **[Queryable]** атрибут, параметры запроса можно вызвать непосредственно в контроллере. Чтобы сделать это, добавьте **ODataQueryOptions** параметр для метода контроллера. В этом случае не требуется **[Queryable]** атрибута.

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

Заполняет веб-API **ODataQueryOptions** строка запроса из URI. Чтобы применить запрос, передайте **IQueryable** для **ApplyTo** метод. Этот метод возвращает другой **IQueryable**.

Для более сложных сценариев, если у вас нет **IQueryable** поставщик запросов, можно изучить **ODataQueryOptions** и преобразование параметров запроса в другой форме. (Например, см. в разделе блога RaghuRam Nadiminti [запросов преобразования OData в HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), также включает [пример](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)

<a id="query-validation"></a>
## <a name="query-validation"></a>Проверка запроса

**[Queryable]** атрибут проверяет запрос перед его выполнением. Действие проверки выполняется в **QueryableAttribute.ValidateQuery** метод. Можно также настроить процесс проверки.

Также см. в разделе [рекомендации по безопасности OData](odata-security-guidance.md).

Во-первых, переопределение одного модуля проверки классы, то есть определенные в **Web.Http.OData.Query.Validators** пространства имен. Например следующий класс проверяющего элемента управления отключает параметр «desc» для параметра $orderby.

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

Подкласс **[Queryable]** атрибута для переопределения **ValidateQuery** метод.

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

Задайте пользовательский атрибут либо глобально или на контроллер:

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

Если вы используете **ODataQueryOptions** напрямую, установить проверяющий элемент управления о параметрах:

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
