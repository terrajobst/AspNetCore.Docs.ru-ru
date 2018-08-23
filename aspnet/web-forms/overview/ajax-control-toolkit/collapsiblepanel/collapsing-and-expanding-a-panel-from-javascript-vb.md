---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
title: Свертывание и развертывание панели из кода JavaScript (VB) | Документация Майкрософт
author: wenz
description: Элемент управления CollapsiblePanel в ASP.NET AJAX Control Toolkit расширяет панель и предоставляет ему возможность свернуть его содержимое и разверните его...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 298789b4-2964-49f5-a0a8-d4dbeb9ff2c2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: 1f80a6979966a887db0557b4f1b98570e10b1ab7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837933"
---
<a name="collapsing-and-expanding-a-panel-from-javascript-vb"></a><span data-ttu-id="5b100-103">Свертывание и развертывание панели из кода JavaScript (VB)</span><span class="sxs-lookup"><span data-stu-id="5b100-103">Collapsing and Expanding a Panel from JavaScript (VB)</span></span>
====================
<span data-ttu-id="5b100-104">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5b100-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5b100-105">[Скачать код](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="5b100-105">[Download Code](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1VB.pdf)</span></span>

> <span data-ttu-id="5b100-106">Элемент управления CollapsiblePanel в ASP.NET AJAX Control Toolkit расширяет панель и предоставляет ему возможность свернуть его содержимое и развернуть ее снова.</span><span class="sxs-lookup"><span data-stu-id="5b100-106">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="5b100-107">Эти два действия также может запускаться из пользовательского кода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5b100-107">These two actions can also be triggered from custom JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="5b100-108">Обзор</span><span class="sxs-lookup"><span data-stu-id="5b100-108">Overview</span></span>

<span data-ttu-id="5b100-109">Элемент управления CollapsiblePanel в ASP.NET AJAX Control Toolkit расширяет панель и предоставляет ему возможность свернуть его содержимое и развернуть ее снова.</span><span class="sxs-lookup"><span data-stu-id="5b100-109">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="5b100-110">Эти два действия также может запускаться из пользовательского кода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5b100-110">These two actions can also be triggered from custom JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="5b100-111">Шаги</span><span class="sxs-lookup"><span data-stu-id="5b100-111">Steps</span></span>

<span data-ttu-id="5b100-112">Во-первых, создайте новую страницу ASP.NET и включите `ScriptManager` внутри `<form>` элемент.</span><span class="sxs-lookup"><span data-stu-id="5b100-112">First of all, create a new ASP.NET page and include the `ScriptManager` within the one `<form>` element.</span></span> <span data-ttu-id="5b100-113">Это загружает библиотеки ASP.NET AJAX, который необходим набор элементов управления:</span><span class="sxs-lookup"><span data-stu-id="5b100-113">This loads the ASP.NET AJAX library which is required by the Control Toolkit:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample1.aspx)]

<span data-ttu-id="5b100-114">Затем создайте панели с определенным текстом, можно увидеть эффект развернут или свернут.</span><span class="sxs-lookup"><span data-stu-id="5b100-114">Then, create a panel with some text so that the collapse/expand effect can be seen:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample2.aspx)]

<span data-ttu-id="5b100-115">Как вы видите, панели ссылается на класс CSS, который приведен ниже (и по сути определяет фонового цвета и ширины панели):</span><span class="sxs-lookup"><span data-stu-id="5b100-115">As you can see, the panel references a CSS class which is shown here (and basically defines a background color and the panel's width):</span></span>

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample3.css)]

<span data-ttu-id="5b100-116">`CollapsiblePanelExtender` Элемент управления требует `TargetControlID` таким образом, набор средств знает, какая панель, чтобы свернуть или развернуть по запросу:</span><span class="sxs-lookup"><span data-stu-id="5b100-116">The `CollapsiblePanelExtender` control requires the `TargetControlID` attribute so that the toolkit knows which panel to collapse or expand upon request:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample4.aspx)]

<span data-ttu-id="5b100-117">К сожалению расширитель в настоящее время не предоставляет интерфейс API для сворачивать и разворачивать панели, но некоторые методы недокументированные происходит.</span><span class="sxs-lookup"><span data-stu-id="5b100-117">Unfortunately, the extender currently does not expose a specific API for collapsing or expanding the panel, but some undocumented methods will do.</span></span> <span data-ttu-id="5b100-118">Во-первых добавьте три кнопки HTML для страницы, который затем инициирует клиентским сценарием JavaScript, чтобы свернуть или развернуть содержимое панели:</span><span class="sxs-lookup"><span data-stu-id="5b100-118">First of all, add three HTML buttons to the page which will then trigger the client-side JavaScript to collapse or expand the panel's contents:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample5.aspx)]

<span data-ttu-id="5b100-119">В коде JavaScript на стороне клиента (к работе с `<script type="text/javascript">`), `$find()` метод должен использоваться для доступа к `CollapsiblePanelExtender`.</span><span class="sxs-lookup"><span data-stu-id="5b100-119">In the client-side JavaScript code (started with `<script type="text/javascript">`), the `$find()` method needs to be used to access the `CollapsiblePanelExtender`.</span></span> <span data-ttu-id="5b100-120">`$find("cpe")` Возвращает ссылку на него.</span><span class="sxs-lookup"><span data-stu-id="5b100-120">`$find("cpe")` will return a reference to it.</span></span> <span data-ttu-id="5b100-121">Оттуда на конкретные методы решит поставленной задачи.</span><span class="sxs-lookup"><span data-stu-id="5b100-121">From there on, specific methods will solve the task at hand.</span></span>

<span data-ttu-id="5b100-122">Метод для открытия (расширение) называется панели `_doOpen()`; в следующем коде реализуется `doOpen()` функция, вызываемая при нажатии первой кнопки:</span><span class="sxs-lookup"><span data-stu-id="5b100-122">The method for opening (expanding) the panel is called `_doOpen()`; the following code implements the `doOpen()` function called when the first button is clicked:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample6.js)]

<span data-ttu-id="5b100-123">Для закрытия или свертывания панели, `_doClose()` метод должен быть выполнен.</span><span class="sxs-lookup"><span data-stu-id="5b100-123">For closing, or collapsing the panel, the `_doClose()` method needs to be executed.</span></span> <span data-ttu-id="5b100-124">Поэтому когда пользователь щелкает второй кнопки, называется следующий код JavaScript:</span><span class="sxs-lookup"><span data-stu-id="5b100-124">So when the user clicks on the second button, the following JavaScript code is called:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample7.js)]

<span data-ttu-id="5b100-125">Третья кнопка переключает состояние панели: из свернуты развернут и наоборот.</span><span class="sxs-lookup"><span data-stu-id="5b100-125">The third button toggles the state of the panel: from collapsed to expanded, and vice versa.</span></span> <span data-ttu-id="5b100-126">`CollapsiblePanelExtender` Предоставляет `toggle()` метод, который делает именно это: изменяет состояние панели.</span><span class="sxs-lookup"><span data-stu-id="5b100-126">The `CollapsiblePanelExtender` exposes the `toggle()` method which does exactly that: reverses the state of the panel.</span></span> <span data-ttu-id="5b100-127">Однако имеется также другой подход (который внутренне используется `toggle()` метод): `get_Collapsed()` метод `CollapsiblePanelExtender()` сообщает о том, свернута ли панели или нет.</span><span class="sxs-lookup"><span data-stu-id="5b100-127">However there is also another approach (which is internally used by the `toggle()` method): The `get_Collapsed()` method of the `CollapsiblePanelExtender()` tells us whether the panel is collapsed or not.</span></span> <span data-ttu-id="5b100-128">В зависимости от возвращаемого значения функции, она затем либо развернут (`_doOpen()` метод) или свернут (`_doClose()`) метод:</span><span class="sxs-lookup"><span data-stu-id="5b100-128">Depending on the return value of this function, the panel is then either expanded (`_doOpen()` method) or collapsed (`_doClose()`) method:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample8.js)]


<span data-ttu-id="5b100-129">[![Третья кнопка изменяет состояние панели: из свернуть, чтобы развернутое и обратно](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5b100-129">[![The third button changes the state of the panel: from collapsed to expanded and back](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image1.png)</span></span>

<span data-ttu-id="5b100-130">Третья кнопка изменяет состояние панели: из свернуть, чтобы развернутые и назад ([Просмотр полноразмерного изображения](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5b100-130">The third button changes the state of the panel: from collapsed to expanded and back ([Click to view full-size image](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="5b100-131">Назад</span><span class="sxs-lookup"><span data-stu-id="5b100-131">Previous</span></span>](collapsing-and-expanding-a-panel-from-javascript-cs.md)
