---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: Заполнение списка, используя CascadingDropDown (C#) | Документы Microsoft
author: wenz
description: CascadingDropDown управления в наборе элементов управления AJAX расширяет элемент управления DropDownList, чтобы изменения в один загружает DropDownList соответствующих значений в anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: c9e47f6484e49013004bf15084f98440ee67558e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870977"
---
<a name="filling-a-list-using-cascadingdropdown-c"></a><span data-ttu-id="6f888-103">Заполнение списка, используя CascadingDropDown (C#)</span><span class="sxs-lookup"><span data-stu-id="6f888-103">Filling a List Using CascadingDropDown (C#)</span></span>
====================
<span data-ttu-id="6f888-104">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="6f888-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="6f888-105">[Загрузить код](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="6f888-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span></span>

> <span data-ttu-id="6f888-106">CascadingDropDown управления в наборе элементов управления AJAX расширяет элемент управления DropDownList, чтобы изменения в один загружает DropDownList соответствующих значений в другой DropDownList.</span><span class="sxs-lookup"><span data-stu-id="6f888-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="6f888-107">(Для экземпляра один список содержит список штатов США, и далее список заполняется основных городов, в этом состоянии.) Для решения первой проверки — фактически заполнить раскрывающийся список с помощью этого элемента управления.</span><span class="sxs-lookup"><span data-stu-id="6f888-107">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>


## <a name="overview"></a><span data-ttu-id="6f888-108">Обзор</span><span class="sxs-lookup"><span data-stu-id="6f888-108">Overview</span></span>

<span data-ttu-id="6f888-109">CascadingDropDown управления в наборе элементов управления AJAX расширяет элемент управления DropDownList, чтобы изменения в один загружает DropDownList соответствующих значений в другой DropDownList.</span><span class="sxs-lookup"><span data-stu-id="6f888-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="6f888-110">(Для экземпляра один список содержит список штатов США, и далее список заполняется основных городов, в этом состоянии.) Для решения первой проверки — фактически заполнить раскрывающийся список с помощью этого элемента управления.</span><span class="sxs-lookup"><span data-stu-id="6f888-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="steps"></a><span data-ttu-id="6f888-111">Шаги</span><span class="sxs-lookup"><span data-stu-id="6f888-111">Steps</span></span>

<span data-ttu-id="6f888-112">Чтобы активировать функциональные возможности ASP.NET AJAX и набора средств управления `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемент):</span><span class="sxs-lookup"><span data-stu-id="6f888-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="6f888-113">Затем элемент управления DropDownList требуется:</span><span class="sxs-lookup"><span data-stu-id="6f888-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="6f888-114">Расширитель CascadingDropDown добавляется для этого списка.</span><span class="sxs-lookup"><span data-stu-id="6f888-114">For this list, a CascadingDropDown extender is added.</span></span> <span data-ttu-id="6f888-115">Он передает выполнение асинхронного запроса для веб-службы, которая будет возвращать список записей, отображаемых в списке.</span><span class="sxs-lookup"><span data-stu-id="6f888-115">It will send an asynchronous request to a web service which will then return a list of entries to be displayed in the list.</span></span> <span data-ttu-id="6f888-116">Чтобы это работало необходимо задать следующие атрибуты CascadingDropDown:</span><span class="sxs-lookup"><span data-stu-id="6f888-116">For this to work, the following CascadingDropDown attributes need to be set:</span></span>

- <span data-ttu-id="6f888-117">`ServicePath`: URL-адрес веб-службы доставки элементов списка</span><span class="sxs-lookup"><span data-stu-id="6f888-117">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="6f888-118">`ServiceMethod`: Веб-метода доставки элементов списка</span><span class="sxs-lookup"><span data-stu-id="6f888-118">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="6f888-119">`TargetControlID`: Идентификатор из раскрывающегося списка</span><span class="sxs-lookup"><span data-stu-id="6f888-119">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="6f888-120">`Category`: Категории сведений, переданных при вызове веб-метода</span><span class="sxs-lookup"><span data-stu-id="6f888-120">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="6f888-121">`PromptText`: Текст, отображаемый при асинхронной загрузке данных списка с сервера</span><span class="sxs-lookup"><span data-stu-id="6f888-121">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>

<span data-ttu-id="6f888-122">Разметка для `CascadingDropDown` элемента.</span><span class="sxs-lookup"><span data-stu-id="6f888-122">Here is the markup for the `CascadingDropDown` element.</span></span> <span data-ttu-id="6f888-123">Единственное различие между C# и VB — имя связанного веб-службы:</span><span class="sxs-lookup"><span data-stu-id="6f888-123">The only difference between C# and VB is the name of the associated web service:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="6f888-124">Код JavaScript, поступающих от `CascadingDropDown` расширителя вызывает метод веб-службы со следующей сигнатурой:</span><span class="sxs-lookup"><span data-stu-id="6f888-124">The JavaScript code coming from the `CascadingDropDown` extender calls a web service method with the following signature:</span></span>

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="6f888-125">Поэтому является важным аспектом методу требуется вернуть массив объектов типа `CascadingDropDownNameValue` (определяемые элементов управления ASP.NET AJAX).</span><span class="sxs-lookup"><span data-stu-id="6f888-125">So the important aspect is that the method needs to return an array of type `CascadingDropDownNameValue` (defined by the ASP.NET AJAX Control Toolkit).</span></span> <span data-ttu-id="6f888-126">В `CascadingDropDownNameValue` конструктор первый текстовый элемент списка, а затем его значение необходимо указать, как `<option value="VALUE">NAME</option>` в HTML.</span><span class="sxs-lookup"><span data-stu-id="6f888-126">In the `CascadingDropDownNameValue` contructor, first the list entry's text and then its value must be provided, just as `<option value="VALUE">NAME</option>` would do in HTML.</span></span> <span data-ttu-id="6f888-127">Вот некоторые образцы данных:</span><span class="sxs-lookup"><span data-stu-id="6f888-127">Here is some sample data:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="6f888-128">Загрузка страницы в браузере, запускающих списка должен быть заполнен трех поставщиков.</span><span class="sxs-lookup"><span data-stu-id="6f888-128">Loading the page in the browser will trigger the list to be filled with three vendors.</span></span>


<span data-ttu-id="6f888-129">[![Список заполняется автоматически](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6f888-129">[![The list is filled automatically](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="6f888-130">Список заполняется автоматически ([Просмотр полноразмерное изображение](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6f888-130">The list is filled automatically ([Click to view full-size image](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="6f888-131">Вперед</span><span class="sxs-lookup"><span data-stu-id="6f888-131">Next</span></span>](using-cascadingdropdown-with-a-database-cs.md)
