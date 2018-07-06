---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
title: Выполнение нескольких анимаций друг за другом (Visual Basic) | Документация Майкрософт
author: wenz
description: Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления. Он позволяет выполнять severa...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 21ece509-79cc-4d9d-892d-7b6e9c4d3502
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
msc.type: authoredcontent
ms.openlocfilehash: 3d94619ca40d288f8a5a391ef02103902bd2550c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835959"
---
<a name="executing-several-animations-after-each-other-vb"></a>Выполнение нескольких анимаций друг за другом (VB)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)

> Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления. Он одновременное выполнение нескольких анимаций один за другим.


## <a name="overview"></a>Обзор

Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления. Он одновременное выполнение нескольких анимаций один за другим.

## <a name="steps"></a>Шаги

Во-первых, включите `ScriptManager` страницы; затем ASP.NET AJAX library загружена, что позволяет использовать набор средств управления:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample1.aspx)]

Анимация будет применяться к панели текста, который выглядит следующим образом:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample2.aspx)]

В связанный класс CSS для панели определить цвет фона, удобная и также установить фиксированную ширину для панели:

[!code-css[Main](executing-several-animations-after-each-other-vb/samples/sample3.css)]

Затем добавьте `AnimationExtender` на страницу, предоставляя `ID`, `TargetControlID` атрибут и обязательным `runat="server":`

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample4.aspx)]

В рамках `<Animations>` узла, используйте `<OnLoad>` для запуска анимации после полной загрузки страницы. Как правило `<OnLoad>` принимает только одна анимация. Платформа анимации позволяет объединить несколько анимаций в один с помощью `<Sequence>` элемент. Все анимации в `<Sequence>` , выполняются за другим. Вот возможные разметку для `AnimationExtender` элемента управления, сначала внесение панели «ширина» и затем уменьшая высоту:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample5.aspx)]

При выполнении этого сценария, панели первой получает шире и затем меньшего размера.


[![Сначала увеличивается ширина](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)

Сначала ширина увеличивается ([Просмотр полноразмерного изображения](executing-several-animations-after-each-other-vb/_static/image3.png))


[![Затем уменьшается на высоту](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)

Затем уменьшается на высоту ([Просмотр полноразмерного изображения](executing-several-animations-after-each-other-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Назад](executing-several-animations-at-the-same-time-vb.md)
> [Вперед](animation-depending-on-a-condition-vb.md)
