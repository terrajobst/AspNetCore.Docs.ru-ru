---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
title: Обработка операций обратной передачи из контекстного меню элемента управления без UpdatePanel (VB) | Документы Microsoft
author: wenz
description: Расширитель PopupControl в наборе элементов управления AJAX предлагает простой способ запуска всплывающего окна при активации любого другого элемента управления. При обратной передаче в su...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a0b9186c-0912-4fff-916a-6d17e696a50b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: c92083d4a57067c7b646721f21af059fc1e1e7d9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-vb"></a><span data-ttu-id="c4c9f-104">Обработка операций обратной передачи из контекстного меню элемента управления без UpdatePanel (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="c4c9f-104">Handling Postbacks from A Popup Control Without an UpdatePanel (VB)</span></span>
====================
<span data-ttu-id="c4c9f-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c4c9f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c4c9f-106">[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="c4c9f-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)</span></span>

> <span data-ttu-id="c4c9f-107">Расширитель PopupControl в наборе элементов управления AJAX предлагает простой способ запуска всплывающего окна при активации любого другого элемента управления.</span><span class="sxs-lookup"><span data-stu-id="c4c9f-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="c4c9f-108">При обратной передаче таких панели и на странице имеется несколько панелей бывает трудно определить, какая панель была нажата.</span><span class="sxs-lookup"><span data-stu-id="c4c9f-108">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>


## <a name="overview"></a><span data-ttu-id="c4c9f-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="c4c9f-109">Overview</span></span>

<span data-ttu-id="c4c9f-110">Расширитель PopupControl в наборе элементов управления AJAX предлагает простой способ запуска всплывающего окна при активации любого другого элемента управления.</span><span class="sxs-lookup"><span data-stu-id="c4c9f-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="c4c9f-111">При обратной передаче таких панели и на странице имеется несколько панелей бывает трудно определить, какая панель была нажата.</span><span class="sxs-lookup"><span data-stu-id="c4c9f-111">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="steps"></a><span data-ttu-id="c4c9f-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="c4c9f-112">Steps</span></span>

<span data-ttu-id="c4c9f-113">При использовании `PopupControl` при обратной передаче, но без `UpdatePanel` набор элементов управления на странице не предлагает способ определить, какой элемент клиента инициировал контекстного меню, который в свою очередь, вызвавшего обратную передачу.</span><span class="sxs-lookup"><span data-stu-id="c4c9f-113">When using a `PopupControl` with a postback, but without having an `UpdatePanel` on the page, the Control Toolkit does not offer a way to determine which client element has triggered the popup which in turn caused the postback.</span></span> <span data-ttu-id="c4c9f-114">Однако небольшой прием обеспечивает временное решение для этого сценария.</span><span class="sxs-lookup"><span data-stu-id="c4c9f-114">However a small trick provides a workaround for this scenario.</span></span>

<span data-ttu-id="c4c9f-115">Во-первых, вот базовая настройка: два текстовых поля, для которых триггера же всплывающем календаре.</span><span class="sxs-lookup"><span data-stu-id="c4c9f-115">First of all, here is the basic setup: two text boxes which both trigger the same popup, a calendar.</span></span> <span data-ttu-id="c4c9f-116">Два `PopupControlExtenders` объединяйте текстовые поля и контекстного меню.</span><span class="sxs-lookup"><span data-stu-id="c4c9f-116">Two `PopupControlExtenders` bring text boxes and popup together.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample1.aspx)]

<span data-ttu-id="c4c9f-117">Основная идея заключается в том, чтобы добавить скрытое поле формы в &lt; `form` &gt; элементом, содержащим текстовое поле, которое запускается всплывающее окно:</span><span class="sxs-lookup"><span data-stu-id="c4c9f-117">The basic idea is to add a hidden form field in the &lt;`form`&gt; element that holds the text box which launched the popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample2.aspx)]

<span data-ttu-id="c4c9f-118">При загрузке страницы, кода JavaScript добавляет обработчик событий в оба поля: когда пользователь нажимает на текстовое поле, его имя записывается в скрытое поле формы:</span><span class="sxs-lookup"><span data-stu-id="c4c9f-118">When the page is loaded, JavaScript code adds an event handler to both text boxes: Whenever the user clicks on a text box, its name is written into the hidden form field:</span></span>

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample3.html)]

<span data-ttu-id="c4c9f-119">В серверный код необходимо считать значение скрытого поля.</span><span class="sxs-lookup"><span data-stu-id="c4c9f-119">In the server-side code, the value of the hidden field must be read.</span></span> <span data-ttu-id="c4c9f-120">Поскольку скрытые поля формы просты для работы, белого списка способ проверить скрытые значение является обязательным.</span><span class="sxs-lookup"><span data-stu-id="c4c9f-120">Since hidden form fields are trivial to manipulate, a whitelist approach to validate the hidden value is required.</span></span> <span data-ttu-id="c4c9f-121">После определения правильного текстовое поле, в него записывается дату из календаря.</span><span class="sxs-lookup"><span data-stu-id="c4c9f-121">Once the correct text box has been identified, the date from the calendar is written into it.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample4.aspx)]


<span data-ttu-id="c4c9f-122">[![Календарь появляется, когда пользователь щелкает в текстовое поле](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c4c9f-122">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)</span></span>

<span data-ttu-id="c4c9f-123">Календарь появляется, когда пользователь щелкает в текстовое поле ([Просмотр полноразмерное изображение](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c4c9f-123">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))</span></span>


<span data-ttu-id="c4c9f-124">[![Щелкните дату помещает его в текстовое поле](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="c4c9f-124">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)</span></span>

<span data-ttu-id="c4c9f-125">Щелкните дату помещает его в текстовое поле ([Просмотр полноразмерное изображение](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="c4c9f-125">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c4c9f-126">Назад</span><span class="sxs-lookup"><span data-stu-id="c4c9f-126">Previous</span></span>](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
