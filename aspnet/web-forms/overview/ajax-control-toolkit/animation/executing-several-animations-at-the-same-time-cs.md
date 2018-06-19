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
<a name="executing-several-animations-at-the-same-time-c"></a><span data-ttu-id="b0c74-104">Выполнение нескольких анимации в то же время (C#)</span><span class="sxs-lookup"><span data-stu-id="b0c74-104">Executing Several Animations at The Same Time (C#)</span></span>
====================
<span data-ttu-id="b0c74-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b0c74-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b0c74-106">[Загрузить код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="b0c74-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)</span></span>

> <span data-ttu-id="b0c74-107">Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="b0c74-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="b0c74-108">Это позволяет запустить несколько анимаций параллельных образом.</span><span class="sxs-lookup"><span data-stu-id="b0c74-108">It allows to run several animations in a parallel fashion.</span></span>


## <a name="overview"></a><span data-ttu-id="b0c74-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="b0c74-109">Overview</span></span>

<span data-ttu-id="b0c74-110">Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="b0c74-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="b0c74-111">Это позволяет запустить несколько анимаций параллельных образом.</span><span class="sxs-lookup"><span data-stu-id="b0c74-111">It allows to run several animations in a parallel fashion.</span></span>

## <a name="steps"></a><span data-ttu-id="b0c74-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="b0c74-112">Steps</span></span>

<span data-ttu-id="b0c74-113">Во-первых, включают `ScriptManager` страницы; затем библиотека ASP.NET AJAX загружена, что позволяет использовать набор элементов управления:</span><span class="sxs-lookup"><span data-stu-id="b0c74-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample1.aspx)]

<span data-ttu-id="b0c74-114">Анимация применяется панель текста, который выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="b0c74-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample2.aspx)]

<span data-ttu-id="b0c74-115">В связанный класс CSS для панели определяются цвет фона для работы с низким приоритетом, а также задать фиксированную ширину панели:</span><span class="sxs-lookup"><span data-stu-id="b0c74-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-at-the-same-time-cs/samples/sample3.css)]

<span data-ttu-id="b0c74-116">Затем добавьте `AnimationExtender` страницу, предоставляя `ID`, `TargetControlID` атрибута и обязательным `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="b0c74-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample4.aspx)]

<span data-ttu-id="b0c74-117">В пределах `<Animations>` узла, используйте `<OnLoad>` для выполнения анимации после полной загрузки страницы.</span><span class="sxs-lookup"><span data-stu-id="b0c74-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="b0c74-118">Как правило `<OnLoad>` принимает только одна анимация.</span><span class="sxs-lookup"><span data-stu-id="b0c74-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="b0c74-119">Платформа анимации позволяет объединить несколько анимаций в один с помощью `<Parallel>` элемента.</span><span class="sxs-lookup"><span data-stu-id="b0c74-119">The Animation framework allows you to join several animations into one using the `<Parallel>` element.</span></span> <span data-ttu-id="b0c74-120">Все анимации в `<Parallel>` выполняются одновременно.</span><span class="sxs-lookup"><span data-stu-id="b0c74-120">All animations within `<Parallel>` are executed at the same time.</span></span>

<span data-ttu-id="b0c74-121">Вот возможные разметку для `AnimationExtender` управления исчезновение и изменение размера панели в то же время:</span><span class="sxs-lookup"><span data-stu-id="b0c74-121">Here is the a possible markup for the `AnimationExtender` control, fading out and resizing the panel at the same time:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample5.aspx)]

<span data-ttu-id="b0c74-122">И на самом деле: при выполнении этого скрипта панели отображается, а затем изменяет размер (больше, чем threefolding высоты и halfing его высота) и Исчезание, в то же время.</span><span class="sxs-lookup"><span data-stu-id="b0c74-122">And indeed: when you run this script, the panel is displayed, then resizes (more than threefolding its width and halfing its height) and fades out at the same time.</span></span>


<span data-ttu-id="b0c74-123">[![Исчезновение и изменение размера (в том числе его содержимого, благодаря механизм визуализации в браузере) панели](executing-several-animations-at-the-same-time-cs/_static/image2.png)](executing-several-animations-at-the-same-time-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b0c74-123">[![The panel is fading out and resizing (including its content, thanks to the browser's rendering engine)](executing-several-animations-at-the-same-time-cs/_static/image2.png)](executing-several-animations-at-the-same-time-cs/_static/image1.png)</span></span>

<span data-ttu-id="b0c74-124">Исчезновение и изменение размера (в том числе его содержимого, благодаря механизм визуализации в браузере) панели ([Просмотр полноразмерное изображение](executing-several-animations-at-the-same-time-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b0c74-124">The panel is fading out and resizing (including its content, thanks to the browser's rendering engine) ([Click to view full-size image](executing-several-animations-at-the-same-time-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b0c74-125">[Назад](adding-animation-to-a-control-cs.md)
> [Вперед](executing-several-animations-after-each-other-cs.md)</span><span class="sxs-lookup"><span data-stu-id="b0c74-125">[Previous](adding-animation-to-a-control-cs.md)
[Next](executing-several-animations-after-each-other-cs.md)</span></span>
