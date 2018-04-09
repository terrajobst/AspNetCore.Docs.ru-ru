---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: Добавление анимации в элемент управления (C#) | Документы Microsoft
author: wenz
description: Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. В этом учебнике показано как...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: ba122660045c3f5dd4b11f118df174a79de814a1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="adding-animation-to-a-control-c"></a>Добавление анимации в элемент управления (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)

> Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Этого учебника показано, как настроить такие анимации.


## <a name="overview"></a>Обзор

Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Этого учебника показано, как настроить такие анимации.

## <a name="steps"></a>Шаги

Первым шагом является обычным образом, чтобы включить `ScriptManager` на странице, чтобы загрузить библиотеки ASP.NET AJAX и может использоваться набор элементов управления:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

Анимации в этом сценарии будет применяться к панель текста, который выглядит следующим образом:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

Связанный класс CSS для панели определяет цвет фона ширины и высоты.

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

Далее, нам нужно `AnimationExtender`. После предоставления `ID` и обычные `runat="server"`, `TargetControlID` атрибута должно быть присвоено элемента управления для анимации в нашем случае панели:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

Всего применяется анимация декларативно, используя синтаксис XML, к сожалению в настоящее время не полностью поддерживается технологией IntelliSense в Visual Studio. Корневой узел — `<Animations>;` в этом узле допускается несколько событий определить, когда анимаций take(s) месте:

- `OnClick` (щелчок мыши)
- `OnHoverOut` (когда указатель мыши покидает элемент управления)
- `OnHoverOver` (при наведении курсора мыши на элемент управления, остановка `OnHoverOut` анимации)
- `OnLoad` (если страница загрузки)
- `OnMouseOut` (когда указатель мыши покидает элемент управления)
- `OnMouseOver` (при наведении курсора мыши на элемент управления, не останавливая `OnMouseOut` анимации)

Платформа поставляется с набором анимации, каждый из представленного собственным XML-элементом. Ниже приведен фрагмент.

- `<Color>` (изменить цвета)
- `<FadeIn>` (затухание в)
- `<FadeOut>` (исчезновение)
- `<Property>` (изменение свойства элемента управления)
- `<Pulse>` (pulsating)
- `<Resize>` (изменение размера)
- `<Scale>` (пропорционально изменять размер)

В этом примере панели должны Исчезание. Анимация должна осуществляться 1,5 секунды (`Duration` атрибут), отображение 24 (шаги анимации) кадров в секунду (`Fps` attributs). Вот полную разметку для `AnimationExtender` управления:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

При запуске этого скрипта панели отображается и отчетливее Полуторный секунд.


[![Исчезновение панели](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)

Исчезновение панели ([Просмотр полноразмерное изображение](adding-animation-to-a-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Вперед](executing-several-animations-at-the-same-time-cs.md)
