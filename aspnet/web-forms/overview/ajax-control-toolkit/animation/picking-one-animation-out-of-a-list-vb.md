---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
title: Подбор одна анимация из списка (VB) | Документы Microsoft
author: wenz
description: Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Платформа также р...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 81ba9116-d485-40c0-8ff6-7e9ae23e0a0c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
msc.type: authoredcontent
ms.openlocfilehash: f2bd1b3cc72595da7e8901786ea8415d7c1c524a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872069"
---
<a name="picking-one-animation-out-of-a-list-vb"></a>Подбор одна анимация из списка (Visual Basic)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)

> Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Платформа также позволяет программисту одна анимация анимации, в зависимости от оценки кода JavaScript из списка выбора.


## <a name="overview"></a>Обзор

Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Платформа также позволяет программисту одна анимация анимации, в зависимости от оценки кода JavaScript из списка выбора.

## <a name="steps"></a>Шаги

Во-первых, включают `ScriptManager` страницы; затем библиотека ASP.NET AJAX загружена, что позволяет использовать набор элементов управления:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample1.aspx)]

Анимация применяется панель текста, который выглядит следующим образом:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample2.aspx)]

В связанный класс CSS для панели определяются цвет фона для работы с низким приоритетом, а также задать фиксированную ширину панели:

[!code-css[Main](picking-one-animation-out-of-a-list-vb/samples/sample3.css)]

Затем добавьте `AnimationExtender` страницу, предоставляя `ID`, `TargetControlID` атрибута и обязательным `runat="server":`

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample4.aspx)]

В пределах `<Animations>` узла, используйте `<OnLoad>` для выполнения анимации после полной загрузки страницы. Вместо обычных анимацию `<Case>` элемент вступает в действие. Значение атрибута SelectScript вычисляется; Возвращаемое значение должно быть числовым. В зависимости от того, это число, а один из subanimations в &lt;случай&gt; выполняется. Для экземпляра, если SelectScript равно 2, набор элементов управления выполняется третий анимации в &lt;случай&gt; (перечисление начинается с 0).

Приведенная ниже разметка определяет три subanimations: изменение размера ширину, высоту изменение размеров и исчезновение. Код JavaScript (`Math.floor(3 * Math.random())`) затем выбирает число между 0 и 2, чтобы одно из трех анимации выполняется:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample5.aspx)]


[![Одно из возможных три анимации: Получает ширину панели](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)

Одно из возможных три анимации: Получает ширину панели ([Просмотр полноразмерное изображение](picking-one-animation-out-of-a-list-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](animation-depending-on-a-condition-vb.md)
> [Вперед](animating-in-response-to-user-interaction-vb.md)
