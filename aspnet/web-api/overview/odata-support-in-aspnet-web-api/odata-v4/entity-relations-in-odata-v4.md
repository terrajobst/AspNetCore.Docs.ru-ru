---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: Отношения сущностей в OData v4 с помощью ASP.NET Web API 2.2 | Документация Майкрософт
author: MikeWasson
description: 'В большинстве наборов данных определения отношений между сущностями: клиенты имеют заказы; у книги может быть авторов; продукты, имеют поставщики. С помощью OData, клиенты могут переходить по...'
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 80173519f1c8abd77b4138b7d29f780ffc60a188
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828798"
---
<a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a>Отношения сущностей в OData v4 с помощью ASP.NET Web API 2.2
====================
по [Майк Уоссон](https://github.com/MikeWasson)

> В большинстве наборов данных определения отношений между сущностями: клиенты имеют заказы; у книги может быть авторов; продукты, имеют поставщики. С помощью OData, клиенты можно переходить через отношения сущности. Учитывая продукта, можно найти поставщика. Также можно создать или удалить связи. Например можно задать поставщик для продукта.
> 
> Этом руководстве показано, как для поддержки этих операций в OData v4, с помощью веб-API ASP.NET. Учебном курсе руководство [создания OData v4 конечной точки с помощью веб-API ASP.NET 2](create-an-odata-v4-endpoint.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемые в этом руководстве
> 
> 
> - Веб-API 2.1
> - OData v4
> - [Visual Studio 2013 с обновлением 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Учебника по версии
> 
> OData версии 3, см. в разделе [поддержка отношений сущностей в OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).


## <a name="add-a-supplier-entity"></a>Добавление сущности Supplier

> [!NOTE]
> Учебном курсе руководство [создания OData v4 конечной точки с помощью веб-API ASP.NET 2](create-an-odata-v4-endpoint.md).


Во-первых мы должны связанной сущности. Добавьте класс с именем `Supplier` в папку Models.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

Добавьте свойство навигации, чтобы `Product` класса:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

Добавьте новый **DbSet** для `ProductsContext` класса, таким образом, чтобы платформа Entity Framework будет включать в таблице поставщиков в базе данных.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

В файле WebApiConfig.cs добавьте &quot;поставщики&quot; набора сущностей в модели EDM:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a>Добавить контроллер поставщики

Добавление `SuppliersController` класс в папку "контроллеры".

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

Я не будет показано, как добавлять операции CRUD для этого контроллера. Действия не отличаются от контроллера Products (см. в разделе [Создайте конечную точку OData v4](create-an-odata-v4-endpoint.md)).

## <a name="getting-related-entities"></a>Получение связанных сущностей

Чтобы получить поставщик для продукта, клиент отправляет запрос GET:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

Чтобы этот запрос в службу поддержки, добавьте следующий метод в `ProductsController` класса:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

Этот метод использует соглашение об именовании по умолчанию

- Имя метода: GetX, где X — свойство навигации.
- Имя параметра: *ключ*

Если следовать соглашению по именованию, веб-API автоматически сопоставляет HTTP-запроса для метода контроллера.

Пример HTTP-запроса:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

Пример ответа HTTP:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a>Получение связанной коллекции

В предыдущем примере продукт имеет один поставщик. Свойство навигации может также возвращать коллекцию. Следующий код получает продукты для поставщика:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

В этом случае метод возвращает **IQueryable** вместо **— SingleResult&lt;T&gt;**

Пример HTTP-запроса:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

Пример ответа HTTP:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a>Создание связи между сущностями

OData поддерживает создание или удаление связи между двумя существующими сущностями. В терминологии OData v4, связь является &quot;ссылку&quot;. (OData v3, связь была вызвана *ссылку*. Различия протокола не имеет значения для этого руководства.)

Ссылка имеет свой собственный URI с формой `/Entity/NavigationProperty/$ref`. Например вот URI для адресации ссылки между продуктом и его поставщиком:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

Добавление отношения, клиент отправляет запрос POST или PUT по этому адресу.

- ПОМЕСТИТЬ, если свойство навигации является единственной сущностью, такой как `Product.Supplier`.
- Учет, если свойство навигации является коллекцией, такие как `Supplier.Products`.

Текст запроса содержит идентификатор URI сущности, в связи. Ниже приведен пример запроса:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

В этом примере клиент отправляет запрос PUT к `/Products(6)/Supplier/$ref`, который представляет собой $ref URI `Supplier` продукта с Идентификатором = 6. Если запрос выполнен успешно, сервер отправляет ответа 204 (нет содержимого):

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

Ниже приведен метод контроллера, чтобы добавить отношение к `Product`:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

*NavigationProperty* параметр указывает, какие связи для задания. (Если имеется более одного свойства навигации в сущности, можно добавить несколько `case` инструкций.)

*Ссылку* параметр содержит URI поставщика. Веб-API автоматически выполняет синтаксический анализ текста запроса для получения значения для этого параметра.

Чтобы найти поставщика, нам нужен идентификатор (или ключ), который является частью *ссылку* параметра. Чтобы сделать это, используйте следующий вспомогательный метод:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

По сути этот метод использует библиотеку OData разбить на сегменты пути URI, найти сегмент, который содержит ключ и преобразовать его в правильный тип.

## <a name="deleting-a-relationship-between-entities"></a>Удаление связи между сущностями

Чтобы удалить связь, клиент отправляет запрос HTTP DELETE к $ref URI:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

Ниже приведен метод контроллера, удаление связи между продуктом и других поставщиков.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

В этом случае `Product.Supplier` — &quot;1&quot; конец отношения 1-ко многим, чтобы вы могли удалить связи, просто задав `Product.Supplier` для `null`.

В &quot;многих&quot; конечную точку связи клиента необходимо указать, какие связанные сущности для удаления. Чтобы сделать это, клиент отправляет URI связанной сущности в строке запроса. Например, чтобы удалить «Product 1» из «Поставщик 1":

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

Для этого в веб-API, нам нужно включать дополнительный параметр в `DeleteRef` метод. Ниже приведен метод контроллера, чтобы удалить продукт из `Supplier.Products` отношения.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

*Ключ* является основой для поставщика и *relatedKey* параметр — это ключ продукта для удаления из `Products` связи. Обратите внимание на то, что веб-API автоматически получает ключ из строки запроса.
