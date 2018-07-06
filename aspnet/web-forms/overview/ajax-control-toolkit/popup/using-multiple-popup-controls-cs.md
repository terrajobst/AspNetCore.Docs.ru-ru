---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
title: Использование нескольких элементов управления Popup (C#) | Документация Майкрософт
author: wenz
description: Расширитель PopupControl в AJAX Control Toolkit предлагает простой способ активации всплывающего окна при активации любого другого элемента управления. Можно также использовать m...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 91511b0b-311d-481f-9e7c-73f07b813b79
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 681ee1433130ef1cf6d1521497001215a1bfbb66
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814744"
---
<a name="using-multiple-popup-controls-c"></a>Использование нескольких элементов управления Popup (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)

> Расширитель PopupControl в AJAX Control Toolkit предлагает простой способ активации всплывающего окна при активации любого другого элемента управления. Можно также использовать более одного элемента управления всплывающего окна на одной странице.


## <a name="overview"></a>Обзор

Расширитель PopupControl в AJAX Control Toolkit предлагает простой способ активации всплывающего окна при активации любого другого элемента управления. Можно также использовать более одного элемента управления всплывающего окна на одной странице.

## <a name="steps"></a>Шаги

Для активации функции ASP.NET AJAX и Control Toolkit, `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемента):

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample1.aspx)]

Добавьте панель, который служит в качестве всплывающего окна. В данном способе панель содержит `Calendar` элемента управления. Во избежание обновления страниц, вызванных календаря обратных передач, панели помещается в `UpdatePanel` управления:

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample2.aspx)]

Страница также содержит два текстовых поля. Для каждого текстового поля всплывающего окна календаря появляться после активации текстового поля.

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample3.aspx)]

Теперь Расширьте каждый из двух текстовых полях с `PopupControlExtender`. `TargetControlID` Атрибут содержит идентификатор элемента управления, привязанных к расширителя. `PopupControlID` Атрибут содержит идентификатор всплывающей панели. В этом случае оба расширителей Показать той же панели, но различных панелях, возможно, также.

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample4.aspx)]

Теперь при каждом нажатии в текстовом поле появляется календарь под полем, где можно выбрать дату. (Получение выбранную дату в текстовые поля будут освещены в другого учебника.)


[![Календарь появляется, когда пользователь нажимает кнопку в текстовое поле](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)

Календарь появляется, когда пользователь нажимает кнопку в текстовое поле ([Просмотр полноразмерного изображения](using-multiple-popup-controls-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Вперед](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
