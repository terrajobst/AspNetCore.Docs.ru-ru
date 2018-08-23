---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: Добавление анимации в элемент управления (C#) | Документация Майкрософт
author: wenz
description: Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления. В этом руководстве показано как...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: aa977883af931bb74b791104cf4ee3212079e43a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838462"
---
<a name="adding-animation-to-a-control-c"></a>Добавление анимации в элемент управления (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)

> Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления. Этом руководстве показано, как настроить такой анимации.


## <a name="overview"></a>Обзор

Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления. Этом руководстве показано, как настроить такой анимации.

## <a name="steps"></a>Шаги

Первым шагом является обычным образом, чтобы включить `ScriptManager` на странице, ASP.NET AJAX library загружается и может использоваться набор элементов управления:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

Анимация в этом сценарии будут применяться к панели текста, который выглядит следующим образом:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

Связанный класс CSS для панели определяет цвет фона и шириной:

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

Далее, необходимо `AnimationExtender`. После предоставления `ID` и обычные `runat="server"`, `TargetControlID` атрибута необходимо задать для элемента управления для анимации в нашем случае панели:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

Всего применяется анимация декларативно, используя синтаксис XML, к сожалению в настоящее время не полностью поддерживается технологией IntelliSense в Visual Studio. Корневым узлом является `<Animations>;` в этот узел, несколько событий разрешены определяющие при анимации take(s) месте:

- `OnClick` (щелчка мыши)
- `OnHoverOut` (когда указатель мыши покидает элемент управления)
- `OnHoverOver` (при наведении указателя мыши над элементом управления, остановка `OnHoverOut` анимации)
- `OnLoad` (когда страницы загрузки)
- `OnMouseOut` (когда указатель мыши покидает элемент управления)
- `OnMouseOver` (при наведении указателя мыши над элементом управления, не останавливает `OnMouseOut` анимации)

Платформа framework поставляется с набором анимации, каждый из них, представленный собственным XML-элементом. Ниже указаны:

- `<Color>` (Изменение цвета)
- `<FadeIn>` (плавный переход)
- `<FadeOut>` (исчезновение)
- `<Property>` (изменение свойства элемента управления)
- `<Pulse>` (pulsating)
- `<Resize>` (изменение размера)
- `<Scale>` (Пропорциональное изменение размера)

В этом примере панели должны скрывать. Анимация должна осуществляться 1,5 секунды (`Duration` атрибут), отображение 24 (анимации шаги) кадров в секунду (`Fps` attributs). Вот полная разметка для `AnimationExtender` управления:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

При выполнении этого скрипта панели отображается и исчезает в один с половиной секунд.


[![Исчезновение панели](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)

Исчезновение панели ([Просмотр полноразмерного изображения](adding-animation-to-a-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Вперед](executing-several-animations-at-the-same-time-cs.md)
