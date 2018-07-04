---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
title: Обработка операций обратной передачи из элемента управления всплывающего окна без элемента управления UpdatePanel (VB) | Документация Майкрософт
author: wenz
description: Расширитель PopupControl в AJAX Control Toolkit предлагает простой способ активации всплывающего окна при активации любого другого элемента управления. При обратной передаче в единицах поиска...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a0b9186c-0912-4fff-916a-6d17e696a50b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: 5ef1d0300cba42eefb5f6cda77693aeafb93a31a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363848"
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-vb"></a><span data-ttu-id="45446-104">Обработка операций обратной передачи из элемента управления всплывающего окна без элемента управления UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="45446-104">Handling Postbacks from A Popup Control Without an UpdatePanel (VB)</span></span>
====================
<span data-ttu-id="45446-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="45446-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="45446-106">[Скачать код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="45446-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)</span></span>

> <span data-ttu-id="45446-107">Расширитель PopupControl в AJAX Control Toolkit предлагает простой способ активации всплывающего окна при активации любого другого элемента управления.</span><span class="sxs-lookup"><span data-stu-id="45446-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="45446-108">При обратной передаче такие панели и на странице имеется несколько панелей трудно определить, какая из панелей был выполнен щелчок.</span><span class="sxs-lookup"><span data-stu-id="45446-108">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>


## <a name="overview"></a><span data-ttu-id="45446-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="45446-109">Overview</span></span>

<span data-ttu-id="45446-110">Расширитель PopupControl в AJAX Control Toolkit предлагает простой способ активации всплывающего окна при активации любого другого элемента управления.</span><span class="sxs-lookup"><span data-stu-id="45446-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="45446-111">При обратной передаче такие панели и на странице имеется несколько панелей трудно определить, какая из панелей был выполнен щелчок.</span><span class="sxs-lookup"><span data-stu-id="45446-111">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="steps"></a><span data-ttu-id="45446-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="45446-112">Steps</span></span>

<span data-ttu-id="45446-113">При использовании `PopupControl` при обратной передаче, но без `UpdatePanel` набор элементов управления на странице не предлагает способ определить, какой элемент клиент активировал всплывающее окно, которое в свою очередь вызвало обратной передачи.</span><span class="sxs-lookup"><span data-stu-id="45446-113">When using a `PopupControl` with a postback, but without having an `UpdatePanel` on the page, the Control Toolkit does not offer a way to determine which client element has triggered the popup which in turn caused the postback.</span></span> <span data-ttu-id="45446-114">Тем не менее небольшую хитрость предоставляет обходной путь для этого сценария.</span><span class="sxs-lookup"><span data-stu-id="45446-114">However a small trick provides a workaround for this scenario.</span></span>

<span data-ttu-id="45446-115">Во-первых, вот базовую настройку: два текстовых поля, оба способа активации же всплывающего окна календаря.</span><span class="sxs-lookup"><span data-stu-id="45446-115">First of all, here is the basic setup: two text boxes which both trigger the same popup, a calendar.</span></span> <span data-ttu-id="45446-116">Два `PopupControlExtenders` объединяйте текстовые поля и всплывающего окна.</span><span class="sxs-lookup"><span data-stu-id="45446-116">Two `PopupControlExtenders` bring text boxes and popup together.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample1.aspx)]

<span data-ttu-id="45446-117">Основная идея заключается в том, чтобы добавить скрытого поля формы в &lt; `form` &gt; элемент, который содержит текстовое поле, которое запускается всплывающее окно:</span><span class="sxs-lookup"><span data-stu-id="45446-117">The basic idea is to add a hidden form field in the &lt;`form`&gt; element that holds the text box which launched the popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample2.aspx)]

<span data-ttu-id="45446-118">При загрузке страницы, кода JavaScript добавляет обработчик событий, чтобы оба поля: каждый раз, когда пользователь нажимает на текстовое поле, его имя, записываются в скрытое поле формы:</span><span class="sxs-lookup"><span data-stu-id="45446-118">When the page is loaded, JavaScript code adds an event handler to both text boxes: Whenever the user clicks on a text box, its name is written into the hidden form field:</span></span>

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample3.html)]

<span data-ttu-id="45446-119">В коде на стороне сервера необходимо считать значение скрытого поля.</span><span class="sxs-lookup"><span data-stu-id="45446-119">In the server-side code, the value of the hidden field must be read.</span></span> <span data-ttu-id="45446-120">Поскольку скрытые поля формы просты для управления, белый список способов проверки скрытые значения является обязательным.</span><span class="sxs-lookup"><span data-stu-id="45446-120">Since hidden form fields are trivial to manipulate, a whitelist approach to validate the hidden value is required.</span></span> <span data-ttu-id="45446-121">После определения правильного текстовое поле, в него записывается дату из календаря.</span><span class="sxs-lookup"><span data-stu-id="45446-121">Once the correct text box has been identified, the date from the calendar is written into it.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample4.aspx)]


<span data-ttu-id="45446-122">[![Календарь появляется, когда пользователь нажимает кнопку в текстовое поле](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="45446-122">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)</span></span>

<span data-ttu-id="45446-123">Календарь появляется, когда пользователь нажимает кнопку в текстовое поле ([Просмотр полноразмерного изображения](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="45446-123">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))</span></span>


<span data-ttu-id="45446-124">[![Щелкните дату помещает его в текстовое поле](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="45446-124">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)</span></span>

<span data-ttu-id="45446-125">Щелкните дату помещает его в текстовое поле ([Просмотр полноразмерного изображения](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="45446-125">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="45446-126">Назад</span><span class="sxs-lookup"><span data-stu-id="45446-126">Previous</span></span>](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
