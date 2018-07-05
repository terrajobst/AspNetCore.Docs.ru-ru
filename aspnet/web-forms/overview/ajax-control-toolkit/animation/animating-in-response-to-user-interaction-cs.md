---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
title: Анимация в ответ на взаимодействие пользователя (C#) | Документация Майкрософт
author: wenz
description: Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления. Анимацию можно star...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ea26549d-fbbf-4973-a108-b14cd1d6de26
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
msc.type: authoredcontent
ms.openlocfilehash: 9dea9daf3df76558eb19a524475cedd8e2085297
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379823"
---
<a name="animating-in-response-to-user-interaction-c"></a><span data-ttu-id="8b6dc-104">Анимация в ответ на взаимодействие пользователя (C#)</span><span class="sxs-lookup"><span data-stu-id="8b6dc-104">Animating in Response To User Interaction (C#)</span></span>
====================
<span data-ttu-id="8b6dc-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8b6dc-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8b6dc-106">[Скачать код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="8b6dc-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span></span>

> <span data-ttu-id="8b6dc-107">Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="8b6dc-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="8b6dc-108">Анимации могут запускаться автоматически или могут быть предприняты взаимодействием с пользователем, например, щелкнув мышью.</span><span class="sxs-lookup"><span data-stu-id="8b6dc-108">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>


## <a name="overview"></a><span data-ttu-id="8b6dc-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="8b6dc-109">Overview</span></span>

<span data-ttu-id="8b6dc-110">Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="8b6dc-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="8b6dc-111">Анимации могут запускаться автоматически или могут быть предприняты взаимодействием с пользователем, например, щелкнув мышью.</span><span class="sxs-lookup"><span data-stu-id="8b6dc-111">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="steps"></a><span data-ttu-id="8b6dc-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="8b6dc-112">Steps</span></span>

<span data-ttu-id="8b6dc-113">Во-первых, включите `ScriptManager` страницы; затем ASP.NET AJAX library загружена, что позволяет использовать набор средств управления:</span><span class="sxs-lookup"><span data-stu-id="8b6dc-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample1.aspx)]

<span data-ttu-id="8b6dc-114">Анимация будет применяться к панели текста, который выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="8b6dc-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample2.aspx)]

<span data-ttu-id="8b6dc-115">В связанный класс CSS для панели определить цвет фона, удобная и также установить фиксированную ширину для панели:</span><span class="sxs-lookup"><span data-stu-id="8b6dc-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animating-in-response-to-user-interaction-cs/samples/sample3.css)]

<span data-ttu-id="8b6dc-116">Затем добавьте `AnimationExtender` на страницу, предоставляя `ID`, `TargetControlID` атрибут и обязательным `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="8b6dc-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample4.aspx)]

<span data-ttu-id="8b6dc-117">В рамках `<Animations>` узел, существует пять способов для запуска анимации с помощью взаимодействия с пользователем (неотображаемый элемент является `<OnLoad>` которого будет выполнена после полной загрузки страницы в целом):</span><span class="sxs-lookup"><span data-stu-id="8b6dc-117">Within the `<Animations>` node, there are five ways to start the animation via user interaction (the missing element is `<OnLoad>` which is executed once the whole page has been fully loaded):</span></span>

- <span data-ttu-id="8b6dc-118">`<OnClick>` (щелчка мыши на элементе управления)</span><span class="sxs-lookup"><span data-stu-id="8b6dc-118">`<OnClick>` (mouse click on the control)</span></span>
- <span data-ttu-id="8b6dc-119">`<OnHoverOut>` (указатель мыши покидает элемент управления)</span><span class="sxs-lookup"><span data-stu-id="8b6dc-119">`<OnHoverOut>` (mouse leaves the control)</span></span>
- <span data-ttu-id="8b6dc-120">`<OnHoverOver>` (указатель мыши находится над элементом управления, остановка `<OnHoverOut>` анимации)</span><span class="sxs-lookup"><span data-stu-id="8b6dc-120">`<OnHoverOver>` (mouse hovers over a control, stopping the `<OnHoverOut>` animation)</span></span>
- <span data-ttu-id="8b6dc-121">`<OnMouseOut>` (указатель мыши покидает элемент управления)</span><span class="sxs-lookup"><span data-stu-id="8b6dc-121">`<OnMouseOut>` (mouse leaves a control)</span></span>
- <span data-ttu-id="8b6dc-122">`<OnMouseOver>` (указатель мыши находится над элементом управления, не останавливает `<OnMouseOut>` анимации)</span><span class="sxs-lookup"><span data-stu-id="8b6dc-122">`<OnMouseOver>` (mouse hovers over a control, not stopping the `<OnMouseOut>` animation)</span></span>

<span data-ttu-id="8b6dc-123">В этом случае `<OnClick>` используется.</span><span class="sxs-lookup"><span data-stu-id="8b6dc-123">In this scenario, `<OnClick>` is used.</span></span> <span data-ttu-id="8b6dc-124">При нажатии на панели, он изменяется и исчезает, в то же время.</span><span class="sxs-lookup"><span data-stu-id="8b6dc-124">When the user clicks on the panel, it is resized and fades out at the same time.</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample5.aspx)]


<span data-ttu-id="8b6dc-125">[![Нажатие клавиши мыши запускает анимацию](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8b6dc-125">[![A mouse click starts the animation](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span></span>

<span data-ttu-id="8b6dc-126">Нажатие клавиши мыши запускает анимацию ([Просмотр полноразмерного изображения](animating-in-response-to-user-interaction-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8b6dc-126">A mouse click starts the animation ([Click to view full-size image](animating-in-response-to-user-interaction-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8b6dc-127">[Назад](picking-one-animation-out-of-a-list-cs.md)
> [Вперед](disabling-actions-during-animation-cs.md)</span><span class="sxs-lookup"><span data-stu-id="8b6dc-127">[Previous](picking-one-animation-out-of-a-list-cs.md)
[Next](disabling-actions-during-animation-cs.md)</span></span>
