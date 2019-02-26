---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
title: Обработка операций обратной передачи из элемента управления всплывающего окна без элемента управления UpdatePanel (VB) | Документация Майкрософт
author: wenz
description: Расширитель PopupControl в AJAX Control Toolkit предлагает простой способ активации всплывающего окна при активации любого другого элемента управления. При обратной передаче в единицах поиска...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a0b9186c-0912-4fff-916a-6d17e696a50b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: 6dcb2b09279ec6400465f79fadc2a1b6c72f8f07
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828546"
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-vb"></a><span data-ttu-id="8335f-104">Обработка операций обратной передачи из элемента управления всплывающего окна без элемента управления UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="8335f-104">Handling Postbacks from A Popup Control Without an UpdatePanel (VB)</span></span>
====================
<span data-ttu-id="8335f-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8335f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8335f-106">[Скачать код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="8335f-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)</span></span>

> <span data-ttu-id="8335f-107">Расширитель PopupControl в AJAX Control Toolkit предлагает простой способ активации всплывающего окна при активации любого другого элемента управления.</span><span class="sxs-lookup"><span data-stu-id="8335f-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="8335f-108">При обратной передаче такие панели и на странице имеется несколько панелей трудно определить, какая из панелей был выполнен щелчок.</span><span class="sxs-lookup"><span data-stu-id="8335f-108">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>


## <a name="overview"></a><span data-ttu-id="8335f-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="8335f-109">Overview</span></span>

<span data-ttu-id="8335f-110">Расширитель PopupControl в AJAX Control Toolkit предлагает простой способ активации всплывающего окна при активации любого другого элемента управления.</span><span class="sxs-lookup"><span data-stu-id="8335f-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="8335f-111">При обратной передаче такие панели и на странице имеется несколько панелей трудно определить, какая из панелей был выполнен щелчок.</span><span class="sxs-lookup"><span data-stu-id="8335f-111">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="steps"></a><span data-ttu-id="8335f-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="8335f-112">Steps</span></span>

<span data-ttu-id="8335f-113">При использовании `PopupControl` при обратной передаче, но без `UpdatePanel` набор элементов управления на странице не предлагает способ определить, какой элемент клиент активировал всплывающее окно, которое в свою очередь вызвало обратной передачи.</span><span class="sxs-lookup"><span data-stu-id="8335f-113">When using a `PopupControl` with a postback, but without having an `UpdatePanel` on the page, the Control Toolkit does not offer a way to determine which client element has triggered the popup which in turn caused the postback.</span></span> <span data-ttu-id="8335f-114">Тем не менее небольшую хитрость предоставляет обходной путь для этого сценария.</span><span class="sxs-lookup"><span data-stu-id="8335f-114">However a small trick provides a workaround for this scenario.</span></span>

<span data-ttu-id="8335f-115">Во-первых, вот базовую настройку: два текстовых поля, оба способа активации же всплывающего окна календаря.</span><span class="sxs-lookup"><span data-stu-id="8335f-115">First of all, here is the basic setup: two text boxes which both trigger the same popup, a calendar.</span></span> <span data-ttu-id="8335f-116">Два `PopupControlExtenders` объединяйте текстовые поля и всплывающего окна.</span><span class="sxs-lookup"><span data-stu-id="8335f-116">Two `PopupControlExtenders` bring text boxes and popup together.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample1.aspx)]

<span data-ttu-id="8335f-117">Основная идея заключается в том, чтобы добавить скрытого поля формы в &lt; `form` &gt; элемент, который содержит текстовое поле, которое запускается всплывающее окно:</span><span class="sxs-lookup"><span data-stu-id="8335f-117">The basic idea is to add a hidden form field in the &lt;`form`&gt; element that holds the text box which launched the popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample2.aspx)]

<span data-ttu-id="8335f-118">При загрузке страницы, кода JavaScript добавляет обработчик событий, чтобы оба поля: каждый раз, когда пользователь нажимает на текстовое поле, его имя, записываются в скрытое поле формы:</span><span class="sxs-lookup"><span data-stu-id="8335f-118">When the page is loaded, JavaScript code adds an event handler to both text boxes: Whenever the user clicks on a text box, its name is written into the hidden form field:</span></span>

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample3.html)]

<span data-ttu-id="8335f-119">В коде на стороне сервера необходимо считать значение скрытого поля.</span><span class="sxs-lookup"><span data-stu-id="8335f-119">In the server-side code, the value of the hidden field must be read.</span></span> <span data-ttu-id="8335f-120">Поскольку скрытые поля формы просты для управления, утвержденный список способов проверки скрытые значения является обязательным.</span><span class="sxs-lookup"><span data-stu-id="8335f-120">Since hidden form fields are trivial to manipulate, a whitelist approach to validate the hidden value is required.</span></span> <span data-ttu-id="8335f-121">После определения правильного текстовое поле, в него записывается дату из календаря.</span><span class="sxs-lookup"><span data-stu-id="8335f-121">Once the correct text box has been identified, the date from the calendar is written into it.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample4.aspx)]


<span data-ttu-id="8335f-122">[![Календарь появляется, когда пользователь нажимает кнопку в текстовое поле](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8335f-122">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)</span></span>

<span data-ttu-id="8335f-123">Календарь появляется, когда пользователь нажимает кнопку в текстовое поле ([Просмотр полноразмерного изображения](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8335f-123">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))</span></span>


<span data-ttu-id="8335f-124">[![Щелкните дату помещает его в текстовое поле](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="8335f-124">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)</span></span>

<span data-ttu-id="8335f-125">Щелкните дату помещает его в текстовое поле ([Просмотр полноразмерного изображения](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="8335f-125">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="8335f-126">Назад</span><span class="sxs-lookup"><span data-stu-id="8335f-126">Previous</span></span>](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
