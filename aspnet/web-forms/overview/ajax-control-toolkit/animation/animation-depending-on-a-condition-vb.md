---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
title: Анимация в зависимости от условия (VB) | Документация Майкрософт
author: wenz
description: Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления. Является ли анимация...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 1b87d8d6-b3f7-4126-b51c-d41442fbf947
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
msc.type: authoredcontent
ms.openlocfilehash: 4b815d7000d94ceec8864b29f0dddf0171344de1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836292"
---
<a name="animation-depending-on-a-condition-vb"></a><span data-ttu-id="e468b-104">Анимация в зависимости от условия (VB)</span><span class="sxs-lookup"><span data-stu-id="e468b-104">Animation Depending On a Condition (VB)</span></span>
====================
<span data-ttu-id="e468b-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e468b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e468b-106">[Скачать код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="e468b-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)</span></span>

> <span data-ttu-id="e468b-107">Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="e468b-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="e468b-108">Запускается ли анимация или не может также зависеть условие в виде кода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e468b-108">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="e468b-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="e468b-109">Overview</span></span>

<span data-ttu-id="e468b-110">Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="e468b-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="e468b-111">Запускается ли анимация или не может также зависеть условие в виде кода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e468b-111">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="e468b-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="e468b-112">Steps</span></span>

<span data-ttu-id="e468b-113">Во-первых, включите `ScriptManager` страницы; затем ASP.NET AJAX library загружена, что позволяет использовать набор средств управления:</span><span class="sxs-lookup"><span data-stu-id="e468b-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample1.aspx)]

<span data-ttu-id="e468b-114">Анимация будет применяться к панели текста, который выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="e468b-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample2.aspx)]

<span data-ttu-id="e468b-115">В связанный класс CSS для панели определить цвет фона, удобная и также установить фиксированную ширину для панели:</span><span class="sxs-lookup"><span data-stu-id="e468b-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animation-depending-on-a-condition-vb/samples/sample3.css)]

<span data-ttu-id="e468b-116">Затем добавьте `AnimationExtender` на страницу, предоставляя `ID`, `TargetControlID` атрибут и обязательным `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="e468b-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample4.aspx)]

<span data-ttu-id="e468b-117">В рамках `<Animations>` узла, используйте `<OnLoad>` для запуска анимации после полной загрузки страницы.</span><span class="sxs-lookup"><span data-stu-id="e468b-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="e468b-118">Вместо одного из обычных анимаций `<Condition>` элемент вступает в действие.</span><span class="sxs-lookup"><span data-stu-id="e468b-118">Instead of one of the regular animations, the `<Condition>` element comes into play.</span></span> <span data-ttu-id="e468b-119">Код JavaScript, предоставленных в качестве значения из `ConditionScript` атрибут выполняется во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="e468b-119">The JavaScript code provided as the value of the `ConditionScript` attribute is executed at runtime.</span></span> <span data-ttu-id="e468b-120">Если значение равно true, анимация выполняется, в противном случае не.</span><span class="sxs-lookup"><span data-stu-id="e468b-120">If it evaluates to true, the animation is executed, otherwise not.</span></span> <span data-ttu-id="e468b-121">Следующая разметка предоставляет две анимации, каждый из них выполняется в 50% случаев после случайных.</span><span class="sxs-lookup"><span data-stu-id="e468b-121">The following markup provides two animations, each of them being executed in 50% of cases upon random.</span></span> <span data-ttu-id="e468b-122">Так как может существовать только одна анимация в `<OnLoad>`, два `<Condition>` анимации соединяются друг с другом с использованием `<Sequence>` элемент:</span><span class="sxs-lookup"><span data-stu-id="e468b-122">Since there may only be one animation within `<OnLoad>`, the two `<Condition>` animations are joined together using the `<Sequence>` element:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample5.aspx)]

<span data-ttu-id="e468b-123">Обратите внимание, что знак «меньше» (`<`) в `ConditionScript` атрибут должен быть escape-().</span><span class="sxs-lookup"><span data-stu-id="e468b-123">Note that the less than sign (`<`) in the `ConditionScript` attribute must be escaped ().</span></span> <span data-ttu-id="e468b-124">При запуске этого скрипта, либо нет запусков анимации, один из двух или для обоих сделать.</span><span class="sxs-lookup"><span data-stu-id="e468b-124">When you run this script, either no animation runs, or one of the two does, or both do.</span></span>


<span data-ttu-id="e468b-125">[![Панели исчезновение без изменения размера, поэтому второй анимации запуски, первый из них не](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e468b-125">[![The panel is fading out without resizing, so the second animation runs, the first one didn't](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)</span></span>

<span data-ttu-id="e468b-126">Панели исчезновение без изменения размера, поэтому второй анимации запуски, первый из них не ([Просмотр полноразмерного изображения](animation-depending-on-a-condition-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e468b-126">The panel is fading out without resizing, so the second animation runs, the first one didn't ([Click to view full-size image](animation-depending-on-a-condition-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e468b-127">[Назад](executing-several-animations-after-each-other-vb.md)
> [Вперед](picking-one-animation-out-of-a-list-vb.md)</span><span class="sxs-lookup"><span data-stu-id="e468b-127">[Previous](executing-several-animations-after-each-other-vb.md)
[Next](picking-one-animation-out-of-a-list-vb.md)</span></span>
