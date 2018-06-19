---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
title: Динамическому управлению UpdatePanel анимации (VB) | Документы Microsoft
author: wenz
description: Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Для содержимого...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: bea66072-59b6-42b4-98fa-211812f5925f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
msc.type: authoredcontent
ms.openlocfilehash: ff2853b4457a83a7367b4d1072d21929c40a3ed2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871549"
---
<a name="dynamically-controlling-updatepanel-animations-vb"></a>Динамическому управлению UpdatePanel анимации (Visual Basic)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)

> Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Для содержимого элемента управления UpdatePanel, существует специальные расширения, в большой степени основывается на платформе анимации: UpdatePanelAnimation. Она также может работать вместе с триггерами UpdatePanel.


## <a name="overview"></a>Обзор

Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Для содержимого `UpdatePanel`, специальные расширения существует, в большой степени основывается на платформе анимации: `UpdatePanelAnimation`. Можно также работать вместе с `UpdatePanel` триггеров.

## <a name="steps"></a>Шаги

Первым шагом является обычным образом, чтобы включить `ScriptManager` на странице, чтобы загрузить библиотеки ASP.NET AJAX и может использоваться набор элементов управления:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample1.aspx)]

Анимация в этом сценарии будет применяться для отображения текущего времени. Эти сведения могут записываться в метку с использованием `Page_Load()` метода, или (для простоты) используется следующий встроенный код:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample2.aspx)]

Кроме того создается кнопка для активации, обновляя время:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample3.aspx)]

Этот код затем помещаются в `<ContentTemplate>` раздел `UpdatePanel` элемента. Панель `UpdateMode` атрибуту должно быть присвоено `"Conditional"`, поскольку только триггеры могут обновлять ее содержимого. В `<Triggers>` раздел `UpdatePanel`, создается триггер асинхронной обратной передачи и привязан к `Click` события кнопки. Таким образом, если пользователь нажимает кнопку, `UpdatePanel` не будет обновлен. Разметка для `UpdatePanel` управления:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample4.aspx)]

Наконец `UpdatePanelAnimationExtender` должны быть настроены: задать `TargetControlID` в идентификатор панели и определении анимации в модуле. Эффект постепенного увеличения делает смысле, создающий хорошо visual акцент на время обновления. Расширения разметки может выглядеть следующим образом:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample5.aspx)]

Запустите файл в браузере. Всякий раз при нажатии на кнопку на панели всегда эффект постепенного увеличения в течение одной секунды отображается текущее время.


[![Эффект постепенного увеличения текущего времени](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)

Эффект постепенного увеличения текущего времени ([Просмотр полноразмерное изображение](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](animating-an-updatepanel-control-vb.md)
