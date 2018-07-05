---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
title: С помощью автоматической обратной передачи с помощью с CascadingDropDown (C#) | Документация Майкрософт
author: wenz
description: Элемент управления CascadingDropDown в AJAX Control Toolkit расширяет элемент управления DropDownList, что изменения в одной загрузке DropDownList соответствующих значений в anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 6755d8d9-14be-4a1d-86e5-1a6110f3dea8
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: f1e0416a9c45cc0478a0179a4eef1788b7e2a8a5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384752"
---
<a name="using-auto-postback-with-cascadingdropdown-c"></a><span data-ttu-id="86d52-103">С помощью автоматической обратной передачи с помощью с CascadingDropDown (C#)</span><span class="sxs-lookup"><span data-stu-id="86d52-103">Using Auto-Postback with CascadingDropDown (C#)</span></span>
====================
<span data-ttu-id="86d52-104">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="86d52-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="86d52-105">[Скачать код](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="86d52-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)</span></span>

> <span data-ttu-id="86d52-106">Элемент управления CascadingDropDown в AJAX Control Toolkit расширяет элемент управления DropDownList, что изменения в одной загрузке DropDownList соответствующих значений в другой DropDownList.</span><span class="sxs-lookup"><span data-stu-id="86d52-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="86d52-107">Тем не менее при использовании элемента управления CascadingDropDown, ASP. Элемент управления DropDownList NET AutoPostBack функция работает, поскольку асинхронно загрузки данных в списке создает (ненужные) обратную передачу, сам.</span><span class="sxs-lookup"><span data-stu-id="86d52-107">However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="86d52-108">Этот эффект можно избежать с помощью кода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="86d52-108">With some JavaScript code, this effect can be avoided.</span></span>


## <a name="overview"></a><span data-ttu-id="86d52-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="86d52-109">Overview</span></span>

<span data-ttu-id="86d52-110">Элемент управления CascadingDropDown в AJAX Control Toolkit расширяет элемент управления DropDownList, что изменения в одной загрузке DropDownList соответствующих значений в другой DropDownList.</span><span class="sxs-lookup"><span data-stu-id="86d52-110">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="86d52-111">(К примеру, один список содержит список штатов США, и следующий список заполняется крупнейших городов в этом состоянии.) Тем не менее при использовании элемента управления CascadingDropDown, ASP. Элемент управления DropDownList NET AutoPostBack функция работает, поскольку асинхронно загрузки данных в списке создает (ненужные) обратную передачу, сам.</span><span class="sxs-lookup"><span data-stu-id="86d52-111">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="86d52-112">Этот эффект можно избежать с помощью кода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="86d52-112">With some JavaScript code, this effect can be avoided.</span></span>

## <a name="steps"></a><span data-ttu-id="86d52-113">Шаги</span><span class="sxs-lookup"><span data-stu-id="86d52-113">Steps</span></span>

<span data-ttu-id="86d52-114">Для активации функции ASP.NET AJAX и Control Toolkit, `ScriptManager` управления необходимо поместить в любом месте на странице (но в &lt; `form` &gt; элемента):</span><span class="sxs-lookup"><span data-stu-id="86d52-114">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="86d52-115">Затем необходим элемент управления DropDownList:</span><span class="sxs-lookup"><span data-stu-id="86d52-115">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="86d52-116">Для этого списка добавляется расширитель CascadingDropDown, предоставляя данные веб-службы URL-адрес и метод:</span><span class="sxs-lookup"><span data-stu-id="86d52-116">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="86d52-117">Расширитель CascadingDropDown затем производит асинхронный вызов веб-службы со следующей сигнатурой метода:</span><span class="sxs-lookup"><span data-stu-id="86d52-117">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-csharp[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="86d52-118">Метод возвращает массив объектов типа CascadingDropDown значение.</span><span class="sxs-lookup"><span data-stu-id="86d52-118">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="86d52-119">Конструктор типа сначала ожидает заголовок элемента списка, а затем значение (HTML `value` атрибут).</span><span class="sxs-lookup"><span data-stu-id="86d52-119">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="86d52-120">Загрузку страницы в браузере будет заполнить раскрывающийся список с тремя поставщиками второй выбраны заранее.</span><span class="sxs-lookup"><span data-stu-id="86d52-120">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span> <span data-ttu-id="86d52-121">Кроме того, ASP.NET определяет `__doPostBack()` метод JavaScript.</span><span class="sxs-lookup"><span data-stu-id="86d52-121">Also, ASP.NET defines the `__doPostBack()` JavaScript method.</span></span> <span data-ttu-id="86d52-122">После загрузки страницы, этот вызов JavaScript добавляется к раскрывающемуся списку, но только при наличии элементов в нем.</span><span class="sxs-lookup"><span data-stu-id="86d52-122">Once the page has been loaded, this JavaScript call is added to the dropdown list, but only if there are elements in it.</span></span> <span data-ttu-id="86d52-123">Если в списке нет элементов, набор элементов управления в процессе загрузки, поэтому в коде JavaScript используется время ожидания и предпринимает попытку через половины секунды.</span><span class="sxs-lookup"><span data-stu-id="86d52-123">If there are no elements in the list, the Control Toolkit is currently loading them, so the JavaScript code uses a timeout and tries again in a half second.</span></span>

[!code-html[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample6.html)]

<span data-ttu-id="86d52-124">Таким образом, обратная передача выполняется только при наличии фактически элементы в списке, и пользователь выбирает запись.</span><span class="sxs-lookup"><span data-stu-id="86d52-124">This way, a postback is only executed when there are actually elements in the list and the user selects an entry.</span></span>


<span data-ttu-id="86d52-125">[![Выбор элемента списка вызывает обратную передачу](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="86d52-125">[![Selecting a list element causes a postback](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="86d52-126">Выбор элемента списка вызывает обратную передачу ([Просмотр полноразмерного изображения](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="86d52-126">Selecting a list element causes a postback ([Click to view full-size image](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="86d52-127">[Назад](presetting-list-entries-with-cascadingdropdown-cs.md)
> [Вперед](filling-a-list-using-cascadingdropdown-vb.md)</span><span class="sxs-lookup"><span data-stu-id="86d52-127">[Previous](presetting-list-entries-with-cascadingdropdown-cs.md)
[Next](filling-a-list-using-cascadingdropdown-vb.md)</span></span>
