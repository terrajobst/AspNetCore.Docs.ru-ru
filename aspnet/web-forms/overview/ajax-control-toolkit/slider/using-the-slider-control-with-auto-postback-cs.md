---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: С помощью элемента управления "ползунок" с автоматическая обратная передача (C#) | Документы Microsoft
author: wenz
description: Ползунок в наборе элементов управления AJAX имеет графический ползунок, можно управлять с помощью мыши. Это можно делать Автоматическая разноска ползунок...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: e347d20c5c2ee48e6ed801e95459af6f0bcd2667
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879248"
---
<a name="using-the-slider-control-with-auto-postback-c"></a><span data-ttu-id="4a33c-104">С помощью элемента управления "ползунок" с автоматическая обратная передача (C#)</span><span class="sxs-lookup"><span data-stu-id="4a33c-104">Using the Slider Control With Auto-Postback (C#)</span></span>
====================
<span data-ttu-id="4a33c-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4a33c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4a33c-106">[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="4a33c-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span></span>

> <span data-ttu-id="4a33c-107">Ползунок в наборе элементов управления AJAX имеет графический ползунок, можно управлять с помощью мыши.</span><span class="sxs-lookup"><span data-stu-id="4a33c-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="4a33c-108">Это можно делать autopostback ползунок после изменения его значения.</span><span class="sxs-lookup"><span data-stu-id="4a33c-108">It is possible to make the slider autopostback once its value changes.</span></span>


## <a name="overview"></a><span data-ttu-id="4a33c-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="4a33c-109">Overview</span></span>

<span data-ttu-id="4a33c-110">Ползунок в наборе элементов управления AJAX имеет графический ползунок, можно управлять с помощью мыши.</span><span class="sxs-lookup"><span data-stu-id="4a33c-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="4a33c-111">Это можно делать autopostback ползунок после изменения его значения.</span><span class="sxs-lookup"><span data-stu-id="4a33c-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="4a33c-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="4a33c-112">Steps</span></span>

<span data-ttu-id="4a33c-113">Для упрощения ползунок автоматически обратного запроса на изменение, оба поля требуется атрибут `AutoPostBack="true"`: текстовое поле, которое станет ползунок самого и текстовое поле, которое содержит положение ползунка.</span><span class="sxs-lookup"><span data-stu-id="4a33c-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="4a33c-114">Вот разметки, необходимые для этого:</span><span class="sxs-lookup"><span data-stu-id="4a33c-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

<span data-ttu-id="4a33c-115">`SliderExtender` Управления из набора элементов управления ASP.NET AJAX назначает функциональность ползунок два текстовых поля:</span><span class="sxs-lookup"><span data-stu-id="4a33c-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

<span data-ttu-id="4a33c-116">Элемент Дополнительные метки позже будет использоваться для уведомления пользователя обратной передачи:</span><span class="sxs-lookup"><span data-stu-id="4a33c-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

<span data-ttu-id="4a33c-117">Наконец `ScriptManager` элемент управления AJAX для ASP.NET загружает набор средств управления для работы требуется JavaScript:</span><span class="sxs-lookup"><span data-stu-id="4a33c-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

<span data-ttu-id="4a33c-118">Теперь в учетный ползунок обратно; на стороне сервера это событие может выявить и ответных:</span><span class="sxs-lookup"><span data-stu-id="4a33c-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]


<span data-ttu-id="4a33c-119">[![При перемещении ползунка вызывает обратную передачу](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4a33c-119">[![Moving the slider triggers a postback](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span></span>

<span data-ttu-id="4a33c-120">При перемещении ползунка вызывает обратную передачу ([Просмотр полноразмерное изображение](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4a33c-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span></span>


<span data-ttu-id="4a33c-121">[![После этого дата это изменение записывается в метке](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="4a33c-121">[![Afterwards, the date of this change is written in the label](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span></span>

<span data-ttu-id="4a33c-122">После этого дата это изменение записывается в метке ([Просмотр полноразмерное изображение](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="4a33c-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4a33c-123">Вперед</span><span class="sxs-lookup"><span data-stu-id="4a33c-123">Next</span></span>](databinding-the-slider-control-cs.md)
