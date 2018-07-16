---
title: Добавление поиска в приложение MVC ASP.NET Core
author: rick-anderson
description: Инструкции по добавлению поиска в простое приложение ASP.NET Core MVC
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: d0581142b505323385feba441b707d29b3f6db5d
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2018
ms.locfileid: "38216200"
---
[!INCLUDE [adding-model](~/includes/mvc-intro/search1.md)]

Параметр `searchString` можно быстро переименовать в `id` с помощью команды **rename**. Щелкните правой кнопкой мыши элемент `searchString` **> Rename**.

![Контекстное меню](search/_static/rename.png)

Выделены целевые объекты для команды переименования.

![Редактор кода с выделенной переменной в методе Index ActionResult](search/_static/rename2.png)

Измените параметр на `id`, а все вхождения `searchString` — на `id`.

![Редактор кода, в котором переменная изменена на идентификатор](search/_static/rename3.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search2.md)]

Обратите внимание на то, как IntelliSense автоматически обновляет разметку.

![Контекстное меню IntelliSense с методом, выбранным в списке атрибутов для элемента form](search/_static/int_m.png)

![Контекстное меню IntelliSense с методом get, выбранным в списке значений атрибутов метода](search/_static/int_get.png)

Обратите внимание на другой шрифт тега `<form>`. Такое выделение свидетельствует о том, что тег поддерживается [вспомогательными функциями тегов](~/mvc/views/tag-helpers/intro.md).

![Тег form, выделенный фиолетовым цветом](search/_static/th_font.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> [Назад](controller-methods-views.md)
> [Вперед](new-field.md)  
