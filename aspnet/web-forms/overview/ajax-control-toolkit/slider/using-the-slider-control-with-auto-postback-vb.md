---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
title: "С помощью элемента управления \"ползунок\" с автоматическая обратная передача (VB) | Документы Microsoft"
author: wenz
description: "Ползунок в наборе элементов управления AJAX имеет графический ползунок, можно управлять с помощью мыши. Это можно делать Автоматическая разноска ползунок..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 41d1abba-97a5-4a45-9b44-d05624c19777
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
msc.type: authoredcontent
ms.openlocfilehash: fedd3ff947c6f5d5d4d00791087e9fd825fdf9d3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="using-the-slider-control-with-auto-postback-vb"></a>С помощью элемента управления "ползунок" с автоматическая обратная передача (Visual Basic)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)

> Ползунок в наборе элементов управления AJAX имеет графический ползунок, можно управлять с помощью мыши. Это можно делать autopostback ползунок после изменения его значения.


## <a name="overview"></a>Обзор

Ползунок в наборе элементов управления AJAX имеет графический ползунок, можно управлять с помощью мыши. Это можно делать autopostback ползунок после изменения его значения.

## <a name="steps"></a>Шаги

Для упрощения ползунок автоматически обратного запроса на изменение, оба поля требуется атрибут `AutoPostBack="true"`: текстовое поле, которое станет ползунок самого и текстовое поле, которое содержит положение ползунка. Вот разметки, необходимые для этого:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample1.aspx)]

`SliderExtender` Управления из набора элементов управления ASP.NET AJAX назначает функциональность ползунок два текстовых поля:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample2.aspx)]

Элемент Дополнительные метки позже будет использоваться для уведомления пользователя обратной передачи:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample3.aspx)]

Наконец `ScriptManager` элемент управления AJAX для ASP.NET загружает набор средств управления для работы требуется JavaScript:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample4.aspx)]

Теперь в учетный ползунок обратно; на стороне сервера это событие может выявить и ответных:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample5.aspx)]


[![При перемещении ползунка вызывает обратную передачу](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)

При перемещении ползунка вызывает обратную передачу ([Просмотр полноразмерное изображение](using-the-slider-control-with-auto-postback-vb/_static/image3.png))


[![После этого дата это изменение записывается в метке](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)

После этого дата это изменение записывается в метке ([Просмотр полноразмерное изображение](using-the-slider-control-with-auto-postback-vb/_static/image6.png))

>[!div class="step-by-step"]
[Назад](databinding-the-slider-control-cs.md)
[Вперед](databinding-the-slider-control-vb.md)
