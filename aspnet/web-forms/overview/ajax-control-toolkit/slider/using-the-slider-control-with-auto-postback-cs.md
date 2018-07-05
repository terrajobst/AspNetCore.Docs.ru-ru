---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: С помощью элемента управления Slider с автоматической обратной передачей (C#) | Документация Майкрософт
author: wenz
description: Элемент управления "ползунок" в AJAX Control Toolkit предоставляет графическим ползунком, которые могут контролироваться с помощью мыши. Это можно делать Автоматическая разноска "ползунок"...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: 8e855bebf557536b5f9e2b2730f828e54c63221e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366836"
---
<a name="using-the-slider-control-with-auto-postback-c"></a>С помощью элемента управления Slider с автоматической обратной передачей (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)

> Элемент управления "ползунок" в AJAX Control Toolkit предоставляет графическим ползунком, которые могут контролироваться с помощью мыши. Это можно делать autopostback ползунок один раз изменении ее значения.


## <a name="overview"></a>Обзор

Элемент управления "ползунок" в AJAX Control Toolkit предоставляет графическим ползунком, которые могут контролироваться с помощью мыши. Это можно делать autopostback ползунок один раз изменении ее значения.

## <a name="steps"></a>Шаги

Чтобы сделать ползунок автоматически обратной передачи, при изменении, оба поля атрибут должен быть добавлен `AutoPostBack="true"`: текстовое поле, которое станет самой ползунок и текстовое поле, которое содержит положение ползунка. Далее приведена разметка необходимые для этого:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

`SliderExtender` Элемента управления ASP.NET AJAX Control Toolkit назначает функциональные возможности "ползунок" для двух текстовых полях:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

Дополнительные метки элемента будет использоваться позже для информирования пользователей обратной передачи:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

Наконец `ScriptManager` элемент управления AJAX для ASP.NET загружает требуемого кода JavaScript для набора средств управления для работы:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

Теперь ползунок обратной передаче; на стороне сервера это событие может быть перехвачено и обрабатываемое:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]


[![Перемещая ползунок инициирует обратную передачу](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)

Перемещая ползунок инициирует обратную передачу ([Просмотр полноразмерного изображения](using-the-slider-control-with-auto-postback-cs/_static/image3.png))


[![После этого дата это изменение записывается в метке](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)

После этого дата это изменение записывается в метке ([Просмотр полноразмерного изображения](using-the-slider-control-with-auto-postback-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Вперед](databinding-the-slider-control-cs.md)
