---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
title: Использование автоматического выполнения обратной с CascadingDropDown (VB) | Документы Microsoft
author: wenz
description: CascadingDropDown управления в наборе элементов управления AJAX расширяет элемент управления DropDownList, чтобы изменения в один загружает DropDownList соответствующих значений в anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0b34f7f6-a0cc-4b9f-9761-643fb0bb3ece
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: e8a48692dd96ee6a647bfb57ce2915b4e85544fe
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871380"
---
<a name="using-auto-postback-with-cascadingdropdown-vb"></a><span data-ttu-id="06c55-103">Использование автоматического выполнения обратной с CascadingDropDown (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="06c55-103">Using Auto-Postback with CascadingDropDown (VB)</span></span>
====================
<span data-ttu-id="06c55-104">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="06c55-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="06c55-105">[Загрузить код](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="06c55-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)</span></span>

> <span data-ttu-id="06c55-106">CascadingDropDown управления в наборе элементов управления AJAX расширяет элемент управления DropDownList, чтобы изменения в один загружает DropDownList соответствующих значений в другой DropDownList.</span><span class="sxs-lookup"><span data-stu-id="06c55-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="06c55-107">Однако при использовании элемента управления CascadingDropDown, ASP. Компонент управления DropDownList в NET AutoPostBack не работает, поскольку асинхронно загрузку данных в списке создает (неиспользуемые) обратной передачи, сам.</span><span class="sxs-lookup"><span data-stu-id="06c55-107">However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="06c55-108">Этот эффект можно избежать с помощью кода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="06c55-108">With some JavaScript code, this effect can be avoided.</span></span>


## <a name="overview"></a><span data-ttu-id="06c55-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="06c55-109">Overview</span></span>

<span data-ttu-id="06c55-110">CascadingDropDown управления в наборе элементов управления AJAX расширяет элемент управления DropDownList, чтобы изменения в один загружает DropDownList соответствующих значений в другой DropDownList.</span><span class="sxs-lookup"><span data-stu-id="06c55-110">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="06c55-111">(Для экземпляра один список содержит список штатов США, и далее список заполняется основных городов, в этом состоянии.) Однако при использовании элемента управления CascadingDropDown, ASP. Компонент управления DropDownList в NET AutoPostBack не работает, поскольку асинхронно загрузку данных в списке создает (неиспользуемые) обратной передачи, сам.</span><span class="sxs-lookup"><span data-stu-id="06c55-111">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="06c55-112">Этот эффект можно избежать с помощью кода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="06c55-112">With some JavaScript code, this effect can be avoided.</span></span>

## <a name="steps"></a><span data-ttu-id="06c55-113">Шаги</span><span class="sxs-lookup"><span data-stu-id="06c55-113">Steps</span></span>

<span data-ttu-id="06c55-114">Чтобы активировать функциональные возможности ASP.NET AJAX и набора средств управления `ScriptManager` управления необходимо поместить в любом месте на странице (но в &lt; `form` &gt; элемент):</span><span class="sxs-lookup"><span data-stu-id="06c55-114">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="06c55-115">Затем элемент управления DropDownList требуется:</span><span class="sxs-lookup"><span data-stu-id="06c55-115">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="06c55-116">Для этого списка добавляется расширителя CascadingDropDown, предоставляя URL-адрес и метод информации веб-службы:</span><span class="sxs-lookup"><span data-stu-id="06c55-116">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="06c55-117">Расширитель CascadingDropDown асинхронно вызывает веб-службы со следующей сигнатурой метода:</span><span class="sxs-lookup"><span data-stu-id="06c55-117">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-vb[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="06c55-118">Метод возвращает массив объектов типа CascadingDropDown значение.</span><span class="sxs-lookup"><span data-stu-id="06c55-118">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="06c55-119">Тип конструктора сначала требуется заголовок элемента списка, а затем значение (HTML `value` атрибут).</span><span class="sxs-lookup"><span data-stu-id="06c55-119">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="06c55-120">Загрузка страницы в браузере будут заполнены раскрывающийся список с тремя поставщиков предварительно выбран второй.</span><span class="sxs-lookup"><span data-stu-id="06c55-120">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span> <span data-ttu-id="06c55-121">Кроме того, определяет ASP.NET `__doPostBack()` метода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="06c55-121">Also, ASP.NET defines the `__doPostBack()` JavaScript method.</span></span> <span data-ttu-id="06c55-122">После загрузки страницы, этот вызов JavaScript добавляется к раскрывающемуся списку, но только при наличии элементов в нем.</span><span class="sxs-lookup"><span data-stu-id="06c55-122">Once the page has been loaded, this JavaScript call is added to the dropdown list, but only if there are elements in it.</span></span> <span data-ttu-id="06c55-123">Если в списке нет элементов, набор элементов управления загружается, поэтому код JavaScript использует время ожидания и предпринимает попытку через полсекунды.</span><span class="sxs-lookup"><span data-stu-id="06c55-123">If there are no elements in the list, the Control Toolkit is currently loading them, so the JavaScript code uses a timeout and tries again in a half second.</span></span>

[!code-html[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample6.html)]

<span data-ttu-id="06c55-124">Таким образом, обратная передача выполняется только при наличии фактически элементов в списке, а пользователь выбирает запись.</span><span class="sxs-lookup"><span data-stu-id="06c55-124">This way, a postback is only executed when there are actually elements in the list and the user selects an entry.</span></span>


<span data-ttu-id="06c55-125">[![При выборе элемента списка вызывает обратную передачу](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="06c55-125">[![Selecting a list element causes a postback](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="06c55-126">При выборе элемента списка вызывает обратную передачу ([Просмотр полноразмерное изображение](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="06c55-126">Selecting a list element causes a postback ([Click to view full-size image](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="06c55-127">Назад</span><span class="sxs-lookup"><span data-stu-id="06c55-127">Previous</span></span>](presetting-list-entries-with-cascadingdropdown-vb.md)
