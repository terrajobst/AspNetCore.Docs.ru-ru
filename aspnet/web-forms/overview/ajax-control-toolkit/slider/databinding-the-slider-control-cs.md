---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
title: Привязка данных управления "ползунок" (C#) | Документы Microsoft
author: wenz
description: Ползунок в наборе элементов управления AJAX имеет графический ползунок, можно управлять с помощью мыши. Это позволяет привязать текущий иция...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b7f77869-aa1d-4025-924f-622c57112db6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 7644c991cd88868235511ba372be1f5b47c68fea
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870587"
---
<a name="databinding-the-slider-control-c"></a><span data-ttu-id="2faf7-104">Привязка данных управления "ползунок" (C#)</span><span class="sxs-lookup"><span data-stu-id="2faf7-104">Databinding the Slider Control (C#)</span></span>
====================
<span data-ttu-id="2faf7-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2faf7-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2faf7-106">[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="2faf7-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span></span>

> <span data-ttu-id="2faf7-107">Ползунок в наборе элементов управления AJAX имеет графический ползунок, можно управлять с помощью мыши.</span><span class="sxs-lookup"><span data-stu-id="2faf7-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="2faf7-108">Это позволяет привязать текущее положение ползунка на другой элемент управления ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2faf7-108">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>


## <a name="overview"></a><span data-ttu-id="2faf7-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="2faf7-109">Overview</span></span>

<span data-ttu-id="2faf7-110">Ползунок в наборе элементов управления AJAX имеет графический ползунок, можно управлять с помощью мыши.</span><span class="sxs-lookup"><span data-stu-id="2faf7-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="2faf7-111">Это позволяет привязать текущее положение ползунка на другой элемент управления ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2faf7-111">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="steps"></a><span data-ttu-id="2faf7-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="2faf7-112">Steps</span></span>

<span data-ttu-id="2faf7-113">Чтобы активировать функциональные возможности ASP.NET AJAX и набора средств управления `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемент):</span><span class="sxs-lookup"><span data-stu-id="2faf7-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample1.aspx)]

<span data-ttu-id="2faf7-114">Добавьте два `TextBox` элементов управления на страницу.</span><span class="sxs-lookup"><span data-stu-id="2faf7-114">Next, add two `TextBox` controls to the page.</span></span> <span data-ttu-id="2faf7-115">Один будет преобразован в графический ползунок, а другой будет содержать положение движка.</span><span class="sxs-lookup"><span data-stu-id="2faf7-115">One will be transformed into a graphical slider, and the other one will hold the position of the slider.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample2.aspx)]

<span data-ttu-id="2faf7-116">Следующий шаг уже является последним шагом.</span><span class="sxs-lookup"><span data-stu-id="2faf7-116">The next step is already the final step.</span></span> <span data-ttu-id="2faf7-117">`SliderExtender` Управления из набора элементов управления ASP.NET AJAX делает ползунок из первого текстового поля и автоматически обновляет второе текстовое поле, когда изменения положение ползунка.</span><span class="sxs-lookup"><span data-stu-id="2faf7-117">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit makes a slider out of the first text box and automatically updates the second text box when the slider position changes.</span></span> <span data-ttu-id="2faf7-118">Чтобы это работало `SliderExtender` `TargetControlID` атрибут необходимо задать идентификатор элемента в первом текстовом поле; `BoundControlID` атрибуту необходимо присвоить идентификатор второе текстовое поле.</span><span class="sxs-lookup"><span data-stu-id="2faf7-118">In order for that to work, The `SliderExtender`'s `TargetControlID` attribute must be set to the ID of the first text box; the `BoundControlID` attribute must be set to the ID of the second text box.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample3.aspx)]

<span data-ttu-id="2faf7-119">Как видно в браузере, привязка данных работает в обоих направлениях: Введите новое значение в текстовом поле обновляет положение ползунка.</span><span class="sxs-lookup"><span data-stu-id="2faf7-119">As you can see in the browser, the data binding works in both directions: entering a new value in the text box updates the slider's position.</span></span> <span data-ttu-id="2faf7-120">Если второе текстовое поле только для чтения, могут добавлять слабую защиту с текстовым полем, чтобы оно было сложнее для пользователя, чтобы вручную обновить значение в нем.</span><span class="sxs-lookup"><span data-stu-id="2faf7-120">If you make the second text box read only, you may add a weak protection to the text field so that it is harder for the user to manually update the value in there.</span></span>


<span data-ttu-id="2faf7-121">[![Ползунок и текстового поля находятся в синхронизированном состоянии](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2faf7-121">[![Slider and text box are in sync](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="2faf7-122">Ползунок и текстового поля находятся в синхронизированном состоянии ([Просмотр полноразмерное изображение](databinding-the-slider-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2faf7-122">Slider and text box are in sync ([Click to view full-size image](databinding-the-slider-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2faf7-123">[Назад](using-the-slider-control-with-auto-postback-cs.md)
> [Вперед](using-the-slider-control-with-auto-postback-vb.md)</span><span class="sxs-lookup"><span data-stu-id="2faf7-123">[Previous](using-the-slider-control-with-auto-postback-cs.md)
[Next](using-the-slider-control-with-auto-postback-vb.md)</span></span>
