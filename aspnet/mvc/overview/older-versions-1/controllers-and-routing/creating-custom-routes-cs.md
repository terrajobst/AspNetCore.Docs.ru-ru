---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
title: Создание пользовательских маршрутов (C#) | Документация Майкрософт
author: microsoft
description: Узнайте, как добавить пользовательские маршруты для приложения ASP.NET MVC. В этом руководстве вы узнаете, как изменить таблицы маршрутизации по умолчанию в файле Global.asax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 3cd08f02-8763-490a-b625-2ac96a24b73f
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
msc.type: authoredcontent
ms.openlocfilehash: 1bb694b08d595b2ce75123c3da0e9b8e8d60a652
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399961"
---
<a name="creating-custom-routes-c"></a><span data-ttu-id="3f351-104">Создание пользовательских маршрутов (C#)</span><span class="sxs-lookup"><span data-stu-id="3f351-104">Creating Custom Routes (C#)</span></span>
====================
<span data-ttu-id="3f351-105">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="3f351-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="3f351-106">Узнайте, как добавить пользовательские маршруты для приложения ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3f351-106">Learn how to add custom routes to an ASP.NET MVC application.</span></span> <span data-ttu-id="3f351-107">В этом руководстве вы узнаете, как изменить таблицы маршрутизации по умолчанию в файле Global.asax.</span><span class="sxs-lookup"><span data-stu-id="3f351-107">In this tutorial, you learn how to modify the default route table in the Global.asax file.</span></span>


<span data-ttu-id="3f351-108">В этом руководстве вы узнаете, как добавить настраиваемый маршрут в приложении ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3f351-108">In this tutorial, you learn how to add a custom route to an ASP.NET MVC application.</span></span> <span data-ttu-id="3f351-109">Вы узнаете, как изменение таблицы маршрутизации по умолчанию в файле Global.asax с настраиваемый маршрут.</span><span class="sxs-lookup"><span data-stu-id="3f351-109">You learn how to modify the default route table in the Global.asax file with a custom route.</span></span>

<span data-ttu-id="3f351-110">Для многих простых приложений ASP.NET MVC таблицы маршрутизации по умолчанию будет работать нормально.</span><span class="sxs-lookup"><span data-stu-id="3f351-110">For many simple ASP.NET MVC applications, the default route table will work just fine.</span></span> <span data-ttu-id="3f351-111">Тем не менее может выясниться, что специализированные требования к маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="3f351-111">However, you might discover that you have specialized routing needs.</span></span> <span data-ttu-id="3f351-112">В этом случае можно создать настраиваемый маршрут.</span><span class="sxs-lookup"><span data-stu-id="3f351-112">In that case, you can create a custom route.</span></span>

<span data-ttu-id="3f351-113">Допустим, что вы создаете приложение блога.</span><span class="sxs-lookup"><span data-stu-id="3f351-113">Imagine, for example, that you are building a blog application.</span></span> <span data-ttu-id="3f351-114">Может потребоваться обрабатывать входящие запросы, которые выглядят следующим образом:</span><span class="sxs-lookup"><span data-stu-id="3f351-114">You might want to handle incoming requests that look like this:</span></span>

<span data-ttu-id="3f351-115">/ Archive/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="3f351-115">/Archive/12-25-2009</span></span>

<span data-ttu-id="3f351-116">Когда пользователь вводит этот запрос, необходимо вернуть в записи блога, соответствующий дате 12/25/2009.</span><span class="sxs-lookup"><span data-stu-id="3f351-116">When a user enters this request, you want to return the blog entry that corresponds to the date 12/25/2009.</span></span> <span data-ttu-id="3f351-117">Чтобы обработать этот тип запроса, необходимо создать настраиваемый маршрут.</span><span class="sxs-lookup"><span data-stu-id="3f351-117">In order to handle this type of request, you need to create a custom route.</span></span>

<span data-ttu-id="3f351-118">Файл Global.asax в листинге 1 содержит новый настраиваемый маршрут с именем блог, который обрабатывает запросы, которые выглядят как /Archive/*Дата записи*.</span><span class="sxs-lookup"><span data-stu-id="3f351-118">The Global.asax file in Listing 1 contains a new custom route, named Blog, which handles requests that look like /Archive/*entry date*.</span></span>

<span data-ttu-id="3f351-119">**В листинге 1 - Global.asax (с настраиваемый маршрут)**</span><span class="sxs-lookup"><span data-stu-id="3f351-119">**Listing 1 - Global.asax (with custom route)**</span></span>

[!code-csharp[Main](creating-custom-routes-cs/samples/sample1.cs)]

<span data-ttu-id="3f351-120">Важен порядок маршруты, добавленные в таблицу маршрутов.</span><span class="sxs-lookup"><span data-stu-id="3f351-120">The order of the routes that you add to the route table is important.</span></span> <span data-ttu-id="3f351-121">Наш новый настраиваемый маршрут блога добавляется перед существующего маршрута по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3f351-121">Our new custom Blog route is added before the existing Default route.</span></span> <span data-ttu-id="3f351-122">Если вы изменен порядок, а затем маршрут по умолчанию всегда будет вызван вместо пользовательского маршрута.</span><span class="sxs-lookup"><span data-stu-id="3f351-122">If you reversed the order, then the Default route always will get called instead of the custom route.</span></span>

<span data-ttu-id="3f351-123">Настраиваемый маршрут блог соответствует любой запрос, который начинается с/Archive /.</span><span class="sxs-lookup"><span data-stu-id="3f351-123">The custom Blog route matches any request that starts with /Archive/.</span></span> <span data-ttu-id="3f351-124">Таким образом он удовлетворяет всем следующим URL-адресам:</span><span class="sxs-lookup"><span data-stu-id="3f351-124">So, it matches all of the following URLs:</span></span>

- <span data-ttu-id="3f351-125">/ Archive/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="3f351-125">/Archive/12-25-2009</span></span>

- <span data-ttu-id="3f351-126">/ Archive/10-6-2004</span><span class="sxs-lookup"><span data-stu-id="3f351-126">/Archive/10-6-2004</span></span>

- <span data-ttu-id="3f351-127">/ Archive/apple</span><span class="sxs-lookup"><span data-stu-id="3f351-127">/Archive/apple</span></span>

<span data-ttu-id="3f351-128">Настраиваемый маршрут сопоставляет входящий запрос к контроллеру с именем архива и вызывает действие Entry().</span><span class="sxs-lookup"><span data-stu-id="3f351-128">The custom route maps the incoming request to a controller named Archive and invokes the Entry() action.</span></span> <span data-ttu-id="3f351-129">При вызове метода Entry() Дата записи передается как параметр с именем entryDate.</span><span class="sxs-lookup"><span data-stu-id="3f351-129">When the Entry() method is called, the entry date is passed as a parameter named entryDate.</span></span>

<span data-ttu-id="3f351-130">Можно использовать настраиваемый маршрут блога с помощью контроллера в листинге 2.</span><span class="sxs-lookup"><span data-stu-id="3f351-130">You can use the Blog custom route with the controller in Listing 2.</span></span>

<span data-ttu-id="3f351-131">**В листинге 2 - ArchiveController.cs**</span><span class="sxs-lookup"><span data-stu-id="3f351-131">**Listing 2 - ArchiveController.cs**</span></span>

[!code-csharp[Main](creating-custom-routes-cs/samples/sample2.cs)]

<span data-ttu-id="3f351-132">Обратите внимание на то, что метод Entry() в листинге 2 принимает параметр типа DateTime.</span><span class="sxs-lookup"><span data-stu-id="3f351-132">Notice that the Entry() method in Listing 2 accepts a parameter of type DateTime.</span></span> <span data-ttu-id="3f351-133">Платформа MVC может преобразовать эту дату записи из URL-адрес в значение даты и времени автоматически.</span><span class="sxs-lookup"><span data-stu-id="3f351-133">The MVC framework is smart enough to convert the entry date from the URL into a DateTime value automatically.</span></span> <span data-ttu-id="3f351-134">Если параметр записи даты в URL-адресе невозможно преобразовать в значение DateTime, возникает ошибка (см. рис. 1).</span><span class="sxs-lookup"><span data-stu-id="3f351-134">If the entry date parameter from the URL cannot be converted to a DateTime, an error is raised (see Figure 1).</span></span>

<span data-ttu-id="3f351-135">**Рис. 1. Ошибка в результате преобразования параметра**</span><span class="sxs-lookup"><span data-stu-id="3f351-135">**Figure 1 - Error from converting parameter**</span></span>


<span data-ttu-id="3f351-136">[![В диалоговом окне нового проекта](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3f351-136">[![The New Project dialog box](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)</span></span>

<span data-ttu-id="3f351-137">**Рис 01**: ошибка в результате преобразования параметра ([Просмотр полноразмерного изображения](creating-custom-routes-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="3f351-137">**Figure 01**: Error from converting parameter ([Click to view full-size image](creating-custom-routes-cs/_static/image2.png))</span></span>


## <a name="summary"></a><span data-ttu-id="3f351-138">Сводка</span><span class="sxs-lookup"><span data-stu-id="3f351-138">Summary</span></span>

<span data-ttu-id="3f351-139">Цель этого учебника было продемонстрировать, как можно создать настраиваемый маршрут.</span><span class="sxs-lookup"><span data-stu-id="3f351-139">The goal of this tutorial was to demonstrate how you can create a custom route.</span></span> <span data-ttu-id="3f351-140">Вы узнали, как добавить настраиваемый маршрут в таблицу маршрутов в файле Global.asax, который представляет записи в блогах.</span><span class="sxs-lookup"><span data-stu-id="3f351-140">You learned how to add a custom route to the route table in the Global.asax file that represents blog entries.</span></span> <span data-ttu-id="3f351-141">Мы рассмотрели способ сопоставления запросов для записи в блогах контроллер с именем ArchiveController и действие контроллера с именем Entry().</span><span class="sxs-lookup"><span data-stu-id="3f351-141">We discussed how to map requests for blog entries to a controller named ArchiveController and a controller action named Entry().</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3f351-142">[Назад](aspnet-mvc-controllers-overview-cs.md)
> [Вперед](creating-a-route-constraint-cs.md)</span><span class="sxs-lookup"><span data-stu-id="3f351-142">[Previous](aspnet-mvc-controllers-overview-cs.md)
[Next](creating-a-route-constraint-cs.md)</span></span>
