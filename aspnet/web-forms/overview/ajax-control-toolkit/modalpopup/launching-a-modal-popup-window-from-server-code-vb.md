---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: "Открыв окно модальное окно из серверного кода (VB) | Документы Microsoft"
author: wenz
description: "Элемент управления ModalPopup в наборе элементов управления AJAX предлагает простой способ создания модальное окно, с помощью клиентских средств. Однако в некоторых сценариях требуется, t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: c4bcf3e32b3aa91bb73e01296bc1fc1a2e064711
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="launching-a-modal-popup-window-from-server-code-vb"></a>Открыв окно модальное окно из серверного кода (Visual Basic)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)

> Элемент управления ModalPopup в наборе элементов управления AJAX предлагает простой способ создания модальное окно, с помощью клиентских средств. Однако в некоторых сценариях требуется запуск открывающий модальное окно на стороне сервера.


## <a name="overview"></a>Обзор

Элемент управления ModalPopup в наборе элементов управления AJAX предлагает простой способ создания модальное окно, с помощью клиентских средств. Однако в некоторых сценариях требуется запуск открывающий модальное окно на стороне сервера.

## <a name="steps"></a>Шаги

Во-первых веб-элемента управления кнопки ASP.NET требуется продемонстрировать, как работает элемент управления ModalPopup. Добавление кнопки в &lt;формы&gt; элемент на новую страницу:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

Затем необходимо разметку для контекстного меню, который требуется создать. Определить ее как `<asp:Panel>` управления и убедитесь, что он включает элемент управления Button. Элемент управления ModalPopup предоставляет функциональные возможности для создания таких кнопки Закрыть всплывающее окно; в противном случае нет никакого простого способа запускать его удалить.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

Затем добавьте элемент управления ModalPopup из набора средств ASP.NET AJAX на страницу. Задание свойств для кнопку, которая загружается элемент управления, кнопки, что делает его исчезают и идентификатор фактическое всплывающего окна.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

Как и все веб-страницы, на базе ASP.NET AJAX; Диспетчера скриптов, необходимой для загрузки необходимые библиотеки JavaScript для различных целевых браузеров:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

В браузере Запустите пример. При нажатии на кнопке отображается модальное окно. Для достижения такого же эффекта, с помощью кода на стороне сервера, требуется новая кнопка:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

Как видно, нажмите кнопку создает обратную передачу и выполняет `ServerButton_Click()` метод на сервере. Этот метод вызывается функция JavaScript `launchModal()` выполняется быть точным, функция JavaScript, которая выполняется после загрузки страницы:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

Задача `launchModal()` — отображение ModalPopup. `launchModal()` Функция выполняется после загрузки страницы HTML. В этот момент тем не менее, платформа AJAX для ASP.NET не был полностью загружен еще. Таким образом `launchModal()` функция просто задает переменную, которая управления ModalPopup должно быть показано ниже на:

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

`pageLoad()` Функцию JavaScript — это специальная функция, который исполняется после полной загрузки ASP.NET AJAX. Поэтому мы добавьте код для этой функции для отображения элемента управления ModalPopup, но только если `launchModal()` был вызван до:

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

`$find()` Функция ищет именованного элемента на странице и ожидает в качестве параметра идентификатор серверные. Таким образом `$find("mpe")` возвращает представление клиента управления ModalPopup; его `show()` метод позволяет отображается всплывающее окно.


[![Модальное окно появляется при нажатии любой кнопки](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)

Модальное окно появляется при нажатии любой кнопки ([Просмотр полноразмерное изображение](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))

>[!div class="step-by-step"]
[Назад](positioning-a-modalpopup-cs.md)
[Вперед](using-modalpopup-with-a-repeater-control-vb.md)
