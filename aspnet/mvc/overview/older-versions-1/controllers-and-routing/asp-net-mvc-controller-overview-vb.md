---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
title: "Общие сведения о ASP.NET MVC контроллера (VB) | Документы Microsoft"
author: StephenWalther
description: "В этом учебнике Стивен Вальтер представлены контроллеров ASP.NET MVC. Вы узнаете, как для создания новых контроллеров и возвращать различные типы действий res..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: 94c3e5d9-a904-445e-a34e-d92fd1ca108a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: 452e2cf771e8b1f298ce9693ec2a707e7c1d4dd1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-controller-overview-vb"></a><span data-ttu-id="726bd-104">Общие сведения о ASP.NET MVC контроллера (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="726bd-104">ASP.NET MVC Controller Overview (VB)</span></span>
====================
<span data-ttu-id="726bd-105">по [Стивен Вальтер](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="726bd-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="726bd-106">В этом учебнике Стивен Вальтер представлены контроллеров ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="726bd-106">In this tutorial, Stephen Walther introduces you to ASP.NET MVC controllers.</span></span> <span data-ttu-id="726bd-107">Вы узнаете, как для создания новых контроллеров и возвращать различные типы результатов действий.</span><span class="sxs-lookup"><span data-stu-id="726bd-107">You learn how to create new controllers and return different types of action results.</span></span>


<span data-ttu-id="726bd-108">Этот учебник изучает темы ASP.NET MVC контроллеры действия контроллера и результаты действий.</span><span class="sxs-lookup"><span data-stu-id="726bd-108">This tutorial explores the topic of ASP.NET MVC controllers, controller actions, and action results.</span></span> <span data-ttu-id="726bd-109">После завершения этого учебника вы сможете понять, как контроллеры используются для управления способом, когда посетитель взаимодействует с веб-сайта ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="726bd-109">After you complete this tutorial, you will understand how controllers are used to control the way a visitor interacts with an ASP.NET MVC website.</span></span>

## <a name="understanding-controllers"></a><span data-ttu-id="726bd-110">Общее представление о контроллерах</span><span class="sxs-lookup"><span data-stu-id="726bd-110">Understanding Controllers</span></span>

<span data-ttu-id="726bd-111">Контроллеров MVC отвечают за отвечает на запросы, адресованные на веб-сайт ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="726bd-111">MVC controllers are responsible for responding to requests made against an ASP.NET MVC website.</span></span> <span data-ttu-id="726bd-112">Каждый запрос браузера сопоставляется конкретном контроллере.</span><span class="sxs-lookup"><span data-stu-id="726bd-112">Each browser request is mapped to a particular controller.</span></span> <span data-ttu-id="726bd-113">Например предположим, что введите следующий URL-адрес в адресную строку браузера:</span><span class="sxs-lookup"><span data-stu-id="726bd-113">For example, imagine that you enter the following URL into the address bar of your browser:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="726bd-114">В этом случае вызывается контроллер с именем ProductController.</span><span class="sxs-lookup"><span data-stu-id="726bd-114">In this case, a controller named ProductController is invoked.</span></span> <span data-ttu-id="726bd-115">ProductController отвечает за создание ответа на запрос браузера.</span><span class="sxs-lookup"><span data-stu-id="726bd-115">The ProductController is responsible for generating the response to the browser request.</span></span> <span data-ttu-id="726bd-116">Например контроллер может вернуть конкретное представление браузера или контроллер может перенаправлять пользователя на другом контроллере.</span><span class="sxs-lookup"><span data-stu-id="726bd-116">For example, the controller might return a particular view back to the browser or the controller might redirect the user to another controller.</span></span>

<span data-ttu-id="726bd-117">Листинг 1 содержит простой контроллер с именем ProductController.</span><span class="sxs-lookup"><span data-stu-id="726bd-117">Listing 1 contains a simple controller named ProductController.</span></span>

<span data-ttu-id="726bd-118">**Listing1 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="726bd-118">**Listing1 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample1.vb)]

<span data-ttu-id="726bd-119">Как видно из список 1 контроллера — это класс (класс Visual Basic .NET или C#).</span><span class="sxs-lookup"><span data-stu-id="726bd-119">As you can see from Listing 1, a controller is just a class (a Visual Basic .NET or C# class).</span></span> <span data-ttu-id="726bd-120">Контроллер — класс, производный от базового класса System.Web.Mvc.Controller.</span><span class="sxs-lookup"><span data-stu-id="726bd-120">A controller is a class that derives from the base System.Web.Mvc.Controller class.</span></span> <span data-ttu-id="726bd-121">Поскольку контроллер наследует от этого базового класса, контроллер наследует несколько полезных методов для получения бесплатной (мы обсудим эти методы чуть позже).</span><span class="sxs-lookup"><span data-stu-id="726bd-121">Because a controller inherits from this base class, a controller inherits several useful methods for free (We discuss these methods in a moment).</span></span>

## <a name="understanding-controller-actions"></a><span data-ttu-id="726bd-122">Основные сведения о действиях контроллера</span><span class="sxs-lookup"><span data-stu-id="726bd-122">Understanding Controller Actions</span></span>

<span data-ttu-id="726bd-123">Контроллер предоставляет действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="726bd-123">A controller exposes controller actions.</span></span> <span data-ttu-id="726bd-124">Действие — это метод на контроллере, который вызывается при вводе определенный URL-адрес в адресную строку браузера.</span><span class="sxs-lookup"><span data-stu-id="726bd-124">An action is a method on a controller that gets called when you enter a particular URL in your browser address bar.</span></span> <span data-ttu-id="726bd-125">Например предположим, сделать запрос для следующего URL:</span><span class="sxs-lookup"><span data-stu-id="726bd-125">For example, imagine that you make a request for the following URL:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="726bd-126">В этом случае метод Index() вызывается в классе ProductController.</span><span class="sxs-lookup"><span data-stu-id="726bd-126">In this case, the Index() method is called on the ProductController class.</span></span> <span data-ttu-id="726bd-127">Метод Index() приведен пример действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="726bd-127">The Index() method is an example of a controller action.</span></span>

<span data-ttu-id="726bd-128">Действие контроллера должен быть открытый метод класса контроллера.</span><span class="sxs-lookup"><span data-stu-id="726bd-128">A controller action must be a public method of a controller class.</span></span> <span data-ttu-id="726bd-129">Visual Basic.NET методы, по умолчанию являются открытыми методами.</span><span class="sxs-lookup"><span data-stu-id="726bd-129">Visual Basic.NET methods, by default, are public methods.</span></span> <span data-ttu-id="726bd-130">Учтите, что любой открытый метод, который можно добавить в класс контроллера предоставляется как действия контроллера, автоматически (необходимо быть осторожным это так, как действие контроллера может вызываться кем-либо во вселенной просто введя правой URL-адрес в адресную строку браузера).</span><span class="sxs-lookup"><span data-stu-id="726bd-130">Realize that any public method that you add to a controller class is exposed as a controller action automatically (You must be careful about this since a controller action can be invoked by anyone in the universe simply by typing the right URL into a browser address bar).</span></span>

<span data-ttu-id="726bd-131">Существуют некоторые дополнительные требования, которые должны быть удовлетворены с помощью действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="726bd-131">There are some additional requirements that must be satisfied by a controller action.</span></span> <span data-ttu-id="726bd-132">Метод, который используется в качестве действия контроллера, не могут быть перегружены.</span><span class="sxs-lookup"><span data-stu-id="726bd-132">A method used as a controller action cannot be overloaded.</span></span> <span data-ttu-id="726bd-133">Кроме того действия контроллера не может быть статическим методом.</span><span class="sxs-lookup"><span data-stu-id="726bd-133">Furthermore, a controller action cannot be a static method.</span></span> <span data-ttu-id="726bd-134">За исключением этого можно использовать практически любой метод, как действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="726bd-134">Other than that, you can use just about any method as a controller action.</span></span>

## <a name="understanding-action-results"></a><span data-ttu-id="726bd-135">Основные сведения о результатах действия</span><span class="sxs-lookup"><span data-stu-id="726bd-135">Understanding Action Results</span></span>

<span data-ttu-id="726bd-136">Возвращает действие контроллера называемое *результат действия*.</span><span class="sxs-lookup"><span data-stu-id="726bd-136">A controller action returns something called an *action result*.</span></span> <span data-ttu-id="726bd-137">Результат действия — возвращает в ответ на запрос браузера действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="726bd-137">An action result is what a controller action returns in response to a browser request.</span></span>

<span data-ttu-id="726bd-138">Платформа ASP.NET MVC поддерживает несколько типов результатов действий, в том числе:</span><span class="sxs-lookup"><span data-stu-id="726bd-138">The ASP.NET MVC framework supports several types of action results including:</span></span>

1. <span data-ttu-id="726bd-139">ViewResult - представляет HTML и разметки.</span><span class="sxs-lookup"><span data-stu-id="726bd-139">ViewResult - Represents HTML and markup.</span></span>
2. <span data-ttu-id="726bd-140">EmptyResult - представляет никакого результата.</span><span class="sxs-lookup"><span data-stu-id="726bd-140">EmptyResult - Represents no result.</span></span>
3. <span data-ttu-id="726bd-141">RedirectResult - представляет перенаправление на новый URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="726bd-141">RedirectResult - Represents a redirection to a new URL.</span></span>
4. <span data-ttu-id="726bd-142">JsonResult - представляет результат нотации объектов JavaScript, который может использоваться в приложениях AJAX.</span><span class="sxs-lookup"><span data-stu-id="726bd-142">JsonResult - Represents a JavaScript Object Notation result that can be used in an AJAX application.</span></span>
5. <span data-ttu-id="726bd-143">JavaScriptResult - представляет сценарий JavaScript.</span><span class="sxs-lookup"><span data-stu-id="726bd-143">JavaScriptResult - Represents a JavaScript script.</span></span>
6. <span data-ttu-id="726bd-144">ContentResult - представляет результат текста.</span><span class="sxs-lookup"><span data-stu-id="726bd-144">ContentResult - Represents a text result.</span></span>
7. <span data-ttu-id="726bd-145">FileContentResult - представляет загружаемый файл (с двоичное содержимое).</span><span class="sxs-lookup"><span data-stu-id="726bd-145">FileContentResult - Represents a downloadable file (with the binary content).</span></span>
8. <span data-ttu-id="726bd-146">FilePathResult - представляет загружаемый файл (с путем).</span><span class="sxs-lookup"><span data-stu-id="726bd-146">FilePathResult - Represents a downloadable file (with a path).</span></span>
9. <span data-ttu-id="726bd-147">FileStreamResult - представляет загружаемый файл (с файлового потока).</span><span class="sxs-lookup"><span data-stu-id="726bd-147">FileStreamResult - Represents a downloadable file (with a file stream).</span></span>

<span data-ttu-id="726bd-148">Результаты этих действий наследуются от базового класса ActionResult.</span><span class="sxs-lookup"><span data-stu-id="726bd-148">All of these action results inherit from the base ActionResult class.</span></span>

<span data-ttu-id="726bd-149">В большинстве случаев действия контроллера возвращает ViewResult.</span><span class="sxs-lookup"><span data-stu-id="726bd-149">In most cases, a controller action returns a ViewResult.</span></span> <span data-ttu-id="726bd-150">Например действие контроллера индекса в списке 2 Возвращает ViewResult.</span><span class="sxs-lookup"><span data-stu-id="726bd-150">For example, the Index controller action in Listing 2 returns a ViewResult.</span></span>

<span data-ttu-id="726bd-151">**Листинг 2 - Controllers\BookController.vb**</span><span class="sxs-lookup"><span data-stu-id="726bd-151">**Listing 2 - Controllers\BookController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample2.vb)]

<span data-ttu-id="726bd-152">Если действие возвращает ViewResult, HTML возвращается в браузере.</span><span class="sxs-lookup"><span data-stu-id="726bd-152">When an action returns a ViewResult, HTML is returned to the browser.</span></span> <span data-ttu-id="726bd-153">Метод Index() в списке 2 Возвращает представление с именем Index в браузере.</span><span class="sxs-lookup"><span data-stu-id="726bd-153">The Index() method in Listing 2 returns a view named Index to the browser.</span></span>

<span data-ttu-id="726bd-154">Обратите внимание, что Index() действие в списке 2 Возвращает ViewResult().</span><span class="sxs-lookup"><span data-stu-id="726bd-154">Notice that the Index() action in Listing 2 does not return a ViewResult().</span></span> <span data-ttu-id="726bd-155">Вместо этого вызывается метод View() базового класса контроллера.</span><span class="sxs-lookup"><span data-stu-id="726bd-155">Instead, the View() method of the Controller base class is called.</span></span> <span data-ttu-id="726bd-156">Как правило вы не возвращают результат действия непосредственно.</span><span class="sxs-lookup"><span data-stu-id="726bd-156">Normally, you do not return an action result directly.</span></span> <span data-ttu-id="726bd-157">Вместо этого вызовите один из следующих методов базового класса контроллера.</span><span class="sxs-lookup"><span data-stu-id="726bd-157">Instead, you call one of the following methods of the Controller base class:</span></span>

1. <span data-ttu-id="726bd-158">Просмотр - возвращает результат действия ViewResult.</span><span class="sxs-lookup"><span data-stu-id="726bd-158">View - Returns a ViewResult action result.</span></span>
2. <span data-ttu-id="726bd-159">Перенаправить — возвращает результат действия RedirectResult.</span><span class="sxs-lookup"><span data-stu-id="726bd-159">Redirect - Returns a RedirectResult action result.</span></span>
3. <span data-ttu-id="726bd-160">RedirectToAction - возвращает результат действия RedirectToRouteResult.</span><span class="sxs-lookup"><span data-stu-id="726bd-160">RedirectToAction - Returns a RedirectToRouteResult action result.</span></span>
4. <span data-ttu-id="726bd-161">RedirectToRoute - возвращает результат действия RedirectToRouteResult.</span><span class="sxs-lookup"><span data-stu-id="726bd-161">RedirectToRoute - Returns a RedirectToRouteResult action result.</span></span>
5. <span data-ttu-id="726bd-162">JSON - возвращает результат действия JsonResult.</span><span class="sxs-lookup"><span data-stu-id="726bd-162">Json - Returns a JsonResult action result.</span></span>
6. <span data-ttu-id="726bd-163">JavaScriptResult - возвращает JavaScriptResult.</span><span class="sxs-lookup"><span data-stu-id="726bd-163">JavaScriptResult - Returns a JavaScriptResult.</span></span>
7. <span data-ttu-id="726bd-164">Содержания - возвращает результат действия ContentResult.</span><span class="sxs-lookup"><span data-stu-id="726bd-164">Content - Returns a ContentResult action result.</span></span>
8. <span data-ttu-id="726bd-165">Файл - возвращает FileContentResult, FilePathResult или FileStreamResult в зависимости от параметров, переданного методу.</span><span class="sxs-lookup"><span data-stu-id="726bd-165">File - Returns a FileContentResult, FilePathResult, or FileStreamResult depending on the parameters passed to the method.</span></span>

<span data-ttu-id="726bd-166">Таким образом Если требуется возвратить представление в браузере, вызовите метод View().</span><span class="sxs-lookup"><span data-stu-id="726bd-166">So, if you want to return a View to the browser, you call the View() method.</span></span> <span data-ttu-id="726bd-167">Если вы хотите перенаправить пользователя из одного контроллера действия к другому, можно вызвать метод RedirectToAction().</span><span class="sxs-lookup"><span data-stu-id="726bd-167">If you want to redirect the user from one controller action to another, you call the RedirectToAction() method.</span></span> <span data-ttu-id="726bd-168">Например действие Details() в списке 3 отображает представление либо перенаправляет пользователя на Index() действие в зависимости от того, является ли параметр Id имеет значение.</span><span class="sxs-lookup"><span data-stu-id="726bd-168">For example, the Details() action in Listing 3 either displays a view or redirects the user to the Index() action depending on whether the Id parameter has a value.</span></span>

<span data-ttu-id="726bd-169">**Листинг 3 - CustomerController.vb**</span><span class="sxs-lookup"><span data-stu-id="726bd-169">**Listing 3 - CustomerController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample3.vb)]

<span data-ttu-id="726bd-170">Результат действия ContentResult специальные.</span><span class="sxs-lookup"><span data-stu-id="726bd-170">The ContentResult action result is special.</span></span> <span data-ttu-id="726bd-171">Результат действия ContentResult можно использовать для возвращения результата действия в виде обычного текста.</span><span class="sxs-lookup"><span data-stu-id="726bd-171">You can use the ContentResult action result to return an action result as plain text.</span></span> <span data-ttu-id="726bd-172">Например метод Index() в листинге 4 возвращает сообщение, как обычный текст, а не как HTML.</span><span class="sxs-lookup"><span data-stu-id="726bd-172">For example, the Index() method in Listing 4 returns a message as plain text and not as HTML.</span></span>

<span data-ttu-id="726bd-173">**Листинг 4 - Controllers\StatusController.vb**</span><span class="sxs-lookup"><span data-stu-id="726bd-173">**Listing 4 - Controllers\StatusController.vb**</span></span>

> <span data-ttu-id="726bd-174">StatusController</span><span class="sxs-lookup"><span data-stu-id="726bd-174">StatusController</span></span>


> <span data-ttu-id="726bd-175">System.Web.Mvc.Controller</span><span class="sxs-lookup"><span data-stu-id="726bd-175">System.Web.Mvc.Controller</span></span>


[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample4.vb)]

<span data-ttu-id="726bd-176">При вызове действия StatusController.Index() представления не возвращается.</span><span class="sxs-lookup"><span data-stu-id="726bd-176">When the StatusController.Index() action is invoked, a view is not returned.</span></span> <span data-ttu-id="726bd-177">Вместо этого необработанный текст «Hello World!»</span><span class="sxs-lookup"><span data-stu-id="726bd-177">Instead, the raw text "Hello World!"</span></span> <span data-ttu-id="726bd-178">возвращается в браузере.</span><span class="sxs-lookup"><span data-stu-id="726bd-178">is returned to the browser.</span></span>

<span data-ttu-id="726bd-179">Если действие контроллера возвращает результат, не результат действия — например, даты или целое число — затем результат заключается в ContentResult автоматически.</span><span class="sxs-lookup"><span data-stu-id="726bd-179">If a controller action returns a result that is not an action result - for example, a date or an integer - then the result is wrapped in a ContentResult automatically.</span></span> <span data-ttu-id="726bd-180">Например при вызове действия Index() WorkController в листинге 5 даты возвращается в виде ContentResult автоматически.</span><span class="sxs-lookup"><span data-stu-id="726bd-180">For example, when the Index() action of the WorkController in Listing 5 is invoked, the date is returned as a ContentResult automatically.</span></span>

<span data-ttu-id="726bd-181">**Список 5 - WorkController.vb**</span><span class="sxs-lookup"><span data-stu-id="726bd-181">**Listing 5 - WorkController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample5.vb)]

<span data-ttu-id="726bd-182">Действие Index() в листинге 5 возвращает объект DateTime.</span><span class="sxs-lookup"><span data-stu-id="726bd-182">The Index() action in Listing 5 returns a DateTime object.</span></span> <span data-ttu-id="726bd-183">Платформа ASP.NET MVC преобразует объект DateTime в строку и создает оболочку значения даты и времени в ContentResult автоматически.</span><span class="sxs-lookup"><span data-stu-id="726bd-183">The ASP.NET MVC framework converts the DateTime object to a string and wraps the DateTime value in a ContentResult automatically.</span></span> <span data-ttu-id="726bd-184">Обозреватель получает дату и время в виде обычного текста.</span><span class="sxs-lookup"><span data-stu-id="726bd-184">The browser receives the date and time as plain text.</span></span>

## <a name="summary"></a><span data-ttu-id="726bd-185">Сводка</span><span class="sxs-lookup"><span data-stu-id="726bd-185">Summary</span></span>

<span data-ttu-id="726bd-186">Цель этого учебника было для ознакомления с основными понятиями контроллеров ASP.NET MVC, действия контроллера и результаты действий контроллера.</span><span class="sxs-lookup"><span data-stu-id="726bd-186">The purpose of this tutorial was to introduce you to the concepts of ASP.NET MVC controllers, controller actions, and controller action results.</span></span> <span data-ttu-id="726bd-187">В первом разделе вы узнали, как добавление новых контроллеров в проект ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="726bd-187">In the first section, you learned how to add new controllers to an ASP.NET MVC project.</span></span> <span data-ttu-id="726bd-188">Далее вы узнали, как открытые методы контроллера доступны вселенной действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="726bd-188">Next, you learned how public methods of a controller are exposed to the universe as controller actions.</span></span> <span data-ttu-id="726bd-189">Наконец мы рассмотрели различные типы результатов действий, которые могут быть возвращены из действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="726bd-189">Finally, we discussed the different types of action results that can be returned from a controller action.</span></span> <span data-ttu-id="726bd-190">В частности мы рассмотрели, как получить ViewResult, RedirectToActionResult и ContentResult из действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="726bd-190">In particular, we discussed how to return a ViewResult, RedirectToActionResult, and ContentResult from a controller action.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="726bd-191">[Назад](creating-a-custom-route-constraint-cs.md)
[Вперед](creating-custom-routes-vb.md)</span><span class="sxs-lookup"><span data-stu-id="726bd-191">[Previous](creating-a-custom-route-constraint-cs.md)
[Next](creating-custom-routes-vb.md)</span></span>
