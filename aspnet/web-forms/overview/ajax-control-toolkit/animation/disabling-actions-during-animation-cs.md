---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: Отключить действия во время анимации (C#) | Документы Microsoft
author: wenz
description: Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Оно также поддерживает действия...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: 7862c5026a48fbee6eb48beb411e5e1d60c8b406
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="disabling-actions-during-animation-c"></a>Отключить действия во время анимации (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)

> Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Оно также поддерживает действия, например, щелчки мышью. Однако при запуске анимации щелчка кнопкой мыши является предпочтительным для отключения щелчки мышью во время анимации.


## <a name="overview"></a>Обзор

Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Оно также поддерживает действия, например, щелчки мышью. Однако при запуске анимации щелчка кнопкой мыши является предпочтительным для отключения щелчки мышью во время анимации.

## <a name="steps"></a>Шаги

Во-первых, включают `ScriptManager` страницы; затем библиотека ASP.NET AJAX загружена, что позволяет использовать набор элементов управления:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

Анимация будет применяться к HTML-кнопка следующим образом:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

Обратите внимание, что HTML-элемент управления используется вместо веб-элемент управления, поскольку мы не хотим кнопка для создания обратной передачи; он просто должны запустить клиентские анимации для нас.

Затем добавьте `AnimationExtender` страницу, предоставляя `ID`, `TargetControlID` атрибута и обязательным `runat="server"`:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

В пределах `<Animations>` узел, `<OnClick>` является правильного элемента для обработки щелчка мыши. Однако во время анимации удалось нажата кнопка. `<EnableAction>` Элемент заняться. Параметр `Enabled="false"` отключает кнопку как часть анимации. Так как мы используем несколько отдельных анимации (Отключение кнопки и фактический анимации), `<Parallel>` элемент является обязательным для приклейте одной анимации вместе в одну. Вот полную разметку для `AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

Также возможно включить кнопку после анимации, используя следующий XML-элемент в конец списка:

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

Однако в данном сценарии это будет бесполезен после кнопки исчезает и остается невидимым в конце анимации.


[![Кнопка отключена, как только выполняется анимация](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)

Кнопка отключена, как только выполняется анимация ([Просмотр полноразмерное изображение](disabling-actions-during-animation-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](animating-in-response-to-user-interaction-cs.md)
> [Вперед](triggering-an-animation-in-another-control-cs.md)
