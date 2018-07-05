---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
title: Динамическое управление анимациями UpdatePanel (VB) | Документация Майкрософт
author: wenz
description: Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления. Для содержимого...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: bea66072-59b6-42b4-98fa-211812f5925f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
msc.type: authoredcontent
ms.openlocfilehash: 79a256f5d5c62123184beffce3365ade0ac13e46
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365945"
---
<a name="dynamically-controlling-updatepanel-animations-vb"></a>Динамическое управление анимациями UpdatePanel (VB)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)

> Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления. Для содержимого элемента управления UpdatePanel, существует специальные расширения, которая основывается на использовании framework анимации: UpdatePanelAnimation. Он также может работать вместе с триггерах UpdatePanel.


## <a name="overview"></a>Обзор

Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления. Для содержимого `UpdatePanel`, существует специальные расширения, которая основывается на использовании framework анимации: `UpdatePanelAnimation`. Он также может работать вместе с `UpdatePanel` триггеров.

## <a name="steps"></a>Шаги

Первым шагом является обычным образом, чтобы включить `ScriptManager` на странице, ASP.NET AJAX library загружается и может использоваться набор элементов управления:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample1.aspx)]

Анимация в этом сценарии будет применяться для отображения в виде текущего времени. Эти сведения можно записать в метку с помощью `Page_Load()` метод, или (для простоты) используется следующий встроенный код:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample2.aspx)]

Кроме того создается кнопка для активации, обновляя время:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample3.aspx)]

Этот код помещается в переменную `<ContentTemplate>` раздел `UpdatePanel` элемент. Панели `UpdateMode` атрибута должно быть присвоено `"Conditional"`, поскольку только триггеры могут обновлять содержимое панели. В `<Triggers>` раздел `UpdatePanel`, создается триггер асинхронной обратной передачи и привязаны к `Click` события кнопки. Таким образом, если пользователь нажимает кнопку, `UpdatePanel` обновляется. Далее приведена разметка для `UpdatePanel` управления:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample4.aspx)]

Наконец `UpdatePanelAnimationExtender` должен быть настроен: задать `TargetControlID` атрибут с идентификатором панели и определите анимацию в модуле. Плавный переход в делает смысле, который создает удобный визуальное выделение важных фрагментов, на время обновления. Расширения разметки может выглядеть следующим образом:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample5.aspx)]

Запустите файл в браузере. При каждом нажатии кнопки текущее время отображается на панели, всегда плавный переход течение одной секунды.


[![Текущее время плавный переход](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)

Текущее время плавный переход ([Просмотр полноразмерного изображения](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](animating-an-updatepanel-control-vb.md)
