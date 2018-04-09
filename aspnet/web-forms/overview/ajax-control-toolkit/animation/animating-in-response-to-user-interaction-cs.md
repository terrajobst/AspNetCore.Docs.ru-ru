---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
title: Анимация в ответ на взаимодействие пользователя (C#) | Документы Microsoft
author: wenz
description: Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Анимации можно звездочек...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ea26549d-fbbf-4973-a108-b14cd1d6de26
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
msc.type: authoredcontent
ms.openlocfilehash: 783563f4e33087e99a071cf829ca6bab246ba3b0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="animating-in-response-to-user-interaction-c"></a><span data-ttu-id="bff75-104">Анимация в ответ на взаимодействие пользователя (C#)</span><span class="sxs-lookup"><span data-stu-id="bff75-104">Animating in Response To User Interaction (C#)</span></span>
====================
<span data-ttu-id="bff75-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="bff75-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="bff75-106">[Загрузить код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="bff75-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span></span>

> <span data-ttu-id="bff75-107">Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="bff75-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="bff75-108">Анимации можно запускать автоматически или могут быть предприняты взаимодействием с пользователем, например, щелкнув мышью.</span><span class="sxs-lookup"><span data-stu-id="bff75-108">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>


## <a name="overview"></a><span data-ttu-id="bff75-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="bff75-109">Overview</span></span>

<span data-ttu-id="bff75-110">Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="bff75-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="bff75-111">Анимации можно запускать автоматически или могут быть предприняты взаимодействием с пользователем, например, щелкнув мышью.</span><span class="sxs-lookup"><span data-stu-id="bff75-111">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="steps"></a><span data-ttu-id="bff75-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="bff75-112">Steps</span></span>

<span data-ttu-id="bff75-113">Во-первых, включают `ScriptManager` страницы; затем библиотека ASP.NET AJAX загружена, что позволяет использовать набор элементов управления:</span><span class="sxs-lookup"><span data-stu-id="bff75-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample1.aspx)]

<span data-ttu-id="bff75-114">Анимация применяется панель текста, который выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="bff75-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample2.aspx)]

<span data-ttu-id="bff75-115">В связанный класс CSS для панели определяются цвет фона для работы с низким приоритетом, а также задать фиксированную ширину панели:</span><span class="sxs-lookup"><span data-stu-id="bff75-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animating-in-response-to-user-interaction-cs/samples/sample3.css)]

<span data-ttu-id="bff75-116">Затем добавьте `AnimationExtender` страницу, предоставляя `ID`, `TargetControlID` атрибута и обязательным `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="bff75-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample4.aspx)]

<span data-ttu-id="bff75-117">В пределах `<Animations>` узла существует пять способов запуска анимации через взаимодействие с пользователем (отсутствует элемент `<OnLoad>` которое выполняется после полной загрузки страницы в целом):</span><span class="sxs-lookup"><span data-stu-id="bff75-117">Within the `<Animations>` node, there are five ways to start the animation via user interaction (the missing element is `<OnLoad>` which is executed once the whole page has been fully loaded):</span></span>

- <span data-ttu-id="bff75-118">`<OnClick>` (щелкните мышью элемент управления)</span><span class="sxs-lookup"><span data-stu-id="bff75-118">`<OnClick>` (mouse click on the control)</span></span>
- <span data-ttu-id="bff75-119">`<OnHoverOut>` (указатель мыши покидает элемент управления)</span><span class="sxs-lookup"><span data-stu-id="bff75-119">`<OnHoverOut>` (mouse leaves the control)</span></span>
- <span data-ttu-id="bff75-120">`<OnHoverOver>` (указатель мыши находится над элементом управления, остановка `<OnHoverOut>` анимации)</span><span class="sxs-lookup"><span data-stu-id="bff75-120">`<OnHoverOver>` (mouse hovers over a control, stopping the `<OnHoverOut>` animation)</span></span>
- <span data-ttu-id="bff75-121">`<OnMouseOut>` (указатель мыши покидает элемент управления)</span><span class="sxs-lookup"><span data-stu-id="bff75-121">`<OnMouseOut>` (mouse leaves a control)</span></span>
- <span data-ttu-id="bff75-122">`<OnMouseOver>` (указатель мыши находится над элементом управления, не останавливая `<OnMouseOut>` анимации)</span><span class="sxs-lookup"><span data-stu-id="bff75-122">`<OnMouseOver>` (mouse hovers over a control, not stopping the `<OnMouseOut>` animation)</span></span>

<span data-ttu-id="bff75-123">В этом сценарии `<OnClick>` используется.</span><span class="sxs-lookup"><span data-stu-id="bff75-123">In this scenario, `<OnClick>` is used.</span></span> <span data-ttu-id="bff75-124">При нажатии на панели, он изменяется и Исчезание, в то же время.</span><span class="sxs-lookup"><span data-stu-id="bff75-124">When the user clicks on the panel, it is resized and fades out at the same time.</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample5.aspx)]


<span data-ttu-id="bff75-125">[![Эффект запускается щелчка кнопкой мыши](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bff75-125">[![A mouse click starts the animation](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span></span>

<span data-ttu-id="bff75-126">Щелчок мыши запуск анимации ([Просмотр полноразмерное изображение](animating-in-response-to-user-interaction-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="bff75-126">A mouse click starts the animation ([Click to view full-size image](animating-in-response-to-user-interaction-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bff75-127">[Назад](picking-one-animation-out-of-a-list-cs.md)
> [Вперед](disabling-actions-during-animation-cs.md)</span><span class="sxs-lookup"><span data-stu-id="bff75-127">[Previous](picking-one-animation-out-of-a-list-cs.md)
[Next](disabling-actions-during-animation-cs.md)</span></span>
