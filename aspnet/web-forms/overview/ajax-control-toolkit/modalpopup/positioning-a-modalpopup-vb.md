---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
title: Размещение ModalPopup (VB) | Документация Майкрософт
author: wenz
description: Элемент управления ModalPopup в AJAX Control Toolkit предоставляет простой способ создания модального всплывающего окна с помощью средств на стороне клиента. Тем не менее не предлагает элемент управления...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 8a07210c-eb0e-485e-9ee8-82a101520e65
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: a507cb173b6dba96ca532a249fa9d495ded7d4e8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837662"
---
<a name="positioning-a-modalpopup-vb"></a>Размещение ModalPopup (VB)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)

> Элемент управления ModalPopup в AJAX Control Toolkit предоставляет простой способ создания модального всплывающего окна с помощью средств на стороне клиента. Однако элемент управления не предоставляет встроенные функциональные возможности для размещения всплывающего окна.


## <a name="overview"></a>Обзор

Элемент управления ModalPopup в AJAX Control Toolkit предоставляет простой способ создания модального всплывающего окна с помощью средств на стороне клиента. Однако элемент управления не предоставляет встроенные функциональные возможности для размещения всплывающего окна.

## <a name="steps"></a>Шаги

Для активации функции ASP.NET AJAX и Control Toolkit, `ScriptManager`. элемент управления необходимо поместить в любом месте на странице (но в `<form>` элемента):

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample1.aspx)]

Добавьте панель, который служит в качестве модального всплывающего окна. Кнопка используется для закрытия всплывающего окна:

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample2.aspx)]

Каждый раз, когда всплывающее окно отображается, он должен размещаться в определенное место на странице. Для выполнения этой задачи будет создана функция JavaScript на стороне клиента. Сначала выполняется попытка доступа к панели. Выполняется успешно, положение панели задается с помощью CSS и JavaScript (изменение будет положение всплывающего окна в). Тем не менее `ModalPopupExtender` управления также пытается положение всплывающего окна. Таким образом код JavaScript многократно всплывающее окно, каждую десятую долю секунды.

[!code-html[Main](positioning-a-modalpopup-vb/samples/sample3.html)]

Как вы видите, возвращаемое значение `setTimeout()` метод JavaScript сохраняется в глобальной переменной. Это позволяет остановить повторное размещение всплывающего окна по требованию, с помощью `clearTimeout()` метод:

[!code-javascript[Main](positioning-a-modalpopup-vb/samples/sample4.js)]

Теперь осталось сделать лишь для вызова этих функций всякий раз, когда соответствующие браузера. `movePanel()` Функция JavaScript должен вызываться при нажатии кнопки, который запускает панели:

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample5.aspx)]

И `stopMoving()` функции вступает в действие, когда всплывающее окно закрывается, это может быть предприняты в `ModalPopupExtender` управления:

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample6.aspx)]


[![Появится модальное всплывающее окно в указанной позиции](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)

Появится модальное всплывающее окно в указанной позиции ([Просмотр полноразмерного изображения](positioning-a-modalpopup-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](handling-postbacks-from-a-modalpopup-vb.md)
