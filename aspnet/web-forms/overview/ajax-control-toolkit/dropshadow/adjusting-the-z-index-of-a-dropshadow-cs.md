---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
title: Настройка Z-Index DropShadow (C#) | Документы Microsoft
author: wenz
description: Элемент управления DropShadow в наборе элементов управления AJAX расширяет панель с тенью. Однако эта теневая иногда конфликтует с другими элементами управления для пап...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 14133833-e518-4347-87b9-6b6f71f14a77
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
msc.type: authoredcontent
ms.openlocfilehash: 82add8427c8e574b213b67315e69bb4c28846095
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="adjusting-the-z-index-of-a-dropshadow-c"></a>Настройка Z-Index DropShadow (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)

> Элемент управления DropShadow в наборе элементов управления AJAX расширяет панель с тенью. Однако эта теневая иногда конфликтует с другими элементами управления, например элемент управления меню ASP.NET. При записи меню появится окно, она находится за тени.


## <a name="overview"></a>Обзор

Элемент управления DropShadow в наборе элементов управления AJAX расширяет панель с тенью. Однако эта теневая иногда конфликтует с другими элементами управления, например элемент управления меню ASP.NET. При записи меню появится окно, она находится за тени.

## <a name="steps"></a>Шаги

Код начинается с элемента управления, содержащий достаточный фрагмент текста, чтобы панель содержит достаточный фрагмент текста для эффекта должен отображаться:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample1.aspx)]

Другой панели помещается непосредственно перед `panelShadow` панели. Он содержит меню в горизонтальном направлении, чтобы пункты меню отображаются над (или вместо: в списке) `dropShadow` панель):

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample2.aspx)]

Затем `DropShadowExtender` добавляется расширение `panelShadow` панель с тенью:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample3.aspx)]

Наконец, ASP.NET AJAX `ScriptManager` управления включает набор средств управления для работы:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample4.aspx)]

При выполнении этого скрипта, пункты меню отображаются под панели. Однако меню использует класс CSS `panel` где просто нужно определить две вещи, чтобы сделать элементы отображаться перед другим панели:

- Относительное расположение
- Положительное значение z индекса

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample5.css)]

Затем `DropShadowExtender` управления больше не конфликтуют с меню элемента управления.


[![Ранее: Элемент меню не отображается](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)

Ранее: Элемент меню не отображается ([Просмотр полноразмерное изображение](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))


[![После: Запись меню появится](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)

После: Запись меню появится ([Просмотр полноразмерное изображение](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Вперед](manipulating-dropshadow-properties-from-client-code-cs.md)
