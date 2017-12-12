---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
title: "Использование обратных передач с ReorderList (VB) | Документы Microsoft"
author: wenz
description: "Элемент управления ReorderList в наборе элементов управления AJAX предоставляет список, который можно переупорядочить пользователем с помощью операции перетаскивания. Каждый раз, когда список переупорядочивается, po..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e5b6ed70-19ed-4024-ba4f-6d78e8acdc0f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 971d060f2ee69e82ec574392a308754e015b0fd0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="using-postbacks-with-reorderlist-vb"></a>Использование обратных передач с ReorderList (Visual Basic)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)

> Элемент управления ReorderList в наборе элементов управления AJAX предоставляет список, который можно переупорядочить пользователем с помощью операции перетаскивания. Всякий раз, когда список переупорядочивается, обратную передачу должны сообщить серверу изменения.


## <a name="overview"></a>Обзор

`ReorderList` Список элемента управления в наборе элементов управления AJAX можно переупорядочить пользователем с помощью операции перетаскивания. Всякий раз, когда список переупорядочивается, обратную передачу должны сообщить серверу изменения.

## <a name="steps"></a>Шаги

Существует несколько источников данных для `ReorderList` элемента управления. Один является применение `XmlDataSource` управления:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample1.aspx)]

Чтобы привязать этот XML-код для `ReorderList` необходимо задать элемент управления и Включение обратной передачи, следующие атрибуты:

- `DataSourceID`: Идентификатор источника данных
- `SortOrderField`: Свойство, по которому выполняется сортировка
- `AllowReorder`: Следует ли разрешить пользователю изменить порядок элементов списка
- `PostBackOnReorder`: Следует ли создавать обратной передачи, когда переупорядочить список

Ниже приведен соответствующий разметки для элемента управления.

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample2.aspx)]

В пределах `ReorderList` управления определенных данных из источника данных может быть связана с помощью `Eval()` метод:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample3.aspx)]

В случайном положении на странице метку хранения информации, когда произошло последнее изменение порядка:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample4.aspx)]

Эта метка заполняется текстом в серверный код, обработка обратной передачи:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample5.aspx)]

Наконец, чтобы активировать функциональные возможности ASP.NET AJAX и набора средств управления `ScriptManager` управления могут быть помещены на странице:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample6.aspx)]


[![Каждый переупорядочение вызывает обратную передачу](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)

Каждый переупорядочение вызывает обратную передачу ([Просмотр полноразмерное изображение](using-postbacks-with-reorderlist-vb/_static/image3.png))

>[!div class="step-by-step"]
[Назад](drag-and-drop-via-reorderlist-cs.md)
[Вперед](drag-and-drop-via-reorderlist-vb.md)
