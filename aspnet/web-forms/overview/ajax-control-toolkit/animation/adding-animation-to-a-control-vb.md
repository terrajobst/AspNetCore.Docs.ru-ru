---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
title: Добавление анимации в элемент управления (Visual Basic) | Документация Майкрософт
author: wenz
description: Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления. В этом руководстве показано как...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c120187e-963e-4439-bb85-32771bc7f1f4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 3909422dc5d261b39f3efd7d7eaeb5cfb1976f2b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364930"
---
<a name="adding-animation-to-a-control-vb"></a>Добавление анимации в элемент управления (VB)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)

> Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления. Этом руководстве показано, как настроить такой анимации.


## <a name="overview"></a>Обзор

Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления. Этом руководстве показано, как настроить такой анимации.

## <a name="steps"></a>Шаги

Первым шагом является обычным образом, чтобы включить `ScriptManager` на странице, ASP.NET AJAX library загружается и может использоваться набор элементов управления:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample1.aspx)]

Анимация в этом сценарии будут применяться к панели текста, который выглядит следующим образом:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample2.aspx)]

Связанный класс CSS для панели определяет цвет фона и шириной:

[!code-css[Main](adding-animation-to-a-control-vb/samples/sample3.css)]

Далее, необходимо `AnimationExtender`. После предоставления `ID` и обычные `runat="server"`, `TargetControlID` атрибута необходимо задать для элемента управления для анимации в нашем случае панели:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample4.aspx)]

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

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample5.aspx)]

При выполнении этого скрипта панели отображается и исчезает в один с половиной секунд.


[![Исчезновение панели](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)

Исчезновение панели ([Просмотр полноразмерного изображения](adding-animation-to-a-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](dynamically-controlling-updatepanel-animations-cs.md)
> [Вперед](executing-several-animations-at-the-same-time-vb.md)
