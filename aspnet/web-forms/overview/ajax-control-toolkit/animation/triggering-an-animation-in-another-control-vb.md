---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
title: Запуск анимации в другой элемент управления (Visual Basic) | Документация Майкрософт
author: wenz
description: Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления. Как правило, запуск...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 25ebaf1f-5a9f-423d-98c7-1d694e93664f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
msc.type: authoredcontent
ms.openlocfilehash: b316b22bf355d7abb0909e43f0c2c38ea3e68f24
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367814"
---
<a name="triggering-an-animation-in-another-control-vb"></a><span data-ttu-id="3d41e-104">Запуск анимации в другом элементе управления (VB)</span><span class="sxs-lookup"><span data-stu-id="3d41e-104">Triggering an Animation in another Control (VB)</span></span>
====================
<span data-ttu-id="3d41e-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="3d41e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="3d41e-106">[Скачать код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="3d41e-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)</span></span>

> <span data-ttu-id="3d41e-107">Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="3d41e-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="3d41e-108">Как правило запуск анимации активируется взаимодействие пользователя с одного элемента управления.</span><span class="sxs-lookup"><span data-stu-id="3d41e-108">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="3d41e-109">Тем не менее также возможность взаимодействия с одного элемента управления, а затем анимации другого элемента управления.</span><span class="sxs-lookup"><span data-stu-id="3d41e-109">It is however also possible to interact with one control and then animation another control.</span></span>


## <a name="overview"></a><span data-ttu-id="3d41e-110">Обзор</span><span class="sxs-lookup"><span data-stu-id="3d41e-110">Overview</span></span>

<span data-ttu-id="3d41e-111">Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="3d41e-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="3d41e-112">Как правило запуск анимации активируется взаимодействие пользователя с одного элемента управления.</span><span class="sxs-lookup"><span data-stu-id="3d41e-112">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="3d41e-113">Тем не менее также возможность взаимодействия с одного элемента управления, а затем анимации другого элемента управления.</span><span class="sxs-lookup"><span data-stu-id="3d41e-113">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="steps"></a><span data-ttu-id="3d41e-114">Шаги</span><span class="sxs-lookup"><span data-stu-id="3d41e-114">Steps</span></span>

<span data-ttu-id="3d41e-115">Во-первых, включите `ScriptManager` страницы; затем ASP.NET AJAX library загружена, что позволяет использовать набор средств управления:</span><span class="sxs-lookup"><span data-stu-id="3d41e-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample1.aspx)]

<span data-ttu-id="3d41e-116">Анимация будет применяться к панели текста, который выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="3d41e-116">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample2.aspx)]

<span data-ttu-id="3d41e-117">В связанный класс CSS для панели определить цвет фона, удобная и также установить фиксированную ширину для панели:</span><span class="sxs-lookup"><span data-stu-id="3d41e-117">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](triggering-an-animation-in-another-control-vb/samples/sample3.css)]

<span data-ttu-id="3d41e-118">Для запуска анимации на панели, используется как кнопка HTML.</span><span class="sxs-lookup"><span data-stu-id="3d41e-118">In order to start animating the panel, an HTML button is used.</span></span> <span data-ttu-id="3d41e-119">Обратите внимание, что `<input type="button" />` favoured через `<asp:Button />` поскольку мы не хотим обратную передачу при нажатии этой кнопки.</span><span class="sxs-lookup"><span data-stu-id="3d41e-119">Note that `<input type="button" />` is favoured over `<asp:Button />` since we do not want a postback when the user clicks on that button.</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample4.aspx)]

<span data-ttu-id="3d41e-120">Затем добавьте `AnimationExtender` на страницу, предоставляя `ID`, `TargetControlID` атрибут и обязательным `runat="server"`.</span><span class="sxs-lookup"><span data-stu-id="3d41e-120">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`.</span></span> <span data-ttu-id="3d41e-121">Очень важно задать `TargetControlID` для ИД кнопки (элемент триггера в анимации), не идентификатору панели (элемент, выполняется анимация)</span><span class="sxs-lookup"><span data-stu-id="3d41e-121">It is important to set `TargetControlID` to the ID of the button (the element triggering the animation), not to the ID of the panel (the element being animated)</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample5.aspx)]

<span data-ttu-id="3d41e-122">В рамках `<Animations>` узел, анимации месте как обычно.</span><span class="sxs-lookup"><span data-stu-id="3d41e-122">Within the `<Animations>` node, place animations as usual.</span></span> <span data-ttu-id="3d41e-123">Чтобы сделать их изменить на панели, не нажатием кнопки, задайте `AnimationTarget` атрибут для каждого элемента анимации в `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="3d41e-123">In order to make them change the panel, not the button, set the `AnimationTarget` attribute for every animation element within `AnimationExtender`.</span></span> <span data-ttu-id="3d41e-124">Значение для `AnimationTarget` — это идентификатор панели, конечно.</span><span class="sxs-lookup"><span data-stu-id="3d41e-124">The value for `AnimationTarget` is the ID of the panel, of course.</span></span> <span data-ttu-id="3d41e-125">Таким образом, анимации, которые происходят с панелью, не с помощью активации кнопки.</span><span class="sxs-lookup"><span data-stu-id="3d41e-125">That way, the animations happen with the panel, not with the triggering button.</span></span> <span data-ttu-id="3d41e-126">Вот `AnimationExtender` разметки для этого сценария:</span><span class="sxs-lookup"><span data-stu-id="3d41e-126">Here is the `AnimationExtender` markup for this scenario:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample6.aspx)]

<span data-ttu-id="3d41e-127">Обратите внимание, специальный заказ, в котором отображаются отдельные анимации.</span><span class="sxs-lookup"><span data-stu-id="3d41e-127">Note the special order in which the individual animations appear.</span></span> <span data-ttu-id="3d41e-128">Во-первых кнопки получает отключена после выполнения анимации.</span><span class="sxs-lookup"><span data-stu-id="3d41e-128">First of all, the button gets deactivated once the animation runs.</span></span> <span data-ttu-id="3d41e-129">Так как она не `AnimationTarget` атрибут в `<EnableAction>` элемент, эта анимация применяется к исходного управления: кнопки.</span><span class="sxs-lookup"><span data-stu-id="3d41e-129">Since there is no `AnimationTarget` attribute in the `<EnableAction>` element, this animation is applied to the originating control: the button.</span></span> <span data-ttu-id="3d41e-130">Следующие две анимации действия должны выполняться parallelly (`<Parallel>` элемент).</span><span class="sxs-lookup"><span data-stu-id="3d41e-130">The next two animation steps shall be carried out parallelly (`<Parallel>` element).</span></span> <span data-ttu-id="3d41e-131">Оба имеют свои `AnimationTarget` атрибуты значения `"Panel1"`, таким образом анимации на панели, а не кнопки.</span><span class="sxs-lookup"><span data-stu-id="3d41e-131">Both have their `AnimationTarget` attributes set to `"Panel1"`, thus animating the panel, not the button.</span></span>


<span data-ttu-id="3d41e-132">[![Нажмите кнопку мыши запускает анимацию панели](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3d41e-132">[![A mouse click on the button starts the panel animation](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="3d41e-133">Нажмите кнопку мыши запускает анимацию панели ([Просмотр полноразмерного изображения](triggering-an-animation-in-another-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="3d41e-133">A mouse click on the button starts the panel animation ([Click to view full-size image](triggering-an-animation-in-another-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3d41e-134">[Назад](disabling-actions-during-animation-vb.md)
> [Вперед](modifying-animations-from-the-server-side-vb.md)</span><span class="sxs-lookup"><span data-stu-id="3d41e-134">[Previous](disabling-actions-during-animation-vb.md)
[Next](modifying-animations-from-the-server-side-vb.md)</span></span>
