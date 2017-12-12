---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: "Отображать данные элементы и подробное описание | Документы Microsoft"
author: Erikre
description: "Этот учебник ряд описываются основы построения приложения веб-форм ASP.NET с помощью ASP.NET 4.5 и Microsoft Visual Studio Express 2013 для мы..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 809d7a9c21a3ddf5dfd07d079eb8fe0d1d81712d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="display-data-items-and-details"></a><span data-ttu-id="a618d-103">Отображать данные элементов и сведения</span><span class="sxs-lookup"><span data-stu-id="a618d-103">Display Data Items and Details</span></span>
====================
<span data-ttu-id="a618d-104">По [Эрик Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="a618d-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="a618d-105">[Загрузите образец проекта Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) или [загрузить электронную (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="a618d-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="a618d-106">Этот учебник ряд описываются основы построения с помощью ASP.NET 4.5 и Microsoft Visual Studio Express 2013 для веб-приложения веб-форм ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a618d-106">This tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="a618d-107">Visual Studio 2013 [проект с исходным кодом C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) доступен по следующему этого учебника ряда.</span><span class="sxs-lookup"><span data-stu-id="a618d-107">A Visual Studio 2013 [project with C# source code](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) is available to accompany this tutorial series.</span></span>


<span data-ttu-id="a618d-108">Этот учебник содержит сведения по отображению элементов данных и сведения о данных элемента, с помощью веб-форм ASP.NET и Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="a618d-108">This tutorial describes how to display data items and data item details using ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="a618d-109">Этот учебник основана на работу с предыдущим учебником «И навигация по пользовательскому Интерфейсу» и является частью учебника по магазину игрушек Wingtip ряда.</span><span class="sxs-lookup"><span data-stu-id="a618d-109">This tutorial builds on the previous tutorial "UI and Navigation" and is part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="a618d-110">После завершения этого учебника, можно будет увидеть продуктов на *ProductsList.aspx* страницы и подробные сведения об отдельных продуктов на *ProductDetails.aspx* страницы.</span><span class="sxs-lookup"><span data-stu-id="a618d-110">When you've completed this tutorial, you'll be able to see products on the *ProductsList.aspx* page and details about an individual product on the *ProductDetails.aspx* page.</span></span>

## <a name="what-youll-learn"></a><span data-ttu-id="a618d-111">Что вы узнаете следующее.</span><span class="sxs-lookup"><span data-stu-id="a618d-111">What you'll learn:</span></span>

- <span data-ttu-id="a618d-112">Инструкции по добавлению элемента управления данных для отображения продуктов из базы данных.</span><span class="sxs-lookup"><span data-stu-id="a618d-112">How to add a data control to display products from the database.</span></span>
- <span data-ttu-id="a618d-113">Способ подключения элемента управления данными для выбранных данных.</span><span class="sxs-lookup"><span data-stu-id="a618d-113">How to connect a data control to the selected data.</span></span>
- <span data-ttu-id="a618d-114">Инструкции по добавлению элемента управления данных для отображения сведений о продукте из базы данных.</span><span class="sxs-lookup"><span data-stu-id="a618d-114">How to add a data control to display product details from the database.</span></span>
- <span data-ttu-id="a618d-115">Как получить значение из строки запроса и использовать это значение для ограничения данных, которые извлекаются из базы данных.</span><span class="sxs-lookup"><span data-stu-id="a618d-115">How to retrieve a value from the query string and use that value to limit the data that's retrieved from the database.</span></span>

### <a name="these-are-the-features-introduced-in-the-tutorial"></a><span data-ttu-id="a618d-116">Существуют следующие функции, представленные в этом учебнике:</span><span class="sxs-lookup"><span data-stu-id="a618d-116">These are the features introduced in the tutorial:</span></span>

- <span data-ttu-id="a618d-117">Привязка модели</span><span class="sxs-lookup"><span data-stu-id="a618d-117">Model Binding</span></span>
- <span data-ttu-id="a618d-118">Поставщики значений</span><span class="sxs-lookup"><span data-stu-id="a618d-118">Value providers</span></span>

## <a name="adding-a-data-control-to-display-products"></a><span data-ttu-id="a618d-119">Добавление элемента управления данных для отображения продуктов</span><span class="sxs-lookup"><span data-stu-id="a618d-119">Adding a Data Control to Display Products</span></span>

<span data-ttu-id="a618d-120">При привязке данных к элементу управления сервера, существует несколько вариантов, которые можно использовать.</span><span class="sxs-lookup"><span data-stu-id="a618d-120">When binding data to a server control, there are a few different options you can use.</span></span> <span data-ttu-id="a618d-121">Основные параметры включают добавление элемента управления источником данных, добавления кода вручную или с использованием привязки модели.</span><span class="sxs-lookup"><span data-stu-id="a618d-121">The most common options include adding a data source control, adding code by hand, or using model binding.</span></span>

### <a name="using-a-data-source-control-to-bind-data"></a><span data-ttu-id="a618d-122">С помощью элемента управления источником данных для привязки данных</span><span class="sxs-lookup"><span data-stu-id="a618d-122">Using a Data Source Control to Bind Data</span></span>

<span data-ttu-id="a618d-123">Добавление элемента управления источником данных позволяет связать элемент управления, отображающий данные управления источником данных.</span><span class="sxs-lookup"><span data-stu-id="a618d-123">Adding a data source control allows you to link the data source control to the control that displays the data.</span></span> <span data-ttu-id="a618d-124">Такой подход дает возможность декларативно серверных элементов управления напрямую подключаться к источникам данных, а не используя программный подход.</span><span class="sxs-lookup"><span data-stu-id="a618d-124">This approach allows you to declaratively connect server-side controls directly to data sources, rather than using a programmatic approach.</span></span>

### <a name="coding-by-hand-to-bind-data"></a><span data-ttu-id="a618d-125">Кодирование вручную для привязки данных</span><span class="sxs-lookup"><span data-stu-id="a618d-125">Coding By Hand to Bind Data</span></span>

<span data-ttu-id="a618d-126">Включает в себя код вручную для добавления считывания значения, проверку значение null, предпринимается попытка преобразовать его в соответствующий тип, проверки, успешно ли выполнено преобразование и наконец, используя значение в запросе.</span><span class="sxs-lookup"><span data-stu-id="a618d-126">Adding code by hand involves reading the value, checking for a null value, attempting to convert it to the appropriate type, checking whether the conversion was successful, and finally, using the value in the query.</span></span> <span data-ttu-id="a618d-127">Этот подход применяется при необходимости сохранить полный контроль над логики доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="a618d-127">You would use this approach when you need to retain full control over your data-access logic.</span></span>

### <a name="using-model-binding-to-bind-data"></a><span data-ttu-id="a618d-128">С помощью модели привязки для привязки данных</span><span class="sxs-lookup"><span data-stu-id="a618d-128">Using Model Binding to Bind Data</span></span>

<span data-ttu-id="a618d-129">С помощью привязки модели позволяет привязать результатов с помощью гораздо меньше кода и дает возможность повторно использовать функциональные возможности во всем приложении.</span><span class="sxs-lookup"><span data-stu-id="a618d-129">Using model binding allows you to bind results using far less code and gives you the ability to reuse the functionality throughout your application.</span></span> <span data-ttu-id="a618d-130">Привязка модели предназначен для упрощения работы с логики доступа к данным на ориентированный на код в то же время сохраняя преимущества широкие возможности привязки к данным платформы.</span><span class="sxs-lookup"><span data-stu-id="a618d-130">Model binding aims to simplify working with code-focused data-access logic while still retaining the benefits of a rich, data-binding framework.</span></span>

## <a name="displaying-products"></a><span data-ttu-id="a618d-131">Отображение продуктов</span><span class="sxs-lookup"><span data-stu-id="a618d-131">Displaying Products</span></span>

<span data-ttu-id="a618d-132">В этом учебнике будет использоваться модель привязки для привязки данных.</span><span class="sxs-lookup"><span data-stu-id="a618d-132">In this tutorial, you'll use model binding to bind data.</span></span> <span data-ttu-id="a618d-133">Настройка элемента управления данных для использования привязки модели для выбора данных, задать для элемента управления `SelectMethod` на имя метода в коде страницы.</span><span class="sxs-lookup"><span data-stu-id="a618d-133">To configure a data control to use model binding to select data, you set the control's `SelectMethod` property to the name of a method in the page's code.</span></span> <span data-ttu-id="a618d-134">Элемент управления данных вызывает метод в соответствующее время жизненного цикла страницы и автоматически связывает возвращаемых данных.</span><span class="sxs-lookup"><span data-stu-id="a618d-134">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="a618d-135">Нет необходимости явно вызвать `DataBind` метод.</span><span class="sxs-lookup"><span data-stu-id="a618d-135">There's no need to explicitly call the `DataBind` method.</span></span>

<span data-ttu-id="a618d-136">Выполнив следующие действия, необходимо изменить разметку в *ProductList.aspx* страницы, чтобы страница может отобразить продукты.</span><span class="sxs-lookup"><span data-stu-id="a618d-136">Using the steps below, you'll modify the markup in the *ProductList.aspx* page so that the page can display products.</span></span>

1. <span data-ttu-id="a618d-137">В **обозревателе решений**откройте *ProductList.aspx* страницы.</span><span class="sxs-lookup"><span data-stu-id="a618d-137">In **Solution Explorer**, open the *ProductList.aspx* page.</span></span>
2. <span data-ttu-id="a618d-138">Замените существующую разметку следующей разметкой:</span><span class="sxs-lookup"><span data-stu-id="a618d-138">Replace the existing markup with the following markup:</span></span>   

    [!code-aspx[Main](display_data_items_and_details/samples/sample1.aspx)]

<span data-ttu-id="a618d-139">Этот код использует **ListView** элемент управления с именем «productList» для демонстрации продуктов.</span><span class="sxs-lookup"><span data-stu-id="a618d-139">This code uses a **ListView** control named "productList" to display the products.</span></span>

[!code-aspx[Main](display_data_items_and_details/samples/sample2.aspx)]

<span data-ttu-id="a618d-140">**ListView** элемент управления отображает данные в формате, который определяется с помощью шаблонов и стилей.</span><span class="sxs-lookup"><span data-stu-id="a618d-140">The **ListView** control displays data in a format that you define by using templates and styles.</span></span> <span data-ttu-id="a618d-141">Это полезно для данных в любой повторяющейся структуре.</span><span class="sxs-lookup"><span data-stu-id="a618d-141">It is useful for data in any repeating structure.</span></span> <span data-ttu-id="a618d-142">Это **ListView** просто пример данных из базы данных, однако можно включить пользователям для редактирования, вставки и удаления данных, а также сортировать и страницы данных, без использования кода.</span><span class="sxs-lookup"><span data-stu-id="a618d-142">This **ListView** example simply shows data from the database, however you can enable users to edit, insert, and delete data, and to sort and page data, all without code.</span></span>

<span data-ttu-id="a618d-143">Установив `ItemType` свойство в **ListView** управления выражение привязки данных `Item` доступна и элемент управления становится строго типизированные.</span><span class="sxs-lookup"><span data-stu-id="a618d-143">By setting the `ItemType` property in the **ListView** control, the data-binding expression `Item` is available and the control becomes strongly typed.</span></span> <span data-ttu-id="a618d-144">Как упоминалось в предыдущем учебнике, можно выбрать сведений элемента объекта, с помощью технологии IntelliSense, таких как задание `ProductName`:</span><span class="sxs-lookup"><span data-stu-id="a618d-144">As mentioned in the previous tutorial, you can select details of the Item object using IntelliSense, such as specifying the `ProductName`:</span></span>

![Отображение данных элементов и подробностей - IntelliSense](display_data_items_and_details/_static/image1.png)

<span data-ttu-id="a618d-146">Кроме того, привязка модели используется для указания `SelectMethod` значение.</span><span class="sxs-lookup"><span data-stu-id="a618d-146">In addition, you are using model binding to specify a `SelectMethod` value.</span></span> <span data-ttu-id="a618d-147">Это значение (`GetProducts`) будет соответствовать метод, который вы добавите кода программной части для отображения продуктов на следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="a618d-147">This value (`GetProducts`) will correspond to the method that you will add to the code behind to display products in the next step.</span></span>

### <a name="adding-code-to-display-products"></a><span data-ttu-id="a618d-148">Добавление кода для отображения продуктов</span><span class="sxs-lookup"><span data-stu-id="a618d-148">Adding Code to Display Products</span></span>

<span data-ttu-id="a618d-149">На этом шаге вы добавите код, чтобы заполнить **ListView** управления с данными о продуктах из базы данных.</span><span class="sxs-lookup"><span data-stu-id="a618d-149">In this step, you'll add code to populate the **ListView** control with product data from the database.</span></span> <span data-ttu-id="a618d-150">Код будет поддерживать отображение продукты по отдельной категории, отображающий все продукты.</span><span class="sxs-lookup"><span data-stu-id="a618d-150">The code will support showing products by individual category, as well as showing all products.</span></span>

1. <span data-ttu-id="a618d-151">В **обозревателе решений**, щелкните правой кнопкой мыши *ProductList.aspx* и нажмите кнопку **Просмотр кода**.</span><span class="sxs-lookup"><span data-stu-id="a618d-151">In **Solution Explorer**, right-click *ProductList.aspx* and then click **View Code**.</span></span>
2. <span data-ttu-id="a618d-152">Замените существующий код в *ProductList.aspx.cs* файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="a618d-152">Replace the existing code in the *ProductList.aspx.cs* file with the following code:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

<span data-ttu-id="a618d-153">Этот код показывает `GetProducts` метода, на который ссылается `ItemType` свойство **ListView** управления *ProductList.aspx* страницы.</span><span class="sxs-lookup"><span data-stu-id="a618d-153">This code shows the `GetProducts` method that's referenced by the `ItemType` property of the **ListView** control in the *ProductList.aspx* page.</span></span> <span data-ttu-id="a618d-154">Чтобы ограничить результаты к определенной категории в базе данных, в коде задается `categoryId` значение из значения строки запроса, передаваемый *ProductList.aspx* странице, если *ProductList.aspx* страница является Переход к.</span><span class="sxs-lookup"><span data-stu-id="a618d-154">To limit the results to a specific category in the database, the code sets the `categoryId` value from the query string value passed to the *ProductList.aspx* page when the *ProductList.aspx* page is navigated to.</span></span> <span data-ttu-id="a618d-155">`QueryStringAttribute` Класса в `System.Web.ModelBinding` пространства имен используется для получения значения идентификатора переменной строки запроса. Это указывает, что привязка модели для привязки значение из строки запроса для `categoryId` параметра во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="a618d-155">The `QueryStringAttribute` class in the `System.Web.ModelBinding` namespace is used to retrieve the value of the query string variable id. This instructs model binding to try to bind a value from the query string to the `categoryId` parameter at run time.</span></span>

<span data-ttu-id="a618d-156">Когда допустимую категорию передается на страницу в качестве строки запроса, результаты запроса ограничены этих продуктов в базе данных, которые соответствуют `categoryId` значение.</span><span class="sxs-lookup"><span data-stu-id="a618d-156">When a valid category is passed as a query string to the page, the results of the query are limited to those products in the database that match the `categoryId` value.</span></span> <span data-ttu-id="a618d-157">Например если URL-адрес *ProductsList.aspx* страница находится в следующем:</span><span class="sxs-lookup"><span data-stu-id="a618d-157">For instance, if the URL to the *ProductsList.aspx* page is the following:</span></span>

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

<span data-ttu-id="a618d-158">На странице отображаются только те продукты, где `category` равняется `1`.</span><span class="sxs-lookup"><span data-stu-id="a618d-158">The page displays only the products where the `category` equals `1`.</span></span>

<span data-ttu-id="a618d-159">Если строка запроса не включен, при переходе к *ProductList.aspx* страницы, отображаются все продукты.</span><span class="sxs-lookup"><span data-stu-id="a618d-159">If no query string is included when navigating to the *ProductList.aspx* page, all products will be displayed.</span></span>

<span data-ttu-id="a618d-160">Источники значений для этих методов, называются *значение поставщики* (такие как *QueryString*), и атрибуты параметра, которые указывают, какой поставщик значений для использования, рассматриваются как значения атрибуты поставщика (например, «`id`»).</span><span class="sxs-lookup"><span data-stu-id="a618d-160">The sources of values for these methods are referred to as *value providers* (such as *QueryString*), and the parameter attributes that indicate which value provider to use are referred to as value provider attributes (such as "`id`").</span></span> <span data-ttu-id="a618d-161">ASP.NET содержит поставщики значений и соответствующих атрибутов для всех обычные параметры вводимых пользователем данных в приложении Web Forms, такие как строка запроса, файлы cookie, значения формы, элементы управления, состояние представления, состояние сеанса и свойства профиля.</span><span class="sxs-lookup"><span data-stu-id="a618d-161">ASP.NET includes value providers and corresponding attributes for all of the typical sources of user input in a Web Forms application, such as the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="a618d-162">Можно также написать поставщиках пользовательских значений.</span><span class="sxs-lookup"><span data-stu-id="a618d-162">You can also write custom value providers.</span></span>

### <a name="running-the-application"></a><span data-ttu-id="a618d-163">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="a618d-163">Running the Application</span></span>

<span data-ttu-id="a618d-164">Запустите приложение сейчас, чтобы увидеть, как можно просмотреть все продукты или просто набор продуктов, ограниченные по категориям.</span><span class="sxs-lookup"><span data-stu-id="a618d-164">Run the application now to see how you can view all of the products or just a set of products limited by category.</span></span>

1. <span data-ttu-id="a618d-165">В **обозревателе решений**, щелкните правой кнопкой мыши *Default.aspx* и выберите **Просмотр в браузере**.</span><span class="sxs-lookup"><span data-stu-id="a618d-165">In the **Solution Explorer**, right-click the *Default.aspx* page and select **View in Browser**.</span></span>  
 <span data-ttu-id="a618d-166">Обозреватель и Показать *Default.aspx* страницы.</span><span class="sxs-lookup"><span data-stu-id="a618d-166">The browser will open and show the *Default.aspx* page.</span></span>
2. <span data-ttu-id="a618d-167">Выберите **автомобилей** меню навигации категории продукта.</span><span class="sxs-lookup"><span data-stu-id="a618d-167">Select **Cars** from the product category navigation menu.</span></span>  
 <span data-ttu-id="a618d-168">*ProductList.aspx* страница отображается только продуктами, включенных в категорию «Автомобили».</span><span class="sxs-lookup"><span data-stu-id="a618d-168">The *ProductList.aspx* page is displayed showing only products included in the "Cars" category.</span></span> <span data-ttu-id="a618d-169">Далее в этом учебнике будут отображаться сведения о продукте.</span><span class="sxs-lookup"><span data-stu-id="a618d-169">Later in this tutorial, you will display product details.</span></span>  

    ![Отображение данных элементов и подробностей - автомобили](display_data_items_and_details/_static/image2.png)
3. <span data-ttu-id="a618d-171">Выберите **продуктов** меню навигации в верхней.</span><span class="sxs-lookup"><span data-stu-id="a618d-171">Select **Products** from the navigation menu at the top.</span></span>  
 <span data-ttu-id="a618d-172">Опять же *ProductList.aspx* отображается страница, однако в данный момент показан весь список продуктов.</span><span class="sxs-lookup"><span data-stu-id="a618d-172">Again, the *ProductList.aspx* page is displayed, however this time it shows the entire list of products.</span></span>   

    ![Отображение данных элементов и подробностей - продуктов](display_data_items_and_details/_static/image3.png)
4. <span data-ttu-id="a618d-174">Закройте окно браузера и вернитесь в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a618d-174">Close the browser and return to Visual Studio.</span></span>

### <a name="adding-a-data-control-to-display-product-details"></a><span data-ttu-id="a618d-175">Добавление данных управления для отображения сведений о продукте</span><span class="sxs-lookup"><span data-stu-id="a618d-175">Adding a Data Control to Display Product Details</span></span>

<span data-ttu-id="a618d-176">Далее предстоит изменить разметку в *ProductDetails.aspx* страницы, добавленный в предыдущем учебнике, чтобы страницу можно отобразить сведения о отдельного продукта.</span><span class="sxs-lookup"><span data-stu-id="a618d-176">Next, you'll modify the markup in the *ProductDetails.aspx* page that you added in the previous tutorial so that the page can display information about an individual product.</span></span>

1. <span data-ttu-id="a618d-177">В **обозревателе решений**откройте *ProductDetails.aspx* страницы.</span><span class="sxs-lookup"><span data-stu-id="a618d-177">In **Solution Explorer**, open the *ProductDetails.aspx* page.</span></span>
2. <span data-ttu-id="a618d-178">Замените существующую разметку следующей разметкой:</span><span class="sxs-lookup"><span data-stu-id="a618d-178">Replace the existing markup with the following markup:</span></span>   

    [!code-aspx[Main](display_data_items_and_details/samples/sample5.aspx)]

<span data-ttu-id="a618d-179">Этот код использует **FormView** элемента управления для отображения сведений о отдельного продукта.</span><span class="sxs-lookup"><span data-stu-id="a618d-179">This code uses a **FormView** control to display details about an individual product.</span></span> <span data-ttu-id="a618d-180">Эта разметка использует методы, такие как те, которые используются для отображения данных в *ProductList.aspx* страницы.</span><span class="sxs-lookup"><span data-stu-id="a618d-180">This markup uses methods like those that are used to display data in the *ProductList.aspx* page.</span></span> <span data-ttu-id="a618d-181">**FormView** элемент управления используется для отображения одной записи за раз из источника данных.</span><span class="sxs-lookup"><span data-stu-id="a618d-181">The **FormView** control is used to display a single record at a time from a data source.</span></span> <span data-ttu-id="a618d-182">При использовании **FormView** управления, создавать шаблоны для отображения и редактирования значений с привязкой к данным.</span><span class="sxs-lookup"><span data-stu-id="a618d-182">When you use the **FormView** control, you create templates to display and edit data-bound values.</span></span> <span data-ttu-id="a618d-183">Шаблоны содержат элементы управления, привязка выражения и форматирование, определяющие внешний вид и функциональные возможности формы.</span><span class="sxs-lookup"><span data-stu-id="a618d-183">The templates contain controls, binding expressions, and formatting that define the look and functionality of the form.</span></span>

<span data-ttu-id="a618d-184">Чтобы соединиться с базой данных выше разметки, необходимо добавить дополнительный код для *ProductDetails.aspx* кода.</span><span class="sxs-lookup"><span data-stu-id="a618d-184">To connect the above markup to the database, you must add additional code to the *ProductDetails.aspx* code.</span></span>

1. <span data-ttu-id="a618d-185">В **обозревателе решений**, щелкните правой кнопкой мыши *ProductDetails.aspx* и нажмите кнопку **Просмотр кода**.</span><span class="sxs-lookup"><span data-stu-id="a618d-185">In **Solution Explorer**, right-click *ProductDetails.aspx* and then click **View Code**.</span></span>  
 <span data-ttu-id="a618d-186">*ProductDetails.aspx.cs* будет отображен файл.</span><span class="sxs-lookup"><span data-stu-id="a618d-186">The *ProductDetails.aspx.cs* file will be displayed.</span></span>
2. <span data-ttu-id="a618d-187">Замените существующий код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="a618d-187">Replace the existing code with the following code:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

<span data-ttu-id="a618d-188">Этот код проверяет наличие «`productID`» значение строки запроса.</span><span class="sxs-lookup"><span data-stu-id="a618d-188">This code checks for a "`productID`" query-string value.</span></span> <span data-ttu-id="a618d-189">Если обнаружено значение допустимые строки запроса, отображается соответствующий продукт.</span><span class="sxs-lookup"><span data-stu-id="a618d-189">If a valid query-string value is found, the matching product is displayed.</span></span> <span data-ttu-id="a618d-190">Если строка запроса не найдена или недопустимое значение строки запроса, ни один продукт не отображается на *ProductDetails.aspx* страницы.</span><span class="sxs-lookup"><span data-stu-id="a618d-190">If no query-string is found, or the query-string value is not valid, no product is displayed on the *ProductDetails.aspx* page.</span></span>

### <a name="running-the-application"></a><span data-ttu-id="a618d-191">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="a618d-191">Running the Application</span></span>

<span data-ttu-id="a618d-192">Теперь можно запустить приложение, чтобы увидеть отдельные продукта, отображаемое на основе идентификатора продукта.</span><span class="sxs-lookup"><span data-stu-id="a618d-192">Now you can run the application to see an individual product displayed based on the id of the product.</span></span>

1. <span data-ttu-id="a618d-193">Нажмите клавишу **F5** при работе в Visual Studio для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="a618d-193">Press **F5** while in Visual Studio to run the application.</span></span>  
 <span data-ttu-id="a618d-194">Обозреватель и Показать *Default.aspx* страницы.</span><span class="sxs-lookup"><span data-stu-id="a618d-194">The browser will open and show the *Default.aspx* page.</span></span>
2. <span data-ttu-id="a618d-195">Выберите «Лодок» из меню навигации по категории.</span><span class="sxs-lookup"><span data-stu-id="a618d-195">Select "Boats" from the category navigation menu.</span></span>  
 <span data-ttu-id="a618d-196">*ProductList.aspx* -страница.</span><span class="sxs-lookup"><span data-stu-id="a618d-196">The *ProductList.aspx* page is displayed.</span></span>
3. <span data-ttu-id="a618d-197">Выберите «Boat бумаги» продукт из списка продуктов.</span><span class="sxs-lookup"><span data-stu-id="a618d-197">Select the "Paper Boat" product from the product list.</span></span>  
 <span data-ttu-id="a618d-198">*ProductDetails.aspx* -страница.</span><span class="sxs-lookup"><span data-stu-id="a618d-198">The *ProductDetails.aspx* page is displayed.</span></span>   

    ![Отображение данных элементов и подробностей - продуктов](display_data_items_and_details/_static/image4.png)
4. <span data-ttu-id="a618d-200">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="a618d-200">Close the browser.</span></span>

## <a name="summary"></a><span data-ttu-id="a618d-201">Сводка</span><span class="sxs-lookup"><span data-stu-id="a618d-201">Summary</span></span>

<span data-ttu-id="a618d-202">В этом учебнике ряды имеют добавить разметку и код для отображения списка продуктов и для отображения сведений о продукте.</span><span class="sxs-lookup"><span data-stu-id="a618d-202">In this tutorial of the series you have add markup and code to display a product list and to display product details.</span></span> <span data-ttu-id="a618d-203">Во время этого процесса вы узнали о строго типизированные данные элементы управления, привязка модели и поставщиков значений.</span><span class="sxs-lookup"><span data-stu-id="a618d-203">During this process you have learned about strongly typed data controls, model binding, and value providers.</span></span> <span data-ttu-id="a618d-204">В следующем уроке вы добавите корзины в пример приложения Wingtip Toys.</span><span class="sxs-lookup"><span data-stu-id="a618d-204">In the next tutorial, you'll add a shopping cart to the Wingtip Toys sample application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a618d-205">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a618d-205">Additional Resources</span></span>

[<span data-ttu-id="a618d-206">Извлечение и отображение данных с помощью привязки модели и веб-форм</span><span class="sxs-lookup"><span data-stu-id="a618d-206">Retrieving and displaying data with model binding and web forms</span></span>](../../presenting-and-managing-data/model-binding/retrieving-data.md)

>[!div class="step-by-step"]
<span data-ttu-id="a618d-207">[Назад](ui_and_navigation.md)
[Вперед](shopping-cart.md)</span><span class="sxs-lookup"><span data-stu-id="a618d-207">[Previous](ui_and_navigation.md)
[Next](shopping-cart.md)</span></span>
