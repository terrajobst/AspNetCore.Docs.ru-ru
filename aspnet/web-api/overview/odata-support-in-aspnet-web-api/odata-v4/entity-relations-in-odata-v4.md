---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: "Отношениями сущностей в OData v4, с помощью ASP.NET Web API 2.2 | Документы Microsoft"
author: MikeWasson
description: "Большинство наборов данных определить отношения между сущностями: клиенты имеют заказы; у книги может быть авторов; продукты имеют поставщиков. С помощью OData, клиенты могут переходить по..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 4127fb2fa83f513a4faeb222068ca99f234ece22
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a>Отношениями сущностей в OData v4, с помощью ASP.NET Web API 2.2
====================
по [Mike Wasson](https://github.com/MikeWasson)

> Большинство наборов данных определить отношения между сущностями: клиенты имеют заказы; у книги может быть авторов; продукты имеют поставщиков. С помощью OData, клиенты могут переходить через отношения сущности. Имея продукта, можно найти сведения о поставщике. Также можно создать или удалить связи. Например можно задать сведения о поставщике для продукта.
> 
> Этот учебник показывает, как для поддержки этих операций OData v4, с помощью веб-API ASP.NET. Учебник построен на учебника [создания OData v4 конечной точки с помощью веб-API ASP.NET 2](create-an-odata-v4-endpoint.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемая в этом учебнике
> 
> 
> - Веб-API 2.1
> - OData v4
> - [Visual Studio 2013 с обновлением 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Версии учебника
> 
> OData версии 3, в разделе [поддержки отношениями сущностей в OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).


## <a name="add-a-supplier-entity"></a>Добавление сущности поставщика

> [!NOTE]
> Учебник построен на учебника [создания OData v4 конечной точки с помощью веб-API ASP.NET 2](create-an-odata-v4-endpoint.md).


Во-первых мы должны связанной сущности. Добавьте класс с именем `Supplier` в папке «Models».

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

Свойство навигации, чтобы добавить `Product` класса:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

Добавьте новый **DbSet** для `ProductsContext` класса, чтобы платформа Entity Framework будет включать таблицы «Поставщики» в базе данных.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

Добавьте в WebApiConfig.cs, &quot;поставщики&quot; набора сущностей в модели EDM:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a>Добавить контроллер поставщиков

Добавить `SuppliersController` класс в папку контроллеров.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

Я не отображаются, как добавить операции CRUD для этого контроллера. Действия, идентичны представлениям контроллера Products (см. [Создайте конечную точку OData v4](create-an-odata-v4-endpoint.md)).

## <a name="getting-related-entities"></a>Получение связанных сущностей.

Чтобы получить сведения о поставщике для продукта, клиент отправляет запрос GET:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

Для поддержки этого запроса, добавьте следующий метод `ProductsController` класса:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

Этот метод использует соглашение об именовании по умолчанию

- Имя метода: методы GetX, где X — свойство навигации.
- Имя параметра: *ключ*

Если выполнить это соглашение об именовании, веб-API автоматически сопоставляет HTTP-запроса метода контроллера.

Пример HTTP-запроса:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

Пример HTTP-ответа:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a>Получение связанной коллекции

В предыдущем примере продукт имеет один поставщик. Свойство навигации также можно вернуть коллекцию. Следующий код возвращает продукты для других поставщиков:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

В этом случае метод возвращает **IQueryable** вместо **SingleResult&lt;T&gt;**

Пример HTTP-запроса:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

Пример HTTP-ответа:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a>Создание связи между сущностями

OData поддерживает создание или удаление связи между двумя существующими сущностями. В терминологии OData v4, связь является &quot;ссылка&quot;. (OData v3 связь была вызвана *ссылку*. Протокол различия не имеет значения для этого учебника.)

Ссылка имеет свой собственный URI с формой `/Entity/NavigationProperty/$ref`. Например вот URI для решения ссылки между продукта и его поставщиком.

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

Чтобы добавить связь, клиент отправляет запрос POST или PUT на этот адрес.

- ПЕРЕВЕСТИ, если свойство навигации является единственной сущностью, такой как `Product.Supplier`.
- Учет, если свойство навигации коллекции, такие как `Supplier.Products`.

Текст запроса содержит URI сущности, в связи. Ниже приведен пример запроса.

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

В этом примере клиент отправляет запрос PUT в `/Products(6)/Supplier/$ref`, который является URI $ref для `Supplier` продукта с Идентификатором = 6. Если запрос выполнен успешно, сервер отправляет ответ 204 (нет содержимого):

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

Ниже приведен метод контроллера, чтобы добавить связь с `Product`:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

*NavigationProperty* параметр указывает, какие связи для задания. (Если имеется более одного свойства навигации сущности, можно добавить несколько `case` инструкции.)

*Ссылку* параметр содержит URI поставщика. Веб-API автоматически анализирует текст запроса для получения значения для этого параметра.

Чтобы получить сведения о поставщике, нам нужно ID (или ключа), который является частью *ссылку* параметра. Чтобы сделать это, используйте следующий вспомогательный метод:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

По сути этот метод использует библиотеку OData разделение на сегменты пути URI и найдите сегмент, содержащий ключ преобразует ключ в правильный тип.

## <a name="deleting-a-relationship-between-entities"></a>Удаление связи между сущностями

Чтобы удалить связь, клиент отправляет запрос HTTP DELETE $ref URI:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

Далее приводится метод контроллера удаление связи между продуктом и других поставщиков.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

В этом случае `Product.Supplier` — &quot;1&quot; конец отношения 1-ко многим, чтобы можно было удалить связи достаточно установить `Product.Supplier` для `null`.

В &quot;много&quot; конец связи клиента необходимо указать, какие связанные сущности для удаления. Чтобы сделать это, клиент отправляет URI связанной сущности в строке запроса. Например, чтобы удалить «Product 1» из «Поставщик 1»:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

Для поддержки этой веб-API необходимо включить дополнительный параметр в `DeleteRef` метод. Ниже приведен метод контроллера, чтобы удалить продукт из `Supplier.Products` отношения.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

*Ключ* параметра является ключом для поставщика и *relatedKey* параметр — это ключ продукта для удаления из `Products` связи. Обратите внимание, что веб-API автоматически получает ключ из строки запроса.
