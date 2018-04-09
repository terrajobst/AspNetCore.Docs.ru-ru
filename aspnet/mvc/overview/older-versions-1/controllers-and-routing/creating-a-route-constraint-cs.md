---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
title: Создание ограничения маршрута (C#) | Документы Microsoft
author: StephenWalther
description: В этом учебнике Стивен Вальтер показано, как можно контролировать, как браузер запрашивает маршруты соответствия путем создания ограничения маршрута с помощью регулярных выражений.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 0bfd06b1-12d3-4fbb-9779-a82e5eb7fe7d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 3159feb6538e3048f4f235f7d549e692604ca4e7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-route-constraint-c"></a><span data-ttu-id="8fa6d-103">Создание ограничения маршрута (C#)</span><span class="sxs-lookup"><span data-stu-id="8fa6d-103">Creating a Route Constraint (C#)</span></span>
====================
<span data-ttu-id="8fa6d-104">по [Стивен Вальтер](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="8fa6d-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="8fa6d-105">В этом учебнике Стивен Вальтер показано, как можно контролировать, как браузер запрашивает маршруты соответствия путем создания ограничения маршрута с помощью регулярных выражений.</span><span class="sxs-lookup"><span data-stu-id="8fa6d-105">In this tutorial, Stephen Walther demonstrates how you can control how browser requests match routes by creating route constraints with regular expressions.</span></span>


<span data-ttu-id="8fa6d-106">Ограничения маршрута используется для ограничения запросы браузера, которые соответствуют конкретной маршрута.</span><span class="sxs-lookup"><span data-stu-id="8fa6d-106">You use route constraints to restrict the browser requests that match a particular route.</span></span> <span data-ttu-id="8fa6d-107">Регулярное выражение можно использовать для указания ограничения маршрута.</span><span class="sxs-lookup"><span data-stu-id="8fa6d-107">You can use a regular expression to specify a route constraint.</span></span>

<span data-ttu-id="8fa6d-108">Например предположим, что в файле Global.asax вы определили маршрута в листинге 1.</span><span class="sxs-lookup"><span data-stu-id="8fa6d-108">For example, imagine that you have defined the route in Listing 1 in your Global.asax file.</span></span>

<span data-ttu-id="8fa6d-109">**1 — Global.asax.cs со списком**</span><span class="sxs-lookup"><span data-stu-id="8fa6d-109">**Listing 1 - Global.asax.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample1.cs)]

<span data-ttu-id="8fa6d-110">Листинг 1 содержит маршрут с именем продукта.</span><span class="sxs-lookup"><span data-stu-id="8fa6d-110">Listing 1 contains a route named Product.</span></span> <span data-ttu-id="8fa6d-111">Маршрут продукта можно использовать для сопоставления запросы браузера ProductController, содержащихся в списке 2.</span><span class="sxs-lookup"><span data-stu-id="8fa6d-111">You can use the Product route to map browser requests to the ProductController contained in Listing 2.</span></span>

<span data-ttu-id="8fa6d-112">**Листинг 2 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="8fa6d-112">**Listing 2 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample2.cs)]

<span data-ttu-id="8fa6d-113">Обратите внимание, что Details() действию, предоставляемым контроллером продукта принимает один параметр с именем productId.</span><span class="sxs-lookup"><span data-stu-id="8fa6d-113">Notice that the Details() action exposed by the Product controller accepts a single parameter named productId.</span></span> <span data-ttu-id="8fa6d-114">Этот параметр является параметром целое число со знаком.</span><span class="sxs-lookup"><span data-stu-id="8fa6d-114">This parameter is an integer parameter.</span></span>

<span data-ttu-id="8fa6d-115">Маршрут, определенный в список 1 будет соответствовать любой из следующих URL-адресов:</span><span class="sxs-lookup"><span data-stu-id="8fa6d-115">The route defined in Listing 1 will match any of the following URLs:</span></span>

- <span data-ttu-id="8fa6d-116">/ Продукта/23</span><span class="sxs-lookup"><span data-stu-id="8fa6d-116">/Product/23</span></span>
- <span data-ttu-id="8fa6d-117">/ Продукта/7</span><span class="sxs-lookup"><span data-stu-id="8fa6d-117">/Product/7</span></span>

<span data-ttu-id="8fa6d-118">К сожалению маршрут будет также сопоставлять следующие URL-адреса:</span><span class="sxs-lookup"><span data-stu-id="8fa6d-118">Unfortunately, the route will also match the following URLs:</span></span>

- <span data-ttu-id="8fa6d-119">/ Продукта или сведения</span><span class="sxs-lookup"><span data-stu-id="8fa6d-119">/Product/blah</span></span>
- <span data-ttu-id="8fa6d-120">/ Продукта или apple</span><span class="sxs-lookup"><span data-stu-id="8fa6d-120">/Product/apple</span></span>

<span data-ttu-id="8fa6d-121">Поскольку действие Details() ожидает целочисленный параметр, выполняющего запрос, содержащий нечто, отличное от целочисленное значение приведет к ошибке.</span><span class="sxs-lookup"><span data-stu-id="8fa6d-121">Because the Details() action expects an integer parameter, making a request that contains something other than an integer value will cause an error.</span></span> <span data-ttu-id="8fa6d-122">К примеру при вводе /Product/apple URL-адрес в адресную строку браузера будет возвращена страница ошибки на рис. 1.</span><span class="sxs-lookup"><span data-stu-id="8fa6d-122">For example, if you type the URL /Product/apple into your browser then you will get the error page in Figure 1.</span></span>


<span data-ttu-id="8fa6d-123">[![Диалоговое окно нового проекта](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8fa6d-123">[![The New Project dialog box](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)</span></span>

<span data-ttu-id="8fa6d-124">**На рисунке 01**: страница разрезать ([щелкните, чтобы просмотреть полноразмерное изображение](creating-a-route-constraint-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="8fa6d-124">**Figure 01**: Seeing a page explode ([Click to view full-size image](creating-a-route-constraint-cs/_static/image2.png))</span></span>


<span data-ttu-id="8fa6d-125">Что Вы действительно хотите сделать — только соответствует URL-адреса, содержащие productId правильную целое число со знаком.</span><span class="sxs-lookup"><span data-stu-id="8fa6d-125">What you really want to do is only match URLs that contain a proper integer productId.</span></span> <span data-ttu-id="8fa6d-126">Ограничение можно использовать при определении маршрута для ограничения URL-адреса, которые соответствуют маршруту.</span><span class="sxs-lookup"><span data-stu-id="8fa6d-126">You can use a constraint when defining a route to restrict the URLs that match the route.</span></span> <span data-ttu-id="8fa6d-127">Измененный маршрут продукта в списке 3 содержит ограничение регулярное выражение, которое соответствует только целые числа.</span><span class="sxs-lookup"><span data-stu-id="8fa6d-127">The modified Product route in Listing 3 contains a regular expression constraint that only matches integers.</span></span>

<span data-ttu-id="8fa6d-128">**Листинг 3 - Global.asax.cs**</span><span class="sxs-lookup"><span data-stu-id="8fa6d-128">**Listing 3 - Global.asax.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample3.cs)]

<span data-ttu-id="8fa6d-129">\D+ регулярного выражения соответствует одной или более целых чисел.</span><span class="sxs-lookup"><span data-stu-id="8fa6d-129">The regular expression \d+ matches one or more integers.</span></span> <span data-ttu-id="8fa6d-130">Это ограничение в результате сопоставления следующие URL-адреса маршрута продукта:</span><span class="sxs-lookup"><span data-stu-id="8fa6d-130">This constraint causes the Product route to match the following URLs:</span></span>

- <span data-ttu-id="8fa6d-131">/ Продукта/3</span><span class="sxs-lookup"><span data-stu-id="8fa6d-131">/Product/3</span></span>
- <span data-ttu-id="8fa6d-132">/ Product-8999</span><span class="sxs-lookup"><span data-stu-id="8fa6d-132">/Product/8999</span></span>

<span data-ttu-id="8fa6d-133">Но не следующие URL-адреса:</span><span class="sxs-lookup"><span data-stu-id="8fa6d-133">But not the following URLs:</span></span>

- <span data-ttu-id="8fa6d-134">/ Продукта или apple</span><span class="sxs-lookup"><span data-stu-id="8fa6d-134">/Product/apple</span></span>
- <span data-ttu-id="8fa6d-135">/ Продукта</span><span class="sxs-lookup"><span data-stu-id="8fa6d-135">/Product</span></span>

- <span data-ttu-id="8fa6d-136">Эти запросы браузера будет обрабатываться по другому маршруту или, если отсутствуют соответствующие маршруты *не удалось найти ресурс* будет возвращена ошибка.</span><span class="sxs-lookup"><span data-stu-id="8fa6d-136">These browser requests will be handled by another route or, if there are no matching routes, a *The resource could not be found* error will be returned.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8fa6d-137">[Назад](creating-custom-routes-cs.md)
> [Вперед](creating-a-custom-route-constraint-cs.md)</span><span class="sxs-lookup"><span data-stu-id="8fa6d-137">[Previous](creating-custom-routes-cs.md)
[Next](creating-a-custom-route-constraint-cs.md)</span></span>
