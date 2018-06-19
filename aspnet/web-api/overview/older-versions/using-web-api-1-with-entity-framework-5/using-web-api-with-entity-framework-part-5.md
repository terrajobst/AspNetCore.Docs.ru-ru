---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 'Часть 5: Создание динамического пользовательского интерфейса с помощью Knockout.js | Документы Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: b63446d076fbb1143641dead788042967b996bf8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873814"
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a>Часть 5: Создание динамического пользовательского интерфейса с помощью Knockout.js
====================
по [Mike Wasson](https://github.com/MikeWasson)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a>Создание динамического пользовательского интерфейса с помощью Knockout.js

В этом разделе мы будем использовать Knockout.js Добавление функциональности в представления администрирования.

[Knockout.js](http://knockoutjs.com/) — это библиотека Javascript, которая позволяет легко привязать к данным элементы управления HTML. Knockout.js использует шаблон Model-View-ViewModel (MVVM).

- *Модель* серверные представление данных в бизнес-среде (в нашем случае продуктов и заказов).
- *Представление* является уровень представления (HTML).
- *Модель представления* представляет собой объект Javascript, который содержит данные модели. Модель представления — абстракция код пользовательского интерфейса. Он не имеет сведений о представление HTML. Вместо этого он представляет абстрактные функции, представления, такие как «список элементов».

Представление данных привязан к модели представления. Обновления для модели представления автоматически отражаются в представлении. Модель представления также получает события из представления, например нажатие кнопки и выполняет операции с моделью, таких как создание заказа.

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

Сначала мы определим модели представления. После этого выполняется привязка HTML-разметка для представления модели.

Добавьте следующий раздел Razor Admin.cshtml:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

В этом разделе можно добавить в любом месте в файле. При отображении представления раздел отображается в нижней части HTML-страницу правой перед закрывающим тегом &lt;/body&gt; тег.

Весь сценарий для этой страницы будет находиться внутри тега script, обозначенных комментарием:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

Во-первых можно определите класс модели представления.

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

**ko.observableArray** — это особый тип объекта в маскирования, вызывается *наблюдаемый объект*. Из [Knockout.js документации](http://knockoutjs.com/documentation/observables.html): наблюдаемым является «объект JavaScript, можно уведомлять подписчиков об изменениях.» При изменении содержимого наблюдаемый объект, представление автоматически обновляется для соответствия.

Для заполнения `products` массива, сделать запрос AJAX в веб-API. Помните, что мы хранятся базовый URI для API-интерфейса в пакет представления (см. [часть 4](using-web-api-with-entity-framework-part-4.md) учебника).

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

Затем добавьте функции модели представления создания, обновления и удаления продуктов. Эти функции отправки вызовов AJAX в веб-API и использовать результаты для обновления модели представления.

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

Теперь наиболее важной частью: когда модель DOM является fulled загружено, вызов **ko.applyBindings** функции и передайте новый экземпляр `ProductsViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

**Ko.applyBindings** метод активирует Knockout и настраивающий модели представления в представление.

Теперь, когда у нас есть модель представления, можно создать привязки. В Knockout.js, это можно сделать, добавив `data-bind` атрибутов к элементам HTML. Например, чтобы привязать список HTML в массив, используйте `foreach` привязки:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

`foreach` Привязки перебор массива и создает дочерние элементы для каждого объекта в массиве. Привязки для дочерних элементов может ссылаться на свойства объектов массива.

Добавьте следующие привязки к списку «обновление продукты»:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

`<li>` Элемент присутствует в области **foreach** привязки. То, что означает маскирования будет отображаться элемент один раз для каждого продукта в `products` массива. Все привязки в `<li>` элемент ссылаться на этот экземпляр продукта. Например `$data.Name` ссылается на `Name` свойство на продукт.

Чтобы задать значения поля ввода, используйте `value` привязки. Кнопки привязаны к функциям на представление модели с помощью `click` привязки. Экземпляр продукта передается как параметр для каждой функции. Для получения дополнительной информации [Knockout.js документации](http://knockoutjs.com/documentation/observables.html) имеет хорошее описания различных привязок.

Добавьте привязку для **отправить** событий в форме добавить продукт:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

Эта привязка вызывает `create` функции в модель представления для создания нового продукта.

Ниже приведен полный код для представления администрирования:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

Запустите приложение, войдите с помощью учетной записи администратора и щелкните ссылку «Admin». Просмотреть список продуктов и иметь возможность создания, обновления или удаления продуктов.

> [!div class="step-by-step"]
> [Назад](using-web-api-with-entity-framework-part-4.md)
> [Вперед](using-web-api-with-entity-framework-part-6.md)
