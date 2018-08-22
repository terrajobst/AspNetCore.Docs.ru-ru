---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Добавить новый элемент в базу данных | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 01586ef6eecdbe1acfadfcfcc0c9ca8b442de2d3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824095"
---
<a name="add-a-new-item-to-the-database"></a>Добавить новый элемент в базу данных
====================
по [Майк Уоссон](https://github.com/MikeWasson)

[Скачать завершенный проект](https://github.com/MikeWasson/BookService)

В этом разделе вы добавите возможность для пользователей создать новую книгу. В файле app.js добавьте следующий код для модели представления:

[!code-javascript[Main](part-9/samples/sample1.js)]

В файле Index.cshtml Замените следующую разметку:

[!code-html[Main](part-9/samples/sample2.html)]

На:

[!code-html[Main](part-9/samples/sample3.html)]

Эта разметка создает формы для отправки нового автора. Значения для раскрывающегося списка автор привязаны к `authors` наблюдаемые в модели представления. Для других формы ввода данных, значения привязаны к `newBook` свойство модели представления.

Обработчик события submit в форме привязан к `addBook` функции:

[!code-html[Main](part-9/samples/sample4.html)]

`addBook` Функция считывает текущие значения входных параметров формы с привязкой к данным для создания объекта JSON. Затем она отправляет объект JSON для `/api/books`.

> [!div class="step-by-step"]
> [Назад](part-8.md)
> [Вперед](part-10.md)
