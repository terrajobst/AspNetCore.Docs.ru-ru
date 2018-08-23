---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
title: Общие сведения о контроллерах ASP.NET MVC (Visual Basic) | Документация Майкрософт
author: StephenWalther
description: В этом руководстве Стивен Вальтер вы познакомитесь с контроллеров ASP.NET MVC. Вы узнаете, как создать новые контроллеры и вернуть различные виды res действие...
ms.author: riande
ms.date: 02/16/2008
ms.assetid: 94c3e5d9-a904-445e-a34e-d92fd1ca108a
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: 604bf4af2a46e56d9445de141fae1a1651acf47f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838825"
---
<a name="aspnet-mvc-controller-overview-vb"></a><span data-ttu-id="e5abe-104">Общие сведения о контроллерах ASP.NET MVC (VB)</span><span class="sxs-lookup"><span data-stu-id="e5abe-104">ASP.NET MVC Controller Overview (VB)</span></span>
====================
<span data-ttu-id="e5abe-105">по [Стивен Вальтер](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="e5abe-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="e5abe-106">В этом руководстве Стивен Вальтер вы познакомитесь с контроллеров ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e5abe-106">In this tutorial, Stephen Walther introduces you to ASP.NET MVC controllers.</span></span> <span data-ttu-id="e5abe-107">Вы узнаете, как создать новые контроллеры и вернуть различные типы результатов действий.</span><span class="sxs-lookup"><span data-stu-id="e5abe-107">You learn how to create new controllers and return different types of action results.</span></span>


<span data-ttu-id="e5abe-108">В этом руководстве рассматриваются в разделе ASP.NET MVC контроллеров, действий контроллера, и результаты действий.</span><span class="sxs-lookup"><span data-stu-id="e5abe-108">This tutorial explores the topic of ASP.NET MVC controllers, controller actions, and action results.</span></span> <span data-ttu-id="e5abe-109">После завершения этого учебника вы узнаете, как контроллеры используются для управления способом посетитель взаимодействует с веб-сайта ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e5abe-109">After you complete this tutorial, you will understand how controllers are used to control the way a visitor interacts with an ASP.NET MVC website.</span></span>

## <a name="understanding-controllers"></a><span data-ttu-id="e5abe-110">Общее представление о контроллерах</span><span class="sxs-lookup"><span data-stu-id="e5abe-110">Understanding Controllers</span></span>

<span data-ttu-id="e5abe-111">Контроллеры MVC отвечают за реагирование на запросы к веб-сайта ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e5abe-111">MVC controllers are responsible for responding to requests made against an ASP.NET MVC website.</span></span> <span data-ttu-id="e5abe-112">Каждый запрос браузера сопоставляется с определенным контроллером.</span><span class="sxs-lookup"><span data-stu-id="e5abe-112">Each browser request is mapped to a particular controller.</span></span> <span data-ttu-id="e5abe-113">Например представьте, что введите следующий URL-адрес в адресную строку браузера:</span><span class="sxs-lookup"><span data-stu-id="e5abe-113">For example, imagine that you enter the following URL into the address bar of your browser:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="e5abe-114">В этом случае вызывается контроллер с именем ProductController.</span><span class="sxs-lookup"><span data-stu-id="e5abe-114">In this case, a controller named ProductController is invoked.</span></span> <span data-ttu-id="e5abe-115">ProductController отвечает за создание ответ на запрос браузера.</span><span class="sxs-lookup"><span data-stu-id="e5abe-115">The ProductController is responsible for generating the response to the browser request.</span></span> <span data-ttu-id="e5abe-116">Например контроллер может вернуть это конкретное представление браузера или контроллер может перенаправить пользователя к другому контроллеру.</span><span class="sxs-lookup"><span data-stu-id="e5abe-116">For example, the controller might return a particular view back to the browser or the controller might redirect the user to another controller.</span></span>

<span data-ttu-id="e5abe-117">В листинге 1 содержит простой контроллер с именем ProductController.</span><span class="sxs-lookup"><span data-stu-id="e5abe-117">Listing 1 contains a simple controller named ProductController.</span></span>

<span data-ttu-id="e5abe-118">**Listing1 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="e5abe-118">**Listing1 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample1.vb)]

<span data-ttu-id="e5abe-119">Как показано в листинге 1, контроллер — это класс (класс Visual Basic .NET или C#).</span><span class="sxs-lookup"><span data-stu-id="e5abe-119">As you can see from Listing 1, a controller is just a class (a Visual Basic .NET or C# class).</span></span> <span data-ttu-id="e5abe-120">Контроллер, является класс, производный от базового класса класса System.Web.Mvc.Controller.</span><span class="sxs-lookup"><span data-stu-id="e5abe-120">A controller is a class that derives from the base System.Web.Mvc.Controller class.</span></span> <span data-ttu-id="e5abe-121">Поскольку контроллер наследует от этого базового класса, контроллер наследует несколько полезных методов бесплатно (мы обсудим эти методы чуть позже).</span><span class="sxs-lookup"><span data-stu-id="e5abe-121">Because a controller inherits from this base class, a controller inherits several useful methods for free (We discuss these methods in a moment).</span></span>

## <a name="understanding-controller-actions"></a><span data-ttu-id="e5abe-122">Основные сведения о действиях контроллера</span><span class="sxs-lookup"><span data-stu-id="e5abe-122">Understanding Controller Actions</span></span>

<span data-ttu-id="e5abe-123">Контроллер обеспечивает доступ к действиям контроллера.</span><span class="sxs-lookup"><span data-stu-id="e5abe-123">A controller exposes controller actions.</span></span> <span data-ttu-id="e5abe-124">Действие — это метод на контроллере, который вызывается при вводе определенного URL-адреса в адресной строке браузера.</span><span class="sxs-lookup"><span data-stu-id="e5abe-124">An action is a method on a controller that gets called when you enter a particular URL in your browser address bar.</span></span> <span data-ttu-id="e5abe-125">Например представьте, что можно сделать запрос следующий URL-адрес:</span><span class="sxs-lookup"><span data-stu-id="e5abe-125">For example, imagine that you make a request for the following URL:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="e5abe-126">В этом случае метод Index() вызывается в классе ProductController.</span><span class="sxs-lookup"><span data-stu-id="e5abe-126">In this case, the Index() method is called on the ProductController class.</span></span> <span data-ttu-id="e5abe-127">Метод Index() — пример действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="e5abe-127">The Index() method is an example of a controller action.</span></span>

<span data-ttu-id="e5abe-128">Действие контроллера должен быть открытый метод класса контроллера.</span><span class="sxs-lookup"><span data-stu-id="e5abe-128">A controller action must be a public method of a controller class.</span></span> <span data-ttu-id="e5abe-129">Visual Basic.NET, по умолчанию являются открытые методы.</span><span class="sxs-lookup"><span data-stu-id="e5abe-129">Visual Basic.NET methods, by default, are public methods.</span></span> <span data-ttu-id="e5abe-130">Учтите, что любой открытый метод, который можно добавить в класс контроллера указывается в виде действие контроллера автоматически (Будьте внимательны, чтобы об этом, так как действия контроллера могут быть вызваны любой пользователь во вселенной просто введя правой URL-адрес в адресную строку браузера).</span><span class="sxs-lookup"><span data-stu-id="e5abe-130">Realize that any public method that you add to a controller class is exposed as a controller action automatically (You must be careful about this since a controller action can be invoked by anyone in the universe simply by typing the right URL into a browser address bar).</span></span>

<span data-ttu-id="e5abe-131">Существуют некоторые дополнительные требования, которые должны быть удовлетворены с помощью действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="e5abe-131">There are some additional requirements that must be satisfied by a controller action.</span></span> <span data-ttu-id="e5abe-132">Метод, используемый как действие контроллера, не могут быть перегружены.</span><span class="sxs-lookup"><span data-stu-id="e5abe-132">A method used as a controller action cannot be overloaded.</span></span> <span data-ttu-id="e5abe-133">Кроме того действие контроллера не может быть статический метод.</span><span class="sxs-lookup"><span data-stu-id="e5abe-133">Furthermore, a controller action cannot be a static method.</span></span> <span data-ttu-id="e5abe-134">За исключением этого можно использовать практически любой метод, как действие контроллера.</span><span class="sxs-lookup"><span data-stu-id="e5abe-134">Other than that, you can use just about any method as a controller action.</span></span>

## <a name="understanding-action-results"></a><span data-ttu-id="e5abe-135">Основные сведения о результатах действий</span><span class="sxs-lookup"><span data-stu-id="e5abe-135">Understanding Action Results</span></span>

<span data-ttu-id="e5abe-136">Возвращает действие контроллера, так называемую *результат действия*.</span><span class="sxs-lookup"><span data-stu-id="e5abe-136">A controller action returns something called an *action result*.</span></span> <span data-ttu-id="e5abe-137">Результат действия является действие контроллера возвращает в ответ на запрос браузера.</span><span class="sxs-lookup"><span data-stu-id="e5abe-137">An action result is what a controller action returns in response to a browser request.</span></span>

<span data-ttu-id="e5abe-138">Платформа ASP.NET MVC поддерживает несколько типов результатов действий, в том числе:</span><span class="sxs-lookup"><span data-stu-id="e5abe-138">The ASP.NET MVC framework supports several types of action results including:</span></span>

1. <span data-ttu-id="e5abe-139">ViewResult - HTML представляет и разметки.</span><span class="sxs-lookup"><span data-stu-id="e5abe-139">ViewResult - Represents HTML and markup.</span></span>
2. <span data-ttu-id="e5abe-140">EmptyResult - представляет результат отсутствует.</span><span class="sxs-lookup"><span data-stu-id="e5abe-140">EmptyResult - Represents no result.</span></span>
3. <span data-ttu-id="e5abe-141">RedirectResult - представляет перенаправление на новый URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="e5abe-141">RedirectResult - Represents a redirection to a new URL.</span></span>
4. <span data-ttu-id="e5abe-142">JsonResult - представляет нотация объектов JavaScript результат, который может использоваться в приложении AJAX.</span><span class="sxs-lookup"><span data-stu-id="e5abe-142">JsonResult - Represents a JavaScript Object Notation result that can be used in an AJAX application.</span></span>
5. <span data-ttu-id="e5abe-143">JavaScriptResult - представляет скрипт JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e5abe-143">JavaScriptResult - Represents a JavaScript script.</span></span>
6. <span data-ttu-id="e5abe-144">ContentResult - представляет текстовый результат.</span><span class="sxs-lookup"><span data-stu-id="e5abe-144">ContentResult - Represents a text result.</span></span>
7. <span data-ttu-id="e5abe-145">FileContentResult - представляет загружаемый файл (с двоичное содержимое).</span><span class="sxs-lookup"><span data-stu-id="e5abe-145">FileContentResult - Represents a downloadable file (with the binary content).</span></span>
8. <span data-ttu-id="e5abe-146">FilePathResult - представляет загружаемый файл (с путем).</span><span class="sxs-lookup"><span data-stu-id="e5abe-146">FilePathResult - Represents a downloadable file (with a path).</span></span>
9. <span data-ttu-id="e5abe-147">FileStreamResult - представляет загружаемый файл (с файловым потоком).</span><span class="sxs-lookup"><span data-stu-id="e5abe-147">FileStreamResult - Represents a downloadable file (with a file stream).</span></span>

<span data-ttu-id="e5abe-148">Все эти результаты действия наследуют от базового класса ActionResult.</span><span class="sxs-lookup"><span data-stu-id="e5abe-148">All of these action results inherit from the base ActionResult class.</span></span>

<span data-ttu-id="e5abe-149">В большинстве случаев действие контроллера возвращает ViewResult.</span><span class="sxs-lookup"><span data-stu-id="e5abe-149">In most cases, a controller action returns a ViewResult.</span></span> <span data-ttu-id="e5abe-150">Например действие контроллера индекса в листинге 2 Возвращает ViewResult.</span><span class="sxs-lookup"><span data-stu-id="e5abe-150">For example, the Index controller action in Listing 2 returns a ViewResult.</span></span>

<span data-ttu-id="e5abe-151">**В листинге 2 - Controllers\BookController.vb**</span><span class="sxs-lookup"><span data-stu-id="e5abe-151">**Listing 2 - Controllers\BookController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample2.vb)]

<span data-ttu-id="e5abe-152">Когда действие возвращает ViewResult, HTML, возвращается в браузер.</span><span class="sxs-lookup"><span data-stu-id="e5abe-152">When an action returns a ViewResult, HTML is returned to the browser.</span></span> <span data-ttu-id="e5abe-153">Метод Index() в листинге 2 Возвращает представление с именем Index в браузер.</span><span class="sxs-lookup"><span data-stu-id="e5abe-153">The Index() method in Listing 2 returns a view named Index to the browser.</span></span>

<span data-ttu-id="e5abe-154">Обратите внимание на то, что действие Index() в листинге 2 не возвращает ViewResult().</span><span class="sxs-lookup"><span data-stu-id="e5abe-154">Notice that the Index() action in Listing 2 does not return a ViewResult().</span></span> <span data-ttu-id="e5abe-155">Вместо этого вызывается метод View() базового класса контроллера.</span><span class="sxs-lookup"><span data-stu-id="e5abe-155">Instead, the View() method of the Controller base class is called.</span></span> <span data-ttu-id="e5abe-156">Как правило вы не возвращают результат действия напрямую.</span><span class="sxs-lookup"><span data-stu-id="e5abe-156">Normally, you do not return an action result directly.</span></span> <span data-ttu-id="e5abe-157">Вместо этого вызовите один из следующих методов базового класса контроллера.</span><span class="sxs-lookup"><span data-stu-id="e5abe-157">Instead, you call one of the following methods of the Controller base class:</span></span>

1. <span data-ttu-id="e5abe-158">Просмотр - возвращает результат действия ViewResult.</span><span class="sxs-lookup"><span data-stu-id="e5abe-158">View - Returns a ViewResult action result.</span></span>
2. <span data-ttu-id="e5abe-159">Перенаправление — возвращает результат действия RedirectResult.</span><span class="sxs-lookup"><span data-stu-id="e5abe-159">Redirect - Returns a RedirectResult action result.</span></span>
3. <span data-ttu-id="e5abe-160">RedirectToAction - возвращает результат действия RedirectToRouteResult.</span><span class="sxs-lookup"><span data-stu-id="e5abe-160">RedirectToAction - Returns a RedirectToRouteResult action result.</span></span>
4. <span data-ttu-id="e5abe-161">RedirectToRoute - возвращает результат действия RedirectToRouteResult.</span><span class="sxs-lookup"><span data-stu-id="e5abe-161">RedirectToRoute - Returns a RedirectToRouteResult action result.</span></span>
5. <span data-ttu-id="e5abe-162">JSON — возвращает результат действия JsonResult.</span><span class="sxs-lookup"><span data-stu-id="e5abe-162">Json - Returns a JsonResult action result.</span></span>
6. <span data-ttu-id="e5abe-163">JavaScriptResult - возвращает JavaScriptResult.</span><span class="sxs-lookup"><span data-stu-id="e5abe-163">JavaScriptResult - Returns a JavaScriptResult.</span></span>
7. <span data-ttu-id="e5abe-164">Содержания - возвращает результат действия ContentResult.</span><span class="sxs-lookup"><span data-stu-id="e5abe-164">Content - Returns a ContentResult action result.</span></span>
8. <span data-ttu-id="e5abe-165">Файл — возвращает FileContentResult, FilePathResult или FileStreamResult в зависимости от параметров, передается в метод.</span><span class="sxs-lookup"><span data-stu-id="e5abe-165">File - Returns a FileContentResult, FilePathResult, or FileStreamResult depending on the parameters passed to the method.</span></span>

<span data-ttu-id="e5abe-166">Таким образом Если вы хотите вернуть представление в браузере, можно вызвать метод View().</span><span class="sxs-lookup"><span data-stu-id="e5abe-166">So, if you want to return a View to the browser, you call the View() method.</span></span> <span data-ttu-id="e5abe-167">Если вы хотите перенаправить пользователя из действия один контроллер в другой, можно вызвать метод RedirectToAction().</span><span class="sxs-lookup"><span data-stu-id="e5abe-167">If you want to redirect the user from one controller action to another, you call the RedirectToAction() method.</span></span> <span data-ttu-id="e5abe-168">Например действие Details() в листинге 3 отображается представление либо перенаправляет пользователя на действие Index() в зависимости от того, является ли параметр Id имеет значение.</span><span class="sxs-lookup"><span data-stu-id="e5abe-168">For example, the Details() action in Listing 3 either displays a view or redirects the user to the Index() action depending on whether the Id parameter has a value.</span></span>

<span data-ttu-id="e5abe-169">**Листинг 3 - CustomerController.vb**</span><span class="sxs-lookup"><span data-stu-id="e5abe-169">**Listing 3 - CustomerController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample3.vb)]

<span data-ttu-id="e5abe-170">Результат действия ContentResult специальные.</span><span class="sxs-lookup"><span data-stu-id="e5abe-170">The ContentResult action result is special.</span></span> <span data-ttu-id="e5abe-171">Результат действия ContentResult можно использовать для возвращения результата действия в виде обычного текста.</span><span class="sxs-lookup"><span data-stu-id="e5abe-171">You can use the ContentResult action result to return an action result as plain text.</span></span> <span data-ttu-id="e5abe-172">Например метод Index() в листинге 4 возвращает сообщение, как обычный текст, а не как HTML.</span><span class="sxs-lookup"><span data-stu-id="e5abe-172">For example, the Index() method in Listing 4 returns a message as plain text and not as HTML.</span></span>

<span data-ttu-id="e5abe-173">**Листинг 4 - Controllers\StatusController.vb**</span><span class="sxs-lookup"><span data-stu-id="e5abe-173">**Listing 4 - Controllers\StatusController.vb**</span></span>

> <span data-ttu-id="e5abe-174">StatusController</span><span class="sxs-lookup"><span data-stu-id="e5abe-174">StatusController</span></span>
> 
> 
> <span data-ttu-id="e5abe-175">Класса System.Web.Mvc.Controller</span><span class="sxs-lookup"><span data-stu-id="e5abe-175">System.Web.Mvc.Controller</span></span>


[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample4.vb)]

<span data-ttu-id="e5abe-176">При вызове действия StatusController.Index(), представление не возвращается.</span><span class="sxs-lookup"><span data-stu-id="e5abe-176">When the StatusController.Index() action is invoked, a view is not returned.</span></span> <span data-ttu-id="e5abe-177">Вместо этого необработанный текст «Hello World!»</span><span class="sxs-lookup"><span data-stu-id="e5abe-177">Instead, the raw text "Hello World!"</span></span> <span data-ttu-id="e5abe-178">возвращается в браузер.</span><span class="sxs-lookup"><span data-stu-id="e5abe-178">is returned to the browser.</span></span>

<span data-ttu-id="e5abe-179">Если действие контроллера возвращает результат, не результат действия — например, даты или целое число — затем результат заключается в ContentResult автоматически.</span><span class="sxs-lookup"><span data-stu-id="e5abe-179">If a controller action returns a result that is not an action result - for example, a date or an integer - then the result is wrapped in a ContentResult automatically.</span></span> <span data-ttu-id="e5abe-180">Например при вызове действия Index() WorkController в листинге 5 даты возвращается в виде ContentResult автоматически.</span><span class="sxs-lookup"><span data-stu-id="e5abe-180">For example, when the Index() action of the WorkController in Listing 5 is invoked, the date is returned as a ContentResult automatically.</span></span>

<span data-ttu-id="e5abe-181">**В листинге 5 - WorkController.vb**</span><span class="sxs-lookup"><span data-stu-id="e5abe-181">**Listing 5 - WorkController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample5.vb)]

<span data-ttu-id="e5abe-182">Это действие Index() в листинге 5 возвращает объект DateTime.</span><span class="sxs-lookup"><span data-stu-id="e5abe-182">The Index() action in Listing 5 returns a DateTime object.</span></span> <span data-ttu-id="e5abe-183">Платформа ASP.NET MVC преобразует объект DateTime в строку и создает оболочку значения даты и времени в ContentResult автоматически.</span><span class="sxs-lookup"><span data-stu-id="e5abe-183">The ASP.NET MVC framework converts the DateTime object to a string and wraps the DateTime value in a ContentResult automatically.</span></span> <span data-ttu-id="e5abe-184">Обозреватель получает дату и время в виде обычного текста.</span><span class="sxs-lookup"><span data-stu-id="e5abe-184">The browser receives the date and time as plain text.</span></span>

## <a name="summary"></a><span data-ttu-id="e5abe-185">Сводка</span><span class="sxs-lookup"><span data-stu-id="e5abe-185">Summary</span></span>

<span data-ttu-id="e5abe-186">Цель этого руководства мы хотели вам познакомиться с основными понятиями, контроллеры ASP.NET MVC, контроллер действий и результатов действий контроллера.</span><span class="sxs-lookup"><span data-stu-id="e5abe-186">The purpose of this tutorial was to introduce you to the concepts of ASP.NET MVC controllers, controller actions, and controller action results.</span></span> <span data-ttu-id="e5abe-187">В первом разделе вы узнали, как добавить новых контроллеров в проекте ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e5abe-187">In the first section, you learned how to add new controllers to an ASP.NET MVC project.</span></span> <span data-ttu-id="e5abe-188">Далее вы узнали, как открытые методы контроллера предоставляются вселенной виде действий контроллера.</span><span class="sxs-lookup"><span data-stu-id="e5abe-188">Next, you learned how public methods of a controller are exposed to the universe as controller actions.</span></span> <span data-ttu-id="e5abe-189">Наконец мы рассмотрели различные типы результатов действий, которые могут быть возвращены из действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="e5abe-189">Finally, we discussed the different types of action results that can be returned from a controller action.</span></span> <span data-ttu-id="e5abe-190">В частности мы рассмотрели, как вернуть ViewResult, RedirectToActionResult и ContentResult из действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="e5abe-190">In particular, we discussed how to return a ViewResult, RedirectToActionResult, and ContentResult from a controller action.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e5abe-191">[Назад](creating-a-custom-route-constraint-cs.md)
> [Вперед](creating-custom-routes-vb.md)</span><span class="sxs-lookup"><span data-stu-id="e5abe-191">[Previous](creating-a-custom-route-constraint-cs.md)
[Next](creating-custom-routes-vb.md)</span></span>
