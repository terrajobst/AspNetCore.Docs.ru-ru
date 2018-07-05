---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Часть 6: Создание контроллеров продуктов и заказов | Документация Майкрософт'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: 08abc66624526f4b1931231d114158afe8b63247
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382852"
---
<a name="part-6-creating-product-and-order-controllers"></a>Часть 6: Создание контроллеров продуктов и заказов
====================
по [Майк Уоссон](https://github.com/MikeWasson)

[Скачать завершенный проект](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a>Добавить контроллер продуктов

Контроллера администрирования является для пользователей с правами администратора. Клиенты, с другой стороны, можно Просмотр продуктов, но нельзя создать, обновить или удалить их.

Мы можно легко ограничить доступ к методам Post, Put и Delete, не закрывая методы Get. Но посмотрите на данные, возвращаемые для продукта:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

`ActualCost` Свойства не должны быть видимыми для клиентов! Решением является определение *объект передачи данных* (DTO), которая включает подмножество свойств, которые должны отображаться для клиентов. Мы будем использовать LINQ для проекта `Product` экземпляры `ProductDTO` экземпляров.

Добавьте класс с именем `ProductDTO` папке «модели».

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

Теперь добавьте контроллер. В обозревателе решений щелкните правой кнопкой мыши папку Controllers. Выберите **добавить**, а затем выберите **контроллера**. В **Добавление контроллера** диалоговое окно, назовите контроллер &quot;ProductsController&quot;. В разделе **шаблона**выберите **контроллер пустой API**.

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

Замените весь код в исходном файле следующим кодом:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

Контроллер по-прежнему использует `OrdersContext` для запроса к базе данных. Но вместо возвращения `Product` экземпляры напрямую, мы называем `MapProducts` проецировать их на `ProductDTO` экземпляров:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

`MapProducts` Возвращает **IQueryable**, поэтому составляется результат с другими параметрами запроса. Это можно увидеть в `GetProduct` метод, который добавляет **где** к запросу выражение WHERE:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a>Добавить контроллер заказов

Добавьте контроллер, который позволяет пользователям создавать и просматривать заказы.

Мы начнем с другой DTO. В обозревателе решений щелкните правой кнопкой мыши папку Models и добавьте класс с именем `OrderDTO` использовать следующую реализацию:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

Теперь добавьте контроллер. В обозревателе решений щелкните правой кнопкой мыши папку Controllers. Выберите **добавить**, а затем выберите **контроллера**. В **Добавление контроллера** диалоговое окно, задайте следующие параметры:

- В разделе **имя контроллера**, введите «Orderscontroller, который».
- В разделе **шаблона**, выберите «Контроллер API с действиями чтения и записи, с помощью Entity Framework».
- В разделе **класс модели**выберите &quot;порядке (ProductStore.Models)&quot;.
- В разделе **класс контекста данных**выберите &quot;OrdersContext (ProductStore.Models)&quot;.

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

Нажмите кнопку **Добавить**. Добавляется файл с именем OrdersController.cs. Далее нам нужно изменить реализацию контроллера по умолчанию.

Сначала удалите `PutOrder` и `DeleteOrder` методы. В этом примере клиенты нельзя изменить или удалить существующие заказы. В реальном приложении вам потребуется логика серверной части для обработки таких случаев. (Например, порядок уже отправлен?)

Изменение `GetOrders` метод для возврата только заказы, принадлежащие пользователю:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

Изменение `GetOrder` метод следующим образом:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

Ниже перечислены изменения, которые мы внесли в метод.

- Возвращает значение `OrderDTO` экземпляра, а не `Order`.
- Когда мы запросим базы данных для заказа, мы используем [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) метод для получения связанных `OrderDetail` и `Product` сущностей.
- Мы преобразовать результат с помощью проекции.

HTTP-ответа будет содержать массив продуктов с количества:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

Этот формат не облегчает клиентам использовать, чем исходный объект графа, который содержит вложенные сущности (порядок, подробности и продуктах).

Последний метод рассматривать ее `PostOrder`. Прямо сейчас, этот метод принимает `Order` экземпляра. Но что произойдет, если клиент отправляет текст запроса следующим образом:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

Это хорошо структурированный заказ, и платформа Entity Framework к счастью вставит его в базу данных. Но он содержит сущности Product, которая раньше не было. Клиент только что создали новый продукт в базе данных! Это будет предусмотрена отдел fullfilment заказов, при просмотре заказ koala медведей. Мораль, необходимо быть очень внимательным, о данных, которые вы принимаете в запросе POST или PUT.

Чтобы избежать этой проблемы, измените `PostOrder` метод, чтобы использовать `OrderDTO` экземпляра. Используйте `OrderDTO` для создания `Order`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

Обратите внимание, что мы используем `ProductID` и `Quantity` свойства и мы игнорировать любые значения, которые клиент отправил название продукта и цену. Если код продукта не является допустимым, он будет нарушать ограничение внешнего ключа в базе данных и вставки завершится ошибкой, как должно.

Ниже приведен полный `PostOrder` метод:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

Наконец, добавьте **Authorize** атрибута к контроллеру:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

Теперь только зарегистрированные пользователи могут создавать или Просмотр заказов.

> [!div class="step-by-step"]
> [Назад](using-web-api-with-entity-framework-part-5.md)
> [Вперед](using-web-api-with-entity-framework-part-7.md)
