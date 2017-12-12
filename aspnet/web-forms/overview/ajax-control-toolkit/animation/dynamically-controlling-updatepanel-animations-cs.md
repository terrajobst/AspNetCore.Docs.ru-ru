---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
title: "Динамическому управлению UpdatePanel анимации (C#) | Документы Microsoft"
author: wenz
description: "Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Для содержимого..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 5138b8fe-98ff-4e73-a00b-e263fc3ff11d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
msc.type: authoredcontent
ms.openlocfilehash: 0408556e322a46211388fbd557d7a967044129ef
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-controlling-updatepanel-animations-c"></a>Динамическому управлению UpdatePanel анимации (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)

> Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Для содержимого элемента управления UpdatePanel, существует специальные расширения, в большой степени основывается на платформе анимации: UpdatePanelAnimation. Она также может работать вместе с триггерами UpdatePanel.


## <a name="overview"></a>Обзор

Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Для содержимого `UpdatePanel`, специальные расширения существует, в большой степени основывается на платформе анимации: `UpdatePanelAnimation`. Можно также работать вместе с `UpdatePanel` триггеров.

## <a name="steps"></a>Шаги

Первым шагом является обычным образом, чтобы включить `ScriptManager` на странице, чтобы загрузить библиотеки ASP.NET AJAX и может использоваться набор элементов управления:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample1.aspx)]

Анимация в этом сценарии будет применяться для отображения текущего времени. Эти сведения могут записываться в метку с использованием `Page_Load()` метода, или (для простоты) используется следующий встроенный код:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample2.aspx)]

Кроме того создается кнопка для активации, обновляя время:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample3.aspx)]

Этот код затем помещаются в `<ContentTemplate>` раздел `UpdatePanel` элемента. Панель `UpdateMode` атрибуту должно быть присвоено `"Conditional"`, поскольку только триггеры могут обновлять ее содержимого. В `<Triggers>` раздел `UpdatePanel`, создается триггер асинхронной обратной передачи и привязан к `Click` события кнопки. Таким образом, если пользователь нажимает кнопку, `UpdatePanel` не будет обновлен. Разметка для `UpdatePanel` управления:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample4.aspx)]

Наконец `UpdatePanelAnimationExtender` должны быть настроены: задать `TargetControlID` в идентификатор панели и определении анимации в модуле. Эффект постепенного увеличения делает смысле, создающий хорошо visual акцент на время обновления. Расширения разметки может выглядеть следующим образом:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample5.aspx)]

Запустите файл в браузере. Всякий раз при нажатии на кнопку на панели всегда эффект постепенного увеличения в течение одной секунды отображается текущее время.


[![Эффект постепенного увеличения текущего времени](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)

Эффект постепенного увеличения текущего времени ([Просмотр полноразмерное изображение](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))

>[!div class="step-by-step"]
[Назад](animating-an-updatepanel-control-cs.md)
[Вперед](adding-animation-to-a-control-vb.md)
