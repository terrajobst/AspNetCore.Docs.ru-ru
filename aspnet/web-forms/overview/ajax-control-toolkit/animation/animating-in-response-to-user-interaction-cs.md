---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
title: "Анимация в ответ на взаимодействие пользователя (C#) | Документы Microsoft"
author: wenz
description: "Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Анимации можно звездочек..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ea26549d-fbbf-4973-a108-b14cd1d6de26
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
msc.type: authoredcontent
ms.openlocfilehash: efb9c34c317ec56b43c498f40a857a9b47fa50b2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="animating-in-response-to-user-interaction-c"></a>Анимация в ответ на взаимодействие пользователя (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)

> Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Анимации можно запускать автоматически или могут быть предприняты взаимодействием с пользователем, например, щелкнув мышью.


## <a name="overview"></a>Обзор

Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Анимации можно запускать автоматически или могут быть предприняты взаимодействием с пользователем, например, щелкнув мышью.

## <a name="steps"></a>Шаги

Во-первых, включают `ScriptManager` страницы; затем библиотека ASP.NET AJAX загружена, что позволяет использовать набор элементов управления:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample1.aspx)]

Анимация применяется панель текста, который выглядит следующим образом:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample2.aspx)]

В связанный класс CSS для панели определяются цвет фона для работы с низким приоритетом, а также задать фиксированную ширину панели:

[!code-css[Main](animating-in-response-to-user-interaction-cs/samples/sample3.css)]

Затем добавьте `AnimationExtender` страницу, предоставляя `ID`, `TargetControlID` атрибута и обязательным `runat="server"`:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample4.aspx)]

В пределах `<Animations>` узла существует пять способов запуска анимации через взаимодействие с пользователем (отсутствует элемент `<OnLoad>` которое выполняется после полной загрузки страницы в целом):

- `<OnClick>`(щелкните мышью элемент управления)
- `<OnHoverOut>`(указатель мыши покидает элемент управления)
- `<OnHoverOver>`(указатель мыши находится над элементом управления, остановка `<OnHoverOut>` анимации)
- `<OnMouseOut>`(указатель мыши покидает элемент управления)
- `<OnMouseOver>`(указатель мыши находится над элементом управления, не останавливая `<OnMouseOut>` анимации)

В этом сценарии `<OnClick>` используется. При нажатии на панели, он изменяется и Исчезание, в то же время.

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample5.aspx)]


[![Эффект запускается щелчка кнопкой мыши](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)

Щелчок мыши запуск анимации ([Просмотр полноразмерное изображение](animating-in-response-to-user-interaction-cs/_static/image3.png))

>[!div class="step-by-step"]
[Назад](picking-one-animation-out-of-a-list-cs.md)
[Вперед](disabling-actions-during-animation-cs.md)
