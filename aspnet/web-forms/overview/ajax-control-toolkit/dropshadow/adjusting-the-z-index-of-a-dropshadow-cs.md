---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
title: "Настройка Z-Index DropShadow (C#) | Документы Microsoft"
author: wenz
description: "Элемент управления DropShadow в наборе элементов управления AJAX расширяет панель с тенью. Однако эта теневая иногда конфликтует с другими элементами управления для пап..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 14133833-e518-4347-87b9-6b6f71f14a77
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
msc.type: authoredcontent
ms.openlocfilehash: 161f02daa5dd1f0e21853c1b7c1a65c1a9aa5d03
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="adjusting-the-z-index-of-a-dropshadow-c"></a><span data-ttu-id="15d71-104">Настройка Z-Index DropShadow (C#)</span><span class="sxs-lookup"><span data-stu-id="15d71-104">Adjusting the Z-Index of a DropShadow (C#)</span></span>
====================
<span data-ttu-id="15d71-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="15d71-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="15d71-106">[Загрузить код](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="15d71-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)</span></span>

> <span data-ttu-id="15d71-107">Элемент управления DropShadow в наборе элементов управления AJAX расширяет панель с тенью.</span><span class="sxs-lookup"><span data-stu-id="15d71-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="15d71-108">Однако эта теневая иногда конфликтует с другими элементами управления, например элемент управления меню ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="15d71-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="15d71-109">При записи меню появится окно, она находится за тени.</span><span class="sxs-lookup"><span data-stu-id="15d71-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>


## <a name="overview"></a><span data-ttu-id="15d71-110">Обзор</span><span class="sxs-lookup"><span data-stu-id="15d71-110">Overview</span></span>

<span data-ttu-id="15d71-111">Элемент управления DropShadow в наборе элементов управления AJAX расширяет панель с тенью.</span><span class="sxs-lookup"><span data-stu-id="15d71-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="15d71-112">Однако эта теневая иногда конфликтует с другими элементами управления, например элемент управления меню ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="15d71-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="15d71-113">При записи меню появится окно, она находится за тени.</span><span class="sxs-lookup"><span data-stu-id="15d71-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="15d71-114">Шаги</span><span class="sxs-lookup"><span data-stu-id="15d71-114">Steps</span></span>

<span data-ttu-id="15d71-115">Код начинается с элемента управления, содержащий достаточный фрагмент текста, чтобы панель содержит достаточный фрагмент текста для эффекта должен отображаться:</span><span class="sxs-lookup"><span data-stu-id="15d71-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample1.aspx)]

<span data-ttu-id="15d71-116">Другой панели помещается непосредственно перед `panelShadow` панели.</span><span class="sxs-lookup"><span data-stu-id="15d71-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="15d71-117">Он содержит меню в горизонтальном направлении, чтобы пункты меню отображаются над (или вместо: в списке) `dropShadow` панель):</span><span class="sxs-lookup"><span data-stu-id="15d71-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample2.aspx)]

<span data-ttu-id="15d71-118">Затем `DropShadowExtender` добавляется расширение `panelShadow` панель с тенью:</span><span class="sxs-lookup"><span data-stu-id="15d71-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample3.aspx)]

<span data-ttu-id="15d71-119">Наконец, ASP.NET AJAX `ScriptManager` управления включает набор средств управления для работы:</span><span class="sxs-lookup"><span data-stu-id="15d71-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample4.aspx)]

<span data-ttu-id="15d71-120">При выполнении этого скрипта, пункты меню отображаются под панели.</span><span class="sxs-lookup"><span data-stu-id="15d71-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="15d71-121">Однако меню использует класс CSS `panel` где просто нужно определить две вещи, чтобы сделать элементы отображаться перед другим панели:</span><span class="sxs-lookup"><span data-stu-id="15d71-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="15d71-122">Относительное расположение</span><span class="sxs-lookup"><span data-stu-id="15d71-122">Relative positioning</span></span>
- <span data-ttu-id="15d71-123">Положительное значение z индекса</span><span class="sxs-lookup"><span data-stu-id="15d71-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample5.css)]

<span data-ttu-id="15d71-124">Затем `DropShadowExtender` управления больше не конфликтуют с меню элемента управления.</span><span class="sxs-lookup"><span data-stu-id="15d71-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>


<span data-ttu-id="15d71-125">[![Ранее: Элемент меню не отображается](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="15d71-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)</span></span>

<span data-ttu-id="15d71-126">Ранее: Элемент меню не отображается ([Просмотр полноразмерное изображение](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="15d71-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))</span></span>


<span data-ttu-id="15d71-127">[![После: Запись меню появится](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="15d71-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)</span></span>

<span data-ttu-id="15d71-128">После: Запись меню появится ([Просмотр полноразмерное изображение](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="15d71-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="15d71-129">Вперед</span><span class="sxs-lookup"><span data-stu-id="15d71-129">Next</span></span>](manipulating-dropshadow-properties-from-client-code-cs.md)
