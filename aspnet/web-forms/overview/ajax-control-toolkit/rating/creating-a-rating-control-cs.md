---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
title: Создание элемента управления Rating (C#) | Документация Майкрософт
author: wenz
description: Многие веб-узлы, от электронной коммерции на сайты сообщества, предлагают пользователям скорость статьи или элементов. Обычно для этого требуется некоторые усилия на написание кода, но у нас есть...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 969fb28f-2bff-4fc4-b24a-27f5e2534a37
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 7be954a73c6c08bca9992aacf6ad529bc61c9247
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389590"
---
<a name="creating-a-rating-control-c"></a><span data-ttu-id="fb6e3-104">Создание элемента управления Rating (C#)</span><span class="sxs-lookup"><span data-stu-id="fb6e3-104">Creating a Rating Control (C#)</span></span>
====================
<span data-ttu-id="fb6e3-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="fb6e3-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="fb6e3-106">[Скачать код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="fb6e3-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)</span></span>

> <span data-ttu-id="fb6e3-107">Многие веб-узлы, от электронной коммерции на сайты сообщества, предлагают пользователям скорость статьи или элементов.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-107">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="fb6e3-108">Обычно для этого требуется некоторые усилия на написание кода, но у нас есть набор элементов управления в своем распоряжении.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-108">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>


## <a name="overview"></a><span data-ttu-id="fb6e3-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="fb6e3-109">Overview</span></span>

<span data-ttu-id="fb6e3-110">Многие веб-узлы, от электронной коммерции на сайты сообщества, предлагают пользователям скорость статьи или элементов.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-110">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="fb6e3-111">Обычно для этого требуется некоторые усилия на написание кода, но у нас есть набор элементов управления в своем распоряжении.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-111">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>

## <a name="steps"></a><span data-ttu-id="fb6e3-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="fb6e3-112">Steps</span></span>

<span data-ttu-id="fb6e3-113">Во-первых, необходимо (минимум) два типа образов: один для заполненного Оценка элемента и один для оценки пустой элемент.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-113">First of all, you need (at least) two kinds of images: one for a filled out rating item, and one for an empty rating item.</span></span> <span data-ttu-id="fb6e3-114">Оценка элемент – обычно типа "звезда" или смайлик.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-114">A rating item is usually a star or a smiley.</span></span> <span data-ttu-id="fb6e3-115">В этом сценарии поиск трех файлов, smiley.png и empty.png и done.png смайлик как часть загрузки исходного кода, в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-115">For this scenario, you find three files, smiley.png and empty.png and smiley-done.png as part of the source code downloads for this tutorial.</span></span>

<span data-ttu-id="fb6e3-116">Затем создайте новый файл ASP.NET и начните с добавления `ScriptManager` элемента управления к нему:</span><span class="sxs-lookup"><span data-stu-id="fb6e3-116">Then, create a new ASP.NET file and start with adding a `ScriptManager` control to it:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample1.aspx)]

<span data-ttu-id="fb6e3-117">Затем добавьте `Rating` элемента управления ASP.NET AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-117">Then, add the `Rating` control from the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="fb6e3-118">Следующие атрибуты должны быть заданы в этом примере:</span><span class="sxs-lookup"><span data-stu-id="fb6e3-118">The following attributes need to be set for this example:</span></span>

- <span data-ttu-id="fb6e3-119">`CurrentRating` Начальная оценка для использования</span><span class="sxs-lookup"><span data-stu-id="fb6e3-119">`CurrentRating` the initial rating to be used</span></span>
- <span data-ttu-id="fb6e3-120">`MaxRating` Максимальный рейтинг</span><span class="sxs-lookup"><span data-stu-id="fb6e3-120">`MaxRating` the maximum rating</span></span>
- <span data-ttu-id="fb6e3-121">`EmptyStarCssClass` класс CSS для использования при пустом элемент рейтинг (типа "звезда")</span><span class="sxs-lookup"><span data-stu-id="fb6e3-121">`EmptyStarCssClass` the CSS class to use when a rating item ( star ) is empty</span></span>
- <span data-ttu-id="fb6e3-122">`FilledStarCssClass` класс CSS для использования при заполнении элемента рейтинг (типа "звезда")</span><span class="sxs-lookup"><span data-stu-id="fb6e3-122">`FilledStarCssClass` the CSS class to use when a rating item ( star ) is filled out</span></span>
- <span data-ttu-id="fb6e3-123">`StarCssClass` класс CSS, используемый для видимой stat</span><span class="sxs-lookup"><span data-stu-id="fb6e3-123">`StarCssClass` the CSS class to use for a visible stat</span></span>
- <span data-ttu-id="fb6e3-124">`WaitingStarCssClass` класс CSS для использования, пока число звезд в рейтинге отправляется на сервер</span><span class="sxs-lookup"><span data-stu-id="fb6e3-124">`WaitingStarCssClass` the CSS class to use while a star rating is sent back to the server</span></span>

<span data-ttu-id="fb6e3-125">А вот разметки, который создает элемент управления rating с пятью элементами (smileys), которых нет заполняется изначально:</span><span class="sxs-lookup"><span data-stu-id="fb6e3-125">And here is the markup which creates a rating control with five items (smileys) of which none is filled out initially:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample2.aspx)]

<span data-ttu-id="fb6e3-126">Теперь три упоминаемого классы CSS необходимо показать файлы подходящий образ, это легко сделать с помощью CSS:</span><span class="sxs-lookup"><span data-stu-id="fb6e3-126">The three referenced CSS classes now need to show the appropriate image files, which is easy to do using CSS:</span></span>

[!code-css[Main](creating-a-rating-control-cs/samples/sample3.css)]

<span data-ttu-id="fb6e3-127">Убедитесь, что указываемые ширину и высоту три изображения, в противном случае может выглядеть немного, насколько сильно система отображения.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-127">Make sure that you provide the width and height of the three images, otherwise the display may look a bit messed up.</span></span>

<span data-ttu-id="fb6e3-128">И, наконец результат оценки должно быть отображаемое пользователю (или, по крайней мере сохранены в базе данных).</span><span class="sxs-lookup"><span data-stu-id="fb6e3-128">Finally, the result of the rating should be displayed to the user (or, at least saved in a database).</span></span> <span data-ttu-id="fb6e3-129">Поэтому добавьте метку для вывода текстовое сообщение и кнопка отправки обратной передачи формы оценки на сервер:</span><span class="sxs-lookup"><span data-stu-id="fb6e3-129">So add a label for the output of a text message and a submit button to post back the rating form to the server:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample4.aspx)]

<span data-ttu-id="fb6e3-130">В коде на стороне сервера, доступ через элемент управления Rating его `ID` и затем получить доступ к его `CurrentRating` свойство, которое является число элементов выбранной оценки, в нашем примере значение в диапазоне от 0 до 5.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-130">In the server-side code, access the Rating control via its `ID` and then access its `CurrentRating` property which is the number of the selected rating items, in our example a value between 0 and 5.</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample5.aspx)]

<span data-ttu-id="fb6e3-131">Сохраните страницу и загрузить его в адресную строку браузера.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-131">Save the page and load it into your browser.</span></span> <span data-ttu-id="fb6e3-132">Происходит при наведении указателя мыши на элементы (изначально пуста) рейтинг эффекта JavaScript: изменения рейтинга.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-132">When you hover over the (initially empty) rating items, a JavaScript effect occurs: The rating changes.</span></span> <span data-ttu-id="fb6e3-133">При щелчке набор звезд, сохраняется текущая оценка.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-133">When you click on the set of stars, the current rating is retained.</span></span> <span data-ttu-id="fb6e3-134">Наконец при отправке формы, серверный код выводит выбранные оценка.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-134">Finally, when you submit the form, the server-side code outputs the selected rating.</span></span>


<span data-ttu-id="fb6e3-135">[![Создание оценки системы с минимальным использованием кода](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fb6e3-135">[![Creating a rating system with minimal code](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="fb6e3-136">Создание оценки системы с минимальным использованием кода ([Просмотр полноразмерного изображения](creating-a-rating-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="fb6e3-136">Creating a rating system with minimal code ([Click to view full-size image](creating-a-rating-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="fb6e3-137">Вперед</span><span class="sxs-lookup"><span data-stu-id="fb6e3-137">Next</span></span>](creating-a-rating-control-vb.md)
