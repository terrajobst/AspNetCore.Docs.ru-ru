---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: "Добавление нового элемента в базе данных | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: d33355b1bd286513958f71ce5521942a6cbb584f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="add-a-new-item-to-the-database"></a>Добавление нового элемента в базе данных
====================
по [Mike Wasson](https://github.com/MikeWasson)

[Загрузка завершенного проекта](https://github.com/MikeWasson/BookService)

В этом разделе будет добавлена возможность для пользователей создать новую книгу. В файле app.js добавьте следующий код для модели представления:

[!code-javascript[Main](part-9/samples/sample1.js)]

В Index.cshtml Замените следующую разметку:

[!code-html[Main](part-9/samples/sample2.html)]

с:

[!code-html[Main](part-9/samples/sample3.html)]

Эта разметка создает формы для отправки нового автора. Значения для списка автор привязаны к `authors` наблюдаемый в модели представления. Форма входов значения привязаны к `newBook` свойство модели представления.

Обработчик отправки формы привязан к `addBook` функции:

[!code-html[Main](part-9/samples/sample4.html)]

`addBook` Функция считывает текущие значения входных данных формы с привязкой к данным для создания объекта JSON. Затем он отправляет объект JSON для `/api/books`.

>[!div class="step-by-step"]
[Назад](part-8.md)
[Вперед](part-10.md)
