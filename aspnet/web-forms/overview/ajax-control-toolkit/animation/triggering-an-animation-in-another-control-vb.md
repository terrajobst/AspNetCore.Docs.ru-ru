---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
title: "Запуск анимации в другой элемент управления (Visual Basic) | Документы Microsoft"
author: wenz
description: "Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Как правило, запуск..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 25ebaf1f-5a9f-423d-98c7-1d694e93664f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
msc.type: authoredcontent
ms.openlocfilehash: ce1d29cbd06ef8a470780ff4c7bda8039575d59f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="triggering-an-animation-in-another-control-vb"></a>Запуск анимации в другой элемент управления (Visual Basic)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)

> Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Как правило запуск анимации инициируется взаимодействие пользователя с тем же элементом управления. Однако также возможность взаимодействия с одного элемента управления, а затем анимации другого элемента управления.


## <a name="overview"></a>Обзор

Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Как правило запуск анимации инициируется взаимодействие пользователя с тем же элементом управления. Однако также возможность взаимодействия с одного элемента управления, а затем анимации другого элемента управления.

## <a name="steps"></a>Шаги

Во-первых, включают `ScriptManager` страницы; затем библиотека ASP.NET AJAX загружена, что позволяет использовать набор элементов управления:

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample1.aspx)]

Анимация применяется панель текста, который выглядит следующим образом:

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample2.aspx)]

В связанный класс CSS для панели определяются цвет фона для работы с низким приоритетом, а также задать фиксированную ширину панели:

[!code-css[Main](triggering-an-animation-in-another-control-vb/samples/sample3.css)]

Чтобы запустить панель анимации, используется HTML-кнопка. Обратите внимание, что `<input type="button" />` favoured над `<asp:Button />` поскольку мы не хотим обратную передачу при нажатии этой кнопки.

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample4.aspx)]

Затем добавьте `AnimationExtender` страницу, предоставляя `ID`, `TargetControlID` атрибута и обязательным `runat="server"`. Очень важно задать `TargetControlID` идентификатору кнопки (элемент, активируя анимации) не идентификатору панели (анимируемого элемента)

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample5.aspx)]

В пределах `<Animations>` узел анимации месте как обычно. Чтобы их изменить на панели, не кнопку задать `AnimationTarget` атрибут для каждого элемента анимации в `AnimationExtender`. Значение для `AnimationTarget` — идентификатор панели, конечно. Таким образом, анимаций произойти, если панель не с помощью активации кнопки. Вот `AnimationExtender` разметки для этого сценария:

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample6.aspx)]

Обратите внимание на специальные порядок, в котором отображаются отдельные анимации. Во-первых кнопки возвращает отключена после ее воспроизведения. Так как она не `AnimationTarget` атрибута в `<EnableAction>` элемент анимации применяется к исходного управления: кнопки. Анимация следующие два действия должны выполняться parallelly (`<Parallel>` элемент). Оба имеют их `AnimationTarget` атрибуты значения `"Panel1"`, таким образом анимации панели, а не кнопки.


[![Нажмите кнопку мыши запуск анимации панели](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)

Нажмите кнопку мыши запуск анимации панель ([Просмотр полноразмерное изображение](triggering-an-animation-in-another-control-vb/_static/image3.png))

>[!div class="step-by-step"]
[Назад](disabling-actions-during-animation-vb.md)
[Вперед](modifying-animations-from-the-server-side-vb.md)
