---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
title: "Обработка операций обратной передачи из контекстного меню элемента управления с помощью элемента управления UpdatePanel (Visual Basic) | Документы Microsoft"
author: wenz
description: "Расширитель PopupControl в наборе элементов управления AJAX предлагает простой способ запуска всплывающего окна при активации любого другого элемента управления. Особое внимание не надо..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ec9db57c-9f68-402a-bf4c-0d63d5f6908e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: 4445437f25925429d240b7fe2d019855afc52fe0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-vb"></a>Обработка операций обратной передачи из контекстного меню элемента управления с помощью элемента управления UpdatePanel (Visual Basic)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)

> Расширитель PopupControl в наборе элементов управления AJAX предлагает простой способ запуска всплывающего окна при активации любого другого элемента управления. Особое внимание должен принимать при обратной передаче в таких всплывающего окна.


## <a name="overview"></a>Обзор

Расширитель PopupControl в наборе элементов управления AJAX предлагает простой способ запуска всплывающего окна при активации любого другого элемента управления. Особое внимание должен принимать при обратной передаче в таких всплывающего окна.

## <a name="steps"></a>Шаги

При использовании `PopupControl` при обратной передаче, `UpdatePanel` может предотвратить обновление страницы, вызванные обратной передачи. Приведенная ниже разметка определяет ряд важных элементов:

- Объект `ScriptManager` управления, что набор элементов управления ASP.NET AJAX
- Два `TextBox` элементов управления, которые будут активировать всплывающего окна
- Объект `Panel` элемент управления, который будет использоваться в качестве всплывающего окна
- На панели `Calendar` управления, встроенный в `UpdatePanel` управления
- Два `PopupControlExtender` элементов управления, назначьте панели текстовые поля

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample1.aspx)]

Обратите внимание, что `OnSelectionChanged` атрибут `Calendar` элемента управления задано. Так происходит, когда пользователь выбирает дату в календаре, обратная передача и серверного метода `c1_SelectionChanged()` выполняется. В этом методе необходимо извлечь текущую дату и записана в текстовом поле.

Используется следующий синтаксис для этого: во-первых, прокси-объект для `PopupControlExtender` на странице должен быть создан. Предоставляет набор элементов управления ASP.NET AJAX `GetProxyForCurrentPopup()` метод. Этот метод возвращает объект поддерживает `Commit()` метод, который отправляет значение элемента управления, являющегося контекстное меню (не элемента управления, являющегося вызова метода!). В следующем коде представлен в качестве аргумента для выбранной даты `Commit()` метода, вынуждая кода на обратную запись выбранную дату в текстовом поле:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample2.aspx)]

Теперь по щелчку календарная дата, выбранная дата отображается в соответствующем текстовом поле Создание управляющий элемент выбора даты, в настоящее время находятся на многих веб-сайтах.


[![Календарь появляется, когда пользователь щелкает в текстовое поле](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)

Календарь появляется, когда пользователь щелкает в текстовое поле ([Просмотр полноразмерное изображение](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))


[![Щелкните дату помещает его в текстовое поле](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)

Щелкните дату помещает его в текстовое поле ([Просмотр полноразмерное изображение](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))

>[!div class="step-by-step"]
[Назад](using-multiple-popup-controls-vb.md)
[Вперед](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)
