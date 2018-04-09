---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
title: Отключить действия во время анимации (VB) | Документы Microsoft
author: wenz
description: Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Оно также поддерживает действия...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a86c0276-6481-46ee-8b4f-8c2009399ee9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
msc.type: authoredcontent
ms.openlocfilehash: 9e2a0517800e90788bb67c1d75482a3d9340674b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="disabling-actions-during-animation-vb"></a>Отключить действия во время анимации (Visual Basic)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)

> Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Оно также поддерживает действия, например, щелчки мышью. Однако при запуске анимации щелчка кнопкой мыши является предпочтительным для отключения щелчки мышью во время анимации.


## <a name="overview"></a>Обзор

Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Оно также поддерживает действия, например, щелчки мышью. Однако при запуске анимации щелчка кнопкой мыши является предпочтительным для отключения щелчки мышью во время анимации.

## <a name="steps"></a>Шаги

Во-первых, включают `ScriptManager` страницы; затем библиотека ASP.NET AJAX загружена, что позволяет использовать набор элементов управления:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample1.aspx)]

Анимация будет применяться к HTML-кнопка следующим образом:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample2.aspx)]

Обратите внимание, что HTML-элемент управления используется вместо веб-элемент управления, поскольку мы не хотим кнопка для создания обратной передачи; он просто должны запустить клиентские анимации для нас.

Затем добавьте `AnimationExtender` страницу, предоставляя `ID`, `TargetControlID` атрибута и обязательным `runat="server"`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample3.aspx)]

В пределах `<Animations>` узел, `<OnClick>` является правильного элемента для обработки щелчка мыши. Однако во время анимации удалось нажата кнопка. `<EnableAction>` Элемент заняться. Параметр `Enabled="false"` отключает кнопку как часть анимации. Так как мы используем несколько отдельных анимации (Отключение кнопки и фактический анимации), `<Parallel>` элемент является обязательным для приклейте одной анимации вместе в одну. Вот полную разметку для `AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample4.aspx)]

Также возможно включить кнопку после анимации, используя следующий XML-элемент в конец списка:

[!code-xml[Main](disabling-actions-during-animation-vb/samples/sample5.xml)]

Однако в данном сценарии это будет бесполезен после кнопки исчезает и остается невидимым в конце анимации.


[![Кнопка отключена, как только выполняется анимация](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)

Кнопка отключена, как только выполняется анимация ([Просмотр полноразмерное изображение](disabling-actions-during-animation-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](animating-in-response-to-user-interaction-vb.md)
> [Вперед](triggering-an-animation-in-another-control-vb.md)
