---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: Отключение действий во время анимации (C#) | Документация Майкрософт
author: wenz
description: Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления. Она также поддерживает действия...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: 4315f06ea1599bacb93a23a3759610e19754cfba
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838125"
---
<a name="disabling-actions-during-animation-c"></a>Отключение действий во время анимации (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)

> Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления. Она также поддерживает действия, например, щелчки мышью. Тем не менее при щелчка кнопкой мыши запускает анимацию, желательно отключить щелчки мыши во время анимации.


## <a name="overview"></a>Обзор

Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления. Она также поддерживает действия, например, щелчки мышью. Тем не менее при щелчка кнопкой мыши запускает анимацию, желательно отключить щелчки мыши во время анимации.

## <a name="steps"></a>Шаги

Во-первых, включите `ScriptManager` страницы; затем ASP.NET AJAX library загружена, что позволяет использовать набор средств управления:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

Анимация будет применяться к HTML-кнопка следующим образом:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

Обратите внимание, что элемент управления HTML используется вместо веб-элемент управления, поскольку мы не хотим, кнопку, чтобы создать обратной передачи; он просто должен запустить клиентские анимации для нас.

Затем добавьте `AnimationExtender` на страницу, предоставляя `ID`, `TargetControlID` атрибут и обязательным `runat="server"`:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

В рамках `<Animations>` узел, `<OnClick>` является правильного элемента для обработки щелчка мыши. Тем не менее во время анимации, а также может быть нажата кнопка. `<EnableAction>` Элемент заняться. Параметр `Enabled="false"` отключает кнопку как часть анимации. Так как мы используем несколько отдельных анимаций, (Отключение кнопки и фактическое анимации), `<Parallel>` элемент является обязательным для объединения одной анимации вместе в одну. Вот полная разметка для `AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

Также возможно включить кнопку после анимации, используя следующий XML-элемент в конец списка:

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

Однако в данном сценарии это окажется бесполезной после кнопки исчезает и остается невидимым в конце анимации.


[![Кнопка отключена, а анимация](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)

Кнопка отключена, а анимация ([Просмотр полноразмерного изображения](disabling-actions-during-animation-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](animating-in-response-to-user-interaction-cs.md)
> [Вперед](triggering-an-animation-in-another-control-cs.md)
