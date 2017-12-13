---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: "Обработка операций обратной передачи из контекстного меню элемента управления без UpdatePanel (C#) | Документы Microsoft"
author: wenz
description: "Расширитель PopupControl в наборе элементов управления AJAX предлагает простой способ запуска всплывающего окна при активации любого другого элемента управления. При обратной передаче в su..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: df43052950b6186908fe1baf04808f40cb926f69
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a><span data-ttu-id="7c919-104">Обработка операций обратной передачи из контекстного меню элемента управления без UpdatePanel (C#)</span><span class="sxs-lookup"><span data-stu-id="7c919-104">Handling Postbacks from A Popup Control Without an UpdatePanel (C#)</span></span>
====================
<span data-ttu-id="7c919-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7c919-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7c919-106">[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="7c919-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)</span></span>

> <span data-ttu-id="7c919-107">Расширитель PopupControl в наборе элементов управления AJAX предлагает простой способ запуска всплывающего окна при активации любого другого элемента управления.</span><span class="sxs-lookup"><span data-stu-id="7c919-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="7c919-108">При обратной передаче таких панели и на странице имеется несколько панелей бывает трудно определить, какая панель была нажата.</span><span class="sxs-lookup"><span data-stu-id="7c919-108">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>


## <a name="overview"></a><span data-ttu-id="7c919-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="7c919-109">Overview</span></span>

<span data-ttu-id="7c919-110">Расширитель PopupControl в наборе элементов управления AJAX предлагает простой способ запуска всплывающего окна при активации любого другого элемента управления.</span><span class="sxs-lookup"><span data-stu-id="7c919-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="7c919-111">При обратной передаче таких панели и на странице имеется несколько панелей бывает трудно определить, какая панель была нажата.</span><span class="sxs-lookup"><span data-stu-id="7c919-111">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="steps"></a><span data-ttu-id="7c919-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="7c919-112">Steps</span></span>

<span data-ttu-id="7c919-113">При использовании `PopupControl` при обратной передаче, но без `UpdatePanel` набор элементов управления на странице не предлагает способ определить, какой элемент клиента инициировал контекстного меню, который в свою очередь, вызвавшего обратную передачу.</span><span class="sxs-lookup"><span data-stu-id="7c919-113">When using a `PopupControl` with a postback, but without having an `UpdatePanel` on the page, the Control Toolkit does not offer a way to determine which client element has triggered the popup which in turn caused the postback.</span></span> <span data-ttu-id="7c919-114">Однако небольшой прием обеспечивает временное решение для этого сценария.</span><span class="sxs-lookup"><span data-stu-id="7c919-114">However a small trick provides a workaround for this scenario.</span></span>

<span data-ttu-id="7c919-115">Во-первых, вот базовая настройка: два текстовых поля, для которых триггера же всплывающем календаре.</span><span class="sxs-lookup"><span data-stu-id="7c919-115">First of all, here is the basic setup: two text boxes which both trigger the same popup, a calendar.</span></span> <span data-ttu-id="7c919-116">Два `PopupControlExtenders` объединяйте текстовые поля и контекстного меню.</span><span class="sxs-lookup"><span data-stu-id="7c919-116">Two `PopupControlExtenders` bring text boxes and popup together.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

<span data-ttu-id="7c919-117">Основная идея заключается в том, чтобы добавить скрытое поле формы в &lt; `form` &gt; элементом, содержащим текстовое поле, которое запускается всплывающее окно:</span><span class="sxs-lookup"><span data-stu-id="7c919-117">The basic idea is to add a hidden form field in the &lt;`form`&gt; element that holds the text box which launched the popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

<span data-ttu-id="7c919-118">При загрузке страницы, кода JavaScript добавляет обработчик событий в оба поля: когда пользователь нажимает на текстовое поле, его имя записывается в скрытое поле формы:</span><span class="sxs-lookup"><span data-stu-id="7c919-118">When the page is loaded, JavaScript code adds an event handler to both text boxes: Whenever the user clicks on a text box, its name is written into the hidden form field:</span></span>

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

<span data-ttu-id="7c919-119">В серверный код необходимо считать значение скрытого поля.</span><span class="sxs-lookup"><span data-stu-id="7c919-119">In the server-side code, the value of the hidden field must be read.</span></span> <span data-ttu-id="7c919-120">Поскольку скрытые поля формы просты для работы, белого списка способ проверить скрытые значение является обязательным.</span><span class="sxs-lookup"><span data-stu-id="7c919-120">Since hidden form fields are trivial to manipulate, a whitelist approach to validate the hidden value is required.</span></span> <span data-ttu-id="7c919-121">После определения правильного текстовое поле, в него записывается дату из календаря.</span><span class="sxs-lookup"><span data-stu-id="7c919-121">Once the correct text box has been identified, the date from the calendar is written into it.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]


<span data-ttu-id="7c919-122">[![Календарь появляется, когда пользователь щелкает в текстовое поле](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7c919-122">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)</span></span>

<span data-ttu-id="7c919-123">Календарь появляется, когда пользователь щелкает в текстовое поле ([Просмотр полноразмерное изображение](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7c919-123">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))</span></span>


<span data-ttu-id="7c919-124">[![Щелкните дату помещает его в текстовое поле](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="7c919-124">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)</span></span>

<span data-ttu-id="7c919-125">Щелкните дату помещает его в текстовое поле ([Просмотр полноразмерное изображение](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="7c919-125">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="7c919-126">[Назад](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
[Вперед](using-multiple-popup-controls-vb.md)</span><span class="sxs-lookup"><span data-stu-id="7c919-126">[Previous](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
[Next](using-multiple-popup-controls-vb.md)</span></span>
