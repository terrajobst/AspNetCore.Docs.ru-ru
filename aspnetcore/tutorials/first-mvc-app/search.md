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

<span data-ttu-id="f84a8-103">Параметр `searchString` можно быстро переименовать в `id` с помощью команды **rename**.</span><span class="sxs-lookup"><span data-stu-id="f84a8-103">You can quickly rename the `searchString` parameter to `id` with the **rename** command.</span></span> <span data-ttu-id="f84a8-104">Щелкните правой кнопкой мыши элемент `searchString` **> Rename**.</span><span class="sxs-lookup"><span data-stu-id="f84a8-104">Right click on `searchString` **> Rename**.</span></span>

![Контекстное меню](search/_static/rename.png)

<span data-ttu-id="f84a8-106">Выделены целевые объекты для команды переименования.</span><span class="sxs-lookup"><span data-stu-id="f84a8-106">The rename targets are highlighted.</span></span>

![Редактор кода с выделенной переменной в методе Index ActionResult](search/_static/rename2.png)

<span data-ttu-id="f84a8-108">Измените параметр на `id`, а все вхождения `searchString` — на `id`.</span><span class="sxs-lookup"><span data-stu-id="f84a8-108">Change the parameter to `id` and all occurrences of `searchString` change to `id`.</span></span>

![Редактор кода, в котором переменная изменена на идентификатор](search/_static/rename3.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search2.md)]

<span data-ttu-id="f84a8-110">Обратите внимание на то, как IntelliSense автоматически обновляет разметку.</span><span class="sxs-lookup"><span data-stu-id="f84a8-110">Notice how intelliSense helps us update the markup.</span></span>

![Контекстное меню IntelliSense с методом, выбранным в списке атрибутов для элемента form](search/_static/int_m.png)

![Контекстное меню IntelliSense с методом get, выбранным в списке значений атрибутов метода](search/_static/int_get.png)

<span data-ttu-id="f84a8-113">Обратите внимание на другой шрифт тега `<form>`.</span><span class="sxs-lookup"><span data-stu-id="f84a8-113">Notice the distinctive font in the `<form>` tag.</span></span> <span data-ttu-id="f84a8-114">Такое выделение свидетельствует о том, что тег поддерживается [вспомогательными функциями тегов](~/mvc/views/tag-helpers/intro.md).</span><span class="sxs-lookup"><span data-stu-id="f84a8-114">That distinctive font indicates the tag is supported by [Tag Helpers](~/mvc/views/tag-helpers/intro.md).</span></span>

![Тег form, выделенный фиолетовым цветом](search/_static/th_font.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="f84a8-116">[Назад](controller-methods-views.md)
> [Вперед](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="f84a8-116">[Previous](controller-methods-views.md)
[Next](new-field.md)</span></span>  
