---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
title: Обработка обратных передач из ModalPopup (VB) | Документация Майкрософт
author: wenz
description: Элемент управления ModalPopup в AJAX Control Toolkit предоставляет простой способ создания модального всплывающего окна с помощью средств на стороне клиента. Особое внимание следует принимать при терминалом...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f70ac2b3-900f-40fa-858f-ab057904506b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 3f81a2bf1866d026046fdba815fdbae1b162c1dd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837023"
---
<a name="handling-postbacks-from-a-modalpopup-vb"></a>Обработка обратных передач из ModalPopup (VB)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)

> Элемент управления ModalPopup в AJAX Control Toolkit предоставляет простой способ создания модального всплывающего окна с помощью средств на стороне клиента. Специальные необходимо соблюдать осторожность при обратной передачи из в контекстное меню.


## <a name="overview"></a>Обзор

Элемент управления ModalPopup в AJAX Control Toolkit предоставляет простой способ создания модального всплывающего окна с помощью средств на стороне клиента. Специальные необходимо соблюдать осторожность при обратной передачи из в контекстное меню.

## <a name="steps"></a>Шаги

Для активации функции ASP.NET AJAX и Control Toolkit, `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемента):

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample1.aspx)]

Добавьте панель, который служит в качестве модального всплывающего окна. Существует пользователь может ввести имя и адрес электронной почты. Кнопка позволяет закрыть всплывающее окно и сохранить их. Обратите внимание, что `OnClick` атрибут имеет значение, что обратная передача происходит при нажатии этой кнопки:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample2.aspx)]

Сама страница состоит из двух меток для точно те же данные: имя и адрес электронной почты. Кнопка используется для запуска модального всплывающего окна:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample3.aspx)]

Чтобы сделать всплывающее окно отображается, добавьте `ModalPopupExtender` элемента управления. Задайте `PopupControlID` атрибут ID панели и `TargetControlID` идентификатору кнопки:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample4.aspx)]

Теперь всякий раз, когда `Save` нажатии кнопки внутри модального всплывающего окна на стороне сервера `SaveData()` выполнения метода. Здесь вы можете сохранить введенные данные в хранилище данных. Для простоты новые данные, просто вывести в метке:

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample5.vb)]

Кроме того элементы управления textbox внутри модального всплывающего окна должен быть заполнен действующие имя и адрес электронной почты. Тем не менее это требуется только при отсутствии обратной передачи. Если обратная передача, функция viewstate ASP.NET автоматически заполнят текстовых полей с соответствующими значениями.

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample6.vb)]


[![Модальное всплывающее окно вызывает обратную передачу](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)

Модальное всплывающее окно вызывает обратную передачу ([Просмотр полноразмерного изображения](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](using-modalpopup-with-a-repeater-control-vb.md)
> [Вперед](positioning-a-modalpopup-vb.md)
