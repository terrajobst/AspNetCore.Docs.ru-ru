---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Часть 7: Создание основной страницы | Документы Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: 2c378e68a1e6600daf655c19afbfe355e89496d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/10/2018
---
<a name="part-7-creating-the-main-page"></a>Часть 7: Создание основной страницы
====================
по [Mike Wasson](https://github.com/MikeWasson)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a>Создание основной страницы

В этом разделе вы создадите страница основного приложения. Эта страница будет сложнее, чем для страницы, поэтому мы будем подхода в нескольких шагах. Попутно вы увидите более сложные приемы Knockout.js. Ниже приведен базовый макет страницы.

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- «Продукты» содержит массив продуктов.
- «Корзина» содержит массив продуктов с количествами. Нажав кнопку «Добавить в корзину» обновляет покупок.
- «Orders» содержит массив идентификаторы заказов.
- «Подробности» содержит сведения о заказе, который является массивом элементов (продукты с количествами)

Мы начнем путем определения некоторых базовый макет в формате HTML, без привязки данных или скрипт. Откройте файл Views/Home/Index.cshtml и замените содержимое следующим кодом:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

Затем добавьте раздел скриптов и создать пустую модель представление:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

На основе дизайна составила более ранних версий, нашей модели представления должен наблюдаемые объекты для продуктов, покупок, заказов и сведений. Добавьте следующие переменные для `AppViewModel` объекта:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

Пользователей можно добавить элементы из списка продуктов в корзине и удаления элементов из корзины. Чтобы инкапсулировать эти функции, мы создадим другой класс модели представления, который представляет продукт. Добавьте следующий код в файл `AppViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

`ProductViewModel` Класс содержит две функции, которые используются для перемещения продукта к и из корзины: `addItemToCart` добавляет одну единицу продукта в корзину и `removeAllFromCart` удаляет все количества продукта.

Пользователи могут выбрать существующий заказ и получить сведения о заказе. Мы будем инкапсулировать эти функции в другой модели представления:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

`OrderDetailsViewModel` Инициализируется с заказом, и он извлекает сведения о заказе, отправив запрос AJAX на сервер.

Кроме того, обратите внимание, `total` свойство `OrderDetailsViewModel`. Это свойство имеет специальный вид наблюдаемый объект называется [вычисляемые наблюдаемый объект](http://knockoutjs.com/documentation/computedObservables.html). Как следует из имен, вычисляемых наблюдаемым позволяет привязки данных к вычисляемым значением&#8212;в этом случае общее стоимость заказа.

Добавьте эти функции для `AppViewModel`:

- `resetCart` Удаляет все элементы из корзины.
- `getDetails` Получает сведения для заказа (с pusing новый `OrderDetailsViewModel` на `details` списка).
- `createOrder` Создает новый порядок и очистке корзины для покупок.


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

Наконец инициализируйте модели представления, выполняющим запросы AJAX products и orders:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

ОК большого объема кода, но мы создали его вверх пошаговые инструкции надеемся проекта снимите флажок. Теперь можно добавить некоторые Knockout.js привязки к элементу HTML.

**Продукты**

Ниже приведены привязки для списка продуктов.

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

При этом выполняет итерацию по массиву продуктов и отображаются имя и цену. Кнопка «Добавить для заказа» отображается только в том случае, когда пользователь входит в систему.

Вызовы кнопка «Добавить для заказа» `addItemToCart` на `ProductViewModel` экземпляра для продукта. Этот пример демонстрирует интересной функцией Knockout.js: Если модель представления содержит другие Просмотр моделей, привязки можно применять к внутренние модели. В этом примере привязок в `foreach` применяются к каждому из `ProductViewModel` экземпляров. Этот подход гораздо очистки, помещая все функциональные возможности в одной модели представления.

**Корзина**

Ниже приведены привязки для покупок.

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

При этом выполняет итерацию по массиву покупок и отображаются имя, цену и количество. Обратите внимание, что ссылку «Удалить» и «Создание заказа» кнопка привязаны к функции модели представления.

**Заказы**

Ниже приведены привязки для списка заказов.

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

Это выполняет итерацию по заказам и показывает идентификационный номер заказа. События щелчка по ссылке, привязан к `getDetails` функции.

**Сведения о заказе**

Ниже приведены привязки для сведений о заказе.

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

Перебор элементов в том порядке оно отображает продукта, цену и quanity. Окружающей div виден только в том случае, если сведения о массив содержит один или несколько элементов.

## <a name="conclusion"></a>Заключение

В этом учебнике вы создали приложение, использующее Entity Framework для взаимодействия с базой данных и веб-API ASP.NET для предоставления интерфейса общедоступный поверх уровня данных. Мы используем ASP.NET MVC 4 для отрисовки HTML-страниц и Knockout.js плюс jQuery для обеспечения динамического взаимодействия без перезагрузки страницы.

Дополнительные ресурсы:

- [Карта содержимого для доступа к данных ASP.NET](https://msdn.microsoft.com/library/6759sth4.aspx)
- [Центр разработчиков Entity Framework](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [Назад](using-web-api-with-entity-framework-part-6.md)
