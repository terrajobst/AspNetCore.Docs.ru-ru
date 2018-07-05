---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 'Часть 5: Создание динамического пользовательского интерфейса с помощью Knockout.js | Документация Майкрософт'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: 1f645b853fba999ade11c420eceba1b310f80ca3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368432"
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a>Часть 5: Создание динамического пользовательского интерфейса с помощью Knockout.js
====================
по [Майк Уоссон](https://github.com/MikeWasson)

[Скачать завершенный проект](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a>Создание динамического пользовательского интерфейса с помощью Knockout.js

В этом разделе мы будем использовать Knockout.js для добавления функций в представлении администратора.

[Knockout.js](http://knockoutjs.com/) — это библиотека Javascript, которая позволяет легко выполнить привязку к данным элементы управления HTML. Knockout.js использует шаблон Model-View-ViewModel (MVVM).

- *Модели* — это представление на сервере данные в предметной области бизнеса (в нашем случае продуктов и заказов).
- *Представление* является уровень представления (HTML).
- *Модель представления* — объект Javascript, которая содержит данные модели. Модель представления — это абстракция код пользовательского интерфейса. Он не имеет сведений о представление HTML. Вместо этого он представляет абстрактные функции, представления, например «список элементов».

Представление данных привязан к модели представления. Обновления для модели представления автоматически отражаются в представлении. Модель представления также получает события из представления, например нажатие кнопки и выполняет операции в модели, например создание заказа.

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

Сначала мы определим модели представления. После этого выполняется привязка HTML-разметка для модели представления.

Добавьте следующий раздел Razor Admin.cshtml:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

В этом разделе можно добавить в любом месте в файле. При визуализации представления, раздел отображается в нижней части страницы HTML, прямо перед закрывающим &lt;/body&gt; тега.

Весь сценарий для этой страницы будет находиться внутри тег сценария, обозначается комментарий:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

Во-первых определите класс модели представления:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

**ko.observableArray** — это особый тип объекта в Knockout, вызывается *наблюдаемый объект*. Из [Knockout.js документации](http://knockoutjs.com/documentation/observables.html): наблюдаемый объект — «объект JavaScript, можно уведомлять подписчиков об изменениях.» При изменении содержимого наблюдаемый объект, представление автоматически обновляется для соответствия.

Для заполнения `products` массива, выполнить запрос AJAX в веб-API. Помните, что мы храниться базовый URI для API в пакет представления (см. в разделе [часть 4](using-web-api-with-entity-framework-part-4.md) руководства).

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

Затем добавьте функции модели представления для создания, обновления и удаления продуктов. Эти функции отправки вызовов AJAX в веб-API и использовать результаты для обновления модели представления.

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

Теперь самое главное: когда модель DOM является fulled загружено, вызов **ko.applyBindings** функции и передайте новый экземпляр класса `ProductsViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

**Ko.applyBindings** метод активирует Knockout и сводит модели представления к представлению.

Теперь, когда у нас есть модель представления, можно создать привязки. В Knockout.js, это сделать, добавив `data-bind` атрибуты к элементам HTML. Например, чтобы привязать HTML-списка в массив, используйте `foreach` привязки:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

`foreach` Привязка используется для итерации по массиву и создает дочерние элементы для каждого объекта в массиве. Привязки для дочерних элементов может ссылаться на свойства объектов массива.

Добавьте следующие привязки в список «обновление продукты»:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

`<li>` Элемент присутствует в пределах **foreach** привязки. Что означает, что будет частичной визуализации элемента один раз для каждого продукта в `products` массива. Все привязки в `<li>` элементе ссылаться на этот экземпляр продукта. Например `$data.Name` ссылается на `Name` свойство над продуктом.

Чтобы задать значения текстовые входные данные, используйте `value` привязки. Кнопки привязаны к функциям на модель представление, с помощью `click` привязки. Экземпляр продукта передается как параметр для каждой функции. Дополнительные сведения [Knockout.js документации](http://knockoutjs.com/documentation/observables.html) имеет хорошее описания различных привязок.

Добавьте привязку для **отправить** событий в форме добавить продукт:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

Эта привязка вызывает `create` функции в модели представления для создания нового продукта.

Ниже приведен полный код для представления администрирования:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

Запустите приложение, войдите с помощью учетной записи администратора и щелкните ссылку «Admin». Следует просмотреть список продуктов и иметь возможность создания, обновления или удаления продуктов.

> [!div class="step-by-step"]
> [Назад](using-web-api-with-entity-framework-part-4.md)
> [Вперед](using-web-api-with-entity-framework-part-6.md)
