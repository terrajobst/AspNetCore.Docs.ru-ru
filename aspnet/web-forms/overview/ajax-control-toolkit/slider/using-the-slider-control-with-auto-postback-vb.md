---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
title: С помощью элемента управления Slider с автоматической обратной передачи (Visual Basic) | Документация Майкрософт
author: wenz
description: Элемент управления "ползунок" в AJAX Control Toolkit предоставляет графическим ползунком, которые могут контролироваться с помощью мыши. Это можно делать Автоматическая разноска "ползунок"...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 41d1abba-97a5-4a45-9b44-d05624c19777
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
msc.type: authoredcontent
ms.openlocfilehash: 403e8fa6e54d84eb091769cbdb6ef1003ad4245c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835430"
---
<a name="using-the-slider-control-with-auto-postback-vb"></a><span data-ttu-id="eccfc-104">С помощью элемента управления Slider с автоматической обратной передачей (VB)</span><span class="sxs-lookup"><span data-stu-id="eccfc-104">Using the Slider Control With Auto-Postback (VB)</span></span>
====================
<span data-ttu-id="eccfc-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="eccfc-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="eccfc-106">[Скачать код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="eccfc-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)</span></span>

> <span data-ttu-id="eccfc-107">Элемент управления "ползунок" в AJAX Control Toolkit предоставляет графическим ползунком, которые могут контролироваться с помощью мыши.</span><span class="sxs-lookup"><span data-stu-id="eccfc-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="eccfc-108">Это можно делать autopostback ползунок один раз изменении ее значения.</span><span class="sxs-lookup"><span data-stu-id="eccfc-108">It is possible to make the slider autopostback once its value changes.</span></span>


## <a name="overview"></a><span data-ttu-id="eccfc-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="eccfc-109">Overview</span></span>

<span data-ttu-id="eccfc-110">Элемент управления "ползунок" в AJAX Control Toolkit предоставляет графическим ползунком, которые могут контролироваться с помощью мыши.</span><span class="sxs-lookup"><span data-stu-id="eccfc-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="eccfc-111">Это можно делать autopostback ползунок один раз изменении ее значения.</span><span class="sxs-lookup"><span data-stu-id="eccfc-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="eccfc-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="eccfc-112">Steps</span></span>

<span data-ttu-id="eccfc-113">Чтобы сделать ползунок автоматически обратной передачи, при изменении, оба поля атрибут должен быть добавлен `AutoPostBack="true"`: текстовое поле, которое станет самой ползунок и текстовое поле, которое содержит положение ползунка.</span><span class="sxs-lookup"><span data-stu-id="eccfc-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="eccfc-114">Далее приведена разметка необходимые для этого:</span><span class="sxs-lookup"><span data-stu-id="eccfc-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample1.aspx)]

<span data-ttu-id="eccfc-115">`SliderExtender` Элемента управления ASP.NET AJAX Control Toolkit назначает функциональные возможности "ползунок" для двух текстовых полях:</span><span class="sxs-lookup"><span data-stu-id="eccfc-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample2.aspx)]

<span data-ttu-id="eccfc-116">Дополнительные метки элемента будет использоваться позже для информирования пользователей обратной передачи:</span><span class="sxs-lookup"><span data-stu-id="eccfc-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample3.aspx)]

<span data-ttu-id="eccfc-117">Наконец `ScriptManager` элемент управления AJAX для ASP.NET загружает требуемого кода JavaScript для набора средств управления для работы:</span><span class="sxs-lookup"><span data-stu-id="eccfc-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample4.aspx)]

<span data-ttu-id="eccfc-118">Теперь ползунок обратной передаче; на стороне сервера это событие может быть перехвачено и обрабатываемое:</span><span class="sxs-lookup"><span data-stu-id="eccfc-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample5.aspx)]


<span data-ttu-id="eccfc-119">[![Перемещая ползунок инициирует обратную передачу](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="eccfc-119">[![Moving the slider triggers a postback](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)</span></span>

<span data-ttu-id="eccfc-120">Перемещая ползунок инициирует обратную передачу ([Просмотр полноразмерного изображения](using-the-slider-control-with-auto-postback-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="eccfc-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-vb/_static/image3.png))</span></span>


<span data-ttu-id="eccfc-121">[![После этого дата это изменение записывается в метке](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="eccfc-121">[![Afterwards, the date of this change is written in the label](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)</span></span>

<span data-ttu-id="eccfc-122">После этого дата это изменение записывается в метке ([Просмотр полноразмерного изображения](using-the-slider-control-with-auto-postback-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="eccfc-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="eccfc-123">[Назад](databinding-the-slider-control-cs.md)
> [Вперед](databinding-the-slider-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="eccfc-123">[Previous](databinding-the-slider-control-cs.md)
[Next](databinding-the-slider-control-vb.md)</span></span>
