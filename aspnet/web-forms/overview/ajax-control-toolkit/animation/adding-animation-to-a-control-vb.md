---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
title: Добавление анимации в элемент управления (Visual Basic) | Документы Microsoft
author: wenz
description: Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. В этом учебнике показано как...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c120187e-963e-4439-bb85-32771bc7f1f4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 3da98e478c45213875b3829e51351d03571a05b8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="adding-animation-to-a-control-vb"></a><span data-ttu-id="26e6a-104">Добавление анимации в элемент управления (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="26e6a-104">Adding Animation to a Control (VB)</span></span>
====================
<span data-ttu-id="26e6a-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="26e6a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="26e6a-106">[Загрузить код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="26e6a-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)</span></span>

> <span data-ttu-id="26e6a-107">Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="26e6a-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="26e6a-108">Этого учебника показано, как настроить такие анимации.</span><span class="sxs-lookup"><span data-stu-id="26e6a-108">This tutorial shows how to set up such an animation.</span></span>


## <a name="overview"></a><span data-ttu-id="26e6a-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="26e6a-109">Overview</span></span>

<span data-ttu-id="26e6a-110">Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="26e6a-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="26e6a-111">Этого учебника показано, как настроить такие анимации.</span><span class="sxs-lookup"><span data-stu-id="26e6a-111">This tutorial shows how to set up such an animation.</span></span>

## <a name="steps"></a><span data-ttu-id="26e6a-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="26e6a-112">Steps</span></span>

<span data-ttu-id="26e6a-113">Первым шагом является обычным образом, чтобы включить `ScriptManager` на странице, чтобы загрузить библиотеки ASP.NET AJAX и может использоваться набор элементов управления:</span><span class="sxs-lookup"><span data-stu-id="26e6a-113">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample1.aspx)]

<span data-ttu-id="26e6a-114">Анимации в этом сценарии будет применяться к панель текста, который выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="26e6a-114">The animation in this scenario will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample2.aspx)]

<span data-ttu-id="26e6a-115">Связанный класс CSS для панели определяет цвет фона ширины и высоты.</span><span class="sxs-lookup"><span data-stu-id="26e6a-115">The associated CSS class for the panel defines a background color and a width:</span></span>

[!code-css[Main](adding-animation-to-a-control-vb/samples/sample3.css)]

<span data-ttu-id="26e6a-116">Далее, нам нужно `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="26e6a-116">Next up, we need the `AnimationExtender`.</span></span> <span data-ttu-id="26e6a-117">После предоставления `ID` и обычные `runat="server"`, `TargetControlID` атрибута должно быть присвоено элемента управления для анимации в нашем случае панели:</span><span class="sxs-lookup"><span data-stu-id="26e6a-117">After providing an `ID` and the usual `runat="server"`, the `TargetControlID` attribute must be set to the control to animate in our case, the panel:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample4.aspx)]

<span data-ttu-id="26e6a-118">Всего применяется анимация декларативно, используя синтаксис XML, к сожалению в настоящее время не полностью поддерживается технологией IntelliSense в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="26e6a-118">The whole animation is applied declaratively, using an XML syntax, unfortunately currently not fully supported by Visual Studio's IntelliSense.</span></span> <span data-ttu-id="26e6a-119">Корневой узел — `<Animations>;` в этом узле допускается несколько событий определить, когда анимаций take(s) месте:</span><span class="sxs-lookup"><span data-stu-id="26e6a-119">The root node is `<Animations>;` within this node, several events are allowed which determine when the animation(s) take(s) place:</span></span>

- <span data-ttu-id="26e6a-120">`OnClick` (щелчок мыши)</span><span class="sxs-lookup"><span data-stu-id="26e6a-120">`OnClick` (mouse click)</span></span>
- <span data-ttu-id="26e6a-121">`OnHoverOut` (когда указатель мыши покидает элемент управления)</span><span class="sxs-lookup"><span data-stu-id="26e6a-121">`OnHoverOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="26e6a-122">`OnHoverOver` (при наведении курсора мыши на элемент управления, остановка `OnHoverOut` анимации)</span><span class="sxs-lookup"><span data-stu-id="26e6a-122">`OnHoverOver` (when the mouse hovers over a control, stopping the `OnHoverOut` animation)</span></span>
- <span data-ttu-id="26e6a-123">`OnLoad` (если страница загрузки)</span><span class="sxs-lookup"><span data-stu-id="26e6a-123">`OnLoad` (when the page has been loaded)</span></span>
- <span data-ttu-id="26e6a-124">`OnMouseOut` (когда указатель мыши покидает элемент управления)</span><span class="sxs-lookup"><span data-stu-id="26e6a-124">`OnMouseOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="26e6a-125">`OnMouseOver` (при наведении курсора мыши на элемент управления, не останавливая `OnMouseOut` анимации)</span><span class="sxs-lookup"><span data-stu-id="26e6a-125">`OnMouseOver` (when the mouse hovers over a control, not stopping the `OnMouseOut` animation)</span></span>

<span data-ttu-id="26e6a-126">Платформа поставляется с набором анимации, каждый из представленного собственным XML-элементом.</span><span class="sxs-lookup"><span data-stu-id="26e6a-126">The framework comes with a set of animations, each one represented by its own XML element.</span></span> <span data-ttu-id="26e6a-127">Ниже приведен фрагмент.</span><span class="sxs-lookup"><span data-stu-id="26e6a-127">Here is a selection:</span></span>

- <span data-ttu-id="26e6a-128">`<Color>` (изменить цвета)</span><span class="sxs-lookup"><span data-stu-id="26e6a-128">`<Color>` (changing a color)</span></span>
- <span data-ttu-id="26e6a-129">`<FadeIn>` (затухание в)</span><span class="sxs-lookup"><span data-stu-id="26e6a-129">`<FadeIn>` (fading in)</span></span>
- <span data-ttu-id="26e6a-130">`<FadeOut>` (исчезновение)</span><span class="sxs-lookup"><span data-stu-id="26e6a-130">`<FadeOut>` (fading out)</span></span>
- <span data-ttu-id="26e6a-131">`<Property>` (изменение свойства элемента управления)</span><span class="sxs-lookup"><span data-stu-id="26e6a-131">`<Property>` (changing a control's property)</span></span>
- <span data-ttu-id="26e6a-132">`<Pulse>` (pulsating)</span><span class="sxs-lookup"><span data-stu-id="26e6a-132">`<Pulse>` (pulsating)</span></span>
- <span data-ttu-id="26e6a-133">`<Resize>` (изменение размера)</span><span class="sxs-lookup"><span data-stu-id="26e6a-133">`<Resize>` (changing the size)</span></span>
- <span data-ttu-id="26e6a-134">`<Scale>` (пропорционально изменять размер)</span><span class="sxs-lookup"><span data-stu-id="26e6a-134">`<Scale>` (proportionally changing the size)</span></span>

<span data-ttu-id="26e6a-135">В этом примере панели должны Исчезание. Анимация должна осуществляться 1,5 секунды (`Duration` атрибут), отображение 24 (шаги анимации) кадров в секунду (`Fps` attributs).</span><span class="sxs-lookup"><span data-stu-id="26e6a-135">In this example, the panel shall fade out. The animation shall take 1.5 seconds (`Duration` attribute), displaying 24 frames (animation steps) per second (`Fps` attributs).</span></span> <span data-ttu-id="26e6a-136">Вот полную разметку для `AnimationExtender` управления:</span><span class="sxs-lookup"><span data-stu-id="26e6a-136">Here is the complete markup for the `AnimationExtender` control:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample5.aspx)]

<span data-ttu-id="26e6a-137">При запуске этого скрипта панели отображается и отчетливее Полуторный секунд.</span><span class="sxs-lookup"><span data-stu-id="26e6a-137">When you run this script, the panel is displayed and fades out in one and a half seconds.</span></span>


<span data-ttu-id="26e6a-138">[![Исчезновение панели](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="26e6a-138">[![The panel is fading out](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="26e6a-139">Исчезновение панели ([Просмотр полноразмерное изображение](adding-animation-to-a-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="26e6a-139">The panel is fading out ([Click to view full-size image](adding-animation-to-a-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="26e6a-140">[Назад](dynamically-controlling-updatepanel-animations-cs.md)
> [Вперед](executing-several-animations-at-the-same-time-vb.md)</span><span class="sxs-lookup"><span data-stu-id="26e6a-140">[Previous](dynamically-controlling-updatepanel-animations-cs.md)
[Next](executing-several-animations-at-the-same-time-vb.md)</span></span>
