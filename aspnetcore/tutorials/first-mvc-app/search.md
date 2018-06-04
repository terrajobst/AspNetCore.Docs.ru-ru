---
title: Добавление поиска
author: rick-anderson
description: Инструкции по добавлению поиска в простое приложение ASP.NET Core MVC
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: aee1682755385d9fa292f9ba0814d5d3602f3881
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729912"
---
[!INCLUDE [adding-model](~/includes/mvc-intro/search1.md)]

<span data-ttu-id="7e717-103">Параметр `searchString` можно быстро переименовать в `id` с помощью команды **rename**.</span><span class="sxs-lookup"><span data-stu-id="7e717-103">You can quickly rename the `searchString` parameter to `id` with the **rename** command.</span></span> <span data-ttu-id="7e717-104">Щелкните правой кнопкой мыши элемент `searchString` **> Rename**.</span><span class="sxs-lookup"><span data-stu-id="7e717-104">Right click on `searchString` **> Rename**.</span></span>

![Контекстное меню](search/_static/rename.png)

<span data-ttu-id="7e717-106">Выделены целевые объекты для команды переименования.</span><span class="sxs-lookup"><span data-stu-id="7e717-106">The rename targets are highlighted.</span></span>

![Редактор кода с выделенной переменной в методе Index ActionResult](search/_static/rename2.png)

<span data-ttu-id="7e717-108">Измените параметр на `id`, а все вхождения `searchString` — на `id`.</span><span class="sxs-lookup"><span data-stu-id="7e717-108">Change the parameter to `id` and all occurrences of `searchString` change to `id`.</span></span>

![Редактор кода, в котором переменная изменена на идентификатор](search/_static/rename3.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search2.md)]

<span data-ttu-id="7e717-110">Обратите внимание на то, как IntelliSense автоматически обновляет разметку.</span><span class="sxs-lookup"><span data-stu-id="7e717-110">Notice how intelliSense helps us update the markup.</span></span>

![Контекстное меню IntelliSense с методом, выбранным в списке атрибутов для элемента form](search/_static/int_m.png)

![Контекстное меню IntelliSense с методом get, выбранным в списке значений атрибутов метода](search/_static/int_get.png)

<span data-ttu-id="7e717-113">Обратите внимание на другой шрифт тега `<form>`.</span><span class="sxs-lookup"><span data-stu-id="7e717-113">Notice the distinctive font in the `<form>` tag.</span></span> <span data-ttu-id="7e717-114">Такое выделение свидетельствует о том, что тег поддерживается [вспомогательными функциями тегов](~/mvc/views/tag-helpers/intro.md).</span><span class="sxs-lookup"><span data-stu-id="7e717-114">That distinctive font indicates the tag is supported by [Tag Helpers](~/mvc/views/tag-helpers/intro.md).</span></span>

![Тег form, выделенный фиолетовым цветом](search/_static/th_font.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="7e717-116">[Назад](controller-methods-views.md)
> [Вперед](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="7e717-116">[Previous](controller-methods-views.md)
[Next](new-field.md)</span></span>  
