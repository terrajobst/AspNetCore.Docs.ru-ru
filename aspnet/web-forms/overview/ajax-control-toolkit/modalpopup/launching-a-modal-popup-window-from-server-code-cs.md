---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
title: Запуск модального всплывающего окна из серверного кода (C#) | Документация Майкрософт
author: wenz
description: Элемент управления ModalPopup в AJAX Control Toolkit предоставляет простой способ создания модального всплывающего окна с помощью средств на стороне клиента. Тем не менее в некоторых сценариях требуется, t...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 2f67d8ef-73ca-447d-a0cc-6e3168431e6a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
msc.type: authoredcontent
ms.openlocfilehash: b7b486c4b99e5ddcb9bc244a9c5dcf193d33b696
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815308"
---
<a name="launching-a-modal-popup-window-from-server-code-c"></a>Запуск модального всплывающего окна из серверного кода (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)

> Элемент управления ModalPopup в AJAX Control Toolkit предоставляет простой способ создания модального всплывающего окна с помощью средств на стороне клиента. Тем не менее в некоторых сценариях требуется запуск Открытие модального всплывающего окна на стороне сервера.


## <a name="overview"></a>Обзор

Элемент управления ModalPopup в AJAX Control Toolkit предоставляет простой способ создания модального всплывающего окна с помощью средств на стороне клиента. Тем не менее в некоторых сценариях требуется запуск Открытие модального всплывающего окна на стороне сервера.

## <a name="steps"></a>Шаги

Во-первых веб-элемента управления кнопки ASP.NET должен продемонстрировать, как работает элемент управления ModalPopup. Добавьте кнопку в &lt;формы&gt; элемент на новой странице:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample1.aspx)]

Затем необходимо разметку для контекстного меню, которые вы хотите создать. Определяют его как `<asp:Panel>` управления и убедитесь, что он включает элемент управления Button. Элемент управления ModalPopup предоставляет функциональные возможности для создания такой кнопки закрытия контекстного меню. в противном случае нет простого способа перейдите к нему упразднены.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample2.aspx)]

Добавьте элемент управления ModalPopup из набора средств AJAX ASP.NET на страницу. Задать свойства для кнопки, который загружает элемент управления, кнопки, что делает его исчезают и идентификатор фактическое всплывающего окна.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample3.aspx)]

Как и все веб-страницы, на базе ASP.NET AJAX; Диспетчера скриптов, необходимый для загрузки библиотеки JavaScript, необходимые для различных браузеры:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample4.aspx)]

Запустите пример в браузере. При нажатии кнопки на кнопке, откроется модальное всплывающее окно. Чтобы получить тот же эффект, с помощью кода на стороне сервера, необходима новая кнопка:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample5.aspx)]

Как вы видите, нажмите кнопку создает обратную передачу и выполняет `ServerButton_Click()` метод на сервере. В этом методе вызывается функция JavaScript `launchModal()` выполняется должна быть точной, функция JavaScript, которая выполняется после загрузки страницы:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample6.aspx)]

Задача `launchModal()` является отображение ModalPopup. `launchModal()` Функция выполняется после загрузки страницы HTML. В этот момент Однако платформа AJAX для ASP.NET не был полностью загружен еще. Таким образом `launchModal()` функция просто задает переменную, которая впоследствии необходимо отображать элемент управления ModalPopup:

[!code-html[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample7.html)]

`pageLoad()` Функция JavaScript — это специальная функция, которая выполняется после полной загрузки ASP.NET AJAX. Поэтому добавим код этой функции для отображения элемента управления ModalPopup, но только если `launchModal()` был вызван до:

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample8.js)]

`$find()` Функция ищет именованный элемент на странице и ожидает, что в качестве параметра идентификатор на стороне сервера. Таким образом `$find("mpe")` возвращает представление клиентского элемента управления ModalPopup; его `show()` метод позволяет отображается всплывающее окно.


[![Модальное всплывающее окно появляется при нажатии одной из кнопок](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)

Модальное всплывающее окно появляется при нажатии одной из кнопок ([Просмотр полноразмерного изображения](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Вперед](using-modalpopup-with-a-repeater-control-cs.md)
