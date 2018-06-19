---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
title: Подбор одна анимация из списка (C#) | Документы Microsoft
author: wenz
description: Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Платформа также р...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 06a776fe-7c73-4ca7-8e02-5260a86edc03
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
msc.type: authoredcontent
ms.openlocfilehash: 2d4aac447fcdfbf296560091cfcdf5eb51997a7b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871666"
---
<a name="picking-one-animation-out-of-a-list-c"></a><span data-ttu-id="0d91e-104">Подбор одна анимация из списка (C#)</span><span class="sxs-lookup"><span data-stu-id="0d91e-104">Picking One Animation Out Of a List (C#)</span></span>
====================
<span data-ttu-id="0d91e-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0d91e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0d91e-106">[Загрузить код](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.cs.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="0d91e-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5CS.pdf)</span></span>

> <span data-ttu-id="0d91e-107">Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="0d91e-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="0d91e-108">Платформа также позволяет программисту одна анимация анимации, в зависимости от оценки кода JavaScript из списка выбора.</span><span class="sxs-lookup"><span data-stu-id="0d91e-108">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="0d91e-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="0d91e-109">Overview</span></span>

<span data-ttu-id="0d91e-110">Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="0d91e-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="0d91e-111">Платформа также позволяет программисту одна анимация анимации, в зависимости от оценки кода JavaScript из списка выбора.</span><span class="sxs-lookup"><span data-stu-id="0d91e-111">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="0d91e-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="0d91e-112">Steps</span></span>

<span data-ttu-id="0d91e-113">Во-первых, включают `ScriptManager` страницы; затем библиотека ASP.NET AJAX загружена, что позволяет использовать набор элементов управления:</span><span class="sxs-lookup"><span data-stu-id="0d91e-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample1.aspx)]

<span data-ttu-id="0d91e-114">Анимация применяется панель текста, который выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="0d91e-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample2.aspx)]

<span data-ttu-id="0d91e-115">В связанный класс CSS для панели определяются цвет фона для работы с низким приоритетом, а также задать фиксированную ширину панели:</span><span class="sxs-lookup"><span data-stu-id="0d91e-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](picking-one-animation-out-of-a-list-cs/samples/sample3.css)]

<span data-ttu-id="0d91e-116">Затем добавьте `AnimationExtender` страницу, предоставляя `ID`, `TargetControlID` атрибута и обязательным `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="0d91e-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample4.aspx)]

<span data-ttu-id="0d91e-117">В пределах `<Animations>` узла, используйте `<OnLoad>` для выполнения анимации после полной загрузки страницы.</span><span class="sxs-lookup"><span data-stu-id="0d91e-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="0d91e-118">Вместо обычных анимацию `<Case>` элемент вступает в действие.</span><span class="sxs-lookup"><span data-stu-id="0d91e-118">Instead of one of the regular animations, the `<Case>` element comes into play.</span></span> <span data-ttu-id="0d91e-119">Значение атрибута SelectScript вычисляется; Возвращаемое значение должно быть числовым.</span><span class="sxs-lookup"><span data-stu-id="0d91e-119">The value of its SelectScript attribute is evaluated; the return value must be numerical.</span></span> <span data-ttu-id="0d91e-120">В зависимости от того, это число, а один из subanimations в &lt;случай&gt; выполняется.</span><span class="sxs-lookup"><span data-stu-id="0d91e-120">Depending on this number, one of the subanimations within &lt;Case&gt; is executed.</span></span> <span data-ttu-id="0d91e-121">Для экземпляра, если SelectScript равно 2, набор элементов управления выполняется третий анимации в &lt;случай&gt; (перечисление начинается с 0).</span><span class="sxs-lookup"><span data-stu-id="0d91e-121">For instance, if SelectScript evaluates to 2, the Control Toolkit runs the third animation within &lt;Case&gt; (counting starts at 0).</span></span>

<span data-ttu-id="0d91e-122">Приведенная ниже разметка определяет три subanimations: изменение размера ширину, высоту изменение размеров и исчезновение. Код JavaScript (`Math.floor(3 * Math.random())`) затем выбирает число между 0 и 2, чтобы одно из трех анимации выполняется:</span><span class="sxs-lookup"><span data-stu-id="0d91e-122">The following markup defines three subanimations: Resizing the width, resizing the height, and fading out. The JavaScript code (`Math.floor(3 * Math.random())`) then picks a number between 0 and 2, so that one of the three animations is run:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample5.aspx)]


<span data-ttu-id="0d91e-123">[![Одно из возможных три анимации: Получает ширину панели](picking-one-animation-out-of-a-list-cs/_static/image2.png)](picking-one-animation-out-of-a-list-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0d91e-123">[![One of the possible three animations: The panel gets wider](picking-one-animation-out-of-a-list-cs/_static/image2.png)](picking-one-animation-out-of-a-list-cs/_static/image1.png)</span></span>

<span data-ttu-id="0d91e-124">Одно из возможных три анимации: Получает ширину панели ([Просмотр полноразмерное изображение](picking-one-animation-out-of-a-list-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0d91e-124">One of the possible three animations: The panel gets wider ([Click to view full-size image](picking-one-animation-out-of-a-list-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0d91e-125">[Назад](animation-depending-on-a-condition-cs.md)
> [Вперед](animating-in-response-to-user-interaction-cs.md)</span><span class="sxs-lookup"><span data-stu-id="0d91e-125">[Previous](animation-depending-on-a-condition-cs.md)
[Next](animating-in-response-to-user-interaction-cs.md)</span></span>
