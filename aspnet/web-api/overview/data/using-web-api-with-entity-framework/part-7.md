---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Создание представления (UI) | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: eeaa36ede45b82afd112a270acf68105d1ae6dbc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820990"
---
<a name="create-the-view-ui"></a>Создание представления (пользовательский Интерфейс)
====================
по [Майк Уоссон](https://github.com/MikeWasson)

[Скачать завершенный проект](https://github.com/MikeWasson/BookService)

В этом разделе вам предстоит определить HTML для приложения и добавление привязки данных между HTML и модель представления.

Откройте файл Views/Home/Index.cshtml. Замените все содержимое этого файла следующим.

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

Большая часть `div` элементы существуют ли [Bootstrap](http://getbootstrap.com/) стиля. Важные элементы, имеющие `data-bind` атрибуты. Этот атрибут связывает HTML-код для модели представления.

Пример:

[!code-html[Main](part-7/samples/sample2.html)]

В этом примере &quot; `text` &quot; привязки причины `<p>` элемент для отображения значения `error` свойство из модели представления. Помните, что `error` был объявлен как `ko.observable`:

[!code-javascript[Main](part-7/samples/sample3.js)]

Каждый раз, когда назначается новое значение `error`, Knockout обновляет текст в `<p>` элемент.

`foreach` Привязки сообщает Knockout циклический перебор содержимое `books` массива. Для каждого элемента в массиве, создает новый Knockout &lt;li&gt; элемент. Привязки в контексте `foreach` ссылки на свойства для элемента массива. Пример:

[!code-html[Main](part-7/samples/sample4.html)]

Здесь `text` привязка позволяет считывать свойства Author каждую книгу.

Если запустить приложение сейчас, он должен выглядеть следующим образом:

![](part-7/_static/image1.png)

Список книг загружает в асинхронном режиме, после загрузки страницы. Прямо сейчас &quot;сведения&quot; ссылки не работают. В следующем разделе мы добавим эту функцию.

> [!div class="step-by-step"]
> [Назад](part-6.md)
> [Вперед](part-8.md)
