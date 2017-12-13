---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
title: "Выполнение нескольких анимации друг за другом (VB) | Документы Microsoft"
author: wenz
description: "Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Он позволяет выполнять severa..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 21ece509-79cc-4d9d-892d-7b6e9c4d3502
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
msc.type: authoredcontent
ms.openlocfilehash: e949d181c0b742ee38ebbcc46e0e08efc678a1f8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="executing-several-animations-after-each-other-vb"></a><span data-ttu-id="6a409-104">Выполнение нескольких анимации друг за другом (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="6a409-104">Executing Several Animations after Each Other (VB)</span></span>
====================
<span data-ttu-id="6a409-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="6a409-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="6a409-106">[Загрузить код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="6a409-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span></span>

> <span data-ttu-id="6a409-107">Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="6a409-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="6a409-108">Это позволяет запустить несколько анимаций один за другим.</span><span class="sxs-lookup"><span data-stu-id="6a409-108">It allows to run several animations one after the other.</span></span>


## <a name="overview"></a><span data-ttu-id="6a409-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="6a409-109">Overview</span></span>

<span data-ttu-id="6a409-110">Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="6a409-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="6a409-111">Это позволяет запустить несколько анимаций один за другим.</span><span class="sxs-lookup"><span data-stu-id="6a409-111">It allows to run several animations one after the other.</span></span>

## <a name="steps"></a><span data-ttu-id="6a409-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="6a409-112">Steps</span></span>

<span data-ttu-id="6a409-113">Во-первых, включают `ScriptManager` страницы; затем библиотека ASP.NET AJAX загружена, что позволяет использовать набор элементов управления:</span><span class="sxs-lookup"><span data-stu-id="6a409-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample1.aspx)]

<span data-ttu-id="6a409-114">Анимация применяется панель текста, который выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="6a409-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample2.aspx)]

<span data-ttu-id="6a409-115">В связанный класс CSS для панели определяются цвет фона для работы с низким приоритетом, а также задать фиксированную ширину панели:</span><span class="sxs-lookup"><span data-stu-id="6a409-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-after-each-other-vb/samples/sample3.css)]

<span data-ttu-id="6a409-116">Затем добавьте `AnimationExtender` страницу, предоставляя `ID`, `TargetControlID` атрибута и обязательным`runat="server":`</span><span class="sxs-lookup"><span data-stu-id="6a409-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample4.aspx)]

<span data-ttu-id="6a409-117">В пределах `<Animations>` узла, используйте `<OnLoad>` для выполнения анимации после полной загрузки страницы.</span><span class="sxs-lookup"><span data-stu-id="6a409-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="6a409-118">Как правило `<OnLoad>` принимает только одна анимация.</span><span class="sxs-lookup"><span data-stu-id="6a409-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="6a409-119">Платформа анимации позволяет объединить несколько анимаций в один с помощью `<Sequence>` элемента.</span><span class="sxs-lookup"><span data-stu-id="6a409-119">The Animation framework allows you to join several animations into one using the `<Sequence>` element.</span></span> <span data-ttu-id="6a409-120">Все анимации в `<Sequence>` , выполняются за другим.</span><span class="sxs-lookup"><span data-stu-id="6a409-120">All animations within `<Sequence>` are executed one after the other.</span></span> <span data-ttu-id="6a409-121">Вот возможные разметку для `AnimationExtender` элемента управления, сначала внесение панели «ширина» и затем Уменьшение высоты:</span><span class="sxs-lookup"><span data-stu-id="6a409-121">Here is the a possible markup for the `AnimationExtender` control, first making the panel wider and then decreasing its height:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample5.aspx)]

<span data-ttu-id="6a409-122">При запуске этого скрипта панели возвращает первый шире и затем меньшего размера.</span><span class="sxs-lookup"><span data-stu-id="6a409-122">When you run this script, the panel first gets wider and then smaller.</span></span>


<span data-ttu-id="6a409-123">[![Сначала увеличить ширину](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6a409-123">[![First the width is increased](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span></span>

<span data-ttu-id="6a409-124">Сначала увеличить ширину ([Просмотр полноразмерное изображение](executing-several-animations-after-each-other-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6a409-124">First the width is increased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image3.png))</span></span>


<span data-ttu-id="6a409-125">[![Затем Уменьшение высоты](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="6a409-125">[![Then the height is decreased](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span></span>

<span data-ttu-id="6a409-126">Затем уменьшить высоту ([Просмотр полноразмерное изображение](executing-several-animations-after-each-other-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="6a409-126">Then the height is decreased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image6.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="6a409-127">[Назад](executing-several-animations-at-the-same-time-vb.md)
[Вперед](animation-depending-on-a-condition-vb.md)</span><span class="sxs-lookup"><span data-stu-id="6a409-127">[Previous](executing-several-animations-at-the-same-time-vb.md)
[Next](animation-depending-on-a-condition-vb.md)</span></span>
