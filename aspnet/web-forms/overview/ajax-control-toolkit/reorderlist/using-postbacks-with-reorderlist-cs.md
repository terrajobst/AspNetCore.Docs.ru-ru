---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
title: Использование обратных передач с ReorderList (C#) | Документы Microsoft
author: wenz
description: Элемент управления ReorderList в наборе элементов управления AJAX предоставляет список, который можно переупорядочить пользователем с помощью операции перетаскивания. Каждый раз, когда список переупорядочивается, po...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 70d5d106-b547-442c-a7fd-3492b3e3d646
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: ed01c30c0721c8f1cd8ccb3fea0735ea8fa4f0a1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="using-postbacks-with-reorderlist-c"></a><span data-ttu-id="0f123-104">Использование обратных передач с ReorderList (C#)</span><span class="sxs-lookup"><span data-stu-id="0f123-104">Using Postbacks with ReorderList (C#)</span></span>
====================
<span data-ttu-id="0f123-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0f123-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0f123-106">[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="0f123-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)</span></span>

> <span data-ttu-id="0f123-107">Элемент управления ReorderList в наборе элементов управления AJAX предоставляет список, который можно переупорядочить пользователем с помощью операции перетаскивания.</span><span class="sxs-lookup"><span data-stu-id="0f123-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="0f123-108">Всякий раз, когда список переупорядочивается, обратную передачу должны сообщить серверу изменения.</span><span class="sxs-lookup"><span data-stu-id="0f123-108">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>


## <a name="overview"></a><span data-ttu-id="0f123-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="0f123-109">Overview</span></span>

<span data-ttu-id="0f123-110">`ReorderList` Список элемента управления в наборе элементов управления AJAX можно переупорядочить пользователем с помощью операции перетаскивания.</span><span class="sxs-lookup"><span data-stu-id="0f123-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="0f123-111">Всякий раз, когда список переупорядочивается, обратную передачу должны сообщить серверу изменения.</span><span class="sxs-lookup"><span data-stu-id="0f123-111">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="steps"></a><span data-ttu-id="0f123-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="0f123-112">Steps</span></span>

<span data-ttu-id="0f123-113">Существует несколько источников данных для `ReorderList` элемента управления.</span><span class="sxs-lookup"><span data-stu-id="0f123-113">There are several possible data sources for the `ReorderList` control.</span></span> <span data-ttu-id="0f123-114">Один является применение `XmlDataSource` управления:</span><span class="sxs-lookup"><span data-stu-id="0f123-114">One is to use an `XmlDataSource` control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample1.aspx)]

<span data-ttu-id="0f123-115">Чтобы привязать этот XML-код для `ReorderList` необходимо задать элемент управления и Включение обратной передачи, следующие атрибуты:</span><span class="sxs-lookup"><span data-stu-id="0f123-115">In order to bind this XML to a `ReorderList` control and enable postbacks, the following attributes must be set:</span></span>

- <span data-ttu-id="0f123-116">`DataSourceID`: Идентификатор источника данных</span><span class="sxs-lookup"><span data-stu-id="0f123-116">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="0f123-117">`SortOrderField`: Свойство, по которому выполняется сортировка</span><span class="sxs-lookup"><span data-stu-id="0f123-117">`SortOrderField`: The property to sort by</span></span>
- <span data-ttu-id="0f123-118">`AllowReorder`: Следует ли разрешить пользователю изменить порядок элементов списка</span><span class="sxs-lookup"><span data-stu-id="0f123-118">`AllowReorder`: Whether to allow the user to reorder the list elements</span></span>
- <span data-ttu-id="0f123-119">`PostBackOnReorder`: Следует ли создавать обратной передачи, когда переупорядочить список</span><span class="sxs-lookup"><span data-stu-id="0f123-119">`PostBackOnReorder`: Whether to create a postback whenever the list is rearranged</span></span>

<span data-ttu-id="0f123-120">Ниже приведен соответствующий разметки для элемента управления.</span><span class="sxs-lookup"><span data-stu-id="0f123-120">Here is the appropriate markup for the control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample2.aspx)]

<span data-ttu-id="0f123-121">В пределах `ReorderList` управления определенных данных из источника данных может быть связана с помощью `Eval()` метод:</span><span class="sxs-lookup"><span data-stu-id="0f123-121">Within the `ReorderList` control, specific data from the data source may be bound using the `Eval()` method:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample3.aspx)]

<span data-ttu-id="0f123-122">В случайном положении на странице метку хранения информации, когда произошло последнее изменение порядка:</span><span class="sxs-lookup"><span data-stu-id="0f123-122">At an arbitrary position on the page, a label will hold the information when the last reordering occurred:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample4.aspx)]

<span data-ttu-id="0f123-123">Эта метка заполняется текстом в серверный код, обработка обратной передачи:</span><span class="sxs-lookup"><span data-stu-id="0f123-123">This label is filled with text in the server-side code, handling the postback:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample5.aspx)]

<span data-ttu-id="0f123-124">Наконец, чтобы активировать функциональные возможности ASP.NET AJAX и набора средств управления `ScriptManager` управления могут быть помещены на странице:</span><span class="sxs-lookup"><span data-stu-id="0f123-124">Finally, in order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put on the page:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample6.aspx)]


<span data-ttu-id="0f123-125">[![Каждый переупорядочение вызывает обратную передачу](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0f123-125">[![Each reordering triggers a postback](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)</span></span>

<span data-ttu-id="0f123-126">Каждый переупорядочение вызывает обратную передачу ([Просмотр полноразмерное изображение](using-postbacks-with-reorderlist-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0f123-126">Each reordering triggers a postback ([Click to view full-size image](using-postbacks-with-reorderlist-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0f123-127">Вперед</span><span class="sxs-lookup"><span data-stu-id="0f123-127">Next</span></span>](drag-and-drop-via-reorderlist-cs.md)
