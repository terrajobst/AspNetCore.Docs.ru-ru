---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Часть 7: Создание главной страницы | Документация Майкрософт'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: b748813046637b9972bc96e22c35d2eeb9b57f9d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378901"
---
<a name="part-7-creating-the-main-page"></a>Часть 7: Создание главной страницы
====================
по [Майк Уоссон](https://github.com/MikeWasson)

[Скачать завершенный проект](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a>Создание главной страницы

В этом разделе вы создадите страницу основного приложения. Эта страница будет сложнее, чем для страницы, поэтому мы будем подходите к ней в несколько этапов. Кроме того вы увидите некоторые более сложные приемы Knockout.js. Ниже приведен базовый макет страницы.

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- «Продукты» содержит массив продуктов.
- «Корзины» содержит массив продуктов с количествами. Нажав кнопку «Add to Cart» обновляет корзины для покупок.
- «Orders» содержит массив идентификаторов заказов.
- «Подробности» содержит подробности заказа, который является массивом элементов (продукты с количествами)

Мы начнем с определения некоторых базовый макет в формате HTML, без привязки данных или скрипт. Откройте файл Views/Home/Index.cshtml и замените содержимое следующим кодом:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

Затем добавьте раздел скриптов и создать пустую модель представления:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

Исходя из конструктора, ранее в виде эскиза, наша модель представления должна наблюдаемые объекты для продуктов, для покупок, заказы и сведения. Добавьте следующие переменные `AppViewModel` объекта:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

Пользователей можно добавить элементы из списка продуктов в корзине и удаления элементов из корзины. Чтобы инкапсулировать эти функции, мы создадим другого класса модели представления, который представляет продукт. Добавьте следующий код в файл `AppViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

`ProductViewModel` Класс содержит две функции, которые используются для перемещения продукта и из корзины: `addItemToCart` добавляет одну единицу продукта в корзину и `removeAllFromCart` удаляет все количества продукта.

Пользователи могут выбрать существующий заказ и получить сведения о заказе. Мы оформим этих функций в другой модели представления:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

`OrderDetailsViewModel` Инициализируется с порядком, и она получает сведения о заказе, отправляя запрос AJAX на сервер.

Кроме того, обратите внимание, что `total` свойство `OrderDetailsViewModel`. Это свойство — это особый тип вызывается наблюдаемого объекта [вычисляемые наблюдаемый объект](http://knockoutjs.com/documentation/computedObservables.html). Как и предполагает название, вычисляемый наблюдаемым позволяет привязки данных к вычисляемым значением&#8212;в этом случае общая стоимость заказа.

Затем добавьте эти функции для `AppViewModel`:

- `resetCart` Удаляет все элементы из корзины.
- `getDetails` Возвращает сведения для заказа (с pusing новый `OrderDetailsViewModel` на `details` списка).
- `createOrder` Создает новый порядок и очистке корзины для покупок.


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

Наконец инициализируйте модели представления, отправку запросов AJAX для продуктов и заказов:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

Итак, вот большой объем кода, но мы создали его вверх пошаговые, будем надеяться разработки снят. Теперь мы можем добавить некоторые привязки Knockout.js в HTML-код.

**Продукты**

Ниже приведены привязки для списка продуктов.

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

Это используется для итерации по массиву продуктов и отображает название и цену. Кнопка «Добавить к Order» отображается только в том случае, когда пользователь входит в систему.

Вызовы кнопку «Добавить на заказ» `addItemToCart` на `ProductViewModel` экземпляра для продукта. Этот пример демонстрирует приятной особенностью Knockout.js: Если модель представления содержит другие модели представлений, к внутренним модели можно применить привязок. В этом примере привязки в пределах `foreach` применяются к каждому из `ProductViewModel` экземпляров. Этот подход является более понятны, чем хранение всех функций в одной модели представления.

**Для покупок**

Ниже приведены привязки для корзины для покупок.

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

Это используется для итерации по массиву корзины для покупок и отображает имя, цену и количество. Обратите внимание на то, что ссылку «Удалить» и кнопка «Создание заказа» привязаны к функции модели представления.

**Заказы**

Ниже приведены привязки для списка заказов.

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

Это выполняет итерацию по заказам и показывает идентификатора заказа. Событие щелчка по ссылке, привязан к `getDetails` функции.

**Сведения о заказе**

Ниже приведены привязки для сведений о заказе.

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

Это выполняет итерацию по элементам в порядке и отображает продукта, цену и quanity. Окружающий div отображается только в том случае, если сведения о массив содержит один или несколько элементов.

## <a name="conclusion"></a>Заключение

В этом руководстве вы создали приложение, использующее Entity Framework для взаимодействия с базой данных и веб-API ASP.NET и предоставляет общедоступный интерфейс поверх уровня данных. Мы используем ASP.NET MVC 4 для отрисовки HTML-страниц и Knockout.js плюс jQuery для обеспечения динамического взаимодействия без перезагрузок страницы.

Дополнительные ресурсы:

- [Схема содержимого для доступа к данным ASP.NET](https://msdn.microsoft.com/library/6759sth4.aspx)
- [Центр разработчиков Entity Framework](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [Назад](using-web-api-with-entity-framework-part-6.md)
