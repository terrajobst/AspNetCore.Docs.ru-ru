---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
title: Настройка Z-Index DropShadow (VB) | Документы Microsoft
author: wenz
description: Элемент управления DropShadow в наборе элементов управления AJAX расширяет панель с тенью. Однако эта теневая иногда конфликтует с другими элементами управления для пап...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ecb004b5-82c0-44fb-bcaf-233fffac6195
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
msc.type: authoredcontent
ms.openlocfilehash: b484dc6bfa6f67bd6b70f7c36c2eb2ec7143edaf
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="adjusting-the-z-index-of-a-dropshadow-vb"></a>Настройка Z-Index DropShadow (Visual Basic)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)

> Элемент управления DropShadow в наборе элементов управления AJAX расширяет панель с тенью. Однако эта теневая иногда конфликтует с другими элементами управления, например элемент управления меню ASP.NET. При записи меню появится окно, она находится за тени.


## <a name="overview"></a>Обзор

Элемент управления DropShadow в наборе элементов управления AJAX расширяет панель с тенью. Однако эта теневая иногда конфликтует с другими элементами управления, например элемент управления меню ASP.NET. При записи меню появится окно, она находится за тени.

## <a name="steps"></a>Шаги

Код начинается с элемента управления, содержащий достаточный фрагмент текста, чтобы панель содержит достаточный фрагмент текста для эффекта должен отображаться:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample1.aspx)]

Другой панели помещается непосредственно перед `panelShadow` панели. Он содержит меню в горизонтальном направлении, чтобы пункты меню отображаются над (или вместо: в списке) `dropShadow` панель):

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample2.aspx)]

Затем `DropShadowExtender` добавляется расширение `panelShadow` панель с тенью:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample3.aspx)]

Наконец, ASP.NET AJAX `ScriptManager` управления включает набор средств управления для работы:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample4.aspx)]

При выполнении этого скрипта, пункты меню отображаются под панели. Однако меню использует класс CSS `panel` где просто нужно определить две вещи, чтобы сделать элементы отображаться перед другим панели:

- Относительное расположение
- Положительное значение z индекса

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample5.css)]

Затем `DropShadowExtender` управления больше не конфликтуют с меню элемента управления.


[![Ранее: Элемент меню не отображается](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)

Ранее: Элемент меню не отображается ([Просмотр полноразмерное изображение](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))


[![После: Запись меню появится](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)

После: Запись меню появится ([Просмотр полноразмерное изображение](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Назад](manipulating-dropshadow-properties-from-client-code-cs.md)
> [Вперед](manipulating-dropshadow-properties-from-client-code-vb.md)
