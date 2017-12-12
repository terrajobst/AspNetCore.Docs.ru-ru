---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: "Использование нескольких элементов управления всплывающее окно (Visual Basic) | Документы Microsoft"
author: wenz
description: "Расширитель PopupControl в наборе элементов управления AJAX предлагает простой способ запуска всплывающего окна при активации любого другого элемента управления. Можно также использовать m..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 32e170ebd78a6f849004e789f53c03d9cd40be01
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="using-multiple-popup-controls-vb"></a>Использование нескольких элементов управления всплывающее окно (Visual Basic)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)

> Расширитель PopupControl в наборе элементов управления AJAX предлагает простой способ запуска всплывающего окна при активации любого другого элемента управления. Можно также использовать более одного элемента управления контекстного меню на одной странице.


## <a name="overview"></a>Обзор

Расширитель PopupControl в наборе элементов управления AJAX предлагает простой способ запуска всплывающего окна при активации любого другого элемента управления. Можно также использовать более одного элемента управления контекстного меню на одной странице.

## <a name="steps"></a>Шаги

Чтобы активировать функциональные возможности ASP.NET AJAX и набора средств управления `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемент):

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

Добавьте панель, который служит в качестве контекстного меню. В текущий сценарий содержит панели `Calendar` элемента управления. Во избежание обновления страниц, вызванных обратных передач календаря, панель помещается внутри `UpdatePanel` управления:

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

Данная страница также содержит два текстовых поля. Для каждого текстового поля всплывающего окна календаря должны отображаются при активации в текстовом поле.

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

Теперь Расширьте каждый из двух текстовых полей с `PopupControlExtender`. `TargetControlID` Атрибут содержит идентификатор элемента управления, привязанного к модулю расширения. `PopupControlID` Атрибут содержит идентификатор всплывающей панели. В этом случае оба расширителей Показать той же панели, но различных панелях, возможно, также.

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

Теперь при щелчке в текстовом поле появляется календарь под полем, где можно выбрать дату. (Возвращались в текстовых полях выбранной даты будет рассматриваться в другого учебника.)


[![Календарь появляется, когда пользователь щелкает в текстовое поле](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)

Календарь появляется, когда пользователь щелкает в текстовое поле ([Просмотр полноразмерное изображение](using-multiple-popup-controls-vb/_static/image3.png))

>[!div class="step-by-step"]
[Назад](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
[Вперед](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
