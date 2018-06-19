---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
title: Обработка обратные передачи из ModalPopup (VB) | Документы Microsoft
author: wenz
description: Элемент управления ModalPopup в наборе элементов управления AJAX предлагает простой способ создания модальное окно, с помощью клиентских средств. Специальные необходимо соблюдать осторожность при pos...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f70ac2b3-900f-40fa-858f-ab057904506b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 1aa2c42deb67015dd0b35edf4ba72d8d667ec88c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874215"
---
<a name="handling-postbacks-from-a-modalpopup-vb"></a>Обработка обратные передачи из ModalPopup (Visual Basic)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)

> Элемент управления ModalPopup в наборе элементов управления AJAX предлагает простой способ создания модальное окно, с помощью клиентских средств. Необходимо уделить особое, во время обратной передачи из в контекстное меню.


## <a name="overview"></a>Обзор

Элемент управления ModalPopup в наборе элементов управления AJAX предлагает простой способ создания модальное окно, с помощью клиентских средств. Необходимо уделить особое, во время обратной передачи из в контекстное меню.

## <a name="steps"></a>Шаги

Чтобы активировать функциональные возможности ASP.NET AJAX и набора средств управления `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемент):

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample1.aspx)]

Добавьте панель, который служит в качестве модальное окно. Нет пользователь может ввести имя и адрес электронной почты. Чтобы закрыть всплывающее окно и сохраните используется кнопка. Обратите внимание, что `OnClick` атрибут имеет значение, чтобы обратной передачи возникает при нажатии этой кнопки:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample2.aspx)]

Сама страница состоит из двух меток для точно те же данные: имя и адрес электронной почты. Кнопки используется для запуска модальное окно:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample3.aspx)]

Для упрощения всплывающее окно отображается, добавьте `ModalPopupExtender` элемента управления. Задать `PopupControlID` атрибут ID панели и `TargetControlID` кнопки с идентификатором:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample4.aspx)]

Теперь каждый раз, когда `Save` в модальное окно кнопки, сервере `SaveData()` выполнения метода. Нет может сохранить введенные данные в хранилище данных. Для простоты новые данные просто вывести в метке:

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample5.vb)]

Кроме того элементы управления textbox в модальное окно должен быть заполнен текущего имени и по электронной почте. Однако это требуется только при возникновении без обратной передачи. Если обратную передачу, функция viewstate ASP.NET автоматически вводится текстовых полей с соответствующими значениями.

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample6.vb)]


[![Модальное окно вызывает обратную передачу](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)

Модальное окно вызывает обратную передачу ([Просмотр полноразмерное изображение](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](using-modalpopup-with-a-repeater-control-vb.md)
> [Вперед](positioning-a-modalpopup-vb.md)
