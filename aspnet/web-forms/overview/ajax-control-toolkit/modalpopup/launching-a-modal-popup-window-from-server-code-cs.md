---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
title: Открыв окно модальное окно из серверного кода (C#) | Документы Microsoft
author: wenz
description: Элемент управления ModalPopup в наборе элементов управления AJAX предлагает простой способ создания модальное окно, с помощью клиентских средств. Однако в некоторых сценариях требуется, t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2f67d8ef-73ca-447d-a0cc-6e3168431e6a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 04bb056ee29065a472a70d480568b897a789ae59
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872901"
---
<a name="launching-a-modal-popup-window-from-server-code-c"></a>Открыв окно модальное окно из серверного кода (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)

> Элемент управления ModalPopup в наборе элементов управления AJAX предлагает простой способ создания модальное окно, с помощью клиентских средств. Однако в некоторых сценариях требуется запуск открывающий модальное окно на стороне сервера.


## <a name="overview"></a>Обзор

Элемент управления ModalPopup в наборе элементов управления AJAX предлагает простой способ создания модальное окно, с помощью клиентских средств. Однако в некоторых сценариях требуется запуск открывающий модальное окно на стороне сервера.

## <a name="steps"></a>Шаги

Во-первых веб-элемента управления кнопки ASP.NET требуется продемонстрировать, как работает элемент управления ModalPopup. Добавление кнопки в &lt;формы&gt; элемент на новую страницу:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample1.aspx)]

Затем необходимо разметку для контекстного меню, который требуется создать. Определить ее как `<asp:Panel>` управления и убедитесь, что он включает элемент управления Button. Элемент управления ModalPopup предоставляет функциональные возможности для создания таких кнопки Закрыть всплывающее окно; в противном случае нет никакого простого способа запускать его удалить.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample2.aspx)]

Затем добавьте элемент управления ModalPopup из набора средств ASP.NET AJAX на страницу. Задание свойств для кнопку, которая загружается элемент управления, кнопки, что делает его исчезают и идентификатор фактическое всплывающего окна.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample3.aspx)]

Как и все веб-страницы, на базе ASP.NET AJAX; Диспетчера скриптов, необходимой для загрузки необходимые библиотеки JavaScript для различных целевых браузеров:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample4.aspx)]

В браузере Запустите пример. При нажатии на кнопке отображается модальное окно. Для достижения такого же эффекта, с помощью кода на стороне сервера, требуется новая кнопка:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample5.aspx)]

Как видно, нажмите кнопку создает обратную передачу и выполняет `ServerButton_Click()` метод на сервере. Этот метод вызывается функция JavaScript `launchModal()` выполняется быть точным, функция JavaScript, которая выполняется после загрузки страницы:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample6.aspx)]

Задача `launchModal()` — отображение ModalPopup. `launchModal()` Функция выполняется после загрузки страницы HTML. В этот момент тем не менее, платформа AJAX для ASP.NET не был полностью загружен еще. Таким образом `launchModal()` функция просто задает переменную, которая управления ModalPopup должно быть показано ниже на:

[!code-html[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample7.html)]

`pageLoad()` Функцию JavaScript — это специальная функция, который исполняется после полной загрузки ASP.NET AJAX. Поэтому мы добавьте код для этой функции для отображения элемента управления ModalPopup, но только если `launchModal()` был вызван до:

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample8.js)]

`$find()` Функция ищет именованного элемента на странице и ожидает в качестве параметра идентификатор серверные. Таким образом `$find("mpe")` возвращает представление клиента управления ModalPopup; его `show()` метод позволяет отображается всплывающее окно.


[![Модальное окно появляется при нажатии любой кнопки](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)

Модальное окно появляется при нажатии любой кнопки ([Просмотр полноразмерное изображение](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Вперед](using-modalpopup-with-a-repeater-control-cs.md)
