---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
title: Анимация элемента управления UpdatePanel (VB) | Документы Microsoft
author: wenz
description: Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Для содержимого...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4c306a2c-92b6-4904-b70b-365b847334fe
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 2c1114b74fd152a4ea85aa10850860f75573adee
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873161"
---
<a name="animating-an-updatepanel-control-vb"></a>Анимация элемента управления UpdatePanel (Visual Basic)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)

> Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Для содержимого элемента управления UpdatePanel, существует специальные расширения, в большой степени основывается на платформе анимации: UpdatePanelAnimation. Этот учебник показывает, как для настройки таких анимации для элемента управления UpdatePanel.


## <a name="overview"></a>Обзор

Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Для содержимого `UpdatePanel`, специальные расширения существует, в большой степени основывается на платформе анимации: `UpdatePanelAnimation`. Этого учебника показано, как настроить такие анимацию для `UpdatePanel`.

## <a name="steps"></a>Шаги

Первым шагом является обычным образом, чтобы включить `ScriptManager` на странице, чтобы загрузить библиотеки ASP.NET AJAX и может использоваться набор элементов управления:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample1.aspx)]

Анимация в этом сценарии будет применяться к ASP.NET `Wizard` веб-элемента управления, находящегося в `UpdatePanel`. Недостаточно параметров для запуска обратных передач перечислены три шаги (произвольный).

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample2.aspx)]

Разметка, необходимые для `UpdatePanelAnimationExtender` очень похожа на разметку, используемую для управления `AnimationExtender`. В `TargetControlID` мы предоставляем атрибут `ID` из `UpdatePanel` для анимации; в `UpdatePanelAnimationExtender` управления `<Animations>` элемент содержит XML-разметку для анимаций. Однако есть одно различие: объем события и обработчики событий ограничен по сравнению с `AnimationExtender`. Для `UpdatePanels`, только два из них существует:

- `<OnUpdated>` При обновлении UpdatePanel
- `<OnUpdating>` Когда UpdatePanel начинается обновление

В этом случае новое содержимое `UpdatePanel` (после обратной передачи) должны появление. Это необходимой разметки для этого:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample3.aspx)]

Теперь каждый раз, когда происходит обратная передача в UpdatePanel, новое содержимое панели Плавное.


[![Следующий шаг мастера эффект постепенного увеличения](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)

Следующий шаг мастера эффект постепенного увеличения ([Просмотр полноразмерное изображение](animating-an-updatepanel-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](changing-an-animation-using-client-side-code-vb.md)
> [Вперед](dynamically-controlling-updatepanel-animations-vb.md)
