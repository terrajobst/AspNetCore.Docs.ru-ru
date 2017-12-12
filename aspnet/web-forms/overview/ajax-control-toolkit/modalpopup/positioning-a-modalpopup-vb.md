---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
title: "Позиционирование ModalPopup (VB) | Документы Microsoft"
author: wenz
description: "Элемент управления ModalPopup в наборе элементов управления AJAX предлагает простой способ создания модальное окно, с помощью клиентских средств. Однако элемент управления не предлагает..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 8a07210c-eb0e-485e-9ee8-82a101520e65
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 4b9c0790d1696bcf478bcdea089d4d3d92450369
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="positioning-a-modalpopup-vb"></a>Позиционирование ModalPopup (Visual Basic)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)

> Элемент управления ModalPopup в наборе элементов управления AJAX предлагает простой способ создания модальное окно, с помощью клиентских средств. Однако элемент управления не предоставляет встроенные функциональные возможности для размещения всплывающего окна.


## <a name="overview"></a>Обзор

Элемент управления ModalPopup в наборе элементов управления AJAX предлагает простой способ создания модальное окно, с помощью клиентских средств. Однако элемент управления не предоставляет встроенные функциональные возможности для размещения всплывающего окна.

## <a name="steps"></a>Шаги

Чтобы активировать функциональные возможности ASP.NET AJAX и набора средств управления `ScriptManager`. элемент управления необходимо поместить в любом месте на странице (но в `<form>` элемент):

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample1.aspx)]

Добавьте панель, который служит в качестве модальное окно. Кнопки используется, чтобы закрыть всплывающее окно:

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample2.aspx)]

Каждый раз, когда отображается всплывающее окно, его должны размещаться в определенное место на странице. Эта задача создается клиентские функции JavaScript. Сначала пытается получить доступ к панели. В случае успеха положение панели устанавливается с помощью CSS и JavaScript (изменение будет положение во всплывающем окне). Однако `ModalPopupExtender` управления также пытается положение всплывающего окна. Таким образом код JavaScript многократно всплывающее окно, каждые десять раз меньше секунды.

[!code-html[Main](positioning-a-modalpopup-vb/samples/sample3.html)]

Как видите, возвращаемое значение `setTimeout()` метода JavaScript сохраняется в глобальной переменной. Это позволяет остановить повторное размещение всплывающего окна по требованию, с помощью `clearTimeout()` метод:

[!code-javascript[Main](positioning-a-modalpopup-vb/samples/sample4.js)]

Теперь осталось сделать — сделать вызывать эти функции каждый раз, когда соответствующие браузер. `movePanel()` Функцию JavaScript должен вызываться при нажатии кнопки, которое запускает панели:

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample5.aspx)]

И `stopMoving()` функции вступает в действие, когда всплывающее окно закрывается, это может быть предприняты в `ModalPopupExtender` управления:

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample6.aspx)]


[![Модальное окно отображается в указанной позиции](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)

Модальное окно отображается в указанной позиции ([Просмотр полноразмерное изображение](positioning-a-modalpopup-vb/_static/image3.png))

>[!div class="step-by-step"]
[Назад](handling-postbacks-from-a-modalpopup-vb.md)
