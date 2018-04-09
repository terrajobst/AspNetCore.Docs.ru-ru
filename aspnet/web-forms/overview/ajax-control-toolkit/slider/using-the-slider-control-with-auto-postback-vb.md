---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
title: С помощью элемента управления "ползунок" с автоматическая обратная передача (VB) | Документы Microsoft
author: wenz
description: Ползунок в наборе элементов управления AJAX имеет графический ползунок, можно управлять с помощью мыши. Это можно делать Автоматическая разноска ползунок...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 41d1abba-97a5-4a45-9b44-d05624c19777
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
msc.type: authoredcontent
ms.openlocfilehash: edb8fa13716c3c0beb7cf86dd3843caaec939483
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="using-the-slider-control-with-auto-postback-vb"></a><span data-ttu-id="5709e-104">С помощью элемента управления "ползунок" с автоматическая обратная передача (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="5709e-104">Using the Slider Control With Auto-Postback (VB)</span></span>
====================
<span data-ttu-id="5709e-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5709e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5709e-106">[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="5709e-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)</span></span>

> <span data-ttu-id="5709e-107">Ползунок в наборе элементов управления AJAX имеет графический ползунок, можно управлять с помощью мыши.</span><span class="sxs-lookup"><span data-stu-id="5709e-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="5709e-108">Это можно делать autopostback ползунок после изменения его значения.</span><span class="sxs-lookup"><span data-stu-id="5709e-108">It is possible to make the slider autopostback once its value changes.</span></span>


## <a name="overview"></a><span data-ttu-id="5709e-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="5709e-109">Overview</span></span>

<span data-ttu-id="5709e-110">Ползунок в наборе элементов управления AJAX имеет графический ползунок, можно управлять с помощью мыши.</span><span class="sxs-lookup"><span data-stu-id="5709e-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="5709e-111">Это можно делать autopostback ползунок после изменения его значения.</span><span class="sxs-lookup"><span data-stu-id="5709e-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="5709e-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="5709e-112">Steps</span></span>

<span data-ttu-id="5709e-113">Для упрощения ползунок автоматически обратного запроса на изменение, оба поля требуется атрибут `AutoPostBack="true"`: текстовое поле, которое станет ползунок самого и текстовое поле, которое содержит положение ползунка.</span><span class="sxs-lookup"><span data-stu-id="5709e-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="5709e-114">Вот разметки, необходимые для этого:</span><span class="sxs-lookup"><span data-stu-id="5709e-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample1.aspx)]

<span data-ttu-id="5709e-115">`SliderExtender` Управления из набора элементов управления ASP.NET AJAX назначает функциональность ползунок два текстовых поля:</span><span class="sxs-lookup"><span data-stu-id="5709e-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample2.aspx)]

<span data-ttu-id="5709e-116">Элемент Дополнительные метки позже будет использоваться для уведомления пользователя обратной передачи:</span><span class="sxs-lookup"><span data-stu-id="5709e-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample3.aspx)]

<span data-ttu-id="5709e-117">Наконец `ScriptManager` элемент управления AJAX для ASP.NET загружает набор средств управления для работы требуется JavaScript:</span><span class="sxs-lookup"><span data-stu-id="5709e-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample4.aspx)]

<span data-ttu-id="5709e-118">Теперь в учетный ползунок обратно; на стороне сервера это событие может выявить и ответных:</span><span class="sxs-lookup"><span data-stu-id="5709e-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample5.aspx)]


<span data-ttu-id="5709e-119">[![При перемещении ползунка вызывает обратную передачу](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5709e-119">[![Moving the slider triggers a postback](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)</span></span>

<span data-ttu-id="5709e-120">При перемещении ползунка вызывает обратную передачу ([Просмотр полноразмерное изображение](using-the-slider-control-with-auto-postback-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5709e-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-vb/_static/image3.png))</span></span>


<span data-ttu-id="5709e-121">[![После этого дата это изменение записывается в метке](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="5709e-121">[![Afterwards, the date of this change is written in the label](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)</span></span>

<span data-ttu-id="5709e-122">После этого дата это изменение записывается в метке ([Просмотр полноразмерное изображение](using-the-slider-control-with-auto-postback-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="5709e-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5709e-123">[Назад](databinding-the-slider-control-cs.md)
> [Вперед](databinding-the-slider-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="5709e-123">[Previous](databinding-the-slider-control-cs.md)
[Next](databinding-the-slider-control-vb.md)</span></span>
