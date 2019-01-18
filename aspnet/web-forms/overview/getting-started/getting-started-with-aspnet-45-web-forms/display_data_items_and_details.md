---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Отображение элементов данных и сведений | Документация Майкрософт
author: Erikre
description: В этой серии руководств показано, основы создания приложения веб-форм ASP.NET с помощью ASP.NET 4.7 и Microsoft Visual Studio 2017
ms.author: riande
ms.date: 1/04/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: acc2f8e78375ef0455d467e2af750ecbee623224
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396229"
---
<a name="display-data-items-and-details"></a><span data-ttu-id="a2801-103">Отображаемые элементы данных и сведения</span><span class="sxs-lookup"><span data-stu-id="a2801-103">Display data items and details</span></span>
====================
<span data-ttu-id="a2801-104">по [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="a2801-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

> <span data-ttu-id="a2801-105">В этой серии руководств представлены основы создания приложений веб-форм ASP.NET с помощью ASP.NET 4.7 и Microsoft Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="a2801-105">This tutorial series teaches you the basics of building an ASP.NET Web Forms application with ASP.NET 4.7 and Microsoft Visual Studio 2017.</span></span>

<span data-ttu-id="a2801-106">В этом руководстве вы узнаете, как отобразить элементы данных и сведений об элементе данных с помощью веб-форм ASP.NET и Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="a2801-106">In this tutorial, you'll learn how to display data items and data item details with ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="a2801-107">Этот учебник основан на предыдущем учебном курсе «И навигация по пользовательскому Интерфейсу» как часть этой серии руководств Store Toy Wingtip.</span><span class="sxs-lookup"><span data-stu-id="a2801-107">This tutorial builds on the previous "UI and Navigation" tutorial as part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="a2801-108">После завершения этого учебника, вы увидите продуктов на *ProductsList.aspx* страницы и сведения о продукте на *ProductDetails.aspx* страницы.</span><span class="sxs-lookup"><span data-stu-id="a2801-108">After completing this tutorial, you'll see products on the *ProductsList.aspx* page and a product's details on the *ProductDetails.aspx* page.</span></span>

## <a name="youll-learn-how-to"></a><span data-ttu-id="a2801-109">Вы научитесь:</span><span class="sxs-lookup"><span data-stu-id="a2801-109">You'll learn how to:</span></span>

- <span data-ttu-id="a2801-110">Добавьте элемент управления данные для отображения продуктов из базы данных</span><span class="sxs-lookup"><span data-stu-id="a2801-110">Add a data control to display products from the database</span></span>
- <span data-ttu-id="a2801-111">Подключения элемента управления данными для выбранных данных</span><span class="sxs-lookup"><span data-stu-id="a2801-111">Connect a data control to the selected data</span></span>
- <span data-ttu-id="a2801-112">Добавьте элемент управления данные для отображения сведений о продукте из базы данных</span><span class="sxs-lookup"><span data-stu-id="a2801-112">Add a data control to display product details from the database</span></span>
- <span data-ttu-id="a2801-113">Извлечь значение из строки запроса и использовать это значение, чтобы ограничить данные, которые извлекаются из базы данных</span><span class="sxs-lookup"><span data-stu-id="a2801-113">Retrieve a value from the query string and use that value to limit the data that's retrieved from the database</span></span>

### <a name="features-introduced-in-this-tutorial"></a><span data-ttu-id="a2801-114">Функции, представленных в этом руководстве:</span><span class="sxs-lookup"><span data-stu-id="a2801-114">Features introduced in this tutorial:</span></span>

- <span data-ttu-id="a2801-115">Привязка модели</span><span class="sxs-lookup"><span data-stu-id="a2801-115">Model binding</span></span>
- <span data-ttu-id="a2801-116">Поставщики значений</span><span class="sxs-lookup"><span data-stu-id="a2801-116">Value providers</span></span>

## <a name="add-a-data-control"></a><span data-ttu-id="a2801-117">Добавление данных элемента управления</span><span class="sxs-lookup"><span data-stu-id="a2801-117">Add a data control</span></span>

<span data-ttu-id="a2801-118">Можно использовать несколько вариантов для привязки данных к серверному элементу управления.</span><span class="sxs-lookup"><span data-stu-id="a2801-118">You can use a few different options to bind data to a server control.</span></span> <span data-ttu-id="a2801-119">Наиболее распространенные включают:</span><span class="sxs-lookup"><span data-stu-id="a2801-119">The most common include:</span></span>

 * <span data-ttu-id="a2801-120">Добавление элемента управления источника данных</span><span class="sxs-lookup"><span data-stu-id="a2801-120">Adding a data source control</span></span>
 * <span data-ttu-id="a2801-121">Добавление кода вручную</span><span class="sxs-lookup"><span data-stu-id="a2801-121">Adding code by hand</span></span>
 * <span data-ttu-id="a2801-122">С помощью привязки модели</span><span class="sxs-lookup"><span data-stu-id="a2801-122">Using model binding</span></span>

### <a name="use-a-data-source-control-to-bind-data"></a><span data-ttu-id="a2801-123">Используйте элемент управления источником данных для привязки данных</span><span class="sxs-lookup"><span data-stu-id="a2801-123">Use a data source control to bind data</span></span>

<span data-ttu-id="a2801-124">Добавление элемента управления источника данных позволяет связать элемент управления источником данных элемент управления, который отображает данные.</span><span class="sxs-lookup"><span data-stu-id="a2801-124">Adding a data source control allows you to link the data source control to the control that displays the data.</span></span> <span data-ttu-id="a2801-125">При таком подходе вы можно декларативно, а не программным образом, подключение серверных элементов управления к источникам данных.</span><span class="sxs-lookup"><span data-stu-id="a2801-125">With this approach, you can declaratively,  rather than programmatically, connect server-side controls to data sources.</span></span>

### <a name="code-by-hand-to-bind-data"></a><span data-ttu-id="a2801-126">Код вручную, для привязки данных</span><span class="sxs-lookup"><span data-stu-id="a2801-126">Code by hand to bind data</span></span>

<span data-ttu-id="a2801-127">Кодировании вручную включает в себя:</span><span class="sxs-lookup"><span data-stu-id="a2801-127">Coding by hand involves:</span></span>

1. <span data-ttu-id="a2801-128">Считывая значение</span><span class="sxs-lookup"><span data-stu-id="a2801-128">Reading a value</span></span>
2. <span data-ttu-id="a2801-129">Проверяется, является ли значение null</span><span class="sxs-lookup"><span data-stu-id="a2801-129">Checking if it's null</span></span>
3. <span data-ttu-id="a2801-130">Преобразование его в соответствующий тип</span><span class="sxs-lookup"><span data-stu-id="a2801-130">Converting it to an appropriate type</span></span>
4. <span data-ttu-id="a2801-131">Проверка успешного преобразования</span><span class="sxs-lookup"><span data-stu-id="a2801-131">Checking conversion success</span></span>
5. <span data-ttu-id="a2801-132">С помощью значения в запросе</span><span class="sxs-lookup"><span data-stu-id="a2801-132">Using the value in the query</span></span> 

<span data-ttu-id="a2801-133">Такой подход позволяет иметь полный контроль над логику доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="a2801-133">This approach lets you have full control over your data-access logic.</span></span>

### <a name="use-model-binding-to-bind-data"></a><span data-ttu-id="a2801-134">Использование привязки модели для привязки данных</span><span class="sxs-lookup"><span data-stu-id="a2801-134">Use model binding to bind data</span></span>

<span data-ttu-id="a2801-135">Привязка модели позволяет привязать результаты гораздо меньше кода и дает возможность повторно использовать функциональные возможности во всем приложении.</span><span class="sxs-lookup"><span data-stu-id="a2801-135">Model binding lets you bind results with far less code and gives you the ability to reuse the functionality throughout your application.</span></span> <span data-ttu-id="a2801-136">Она упрощает работу с логики доступа к данным на ориентированный на код по-прежнему предоставляя это богатый инфраструктура привязки данных.</span><span class="sxs-lookup"><span data-stu-id="a2801-136">It simplifies working with code-focused data-access logic while still providing a rich, data-binding framework.</span></span>

## <a name="display-products"></a><span data-ttu-id="a2801-137">Отобразить продукты</span><span class="sxs-lookup"><span data-stu-id="a2801-137">Display products</span></span>

<span data-ttu-id="a2801-138">В этом руководстве вы используете привязки модели для привязки данных.</span><span class="sxs-lookup"><span data-stu-id="a2801-138">In this tutorial, you'll use model binding to bind data.</span></span> <span data-ttu-id="a2801-139">Чтобы настроить элемент управления данные для использования привязки модели для выбора данных, значение элемента управления `SelectMethod` равным имени метода в код страницы.</span><span class="sxs-lookup"><span data-stu-id="a2801-139">To configure a data control to use model binding to select data, you set the control's `SelectMethod` property to a method name in the page's code.</span></span> <span data-ttu-id="a2801-140">Элемент управления данными вызывает метод в соответствующее время жизненного цикла страницы и автоматически связывает возвращаемых данных.</span><span class="sxs-lookup"><span data-stu-id="a2801-140">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="a2801-141">Нет необходимости явно вызвать `DataBind` метод.</span><span class="sxs-lookup"><span data-stu-id="a2801-141">There's no need to explicitly call the `DataBind` method.</span></span>

1. <span data-ttu-id="a2801-142">В **обозревателе решений**откройте *ProductList.aspx*.</span><span class="sxs-lookup"><span data-stu-id="a2801-142">In **Solution Explorer**, open *ProductList.aspx*.</span></span>
2. <span data-ttu-id="a2801-143">Замените существующую разметку с этой разметкой:</span><span class="sxs-lookup"><span data-stu-id="a2801-143">Replace the existing markup with this markup:</span></span>   

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

<span data-ttu-id="a2801-144">Этот код использует **ListView** управления с именем `productList` для отображения продуктов.</span><span class="sxs-lookup"><span data-stu-id="a2801-144">This code uses a **ListView** control named `productList` to display products.</span></span>

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

<span data-ttu-id="a2801-145">С помощью шаблонов и стилей, можно определить как **ListView** отображаются в элементе управления.</span><span class="sxs-lookup"><span data-stu-id="a2801-145">With templates and styles, you define how the **ListView** control displays data.</span></span> <span data-ttu-id="a2801-146">Это полезно для данных в любой повторяющейся структуре.</span><span class="sxs-lookup"><span data-stu-id="a2801-146">It's useful for data in any repeating structure.</span></span> <span data-ttu-id="a2801-147">Хотя это **ListView** примере просто отображает базы данных, кроме того, можно без кода, позволяют пользователям для редактирования, вставки и удаления данных и отсортировать и данные страницы.</span><span class="sxs-lookup"><span data-stu-id="a2801-147">Though this **ListView** example simply displays database data, you can also, without code, enable users to edit, insert, and delete data, and to sort and page data.</span></span>

<span data-ttu-id="a2801-148">Задав `ItemType` свойство в **ListView** управления, выражение привязки данных `Item` доступна и элемент управления становится строго типизированными.</span><span class="sxs-lookup"><span data-stu-id="a2801-148">By setting the `ItemType` property in the **ListView** control, the data-binding expression `Item` is available and the control becomes strongly typed.</span></span> <span data-ttu-id="a2801-149">Как упоминалось в предыдущем учебном курсе, можно выбрать сведения об элементе объекта с поддержкой технологии IntelliSense, например, указать `ProductName`:</span><span class="sxs-lookup"><span data-stu-id="a2801-149">As mentioned in the previous tutorial, you can select Item object details with IntelliSense, such as specifying the `ProductName`:</span></span>

![Отображение данных элементов и сведения о — IntelliSense](display_data_items_and_details/_static/image1.png)

<span data-ttu-id="a2801-151">Вы также используете привязки модели для указания `SelectMethod` значение.</span><span class="sxs-lookup"><span data-stu-id="a2801-151">You're also using model binding to specify a `SelectMethod` value.</span></span> <span data-ttu-id="a2801-152">Это значение (`GetProducts`) соответствует методу, вы добавите код запаздывает, для отображения продуктов на следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="a2801-152">This value (`GetProducts`) corresponds to the method you'll add to the code behind to display products in the next step.</span></span>

### <a name="add-code-to-display-products"></a><span data-ttu-id="a2801-153">Добавьте код для отображения продуктов</span><span class="sxs-lookup"><span data-stu-id="a2801-153">Add code to display products</span></span>

<span data-ttu-id="a2801-154">На этом шаге вы добавите код для заполнения **ListView** элемента управления с данными о продуктах из базы данных.</span><span class="sxs-lookup"><span data-stu-id="a2801-154">In this step, you'll add code to populate the **ListView** control with product data from the database.</span></span> <span data-ttu-id="a2801-155">Этот код поддерживает отображаются все продукты и отдельной категории продуктов.</span><span class="sxs-lookup"><span data-stu-id="a2801-155">The code supports showing all products and  individual category products.</span></span>

1. <span data-ttu-id="a2801-156">В **обозревателе решений**, щелкните правой кнопкой мыши *ProductList.aspx* , а затем выберите **Просмотр кода**.</span><span class="sxs-lookup"><span data-stu-id="a2801-156">In **Solution Explorer**, right-click *ProductList.aspx* and then select **View Code**.</span></span>
2. <span data-ttu-id="a2801-157">Замените существующий код в *ProductList.aspx.cs* файл с этим:</span><span class="sxs-lookup"><span data-stu-id="a2801-157">Replace the existing code in the *ProductList.aspx.cs* file with this:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

<span data-ttu-id="a2801-158">В следующем коде показано `GetProducts` метод, **ListView** элемента управления `ItemType` ссылается на свойство в *ProductList.aspx* страницы.</span><span class="sxs-lookup"><span data-stu-id="a2801-158">This code shows the `GetProducts` method that the **ListView** control's `ItemType` property references in the *ProductList.aspx* page.</span></span> <span data-ttu-id="a2801-159">Чтобы ограничить результаты для конкретной базы данных категории, код задает `categoryId` значение из значения строки запроса, передаваемые *ProductList.aspx* страницы при *ProductList.aspx* страница Переход к.</span><span class="sxs-lookup"><span data-stu-id="a2801-159">To limit the results to a specific database category, the code sets the `categoryId` value from the query string value passed to the *ProductList.aspx* page when the *ProductList.aspx* page is navigated to.</span></span> <span data-ttu-id="a2801-160">`QueryStringAttribute` В класс `System.Web.ModelBinding` пространство имен используется для получения значения переменной строки запроса `id`.</span><span class="sxs-lookup"><span data-stu-id="a2801-160">The `QueryStringAttribute` class in the `System.Web.ModelBinding` namespace is used to retrieve the value of the query string variable `id`.</span></span> <span data-ttu-id="a2801-161">Это указывает, что привязка модели к попытке привязать значение из строки запроса для `categoryId` параметра во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="a2801-161">This instructs model binding to try to bind a value from the query string to the `categoryId` parameter at run time.</span></span>

<span data-ttu-id="a2801-162">При передаче допустимую категорию как строку запроса на страницу, результаты запроса ограничиваются продуктов в базе данных, которые соответствуют `categoryId` значение.</span><span class="sxs-lookup"><span data-stu-id="a2801-162">When a valid category is passed as a query string to the page, the results of the query are limited to those products in the database that match the `categoryId` value.</span></span> <span data-ttu-id="a2801-163">Например если *ProductsList.aspx* это URL-адрес страницы:</span><span class="sxs-lookup"><span data-stu-id="a2801-163">For instance, if the *ProductsList.aspx* page URL is this:</span></span>


[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

<span data-ttu-id="a2801-164">На странице отображаются только те продукты, где `categoryId` равно `1`.</span><span class="sxs-lookup"><span data-stu-id="a2801-164">The page displays only the products where the `categoryId` equals `1`.</span></span>

<span data-ttu-id="a2801-165">Все продукты отображаются в том случае, если строка запроса отсутствует включается при *ProductList.aspx* вызова страницы.</span><span class="sxs-lookup"><span data-stu-id="a2801-165">All products are displayed if no query string is included when the *ProductList.aspx* page is called.</span></span>

<span data-ttu-id="a2801-166">Источники значений для этих методов, называются *значение поставщики* (такие как *QueryString*), и параметр атрибуты, которые указывают, какой поставщик значений для использования, называются *значение атрибуты поставщика* (такие как `id`).</span><span class="sxs-lookup"><span data-stu-id="a2801-166">The sources of values for these methods are referred to as *value providers* (such as *QueryString*), and the parameter attributes that indicate which value provider to use are referred to as *value provider attributes* (such as `id`).</span></span> <span data-ttu-id="a2801-167">ASP.NET включает в себя поставщики значений и соответствующих атрибутов для всех типичных источников данных, введенных пользователем в приложении веб-форм, например строки запроса, файлы cookie, значения формы, элементы управления, состояние представления, состояние сеанса и свойства профиля.</span><span class="sxs-lookup"><span data-stu-id="a2801-167">ASP.NET includes value providers and corresponding attributes for all of the typical sources of user input in a Web Forms application such as the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="a2801-168">Можно также написать пользовательские поставщики значений.</span><span class="sxs-lookup"><span data-stu-id="a2801-168">You can also write custom value providers.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="a2801-169">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="a2801-169">Run the application</span></span>

<span data-ttu-id="a2801-170">Запустите приложение и просмотреть все продукты или категории продуктов.</span><span class="sxs-lookup"><span data-stu-id="a2801-170">Run the application now to view all products or a category's products.</span></span>

1. <span data-ttu-id="a2801-171">Нажмите клавишу **F5** при работе в Visual Studio для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="a2801-171">Press **F5** while in Visual Studio to run the application.</span></span>  
   <span data-ttu-id="a2801-172">Откроется браузер и показывает *Default.aspx* страницы.</span><span class="sxs-lookup"><span data-stu-id="a2801-172">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="a2801-173">Выберите **автомобили** меню навигации категории продукта.</span><span class="sxs-lookup"><span data-stu-id="a2801-173">Select **Cars** from the product category navigation menu.</span></span>  
   <span data-ttu-id="a2801-174">*ProductList.aspx* страница отображает только **автомобили** категории продуктов.</span><span class="sxs-lookup"><span data-stu-id="a2801-174">The *ProductList.aspx* page displays showing only **Cars** category products.</span></span> <span data-ttu-id="a2801-175">Далее в этом руководстве описано как отображать сведения о продукте.</span><span class="sxs-lookup"><span data-stu-id="a2801-175">Later in this tutorial, you'll display product details.</span></span>  

    ![Отображение данных элементов и сведения о — автомобили](display_data_items_and_details/_static/image2.png)

3. <span data-ttu-id="a2801-177">Выберите **продуктов** меню навигации вверху.</span><span class="sxs-lookup"><span data-stu-id="a2801-177">Select **Products** from the navigation menu at the top.</span></span>  
   <span data-ttu-id="a2801-178">Опять же *ProductList.aspx* страница отображается, однако в настоящее время он показан весь список продуктов.</span><span class="sxs-lookup"><span data-stu-id="a2801-178">Again, the *ProductList.aspx* page is displayed, however this time it shows the entire list of products.</span></span>   

    ![Отображение данных элементов и сведения о — продуктов](display_data_items_and_details/_static/image3.png)

4. <span data-ttu-id="a2801-180">Закройте браузер и вернуться в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a2801-180">Close the browser and return to Visual Studio.</span></span>

### <a name="add-a-data-control-to-display-product-details"></a><span data-ttu-id="a2801-181">Добавьте элемент управления данные для отображения сведений о продукте</span><span class="sxs-lookup"><span data-stu-id="a2801-181">Add a data control to display product details</span></span>

<span data-ttu-id="a2801-182">Затем вы измените разметку в *ProductDetails.aspx* страницы, добавленные в предыдущем руководстве, чтобы отобразить сведения о конкретных продуктах.</span><span class="sxs-lookup"><span data-stu-id="a2801-182">Next, you'll modify the markup in the *ProductDetails.aspx* page that you added in the previous tutorial to display specific product information.</span></span>

1. <span data-ttu-id="a2801-183">В **обозревателе решений**откройте *ProductDetails.aspx*.</span><span class="sxs-lookup"><span data-stu-id="a2801-183">In **Solution Explorer**, open *ProductDetails.aspx*.</span></span>

2. <span data-ttu-id="a2801-184">Замените существующую разметку с этой разметкой:</span><span class="sxs-lookup"><span data-stu-id="a2801-184">Replace the existing markup with this markup:</span></span>

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

    <span data-ttu-id="a2801-185">Этот код использует **FormView** управления для отображения сведений определенного продукта.</span><span class="sxs-lookup"><span data-stu-id="a2801-185">This code uses a **FormView** control to display specific product details.</span></span> <span data-ttu-id="a2801-186">Эта разметка использует методы, такие как методы, используемые для отображения данных в *ProductList.aspx* страницы.</span><span class="sxs-lookup"><span data-stu-id="a2801-186">This markup uses methods like the methods used to display data in the *ProductList.aspx* page.</span></span> <span data-ttu-id="a2801-187">**FormView** элемент управления используется для отображения одной записи за раз из источника данных.</span><span class="sxs-lookup"><span data-stu-id="a2801-187">The **FormView** control is used to display a single record at a time from a data source.</span></span> <span data-ttu-id="a2801-188">При использовании **FormView** элемента управления, создавать шаблоны для отображения и редактирования значений с привязкой к данным.</span><span class="sxs-lookup"><span data-stu-id="a2801-188">When you use the **FormView** control, you create templates to display and edit data-bound values.</span></span> <span data-ttu-id="a2801-189">Эти шаблоны содержат элементы управления, выражения, привязки и форматирование, которое определить вид и функциональные возможности формы.</span><span class="sxs-lookup"><span data-stu-id="a2801-189">These templates contain controls, binding expressions, and formatting that define the form's look and functionality.</span></span>

<span data-ttu-id="a2801-190">Предыдущей разметки для подключения к базе данных требуется дополнительный код.</span><span class="sxs-lookup"><span data-stu-id="a2801-190">Connecting the previous markup to the database requires additional code.</span></span>

1. <span data-ttu-id="a2801-191">В **обозревателе решений**, щелкните правой кнопкой мыши *ProductDetails.aspx* и нажмите кнопку **Просмотр кода**.</span><span class="sxs-lookup"><span data-stu-id="a2801-191">In **Solution Explorer**, right-click *ProductDetails.aspx* and then click **View Code**.</span></span>  
   <span data-ttu-id="a2801-192">*ProductDetails.aspx.cs* отобразится файл.</span><span class="sxs-lookup"><span data-stu-id="a2801-192">The *ProductDetails.aspx.cs* file is displayed.</span></span>

2. <span data-ttu-id="a2801-193">Замените существующий код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="a2801-193">Replace the existing code with this code:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

<span data-ttu-id="a2801-194">Этот код проверяет наличие "`productID`" значения строки запроса.</span><span class="sxs-lookup"><span data-stu-id="a2801-194">This code checks for a "`productID`" query-string value.</span></span> <span data-ttu-id="a2801-195">Если значение допустимым строка запроса обнаруживается, отображается соответствующий продукт.</span><span class="sxs-lookup"><span data-stu-id="a2801-195">If a valid query-string value is found, the matching product is displayed.</span></span> <span data-ttu-id="a2801-196">Если не найдено в строке запроса или его значение является недопустимым, программы не отображается.</span><span class="sxs-lookup"><span data-stu-id="a2801-196">If the query-string isn't found, or its value isn't valid, no product is displayed.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="a2801-197">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="a2801-197">Run the application</span></span>

<span data-ttu-id="a2801-198">Теперь вы можете запустить приложение, чтобы просмотреть отдельные продукта, отображаемое в зависимости от идентификатора продукта.</span><span class="sxs-lookup"><span data-stu-id="a2801-198">Now you can run the application to see an individual product displayed based on product ID.</span></span>

1. <span data-ttu-id="a2801-199">Нажмите клавишу **F5** при работе в Visual Studio для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="a2801-199">Press **F5** while in Visual Studio to run the application.</span></span>  
   <span data-ttu-id="a2801-200">Откроется браузер и показывает *Default.aspx* страницы.</span><span class="sxs-lookup"><span data-stu-id="a2801-200">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="a2801-201">Выберите **лодок** меню навигации категории.</span><span class="sxs-lookup"><span data-stu-id="a2801-201">Select **Boats** from the category navigation menu.</span></span>  
   <span data-ttu-id="a2801-202">*ProductList.aspx* откроется страница.</span><span class="sxs-lookup"><span data-stu-id="a2801-202">The *ProductList.aspx* page is displayed.</span></span>

3. <span data-ttu-id="a2801-203">Выберите **яхту бумаги** из списка продуктов.</span><span class="sxs-lookup"><span data-stu-id="a2801-203">Select **Paper Boat** from the product list.</span></span>
   <span data-ttu-id="a2801-204">*ProductDetails.aspx* откроется страница.</span><span class="sxs-lookup"><span data-stu-id="a2801-204">The *ProductDetails.aspx* page is displayed.</span></span>

    ![Отображение данных элементов и сведения о — продуктов](display_data_items_and_details/_static/image4.png)
    
4. <span data-ttu-id="a2801-206">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="a2801-206">Close the browser.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="a2801-207">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a2801-207">Additional resources</span></span>

[<span data-ttu-id="a2801-208">Извлечение и отображение данных с помощью привязки модели и веб-форм</span><span class="sxs-lookup"><span data-stu-id="a2801-208">Retrieving and displaying data with model binding and web forms</span></span>](../../presenting-and-managing-data/model-binding/retrieving-data.md)

## <a name="next-steps"></a><span data-ttu-id="a2801-209">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="a2801-209">Next steps</span></span>

<span data-ttu-id="a2801-210">В этом руководстве вы добавили разметки и кода для отображения продуктов и сведения о продукте.</span><span class="sxs-lookup"><span data-stu-id="a2801-210">In this tutorial, you added markup and code to display products and product details.</span></span> <span data-ttu-id="a2801-211">Вы узнали о строго типизированные элементы управления данными, привязка модели и поставщики значений.</span><span class="sxs-lookup"><span data-stu-id="a2801-211">You learned about strongly typed data controls, model binding, and value providers.</span></span> <span data-ttu-id="a2801-212">В следующем руководстве вы добавите корзины для покупок в пример приложения Wingtip Toys.</span><span class="sxs-lookup"><span data-stu-id="a2801-212">In the next tutorial, you'll add a shopping cart to the Wingtip Toys sample application.</span></span> 

> [!div class="step-by-step"]
> <span data-ttu-id="a2801-213">[Назад](ui_and_navigation.md)
> [Вперед](shopping-cart.md)</span><span class="sxs-lookup"><span data-stu-id="a2801-213">[Previous](ui_and_navigation.md)
[Next](shopping-cart.md)</span></span>
