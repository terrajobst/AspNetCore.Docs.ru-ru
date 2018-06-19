---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: Поддержка параметров запроса OData в ASP.NET Web API 2 | Документы Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/04/2013
ms.topic: article
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 004c029db6f01627f7cadff26aaf5554ce2b93a5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508023"
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a>Поддержка параметров запроса OData в ASP.NET Web API 2
====================
по [Mike Wasson](https://github.com/MikeWasson)

OData определяет параметры, которые можно использовать для изменения запрос OData. Клиент отправляет эти параметры в строке запроса URI запроса. Например чтобы отсортировать результаты, клиент использует параметр $orderby:

`http://localhost/Products?$orderby=Name`

Спецификация протокола OData вызывает эти параметры *параметры запроса*. Можно включить параметры запроса OData для любого контроллера веб-API в проекте & #8212; контроллер не требуется быть конечная точка OData. Это обеспечивает удобный способ добавления компонентов, таких как фильтрация и сортировка, любое приложение веб-API.

Прежде чем включать параметры запроса, ознакомьтесь с разделом [рекомендации по безопасности OData](odata-security-guidance.md).

- [Включение параметров запроса OData](#enable)
- [Примеры запросов](#examples)
- [Разбиение на страницы сервера](#server-paging)
- [Ограничения параметров запроса](#limiting_query_options)
- [Вызов непосредственно параметры запроса](#ODataQueryOptions)
- [Проверка запроса](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a>Включение параметров запроса OData

Веб-API поддерживает следующие параметры запроса OData:

| Параметр | Описание |
| --- | --- |
| $expand | При развертывании встроенного связанных сущностей. |
| $filter | Фильтрует результаты на основе логического условия. |
| $inlinecount | Указывает, что сервер для включения общее число соответствующих сущностей в ответе. (Полезно для постраничный просмотр на стороне сервера). |
| $orderby | Сортирует результаты. |
| $select | Выбирает, какие свойства будут включены в ответе. |
| $skip | Пропускает первые n результатов. |
| $top | Возвращает только первые n результатов. |

Чтобы использовать параметры запроса OData, необходимо включить их явно. Можно включить глобально для всего приложения или включить их для конкретных контроллеров или определенных действий.

Чтобы включить параметры запроса OData глобально, вызовите **EnableQuerySupport** на **HttpConfiguration** класс во время запуска:

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

**EnableQuerySupport** метод включает параметры запроса глобально для любого действия контроллера, который возвращает **IQueryable** типа. Если вы не хотите параметры запроса, которые включены для всего приложения, можно включить их для действий конкретного контроллера, добавив **[Queryable]** атрибут к методу действия.

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a>Примеры запросов

В этом разделе показаны типы запросов, которые, возможно с помощью параметров запроса OData. Подробности о параметрах запроса см. по адресу OData [www.odata.org](http://www.odata.org/).

Сведения о $разверните и $select, в разделе [развернуть с помощью $select, $ и $value в ASP.NET Web API OData](using-select-expand-and-value.md).

**Разбиение на страницы клиента**

Для больших наборов сущностей клиент может потребоваться ограничить количество результатов. Например клиент может показывать 10 записей одновременно со ссылками «Далее» для получения следующей страницы результатов. Чтобы сделать это, клиент использует параметры $top и $skip.

`http://localhost/Products?$top=10&$skip=20`

Параметр $top дает максимальное количество возвращаемых записей, а параметр $skip дает число записей для пропуска. Предыдущий пример извлекает записи 21-30.

**Фильтрация**

Параметр $filter позволяет клиенту фильтрации результатов путем применения логического выражения. Критерии фильтра весьма значительны; они включают логические и арифметические операторы, строковые функции и функции даты.

| Возвратить все продукты с категорией равно «Игрушки». | `http://localhost/Products?$filter=Category`EQ 'Toys» |
| --- | --- |
| Возвратить все продукты с ценой меньше 10. | `http://localhost/Products?$filter=Price`lt 10 |
| Логические операторы: возврата всех продуктов, цена которых > = 5 и цена < = 15. | `http://localhost/Products?$filter=Price`GE 5 и цена le 15 |
| Строковые функции: возвратить все продукты с «zz» в имени. | `http://localhost/Products?$filter=substringof('zz',Name)` |
| Функции даты: возвратить все продукты с ReleaseDate за 2005. | `http://localhost/Products?$filter=year(ReleaseDate)`gt 2005 |

**Сортировка**

Чтобы отсортировать результаты, используйте фильтр $orderby.

| Сортировка по цене. | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| Сортировка по цене в убывающем порядке (от большего к меньшему). | `http://localhost/Products?$orderby=Price desc` |
| Сортировка по категориям, а затем отсортировать по цене в нисходящем порядке по категориям. | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a>Разбиение на страницы сервера

Если база данных содержит миллионы записей, вы не хотите отправлять их все в одной полезной нагрузке. Чтобы избежать этого, сервер можно ограничить число записей, он отправляет в одном ответе. Чтобы включить разбиение на страницы сервера, задайте **PageSize** свойство в **Queryable** атрибута. Значение — максимальное количество возвращаемых элементов.

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

Если ваш контроллер возвращает формат OData, текст ответа будет содержать ссылку на следующую страницу данных:

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

Клиент может использовать эту ссылку для получения следующей страницы. Чтобы получить общее число записей в результирующем наборе, клиент может задать значение параметра запроса $inlinecount «allpages».

`http://localhost/Products?$inlinecount=allpages`

Значение «allpages» указывает, что сервер для включения общее число объектов в ответе:

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> Ссылки следующей страницы и встроенное количество, требуют формате OData. Причина в том, что OData определяет специальные поля в тексте ответа для хранения ссылки и count.


Для форматов не OData можно по-прежнему поддерживает счетчик ссылок и встроенные следующей страницы, заключив результаты запроса в **PageResult&lt;T&gt;**  объекта. Однако он требует немного больше кода. Пример:

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

Ниже приведен пример ответа JSON:

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a>Ограничения параметров запроса

Параметры запроса предоставьте клиенту много контроль над запроса, который выполняется на сервере. В некоторых случаях может потребоваться ограничить доступные параметры для повышения производительности и безопасности. **[Queryable]** атрибута есть некоторые встроенные свойства для этого. Ниже приводятся некоторые примеры.

Разрешить только $skip и $top, для поддержки разбиения на страницы и ничего более:

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

Разрешается только в определенные свойства запретить сортировку по свойствам, которые не проиндексированы в базе данных:

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

Разрешить использование логической функции «eq», но не логические функции:

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

Не разрешать все арифметические операторы:

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

Вы можете ограничить параметры глобально, создав **QueryableAttribute** экземпляра и передается командлету **EnableQuerySupport** функции:

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a>Вызов непосредственно параметры запроса

Вместо использования **[Queryable]** атрибут, можно вызвать параметры запроса непосредственно в контроллере. Чтобы сделать это, добавьте **ODataQueryOptions** параметр метода контроллера. В этом случае не требуется **[Queryable]** атрибута.

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

Заполняет веб-API **ODataQueryOptions** строка запроса из URI. Чтобы применить запрос, передайте **IQueryable** для **ApplyTo** метод. Метод возвращает другой **IQueryable**.

Для более сложных сценариев, если у вас **IQueryable** поставщик запросов, можно изучить **ODataQueryOptions** и преобразовать параметры запроса формы в другую. (К примеру, см. RaghuRam Nadiminti блога [запросов преобразования OData в HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), также включает [пример](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)

<a id="query-validation"></a>
## <a name="query-validation"></a>Проверка запроса

**[Queryable]** атрибут проверяет запрос перед его выполнением. Действие проверки выполняется в **QueryableAttribute.ValidateQuery** метод. Можно также настроить процесс проверки.

См. также [рекомендации по безопасности OData](odata-security-guidance.md).

Во-первых, переопределение одного модуля проверки классы, определенные в **Web.Http.OData.Query.Validators** пространства имен. Например приведенный ниже класс проверяющего элемента управления отключает параметр «desc» для параметра $orderby.

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

Подкласс **[Queryable]** атрибута для переопределения **ValidateQuery** метод.

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

Задайте пользовательский атрибут либо глобально или на контроллер:

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

Если вы используете **ODataQueryOptions** напрямую, установить проверяющий элемент управления с параметрами:

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
