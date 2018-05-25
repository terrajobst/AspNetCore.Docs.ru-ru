---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
title: Соглашение о маршрутизации в ASP.NET Web API 2 Odata | Документы Microsoft
author: MikeWasson
description: В этой статье описаны соглашения маршрутизации, которые веб-API использует конечные точки OData.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/31/2013
ms.topic: article
ms.assetid: adbc175a-14eb-4ab2-a441-d056ffa8266f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
msc.type: authoredcontent
ms.openlocfilehash: 0ab99dd443040b90ffefd2f5b9261a63b91e9463
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="routing-conventions-in-aspnet-web-api-2-odata"></a>Соглашение о маршрутизации в ASP.NET Web API 2 Odata
====================
по [Mike Wasson](https://github.com/MikeWasson)

> В этой статье описаны соглашения маршрутизации, которые веб-API использует конечные точки OData.


Когда запрос OData веб-API, он сопоставляет запрос имени контроллера и действия. Сопоставление основан на HTTP-метод и URI. Например `GET /odata/Products(1)` сопоставляется `ProductsController.GetProduct`.

В первой части в этой статье я опишу встроенных соглашение о маршрутизации OData. Эти правила, предназначенные специально для конечных точек OData, и они заменяют системы маршрутизации по умолчанию веб-API. (Замена возникает при вызове **MapODataRoute**.)

В части 2 показано добавление пользовательские соглашения о маршрутизации. В настоящее время встроенных соглашения не охватывают весь диапазон из OData URL-адреса, но можно расширить их для обработки дополнительных вариантов.

- [Встроенные соглашений о маршрутизации](#conventions)
- [Пользовательские соглашения о маршрутизации](#custom)

<a id="conventions"></a>
## <a name="built-in-routing-conventions"></a>Встроенные соглашений о маршрутизации

Прежде чем я опишу соглашение о маршрутизации OData в веб-API, необходимо понять, идентификаторы URI OData. [OData URI](http://www.odata.org/documentation/odata-v3-documentation/url-conventions/) состоит из:

- Корень службы
- Путь к ресурсу
- Параметры запроса

![](odata-routing-conventions/_static/image1.png)

Для маршрутизации, важно пути к ресурсу. Путь к ресурсу, разделяются на сегменты. Например `/Products(1)/Supplier` состоит из трех сегментов:

- `Products`ссылается на набор сущностей с именем «Продукты».
- `1`ключ сущности, при выборе одной сущности из набора.
- `Supplier`является свойством навигации, которое выбирает связанной сущности.

Поэтому этот путь выбирает out к поставщику продукта 1.

> [!NOTE]
> Сегменты пути OData не всегда соответствуют сегментов URI-адреса. Например «1» считается сегмента пути.


**Имена контроллеров.** Имя контроллера всегда является производным от в корневом пути к ресурсу набора сущностей. Например, если путь к ресурсу `/Products(1)/Supplier`, веб-API ищет контроллер с именем `ProductsController`.

**Имена действий.** Имена действий являются производными от сегменты пути, а также модели EDM (модель EDM), перечисленные в следующих таблицах. В некоторых случаях у вас есть два варианта для имени действия. Например, «Get» или &quot;GetProducts&quot;.

**Выполнения запросов к сущностям**

| Запрос | Пример URI | Имя действия | Пример действия |
| --- | --- | --- | --- |
| ПОЛУЧИТЬ /entityset | / Продуктов | GetEntitySet или Get | GetProducts |
| ПОЛУЧИТЬ /entityset(key) | /Products(1) | GetEntityType или Get | GetProduct |
| ПОЛУЧИТЬ /entityset (ключ) и приведение | / /Models.Book продуктов (1) | GetEntityType или Get | GetBook |

Дополнительные сведения см. в разделе [создать конечную точку OData только для чтения](odata-v3/creating-an-odata-endpoint.md).

**Создание, обновление и удаление сущностей**

| Запрос | Пример URI | Имя действия | Пример действия |
| --- | --- | --- | --- |
| Учет /entityset | / Продуктов | PostEntityType или Post | PostProduct |
| ПОМЕСТИТЕ /entityset(key) | /Products(1) | PutEntityType или Put | PutProduct |
| ПОМЕСТИТЕ /entityset (ключ) и приведение | / /Models.Book продуктов (1) | PutEntityType или Put | PutBook |
| ИСПРАВЛЕНИЕ /entityset(key) | /Products(1) | Исправление или PatchEntityType | PatchProduct |
| Исправление для /entityset (ключ) и приведение | / /Models.Book продуктов (1) | Исправление или PatchEntityType | PatchBook |
| УДАЛИТЬ /entityset(key) | /Products(1) | DeleteEntityType или Delete | DeleteProduct |
| УДАЛИТЬ /entityset (ключ) и приведение | / /Models.Book продуктов (1) | DeleteEntityType или Delete | DeleteBook |

**Запрос свойства навигации**

| Запрос | Пример URI | Имя действия | Пример действия |
| --- | --- | --- | --- |
| GET /entityset (ключ) и навигации | И продукты (1) или поставщика | GetNavigationFromEntityType или GetNavigation | GetSupplierFromProduct |
| ПОЛУЧИТЬ /entityset (ключ) и приведения и навигация | /Products(1)/Models.Book/Author | GetNavigationFromEntityType или GetNavigation | GetAuthorFromBook |

Дополнительные сведения см. в разделе [работа с отношениями сущностей](odata-v3/working-with-entity-relations.md).

**Создание и удаление ссылки**

| Запрос | Пример URI | Имя действия |
| --- | --- | --- |
| /Entityset POST (ключ) / $links/навигации | / Ссылки (1) / $ продукты/поставщика | Команду CreateLink |
| PUT /entityset (ключ) / $links/навигации | / Ссылки (1) / $ продукты/поставщика | Команду CreateLink |
| Удаление /entityset (ключ) / $links/навигации | / Ссылки (1) / $ продукты/поставщика | DeleteLink |
| УДАЛИТЬ /entityset(key)/$links/navigation(relatedKey) | /Products/(1)/$Links/Suppliers(1) | DeleteLink |

Дополнительные сведения см. в разделе [работа с отношениями сущностей](odata-v3/working-with-entity-relations.md).

**Свойства**

*Требуется веб-API 2*

| Запрос | Пример URI | Имя действия | Пример действия |
| --- | --- | --- | --- |
| GET /entityset (ключ) и свойства | И продукты (1) или имя | GetPropertyFromEntityType или GetProperty | GetNameFromProduct |
| ПОЛУЧИТЬ /entityset (ключ) и приведения или свойства | /Products(1)/Models.Book/Author | GetPropertyFromEntityType или GetProperty | GetTitleFromBook |

**Действия**

| Запрос | Пример URI | Имя действия | Пример действия |
| --- | --- | --- | --- |
| /Entityset POST (ключ) и действие | И продукты (1) или скорость | ActionNameOnEntityType или имя действия | RateOnProduct |
| /Entityset (ключ) или приведения типов и действий, выполняемых после | / /Models.Book/CheckOut продуктов (1) | ActionNameOnEntityType или имя действия | CheckOutOnBook |

Дополнительные сведения см. в разделе [действия OData](odata-v3/odata-actions.md).

**Сигнатуры методов**

Ниже приведены некоторые правила для сигнатуры метода.

- Если путь содержит ключ, действие должен иметь параметр с именем *ключ*.
- Если путь содержит ключ в свойство навигации, действие должен иметь параметр с именем *relatedKey*.
- Украшение *ключ* и *relatedKey* параметров с **[FromODataUri]** параметра.
- Запросы PUT и POST, принимают параметр типа сущности.
- Запросы PATCH принимают параметр типа **дельта&lt;T&gt;**, где *T* является типом сущности.

Для справки ниже приведен пример, показывающий сигнатуры методов для каждого встроенных соглашения маршрутизации OData.

[!code-csharp[Main](odata-routing-conventions/samples/sample1.cs)]

<a id="custom"></a>
## <a name="custom-routing-conventions"></a>Пользовательские соглашения о маршрутизации

В настоящее время встроенных соглашения не охватывают все возможные URI OData. Можно добавить новые соглашения путем реализации **IODataRoutingConvention** интерфейса. Этот интерфейс содержит два метода:

[!code-csharp[Main](odata-routing-conventions/samples/sample2.cs)]

- **SelectController** возвращает имя контроллера.
- **SelectAction** возвращает имя действия.

Для обоих методов Если соглашение не применяется на этот запрос, метод должен возвращать значение null.

**ODataPath** представляет параметр синтаксического анализа пути ресурса OData. Он содержит список **[ODataPathSegment](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatapathsegment.aspx)** экземпляров, по одному для каждого сегмента пути к ресурсу. **ODataPathSegment** класс является абстрактным; каждого типа сегмента представляется с помощью класса, производного от **ODataPathSegment**.

**ODataPath.TemplatePath** свойство является строка, представляющая результат объединения всех сегментов пути. Например, если URL-адрес является `/Products(1)/Supplier`, является шаблон пути &quot;~/entityset/key/navigation&quot;. Обратите внимание, что сегменты не соответствуют напрямую сегментов URI-адреса. Например, ключ сущности (1) представляется как собственные **ODataPathSegment**.

Как правило, реализация **IODataRoutingConvention** делает следующее:

1. Сравните шаблон пути ли это соглашение о применяется для текущего запроса. Если он неприменим, возвращает значение null.
2. Если применяется соглашение, используйте свойства **ODataPathSegment** экземпляров для формирования контроллера и действия.
3. Для действий добавьте все значения в словарь маршрута, который должен быть привязан к параметрам действия (обычно ключи сущности).

Давайте взглянем на конкретный пример. Встроенные соглашений о маршрутизации не поддерживают индексирование в коллекции навигации. Другими словами имеют каких-либо для URI следующим образом:

[!code-javascript[Main](odata-routing-conventions/samples/sample3.js)]

Ниже приведен пользовательский соглашение о маршрутизации для обработки этого типа запросов.

[!code-csharp[Main](odata-routing-conventions/samples/sample4.cs)]

Примечания.

1. Я являются производными от **EntitySetRoutingConvention**, так как **SelectController** метода этого класса подходит для этой новой соглашение о маршрутизации. Это означает, что нет необходимости в повторной реализации **SelectController**.
2. Соглашение о применяется только для запросов GET, а только в том случае, когда шаблон пути будет &quot;~/entityset/key/navigation/key&quot;.
3. Имя действия &quot;получить {EntityType}&quot;, где *{EntityType}* тип навигации коллекции. Например &quot;GetSupplier&quot;. Можно использовать любое соглашение об именовании, что вам нравится & #8212; просто убедитесь, что соответствует действий контроллера.
4. Действие принимает два параметра с именем *ключ* и *relatedKey*. (Список некоторые имена стандартных параметров см. в разделе [ODataRouteConstants](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatarouteconstants.aspx).)

Следующим шагом является добавление новое соглашение список соглашений о маршрутизации. Это происходит во время настройки, как показано в следующем коде:

[!code-csharp[Main](odata-routing-conventions/samples/sample5.cs)]

Ниже приведены некоторые другие образец соглашений о маршрутизации, полезно для изучения.

- [CompositeKeyRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataCompositeKeySample/ODataCompositeKeySample/Extensions/CompositeKeyRoutingConvention.cs)
- [CustomNavigationRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataServiceSample/ODataService/Extensions/CustomNavigationRoutingConvention.cs)
- [NonBindableActionRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataActionsSample/ODataActionsSample/NonBindableActionRoutingConvention.cs)
- [ODataVersionRouteConstraint](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataVersioningSample/ODataVersioningSample/Extensions/ODataVersionRouteConstraint.cs)

И конечно веб-API, сам открытый исходный код, чтобы можно было видеть [исходный код](http://aspnetwebstack.codeplex.com/) для встроенных соглашений о маршрутизации. Они определяются в **System.Web.Http.OData.Routing.Conventions** пространства имен.
