---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Часть 6: Создание продукта и порядок контроллеров | Документы Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: 6bd485d29821af12b9ebe31b2d04a2d9ab826731
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="part-6-creating-product-and-order-controllers"></a>Часть 6: Создание продукта и порядок контроллеров
====================
по [Mike Wasson](https://github.com/MikeWasson)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a>Добавить контроллер продуктов

Контроллер администратора — для пользователей с правами администратора. Клиенты, с другой стороны, можно просмотреть продукты, но невозможно создать, обновить или удалить их.

Мы легко можно ограничить доступ к методам Post, Put и Delete, не закрывая методы Get. Но просматривать данные, возвращаемые для продукта:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

`ActualCost` Свойства не должны быть видимыми для клиентов. Решение состоит в определении *объект передачи данных* (DTO), которая включает подмножество свойств, которые должны быть видимыми для клиентов. Мы будем использовать LINQ для проекта `Product` экземпляры `ProductDTO` экземпляров.

Добавьте класс с именем `ProductDTO` в папке «Models».

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

Теперь добавьте контроллера. В обозревателе решений щелкните правой кнопкой мыши папку Controllers. Выберите **добавить**, а затем выберите **контроллера**. В **добавления контроллера** диалога, имя контроллера &quot;ProductsController&quot;. В разделе **шаблона**выберите **контроллер пустой API**.

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

Замените весь код в файле исходного кода с помощью следующего кода:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

По-прежнему использует контроллер `OrdersContext` запросов к базе данных. Однако вместо возвращения `Product` экземпляры напрямую, мы называем `MapProducts` в проект их на `ProductDTO` экземпляров:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

`MapProducts` Возвращает **IQueryable**, поэтому мы можно составить результат с другими параметрами. Это можно увидеть в `GetProduct` метод, который добавляет **где** предложения запроса:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a>Добавить контроллер заказов

Затем добавьте контроллера, который позволяет пользователям создавать и просматривать заказы.

Мы начнем с другой DTO. В обозревателе решений щелкните правой кнопкой мыши папку модели и добавьте класс с именем `OrderDTO` используется следующая реализация:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

Теперь добавьте контроллера. В обозревателе решений щелкните правой кнопкой мыши папку Controllers. Выберите **добавить**, а затем выберите **контроллера**. В **добавить контроллер** диалогового окна, задайте следующие параметры:

- В разделе **имя контроллера**, введите «OrdersController».
- В разделе **шаблона**, выберите «Контроллер API с действиями чтения и записи, использующий Entity Framework».
- В разделе **класс модели**выберите &quot;порядке (ProductStore.Models)&quot;.
- В разделе **класс контекста данных**выберите &quot;OrdersContext (ProductStore.Models)&quot;.

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

Нажмите кнопку **Добавить**. При этом добавляется файл с именем OrdersController.cs. Далее нам нужно изменить реализацию контроллера по умолчанию.

Сначала удалите `PutOrder` и `DeleteOrder` методы. В этом образце клиентов нельзя изменить или удалить существующие заказы. В реальном приложении необходимо значительный объем внутренней логики для обработки таких случаев. (Например, был уже заказ?)

Изменение `GetOrders` метод для возврата только заказы, принадлежащие пользователю:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

Изменение `GetOrder` метод следующим образом:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

Ниже приведены изменения, которые мы внесли в метод.

- Возвращает значение `OrderDTO` экземпляра, а не `Order`.
- Когда мы запросов к базе данных для заказа, мы используем [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) метод, чтобы получить связанный `OrderDetail` и `Product` сущности.
- Мы одноуровневые результат с помощью проекции.

HTTP-ответ будет содержать массив продуктов с количествами:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

Этот формат является облегчает клиентам использование чем исходный объект графа, который содержит вложенные сущности (заказа, подробности и продуктах).

Последний метод учитывать его `PostOrder`. В данный момент, этот метод принимает `Order` экземпляра. Но что произойдет, если клиент отправляет текст запроса следующим образом:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

Это хорошо структурированный заказ и Entity Framework счастью будет вставлять его в базу данных. Однако она содержит сущности Product, которая ранее не существовал. Клиент только что созданный новый продукт в базе данных. Это будет неожиданные отделу fullfilment заказа при обнаружении заказ на koala медведей. Мораль, должны быть очень осторожными данные, которые принимают в запросе POST или PUT.

Чтобы избежать этой проблемы, измените `PostOrder` метод, чтобы использовать `OrderDTO` экземпляра. Используйте `OrderDTO` для создания `Order`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

Обратите внимание, что мы используем `ProductID` и `Quantity` свойства и мы игнорировать любые значения, отправляемых клиентом для имени продукта или цены. Если код продукта не является допустимым, он будет нарушать ограничение внешнего ключа в базе данных, и операция вставки завершится ошибкой, как должно.

Ниже приведен полный `PostOrder` метод:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

Наконец, добавьте **авторизовать** атрибут контроллера:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

Теперь только зарегистрированные пользователи можно создать или просмотреть заказы.

> [!div class="step-by-step"]
> [Назад](using-web-api-with-entity-framework-part-5.md)
> [Вперед](using-web-api-with-entity-framework-part-7.md)
