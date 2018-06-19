---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: Обработка операций обратной передачи из контекстного меню элемента управления без UpdatePanel (C#) | Документы Microsoft
author: wenz
description: Расширитель PopupControl в наборе элементов управления AJAX предлагает простой способ запуска всплывающего окна при активации любого другого элемента управления. При обратной передаче в su...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 59ffa05945289de6e01e2c21dd5a0f82ca1fa374
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879547"
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a>Обработка операций обратной передачи из контекстного меню элемента управления без UpdatePanel (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)

> Расширитель PopupControl в наборе элементов управления AJAX предлагает простой способ запуска всплывающего окна при активации любого другого элемента управления. При обратной передаче таких панели и на странице имеется несколько панелей бывает трудно определить, какая панель была нажата.


## <a name="overview"></a>Обзор

Расширитель PopupControl в наборе элементов управления AJAX предлагает простой способ запуска всплывающего окна при активации любого другого элемента управления. При обратной передаче таких панели и на странице имеется несколько панелей бывает трудно определить, какая панель была нажата.

## <a name="steps"></a>Шаги

При использовании `PopupControl` при обратной передаче, но без `UpdatePanel` набор элементов управления на странице не предлагает способ определить, какой элемент клиента инициировал контекстного меню, который в свою очередь, вызвавшего обратную передачу. Однако небольшой прием обеспечивает временное решение для этого сценария.

Во-первых, вот базовая настройка: два текстовых поля, для которых триггера же всплывающем календаре. Два `PopupControlExtenders` объединяйте текстовые поля и контекстного меню.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

Основная идея заключается в том, чтобы добавить скрытое поле формы в &lt; `form` &gt; элементом, содержащим текстовое поле, которое запускается всплывающее окно:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

При загрузке страницы, кода JavaScript добавляет обработчик событий в оба поля: когда пользователь нажимает на текстовое поле, его имя записывается в скрытое поле формы:

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

В серверный код необходимо считать значение скрытого поля. Поскольку скрытые поля формы просты для работы, белого списка способ проверить скрытые значение является обязательным. После определения правильного текстовое поле, в него записывается дату из календаря.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]


[![Календарь появляется, когда пользователь щелкает в текстовое поле](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)

Календарь появляется, когда пользователь щелкает в текстовое поле ([Просмотр полноразмерное изображение](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))


[![Щелкните дату помещает его в текстовое поле](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)

Щелкните дату помещает его в текстовое поле ([Просмотр полноразмерное изображение](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Назад](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
> [Вперед](using-multiple-popup-controls-vb.md)
