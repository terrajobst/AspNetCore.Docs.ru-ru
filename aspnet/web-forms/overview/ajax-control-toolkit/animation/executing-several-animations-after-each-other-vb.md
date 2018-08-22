---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
title: Выполнение нескольких анимаций друг за другом (Visual Basic) | Документация Майкрософт
author: wenz
description: Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления. Он позволяет выполнять severa...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 21ece509-79cc-4d9d-892d-7b6e9c4d3502
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
msc.type: authoredcontent
ms.openlocfilehash: 89412c078bbe40f06d31327d0a17bf3ea8bc8314
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835683"
---
<a name="executing-several-animations-after-each-other-vb"></a><span data-ttu-id="6df76-104">Выполнение нескольких анимаций друг за другом (VB)</span><span class="sxs-lookup"><span data-stu-id="6df76-104">Executing Several Animations after Each Other (VB)</span></span>
====================
<span data-ttu-id="6df76-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="6df76-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="6df76-106">[Скачать код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="6df76-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span></span>

> <span data-ttu-id="6df76-107">Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="6df76-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="6df76-108">Он одновременное выполнение нескольких анимаций один за другим.</span><span class="sxs-lookup"><span data-stu-id="6df76-108">It allows to run several animations one after the other.</span></span>


## <a name="overview"></a><span data-ttu-id="6df76-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="6df76-109">Overview</span></span>

<span data-ttu-id="6df76-110">Отображается этот элемент управления в ASP.NET AJAX Control Toolkit не только элемент управления, но всю платформу для добавления анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="6df76-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="6df76-111">Он одновременное выполнение нескольких анимаций один за другим.</span><span class="sxs-lookup"><span data-stu-id="6df76-111">It allows to run several animations one after the other.</span></span>

## <a name="steps"></a><span data-ttu-id="6df76-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="6df76-112">Steps</span></span>

<span data-ttu-id="6df76-113">Во-первых, включите `ScriptManager` страницы; затем ASP.NET AJAX library загружена, что позволяет использовать набор средств управления:</span><span class="sxs-lookup"><span data-stu-id="6df76-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample1.aspx)]

<span data-ttu-id="6df76-114">Анимация будет применяться к панели текста, который выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="6df76-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample2.aspx)]

<span data-ttu-id="6df76-115">В связанный класс CSS для панели определить цвет фона, удобная и также установить фиксированную ширину для панели:</span><span class="sxs-lookup"><span data-stu-id="6df76-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-after-each-other-vb/samples/sample3.css)]

<span data-ttu-id="6df76-116">Затем добавьте `AnimationExtender` на страницу, предоставляя `ID`, `TargetControlID` атрибут и обязательным `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="6df76-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample4.aspx)]

<span data-ttu-id="6df76-117">В рамках `<Animations>` узла, используйте `<OnLoad>` для запуска анимации после полной загрузки страницы.</span><span class="sxs-lookup"><span data-stu-id="6df76-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="6df76-118">Как правило `<OnLoad>` принимает только одна анимация.</span><span class="sxs-lookup"><span data-stu-id="6df76-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="6df76-119">Платформа анимации позволяет объединить несколько анимаций в один с помощью `<Sequence>` элемент.</span><span class="sxs-lookup"><span data-stu-id="6df76-119">The Animation framework allows you to join several animations into one using the `<Sequence>` element.</span></span> <span data-ttu-id="6df76-120">Все анимации в `<Sequence>` , выполняются за другим.</span><span class="sxs-lookup"><span data-stu-id="6df76-120">All animations within `<Sequence>` are executed one after the other.</span></span> <span data-ttu-id="6df76-121">Вот возможные разметку для `AnimationExtender` элемента управления, сначала внесение панели «ширина» и затем уменьшая высоту:</span><span class="sxs-lookup"><span data-stu-id="6df76-121">Here is the a possible markup for the `AnimationExtender` control, first making the panel wider and then decreasing its height:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample5.aspx)]

<span data-ttu-id="6df76-122">При выполнении этого сценария, панели первой получает шире и затем меньшего размера.</span><span class="sxs-lookup"><span data-stu-id="6df76-122">When you run this script, the panel first gets wider and then smaller.</span></span>


<span data-ttu-id="6df76-123">[![Сначала увеличивается ширина](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6df76-123">[![First the width is increased](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span></span>

<span data-ttu-id="6df76-124">Сначала ширина увеличивается ([Просмотр полноразмерного изображения](executing-several-animations-after-each-other-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6df76-124">First the width is increased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image3.png))</span></span>


<span data-ttu-id="6df76-125">[![Затем уменьшается на высоту](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="6df76-125">[![Then the height is decreased](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span></span>

<span data-ttu-id="6df76-126">Затем уменьшается на высоту ([Просмотр полноразмерного изображения](executing-several-animations-after-each-other-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="6df76-126">Then the height is decreased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6df76-127">[Назад](executing-several-animations-at-the-same-time-vb.md)
> [Вперед](animation-depending-on-a-condition-vb.md)</span><span class="sxs-lookup"><span data-stu-id="6df76-127">[Previous](executing-several-animations-at-the-same-time-vb.md)
[Next](animation-depending-on-a-condition-vb.md)</span></span>
