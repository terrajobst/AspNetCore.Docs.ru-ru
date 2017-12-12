---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
title: "Обзор маршрутизации ASP.NET MVC (C#) | Документы Microsoft"
author: StephenWalther
description: "В этом учебнике Стивен Вальтер показано, как платформа ASP.NET MVC сопоставляет запросы браузера действия контроллера."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 5b39d2d5-4bf9-4d04-94c7-81b84dfeeb31
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 714fd1939ffeba11b84a82e80193ecbbe4b12e09
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-routing-overview-c"></a><span data-ttu-id="13cf0-103">Обзор маршрутизации ASP.NET MVC (C#)</span><span class="sxs-lookup"><span data-stu-id="13cf0-103">ASP.NET MVC Routing Overview (C#)</span></span>
====================
<span data-ttu-id="13cf0-104">по [Стивен Вальтер](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="13cf0-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="13cf0-105">В этом учебнике Стивен Вальтер показано, как платформа ASP.NET MVC сопоставляет запросы браузера действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="13cf0-105">In this tutorial, Stephen Walther shows how the ASP.NET MVC framework maps browser requests to controller actions.</span></span>


<span data-ttu-id="13cf0-106">В этом учебнике, предназначенные для важной характеристикой каждое приложение ASP.NET MVC вызывается *маршрутизация ASP.NET*.</span><span class="sxs-lookup"><span data-stu-id="13cf0-106">In this tutorial, you are introduced to an important feature of every ASP.NET MVC application called *ASP.NET Routing*.</span></span> <span data-ttu-id="13cf0-107">Модуль маршрутизации ASP.NET выполняет сопоставление входящих запросов браузера для конкретного действия контроллера MVC.</span><span class="sxs-lookup"><span data-stu-id="13cf0-107">The ASP.NET Routing module is responsible for mapping incoming browser requests to particular MVC controller actions.</span></span> <span data-ttu-id="13cf0-108">Изучив этот учебник вы сможете понять, как в таблице маршрутов стандартные запросы сопоставляются действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="13cf0-108">By the end of this tutorial, you will understand how the standard route table maps requests to controller actions.</span></span>

## <a name="using-the-default-route-table"></a><span data-ttu-id="13cf0-109">С помощью таблицы маршрутов по умолчанию</span><span class="sxs-lookup"><span data-stu-id="13cf0-109">Using the Default Route Table</span></span>

<span data-ttu-id="13cf0-110">При создании нового приложения ASP.NET MVC, приложение уже настроено для использования маршрутизации ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="13cf0-110">When you create a new ASP.NET MVC application, the application is already configured to use ASP.NET Routing.</span></span> <span data-ttu-id="13cf0-111">В двух местах используется маршрутизации ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="13cf0-111">ASP.NET Routing is setup in two places.</span></span>

<span data-ttu-id="13cf0-112">Во-первых маршрутизация ASP.NET включена в файле конфигурации веб-приложения (файл Web.config).</span><span class="sxs-lookup"><span data-stu-id="13cf0-112">First, ASP.NET Routing is enabled in your application's Web configuration file (Web.config file).</span></span> <span data-ttu-id="13cf0-113">Есть четыре раздела в файле конфигурации, относящиеся к маршрутизации: раздел system.web.httpModules, раздел system.web.httpHandlers, system.webserver.modules раздел и раздел system.webserver.handlers.</span><span class="sxs-lookup"><span data-stu-id="13cf0-113">There are four sections in the configuration file that are relevant to routing: the system.web.httpModules section, the system.web.httpHandlers section, the system.webserver.modules section, and the system.webserver.handlers section.</span></span> <span data-ttu-id="13cf0-114">Будьте внимательны и не удалить эти разделы, поскольку без этих разделов маршрутизации больше не будет работать.</span><span class="sxs-lookup"><span data-stu-id="13cf0-114">Be careful not to delete these sections because without these sections routing will no longer work.</span></span>

<span data-ttu-id="13cf0-115">Во-вторых, что более важно таблицы маршрутов создается в файле Global.asax приложения.</span><span class="sxs-lookup"><span data-stu-id="13cf0-115">Second, and more importantly, a route table is created in the application's Global.asax file.</span></span> <span data-ttu-id="13cf0-116">Файл Global.asax является особым файлом, содержащий обработчики событий жизненного цикла приложения ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="13cf0-116">The Global.asax file is a special file that contains event handlers for ASP.NET application lifecycle events.</span></span> <span data-ttu-id="13cf0-117">Таблица маршрутов создается во время события запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="13cf0-117">The route table is created during the Application Start event.</span></span>

<span data-ttu-id="13cf0-118">Файл, в список 1 содержит файл Global.asax по умолчанию для приложения ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="13cf0-118">The file in Listing 1 contains the default Global.asax file for an ASP.NET MVC application.</span></span>

<span data-ttu-id="13cf0-119">**1 — Global.asax.cs со списком**</span><span class="sxs-lookup"><span data-stu-id="13cf0-119">**Listing 1 - Global.asax.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample1.cs)]

<span data-ttu-id="13cf0-120">Когда приложение MVC первом запуске приложения\_вызывается метод Start().</span><span class="sxs-lookup"><span data-stu-id="13cf0-120">When an MVC application first starts, the Application\_Start() method is called.</span></span> <span data-ttu-id="13cf0-121">Этот метод в свою очередь, вызывает метод RegisterRoutes().</span><span class="sxs-lookup"><span data-stu-id="13cf0-121">This method, in turn, calls the RegisterRoutes() method.</span></span> <span data-ttu-id="13cf0-122">Метод RegisterRoutes() создает таблицы маршрутов.</span><span class="sxs-lookup"><span data-stu-id="13cf0-122">The RegisterRoutes() method creates the route table.</span></span>

<span data-ttu-id="13cf0-123">Таблица маршрутов по умолчанию содержит один маршрут (с именем по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="13cf0-123">The default route table contains a single route (named Default).</span></span> <span data-ttu-id="13cf0-124">Маршрут по умолчанию сопоставляется первый сегмент URL-адреса имя контроллера, второй сегмент URL-адрес действия контроллера и третьего сегмента в параметр с именем **идентификатор**.</span><span class="sxs-lookup"><span data-stu-id="13cf0-124">The Default route maps the first segment of a URL to a controller name, the second segment of a URL to a controller action, and the third segment to a parameter named **id**.</span></span>

<span data-ttu-id="13cf0-125">Представьте, что введите следующий URL-адрес в адресной строке веб-браузера:</span><span class="sxs-lookup"><span data-stu-id="13cf0-125">Imagine that you enter the following URL into your web browser's address bar:</span></span>

<span data-ttu-id="13cf0-126">/ Home/индекс/3</span><span class="sxs-lookup"><span data-stu-id="13cf0-126">/Home/Index/3</span></span>

<span data-ttu-id="13cf0-127">Этот URL-адрес маршрута по умолчанию сопоставляется со следующими параметрами:</span><span class="sxs-lookup"><span data-stu-id="13cf0-127">The Default route maps this URL to the following parameters:</span></span>

- <span data-ttu-id="13cf0-128">контроллер = Home</span><span class="sxs-lookup"><span data-stu-id="13cf0-128">controller = Home</span></span>

- <span data-ttu-id="13cf0-129">Действие = индекс</span><span class="sxs-lookup"><span data-stu-id="13cf0-129">action = Index</span></span>

- <span data-ttu-id="13cf0-130">идентификатор = 3</span><span class="sxs-lookup"><span data-stu-id="13cf0-130">id = 3</span></span>

<span data-ttu-id="13cf0-131">При запросе URL-адрес/Home/Index/3, выполняется следующий код:</span><span class="sxs-lookup"><span data-stu-id="13cf0-131">When you request the URL /Home/Index/3, the following code is executed:</span></span>

<span data-ttu-id="13cf0-132">HomeController.Index(3)</span><span class="sxs-lookup"><span data-stu-id="13cf0-132">HomeController.Index(3)</span></span>

<span data-ttu-id="13cf0-133">Маршрут по умолчанию включаются по умолчанию для всех трех параметров.</span><span class="sxs-lookup"><span data-stu-id="13cf0-133">The Default route includes defaults for all three parameters.</span></span> <span data-ttu-id="13cf0-134">Если вы не предоставите контроллера, затем контроллера параметра по умолчанию используется значение **Главная**.</span><span class="sxs-lookup"><span data-stu-id="13cf0-134">If you don't supply a controller, then the controller parameter defaults to the value **Home**.</span></span> <span data-ttu-id="13cf0-135">Если вы не предоставите действие, параметр действия по умолчанию используется значение **индекса**.</span><span class="sxs-lookup"><span data-stu-id="13cf0-135">If you don't supply an action, the action parameter defaults to the value **Index**.</span></span> <span data-ttu-id="13cf0-136">Наконец Если не указан идентификатор, параметр id по умолчанию используется пустая строка.</span><span class="sxs-lookup"><span data-stu-id="13cf0-136">Finally, if you don't supply an id, the id parameter defaults to an empty string.</span></span>

<span data-ttu-id="13cf0-137">Рассмотрим несколько примеров маршрут по умолчанию сопоставление URL-адреса действий контроллера.</span><span class="sxs-lookup"><span data-stu-id="13cf0-137">Let's look at a few examples of how the Default route maps URLs to controller actions.</span></span> <span data-ttu-id="13cf0-138">Представьте, что введите следующий URL-адрес в адресную строку браузера:</span><span class="sxs-lookup"><span data-stu-id="13cf0-138">Imagine that you enter the following URL into your browser address bar:</span></span>

<span data-ttu-id="13cf0-139">Домашние</span><span class="sxs-lookup"><span data-stu-id="13cf0-139">/Home</span></span>

<span data-ttu-id="13cf0-140">Из-за значения по умолчанию маршрута параметр по умолчанию этот URL-адрес вызовет метод Index() HomeController класса в списке 2 для вызова.</span><span class="sxs-lookup"><span data-stu-id="13cf0-140">Because of the Default route parameter defaults, entering this URL will cause the Index() method of the HomeController class in Listing 2 to be called.</span></span>

<span data-ttu-id="13cf0-141">**Листинг 2 - HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="13cf0-141">**Listing 2 - HomeController.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample2.cs)]

<span data-ttu-id="13cf0-142">В списке 2 класс HomeController содержит метод с именем Index(), который принимает один параметр с именем идентификатора. URL-адрес/Home заставляет Index() метод может вызываться с пустой строкой в качестве значения параметра Id.</span><span class="sxs-lookup"><span data-stu-id="13cf0-142">In Listing 2, the HomeController class includes a method named Index() that accepts a single parameter named Id. The URL /Home causes the Index() method to be called with an empty string as the value of the Id parameter.</span></span>

<span data-ttu-id="13cf0-143">Из-за того, что платформа MVC вызывает действия контроллера / Home URL-адрес также соответствует метод Index() HomeController класса в списке 3.</span><span class="sxs-lookup"><span data-stu-id="13cf0-143">Because of the way that the MVC framework invokes controller actions, the URL /Home also matches the Index() method of the HomeController class in Listing 3.</span></span>

<span data-ttu-id="13cf0-144">**Листинг 3 - HomeController.cs (действие без параметра индекса)**</span><span class="sxs-lookup"><span data-stu-id="13cf0-144">**Listing 3 - HomeController.cs (Index action with no parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample3.cs)]

<span data-ttu-id="13cf0-145">Метод Index() в списке 3 не принимает параметров.</span><span class="sxs-lookup"><span data-stu-id="13cf0-145">The Index() method in Listing 3 does not accept any parameters.</span></span> <span data-ttu-id="13cf0-146">URL-адрес/Home вызовет этот метод Index() для вызова.</span><span class="sxs-lookup"><span data-stu-id="13cf0-146">The URL /Home will cause this Index() method to be called.</span></span> <span data-ttu-id="13cf0-147">URL-адрес/Home/Index/3 также вызывает этот метод, (идентификатор будет пропускаться).</span><span class="sxs-lookup"><span data-stu-id="13cf0-147">The URL /Home/Index/3 also invokes this method (the Id is ignored).</span></span>

<span data-ttu-id="13cf0-148">URL-адрес/Home также соответствует метод Index() класс HomeController в листинге 4.</span><span class="sxs-lookup"><span data-stu-id="13cf0-148">The URL /Home also matches the Index() method of the HomeController class in Listing 4.</span></span>

<span data-ttu-id="13cf0-149">**Листинг 4 - HomeController.cs (действие индекса с параметром, допускающие значение NULL)**</span><span class="sxs-lookup"><span data-stu-id="13cf0-149">**Listing 4 - HomeController.cs (Index action with nullable parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample4.cs)]

<span data-ttu-id="13cf0-150">В листинге 4 Index() метод имеет один целочисленный параметр.</span><span class="sxs-lookup"><span data-stu-id="13cf0-150">In Listing 4, the Index() method has one Integer parameter.</span></span> <span data-ttu-id="13cf0-151">Поскольку параметр является параметром nullable (может иметь значение Null), Index() может быть вызван без возникновения ошибки.</span><span class="sxs-lookup"><span data-stu-id="13cf0-151">Because the parameter is a nullable parameter (can have the value Null), the Index() can be called without raising an error.</span></span>

<span data-ttu-id="13cf0-152">Наконец, вызов метода Index() в листинге 5 с/Home URL-адрес приводит к возникновению исключения с помощью идентификатора *не* параметр допускает значение NULL.</span><span class="sxs-lookup"><span data-stu-id="13cf0-152">Finally, invoking the Index() method in Listing 5 with the URL /Home causes an exception since the Id parameter *is not* a nullable parameter.</span></span> <span data-ttu-id="13cf0-153">При попытке вызвать метод Index() получить ошибку, отображаемую на рис. 1.</span><span class="sxs-lookup"><span data-stu-id="13cf0-153">If you attempt to invoke the Index() method then you get the error displayed in Figure 1.</span></span>

<span data-ttu-id="13cf0-154">**Список 5 - HomeController.cs (действие индекса с параметром Id)**</span><span class="sxs-lookup"><span data-stu-id="13cf0-154">**Listing 5 - HomeController.cs (Index action with Id parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample5.cs)]


<span data-ttu-id="13cf0-155">[![Вызов действия контроллера, который ожидает значение параметра](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="13cf0-155">[![Invoking a controller action that expects a parameter value](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)</span></span>

<span data-ttu-id="13cf0-156">**На рисунке 01**: вызов действия контроллера, который ожидает значение параметра ([Просмотр полноразмерное изображение](asp-net-mvc-routing-overview-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="13cf0-156">**Figure 01**: Invoking a controller action that expects a parameter value ([Click to view full-size image](asp-net-mvc-routing-overview-cs/_static/image2.png))</span></span>


<span data-ttu-id="13cf0-157">С другой стороны, URL-адрес/Home/Index/3, хорошо работает с действие контроллера индекса в листинге 5.</span><span class="sxs-lookup"><span data-stu-id="13cf0-157">The URL /Home/Index/3, on the other hand, works just fine with the Index controller action in Listing 5.</span></span> <span data-ttu-id="13cf0-158">/Home/Index/3 запроса заставляет метод Index() вызывать идентификатор параметром, который имеет значение 3.</span><span class="sxs-lookup"><span data-stu-id="13cf0-158">The request /Home/Index/3 causes the Index() method to be called with an Id parameter that has the value 3.</span></span>

## <a name="summary"></a><span data-ttu-id="13cf0-159">Сводка</span><span class="sxs-lookup"><span data-stu-id="13cf0-159">Summary</span></span>

<span data-ttu-id="13cf0-160">Целью данного учебника было предоставляют краткое введение в маршрутизации ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="13cf0-160">The goal of this tutorial was to provide you with a brief introduction to ASP.NET Routing.</span></span> <span data-ttu-id="13cf0-161">Мы рассмотрели таблицы маршрутов по умолчанию, вы получаете с нового приложения ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="13cf0-161">We examined the default route table that you get with a new ASP.NET MVC application.</span></span> <span data-ttu-id="13cf0-162">Вы узнали, как маршрут по умолчанию сопоставление URL-адреса действий контроллера.</span><span class="sxs-lookup"><span data-stu-id="13cf0-162">You learned how the default route maps URLs to controller actions.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="13cf0-163">Вперед</span><span class="sxs-lookup"><span data-stu-id="13cf0-163">Next</span></span>](understanding-action-filters-cs.md)
