---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
title: Анимации в зависимости от условия (C#) | Документы Microsoft
author: wenz
description: Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Является ли анимации...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b7a28c0d-efb9-443a-80a4-1a5ee54671cd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
msc.type: authoredcontent
ms.openlocfilehash: b530239e76654bc68a8fa6ac900a20df1d5699b0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878572"
---
<a name="animation-depending-on-a-condition-c"></a><span data-ttu-id="02bdf-104">Анимации в зависимости от условия (C#)</span><span class="sxs-lookup"><span data-stu-id="02bdf-104">Animation Depending On a Condition (C#)</span></span>
====================
<span data-ttu-id="02bdf-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="02bdf-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="02bdf-106">[Загрузить код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="02bdf-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)</span></span>

> <span data-ttu-id="02bdf-107">Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="02bdf-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="02bdf-108">Ли анимация выполняется или не может также зависеть от условие в виде кода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="02bdf-108">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="02bdf-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="02bdf-109">Overview</span></span>

<span data-ttu-id="02bdf-110">Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="02bdf-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="02bdf-111">Ли анимация выполняется или не может также зависеть от условие в виде кода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="02bdf-111">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="02bdf-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="02bdf-112">Steps</span></span>

<span data-ttu-id="02bdf-113">Во-первых, включают `ScriptManager` страницы; затем библиотека ASP.NET AJAX загружена, что позволяет использовать набор элементов управления:</span><span class="sxs-lookup"><span data-stu-id="02bdf-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample1.aspx)]

<span data-ttu-id="02bdf-114">Анимация применяется панель текста, который выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="02bdf-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample2.aspx)]

<span data-ttu-id="02bdf-115">В связанный класс CSS для панели определяются цвет фона для работы с низким приоритетом, а также задать фиксированную ширину панели:</span><span class="sxs-lookup"><span data-stu-id="02bdf-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animation-depending-on-a-condition-cs/samples/sample3.css)]

<span data-ttu-id="02bdf-116">Затем добавьте `AnimationExtender` страницу, предоставляя `ID`, `TargetControlID` атрибута и обязательным `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="02bdf-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample4.aspx)]

<span data-ttu-id="02bdf-117">В пределах `<Animations>` узла, используйте `<OnLoad>` для выполнения анимации после полной загрузки страницы.</span><span class="sxs-lookup"><span data-stu-id="02bdf-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="02bdf-118">Вместо обычных анимацию `<Condition>` элемент вступает в действие.</span><span class="sxs-lookup"><span data-stu-id="02bdf-118">Instead of one of the regular animations, the `<Condition>` element comes into play.</span></span> <span data-ttu-id="02bdf-119">Код JavaScript, предоставленных в качестве значения для `ConditionScript` атрибут выполняется во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="02bdf-119">The JavaScript code provided as the value of the `ConditionScript` attribute is executed at runtime.</span></span> <span data-ttu-id="02bdf-120">Если значение равно true, анимация выполняется, в противном случае не.</span><span class="sxs-lookup"><span data-stu-id="02bdf-120">If it evaluates to true, the animation is executed, otherwise not.</span></span> <span data-ttu-id="02bdf-121">Приведенная ниже разметка предоставляет две анимации, каждый из них выполняется в 50% случаев после случайных.</span><span class="sxs-lookup"><span data-stu-id="02bdf-121">The following markup provides two animations, each of them being executed in 50% of cases upon random.</span></span> <span data-ttu-id="02bdf-122">Так как может существовать только одна анимация в течение `<OnLoad>`, два `<Condition>` анимации соединяются друг с другом с использованием `<Sequence>` элемента:</span><span class="sxs-lookup"><span data-stu-id="02bdf-122">Since there may only be one animation within `<OnLoad>`, the two `<Condition>` animations are joined together using the `<Sequence>` element:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample5.aspx)]

<span data-ttu-id="02bdf-123">Обратите внимание, что знак «меньше» (`<`) в `ConditionScript` атрибут должен быть escape-символом ().</span><span class="sxs-lookup"><span data-stu-id="02bdf-123">Note that the less than sign (`<`) in the `ConditionScript` attribute must be escaped ().</span></span> <span data-ttu-id="02bdf-124">При запуске этого скрипта, либо нет запусков анимации, один из двух или для обоих выполните.</span><span class="sxs-lookup"><span data-stu-id="02bdf-124">When you run this script, either no animation runs, or one of the two does, or both do.</span></span>


<span data-ttu-id="02bdf-125">[![Панель исчезновение без изменения размера, так не выполняется второй анимации, первый](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="02bdf-125">[![The panel is fading out without resizing, so the second animation runs, the first one didn't](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)</span></span>

<span data-ttu-id="02bdf-126">Панель исчезновение без изменения размера, так не выполняется второй анимации, первый ([Просмотр полноразмерное изображение](animation-depending-on-a-condition-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="02bdf-126">The panel is fading out without resizing, so the second animation runs, the first one didn't ([Click to view full-size image](animation-depending-on-a-condition-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="02bdf-127">[Назад](executing-several-animations-after-each-other-cs.md)
> [Вперед](picking-one-animation-out-of-a-list-cs.md)</span><span class="sxs-lookup"><span data-stu-id="02bdf-127">[Previous](executing-several-animations-after-each-other-cs.md)
[Next](picking-one-animation-out-of-a-list-cs.md)</span></span>
