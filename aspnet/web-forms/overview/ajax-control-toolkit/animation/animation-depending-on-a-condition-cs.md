---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
title: Анимация в зависимости от условия (C#) | Документация Майкрософт
author: wenz
description: Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления. Является ли анимация...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b7a28c0d-efb9-443a-80a4-1a5ee54671cd
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
msc.type: authoredcontent
ms.openlocfilehash: c28f4583e6f0d1bb5c1438322980a44aa53fbd89
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391367"
---
<a name="animation-depending-on-a-condition-c"></a>Анимация в зависимости от условия (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)

> Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления. Запускается ли анимация или не может также зависеть условие в виде кода JavaScript.


## <a name="overview"></a>Обзор

Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления. Запускается ли анимация или не может также зависеть условие в виде кода JavaScript.

## <a name="steps"></a>Шаги

Во-первых, включите `ScriptManager` страницы; затем ASP.NET AJAX library загружена, что позволяет использовать набор средств управления:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample1.aspx)]

Анимация будет применяться к панели текста, который выглядит следующим образом:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample2.aspx)]

В связанный класс CSS для панели определить цвет фона, удобная и также установить фиксированную ширину для панели:

[!code-css[Main](animation-depending-on-a-condition-cs/samples/sample3.css)]

Затем добавьте `AnimationExtender` на страницу, предоставляя `ID`, `TargetControlID` атрибут и обязательным `runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample4.aspx)]

В рамках `<Animations>` узла, используйте `<OnLoad>` для запуска анимации после полной загрузки страницы. Вместо одного из обычных анимаций `<Condition>` элемент вступает в действие. Код JavaScript, предоставленных в качестве значения из `ConditionScript` атрибут выполняется во время выполнения. Если значение равно true, анимация выполняется, в противном случае не. Следующая разметка предоставляет две анимации, каждый из них выполняется в 50% случаев после случайных. Так как может существовать только одна анимация в `<OnLoad>`, два `<Condition>` анимации соединяются друг с другом с использованием `<Sequence>` элемент:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample5.aspx)]

Обратите внимание, что знак «меньше» (`<`) в `ConditionScript` атрибут должен быть escape-(). При запуске этого скрипта, либо нет запусков анимации, один из двух или для обоих сделать.


[![Панели исчезновение без изменения размера, поэтому второй анимации запуски, первый из них не](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)

Панели исчезновение без изменения размера, поэтому второй анимации запуски, первый из них не ([Просмотр полноразмерного изображения](animation-depending-on-a-condition-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](executing-several-animations-after-each-other-cs.md)
> [Вперед](picking-one-animation-out-of-a-list-cs.md)
