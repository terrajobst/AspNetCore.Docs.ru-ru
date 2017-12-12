---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: "Создание представления (UI) | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 8c5cc662e2e3c9cb07ca9e30ff57eb084d58e1bb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="create-the-view-ui"></a><span data-ttu-id="febcb-102">Создание представления (UI)</span><span class="sxs-lookup"><span data-stu-id="febcb-102">Create the View (UI)</span></span>
====================
<span data-ttu-id="febcb-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="febcb-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="febcb-104">Загрузка завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="febcb-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="febcb-105">В этом разделе сначала для определения HTML-код для приложения и добавление привязки данных между HTML и модели представления.</span><span class="sxs-lookup"><span data-stu-id="febcb-105">In this section, you will start to define the HTML for the app, and add data binding between the HTML and the view model.</span></span>

<span data-ttu-id="febcb-106">Откройте файл Views/Home/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="febcb-106">Open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="febcb-107">Замените все содержимое этого файла следующим.</span><span class="sxs-lookup"><span data-stu-id="febcb-107">Replace the entire contents of that file with the following.</span></span>

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

<span data-ttu-id="febcb-108">Большинство `div` элементы имеют [начальной загрузки](http://getbootstrap.com/) Задание стиля.</span><span class="sxs-lookup"><span data-stu-id="febcb-108">Most of the `div` elements are there for [Bootstrap](http://getbootstrap.com/) styling.</span></span> <span data-ttu-id="febcb-109">Важные элементы, имеющие `data-bind` атрибуты.</span><span class="sxs-lookup"><span data-stu-id="febcb-109">The important elements are the ones with `data-bind` attributes.</span></span> <span data-ttu-id="febcb-110">Этот атрибут связывает HTML модели представления.</span><span class="sxs-lookup"><span data-stu-id="febcb-110">This attribute links the HTML to the view model.</span></span>

<span data-ttu-id="febcb-111">Пример:</span><span class="sxs-lookup"><span data-stu-id="febcb-111">For example:</span></span>

[!code-html[Main](part-7/samples/sample2.html)]

<span data-ttu-id="febcb-112">В этом примере &quot; `text` &quot; привязки причины `<p>` элемент, чтобы отобразить значение `error` свойство из модели представления.</span><span class="sxs-lookup"><span data-stu-id="febcb-112">In this example, the &quot;`text`&quot; binding causes the `<p>` element to show the value of the `error` property from the view model.</span></span> <span data-ttu-id="febcb-113">Помните, что `error` был объявлен как `ko.observable`:</span><span class="sxs-lookup"><span data-stu-id="febcb-113">Recall that `error` was declared as a `ko.observable`:</span></span>

[!code-javascript[Main](part-7/samples/sample3.js)]

<span data-ttu-id="febcb-114">Каждый раз, когда задается новое значение `error`, маскирования обновит текст в `<p>` элемент.</span><span class="sxs-lookup"><span data-stu-id="febcb-114">Whenever a new value is assigned to `error`, Knockout updates the text in the `<p>` element.</span></span>

<span data-ttu-id="febcb-115">`foreach` Привязки указывает маскирования перебор содержимое `books` массива.</span><span class="sxs-lookup"><span data-stu-id="febcb-115">The `foreach` binding tells Knockout to loop through the contents of the `books` array.</span></span> <span data-ttu-id="febcb-116">Для каждого элемента в массиве маскирования создает новую &lt;li&gt; элемента.</span><span class="sxs-lookup"><span data-stu-id="febcb-116">For each item in the array, Knockout creates a new &lt;li&gt; element.</span></span> <span data-ttu-id="febcb-117">Привязки в контексте `foreach` ссылки на свойства на элемент массива.</span><span class="sxs-lookup"><span data-stu-id="febcb-117">Bindings inside the context of the `foreach` refer to properties on the array item.</span></span> <span data-ttu-id="febcb-118">Пример:</span><span class="sxs-lookup"><span data-stu-id="febcb-118">For example:</span></span>

[!code-html[Main](part-7/samples/sample4.html)]

<span data-ttu-id="febcb-119">Здесь `text` привязки считывает свойство Author каждой книги.</span><span class="sxs-lookup"><span data-stu-id="febcb-119">Here the `text` binding reads the Author property of each book.</span></span>

<span data-ttu-id="febcb-120">Если приложение запускается, он должен выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="febcb-120">If you run the application now, it should look like this:</span></span>

![](part-7/_static/image1.png)

<span data-ttu-id="febcb-121">Список книг загружает асинхронно, после загрузки страницы.</span><span class="sxs-lookup"><span data-stu-id="febcb-121">The list of books loads asynchronously, after the page loads.</span></span> <span data-ttu-id="febcb-122">Прямо сейчас &quot;сведения&quot; ссылки не работают.</span><span class="sxs-lookup"><span data-stu-id="febcb-122">Right now, the &quot;Details&quot; links are not functional.</span></span> <span data-ttu-id="febcb-123">Эта функция будет добавлен в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="febcb-123">We'll add this functionality in the next section.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="febcb-124">[Назад](part-6.md)
[Вперед](part-8.md)</span><span class="sxs-lookup"><span data-stu-id="febcb-124">[Previous](part-6.md)
[Next](part-8.md)</span></span>
