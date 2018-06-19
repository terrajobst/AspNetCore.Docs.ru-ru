---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
title: Использование нескольких элементов управления всплывающее окно (C#) | Документы Microsoft
author: wenz
description: Расширитель PopupControl в наборе элементов управления AJAX предлагает простой способ запуска всплывающего окна при активации любого другого элемента управления. Можно также использовать m...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 91511b0b-311d-481f-9e7c-73f07b813b79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 7acd1b53e1b3e3e0d09d248b68941b166da3e81e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869690"
---
<a name="using-multiple-popup-controls-c"></a>Использование нескольких элементов управления всплывающее окно (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)

> Расширитель PopupControl в наборе элементов управления AJAX предлагает простой способ запуска всплывающего окна при активации любого другого элемента управления. Можно также использовать более одного элемента управления контекстного меню на одной странице.


## <a name="overview"></a>Обзор

Расширитель PopupControl в наборе элементов управления AJAX предлагает простой способ запуска всплывающего окна при активации любого другого элемента управления. Можно также использовать более одного элемента управления контекстного меню на одной странице.

## <a name="steps"></a>Шаги

Чтобы активировать функциональные возможности ASP.NET AJAX и набора средств управления `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемент):

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample1.aspx)]

Добавьте панель, который служит в качестве контекстного меню. В текущий сценарий содержит панели `Calendar` элемента управления. Во избежание обновления страниц, вызванных обратных передач календаря, панель помещается внутри `UpdatePanel` управления:

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample2.aspx)]

Данная страница также содержит два текстовых поля. Для каждого текстового поля всплывающего окна календаря должны отображаются при активации в текстовом поле.

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample3.aspx)]

Теперь Расширьте каждый из двух текстовых полей с `PopupControlExtender`. `TargetControlID` Атрибут содержит идентификатор элемента управления, привязанного к модулю расширения. `PopupControlID` Атрибут содержит идентификатор всплывающей панели. В этом случае оба расширителей Показать той же панели, но различных панелях, возможно, также.

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample4.aspx)]

Теперь при щелчке в текстовом поле появляется календарь под полем, где можно выбрать дату. (Возвращались в текстовых полях выбранной даты будет рассматриваться в другого учебника.)


[![Календарь появляется, когда пользователь щелкает в текстовое поле](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)

Календарь появляется, когда пользователь щелкает в текстовое поле ([Просмотр полноразмерное изображение](using-multiple-popup-controls-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Вперед](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
