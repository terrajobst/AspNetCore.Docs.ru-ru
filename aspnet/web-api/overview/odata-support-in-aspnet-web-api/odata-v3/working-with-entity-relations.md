---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: "Поддержка отношениями сущностей в OData v3 с веб-API 2 | Документы Microsoft"
author: MikeWasson
description: "Большинство наборов данных определить отношения между сущностями: клиенты имеют заказы; у книги может быть авторов; продукты имеют поставщиков. С помощью OData, клиенты могут переходить по..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: dec7e10e59cc2441c967afe062df227b105106a1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a>Поддержка отношениями сущностей в OData v3 с веб-API 2
====================
по [Mike Wasson](https://github.com/MikeWasson)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Большинство наборов данных определить отношения между сущностями: клиенты имеют заказы; у книги может быть авторов; продукты имеют поставщиков. С помощью OData, клиенты могут переходить через отношения сущности. Имея продукта, можно найти сведения о поставщике. Также можно создать или удалить связи. Например можно задать сведения о поставщике для продукта.
> 
> Этот учебник показывает, как для поддержки этих операций в ASP.NET Web API. Учебник построен на учебника [Создание конечной точки OData v3 с веб-API 2](creating-an-odata-endpoint.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемая в этом учебнике
> 
> 
> - Веб-API 2
> - OData версии 3
> - Entity Framework 6


## <a name="add-a-supplier-entity"></a>Добавление сущности поставщика

Сначала необходимо добавить новый тип сущности для наших веб-канала OData. Мы добавим `Supplier` класса.

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

Этот класс использует строку для ключа сущности. На практике, который может быть реже, чем с помощью ключа целое число со знаком. Но стоит отображается как OData обрабатывает другие типы ключей, помимо целых чисел.

Далее предстоит создать отношение путем добавления `Supplier` свойства `Product` класса:

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

Добавьте новый **DbSet** для `ProductServiceContext` класса, чтобы платформа Entity Framework будет включать `Supplier` таблицы в базе данных.

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

В WebApiConfig.cs добавьте сущности «Поставщики» с моделью EDM.

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a>Свойства навигации

Чтобы получить сведения о поставщике для продукта, клиент отправляет запрос GET:

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

Здесь «Поставщик» является свойством навигации на `Product` типа. В этом случае `Supplier` ссылается один элемент, но навигацию свойство также может вернуть коллекцию (связь один ко многим "или" многие ко многим).

Для поддержки этого запроса, добавьте следующий метод `ProductsController` класса:

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

*Ключ* параметр — это ключ продукта. Метод возвращает связанный объект &#8212;в этом случае `Supplier` экземпляра. Имя метода и имени параметра являются как важные. Как правило если свойство навигации с именем «X», необходимо добавить метод с именем «Методы GetX». Метод не должен принимать параметр с именем «*ключ*», соответствующий тип данных ключа родительского элемента.

Также важно для включения **[FromOdataUri]** атрибута в *ключ* параметра. Этот атрибут сообщает веб-API для использования правилами синтаксиса OData при синтаксическом анализе ключ от URI запроса.

## <a name="creating-and-deleting-links"></a>Создание и удаление ссылки

OData поддерживает создание или удаление отношений между двумя сущностями. В терминологии OData связь является «связи». Каждая ссылка имеет URI с формой *сущности*/$links /*сущности*. Например ссылка из продукта в поставщик выглядит следующим образом:

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

Чтобы создать новую ссылку, клиент отправляет запрос POST к URI ссылки. Текст запроса является URI целевой сущности. Например предположим, что нет других поставщиков с помощью ключа «CTSO». Чтобы создать ссылку из «Product(1)» на «Supplier('CTSO')», клиент отправляет запрос следующим образом:

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

Чтобы удалить ссылку, клиент отправляет запрос DELETE URI ссылки.

**Создание ссылок**

Чтобы клиент для создания ссылки продукт поставщика, добавьте следующий код в `ProductsController` класса:

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

Этот метод принимает три параметра:

- *ключ*: ключа на родительский объект («продукт»)
- *Свойство navigationProperty*: имя свойства навигации. В этом примере свойства навигации единственным допустимым является «Поставщик».
- *ссылка*: URI OData связанной сущности. Это значение берется из текста запроса. Например, ссылка URI может быть "`http://localhost/odata/Suppliers('CTSO')`, это означает поставщика с Идентификатором = «CTSO».

Метод использует ссылки для поиска поставщика. При обнаружении соответствующего поставщика, метод задает `Product.Supplier` свойства и сохраняет результат в базе данных.

Самым сложным является синтаксический анализ URI ссылки. По сути необходимо моделировать результат при отправке запроса GET для этого идентификатора URI. Следующий вспомогательный метод показано, как это сделать. Этот метод вызывает процесс маршрутизации веб-API и получает обратно **ODataPath** экземпляр, представляющий синтаксического анализа пути OData. Для URI ссылки один из сегментов должно быть ключ сущности. (В противном случае клиент отправил недопустимый URI.)

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

**Удаление связей**

Чтобы удалить ссылку, добавьте следующий код в `ProductsController` класса:

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

В этом примере свойства навигации представляет собой одну `Supplier` сущности. Если свойство навигации является коллекцией, URI для удаления связи должен содержать ключ для связанной сущности. Пример:

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

Этот запрос удаляет заказа 1 из клиента 1. В этом случае метод DeleteLink будут иметь следующую сигнатуру:

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

*RelatedKey* параметр предоставляет ключ для связанной сущности. Так в вашей `DeleteLink` метод для поиска основной сущности по *ключ* параметра, найти связанные сущности, *relatedKey* параметра, а затем удалите связь. В зависимости от модели данных, может потребоваться реализовать обе версии `DeleteLink`. Веб-API вызовет правильную версию на основе запроса URI.
