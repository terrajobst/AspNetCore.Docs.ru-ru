---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
title: Соглашение о маршрутизации в ASP.NET Web API 2 Odata | Документация Майкрософт
author: MikeWasson
description: В этой статье описывается соглашений о маршрутизации, используемых веб-API для конечных точек OData.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/31/2013
ms.topic: article
ms.assetid: adbc175a-14eb-4ab2-a441-d056ffa8266f
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
msc.type: authoredcontent
ms.openlocfilehash: 63a0e4f4f61580ea9da3bd491e7a45f20cd7aaae
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370543"
---
<a name="routing-conventions-in-aspnet-web-api-2-odata"></a>Соглашение о маршрутизации в ASP.NET Web API 2 Odata
====================
по [Майк Уоссон](https://github.com/MikeWasson)

> В этой статье описывается соглашений о маршрутизации, используемых веб-API для конечных точек OData.


Когда веб-API получает запрос OData, он сопоставляет запрос имени контроллера и действия. Сопоставление основано на методе HTTP и URI. Например `GET /odata/Products(1)` сопоставляется `ProductsController.GetProduct`.

В первой части этой статьи я опишу встроенные соглашения маршрутизации OData. Эти правила предназначены специально для конечных точек OData и Платежная система маршрутизации по умолчанию веб-API. (Замена происходит при вызове **MapODataRoute**.)

Во второй части будет продемонстрирован способ добавления пользовательских соглашений о маршрутизации. В настоящее время встроенные соглашения не охватывают весь диапазон из OData коды URI, но можно расширить их для обработки дополнительных вариантов.

- [Встроенные соглашений о маршрутизации](#conventions)
- [Пользовательские соглашения о маршрутизации](#custom)

<a id="conventions"></a>
## <a name="built-in-routing-conventions"></a>Встроенные соглашений о маршрутизации

Прежде чем описывать соглашение о маршрутизации OData в веб-API, полезно понять идентификаторы URI OData. [OData URI](http://www.odata.org/documentation/odata-v3-documentation/url-conventions/) состоит из:

- Корень службы
- Путь к ресурсу
- Параметры запроса

![](odata-routing-conventions/_static/image1.png)

Для маршрутизации, важной частью является путь к ресурсу. Путь к ресурсу разделяются на сегменты. Например `/Products(1)/Supplier` состоит из трех сегментов:

- `Products` ссылается на набор сущностей с именем «Продукты».
- `1` представляет собой ключ сущности, выбрав одну сущность из набора.
- `Supplier` — Это свойство навигации, который выбирает связанной сущности.

Поэтому этот путь выбирает поставщика продукта 1.

> [!NOTE]
> Сегменты пути OData не всегда соответствуют сегментов URI-адреса. Например «1» считается сегмента пути.


**Имена контроллеров.** Имя контроллера всегда является производным от в корне пути к ресурсу набора сущностей. Например, если путь к ресурсу — `/Products(1)/Supplier`, веб-API ищет контроллер с именем `ProductsController`.

**Имена действий.** Имена действий являются производными от сегменты пути, а также модели EDM (модель EDM), как показано в следующих таблицах. В некоторых случаях у вас есть два варианта для имени действия. Например, «Get» или &quot;GetProducts&quot;.

**Запрос сущностей**

| Запрос | Пример URI | Имя действия | Пример действия |
| --- | --- | --- | --- |
| ПОЛУЧИТЬ /entityset | / Products | GetEntitySet или Get | GetProducts |
| ПОЛУЧИТЬ /entityset(key) | /Products(1) | GetEntityType или Get | GetProduct |
| ПОЛУЧИТЬ /entityset (ключ) / приведения | / /Models.Book products (1) | GetEntityType или Get | GetBook |

Дополнительные сведения см. в разделе [Создание конечной точки OData только для чтения](odata-v3/creating-an-odata-endpoint.md).

**Создание, обновление и удаление сущностей**

| Запрос | Пример URI | Имя действия | Пример действия |
| --- | --- | --- | --- |
| Учет /entityset | / Products | PostEntityType» или «Post | PostProduct |
| ПОМЕСТИТЕ /entityset(key) | /Products(1) | PutEntityType или Put | PutProduct |
| ПОМЕСТИТЕ /entityset (ключ) / приведения | / /Models.Book products (1) | PutEntityType или Put | PutBook |
| ИСПРАВЛЕНИЕ /entityset(key) | /Products(1) | Исправление или PatchEntityType | PatchProduct |
| ИСПРАВЛЕНИЕ /entityset (ключ) / приведения | / /Models.Book products (1) | Исправление или PatchEntityType | PatchBook |
| УДАЛИТЬ /entityset(key) | /Products(1) | DeleteEntityType или Delete | DeleteProduct |
| Удаление /entityset (ключ) / приведения | / /Models.Book products (1) | DeleteEntityType или Delete | DeleteBook |

**Запрос свойства навигации**

| Запрос | Пример URI | Имя действия | Пример действия |
| --- | --- | --- | --- |
| GET /entityset (ключ) и навигации | / Products (1) / поставщика | GetNavigationFromEntityType или GetNavigation | GetSupplierFromProduct |
| ПОЛУЧИТЬ /entityset (ключ) / cast/навигации | / /Models.Book/Author products (1) | GetNavigationFromEntityType или GetNavigation | GetAuthorFromBook |

Дополнительные сведения см. в разделе [работа с отношениями сущностей](odata-v3/working-with-entity-relations.md).

**Создание и удаление ссылки**

| Запрос | Пример URI | Имя действия |
| --- | --- | --- |
| /Entityset POST (ключ) / $links/навигации | / Ссылки (1) / $ products/поставщика | Команду CreateLink |
| PUT /entityset (ключ) / $links/навигации | / Ссылки (1) / $ products/поставщика | Команду CreateLink |
| DELETE /entityset (ключ) / $links/навигации | / Ссылки (1) / $ products/поставщика | DeleteLink |
| УДАЛИТЬ /entityset(key)/$links/navigation(relatedKey) | /Products/(1)/$Links/Suppliers(1) | DeleteLink |

Дополнительные сведения см. в разделе [работа с отношениями сущностей](odata-v3/working-with-entity-relations.md).

**Свойства**

*Требуется веб-API 2*

| Запрос | Пример URI | Имя действия | Пример действия |
| --- | --- | --- | --- |
| GET /entityset (ключ) или свойство | / Products (1) / имя | GetPropertyFromEntityType или GetProperty | GetNameFromProduct |
| ПОЛУЧИТЬ /entityset (ключ) / приведения или свойства | / /Models.Book/Author products (1) | GetPropertyFromEntityType или GetProperty | GetTitleFromBook |

**Действия**

| Запрос | Пример URI | Имя действия | Пример действия |
| --- | --- | --- | --- |
| /Entityset POST (ключ) / действие | / Products (1) / скорость | ActionNameOnEntityType или имя действия | RateOnProduct |
| /Entityset (ключ) / cast/действий, выполняемых после | / /Models.Book/CheckOut products (1) | ActionNameOnEntityType или имя действия | CheckOutOnBook |

Дополнительные сведения см. в разделе [действия OData](odata-v3/odata-actions.md).

**Сигнатуры методов**

Ниже приведены некоторые правила для сигнатур методов.

- Если путь содержит ключ, действие должен иметь параметр с именем *ключ*.
- Если путь содержит ключ в свойство навигации, действие должен иметь параметр с именем *relatedKey*.
- Decorate *ключ* и *relatedKey* параметров с **[FromODataUri]** параметра.
- POST и PUT запросы принимают параметр типа сущности.
- Запросы PATCH принимать параметр типа **разностных&lt;T&gt;**, где *T* является типом сущности.

Для справки ниже приведен пример, в котором показан метод подписи для каждого встроенные соглашения маршрутизации OData.

[!code-csharp[Main](odata-routing-conventions/samples/sample1.cs)]

<a id="custom"></a>
## <a name="custom-routing-conventions"></a>Пользовательские соглашения о маршрутизации

В настоящее время встроенные соглашения не охватывают все возможные URI OData. Можно добавить новые соглашения об путем реализации **IODataRoutingConvention** интерфейс. Этот интерфейс содержит два метода:

[!code-csharp[Main](odata-routing-conventions/samples/sample2.cs)]

- **SelectController** возвращает имя контроллера.
- **SelectAction** возвращает имя действия.

Оба метода Если соглашение не применяется к запросу. метод должен возвращать значение null.

**ODataPath** параметр представляет проанализированное пути к ресурсу OData. Он содержит список **[ODataPathSegment](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatapathsegment.aspx)** экземпляров, по одной для каждого сегмента пути к ресурсу. **ODataPathSegment** является абстрактным классом; каждый тип сегмента представляется классом, который является производным от **ODataPathSegment**.

**ODataPath.TemplatePath** свойство является строка, представляющая результат объединения все сегменты пути. Например, если URL-адрес является `/Products(1)/Supplier`, является шаблон пути &quot;~/entityset/key/navigation&quot;. Обратите внимание на то, что сегменты не соответствуют непосредственно сегментов URI-адреса. Например, ключ сущности (1) представляется как свой собственный **ODataPathSegment**.

Как правило, реализация **IODataRoutingConvention** делает следующее:

1. Сравните шаблон пути, чтобы увидеть, если это соглашение применяется к текущему запросу. Если он неприменим, возвращает значение null.
2. Если применяется соглашение, использование свойств объекта **ODataPathSegment** экземпляров для получения имени контроллера и действия.
3. Для действий добавьте все значения в словарь маршрута, необходимо привязать к параметрам действия (обычно ключи сущностей).

Рассмотрим конкретный пример. Встроенные соглашений о маршрутизации не поддерживают индексирование в коллекции навигации. Другими словами имеют каких-либо для URI следующего вида:

[!code-javascript[Main](odata-routing-conventions/samples/sample3.js)]

Вот пользовательский соглашение о маршрутизации для обработки этого типа запросов.

[!code-csharp[Main](odata-routing-conventions/samples/sample4.cs)]

Примечания.

1. Он является производным от **EntitySetRoutingConvention**, так как **SelectController** метода этого класса подходит для этой новой соглашение о маршрутизации. Это означает, что нет необходимости в повторной реализации **SelectController**.
2. Соглашение применяется только для запросов GET, а только в том случае, если шаблон пути является &quot;~/entityset/key/navigation/key&quot;.
3. Имя действия &quot;получить {EntityType}&quot;, где *{EntityType}* — это тип навигации коллекции. Например &quot;GetSupplier&quot;. Можно использовать любое соглашение об именовании, что вам нравится &#8212; просто убедитесь, что соответствует действий контроллера.
4. Действие принимает два параметра с именем *ключ* и *relatedKey*. (Список некоторые имена предопределенных параметров, см. в разделе [ODataRouteConstants](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatarouteconstants.aspx).)

Следующим шагом является добавление новое соглашение в список соглашений о маршрутизации. Это происходит во время настройки, как показано в следующем коде:

[!code-csharp[Main](odata-routing-conventions/samples/sample5.cs)]

Ниже приведены некоторые другие образец соглашений о маршрутизации, которые помогут изучить.

- [CompositeKeyRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataCompositeKeySample/ODataCompositeKeySample/Extensions/CompositeKeyRoutingConvention.cs)
- [CustomNavigationRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataServiceSample/ODataService/Extensions/CustomNavigationRoutingConvention.cs)
- [NonBindableActionRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataActionsSample/ODataActionsSample/NonBindableActionRoutingConvention.cs)
- [ODataVersionRouteConstraint](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataVersioningSample/ODataVersioningSample/Extensions/ODataVersionRouteConstraint.cs)

И конечно само веб-API является открытым кодом, чтобы можно было видеть [исходный код](http://aspnetwebstack.codeplex.com/) для встроенных соглашений о маршрутизации. Они определяются в **System.Web.Http.OData.Routing.Conventions** пространства имен.
