---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
title: "Анимации в зависимости от условия (VB) | Документы Microsoft"
author: wenz
description: "Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Является ли анимации..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1b87d8d6-b3f7-4126-b51c-d41442fbf947
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
msc.type: authoredcontent
ms.openlocfilehash: cc8600f33f9c27e1045f5083a126b9d2d1e90303
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="animation-depending-on-a-condition-vb"></a>Анимации в зависимости от условия (Visual Basic)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)

> Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Ли анимация выполняется или не может также зависеть от условие в виде кода JavaScript.


## <a name="overview"></a>Обзор

Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Ли анимация выполняется или не может также зависеть от условие в виде кода JavaScript.

## <a name="steps"></a>Шаги

Во-первых, включают `ScriptManager` страницы; затем библиотека ASP.NET AJAX загружена, что позволяет использовать набор элементов управления:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample1.aspx)]

Анимация применяется панель текста, который выглядит следующим образом:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample2.aspx)]

В связанный класс CSS для панели определяются цвет фона для работы с низким приоритетом, а также задать фиксированную ширину панели:

[!code-css[Main](animation-depending-on-a-condition-vb/samples/sample3.css)]

Затем добавьте `AnimationExtender` страницу, предоставляя `ID`, `TargetControlID` атрибута и обязательным`runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample4.aspx)]

В пределах `<Animations>` узла, используйте `<OnLoad>` для выполнения анимации после полной загрузки страницы. Вместо обычных анимацию `<Condition>` элемент вступает в действие. Код JavaScript, предоставленных в качестве значения для `ConditionScript` атрибут выполняется во время выполнения. Если значение равно true, анимация выполняется, в противном случае не. Приведенная ниже разметка предоставляет две анимации, каждый из них выполняется в 50% случаев после случайных. Так как может существовать только одна анимация в течение `<OnLoad>`, два `<Condition>` анимации соединяются друг с другом с использованием `<Sequence>` элемента:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample5.aspx)]

Обратите внимание, что знак «меньше» (`<`) в `ConditionScript` атрибут должен быть escape-символом (). При запуске этого скрипта, либо нет запусков анимации, один из двух или для обоих выполните.


[![Панель исчезновение без изменения размера, так не выполняется второй анимации, первый](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)

Панель исчезновение без изменения размера, так не выполняется второй анимации, первый ([Просмотр полноразмерное изображение](animation-depending-on-a-condition-vb/_static/image3.png))

>[!div class="step-by-step"]
[Назад](executing-several-animations-after-each-other-vb.md)
[Вперед](picking-one-animation-out-of-a-list-vb.md)
