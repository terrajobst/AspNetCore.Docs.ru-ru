---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
title: Привязка данных управления "ползунок" (Visual Basic) | Документы Microsoft
author: wenz
description: Ползунок в наборе элементов управления AJAX имеет графический ползунок, можно управлять с помощью мыши. Это позволяет привязать текущий иция...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4f3ba53f-d166-422d-b29c-403348057836
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 3ecd8598cd7fdcbbb4812e501bb30fa1f563a054
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879144"
---
<a name="databinding-the-slider-control-vb"></a>Привязка данных управления "ползунок" (Visual Basic)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)

> Ползунок в наборе элементов управления AJAX имеет графический ползунок, можно управлять с помощью мыши. Это позволяет привязать текущее положение ползунка на другой элемент управления ASP.NET.


## <a name="overview"></a>Обзор

Ползунок в наборе элементов управления AJAX имеет графический ползунок, можно управлять с помощью мыши. Это позволяет привязать текущее положение ползунка на другой элемент управления ASP.NET.

## <a name="steps"></a>Шаги

Чтобы активировать функциональные возможности ASP.NET AJAX и набора средств управления `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемент):

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample1.aspx)]

Добавьте два `TextBox` элементов управления на страницу. Один будет преобразован в графический ползунок, а другой будет содержать положение движка.

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample2.aspx)]

Следующий шаг уже является последним шагом. `SliderExtender` Управления из набора элементов управления ASP.NET AJAX делает ползунок из первого текстового поля и автоматически обновляет второе текстовое поле, когда изменения положение ползунка. Чтобы это работало `SliderExtender` `TargetControlID` атрибут необходимо задать идентификатор элемента в первом текстовом поле; `BoundControlID` атрибуту необходимо присвоить идентификатор второе текстовое поле.

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample3.aspx)]

Как видно в браузере, привязка данных работает в обоих направлениях: Введите новое значение в текстовом поле обновляет положение ползунка. Если второе текстовое поле только для чтения, могут добавлять слабую защиту с текстовым полем, чтобы оно было сложнее для пользователя, чтобы вручную обновить значение в нем.


[![Ползунок и текстового поля находятся в синхронизированном состоянии](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)

Ползунок и текстового поля находятся в синхронизированном состоянии ([Просмотр полноразмерное изображение](databinding-the-slider-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](using-the-slider-control-with-auto-postback-vb.md)
