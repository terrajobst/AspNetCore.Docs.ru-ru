---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
title: Позиционирование ModalPopup (C#) | Документы Microsoft
author: wenz
description: Элемент управления ModalPopup в наборе элементов управления AJAX предлагает простой способ создания модальное окно, с помощью клиентских средств. Однако элемент управления не предлагает...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1caac9d0-e21e-49d6-a8ff-e563a736d6ca
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: bee5be84259231d8cd5efde74b610d72f5e250cc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874152"
---
<a name="positioning-a-modalpopup-c"></a>Позиционирование ModalPopup (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)

> Элемент управления ModalPopup в наборе элементов управления AJAX предлагает простой способ создания модальное окно, с помощью клиентских средств. Однако элемент управления не предоставляет встроенные функциональные возможности для размещения всплывающего окна.


## <a name="overview"></a>Обзор

Элемент управления ModalPopup в наборе элементов управления AJAX предлагает простой способ создания модальное окно, с помощью клиентских средств. Однако элемент управления не предоставляет встроенные функциональные возможности для размещения всплывающего окна.

## <a name="steps"></a>Шаги

Чтобы активировать функциональные возможности ASP.NET AJAX и набора средств управления `ScriptManager`. элемент управления необходимо поместить в любом месте на странице (но в `<form>` элемент):

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample1.aspx)]

Добавьте панель, который служит в качестве модальное окно. Кнопки используется, чтобы закрыть всплывающее окно:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample2.aspx)]

Каждый раз, когда отображается всплывающее окно, его должны размещаться в определенное место на странице. Эта задача создается клиентские функции JavaScript. Сначала пытается получить доступ к панели. В случае успеха положение панели устанавливается с помощью CSS и JavaScript (изменение будет положение во всплывающем окне). Однако `ModalPopupExtender` управления также пытается положение всплывающего окна. Таким образом код JavaScript многократно всплывающее окно, каждые десять раз меньше секунды.

[!code-html[Main](positioning-a-modalpopup-cs/samples/sample3.html)]

Как видите, возвращаемое значение `setTimeout()` метода JavaScript сохраняется в глобальной переменной. Это позволяет остановить повторное размещение всплывающего окна по требованию, с помощью `clearTimeout()` метод:

[!code-javascript[Main](positioning-a-modalpopup-cs/samples/sample4.js)]

Теперь осталось сделать — сделать вызывать эти функции каждый раз, когда соответствующие браузер. `movePanel()` Функцию JavaScript должен вызываться при нажатии кнопки, которое запускает панели:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample5.aspx)]

И `stopMoving()` функции вступает в действие, когда всплывающее окно закрывается, это может быть предприняты в `ModalPopupExtender` управления:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample6.aspx)]


[![Модальное окно отображается в указанной позиции](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)

Модальное окно отображается в указанной позиции ([Просмотр полноразмерное изображение](positioning-a-modalpopup-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](handling-postbacks-from-a-modalpopup-cs.md)
> [Вперед](launching-a-modal-popup-window-from-server-code-vb.md)
