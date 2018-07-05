---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Создание представления (UI) | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: e9ebe60f88ecbf65a6f8d04de9a23d72a72fda83
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364985"
---
<a name="create-the-view-ui"></a><span data-ttu-id="129d0-102">Создание представления (пользовательский Интерфейс)</span><span class="sxs-lookup"><span data-stu-id="129d0-102">Create the View (UI)</span></span>
====================
<span data-ttu-id="129d0-103">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="129d0-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="129d0-104">Скачать завершенный проект</span><span class="sxs-lookup"><span data-stu-id="129d0-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="129d0-105">В этом разделе вам предстоит определить HTML для приложения и добавление привязки данных между HTML и модель представления.</span><span class="sxs-lookup"><span data-stu-id="129d0-105">In this section, you will start to define the HTML for the app, and add data binding between the HTML and the view model.</span></span>

<span data-ttu-id="129d0-106">Откройте файл Views/Home/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="129d0-106">Open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="129d0-107">Замените все содержимое этого файла следующим.</span><span class="sxs-lookup"><span data-stu-id="129d0-107">Replace the entire contents of that file with the following.</span></span>

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

<span data-ttu-id="129d0-108">Большая часть `div` элементы существуют ли [Bootstrap](http://getbootstrap.com/) стиля.</span><span class="sxs-lookup"><span data-stu-id="129d0-108">Most of the `div` elements are there for [Bootstrap](http://getbootstrap.com/) styling.</span></span> <span data-ttu-id="129d0-109">Важные элементы, имеющие `data-bind` атрибуты.</span><span class="sxs-lookup"><span data-stu-id="129d0-109">The important elements are the ones with `data-bind` attributes.</span></span> <span data-ttu-id="129d0-110">Этот атрибут связывает HTML-код для модели представления.</span><span class="sxs-lookup"><span data-stu-id="129d0-110">This attribute links the HTML to the view model.</span></span>

<span data-ttu-id="129d0-111">Пример:</span><span class="sxs-lookup"><span data-stu-id="129d0-111">For example:</span></span>

[!code-html[Main](part-7/samples/sample2.html)]

<span data-ttu-id="129d0-112">В этом примере &quot; `text` &quot; привязки причины `<p>` элемент для отображения значения `error` свойство из модели представления.</span><span class="sxs-lookup"><span data-stu-id="129d0-112">In this example, the &quot;`text`&quot; binding causes the `<p>` element to show the value of the `error` property from the view model.</span></span> <span data-ttu-id="129d0-113">Помните, что `error` был объявлен как `ko.observable`:</span><span class="sxs-lookup"><span data-stu-id="129d0-113">Recall that `error` was declared as a `ko.observable`:</span></span>

[!code-javascript[Main](part-7/samples/sample3.js)]

<span data-ttu-id="129d0-114">Каждый раз, когда назначается новое значение `error`, Knockout обновляет текст в `<p>` элемент.</span><span class="sxs-lookup"><span data-stu-id="129d0-114">Whenever a new value is assigned to `error`, Knockout updates the text in the `<p>` element.</span></span>

<span data-ttu-id="129d0-115">`foreach` Привязки сообщает Knockout циклический перебор содержимое `books` массива.</span><span class="sxs-lookup"><span data-stu-id="129d0-115">The `foreach` binding tells Knockout to loop through the contents of the `books` array.</span></span> <span data-ttu-id="129d0-116">Для каждого элемента в массиве, создает новый Knockout &lt;li&gt; элемент.</span><span class="sxs-lookup"><span data-stu-id="129d0-116">For each item in the array, Knockout creates a new &lt;li&gt; element.</span></span> <span data-ttu-id="129d0-117">Привязки в контексте `foreach` ссылки на свойства для элемента массива.</span><span class="sxs-lookup"><span data-stu-id="129d0-117">Bindings inside the context of the `foreach` refer to properties on the array item.</span></span> <span data-ttu-id="129d0-118">Пример:</span><span class="sxs-lookup"><span data-stu-id="129d0-118">For example:</span></span>

[!code-html[Main](part-7/samples/sample4.html)]

<span data-ttu-id="129d0-119">Здесь `text` привязка позволяет считывать свойства Author каждую книгу.</span><span class="sxs-lookup"><span data-stu-id="129d0-119">Here the `text` binding reads the Author property of each book.</span></span>

<span data-ttu-id="129d0-120">Если запустить приложение сейчас, он должен выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="129d0-120">If you run the application now, it should look like this:</span></span>

![](part-7/_static/image1.png)

<span data-ttu-id="129d0-121">Список книг загружает в асинхронном режиме, после загрузки страницы.</span><span class="sxs-lookup"><span data-stu-id="129d0-121">The list of books loads asynchronously, after the page loads.</span></span> <span data-ttu-id="129d0-122">Прямо сейчас &quot;сведения&quot; ссылки не работают.</span><span class="sxs-lookup"><span data-stu-id="129d0-122">Right now, the &quot;Details&quot; links are not functional.</span></span> <span data-ttu-id="129d0-123">В следующем разделе мы добавим эту функцию.</span><span class="sxs-lookup"><span data-stu-id="129d0-123">We'll add this functionality in the next section.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="129d0-124">[Назад](part-6.md)
> [Вперед](part-8.md)</span><span class="sxs-lookup"><span data-stu-id="129d0-124">[Previous](part-6.md)
[Next](part-8.md)</span></span>
