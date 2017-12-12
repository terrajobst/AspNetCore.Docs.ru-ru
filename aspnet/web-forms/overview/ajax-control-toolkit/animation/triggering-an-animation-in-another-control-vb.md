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
<a name="triggering-an-animation-in-another-control-vb"></a><span data-ttu-id="58bed-104">Запуск анимации в другой элемент управления (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="58bed-104">Triggering an Animation in another Control (VB)</span></span>
====================
<span data-ttu-id="58bed-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="58bed-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="58bed-106">[Загрузить код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="58bed-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)</span></span>

> <span data-ttu-id="58bed-107">Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="58bed-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="58bed-108">Как правило запуск анимации инициируется взаимодействие пользователя с тем же элементом управления.</span><span class="sxs-lookup"><span data-stu-id="58bed-108">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="58bed-109">Однако также возможность взаимодействия с одного элемента управления, а затем анимации другого элемента управления.</span><span class="sxs-lookup"><span data-stu-id="58bed-109">It is however also possible to interact with one control and then animation another control.</span></span>


## <a name="overview"></a><span data-ttu-id="58bed-110">Обзор</span><span class="sxs-lookup"><span data-stu-id="58bed-110">Overview</span></span>

<span data-ttu-id="58bed-111">Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="58bed-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="58bed-112">Как правило запуск анимации инициируется взаимодействие пользователя с тем же элементом управления.</span><span class="sxs-lookup"><span data-stu-id="58bed-112">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="58bed-113">Однако также возможность взаимодействия с одного элемента управления, а затем анимации другого элемента управления.</span><span class="sxs-lookup"><span data-stu-id="58bed-113">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="steps"></a><span data-ttu-id="58bed-114">Шаги</span><span class="sxs-lookup"><span data-stu-id="58bed-114">Steps</span></span>

<span data-ttu-id="58bed-115">Во-первых, включают `ScriptManager` страницы; затем библиотека ASP.NET AJAX загружена, что позволяет использовать набор элементов управления:</span><span class="sxs-lookup"><span data-stu-id="58bed-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample1.aspx)]

<span data-ttu-id="58bed-116">Анимация применяется панель текста, который выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="58bed-116">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample2.aspx)]

<span data-ttu-id="58bed-117">В связанный класс CSS для панели определяются цвет фона для работы с низким приоритетом, а также задать фиксированную ширину панели:</span><span class="sxs-lookup"><span data-stu-id="58bed-117">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](triggering-an-animation-in-another-control-vb/samples/sample3.css)]

<span data-ttu-id="58bed-118">Чтобы запустить панель анимации, используется HTML-кнопка.</span><span class="sxs-lookup"><span data-stu-id="58bed-118">In order to start animating the panel, an HTML button is used.</span></span> <span data-ttu-id="58bed-119">Обратите внимание, что `<input type="button" />` favoured над `<asp:Button />` поскольку мы не хотим обратную передачу при нажатии этой кнопки.</span><span class="sxs-lookup"><span data-stu-id="58bed-119">Note that `<input type="button" />` is favoured over `<asp:Button />` since we do not want a postback when the user clicks on that button.</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample4.aspx)]

<span data-ttu-id="58bed-120">Затем добавьте `AnimationExtender` страницу, предоставляя `ID`, `TargetControlID` атрибута и обязательным `runat="server"`.</span><span class="sxs-lookup"><span data-stu-id="58bed-120">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`.</span></span> <span data-ttu-id="58bed-121">Очень важно задать `TargetControlID` идентификатору кнопки (элемент, активируя анимации) не идентификатору панели (анимируемого элемента)</span><span class="sxs-lookup"><span data-stu-id="58bed-121">It is important to set `TargetControlID` to the ID of the button (the element triggering the animation), not to the ID of the panel (the element being animated)</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample5.aspx)]

<span data-ttu-id="58bed-122">В пределах `<Animations>` узел анимации месте как обычно.</span><span class="sxs-lookup"><span data-stu-id="58bed-122">Within the `<Animations>` node, place animations as usual.</span></span> <span data-ttu-id="58bed-123">Чтобы их изменить на панели, не кнопку задать `AnimationTarget` атрибут для каждого элемента анимации в `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="58bed-123">In order to make them change the panel, not the button, set the `AnimationTarget` attribute for every animation element within `AnimationExtender`.</span></span> <span data-ttu-id="58bed-124">Значение для `AnimationTarget` — идентификатор панели, конечно.</span><span class="sxs-lookup"><span data-stu-id="58bed-124">The value for `AnimationTarget` is the ID of the panel, of course.</span></span> <span data-ttu-id="58bed-125">Таким образом, анимаций произойти, если панель не с помощью активации кнопки.</span><span class="sxs-lookup"><span data-stu-id="58bed-125">That way, the animations happen with the panel, not with the triggering button.</span></span> <span data-ttu-id="58bed-126">Вот `AnimationExtender` разметки для этого сценария:</span><span class="sxs-lookup"><span data-stu-id="58bed-126">Here is the `AnimationExtender` markup for this scenario:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample6.aspx)]

<span data-ttu-id="58bed-127">Обратите внимание на специальные порядок, в котором отображаются отдельные анимации.</span><span class="sxs-lookup"><span data-stu-id="58bed-127">Note the special order in which the individual animations appear.</span></span> <span data-ttu-id="58bed-128">Во-первых кнопки возвращает отключена после ее воспроизведения.</span><span class="sxs-lookup"><span data-stu-id="58bed-128">First of all, the button gets deactivated once the animation runs.</span></span> <span data-ttu-id="58bed-129">Так как она не `AnimationTarget` атрибута в `<EnableAction>` элемент анимации применяется к исходного управления: кнопки.</span><span class="sxs-lookup"><span data-stu-id="58bed-129">Since there is no `AnimationTarget` attribute in the `<EnableAction>` element, this animation is applied to the originating control: the button.</span></span> <span data-ttu-id="58bed-130">Анимация следующие два действия должны выполняться parallelly (`<Parallel>` элемент).</span><span class="sxs-lookup"><span data-stu-id="58bed-130">The next two animation steps shall be carried out parallelly (`<Parallel>` element).</span></span> <span data-ttu-id="58bed-131">Оба имеют их `AnimationTarget` атрибуты значения `"Panel1"`, таким образом анимации панели, а не кнопки.</span><span class="sxs-lookup"><span data-stu-id="58bed-131">Both have their `AnimationTarget` attributes set to `"Panel1"`, thus animating the panel, not the button.</span></span>


<span data-ttu-id="58bed-132">[![Нажмите кнопку мыши запуск анимации панели](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="58bed-132">[![A mouse click on the button starts the panel animation](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="58bed-133">Нажмите кнопку мыши запуск анимации панель ([Просмотр полноразмерное изображение](triggering-an-animation-in-another-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="58bed-133">A mouse click on the button starts the panel animation ([Click to view full-size image](triggering-an-animation-in-another-control-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="58bed-134">[Назад](disabling-actions-during-animation-vb.md)
[Вперед](modifying-animations-from-the-server-side-vb.md)</span><span class="sxs-lookup"><span data-stu-id="58bed-134">[Previous](disabling-actions-during-animation-vb.md)
[Next](modifying-animations-from-the-server-side-vb.md)</span></span>
