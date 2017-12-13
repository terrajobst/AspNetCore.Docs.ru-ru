---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
title: "Динамическое добавление Гармошка области (C#) | Документы Microsoft"
author: wenz
description: "Гармошка управления в наборе элементов управления AJAX предоставляет несколько областей и пользователь может содержать один из них одновременно. Панелей обычно объявляются w..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 66d88cfa-f26f-46b1-ad52-1c9e03c04a48
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
msc.type: authoredcontent
ms.openlocfilehash: 1c6af79186ca21082647beec500c47974a47c35a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-adding-an-accordion-pane-c"></a><span data-ttu-id="9763f-104">Динамическое добавление Гармошка области (C#)</span><span class="sxs-lookup"><span data-stu-id="9763f-104">Dynamically Adding An Accordion Pane (C#)</span></span>
====================
<span data-ttu-id="9763f-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="9763f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="9763f-106">[Загрузить код](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) или [скачать PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="9763f-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)</span></span>

> <span data-ttu-id="9763f-107">Гармошка управления в наборе элементов управления AJAX предоставляет несколько областей и пользователь может содержать один из них одновременно.</span><span class="sxs-lookup"><span data-stu-id="9763f-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="9763f-108">Обычно панелей объявлены внутри самой страницы, но код на стороне сервера может использоваться для достижения такого же результата.</span><span class="sxs-lookup"><span data-stu-id="9763f-108">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>


## <a name="overview"></a><span data-ttu-id="9763f-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="9763f-109">Overview</span></span>

<span data-ttu-id="9763f-110">Гармошка управления в наборе элементов управления AJAX предоставляет несколько областей и пользователь может содержать один из них одновременно.</span><span class="sxs-lookup"><span data-stu-id="9763f-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="9763f-111">Обычно панелей объявлены внутри самой страницы, но код на стороне сервера может использоваться для достижения такого же результата.</span><span class="sxs-lookup"><span data-stu-id="9763f-111">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="steps"></a><span data-ttu-id="9763f-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="9763f-112">Steps</span></span>

<span data-ttu-id="9763f-113">Элемент управления Гармошка предоставляет все важные свойства серверного кода.</span><span class="sxs-lookup"><span data-stu-id="9763f-113">The Accordion control exposes all important properties to server-side code.</span></span> <span data-ttu-id="9763f-114">Помимо прочего `Panes` свойство предоставляет доступ к коллекции областей, которые составляют Accordion.</span><span class="sxs-lookup"><span data-stu-id="9763f-114">Among other things, the `Panes` property grants access to the collection of panes that make up the Accordion.</span></span> <span data-ttu-id="9763f-115">Каждой области нет типа `AccordionPane`.</span><span class="sxs-lookup"><span data-stu-id="9763f-115">Every pane there is of type `AccordionPane`.</span></span> <span data-ttu-id="9763f-116">Таким образом, это просто создание такой области:</span><span class="sxs-lookup"><span data-stu-id="9763f-116">It is therefore trivial to create such a pane:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

<span data-ttu-id="9763f-117">`HeaderContainer` Свойство `AccordionPane` предоставляет доступ к элементам управления ASP.NET в раздел заголовка области; `ContentContainer` свойство `AccordionPane` делает то же самое для раздела содержимого панели.</span><span class="sxs-lookup"><span data-stu-id="9763f-117">The `HeaderContainer` property of `AccordionPane` provides access to the ASP.NET controls within the header section of the pane; the `ContentContainer` property of `AccordionPane` does the same for the content section of the pane.</span></span> <span data-ttu-id="9763f-118">Это позволяет коду ASP.NET для добавления содержимого в области:</span><span class="sxs-lookup"><span data-stu-id="9763f-118">This allows ASP.NET code to add content to the panes:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

<span data-ttu-id="9763f-119">Наконец, необходимо добавить pane(s) `Panes` коллекцию Гармошка:</span><span class="sxs-lookup"><span data-stu-id="9763f-119">Finally, the pane(s) must be added to the `Panes` collection of the Accordion:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

<span data-ttu-id="9763f-120">Ниже приведен полный серверный код, добавляющий к элементу управления Гармошка две панели:</span><span class="sxs-lookup"><span data-stu-id="9763f-120">Here is a complete server-side code that adds two panes to an Accordion control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

<span data-ttu-id="9763f-121">Отсутствует единственный элемент является Accordion, который зависит от наличия ASP.NET `ScriptManager` управления:</span><span class="sxs-lookup"><span data-stu-id="9763f-121">The only missing element is the Accordion itself, which depends on the presence of the ASP.NET `ScriptManager` control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

<span data-ttu-id="9763f-122">Чтобы завершить процесс в примере, двух классов CSS, на который ссылается элемент управления Гармошка предоставляют сведения о стиле для браузера:</span><span class="sxs-lookup"><span data-stu-id="9763f-122">To finish the example, the two CSS classes referenced in the Accordion control provide style information for the browser:</span></span>

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]


<span data-ttu-id="9763f-123">[![Данные в Гармошка динамически добавленные серверный код](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9763f-123">[![The data in the accordion was dynamically added by server-side code](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)</span></span>

<span data-ttu-id="9763f-124">Данные в Гармошка динамически добавленные серверный код ([Просмотр полноразмерное изображение](dynamically-adding-an-accordion-pane-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9763f-124">The data in the accordion was dynamically added by server-side code ([Click to view full-size image](dynamically-adding-an-accordion-pane-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="9763f-125">[Назад](databinding-to-an-accordion-cs.md)
[Вперед](databinding-to-an-accordion-vb.md)</span><span class="sxs-lookup"><span data-stu-id="9763f-125">[Previous](databinding-to-an-accordion-cs.md)
[Next](databinding-to-an-accordion-vb.md)</span></span>
