---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: "С помощью элемента управления \"ползунок\" с автоматическая обратная передача (C#) | Документы Microsoft"
author: wenz
description: "Ползунок в наборе элементов управления AJAX имеет графический ползунок, можно управлять с помощью мыши. Это можно делать Автоматическая разноска ползунок..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: f6291f4162b3069a6316a60b4b29f82f55121aac
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="using-the-slider-control-with-auto-postback-c"></a>С помощью элемента управления "ползунок" с автоматическая обратная передача (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)

> Ползунок в наборе элементов управления AJAX имеет графический ползунок, можно управлять с помощью мыши. Это можно делать autopostback ползунок после изменения его значения.


## <a name="overview"></a>Обзор

Ползунок в наборе элементов управления AJAX имеет графический ползунок, можно управлять с помощью мыши. Это можно делать autopostback ползунок после изменения его значения.

## <a name="steps"></a>Шаги

Для упрощения ползунок автоматически обратного запроса на изменение, оба поля требуется атрибут `AutoPostBack="true"`: текстовое поле, которое станет ползунок самого и текстовое поле, которое содержит положение ползунка. Вот разметки, необходимые для этого:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

`SliderExtender` Управления из набора элементов управления ASP.NET AJAX назначает функциональность ползунок два текстовых поля:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

Элемент Дополнительные метки позже будет использоваться для уведомления пользователя обратной передачи:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

Наконец `ScriptManager` элемент управления AJAX для ASP.NET загружает набор средств управления для работы требуется JavaScript:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

Теперь в учетный ползунок обратно; на стороне сервера это событие может выявить и ответных:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]


[![При перемещении ползунка вызывает обратную передачу](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)

При перемещении ползунка вызывает обратную передачу ([Просмотр полноразмерное изображение](using-the-slider-control-with-auto-postback-cs/_static/image3.png))


[![После этого дата это изменение записывается в метке](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)

После этого дата это изменение записывается в метке ([Просмотр полноразмерное изображение](using-the-slider-control-with-auto-postback-cs/_static/image6.png))

>[!div class="step-by-step"]
[Вперед](databinding-the-slider-control-cs.md)
