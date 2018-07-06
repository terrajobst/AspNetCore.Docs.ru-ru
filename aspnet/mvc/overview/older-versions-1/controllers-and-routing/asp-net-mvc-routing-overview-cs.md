---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
title: Сведения о маршрутизации ASP.NET MVC (C#) | Документация Майкрософт
author: StephenWalther
description: В этом руководстве Стивен Вальтер показано, как платформа ASP.NET MVC сопоставляет запросы браузера к действиям контроллера.
ms.author: aspnetcontent
ms.date: 08/19/2008
ms.assetid: 5b39d2d5-4bf9-4d04-94c7-81b84dfeeb31
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 2b43c3dca1587d79a60c5a89a451dda989403d56
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819151"
---
<a name="aspnet-mvc-routing-overview-c"></a><span data-ttu-id="9b672-103">Сведения о маршрутизации ASP.NET MVC (C#)</span><span class="sxs-lookup"><span data-stu-id="9b672-103">ASP.NET MVC Routing Overview (C#)</span></span>
====================
<span data-ttu-id="9b672-104">по [Стивен Вальтер](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="9b672-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="9b672-105">В этом руководстве Стивен Вальтер показано, как платформа ASP.NET MVC сопоставляет запросы браузера к действиям контроллера.</span><span class="sxs-lookup"><span data-stu-id="9b672-105">In this tutorial, Stephen Walther shows how the ASP.NET MVC framework maps browser requests to controller actions.</span></span>


<span data-ttu-id="9b672-106">В этом руководстве представлена важной особенностью каждое приложение ASP.NET MVC вызывается *маршрутизация ASP.NET*.</span><span class="sxs-lookup"><span data-stu-id="9b672-106">In this tutorial, you are introduced to an important feature of every ASP.NET MVC application called *ASP.NET Routing*.</span></span> <span data-ttu-id="9b672-107">Модуль маршрутизации ASP.NET отвечает за сопоставление входящий запрос браузера с определенного действия контроллера MVC.</span><span class="sxs-lookup"><span data-stu-id="9b672-107">The ASP.NET Routing module is responsible for mapping incoming browser requests to particular MVC controller actions.</span></span> <span data-ttu-id="9b672-108">Изучив этот учебник вы сможете понять, как в таблице Стандартная Маршрутизация сопоставляет запросы к действиям контроллера.</span><span class="sxs-lookup"><span data-stu-id="9b672-108">By the end of this tutorial, you will understand how the standard route table maps requests to controller actions.</span></span>

## <a name="using-the-default-route-table"></a><span data-ttu-id="9b672-109">С помощью таблицы маршрутов по умолчанию</span><span class="sxs-lookup"><span data-stu-id="9b672-109">Using the Default Route Table</span></span>

<span data-ttu-id="9b672-110">При создании нового приложения ASP.NET MVC, приложение уже настроен для использования маршрутизации ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9b672-110">When you create a new ASP.NET MVC application, the application is already configured to use ASP.NET Routing.</span></span> <span data-ttu-id="9b672-111">В двух местах используется маршрутизация ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9b672-111">ASP.NET Routing is setup in two places.</span></span>

<span data-ttu-id="9b672-112">Во-первых маршрутизация ASP.NET включена в файле конфигурации веб-приложения (файл Web.config).</span><span class="sxs-lookup"><span data-stu-id="9b672-112">First, ASP.NET Routing is enabled in your application's Web configuration file (Web.config file).</span></span> <span data-ttu-id="9b672-113">Существует четыре раздела в файле конфигурации, относящиеся к маршрутизации: разделе system.web.httpModules, в разделе system.web.httpHandlers, system.webserver.modules разделе и разделе system.webserver.handlers.</span><span class="sxs-lookup"><span data-stu-id="9b672-113">There are four sections in the configuration file that are relevant to routing: the system.web.httpModules section, the system.web.httpHandlers section, the system.webserver.modules section, and the system.webserver.handlers section.</span></span> <span data-ttu-id="9b672-114">Не следует удалить эти разделы, так как без этих разделов маршрутизации больше не будет работать.</span><span class="sxs-lookup"><span data-stu-id="9b672-114">Be careful not to delete these sections because without these sections routing will no longer work.</span></span>

<span data-ttu-id="9b672-115">Во-вторых и что более важно таблицы маршрутов создается в файле Global.asax приложения.</span><span class="sxs-lookup"><span data-stu-id="9b672-115">Second, and more importantly, a route table is created in the application's Global.asax file.</span></span> <span data-ttu-id="9b672-116">Файл Global.asax является особым файлом, содержит обработчики событий для события жизненного цикла приложения ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9b672-116">The Global.asax file is a special file that contains event handlers for ASP.NET application lifecycle events.</span></span> <span data-ttu-id="9b672-117">В таблице маршрутов будет создан при событии запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="9b672-117">The route table is created during the Application Start event.</span></span>

<span data-ttu-id="9b672-118">Файл в листинге 1 содержит файл Global.asax по умолчанию для приложения ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="9b672-118">The file in Listing 1 contains the default Global.asax file for an ASP.NET MVC application.</span></span>

<span data-ttu-id="9b672-119">**В листинге 1 - Global.asax.cs**</span><span class="sxs-lookup"><span data-stu-id="9b672-119">**Listing 1 - Global.asax.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample1.cs)]

<span data-ttu-id="9b672-120">Когда приложение MVC первом запуске приложения\_вызывается метод Start().</span><span class="sxs-lookup"><span data-stu-id="9b672-120">When an MVC application first starts, the Application\_Start() method is called.</span></span> <span data-ttu-id="9b672-121">Этот метод в свою очередь, вызывает метод RegisterRoutes().</span><span class="sxs-lookup"><span data-stu-id="9b672-121">This method, in turn, calls the RegisterRoutes() method.</span></span> <span data-ttu-id="9b672-122">Метод RegisterRoutes() создает таблицы маршрутов.</span><span class="sxs-lookup"><span data-stu-id="9b672-122">The RegisterRoutes() method creates the route table.</span></span>

<span data-ttu-id="9b672-123">Таблица маршрутов по умолчанию содержит один маршрут (с именем по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="9b672-123">The default route table contains a single route (named Default).</span></span> <span data-ttu-id="9b672-124">Маршрут по умолчанию сопоставляется первый сегмент URL-адреса, имя контроллера, второй сегмент URL-адреса к действию контроллера и третий сегмент в параметр с именем **идентификатор**.</span><span class="sxs-lookup"><span data-stu-id="9b672-124">The Default route maps the first segment of a URL to a controller name, the second segment of a URL to a controller action, and the third segment to a parameter named **id**.</span></span>

<span data-ttu-id="9b672-125">Представьте себе, что введите следующий URL-адрес в адресную строку веб-браузера.</span><span class="sxs-lookup"><span data-stu-id="9b672-125">Imagine that you enter the following URL into your web browser's address bar:</span></span>

<span data-ttu-id="9b672-126">/ Home/Index/3</span><span class="sxs-lookup"><span data-stu-id="9b672-126">/Home/Index/3</span></span>

<span data-ttu-id="9b672-127">Этот URL-адрес маршрута по умолчанию сопоставляется со следующими параметрами:</span><span class="sxs-lookup"><span data-stu-id="9b672-127">The Default route maps this URL to the following parameters:</span></span>

- <span data-ttu-id="9b672-128">контроллер = Home</span><span class="sxs-lookup"><span data-stu-id="9b672-128">controller = Home</span></span>

- <span data-ttu-id="9b672-129">Действие = индекс</span><span class="sxs-lookup"><span data-stu-id="9b672-129">action = Index</span></span>

- <span data-ttu-id="9b672-130">идентификатор = 3</span><span class="sxs-lookup"><span data-stu-id="9b672-130">id = 3</span></span>

<span data-ttu-id="9b672-131">При запросе URL-адрес/Home/Index/3, выполняется следующий код:</span><span class="sxs-lookup"><span data-stu-id="9b672-131">When you request the URL /Home/Index/3, the following code is executed:</span></span>

<span data-ttu-id="9b672-132">HomeController.Index(3)</span><span class="sxs-lookup"><span data-stu-id="9b672-132">HomeController.Index(3)</span></span>

<span data-ttu-id="9b672-133">Маршрут по умолчанию включает в себя значения по умолчанию для всех трех параметров.</span><span class="sxs-lookup"><span data-stu-id="9b672-133">The Default route includes defaults for all three parameters.</span></span> <span data-ttu-id="9b672-134">Если вы не указали контроллера, затем параметр контроллера по умолчанию значение **Главная**.</span><span class="sxs-lookup"><span data-stu-id="9b672-134">If you don't supply a controller, then the controller parameter defaults to the value **Home**.</span></span> <span data-ttu-id="9b672-135">Если вы не указали действие, параметр действия по умолчанию используется значение **индекс**.</span><span class="sxs-lookup"><span data-stu-id="9b672-135">If you don't supply an action, the action parameter defaults to the value **Index**.</span></span> <span data-ttu-id="9b672-136">И, наконец Если не указан идентификатор, идентификатор по умолчанию: пустая строка.</span><span class="sxs-lookup"><span data-stu-id="9b672-136">Finally, if you don't supply an id, the id parameter defaults to an empty string.</span></span>

<span data-ttu-id="9b672-137">Давайте рассмотрим несколько примеров как маршрут по умолчанию сопоставляет URL-адреса к действиям контроллера.</span><span class="sxs-lookup"><span data-stu-id="9b672-137">Let's look at a few examples of how the Default route maps URLs to controller actions.</span></span> <span data-ttu-id="9b672-138">Представьте себе, что введите следующий URL-адрес в адресной строке браузера.</span><span class="sxs-lookup"><span data-stu-id="9b672-138">Imagine that you enter the following URL into your browser address bar:</span></span>

<span data-ttu-id="9b672-139">/ Home</span><span class="sxs-lookup"><span data-stu-id="9b672-139">/Home</span></span>

<span data-ttu-id="9b672-140">Из-за умолчанию маршрут по умолчанию введя этот URL-адрес вызовет Index() метод в класс HomeController в листинге 2 для вызова.</span><span class="sxs-lookup"><span data-stu-id="9b672-140">Because of the Default route parameter defaults, entering this URL will cause the Index() method of the HomeController class in Listing 2 to be called.</span></span>

<span data-ttu-id="9b672-141">**В листинге 2 - HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="9b672-141">**Listing 2 - HomeController.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample2.cs)]

<span data-ttu-id="9b672-142">В листинге 2 в класс HomeController содержит метод с именем Index(), который принимает один параметр с именем Id. URL-адрес/Home вызывает метод Index(), который вызывается с пустой строкой в качестве значения параметра Id.</span><span class="sxs-lookup"><span data-stu-id="9b672-142">In Listing 2, the HomeController class includes a method named Index() that accepts a single parameter named Id. The URL /Home causes the Index() method to be called with an empty string as the value of the Id parameter.</span></span>

<span data-ttu-id="9b672-143">Из-за того, что платформа MVC вызывает действий контроллера / Home URL-адрес также соответствует Index() метод в класс HomeController в листинге 3.</span><span class="sxs-lookup"><span data-stu-id="9b672-143">Because of the way that the MVC framework invokes controller actions, the URL /Home also matches the Index() method of the HomeController class in Listing 3.</span></span>

<span data-ttu-id="9b672-144">**Листинг 3 - HomeController.cs (действие индекса без параметра)**</span><span class="sxs-lookup"><span data-stu-id="9b672-144">**Listing 3 - HomeController.cs (Index action with no parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample3.cs)]

<span data-ttu-id="9b672-145">Метод Index() в листинге 3 не принимает параметров.</span><span class="sxs-lookup"><span data-stu-id="9b672-145">The Index() method in Listing 3 does not accept any parameters.</span></span> <span data-ttu-id="9b672-146">URL-адрес/Home вызовет этот метод Index() для вызова.</span><span class="sxs-lookup"><span data-stu-id="9b672-146">The URL /Home will cause this Index() method to be called.</span></span> <span data-ttu-id="9b672-147">URL-адрес/Home/Index/3 также вызывает этот метод (идентификатор будет пропускаться).</span><span class="sxs-lookup"><span data-stu-id="9b672-147">The URL /Home/Index/3 also invokes this method (the Id is ignored).</span></span>

<span data-ttu-id="9b672-148">URL-адрес/Home также соответствует Index() метод в класс HomeController в листинге 4.</span><span class="sxs-lookup"><span data-stu-id="9b672-148">The URL /Home also matches the Index() method of the HomeController class in Listing 4.</span></span>

<span data-ttu-id="9b672-149">**Листинг 4 - HomeController.cs (действие Index с параметром, допускающий значение NULL)**</span><span class="sxs-lookup"><span data-stu-id="9b672-149">**Listing 4 - HomeController.cs (Index action with nullable parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample4.cs)]

<span data-ttu-id="9b672-150">В листинге 4 Index() метод имеет один параметр целое число.</span><span class="sxs-lookup"><span data-stu-id="9b672-150">In Listing 4, the Index() method has one Integer parameter.</span></span> <span data-ttu-id="9b672-151">Поскольку параметр является параметром допускает значения NULL (может иметь значение Null), Index() может быть вызван без возникновения ошибки.</span><span class="sxs-lookup"><span data-stu-id="9b672-151">Because the parameter is a nullable parameter (can have the value Null), the Index() can be called without raising an error.</span></span>

<span data-ttu-id="9b672-152">Наконец, вызова метода Index() в листинге 5 с/Home URL-адрес приводит к возникновению исключения, так как параметр Id *не* допускающего значение null параметра.</span><span class="sxs-lookup"><span data-stu-id="9b672-152">Finally, invoking the Index() method in Listing 5 with the URL /Home causes an exception since the Id parameter *is not* a nullable parameter.</span></span> <span data-ttu-id="9b672-153">При попытке вызвать метод Index() вы получите сообщение об ошибке на рис. 1.</span><span class="sxs-lookup"><span data-stu-id="9b672-153">If you attempt to invoke the Index() method then you get the error displayed in Figure 1.</span></span>

<span data-ttu-id="9b672-154">**В листинге 5 - HomeController.cs (действие Index с параметром Id)**</span><span class="sxs-lookup"><span data-stu-id="9b672-154">**Listing 5 - HomeController.cs (Index action with Id parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample5.cs)]


<span data-ttu-id="9b672-155">[![Вызов действия контроллера, который ожидает, что значение параметра](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9b672-155">[![Invoking a controller action that expects a parameter value](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)</span></span>

<span data-ttu-id="9b672-156">**Рис 01**: вызов действия контроллера, который ожидает, что значение параметра ([Просмотр полноразмерного изображения](asp-net-mvc-routing-overview-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="9b672-156">**Figure 01**: Invoking a controller action that expects a parameter value ([Click to view full-size image](asp-net-mvc-routing-overview-cs/_static/image2.png))</span></span>


<span data-ttu-id="9b672-157">С другой стороны, URL-адрес/Home/Index/3, хорошо работает с действие контроллера индекса в листинге 5.</span><span class="sxs-lookup"><span data-stu-id="9b672-157">The URL /Home/Index/3, on the other hand, works just fine with the Index controller action in Listing 5.</span></span> <span data-ttu-id="9b672-158">Запрос /Home/Index/3 вызывает метод Index() может вызываться с параметром идентификатор, который имеет значение 3.</span><span class="sxs-lookup"><span data-stu-id="9b672-158">The request /Home/Index/3 causes the Index() method to be called with an Id parameter that has the value 3.</span></span>

## <a name="summary"></a><span data-ttu-id="9b672-159">Сводка</span><span class="sxs-lookup"><span data-stu-id="9b672-159">Summary</span></span>

<span data-ttu-id="9b672-160">Целью данного учебника мы хотели предоставить Краткое введение в маршрутизации ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9b672-160">The goal of this tutorial was to provide you with a brief introduction to ASP.NET Routing.</span></span> <span data-ttu-id="9b672-161">Мы рассмотрели таблицы маршрутизации по умолчанию, которую можно получить с помощью нового приложения ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="9b672-161">We examined the default route table that you get with a new ASP.NET MVC application.</span></span> <span data-ttu-id="9b672-162">Вы узнали, как маршрут по умолчанию сопоставляет URL-адреса к действиям контроллера.</span><span class="sxs-lookup"><span data-stu-id="9b672-162">You learned how the default route maps URLs to controller actions.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9b672-163">Вперед</span><span class="sxs-lookup"><span data-stu-id="9b672-163">Next</span></span>](understanding-action-filters-cs.md)
