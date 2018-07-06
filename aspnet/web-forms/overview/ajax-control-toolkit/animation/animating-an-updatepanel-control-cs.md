---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
title: Анимация элемента управления UpdatePanel (C#) | Документация Майкрософт
author: wenz
description: Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления. Для содержимого...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: e57f8c7c-3940-4bc0-9468-3a0ca69158ea
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 3214ef85938c374a8fb083bc075bd9390e37a209
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808257"
---
<a name="animating-an-updatepanel-control-c"></a><span data-ttu-id="87286-104">Анимация элемента управления UpdatePanel (C#)</span><span class="sxs-lookup"><span data-stu-id="87286-104">Animating an UpdatePanel Control (C#)</span></span>
====================
<span data-ttu-id="87286-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="87286-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="87286-106">[Скачать код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="87286-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1CS.pdf)</span></span>

> <span data-ttu-id="87286-107">Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="87286-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="87286-108">Для содержимого элемента управления UpdatePanel, существует специальные расширения, которая основывается на использовании framework анимации: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="87286-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="87286-109">Этом руководстве показано, как настроить такой анимации для элемента управления UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="87286-109">This tutorial shows how to set up such an animation for an UpdatePanel.</span></span>


## <a name="overview"></a><span data-ttu-id="87286-110">Обзор</span><span class="sxs-lookup"><span data-stu-id="87286-110">Overview</span></span>

<span data-ttu-id="87286-111">Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="87286-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="87286-112">Для содержимого `UpdatePanel`, существует специальные расширения, которая основывается на использовании framework анимации: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="87286-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="87286-113">Этом руководстве показано, как настроить такой анимацию для `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="87286-113">This tutorial shows how to set up such an animation for an `UpdatePanel`.</span></span>

## <a name="steps"></a><span data-ttu-id="87286-114">Шаги</span><span class="sxs-lookup"><span data-stu-id="87286-114">Steps</span></span>

<span data-ttu-id="87286-115">Первым шагом является обычным образом, чтобы включить `ScriptManager` на странице, ASP.NET AJAX library загружается и может использоваться набор элементов управления:</span><span class="sxs-lookup"><span data-stu-id="87286-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample1.aspx)]

<span data-ttu-id="87286-116">Анимация в этом сценарии будет применяться к ASP.NET `Wizard` веб-элемента управления, находящихся в `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="87286-116">The animation in this scenario will be applied to an ASP.NET `Wizard` web control residing in an `UpdatePanel`.</span></span> <span data-ttu-id="87286-117">Три шага (произвольному) предоставляют достаточно возможности для запуска операций обратной передачи.</span><span class="sxs-lookup"><span data-stu-id="87286-117">Three (arbitrary) steps provide enough options to trigger postbacks:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample2.aspx)]

<span data-ttu-id="87286-118">Разметка, необходимые для `UpdatePanelAnimationExtender` управления очень похож на разметку, используемую для `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="87286-118">The markup necessary for the `UpdatePanelAnimationExtender` control is quite similar to the markup used for the `AnimationExtender`.</span></span> <span data-ttu-id="87286-119">В `TargetControlID` атрибут, мы предоставляем `ID` из `UpdatePanel` для анимации; в `UpdatePanelAnimationExtender` элемента управления, `<Animations>` элемент содержит XML-разметку для анимации.</span><span class="sxs-lookup"><span data-stu-id="87286-119">In the `TargetControlID` attribute we provide the `ID` of the `UpdatePanel` to animate; within the `UpdatePanelAnimationExtender` control, the `<Animations>` element holds XML markup for the animation(s).</span></span> <span data-ttu-id="87286-120">Однако есть одно различие: объем события и обработчики событий ограничено по сравнению с `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="87286-120">However there is one difference: The amount of events and event handlers is limited in comparison to `AnimationExtender`.</span></span> <span data-ttu-id="87286-121">Для `UpdatePanels`, только два из них существует:</span><span class="sxs-lookup"><span data-stu-id="87286-121">For `UpdatePanels`, only two of them exist:</span></span>

- <span data-ttu-id="87286-122">`<OnUpdated>` Когда будет обновлена UpdatePanel</span><span class="sxs-lookup"><span data-stu-id="87286-122">`<OnUpdated>` when the UpdatePanel has been updated</span></span>
- <span data-ttu-id="87286-123">`<OnUpdating>` Когда UpdatePanel начинает обновление</span><span class="sxs-lookup"><span data-stu-id="87286-123">`<OnUpdating>` when the UpdatePanel starts updating</span></span>

<span data-ttu-id="87286-124">В этом случае новый содержимое `UpdatePanel` (после обратной отправки) должны появление.</span><span class="sxs-lookup"><span data-stu-id="87286-124">In this scenario, the new content of the `UpdatePanel` (after the postback) shall fade in.</span></span> <span data-ttu-id="87286-125">Это необходимые средства разметки для этого:</span><span class="sxs-lookup"><span data-stu-id="87286-125">This is the necessary markup for that:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample3.aspx)]

<span data-ttu-id="87286-126">Теперь всякий раз, когда происходит обратная передача внутри UpdatePanel, новое содержимое панели Плавное.</span><span class="sxs-lookup"><span data-stu-id="87286-126">Now whenever a postback occurs within the UpdatePanel, the new contents of the panel fade in smoothly.</span></span>


<span data-ttu-id="87286-127">[![Следующий шаг мастера является плавный переход](animating-an-updatepanel-control-cs/_static/image2.png)](animating-an-updatepanel-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="87286-127">[![The next wizard step is fading in](animating-an-updatepanel-control-cs/_static/image2.png)](animating-an-updatepanel-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="87286-128">Следующий шаг мастера является плавный переход ([Просмотр полноразмерного изображения](animating-an-updatepanel-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="87286-128">The next wizard step is fading in ([Click to view full-size image](animating-an-updatepanel-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="87286-129">[Назад](changing-an-animation-using-client-side-code-cs.md)
> [Вперед](dynamically-controlling-updatepanel-animations-cs.md)</span><span class="sxs-lookup"><span data-stu-id="87286-129">[Previous](changing-an-animation-using-client-side-code-cs.md)
[Next](dynamically-controlling-updatepanel-animations-cs.md)</span></span>
