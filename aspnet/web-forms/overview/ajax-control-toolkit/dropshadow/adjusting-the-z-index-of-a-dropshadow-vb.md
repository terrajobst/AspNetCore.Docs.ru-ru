---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
title: Настройка Z-индекса DropShadow (VB) | Документация Майкрософт
author: wenz
description: Элемент управления DropShadow в AJAX Control Toolkit расширяет панель с эффектом отбрасывания тени. Тем не менее Эта теневая иногда конфликтует с другими элементами управления, для пап...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ecb004b5-82c0-44fb-bcaf-233fffac6195
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
msc.type: authoredcontent
ms.openlocfilehash: a900a074e1507965e7d60e8de4202de57dc6180e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827788"
---
<a name="adjusting-the-z-index-of-a-dropshadow-vb"></a><span data-ttu-id="d6a80-104">Настройка Z-индекса DropShadow (VB)</span><span class="sxs-lookup"><span data-stu-id="d6a80-104">Adjusting the Z-Index of a DropShadow (VB)</span></span>
====================
<span data-ttu-id="d6a80-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d6a80-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d6a80-106">[Скачать код](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="d6a80-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span></span>

> <span data-ttu-id="d6a80-107">Элемент управления DropShadow в AJAX Control Toolkit расширяет панель с эффектом отбрасывания тени.</span><span class="sxs-lookup"><span data-stu-id="d6a80-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="d6a80-108">Тем не менее Эта теневая иногда конфликтует с другими элементами управления, например элемент управления ASP.NET Menu.</span><span class="sxs-lookup"><span data-stu-id="d6a80-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="d6a80-109">Когда запись меню появляется его фоне тени.</span><span class="sxs-lookup"><span data-stu-id="d6a80-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>


## <a name="overview"></a><span data-ttu-id="d6a80-110">Обзор</span><span class="sxs-lookup"><span data-stu-id="d6a80-110">Overview</span></span>

<span data-ttu-id="d6a80-111">Элемент управления DropShadow в AJAX Control Toolkit расширяет панель с эффектом отбрасывания тени.</span><span class="sxs-lookup"><span data-stu-id="d6a80-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="d6a80-112">Тем не менее Эта теневая иногда конфликтует с другими элементами управления, например элемент управления ASP.NET Menu.</span><span class="sxs-lookup"><span data-stu-id="d6a80-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="d6a80-113">Когда запись меню появляется его фоне тени.</span><span class="sxs-lookup"><span data-stu-id="d6a80-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="d6a80-114">Шаги</span><span class="sxs-lookup"><span data-stu-id="d6a80-114">Steps</span></span>

<span data-ttu-id="d6a80-115">Код начинается с элемента управления, содержащий достаточного объема текста, чтобы панель содержит достаточного объема текста для эффекта, который должен отображаться:</span><span class="sxs-lookup"><span data-stu-id="d6a80-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample1.aspx)]

<span data-ttu-id="d6a80-116">Другую панель помещается непосредственно перед `panelShadow` панели.</span><span class="sxs-lookup"><span data-stu-id="d6a80-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="d6a80-117">Она включает меню в горизонтальном направлении, чтобы пункты меню отображаются над (вернее: в разделе) `dropShadow` панель):</span><span class="sxs-lookup"><span data-stu-id="d6a80-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample2.aspx)]

<span data-ttu-id="d6a80-118">Затем `DropShadowExtender` добавляется расширение `panelShadow` панели с помощью эффекта тени:</span><span class="sxs-lookup"><span data-stu-id="d6a80-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample3.aspx)]

<span data-ttu-id="d6a80-119">Наконец, AJAX ASP.NET `ScriptManager` управления включает набор средств управления для работы:</span><span class="sxs-lookup"><span data-stu-id="d6a80-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample4.aspx)]

<span data-ttu-id="d6a80-120">При выполнении этого скрипта, пункты меню отображаются под панели.</span><span class="sxs-lookup"><span data-stu-id="d6a80-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="d6a80-121">Тем не менее меню используется класс CSS `panel` где вам просто нужно определить два действия, чтобы сделать элементы отображаются перед элементами на другие панели:</span><span class="sxs-lookup"><span data-stu-id="d6a80-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="d6a80-122">Относительное расположение</span><span class="sxs-lookup"><span data-stu-id="d6a80-122">Relative positioning</span></span>
- <span data-ttu-id="d6a80-123">Положительное значение z индекса</span><span class="sxs-lookup"><span data-stu-id="d6a80-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample5.css)]

<span data-ttu-id="d6a80-124">Затем `DropShadowExtender` управления больше не конфликтуют с меню элемента управления.</span><span class="sxs-lookup"><span data-stu-id="d6a80-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>


<span data-ttu-id="d6a80-125">[![Ранее: Элемент меню не отображается](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d6a80-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span></span>

<span data-ttu-id="d6a80-126">Ранее: Элемент меню не отображается ([Просмотр полноразмерного изображения](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d6a80-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span></span>


<span data-ttu-id="d6a80-127">[![Стало: Появляется запись меню](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="d6a80-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span></span>

<span data-ttu-id="d6a80-128">Стало: Запись меню появится ([Просмотр полноразмерного изображения](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="d6a80-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d6a80-129">[Назад](manipulating-dropshadow-properties-from-client-code-cs.md)
> [Вперед](manipulating-dropshadow-properties-from-client-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="d6a80-129">[Previous](manipulating-dropshadow-properties-from-client-code-cs.md)
[Next](manipulating-dropshadow-properties-from-client-code-vb.md)</span></span>
