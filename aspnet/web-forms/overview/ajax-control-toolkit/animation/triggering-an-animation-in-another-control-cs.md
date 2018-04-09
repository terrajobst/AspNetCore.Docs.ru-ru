---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: Запуск анимации в другом элементе управления (C#) | Документы Microsoft
author: wenz
description: Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Как правило, запуск...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: e94046ca70607e37c1b5ef57d5cedef67a236b94
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="triggering-an-animation-in-another-control-c"></a><span data-ttu-id="8ecdc-104">Запуск анимации в другом элементе управления (C#)</span><span class="sxs-lookup"><span data-stu-id="8ecdc-104">Triggering an Animation in another Control (C#)</span></span>
====================
<span data-ttu-id="8ecdc-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8ecdc-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8ecdc-106">[Загрузить код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="8ecdc-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)</span></span>

> <span data-ttu-id="8ecdc-107">Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="8ecdc-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="8ecdc-108">Как правило запуск анимации инициируется взаимодействие пользователя с тем же элементом управления.</span><span class="sxs-lookup"><span data-stu-id="8ecdc-108">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="8ecdc-109">Однако также возможность взаимодействия с одного элемента управления, а затем анимации другого элемента управления.</span><span class="sxs-lookup"><span data-stu-id="8ecdc-109">It is however also possible to interact with one control and then animation another control.</span></span>


## <a name="overview"></a><span data-ttu-id="8ecdc-110">Обзор</span><span class="sxs-lookup"><span data-stu-id="8ecdc-110">Overview</span></span>

<span data-ttu-id="8ecdc-111">Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="8ecdc-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="8ecdc-112">Как правило запуск анимации инициируется взаимодействие пользователя с тем же элементом управления.</span><span class="sxs-lookup"><span data-stu-id="8ecdc-112">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="8ecdc-113">Однако также возможность взаимодействия с одного элемента управления, а затем анимации другого элемента управления.</span><span class="sxs-lookup"><span data-stu-id="8ecdc-113">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="steps"></a><span data-ttu-id="8ecdc-114">Шаги</span><span class="sxs-lookup"><span data-stu-id="8ecdc-114">Steps</span></span>

<span data-ttu-id="8ecdc-115">Во-первых, включают `ScriptManager` страницы; затем библиотека ASP.NET AJAX загружена, что позволяет использовать набор элементов управления:</span><span class="sxs-lookup"><span data-stu-id="8ecdc-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

<span data-ttu-id="8ecdc-116">Анимация применяется панель текста, который выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="8ecdc-116">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

<span data-ttu-id="8ecdc-117">В связанный класс CSS для панели определяются цвет фона для работы с низким приоритетом, а также задать фиксированную ширину панели:</span><span class="sxs-lookup"><span data-stu-id="8ecdc-117">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

<span data-ttu-id="8ecdc-118">Чтобы запустить панель анимации, используется HTML-кнопка.</span><span class="sxs-lookup"><span data-stu-id="8ecdc-118">In order to start animating the panel, an HTML button is used.</span></span> <span data-ttu-id="8ecdc-119">Обратите внимание, что `<input type="button" />` favoured над `<asp:Button />` поскольку мы не хотим обратную передачу при нажатии этой кнопки.</span><span class="sxs-lookup"><span data-stu-id="8ecdc-119">Note that `<input type="button" />` is favoured over `<asp:Button />` since we do not want a postback when the user clicks on that button.</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

<span data-ttu-id="8ecdc-120">Затем добавьте `AnimationExtender` страницу, предоставляя `ID`, `TargetControlID` атрибута и обязательным `runat="server"`.</span><span class="sxs-lookup"><span data-stu-id="8ecdc-120">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`.</span></span> <span data-ttu-id="8ecdc-121">Очень важно задать `TargetControlID` идентификатору кнопки (элемент, активируя анимации) не идентификатору панели (анимируемого элемента)</span><span class="sxs-lookup"><span data-stu-id="8ecdc-121">It is important to set `TargetControlID` to the ID of the button (the element triggering the animation), not to the ID of the panel (the element being animated)</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

<span data-ttu-id="8ecdc-122">В пределах `<Animations>` узел анимации месте как обычно.</span><span class="sxs-lookup"><span data-stu-id="8ecdc-122">Within the `<Animations>` node, place animations as usual.</span></span> <span data-ttu-id="8ecdc-123">Чтобы их изменить на панели, не кнопку задать `AnimationTarget` атрибут для каждого элемента анимации в `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="8ecdc-123">In order to make them change the panel, not the button, set the `AnimationTarget` attribute for every animation element within `AnimationExtender`.</span></span> <span data-ttu-id="8ecdc-124">Значение для `AnimationTarget` — идентификатор панели, конечно.</span><span class="sxs-lookup"><span data-stu-id="8ecdc-124">The value for `AnimationTarget` is the ID of the panel, of course.</span></span> <span data-ttu-id="8ecdc-125">Таким образом, анимаций произойти, если панель не с помощью активации кнопки.</span><span class="sxs-lookup"><span data-stu-id="8ecdc-125">That way, the animations happen with the panel, not with the triggering button.</span></span> <span data-ttu-id="8ecdc-126">Вот `AnimationExtender` разметки для этого сценария:</span><span class="sxs-lookup"><span data-stu-id="8ecdc-126">Here is the `AnimationExtender` markup for this scenario:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

<span data-ttu-id="8ecdc-127">Обратите внимание на специальные порядок, в котором отображаются отдельные анимации.</span><span class="sxs-lookup"><span data-stu-id="8ecdc-127">Note the special order in which the individual animations appear.</span></span> <span data-ttu-id="8ecdc-128">Во-первых кнопки возвращает отключена после ее воспроизведения.</span><span class="sxs-lookup"><span data-stu-id="8ecdc-128">First of all, the button gets deactivated once the animation runs.</span></span> <span data-ttu-id="8ecdc-129">Так как она не `AnimationTarget` атрибута в `<EnableAction>` элемент анимации применяется к исходного управления: кнопки.</span><span class="sxs-lookup"><span data-stu-id="8ecdc-129">Since there is no `AnimationTarget` attribute in the `<EnableAction>` element, this animation is applied to the originating control: the button.</span></span> <span data-ttu-id="8ecdc-130">Анимация следующие два действия должны выполняться parallelly (`<Parallel>` элемент).</span><span class="sxs-lookup"><span data-stu-id="8ecdc-130">The next two animation steps shall be carried out parallelly (`<Parallel>` element).</span></span> <span data-ttu-id="8ecdc-131">Оба имеют их `AnimationTarget` атрибуты значения `"Panel1"`, таким образом анимации панели, а не кнопки.</span><span class="sxs-lookup"><span data-stu-id="8ecdc-131">Both have their `AnimationTarget` attributes set to `"Panel1"`, thus animating the panel, not the button.</span></span>


<span data-ttu-id="8ecdc-132">[![Нажмите кнопку мыши запуск анимации панели](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8ecdc-132">[![A mouse click on the button starts the panel animation](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="8ecdc-133">Нажмите кнопку мыши запуск анимации панель ([Просмотр полноразмерное изображение](triggering-an-animation-in-another-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8ecdc-133">A mouse click on the button starts the panel animation ([Click to view full-size image](triggering-an-animation-in-another-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8ecdc-134">[Назад](disabling-actions-during-animation-cs.md)
> [Вперед](modifying-animations-from-the-server-side-cs.md)</span><span class="sxs-lookup"><span data-stu-id="8ecdc-134">[Previous](disabling-actions-during-animation-cs.md)
[Next](modifying-animations-from-the-server-side-cs.md)</span></span>
