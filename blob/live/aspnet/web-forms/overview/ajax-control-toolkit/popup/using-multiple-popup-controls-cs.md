---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
title: "Использование нескольких элементов управления всплывающее окно (C#) | Документы Microsoft"
author: wenz
description: "Расширитель PopupControl в наборе элементов управления AJAX предлагает простой способ запуска всплывающего окна при активации любого другого элемента управления. Можно также использовать m..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 91511b0b-311d-481f-9e7c-73f07b813b79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: d8c9742bb39b655115cb1862ff6e867989e63665
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="using-multiple-popup-controls-c"></a><span data-ttu-id="639c2-104">Использование нескольких элементов управления всплывающее окно (C#)</span><span class="sxs-lookup"><span data-stu-id="639c2-104">Using Multiple Popup Controls (C#)</span></span>
====================
<span data-ttu-id="639c2-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="639c2-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="639c2-106">[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="639c2-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)</span></span>

> <span data-ttu-id="639c2-107">Расширитель PopupControl в наборе элементов управления AJAX предлагает простой способ запуска всплывающего окна при активации любого другого элемента управления.</span><span class="sxs-lookup"><span data-stu-id="639c2-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="639c2-108">Можно также использовать более одного элемента управления контекстного меню на одной странице.</span><span class="sxs-lookup"><span data-stu-id="639c2-108">It is also possible to use more than one popup control on one page.</span></span>


## <a name="overview"></a><span data-ttu-id="639c2-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="639c2-109">Overview</span></span>

<span data-ttu-id="639c2-110">Расширитель PopupControl в наборе элементов управления AJAX предлагает простой способ запуска всплывающего окна при активации любого другого элемента управления.</span><span class="sxs-lookup"><span data-stu-id="639c2-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="639c2-111">Можно также использовать более одного элемента управления контекстного меню на одной странице.</span><span class="sxs-lookup"><span data-stu-id="639c2-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="639c2-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="639c2-112">Steps</span></span>

<span data-ttu-id="639c2-113">Чтобы активировать функциональные возможности ASP.NET AJAX и набора средств управления `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемент):</span><span class="sxs-lookup"><span data-stu-id="639c2-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample1.aspx)]

<span data-ttu-id="639c2-114">Добавьте панель, который служит в качестве контекстного меню.</span><span class="sxs-lookup"><span data-stu-id="639c2-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="639c2-115">В текущий сценарий содержит панели `Calendar` элемента управления.</span><span class="sxs-lookup"><span data-stu-id="639c2-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="639c2-116">Во избежание обновления страниц, вызванных обратных передач календаря, панель помещается внутри `UpdatePanel` управления:</span><span class="sxs-lookup"><span data-stu-id="639c2-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample2.aspx)]

<span data-ttu-id="639c2-117">Данная страница также содержит два текстовых поля.</span><span class="sxs-lookup"><span data-stu-id="639c2-117">The page also contains two text boxes.</span></span> <span data-ttu-id="639c2-118">Для каждого текстового поля всплывающего окна календаря должны отображаются при активации в текстовом поле.</span><span class="sxs-lookup"><span data-stu-id="639c2-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample3.aspx)]

<span data-ttu-id="639c2-119">Теперь Расширьте каждый из двух текстовых полей с `PopupControlExtender`.</span><span class="sxs-lookup"><span data-stu-id="639c2-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="639c2-120">`TargetControlID` Атрибут содержит идентификатор элемента управления, привязанного к модулю расширения.</span><span class="sxs-lookup"><span data-stu-id="639c2-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="639c2-121">`PopupControlID` Атрибут содержит идентификатор всплывающей панели.</span><span class="sxs-lookup"><span data-stu-id="639c2-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="639c2-122">В этом случае оба расширителей Показать той же панели, но различных панелях, возможно, также.</span><span class="sxs-lookup"><span data-stu-id="639c2-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample4.aspx)]

<span data-ttu-id="639c2-123">Теперь при щелчке в текстовом поле появляется календарь под полем, где можно выбрать дату.</span><span class="sxs-lookup"><span data-stu-id="639c2-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="639c2-124">(Возвращались в текстовых полях выбранной даты будет рассматриваться в другого учебника.)</span><span class="sxs-lookup"><span data-stu-id="639c2-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>


<span data-ttu-id="639c2-125">[![Календарь появляется, когда пользователь щелкает в текстовое поле](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="639c2-125">[![The Calendar appears when the user clicks into the textbox](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)</span></span>

<span data-ttu-id="639c2-126">Календарь появляется, когда пользователь щелкает в текстовое поле ([Просмотр полноразмерное изображение](using-multiple-popup-controls-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="639c2-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="639c2-127">Вперед</span><span class="sxs-lookup"><span data-stu-id="639c2-127">Next</span></span>](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
