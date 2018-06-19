---
uid: mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
title: Общие сведения о ASP.NET MVC контроллера (C#) | Документы Microsoft
author: StephenWalther
description: В этом учебнике Стивен Вальтер представлены контроллеров ASP.NET MVC. Вы узнаете, как для создания новых контроллеров и возвращать различные типы действий res...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: b985c49a-3668-455c-a366-f85f6bc64b12
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 95e7c555a52c8c3b765a6fffab15276491cf5714
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869092"
---
<a name="aspnet-mvc-controller-overview-c"></a><span data-ttu-id="60b67-104">Общие сведения о ASP.NET MVC контроллера (C#)</span><span class="sxs-lookup"><span data-stu-id="60b67-104">ASP.NET MVC Controller Overview (C#)</span></span>
====================
<span data-ttu-id="60b67-105">по [Стивен Вальтер](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="60b67-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="60b67-106">В этом учебнике Стивен Вальтер представлены контроллеров ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="60b67-106">In this tutorial, Stephen Walther introduces you to ASP.NET MVC controllers.</span></span> <span data-ttu-id="60b67-107">Вы узнаете, как для создания новых контроллеров и возвращать различные типы результатов действий.</span><span class="sxs-lookup"><span data-stu-id="60b67-107">You learn how to create new controllers and return different types of action results.</span></span>


<span data-ttu-id="60b67-108">Этот учебник изучает темы ASP.NET MVC контроллеры действия контроллера и результаты действий.</span><span class="sxs-lookup"><span data-stu-id="60b67-108">This tutorial explores the topic of ASP.NET MVC controllers, controller actions, and action results.</span></span> <span data-ttu-id="60b67-109">После завершения этого учебника вы сможете понять, как контроллеры используются для управления способом, когда посетитель взаимодействует с веб-сайта ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="60b67-109">After you complete this tutorial, you will understand how controllers are used to control the way a visitor interacts with an ASP.NET MVC website.</span></span>

## <a name="understanding-controllers"></a><span data-ttu-id="60b67-110">Общее представление о контроллерах</span><span class="sxs-lookup"><span data-stu-id="60b67-110">Understanding Controllers</span></span>

<span data-ttu-id="60b67-111">Контроллеров MVC отвечают за отвечает на запросы, адресованные на веб-сайт ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="60b67-111">MVC controllers are responsible for responding to requests made against an ASP.NET MVC website.</span></span> <span data-ttu-id="60b67-112">Каждый запрос браузера сопоставляется конкретном контроллере.</span><span class="sxs-lookup"><span data-stu-id="60b67-112">Each browser request is mapped to a particular controller.</span></span> <span data-ttu-id="60b67-113">Например предположим, что введите следующий URL-адрес в адресную строку браузера:</span><span class="sxs-lookup"><span data-stu-id="60b67-113">For example, imagine that you enter the following URL into the address bar of your browser:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="60b67-114">В этом случае вызывается контроллер с именем ProductController.</span><span class="sxs-lookup"><span data-stu-id="60b67-114">In this case, a controller named ProductController is invoked.</span></span> <span data-ttu-id="60b67-115">ProductController отвечает за создание ответа на запрос браузера.</span><span class="sxs-lookup"><span data-stu-id="60b67-115">The ProductController is responsible for generating the response to the browser request.</span></span> <span data-ttu-id="60b67-116">Например контроллер может вернуть конкретное представление браузера или контроллер может перенаправлять пользователя на другом контроллере.</span><span class="sxs-lookup"><span data-stu-id="60b67-116">For example, the controller might return a particular view back to the browser or the controller might redirect the user to another controller.</span></span>

<span data-ttu-id="60b67-117">Листинг 1 содержит простой контроллер с именем ProductController.</span><span class="sxs-lookup"><span data-stu-id="60b67-117">Listing 1 contains a simple controller named ProductController.</span></span>

<span data-ttu-id="60b67-118">**Listing1 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="60b67-118">**Listing1 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample1.cs)]

<span data-ttu-id="60b67-119">Как видно из список 1 контроллера — это класс (класс Visual Basic .NET или C#).</span><span class="sxs-lookup"><span data-stu-id="60b67-119">As you can see from Listing 1, a controller is just a class (a Visual Basic .NET or C# class).</span></span> <span data-ttu-id="60b67-120">Контроллер — класс, производный от базового класса System.Web.Mvc.Controller.</span><span class="sxs-lookup"><span data-stu-id="60b67-120">A controller is a class that derives from the base System.Web.Mvc.Controller class.</span></span> <span data-ttu-id="60b67-121">Поскольку контроллер наследует от этого базового класса, контроллер наследует несколько полезных методов для получения бесплатной (мы обсудим эти методы чуть позже).</span><span class="sxs-lookup"><span data-stu-id="60b67-121">Because a controller inherits from this base class, a controller inherits several useful methods for free (We discuss these methods in a moment).</span></span>

## <a name="understanding-controller-actions"></a><span data-ttu-id="60b67-122">Основные сведения о действиях контроллера</span><span class="sxs-lookup"><span data-stu-id="60b67-122">Understanding Controller Actions</span></span>

<span data-ttu-id="60b67-123">Контроллер предоставляет действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="60b67-123">A controller exposes controller actions.</span></span> <span data-ttu-id="60b67-124">Действие — это метод на контроллере, который вызывается при вводе определенный URL-адрес в адресную строку браузера.</span><span class="sxs-lookup"><span data-stu-id="60b67-124">An action is a method on a controller that gets called when you enter a particular URL in your browser address bar.</span></span> <span data-ttu-id="60b67-125">Например предположим, сделать запрос для следующего URL:</span><span class="sxs-lookup"><span data-stu-id="60b67-125">For example, imagine that you make a request for the following URL:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="60b67-126">В этом случае метод Index() вызывается в классе ProductController.</span><span class="sxs-lookup"><span data-stu-id="60b67-126">In this case, the Index() method is called on the ProductController class.</span></span> <span data-ttu-id="60b67-127">Метод Index() приведен пример действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="60b67-127">The Index() method is an example of a controller action.</span></span>

<span data-ttu-id="60b67-128">Действие контроллера должен быть открытый метод класса контроллера.</span><span class="sxs-lookup"><span data-stu-id="60b67-128">A controller action must be a public method of a controller class.</span></span> <span data-ttu-id="60b67-129">Методы C#, по умолчанию являются закрытые методы.</span><span class="sxs-lookup"><span data-stu-id="60b67-129">C# methods, by default, are private methods.</span></span> <span data-ttu-id="60b67-130">Учтите, что любой открытый метод, который можно добавить в класс контроллера предоставляется как действия контроллера, автоматически (необходимо быть осторожным это так, как действие контроллера может вызываться кем-либо во вселенной просто введя правой URL-адрес в адресную строку браузера).</span><span class="sxs-lookup"><span data-stu-id="60b67-130">Realize that any public method that you add to a controller class is exposed as a controller action automatically (You must be careful about this since a controller action can be invoked by anyone in the universe simply by typing the right URL into a browser address bar).</span></span>

<span data-ttu-id="60b67-131">Существуют некоторые дополнительные требования, которые должны быть удовлетворены с помощью действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="60b67-131">There are some additional requirements that must be satisfied by a controller action.</span></span> <span data-ttu-id="60b67-132">Метод, который используется в качестве действия контроллера, не могут быть перегружены.</span><span class="sxs-lookup"><span data-stu-id="60b67-132">A method used as a controller action cannot be overloaded.</span></span> <span data-ttu-id="60b67-133">Кроме того действия контроллера не может быть статическим методом.</span><span class="sxs-lookup"><span data-stu-id="60b67-133">Furthermore, a controller action cannot be a static method.</span></span> <span data-ttu-id="60b67-134">За исключением этого можно использовать практически любой метод, как действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="60b67-134">Other than that, you can use just about any method as a controller action.</span></span>

## <a name="understanding-action-results"></a><span data-ttu-id="60b67-135">Основные сведения о результатах действия</span><span class="sxs-lookup"><span data-stu-id="60b67-135">Understanding Action Results</span></span>

<span data-ttu-id="60b67-136">Возвращает действие контроллера называемое *результат действия*.</span><span class="sxs-lookup"><span data-stu-id="60b67-136">A controller action returns something called an *action result*.</span></span> <span data-ttu-id="60b67-137">Результат действия — возвращает в ответ на запрос браузера действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="60b67-137">An action result is what a controller action returns in response to a browser request.</span></span>

<span data-ttu-id="60b67-138">Платформа ASP.NET MVC поддерживает несколько типов результатов действий, в том числе:</span><span class="sxs-lookup"><span data-stu-id="60b67-138">The ASP.NET MVC framework supports several types of action results including:</span></span>

1. <span data-ttu-id="60b67-139">ViewResult - представляет HTML и разметки.</span><span class="sxs-lookup"><span data-stu-id="60b67-139">ViewResult - Represents HTML and markup.</span></span>
2. <span data-ttu-id="60b67-140">EmptyResult - представляет никакого результата.</span><span class="sxs-lookup"><span data-stu-id="60b67-140">EmptyResult - Represents no result.</span></span>
3. <span data-ttu-id="60b67-141">RedirectResult - представляет перенаправление на новый URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="60b67-141">RedirectResult - Represents a redirection to a new URL.</span></span>
4. <span data-ttu-id="60b67-142">JsonResult - представляет результат нотации объектов JavaScript, который может использоваться в приложениях AJAX.</span><span class="sxs-lookup"><span data-stu-id="60b67-142">JsonResult - Represents a JavaScript Object Notation result that can be used in an AJAX application.</span></span>
5. <span data-ttu-id="60b67-143">JavaScriptResult - представляет сценарий JavaScript.</span><span class="sxs-lookup"><span data-stu-id="60b67-143">JavaScriptResult - Represents a JavaScript script.</span></span>
6. <span data-ttu-id="60b67-144">ContentResult - представляет результат текста.</span><span class="sxs-lookup"><span data-stu-id="60b67-144">ContentResult - Represents a text result.</span></span>
7. <span data-ttu-id="60b67-145">FileContentResult - представляет загружаемый файл (с двоичное содержимое).</span><span class="sxs-lookup"><span data-stu-id="60b67-145">FileContentResult - Represents a downloadable file (with the binary content).</span></span>
8. <span data-ttu-id="60b67-146">FilePathResult - представляет загружаемый файл (с путем).</span><span class="sxs-lookup"><span data-stu-id="60b67-146">FilePathResult - Represents a downloadable file (with a path).</span></span>
9. <span data-ttu-id="60b67-147">FileStreamResult - представляет загружаемый файл (с файлового потока).</span><span class="sxs-lookup"><span data-stu-id="60b67-147">FileStreamResult - Represents a downloadable file (with a file stream).</span></span>

<span data-ttu-id="60b67-148">Результаты этих действий наследуются от базового класса ActionResult.</span><span class="sxs-lookup"><span data-stu-id="60b67-148">All of these action results inherit from the base ActionResult class.</span></span>

<span data-ttu-id="60b67-149">В большинстве случаев действия контроллера возвращает ViewResult.</span><span class="sxs-lookup"><span data-stu-id="60b67-149">In most cases, a controller action returns a ViewResult.</span></span> <span data-ttu-id="60b67-150">Например действие контроллера индекса в списке 2 Возвращает ViewResult.</span><span class="sxs-lookup"><span data-stu-id="60b67-150">For example, the Index controller action in Listing 2 returns a ViewResult.</span></span>

<span data-ttu-id="60b67-151">**Листинг 2 - Controllers\BookController.cs**</span><span class="sxs-lookup"><span data-stu-id="60b67-151">**Listing 2 - Controllers\BookController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample2.cs)]

<span data-ttu-id="60b67-152">Если действие возвращает ViewResult, HTML возвращается в браузере.</span><span class="sxs-lookup"><span data-stu-id="60b67-152">When an action returns a ViewResult, HTML is returned to the browser.</span></span> <span data-ttu-id="60b67-153">Метод Index() в списке 2 Возвращает представление с именем Index в браузере.</span><span class="sxs-lookup"><span data-stu-id="60b67-153">The Index() method in Listing 2 returns a view named Index to the browser.</span></span>

<span data-ttu-id="60b67-154">Обратите внимание, что Index() действие в списке 2 Возвращает ViewResult().</span><span class="sxs-lookup"><span data-stu-id="60b67-154">Notice that the Index() action in Listing 2 does not return a ViewResult().</span></span> <span data-ttu-id="60b67-155">Вместо этого вызывается метод View() базового класса контроллера.</span><span class="sxs-lookup"><span data-stu-id="60b67-155">Instead, the View() method of the Controller base class is called.</span></span> <span data-ttu-id="60b67-156">Как правило вы не возвращают результат действия непосредственно.</span><span class="sxs-lookup"><span data-stu-id="60b67-156">Normally, you do not return an action result directly.</span></span> <span data-ttu-id="60b67-157">Вместо этого вызовите один из следующих методов базового класса контроллера.</span><span class="sxs-lookup"><span data-stu-id="60b67-157">Instead, you call one of the following methods of the Controller base class:</span></span>

1. <span data-ttu-id="60b67-158">Просмотр - возвращает результат действия ViewResult.</span><span class="sxs-lookup"><span data-stu-id="60b67-158">View - Returns a ViewResult action result.</span></span>
2. <span data-ttu-id="60b67-159">Перенаправить — возвращает результат действия RedirectResult.</span><span class="sxs-lookup"><span data-stu-id="60b67-159">Redirect - Returns a RedirectResult action result.</span></span>
3. <span data-ttu-id="60b67-160">RedirectToAction - возвращает результат действия RedirectToRouteResult.</span><span class="sxs-lookup"><span data-stu-id="60b67-160">RedirectToAction - Returns a RedirectToRouteResult action result.</span></span>
4. <span data-ttu-id="60b67-161">RedirectToRoute - возвращает результат действия RedirectToRouteResult.</span><span class="sxs-lookup"><span data-stu-id="60b67-161">RedirectToRoute - Returns a RedirectToRouteResult action result.</span></span>
5. <span data-ttu-id="60b67-162">JSON - возвращает результат действия JsonResult.</span><span class="sxs-lookup"><span data-stu-id="60b67-162">Json - Returns a JsonResult action result.</span></span>
6. <span data-ttu-id="60b67-163">JavaScriptResult - возвращает JavaScriptResult.</span><span class="sxs-lookup"><span data-stu-id="60b67-163">JavaScriptResult - Returns a JavaScriptResult.</span></span>
7. <span data-ttu-id="60b67-164">Содержания - возвращает результат действия ContentResult.</span><span class="sxs-lookup"><span data-stu-id="60b67-164">Content - Returns a ContentResult action result.</span></span>
8. <span data-ttu-id="60b67-165">Файл - возвращает FileContentResult, FilePathResult или FileStreamResult в зависимости от параметров, переданного методу.</span><span class="sxs-lookup"><span data-stu-id="60b67-165">File - Returns a FileContentResult, FilePathResult, or FileStreamResult depending on the parameters passed to the method.</span></span>

<span data-ttu-id="60b67-166">Таким образом Если требуется возвратить представление в браузере, вызовите метод View().</span><span class="sxs-lookup"><span data-stu-id="60b67-166">So, if you want to return a View to the browser, you call the View() method.</span></span> <span data-ttu-id="60b67-167">Если вы хотите перенаправить пользователя из одного контроллера действия к другому, можно вызвать метод RedirectToAction().</span><span class="sxs-lookup"><span data-stu-id="60b67-167">If you want to redirect the user from one controller action to another, you call the RedirectToAction() method.</span></span> <span data-ttu-id="60b67-168">Например действие Details() в списке 3 отображает представление либо перенаправляет пользователя на Index() действие в зависимости от того, является ли параметр Id имеет значение.</span><span class="sxs-lookup"><span data-stu-id="60b67-168">For example, the Details() action in Listing 3 either displays a view or redirects the user to the Index() action depending on whether the Id parameter has a value.</span></span>

<span data-ttu-id="60b67-169">**Листинг 3 - CustomerController.cs**</span><span class="sxs-lookup"><span data-stu-id="60b67-169">**Listing 3 - CustomerController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample3.cs)]

<span data-ttu-id="60b67-170">Результат действия ContentResult специальные.</span><span class="sxs-lookup"><span data-stu-id="60b67-170">The ContentResult action result is special.</span></span> <span data-ttu-id="60b67-171">Результат действия ContentResult можно использовать для возвращения результата действия в виде обычного текста.</span><span class="sxs-lookup"><span data-stu-id="60b67-171">You can use the ContentResult action result to return an action result as plain text.</span></span> <span data-ttu-id="60b67-172">Например метод Index() в листинге 4 возвращает сообщение, как обычный текст, а не как HTML.</span><span class="sxs-lookup"><span data-stu-id="60b67-172">For example, the Index() method in Listing 4 returns a message as plain text and not as HTML.</span></span>

<span data-ttu-id="60b67-173">**Листинг 4 - Controllers\StatusController.cs**</span><span class="sxs-lookup"><span data-stu-id="60b67-173">**Listing 4 - Controllers\StatusController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample4.cs)]

<span data-ttu-id="60b67-174">При вызове действия StatusController.Index() представления не возвращается.</span><span class="sxs-lookup"><span data-stu-id="60b67-174">When the StatusController.Index() action is invoked, a view is not returned.</span></span> <span data-ttu-id="60b67-175">Вместо этого необработанный текст «Hello World!»</span><span class="sxs-lookup"><span data-stu-id="60b67-175">Instead, the raw text "Hello World!"</span></span> <span data-ttu-id="60b67-176">возвращается в браузере.</span><span class="sxs-lookup"><span data-stu-id="60b67-176">is returned to the browser.</span></span>

<span data-ttu-id="60b67-177">Если действие контроллера возвращает результат, не результат действия — например, даты или целое число — затем результат заключается в ContentResult автоматически.</span><span class="sxs-lookup"><span data-stu-id="60b67-177">If a controller action returns a result that is not an action result - for example, a date or an integer - then the result is wrapped in a ContentResult automatically.</span></span> <span data-ttu-id="60b67-178">Например при вызове действия Index() WorkController в листинге 5 даты возвращается в виде ContentResult автоматически.</span><span class="sxs-lookup"><span data-stu-id="60b67-178">For example, when the Index() action of the WorkController in Listing 5 is invoked, the date is returned as a ContentResult automatically.</span></span>

<span data-ttu-id="60b67-179">**Список 5 - WorkController.cs**</span><span class="sxs-lookup"><span data-stu-id="60b67-179">**Listing 5 - WorkController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample5.cs)]

<span data-ttu-id="60b67-180">Действие Index() в листинге 5 возвращает объект DateTime.</span><span class="sxs-lookup"><span data-stu-id="60b67-180">The Index() action in Listing 5 returns a DateTime object.</span></span> <span data-ttu-id="60b67-181">Платформа ASP.NET MVC преобразует объект DateTime в строку и создает оболочку значения даты и времени в ContentResult автоматически.</span><span class="sxs-lookup"><span data-stu-id="60b67-181">The ASP.NET MVC framework converts the DateTime object to a string and wraps the DateTime value in a ContentResult automatically.</span></span> <span data-ttu-id="60b67-182">Обозреватель получает дату и время в виде обычного текста.</span><span class="sxs-lookup"><span data-stu-id="60b67-182">The browser receives the date and time as plain text.</span></span>

## <a name="summary"></a><span data-ttu-id="60b67-183">Сводка</span><span class="sxs-lookup"><span data-stu-id="60b67-183">Summary</span></span>

<span data-ttu-id="60b67-184">Цель этого учебника было для ознакомления с основными понятиями контроллеров ASP.NET MVC, действия контроллера и результаты действий контроллера.</span><span class="sxs-lookup"><span data-stu-id="60b67-184">The purpose of this tutorial was to introduce you to the concepts of ASP.NET MVC controllers, controller actions, and controller action results.</span></span> <span data-ttu-id="60b67-185">В первом разделе вы узнали, как добавление новых контроллеров в проект ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="60b67-185">In the first section, you learned how to add new controllers to an ASP.NET MVC project.</span></span> <span data-ttu-id="60b67-186">Далее вы узнали, как открытые методы контроллера доступны вселенной действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="60b67-186">Next, you learned how public methods of a controller are exposed to the universe as controller actions.</span></span> <span data-ttu-id="60b67-187">Наконец мы рассмотрели различные типы результатов действий, которые могут быть возвращены из действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="60b67-187">Finally, we discussed the different types of action results that can be returned from a controller action.</span></span> <span data-ttu-id="60b67-188">В частности мы рассмотрели, как получить ViewResult, RedirectToActionResult и ContentResult из действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="60b67-188">In particular, we discussed how to return a ViewResult, RedirectToActionResult, and ContentResult from a controller action.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="60b67-189">[Назад](creating-an-action-vb.md)
> [Вперед](creating-custom-routes-cs.md)</span><span class="sxs-lookup"><span data-stu-id="60b67-189">[Previous](creating-an-action-vb.md)
[Next](creating-custom-routes-cs.md)</span></span>
