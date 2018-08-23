---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
title: Привязка данных элемента управления "ползунок" (C#) | Документация Майкрософт
author: wenz
description: Элемент управления "ползунок" в AJAX Control Toolkit предоставляет графическим ползунком, которые могут контролироваться с помощью мыши. Это позволяет привязать текущий иция...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b7f77869-aa1d-4025-924f-622c57112db6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 951a7484f0dbb14ee7f1e1d62c9666e5cc49e7c1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838181"
---
<a name="databinding-the-slider-control-c"></a>Привязка данных элемента управления "ползунок" (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)

> Элемент управления "ползунок" в AJAX Control Toolkit предоставляет графическим ползунком, которые могут контролироваться с помощью мыши. Это позволяет привязать текущее положение ползунка на другой элемент управления ASP.NET.


## <a name="overview"></a>Обзор

Элемент управления "ползунок" в AJAX Control Toolkit предоставляет графическим ползунком, которые могут контролироваться с помощью мыши. Это позволяет привязать текущее положение ползунка на другой элемент управления ASP.NET.

## <a name="steps"></a>Шаги

Для активации функции ASP.NET AJAX и Control Toolkit, `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемента):

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample1.aspx)]

Добавьте два `TextBox` элементов управления на страницу. Один будет преобразован графическим ползунком, а другой будет содержать положение ползунка.

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample2.aspx)]

Следующим шагом уже является последним шагом. `SliderExtender` Элемента управления ASP.NET AJAX Control Toolkit делает ползунок из первое текстовое поле и автоматически обновляет второе текстовое поле, когда изменения позиции ползунка. В порядке, чтобы это сработало `SliderExtender` `TargetControlID` атрибута необходимо задать идентификатор элемента в первом текстовом поле; `BoundControlID` атрибута необходимо задать идентификатор второго текстового поля.

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample3.aspx)]

Как вы видите в браузере, привязка данных работает в обоих направлениях: Введите новое значение в текстовом поле обновляет положение ползунка. Если второе текстовое поле только для чтения, может добавить слабую защиту в текстовое поле, так как это сложнее для пользователя, чтобы вручную обновить значение в ней.


[![Ползунок и текстовое поле синхронизированы](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)

Ползунок и текстовое поле синхронизированы ([Просмотр полноразмерного изображения](databinding-the-slider-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](using-the-slider-control-with-auto-postback-cs.md)
> [Вперед](using-the-slider-control-with-auto-postback-vb.md)
