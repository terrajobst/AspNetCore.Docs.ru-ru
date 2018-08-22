---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
title: Привязка данных элемента управления "ползунок" (Visual Basic) | Документация Майкрософт
author: wenz
description: Элемент управления "ползунок" в AJAX Control Toolkit предоставляет графическим ползунком, которые могут контролироваться с помощью мыши. Это позволяет привязать текущий иция...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4f3ba53f-d166-422d-b29c-403348057836
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 284a650d1b63ceed887deb3ddd639fe9fd27019f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835537"
---
<a name="databinding-the-slider-control-vb"></a><span data-ttu-id="a7a7c-104">Привязка данных элемента управления "ползунок" (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="a7a7c-104">Databinding the Slider Control (VB)</span></span>
====================
<span data-ttu-id="a7a7c-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a7a7c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a7a7c-106">[Скачать код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="a7a7c-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)</span></span>

> <span data-ttu-id="a7a7c-107">Элемент управления "ползунок" в AJAX Control Toolkit предоставляет графическим ползунком, которые могут контролироваться с помощью мыши.</span><span class="sxs-lookup"><span data-stu-id="a7a7c-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="a7a7c-108">Это позволяет привязать текущее положение ползунка на другой элемент управления ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a7a7c-108">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>


## <a name="overview"></a><span data-ttu-id="a7a7c-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="a7a7c-109">Overview</span></span>

<span data-ttu-id="a7a7c-110">Элемент управления "ползунок" в AJAX Control Toolkit предоставляет графическим ползунком, которые могут контролироваться с помощью мыши.</span><span class="sxs-lookup"><span data-stu-id="a7a7c-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="a7a7c-111">Это позволяет привязать текущее положение ползунка на другой элемент управления ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a7a7c-111">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="steps"></a><span data-ttu-id="a7a7c-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="a7a7c-112">Steps</span></span>

<span data-ttu-id="a7a7c-113">Для активации функции ASP.NET AJAX и Control Toolkit, `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемента):</span><span class="sxs-lookup"><span data-stu-id="a7a7c-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample1.aspx)]

<span data-ttu-id="a7a7c-114">Добавьте два `TextBox` элементов управления на страницу.</span><span class="sxs-lookup"><span data-stu-id="a7a7c-114">Next, add two `TextBox` controls to the page.</span></span> <span data-ttu-id="a7a7c-115">Один будет преобразован графическим ползунком, а другой будет содержать положение ползунка.</span><span class="sxs-lookup"><span data-stu-id="a7a7c-115">One will be transformed into a graphical slider, and the other one will hold the position of the slider.</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample2.aspx)]

<span data-ttu-id="a7a7c-116">Следующим шагом уже является последним шагом.</span><span class="sxs-lookup"><span data-stu-id="a7a7c-116">The next step is already the final step.</span></span> <span data-ttu-id="a7a7c-117">`SliderExtender` Элемента управления ASP.NET AJAX Control Toolkit делает ползунок из первое текстовое поле и автоматически обновляет второе текстовое поле, когда изменения позиции ползунка.</span><span class="sxs-lookup"><span data-stu-id="a7a7c-117">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit makes a slider out of the first text box and automatically updates the second text box when the slider position changes.</span></span> <span data-ttu-id="a7a7c-118">В порядке, чтобы это сработало `SliderExtender` `TargetControlID` атрибута необходимо задать идентификатор элемента в первом текстовом поле; `BoundControlID` атрибута необходимо задать идентификатор второго текстового поля.</span><span class="sxs-lookup"><span data-stu-id="a7a7c-118">In order for that to work, The `SliderExtender`'s `TargetControlID` attribute must be set to the ID of the first text box; the `BoundControlID` attribute must be set to the ID of the second text box.</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample3.aspx)]

<span data-ttu-id="a7a7c-119">Как вы видите в браузере, привязка данных работает в обоих направлениях: Введите новое значение в текстовом поле обновляет положение ползунка.</span><span class="sxs-lookup"><span data-stu-id="a7a7c-119">As you can see in the browser, the data binding works in both directions: entering a new value in the text box updates the slider's position.</span></span> <span data-ttu-id="a7a7c-120">Если второе текстовое поле только для чтения, может добавить слабую защиту в текстовое поле, так как это сложнее для пользователя, чтобы вручную обновить значение в ней.</span><span class="sxs-lookup"><span data-stu-id="a7a7c-120">If you make the second text box read only, you may add a weak protection to the text field so that it is harder for the user to manually update the value in there.</span></span>


<span data-ttu-id="a7a7c-121">[![Ползунок и текстовое поле синхронизированы](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a7a7c-121">[![Slider and text box are in sync](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="a7a7c-122">Ползунок и текстовое поле синхронизированы ([Просмотр полноразмерного изображения](databinding-the-slider-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a7a7c-122">Slider and text box are in sync ([Click to view full-size image](databinding-the-slider-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a7a7c-123">Назад</span><span class="sxs-lookup"><span data-stu-id="a7a7c-123">Previous</span></span>](using-the-slider-control-with-auto-postback-vb.md)
