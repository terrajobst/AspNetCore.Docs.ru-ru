---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: Запуск модального всплывающего окна из серверного кода (Visual Basic) | Документация Майкрософт
author: wenz
description: Элемент управления ModalPopup в AJAX Control Toolkit предоставляет простой способ создания модального всплывающего окна с помощью средств на стороне клиента. Тем не менее в некоторых сценариях требуется, t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: ea5149e9dece5393bb4c431bfc440a745611496d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362843"
---
<a name="launching-a-modal-popup-window-from-server-code-vb"></a>Запуск модального всплывающего окна из серверного кода (Visual Basic)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)

> Элемент управления ModalPopup в AJAX Control Toolkit предоставляет простой способ создания модального всплывающего окна с помощью средств на стороне клиента. Тем не менее в некоторых сценариях требуется запуск Открытие модального всплывающего окна на стороне сервера.


## <a name="overview"></a>Обзор

Элемент управления ModalPopup в AJAX Control Toolkit предоставляет простой способ создания модального всплывающего окна с помощью средств на стороне клиента. Тем не менее в некоторых сценариях требуется запуск Открытие модального всплывающего окна на стороне сервера.

## <a name="steps"></a>Шаги

Во-первых веб-элемента управления кнопки ASP.NET должен продемонстрировать, как работает элемент управления ModalPopup. Добавьте кнопку в &lt;формы&gt; элемент на новой странице:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

Затем необходимо разметку для контекстного меню, которые вы хотите создать. Определяют его как `<asp:Panel>` управления и убедитесь, что он включает элемент управления Button. Элемент управления ModalPopup предоставляет функциональные возможности для создания такой кнопки закрытия контекстного меню. в противном случае нет простого способа перейдите к нему упразднены.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

Добавьте элемент управления ModalPopup из набора средств AJAX ASP.NET на страницу. Задать свойства для кнопки, который загружает элемент управления, кнопки, что делает его исчезают и идентификатор фактическое всплывающего окна.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

Как и все веб-страницы, на базе ASP.NET AJAX; Диспетчера скриптов, необходимый для загрузки библиотеки JavaScript, необходимые для различных браузеры:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

Запустите пример в браузере. При нажатии кнопки на кнопке, откроется модальное всплывающее окно. Чтобы получить тот же эффект, с помощью кода на стороне сервера, необходима новая кнопка:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

Как вы видите, нажмите кнопку создает обратную передачу и выполняет `ServerButton_Click()` метод на сервере. В этом методе вызывается функция JavaScript `launchModal()` выполняется должна быть точной, функция JavaScript, которая выполняется после загрузки страницы:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

Задача `launchModal()` является отображение ModalPopup. `launchModal()` Функция выполняется после загрузки страницы HTML. В этот момент Однако платформа AJAX для ASP.NET не был полностью загружен еще. Таким образом `launchModal()` функция просто задает переменную, которая впоследствии необходимо отображать элемент управления ModalPopup:

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

`pageLoad()` Функция JavaScript — это специальная функция, которая выполняется после полной загрузки ASP.NET AJAX. Поэтому добавим код этой функции для отображения элемента управления ModalPopup, но только если `launchModal()` был вызван до:

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

`$find()` Функция ищет именованный элемент на странице и ожидает, что в качестве параметра идентификатор на стороне сервера. Таким образом `$find("mpe")` возвращает представление клиентского элемента управления ModalPopup; его `show()` метод позволяет отображается всплывающее окно.


[![Модальное всплывающее окно появляется при нажатии одной из кнопок](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)

Модальное всплывающее окно появляется при нажатии одной из кнопок ([Просмотр полноразмерного изображения](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](positioning-a-modalpopup-cs.md)
> [Вперед](using-modalpopup-with-a-repeater-control-vb.md)
