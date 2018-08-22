---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: Использование нескольких элементов управления Popup (VB) | Документация Майкрософт
author: wenz
description: Расширитель PopupControl в AJAX Control Toolkit предлагает простой способ активации всплывающего окна при активации любого другого элемента управления. Можно также использовать m...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 81b0dbd794d31bc3c55bff4ba8110c590b88b509
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829815"
---
<a name="using-multiple-popup-controls-vb"></a><span data-ttu-id="159a4-104">Использование нескольких элементов управления Popup (VB)</span><span class="sxs-lookup"><span data-stu-id="159a4-104">Using Multiple Popup Controls (VB)</span></span>
====================
<span data-ttu-id="159a4-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="159a4-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="159a4-106">[Скачать код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="159a4-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span></span>

> <span data-ttu-id="159a4-107">Расширитель PopupControl в AJAX Control Toolkit предлагает простой способ активации всплывающего окна при активации любого другого элемента управления.</span><span class="sxs-lookup"><span data-stu-id="159a4-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="159a4-108">Можно также использовать более одного элемента управления всплывающего окна на одной странице.</span><span class="sxs-lookup"><span data-stu-id="159a4-108">It is also possible to use more than one popup control on one page.</span></span>


## <a name="overview"></a><span data-ttu-id="159a4-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="159a4-109">Overview</span></span>

<span data-ttu-id="159a4-110">Расширитель PopupControl в AJAX Control Toolkit предлагает простой способ активации всплывающего окна при активации любого другого элемента управления.</span><span class="sxs-lookup"><span data-stu-id="159a4-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="159a4-111">Можно также использовать более одного элемента управления всплывающего окна на одной странице.</span><span class="sxs-lookup"><span data-stu-id="159a4-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="159a4-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="159a4-112">Steps</span></span>

<span data-ttu-id="159a4-113">Для активации функции ASP.NET AJAX и Control Toolkit, `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемента):</span><span class="sxs-lookup"><span data-stu-id="159a4-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

<span data-ttu-id="159a4-114">Добавьте панель, который служит в качестве всплывающего окна.</span><span class="sxs-lookup"><span data-stu-id="159a4-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="159a4-115">В данном способе панель содержит `Calendar` элемента управления.</span><span class="sxs-lookup"><span data-stu-id="159a4-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="159a4-116">Во избежание обновления страниц, вызванных календаря обратных передач, панели помещается в `UpdatePanel` управления:</span><span class="sxs-lookup"><span data-stu-id="159a4-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

<span data-ttu-id="159a4-117">Страница также содержит два текстовых поля.</span><span class="sxs-lookup"><span data-stu-id="159a4-117">The page also contains two text boxes.</span></span> <span data-ttu-id="159a4-118">Для каждого текстового поля всплывающего окна календаря появляться после активации текстового поля.</span><span class="sxs-lookup"><span data-stu-id="159a4-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

<span data-ttu-id="159a4-119">Теперь Расширьте каждый из двух текстовых полях с `PopupControlExtender`.</span><span class="sxs-lookup"><span data-stu-id="159a4-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="159a4-120">`TargetControlID` Атрибут содержит идентификатор элемента управления, привязанных к расширителя.</span><span class="sxs-lookup"><span data-stu-id="159a4-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="159a4-121">`PopupControlID` Атрибут содержит идентификатор всплывающей панели.</span><span class="sxs-lookup"><span data-stu-id="159a4-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="159a4-122">В этом случае оба расширителей Показать той же панели, но различных панелях, возможно, также.</span><span class="sxs-lookup"><span data-stu-id="159a4-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

<span data-ttu-id="159a4-123">Теперь при каждом нажатии в текстовом поле появляется календарь под полем, где можно выбрать дату.</span><span class="sxs-lookup"><span data-stu-id="159a4-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="159a4-124">(Получение выбранную дату в текстовые поля будут освещены в другого учебника.)</span><span class="sxs-lookup"><span data-stu-id="159a4-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>


<span data-ttu-id="159a4-125">[![Календарь появляется, когда пользователь нажимает кнопку в текстовое поле](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="159a4-125">[![The Calendar appears when the user clicks into the textbox](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span></span>

<span data-ttu-id="159a4-126">Календарь появляется, когда пользователь нажимает кнопку в текстовое поле ([Просмотр полноразмерного изображения](using-multiple-popup-controls-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="159a4-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="159a4-127">[Назад](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [Вперед](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="159a4-127">[Previous](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
[Next](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span></span>
