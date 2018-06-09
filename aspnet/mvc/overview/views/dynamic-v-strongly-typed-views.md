---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Динамические v. Строго типизированные представления | Документы Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2011
ms.topic: article
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 8a96d43e04a0a50d5176c10c26aa49918a0e56ef
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "26504093"
---
<a name="dynamic-v-strongly-typed-views"></a><span data-ttu-id="a1be1-103">Динамические v.</span><span class="sxs-lookup"><span data-stu-id="a1be1-103">Dynamic v.</span></span> <span data-ttu-id="a1be1-104">Строго типизированные представления</span><span class="sxs-lookup"><span data-stu-id="a1be1-104">Strongly Typed Views</span></span>
====================
<span data-ttu-id="a1be1-105">по [Рик Андерсон](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="a1be1-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="a1be1-106">Для передачи данных из контроллера в представление в ASP.NET MVC 3 тремя способами:</span><span class="sxs-lookup"><span data-stu-id="a1be1-106">There are three ways to pass information from a controller to a view in ASP.NET MVC 3:</span></span>

1. <span data-ttu-id="a1be1-107">В виде строго типизированных модели объекта.</span><span class="sxs-lookup"><span data-stu-id="a1be1-107">As a strongly typed model object.</span></span>
2. <span data-ttu-id="a1be1-108">Как динамического типа (с помощью @model динамические)</span><span class="sxs-lookup"><span data-stu-id="a1be1-108">As a dynamic type (using @model dynamic)</span></span>
3. <span data-ttu-id="a1be1-109">Использование ViewBag</span><span class="sxs-lookup"><span data-stu-id="a1be1-109">Using the ViewBag</span></span>

<span data-ttu-id="a1be1-110">Я написал простого приложения MVC 3 верхней блога для сравнения и сравните динамические и строго типизированные представления.</span><span class="sxs-lookup"><span data-stu-id="a1be1-110">I've written a simple MVC 3 Top Blog application to compare and contrast dynamic and strongly typed views.</span></span> <span data-ttu-id="a1be1-111">Контроллер начинается с простой список блогов:</span><span class="sxs-lookup"><span data-stu-id="a1be1-111">The controller starts out with a simple list of blogs:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

<span data-ttu-id="a1be1-112">Щелкните правой кнопкой мыши в методе IndexNotStonglyTyped() и добавьте представления Razor.</span><span class="sxs-lookup"><span data-stu-id="a1be1-112">Right click in the IndexNotStonglyTyped() method and add a Razor view.</span></span>

<span data-ttu-id="a1be1-113">[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a1be1-113">[![8475.NotStronglyTypedView[1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span></span>

<span data-ttu-id="a1be1-114">Убедитесь, что **создать строго типизированное представление** флажок не установлен.</span><span class="sxs-lookup"><span data-stu-id="a1be1-114">Make sure the **Create a strongly-typed view** box is not checked.</span></span> <span data-ttu-id="a1be1-115">Полученное представление не содержит много:</span><span class="sxs-lookup"><span data-stu-id="a1be1-115">The resulting view doesn't contain much:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

<span data-ttu-id="a1be1-116">Так как мы используем динамические и строго типизированного представления, технология intellisense не помогают.</span><span class="sxs-lookup"><span data-stu-id="a1be1-116">Because we're using a dynamic and not a strongly typed view, intellisense doesn't help us.</span></span> <span data-ttu-id="a1be1-117">Ниже приведен полный код:</span><span class="sxs-lookup"><span data-stu-id="a1be1-117">The completed code is shown below:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

<span data-ttu-id="a1be1-118">[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="a1be1-118">[![6646.NotStronglyTypedView_5F00_IE[1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span></span>

<span data-ttu-id="a1be1-119">Теперь мы добавим строго типизированного представления.</span><span class="sxs-lookup"><span data-stu-id="a1be1-119">Now we'll add a strongly typed view.</span></span> <span data-ttu-id="a1be1-120">Добавьте следующий код на контроллер:</span><span class="sxs-lookup"><span data-stu-id="a1be1-120">Add the following code to the controller:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


<span data-ttu-id="a1be1-121">Обратите внимание, что это именно же возвращаемого View(topBlogs); Вызовите как не строго типизированного представления.</span><span class="sxs-lookup"><span data-stu-id="a1be1-121">Notice it's exactly the same return View(topBlogs); call as the non-strongly typed view.</span></span> <span data-ttu-id="a1be1-122">Щелкните правой кнопкой мыши внутри *StonglyTypedIndex()* и выберите **добавить представление**.</span><span class="sxs-lookup"><span data-stu-id="a1be1-122">Right click inside of *StonglyTypedIndex()* and select **Add View**.</span></span> <span data-ttu-id="a1be1-123">На этот раз выберите **блог** класс модели и выберите **списка** как шаблон формирования.</span><span class="sxs-lookup"><span data-stu-id="a1be1-123">This time select the **Blog** Model class and select **List** as the Scaffold template.</span></span>

<span data-ttu-id="a1be1-124">[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="a1be1-124">[![5658.StrongView[1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span></span>

<span data-ttu-id="a1be1-125">В новый шаблон представления мы получить поддержку intellisense.</span><span class="sxs-lookup"><span data-stu-id="a1be1-125">Inside the new view template we get intellisense support.</span></span>

<span data-ttu-id="a1be1-126">[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="a1be1-126">[![7002.intellesince[1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span></span>

<span data-ttu-id="a1be1-127">Можно загрузить проект c# [здесь](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span><span class="sxs-lookup"><span data-stu-id="a1be1-127">The c# project can be downloaded [here](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span></span>
