---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
title: Выполнение нескольких анимации в то же время (C#) | Документы Microsoft
author: wenz
description: Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Он позволяет выполнять severa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 219149e1-3ee9-4b79-8fe4-7433f6b7d15b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
msc.type: authoredcontent
ms.openlocfilehash: 6ecd822f7fa00a24e97b9aa14888798825624617
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869365"
---
<a name="executing-several-animations-at-the-same-time-c"></a>Выполнение нескольких анимации в то же время (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)

> Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Это позволяет запустить несколько анимаций параллельных образом.


## <a name="overview"></a>Обзор

Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Это позволяет запустить несколько анимаций параллельных образом.

## <a name="steps"></a>Шаги

Во-первых, включают `ScriptManager` страницы; затем библиотека ASP.NET AJAX загружена, что позволяет использовать набор элементов управления:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample1.aspx)]

Анимация применяется панель текста, который выглядит следующим образом:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample2.aspx)]

В связанный класс CSS для панели определяются цвет фона для работы с низким приоритетом, а также задать фиксированную ширину панели:

[!code-css[Main](executing-several-animations-at-the-same-time-cs/samples/sample3.css)]

Затем добавьте `AnimationExtender` страницу, предоставляя `ID`, `TargetControlID` атрибута и обязательным `runat="server"`:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample4.aspx)]

В пределах `<Animations>` узла, используйте `<OnLoad>` для выполнения анимации после полной загрузки страницы. Как правило `<OnLoad>` принимает только одна анимация. Платформа анимации позволяет объединить несколько анимаций в один с помощью `<Parallel>` элемента. Все анимации в `<Parallel>` выполняются одновременно.

Вот возможные разметку для `AnimationExtender` управления исчезновение и изменение размера панели в то же время:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample5.aspx)]

И на самом деле: при выполнении этого скрипта панели отображается, а затем изменяет размер (больше, чем threefolding высоты и halfing его высота) и Исчезание, в то же время.


[![Исчезновение и изменение размера (в том числе его содержимого, благодаря механизм визуализации в браузере) панели](executing-several-animations-at-the-same-time-cs/_static/image2.png)](executing-several-animations-at-the-same-time-cs/_static/image1.png)

Исчезновение и изменение размера (в том числе его содержимого, благодаря механизм визуализации в браузере) панели ([Просмотр полноразмерное изображение](executing-several-animations-at-the-same-time-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](adding-animation-to-a-control-cs.md)
> [Вперед](executing-several-animations-after-each-other-cs.md)
