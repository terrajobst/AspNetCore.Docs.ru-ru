---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
title: Создание элемента управления оценки (C#) | Документы Microsoft
author: wenz
description: Многие веб-сайты из электронной коммерции на сайты сообщества предлагают пользователям скорость статей или элементы. Это обычно требует некоторых кодирование, но у нас есть...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 969fb28f-2bff-4fc4-b24a-27f5e2534a37
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
msc.type: authoredcontent
ms.openlocfilehash: a48cf0ed9402e2875e87ba7bdb76afc5f501a670
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879599"
---
<a name="creating-a-rating-control-c"></a><span data-ttu-id="071ef-104">Создание элемента управления оценки (C#)</span><span class="sxs-lookup"><span data-stu-id="071ef-104">Creating a Rating Control (C#)</span></span>
====================
<span data-ttu-id="071ef-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="071ef-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="071ef-106">[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="071ef-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)</span></span>

> <span data-ttu-id="071ef-107">Многие веб-сайты из электронной коммерции на сайты сообщества предлагают пользователям скорость статей или элементы.</span><span class="sxs-lookup"><span data-stu-id="071ef-107">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="071ef-108">Это обычно требует некоторых кодирование, но у нас есть набор элементов управления в своем распоряжении.</span><span class="sxs-lookup"><span data-stu-id="071ef-108">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>


## <a name="overview"></a><span data-ttu-id="071ef-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="071ef-109">Overview</span></span>

<span data-ttu-id="071ef-110">Многие веб-сайты из электронной коммерции на сайты сообщества предлагают пользователям скорость статей или элементы.</span><span class="sxs-lookup"><span data-stu-id="071ef-110">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="071ef-111">Это обычно требует некоторых кодирование, но у нас есть набор элементов управления в своем распоряжении.</span><span class="sxs-lookup"><span data-stu-id="071ef-111">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>

## <a name="steps"></a><span data-ttu-id="071ef-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="071ef-112">Steps</span></span>

<span data-ttu-id="071ef-113">Во-первых, необходимо (минимум) два вида образов: один для заполненного Оценка элемента и один для оценки пустой элемент.</span><span class="sxs-lookup"><span data-stu-id="071ef-113">First of all, you need (at least) two kinds of images: one for a filled out rating item, and one for an empty rating item.</span></span> <span data-ttu-id="071ef-114">Элемент оценка обычно является звезды или смайлик.</span><span class="sxs-lookup"><span data-stu-id="071ef-114">A rating item is usually a star or a smiley.</span></span> <span data-ttu-id="071ef-115">Для этого сценария найти трех файлов, smiley.png и empty.png и done.png смайлик как часть загрузки исходного кода для этого учебника.</span><span class="sxs-lookup"><span data-stu-id="071ef-115">For this scenario, you find three files, smiley.png and empty.png and smiley-done.png as part of the source code downloads for this tutorial.</span></span>

<span data-ttu-id="071ef-116">Затем создайте новый файл ASP.NET и начните с добавления `ScriptManager` управления к нему:</span><span class="sxs-lookup"><span data-stu-id="071ef-116">Then, create a new ASP.NET file and start with adding a `ScriptManager` control to it:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample1.aspx)]

<span data-ttu-id="071ef-117">Затем добавьте `Rating` управления из набора элементов управления ASP.NET AJAX.</span><span class="sxs-lookup"><span data-stu-id="071ef-117">Then, add the `Rating` control from the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="071ef-118">Следующие атрибуты должны быть заданы в этом примере:</span><span class="sxs-lookup"><span data-stu-id="071ef-118">The following attributes need to be set for this example:</span></span>

- <span data-ttu-id="071ef-119">`CurrentRating` для использования первоначальной оценка</span><span class="sxs-lookup"><span data-stu-id="071ef-119">`CurrentRating` the initial rating to be used</span></span>
- <span data-ttu-id="071ef-120">`MaxRating` Максимальное ограничение</span><span class="sxs-lookup"><span data-stu-id="071ef-120">`MaxRating` the maximum rating</span></span>
- <span data-ttu-id="071ef-121">`EmptyStarCssClass` класс CSS для использования при пустом элементе оценку (звездочка)</span><span class="sxs-lookup"><span data-stu-id="071ef-121">`EmptyStarCssClass` the CSS class to use when a rating item ( star ) is empty</span></span>
- <span data-ttu-id="071ef-122">`FilledStarCssClass` класс CSS для использования при заполнении элемента оценку (звездочка)</span><span class="sxs-lookup"><span data-stu-id="071ef-122">`FilledStarCssClass` the CSS class to use when a rating item ( star ) is filled out</span></span>
- <span data-ttu-id="071ef-123">`StarCssClass` класс CSS для видимой stat</span><span class="sxs-lookup"><span data-stu-id="071ef-123">`StarCssClass` the CSS class to use for a visible stat</span></span>
- <span data-ttu-id="071ef-124">`WaitingStarCssClass` класс CSS для использования в процессе оценку отправляется обратно на сервер</span><span class="sxs-lookup"><span data-stu-id="071ef-124">`WaitingStarCssClass` the CSS class to use while a star rating is sent back to the server</span></span>

<span data-ttu-id="071ef-125">А вот разметка, которая создает элемент управления оценок с пятью элементами (smileys), которых нет заполняется изначально:</span><span class="sxs-lookup"><span data-stu-id="071ef-125">And here is the markup which creates a rating control with five items (smileys) of which none is filled out initially:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample2.aspx)]

<span data-ttu-id="071ef-126">Три упоминаемого классов CSS теперь требуется для отображения файлов соответствующий образ, это легко сделать с помощью CSS:</span><span class="sxs-lookup"><span data-stu-id="071ef-126">The three referenced CSS classes now need to show the appropriate image files, which is easy to do using CSS:</span></span>

[!code-css[Main](creating-a-rating-control-cs/samples/sample3.css)]

<span data-ttu-id="071ef-127">Убедитесь, что укажите ширину и высоту три изображения, в противном случае может выглядеть немного сильно система отображения.</span><span class="sxs-lookup"><span data-stu-id="071ef-127">Make sure that you provide the width and height of the three images, otherwise the display may look a bit messed up.</span></span>

<span data-ttu-id="071ef-128">Наконец результат оценки должно быть отображаемое пользователю (или, по крайней мере, сохраненные в базе данных).</span><span class="sxs-lookup"><span data-stu-id="071ef-128">Finally, the result of the rating should be displayed to the user (or, at least saved in a database).</span></span> <span data-ttu-id="071ef-129">Поэтому добавьте метки для вывода текста сообщения, а кнопка отправки для отправки обратно оценка формы на сервер:</span><span class="sxs-lookup"><span data-stu-id="071ef-129">So add a label for the output of a text message and a submit button to post back the rating form to the server:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample4.aspx)]

<span data-ttu-id="071ef-130">В серверный код, обращения к элементу управления оценка через его `ID` и последующий доступ к его `CurrentRating` свойство, являющееся числом элементов выбранным рейтингом в нашем примере значение в диапазоне от 0 до 5.</span><span class="sxs-lookup"><span data-stu-id="071ef-130">In the server-side code, access the Rating control via its `ID` and then access its `CurrentRating` property which is the number of the selected rating items, in our example a value between 0 and 5.</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample5.aspx)]

<span data-ttu-id="071ef-131">Сохраните страницу и загрузить его в адресную строку браузера.</span><span class="sxs-lookup"><span data-stu-id="071ef-131">Save the page and load it into your browser.</span></span> <span data-ttu-id="071ef-132">Происходит при наведении указателя мыши на элементы (изначально пустой) оценка влияния JavaScript: изменения рейтинга.</span><span class="sxs-lookup"><span data-stu-id="071ef-132">When you hover over the (initially empty) rating items, a JavaScript effect occurs: The rating changes.</span></span> <span data-ttu-id="071ef-133">При нажатии кнопки на наборе звезд, сохраняется текущая оценка.</span><span class="sxs-lookup"><span data-stu-id="071ef-133">When you click on the set of stars, the current rating is retained.</span></span> <span data-ttu-id="071ef-134">Наконец при отправке формы, серверный код генерирует выбранным рейтингом.</span><span class="sxs-lookup"><span data-stu-id="071ef-134">Finally, when you submit the form, the server-side code outputs the selected rating.</span></span>


<span data-ttu-id="071ef-135">[![Создание оценок системы с минимальным объемом кода](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="071ef-135">[![Creating a rating system with minimal code](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="071ef-136">Создание систему оценок с помощью минимального программного кода ([Просмотр полноразмерное изображение](creating-a-rating-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="071ef-136">Creating a rating system with minimal code ([Click to view full-size image](creating-a-rating-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="071ef-137">Вперед</span><span class="sxs-lookup"><span data-stu-id="071ef-137">Next</span></span>](creating-a-rating-control-vb.md)
