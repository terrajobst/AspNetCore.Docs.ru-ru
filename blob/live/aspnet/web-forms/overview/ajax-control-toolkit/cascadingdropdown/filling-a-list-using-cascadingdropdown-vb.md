---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
title: "Заполнение списка, используя CascadingDropDown (VB) | Документы Microsoft"
author: wenz
description: "CascadingDropDown управления в наборе элементов управления AJAX расширяет элемент управления DropDownList, чтобы изменения в один загружает DropDownList соответствующих значений в anoth..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 5236695e-5c70-4887-baee-0bfb0afb3448
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 1c5cb23be4366365c73ce4774e6a53e452e2a6c0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="filling-a-list-using-cascadingdropdown-vb"></a><span data-ttu-id="41a10-103">Заполнение списка, используя CascadingDropDown (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="41a10-103">Filling a List Using CascadingDropDown (VB)</span></span>
====================
<span data-ttu-id="41a10-104">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="41a10-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="41a10-105">[Загрузить код](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="41a10-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)</span></span>

> <span data-ttu-id="41a10-106">CascadingDropDown управления в наборе элементов управления AJAX расширяет элемент управления DropDownList, чтобы изменения в один загружает DropDownList соответствующих значений в другой DropDownList.</span><span class="sxs-lookup"><span data-stu-id="41a10-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="41a10-107">(Для экземпляра один список содержит список штатов США, и далее список заполняется основных городов, в этом состоянии.) Для решения первой проверки — фактически заполнить раскрывающийся список с помощью этого элемента управления.</span><span class="sxs-lookup"><span data-stu-id="41a10-107">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>


## <a name="overview"></a><span data-ttu-id="41a10-108">Обзор</span><span class="sxs-lookup"><span data-stu-id="41a10-108">Overview</span></span>

<span data-ttu-id="41a10-109">CascadingDropDown управления в наборе элементов управления AJAX расширяет элемент управления DropDownList, чтобы изменения в один загружает DropDownList соответствующих значений в другой DropDownList.</span><span class="sxs-lookup"><span data-stu-id="41a10-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="41a10-110">(Для экземпляра один список содержит список штатов США, и далее список заполняется основных городов, в этом состоянии.) Для решения первой проверки — фактически заполнить раскрывающийся список с помощью этого элемента управления.</span><span class="sxs-lookup"><span data-stu-id="41a10-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="steps"></a><span data-ttu-id="41a10-111">Шаги</span><span class="sxs-lookup"><span data-stu-id="41a10-111">Steps</span></span>

<span data-ttu-id="41a10-112">Чтобы активировать функциональные возможности ASP.NET AJAX и набора средств управления `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемент):</span><span class="sxs-lookup"><span data-stu-id="41a10-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="41a10-113">Затем элемент управления DropDownList требуется:</span><span class="sxs-lookup"><span data-stu-id="41a10-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="41a10-114">Расширитель CascadingDropDown добавляется для этого списка.</span><span class="sxs-lookup"><span data-stu-id="41a10-114">For this list, a CascadingDropDown extender is added.</span></span> <span data-ttu-id="41a10-115">Он передает выполнение асинхронного запроса для веб-службы, которая будет возвращать список записей, отображаемых в списке.</span><span class="sxs-lookup"><span data-stu-id="41a10-115">It will send an asynchronous request to a web service which will then return a list of entries to be displayed in the list.</span></span> <span data-ttu-id="41a10-116">Чтобы это работало необходимо задать следующие атрибуты CascadingDropDown:</span><span class="sxs-lookup"><span data-stu-id="41a10-116">For this to work, the following CascadingDropDown attributes need to be set:</span></span>

- <span data-ttu-id="41a10-117">`ServicePath`: URL-адрес веб-службы доставки элементов списка</span><span class="sxs-lookup"><span data-stu-id="41a10-117">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="41a10-118">`ServiceMethod`: Веб-метода доставки элементов списка</span><span class="sxs-lookup"><span data-stu-id="41a10-118">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="41a10-119">`TargetControlID`: Идентификатор из раскрывающегося списка</span><span class="sxs-lookup"><span data-stu-id="41a10-119">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="41a10-120">`Category`: Категории сведений, переданных при вызове веб-метода</span><span class="sxs-lookup"><span data-stu-id="41a10-120">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="41a10-121">`PromptText`: Текст, отображаемый при асинхронной загрузке данных списка с сервера</span><span class="sxs-lookup"><span data-stu-id="41a10-121">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>

<span data-ttu-id="41a10-122">Разметка для `CascadingDropDown` элемента.</span><span class="sxs-lookup"><span data-stu-id="41a10-122">Here is the markup for the `CascadingDropDown` element.</span></span> <span data-ttu-id="41a10-123">Единственное различие между C# и VB — имя связанного веб-службы:</span><span class="sxs-lookup"><span data-stu-id="41a10-123">The only difference between C# and VB is the name of the associated web service:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="41a10-124">Код JavaScript, поступающих от `CascadingDropDown` расширителя вызывает метод веб-службы со следующей сигнатурой:</span><span class="sxs-lookup"><span data-stu-id="41a10-124">The JavaScript code coming from the `CascadingDropDown` extender calls a web service method with the following signature:</span></span>

[!code-vb[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="41a10-125">Поэтому является важным аспектом методу требуется вернуть массив объектов типа `CascadingDropDownNameValue` (определяемые элементов управления ASP.NET AJAX).</span><span class="sxs-lookup"><span data-stu-id="41a10-125">So the important aspect is that the method needs to return an array of type `CascadingDropDownNameValue` (defined by the ASP.NET AJAX Control Toolkit).</span></span> <span data-ttu-id="41a10-126">В `CascadingDropDownNameValue` конструктор первый текстовый элемент списка, а затем его значение необходимо указать, как `<option value="VALUE">NAME</option>` в HTML.</span><span class="sxs-lookup"><span data-stu-id="41a10-126">In the `CascadingDropDownNameValue` contructor, first the list entry's text and then its value must be provided, just as `<option value="VALUE">NAME</option>` would do in HTML.</span></span> <span data-ttu-id="41a10-127">Вот некоторые образцы данных:</span><span class="sxs-lookup"><span data-stu-id="41a10-127">Here is some sample data:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="41a10-128">Загрузка страницы в браузере, запускающих списка должен быть заполнен трех поставщиков.</span><span class="sxs-lookup"><span data-stu-id="41a10-128">Loading the page in the browser will trigger the list to be filled with three vendors.</span></span>


<span data-ttu-id="41a10-129">[![Список заполняется автоматически](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="41a10-129">[![The list is filled automatically](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="41a10-130">Список заполняется автоматически ([Просмотр полноразмерное изображение](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="41a10-130">The list is filled automatically ([Click to view full-size image](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="41a10-131">[Назад](using-auto-postback-with-cascadingdropdown-cs.md)
[Вперед](using-cascadingdropdown-with-a-database-vb.md)</span><span class="sxs-lookup"><span data-stu-id="41a10-131">[Previous](using-auto-postback-with-cascadingdropdown-cs.md)
[Next](using-cascadingdropdown-with-a-database-vb.md)</span></span>
