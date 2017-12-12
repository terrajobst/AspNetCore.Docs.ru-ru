---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: "Создание представления (UI) | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 8c5cc662e2e3c9cb07ca9e30ff57eb084d58e1bb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="create-the-view-ui"></a>Создание представления (UI)
====================
по [Mike Wasson](https://github.com/MikeWasson)

[Загрузка завершенного проекта](https://github.com/MikeWasson/BookService)

В этом разделе сначала для определения HTML-код для приложения и добавление привязки данных между HTML и модели представления.

Откройте файл Views/Home/Index.cshtml. Замените все содержимое этого файла следующим.

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

Большинство `div` элементы имеют [начальной загрузки](http://getbootstrap.com/) Задание стиля. Важные элементы, имеющие `data-bind` атрибуты. Этот атрибут связывает HTML модели представления.

Пример:

[!code-html[Main](part-7/samples/sample2.html)]

В этом примере &quot; `text` &quot; привязки причины `<p>` элемент, чтобы отобразить значение `error` свойство из модели представления. Помните, что `error` был объявлен как `ko.observable`:

[!code-javascript[Main](part-7/samples/sample3.js)]

Каждый раз, когда задается новое значение `error`, маскирования обновит текст в `<p>` элемент.

`foreach` Привязки указывает маскирования перебор содержимое `books` массива. Для каждого элемента в массиве маскирования создает новую &lt;li&gt; элемента. Привязки в контексте `foreach` ссылки на свойства на элемент массива. Пример:

[!code-html[Main](part-7/samples/sample4.html)]

Здесь `text` привязки считывает свойство Author каждой книги.

Если приложение запускается, он должен выглядеть следующим образом:

![](part-7/_static/image1.png)

Список книг загружает асинхронно, после загрузки страницы. Прямо сейчас &quot;сведения&quot; ссылки не работают. Эта функция будет добавлен в следующем разделе.

>[!div class="step-by-step"]
[Назад](part-6.md)
[Вперед](part-8.md)
