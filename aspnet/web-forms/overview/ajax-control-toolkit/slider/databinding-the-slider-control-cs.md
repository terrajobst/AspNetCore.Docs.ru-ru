---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
title: Привязка данных элемента управления "ползунок" (C#) | Документация Майкрософт
author: wenz
description: Элемент управления "ползунок" в AJAX Control Toolkit предоставляет графическим ползунком, которые могут контролироваться с помощью мыши. Это позволяет привязать текущий иция...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: b7f77869-aa1d-4025-924f-622c57112db6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
msc.type: authoredcontent
ms.openlocfilehash: b7aebb8dd180113b011ac038e8da4a3baa701485
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818667"
---
<a name="databinding-the-slider-control-c"></a><span data-ttu-id="217c6-104">Привязка данных элемента управления "ползунок" (C#)</span><span class="sxs-lookup"><span data-stu-id="217c6-104">Databinding the Slider Control (C#)</span></span>
====================
<span data-ttu-id="217c6-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="217c6-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="217c6-106">[Скачать код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="217c6-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span></span>

> <span data-ttu-id="217c6-107">Элемент управления "ползунок" в AJAX Control Toolkit предоставляет графическим ползунком, которые могут контролироваться с помощью мыши.</span><span class="sxs-lookup"><span data-stu-id="217c6-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="217c6-108">Это позволяет привязать текущее положение ползунка на другой элемент управления ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="217c6-108">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>


## <a name="overview"></a><span data-ttu-id="217c6-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="217c6-109">Overview</span></span>

<span data-ttu-id="217c6-110">Элемент управления "ползунок" в AJAX Control Toolkit предоставляет графическим ползунком, которые могут контролироваться с помощью мыши.</span><span class="sxs-lookup"><span data-stu-id="217c6-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="217c6-111">Это позволяет привязать текущее положение ползунка на другой элемент управления ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="217c6-111">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="steps"></a><span data-ttu-id="217c6-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="217c6-112">Steps</span></span>

<span data-ttu-id="217c6-113">Для активации функции ASP.NET AJAX и Control Toolkit, `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемента):</span><span class="sxs-lookup"><span data-stu-id="217c6-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample1.aspx)]

<span data-ttu-id="217c6-114">Добавьте два `TextBox` элементов управления на страницу.</span><span class="sxs-lookup"><span data-stu-id="217c6-114">Next, add two `TextBox` controls to the page.</span></span> <span data-ttu-id="217c6-115">Один будет преобразован графическим ползунком, а другой будет содержать положение ползунка.</span><span class="sxs-lookup"><span data-stu-id="217c6-115">One will be transformed into a graphical slider, and the other one will hold the position of the slider.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample2.aspx)]

<span data-ttu-id="217c6-116">Следующим шагом уже является последним шагом.</span><span class="sxs-lookup"><span data-stu-id="217c6-116">The next step is already the final step.</span></span> <span data-ttu-id="217c6-117">`SliderExtender` Элемента управления ASP.NET AJAX Control Toolkit делает ползунок из первое текстовое поле и автоматически обновляет второе текстовое поле, когда изменения позиции ползунка.</span><span class="sxs-lookup"><span data-stu-id="217c6-117">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit makes a slider out of the first text box and automatically updates the second text box when the slider position changes.</span></span> <span data-ttu-id="217c6-118">В порядке, чтобы это сработало `SliderExtender` `TargetControlID` атрибута необходимо задать идентификатор элемента в первом текстовом поле; `BoundControlID` атрибута необходимо задать идентификатор второго текстового поля.</span><span class="sxs-lookup"><span data-stu-id="217c6-118">In order for that to work, The `SliderExtender`'s `TargetControlID` attribute must be set to the ID of the first text box; the `BoundControlID` attribute must be set to the ID of the second text box.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample3.aspx)]

<span data-ttu-id="217c6-119">Как вы видите в браузере, привязка данных работает в обоих направлениях: Введите новое значение в текстовом поле обновляет положение ползунка.</span><span class="sxs-lookup"><span data-stu-id="217c6-119">As you can see in the browser, the data binding works in both directions: entering a new value in the text box updates the slider's position.</span></span> <span data-ttu-id="217c6-120">Если второе текстовое поле только для чтения, может добавить слабую защиту в текстовое поле, так как это сложнее для пользователя, чтобы вручную обновить значение в ней.</span><span class="sxs-lookup"><span data-stu-id="217c6-120">If you make the second text box read only, you may add a weak protection to the text field so that it is harder for the user to manually update the value in there.</span></span>


<span data-ttu-id="217c6-121">[![Ползунок и текстовое поле синхронизированы](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="217c6-121">[![Slider and text box are in sync](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="217c6-122">Ползунок и текстовое поле синхронизированы ([Просмотр полноразмерного изображения](databinding-the-slider-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="217c6-122">Slider and text box are in sync ([Click to view full-size image](databinding-the-slider-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="217c6-123">[Назад](using-the-slider-control-with-auto-postback-cs.md)
> [Вперед](using-the-slider-control-with-auto-postback-vb.md)</span><span class="sxs-lookup"><span data-stu-id="217c6-123">[Previous](using-the-slider-control-with-auto-postback-cs.md)
[Next](using-the-slider-control-with-auto-postback-vb.md)</span></span>
