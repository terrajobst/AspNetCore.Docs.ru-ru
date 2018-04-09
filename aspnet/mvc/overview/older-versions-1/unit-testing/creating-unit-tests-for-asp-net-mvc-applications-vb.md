---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
title: Создание модульных тестов для приложений ASP.NET MVC (VB) | Документы Microsoft
author: StephenWalther
description: Описание способов создания модульных тестов для действий контроллера. Стивен Вальтер в этом учебнике показано, как проверка действия контроллера возвращает parti...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: eb35710d-1d99-44ac-b61f-e50af8cb328a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
msc.type: authoredcontent
ms.openlocfilehash: 299665f45d72fee33f92344ed53c87dfb1a76d60
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-vb"></a><span data-ttu-id="2a689-104">Создание модульных тестов для приложений ASP.NET MVC (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="2a689-104">Creating Unit Tests for ASP.NET MVC Applications (VB)</span></span>
====================
<span data-ttu-id="2a689-105">по [Стивен Вальтер](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="2a689-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

[<span data-ttu-id="2a689-106">Скачать PDF</span><span class="sxs-lookup"><span data-stu-id="2a689-106">Download PDF</span></span>](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_VB.pdf)

> <span data-ttu-id="2a689-107">Описание способов создания модульных тестов для действий контроллера.</span><span class="sxs-lookup"><span data-stu-id="2a689-107">Learn how to create unit tests for controller actions.</span></span> <span data-ttu-id="2a689-108">Стивен Вальтер в этом учебнике показано, как проверка действия контроллера возвращает конкретное представление, возвращает определенного набора данных или другого типа результата действия.</span><span class="sxs-lookup"><span data-stu-id="2a689-108">In this tutorial, Stephen Walther demonstrates how to test whether a controller action returns a particular view, returns a particular set of data, or returns a different type of action result.</span></span>


<span data-ttu-id="2a689-109">Целью данного учебника является демонстрация как приложения можно создавать модульные тесты для контроллеров в your ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="2a689-109">The goal of this tutorial is to demonstrate how you can write unit tests for the controllers in your ASP.NET MVC applications.</span></span> <span data-ttu-id="2a689-110">Рассматриваются способы построения три разных типа модульных тестов.</span><span class="sxs-lookup"><span data-stu-id="2a689-110">We discuss how to build three different types of unit tests.</span></span> <span data-ttu-id="2a689-111">Вы узнаете, как тестирование представления, возвращенных действия контроллера, способа представления данных, возвращаемых функцией действия контроллера тестирования и проверка ли одного контроллера действия перенаправит вас на второе действие контроллера.</span><span class="sxs-lookup"><span data-stu-id="2a689-111">You learn how to test the view returned by a controller action, how to test the View Data returned by a controller action, and how to test whether or not one controller action redirects you to a second controller action.</span></span>

## <a name="creating-the-controller-under-test"></a><span data-ttu-id="2a689-112">Создание контроллера теста</span><span class="sxs-lookup"><span data-stu-id="2a689-112">Creating the Controller under Test</span></span>

<span data-ttu-id="2a689-113">Давайте начнем с создания контроллер, к которому мы собираемся тестирования.</span><span class="sxs-lookup"><span data-stu-id="2a689-113">Let's start by creating the controller that we intend to test.</span></span> <span data-ttu-id="2a689-114">Контроллер с именем `ProductController`, содержащихся в листинге 1.</span><span class="sxs-lookup"><span data-stu-id="2a689-114">The controller, named the `ProductController`, is contained in Listing 1.</span></span>

<span data-ttu-id="2a689-115">**Листинг 1. `ProductController.vb`**</span><span class="sxs-lookup"><span data-stu-id="2a689-115">**Listing 1 – `ProductController.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample1.vb)]

<span data-ttu-id="2a689-116">`ProductController` Содержит два метода действия с именем `Index()` и `Details()`.</span><span class="sxs-lookup"><span data-stu-id="2a689-116">The `ProductController` contains two action methods named `Index()` and `Details()`.</span></span> <span data-ttu-id="2a689-117">Оба метода действия возвращают представления.</span><span class="sxs-lookup"><span data-stu-id="2a689-117">Both action methods return a view.</span></span> <span data-ttu-id="2a689-118">Обратите внимание, что `Details()` действие принимает параметр с именем идентификатора.</span><span class="sxs-lookup"><span data-stu-id="2a689-118">Notice that the `Details()` action accepts a parameter named Id.</span></span>

## <a name="testing-the-view-returned-by-a-controller"></a><span data-ttu-id="2a689-119">Тестирование представления возвращенных контроллера</span><span class="sxs-lookup"><span data-stu-id="2a689-119">Testing the View returned by a Controller</span></span>

<span data-ttu-id="2a689-120">Предположим, что мы хотим проверить ли `ProductController` возвращает правильное представление.</span><span class="sxs-lookup"><span data-stu-id="2a689-120">Imagine that we want to test whether or not the `ProductController` returns the right view.</span></span> <span data-ttu-id="2a689-121">Мы хотим, чтобы убедиться, что при `ProductController.Details()` вызове действия возвращается в представлении сведений.</span><span class="sxs-lookup"><span data-stu-id="2a689-121">We want to make sure that when the `ProductController.Details()` action is invoked, the Details view is returned.</span></span> <span data-ttu-id="2a689-122">Тестовый класс в списке 2 содержит модульный тест для проверки представления, возвращенных `ProductController.Details()` действие.</span><span class="sxs-lookup"><span data-stu-id="2a689-122">The test class in Listing 2 contains a unit test for testing the view returned by the `ProductController.Details()` action.</span></span>

<span data-ttu-id="2a689-123">**Листинг 2. `ProductControllerTest.vb`**</span><span class="sxs-lookup"><span data-stu-id="2a689-123">**Listing 2 – `ProductControllerTest.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample2.vb)]

<span data-ttu-id="2a689-124">Этот класс в списке 2 включает метод теста, с именем `TestDetailsView()`.</span><span class="sxs-lookup"><span data-stu-id="2a689-124">The class in Listing 2 includes a test method named `TestDetailsView()`.</span></span> <span data-ttu-id="2a689-125">Этот метод содержит три строки кода.</span><span class="sxs-lookup"><span data-stu-id="2a689-125">This method contains three lines of code.</span></span> <span data-ttu-id="2a689-126">В первой строке кода создается новый экземпляр `ProductController` класса.</span><span class="sxs-lookup"><span data-stu-id="2a689-126">The first line of code creates a new instance of the `ProductController` class.</span></span> <span data-ttu-id="2a689-127">Вторая строка кода вызывает контроллера `Details()` метода действия.</span><span class="sxs-lookup"><span data-stu-id="2a689-127">The second line of code invokes the controller's `Details()` action method.</span></span> <span data-ttu-id="2a689-128">Наконец, последняя строка кода проверки того, является ли представление возвращенных `Details()` действии подробного представления.</span><span class="sxs-lookup"><span data-stu-id="2a689-128">Finally, the last line of code checks whether or not the view returned by the `Details()` action is the Details view.</span></span>

<span data-ttu-id="2a689-129">`ViewResult.ViewName` Свойство представляет имя представления, возвращенный контроллер.</span><span class="sxs-lookup"><span data-stu-id="2a689-129">The `ViewResult.ViewName` property represents the name of the view returned by a controller.</span></span> <span data-ttu-id="2a689-130">Важное предупреждение о тестировании это свойство.</span><span class="sxs-lookup"><span data-stu-id="2a689-130">One big warning about testing this property.</span></span> <span data-ttu-id="2a689-131">Что контроллер можно получить двумя способами.</span><span class="sxs-lookup"><span data-stu-id="2a689-131">There are two ways that a controller can return a view.</span></span> <span data-ttu-id="2a689-132">Контроллер может явным образом получить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="2a689-132">A controller can explicitly return a view like this:</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample3.vb)]

<span data-ttu-id="2a689-133">Кроме того имя представления может выводиться из имя действия контроллера, следующим образом:</span><span class="sxs-lookup"><span data-stu-id="2a689-133">Alternatively, the name of the view can be inferred from the name of the controller action like this:</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample4.vb)]

<span data-ttu-id="2a689-134">Это действие контроллера также возвращает представление с именем `Details`.</span><span class="sxs-lookup"><span data-stu-id="2a689-134">This controller action also returns a view named `Details`.</span></span> <span data-ttu-id="2a689-135">Имя представления, однако выводится из имени действия.</span><span class="sxs-lookup"><span data-stu-id="2a689-135">However, the name of the view is inferred from the action name.</span></span> <span data-ttu-id="2a689-136">Если вы хотите проверить имя представления, вы должны явно возвращается имя представления из действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="2a689-136">If you want to test the view name, then you must explicitly return the view name from the controller action.</span></span>

<span data-ttu-id="2a689-137">Можно выполнить модульный тест в списке 2, либо введя сочетание клавиш **Ctrl-R, А** или щелкнув **выполнить все тесты в решении** кнопку (см. рис. 1).</span><span class="sxs-lookup"><span data-stu-id="2a689-137">You can run the unit test in Listing 2 by either entering the keyboard combination **Ctrl-R, A** or by clicking the **Run All Tests in Solution** button (see Figure 1).</span></span> <span data-ttu-id="2a689-138">Если тест выполнен, отобразится в окне результатов теста на рис. 2.</span><span class="sxs-lookup"><span data-stu-id="2a689-138">If the test passes, you'll see the Test Results window in Figure 2.</span></span>


<span data-ttu-id="2a689-139">[![Выполнить все тесты в решении](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2a689-139">[![Run All Tests in Solution](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)</span></span>

<span data-ttu-id="2a689-140">**На рисунке 01**: выполнить все тесты в решении ([Просмотр полноразмерное изображение](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2a689-140">**Figure 01**: Run All Tests in Solution ([Click to view full-size image](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))</span></span>


<span data-ttu-id="2a689-141">[![Выполнена успешно!](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="2a689-141">[![Success!](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)</span></span>

<span data-ttu-id="2a689-142">**На рисунке 02**: успех!</span><span class="sxs-lookup"><span data-stu-id="2a689-142">**Figure 02**: Success!</span></span> <span data-ttu-id="2a689-143">([Просмотр полноразмерное изображение](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="2a689-143">([Click to view full-size image](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))</span></span>


## <a name="testing-the-view-data-returned-by-a-controller"></a><span data-ttu-id="2a689-144">Тестирование представления данных возвращенных контроллера</span><span class="sxs-lookup"><span data-stu-id="2a689-144">Testing the View Data returned by a Controller</span></span>

<span data-ttu-id="2a689-145">Контроллер MVC передает данные в представлении с помощью так называемых *`View Data`*.</span><span class="sxs-lookup"><span data-stu-id="2a689-145">An MVC controller passes data to a view by using something called *`View Data`*.</span></span> <span data-ttu-id="2a689-146">Например, предположим, вы хотите отобразить подробные сведения по определенному продукту при вызове `ProductController Details()` действие.</span><span class="sxs-lookup"><span data-stu-id="2a689-146">For example, imagine that you want to display the details for a particular product when you invoke the `ProductController Details()` action.</span></span> <span data-ttu-id="2a689-147">В этом случае можно создать экземпляр `Product` класса (определенный в модели) и передать экземпляр для `Details` представление, используя преимущества `View Data`.</span><span class="sxs-lookup"><span data-stu-id="2a689-147">In that case, you can create an instance of a `Product` class (defined in your model) and pass the instance to the `Details` view by taking advantage of `View Data`.</span></span>

<span data-ttu-id="2a689-148">Измененный `ProductController` в списке 3 включает в себя обновленные `Details()` действие, которое возвращает продукт.</span><span class="sxs-lookup"><span data-stu-id="2a689-148">The modified `ProductController` in Listing 3 includes an updated `Details()` action that returns a Product.</span></span>

<span data-ttu-id="2a689-149">**Листинг 3. `ProductController.vb`**</span><span class="sxs-lookup"><span data-stu-id="2a689-149">**Listing 3 – `ProductController.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample5.vb)]

<span data-ttu-id="2a689-150">Во-первых, `Details()` действие создает новый экземпляр `Product` класс, представляющий переносного компьютера.</span><span class="sxs-lookup"><span data-stu-id="2a689-150">First, the `Details()` action creates a new instance of the `Product` class that represents a laptop computer.</span></span> <span data-ttu-id="2a689-151">Далее, экземпляр `Product` класса передается в качестве второго параметра `View()` метод.</span><span class="sxs-lookup"><span data-stu-id="2a689-151">Next, the instance of the `Product` class is passed as the second parameter to the `View()` method.</span></span>

<span data-ttu-id="2a689-152">Можно создать модульные тесты для проверки, является ли ожидаемый набор данных содержатся в представлении данных.</span><span class="sxs-lookup"><span data-stu-id="2a689-152">You can write unit tests to test whether the expected data is contained in view data.</span></span> <span data-ttu-id="2a689-153">Платформа модульного тестирования в тестах со списком 4 ли продукта, представляющий переносной компьютер возвращается при вызове `ProductController Details()` метода действия.</span><span class="sxs-lookup"><span data-stu-id="2a689-153">The unit test in Listing 4 tests whether or not a Product representing a laptop computer is returned when you call the `ProductController Details()` action method.</span></span>

<span data-ttu-id="2a689-154">**Листинг 4. `ProductControllerTest.vb`**</span><span class="sxs-lookup"><span data-stu-id="2a689-154">**Listing 4 – `ProductControllerTest.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample6.vb)]

<span data-ttu-id="2a689-155">В листинге 4 `TestDetailsView()` метод проверяет данные представления, возвращаемые при вызове `Details()` метода.</span><span class="sxs-lookup"><span data-stu-id="2a689-155">In Listing 4, the `TestDetailsView()` method tests the View Data returned by invoking the `Details()` method.</span></span> <span data-ttu-id="2a689-156">`ViewData` Предоставляется как свойство `ViewResult` возвращенные вызовом `Details()` метод.</span><span class="sxs-lookup"><span data-stu-id="2a689-156">The `ViewData` is exposed as a property on the `ViewResult` returned by invoking the `Details()` method.</span></span> <span data-ttu-id="2a689-157">`ViewData.Model` Свойство содержит продукта, передаются в представление.</span><span class="sxs-lookup"><span data-stu-id="2a689-157">The `ViewData.Model` property contains the product passed to the view.</span></span> <span data-ttu-id="2a689-158">Тест просто проверяет, имеет ли продукт, содержащиеся в представлении данных имя переносного компьютера.</span><span class="sxs-lookup"><span data-stu-id="2a689-158">The test simply verifies that the product contained in the View Data has the name Laptop.</span></span>

## <a name="testing-the-action-result-returned-by-a-controller"></a><span data-ttu-id="2a689-159">Тестирование результата действия возвращается с помощью контроллера</span><span class="sxs-lookup"><span data-stu-id="2a689-159">Testing the Action Result returned by a Controller</span></span>

<span data-ttu-id="2a689-160">Более сложные действия контроллера могут возвращать различные типы результатов действий в зависимости от значения параметров, переданных для действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="2a689-160">A more complex controller action might return different types of action results depending on the values of the parameters passed to the controller action.</span></span> <span data-ttu-id="2a689-161">Действие контроллера может возвращать различные типы результатов действий, в том числе `ViewResult`, `RedirectToRouteResult`, или `JsonResult`.</span><span class="sxs-lookup"><span data-stu-id="2a689-161">A controller action can return a variety of types of action results including a `ViewResult`, `RedirectToRouteResult`, or `JsonResult`.</span></span>

<span data-ttu-id="2a689-162">Например, измененные `Details()` действие в листинге 5 возвращает `Details` просмотра при передаче соответствующим продуктом идентификатор действия.</span><span class="sxs-lookup"><span data-stu-id="2a689-162">For example, the modified `Details()` action in Listing 5 returns the `Details` view when you pass a valid product Id to the action.</span></span> <span data-ttu-id="2a689-163">Если передается недопустимый продукта идентификатор со значением идентификатора — меньше 1, то вы будете перенаправлены `Index()` действие.</span><span class="sxs-lookup"><span data-stu-id="2a689-163">If you pass an invalid product Id -- an Id with a value less than 1 -- then you are redirected to the `Index()` action.</span></span>

<span data-ttu-id="2a689-164">**Листинг 5. `ProductController.vb`**</span><span class="sxs-lookup"><span data-stu-id="2a689-164">**Listing 5 – `ProductController.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample7.vb)]

<span data-ttu-id="2a689-165">Можно проверить поведение `Details()` действие с модульным тестом в 6 со списком.</span><span class="sxs-lookup"><span data-stu-id="2a689-165">You can test the behavior of the `Details()` action with the unit test in Listing 6.</span></span> <span data-ttu-id="2a689-166">Модульный тест в список 6 проверяет, что вы будете перенаправлены на `Index` просмотра при передаче идентификатора со значением -1 `Details()` метод.</span><span class="sxs-lookup"><span data-stu-id="2a689-166">The unit test in Listing 6 verifies that you are redirected to the `Index` view when an Id with the value -1 is passed to the `Details()` method.</span></span>

<span data-ttu-id="2a689-167">**Перечисление 6. `ProductControllerTest.vb`**</span><span class="sxs-lookup"><span data-stu-id="2a689-167">**Listing 6 – `ProductControllerTest.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample8.vb)]

<span data-ttu-id="2a689-168">При вызове `RedirectToAction()` метод действия контроллера, действия контроллера возвращает `RedirectToRouteResult`.</span><span class="sxs-lookup"><span data-stu-id="2a689-168">When you call the `RedirectToAction()` method in a controller action, the controller action returns a `RedirectToRouteResult`.</span></span> <span data-ttu-id="2a689-169">Тест проверяет ли `RedirectToRouteResult` будет перенаправлять пользователя на действие контроллера, с именем `Index`.</span><span class="sxs-lookup"><span data-stu-id="2a689-169">The test checks whether the `RedirectToRouteResult` will redirect the user to a controller action named `Index`.</span></span>

## <a name="summary"></a><span data-ttu-id="2a689-170">Сводка</span><span class="sxs-lookup"><span data-stu-id="2a689-170">Summary</span></span>

<span data-ttu-id="2a689-171">В этом учебнике вы узнали, как для создания модульных тестов для действий контроллеров MVC.</span><span class="sxs-lookup"><span data-stu-id="2a689-171">In this tutorial, you learned how to build unit tests for MVC controller actions.</span></span> <span data-ttu-id="2a689-172">Во-первых вы узнали, как проверить, возвращается ли правильное представление с помощью действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="2a689-172">First, you learned how to verify whether the right view is returned by a controller action.</span></span> <span data-ttu-id="2a689-173">Вы узнали, как использовать `ViewResult.ViewName` свойство для проверки имени представления.</span><span class="sxs-lookup"><span data-stu-id="2a689-173">You learned how to use the `ViewResult.ViewName` property to verify the name of a view.</span></span>

<span data-ttu-id="2a689-174">Далее мы рассмотрели, как можно проверить содержимое `View Data`.</span><span class="sxs-lookup"><span data-stu-id="2a689-174">Next, we examined how you can test the contents of `View Data`.</span></span> <span data-ttu-id="2a689-175">Вы узнали, как проверить, возвращаются ли подходящее в `View Data` после вызова метода действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="2a689-175">You learned how to check whether the right product was returned in `View Data` after calling a controller action.</span></span>

<span data-ttu-id="2a689-176">Наконец мы рассмотрели, как можно проверить ли различные типы результатов действий возвращаются из действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="2a689-176">Finally, we discussed how you can test whether different types of action results are returned from a controller action.</span></span> <span data-ttu-id="2a689-177">Вы узнали, как проверить ли контроллер возвращает `ViewResult` или `RedirectToRouteResult`.</span><span class="sxs-lookup"><span data-stu-id="2a689-177">You learned how to test whether a controller returns a `ViewResult` or a `RedirectToRouteResult`.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="2a689-178">Назад</span><span class="sxs-lookup"><span data-stu-id="2a689-178">Previous</span></span>](creating-unit-tests-for-asp-net-mvc-applications-cs.md)
