---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Отображение элементов данных и сведений | Документация Майкрософт
author: Erikre
description: В этой серии руководств будет основы построения для веб-приложения веб-форм ASP.NET с помощью ASP.NET 4.7 и Microsoft Visual Studio Community 2017
ms.author: riande
ms.date: 1/09/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 73ae1660f5d6e3e28c1c155e745a62936e3502df
ms.sourcegitcommit: cec77d5ad8a0cedb1ecbec32834111492afd0cd2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207438"
---
<a name="display-data-items-and-details"></a><span data-ttu-id="7453e-103">Отображаемые элементы данных и сведения</span><span class="sxs-lookup"><span data-stu-id="7453e-103">Display data items and details</span></span>
====================
<span data-ttu-id="7453e-104">по [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="7453e-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

> <span data-ttu-id="7453e-105">В этой серии руководств представлены основы построения для веб-приложения веб-форм ASP.NET с помощью ASP.NET 4.7 и Microsoft Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="7453e-105">This tutorial series teaches you the basics of building an ASP.NET Web Forms application with ASP.NET 4.7 and Microsoft Visual Studio Community 2017 for the Web.</span></span>

<span data-ttu-id="7453e-106">В этом руководстве вы узнаете, как отобразить элементы данных и сведений об элементе данных с помощью веб-форм ASP.NET и Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="7453e-106">In this tutorial, you learn how to display data items and data item details with ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="7453e-107">Этот учебник основан на предыдущем учебном курсе «И навигация по пользовательскому Интерфейсу» как часть этой серии руководств Store Toy Wingtip.</span><span class="sxs-lookup"><span data-stu-id="7453e-107">This tutorial builds on the previous "UI and Navigation" tutorial as part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="7453e-108">В этом руководстве завершенных продуктов на *ProductsList.aspx* страницы и сведения о продукте на *ProductDetails.aspx* отображаются страницы.</span><span class="sxs-lookup"><span data-stu-id="7453e-108">In the completed tutorial, products on the *ProductsList.aspx* page and a product's details on the *ProductDetails.aspx* page are displayed.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="7453e-109">Вы узнаете</span><span class="sxs-lookup"><span data-stu-id="7453e-109">What you learn</span></span>

- <span data-ttu-id="7453e-110">Добавьте элемент управления данные для отображения продуктов базы данных.</span><span class="sxs-lookup"><span data-stu-id="7453e-110">Add a data control to display database products.</span></span>
- <span data-ttu-id="7453e-111">Подключения элемента управления данными для выбранных данных.</span><span class="sxs-lookup"><span data-stu-id="7453e-111">Connect a data control to selected data.</span></span>
- <span data-ttu-id="7453e-112">Добавьте элемент управления данные для отображения сведений о продукте.</span><span class="sxs-lookup"><span data-stu-id="7453e-112">Add a data control to display product details.</span></span>
- <span data-ttu-id="7453e-113">Синтаксический анализ значения строки запроса и использовать его для фильтрации извлеченных данных базы данных.</span><span class="sxs-lookup"><span data-stu-id="7453e-113">Parse a query string value and use it to filter retrieved database data.</span></span>

<span data-ttu-id="7453e-114">Функции, представленных в этом руководстве включают привязки модели и поставщики значений.</span><span class="sxs-lookup"><span data-stu-id="7453e-114">Features introduced in this tutorial include model binding and value providers.</span></span>

## <a name="add-a-data-control-to-display-products"></a><span data-ttu-id="7453e-115">Добавьте элемент управления данные для отображения продуктов</span><span class="sxs-lookup"><span data-stu-id="7453e-115">Add a data control to display products</span></span>
 
<span data-ttu-id="7453e-116">У вас есть несколько вариантов для привязки данных к серверному элементу управления.</span><span class="sxs-lookup"><span data-stu-id="7453e-116">You have a few options to bind data to a server control.</span></span> <span data-ttu-id="7453e-117">Наиболее распространенные включают:</span><span class="sxs-lookup"><span data-stu-id="7453e-117">The most common include:</span></span>

 * <span data-ttu-id="7453e-118">Добавление элемента управления источника данных</span><span class="sxs-lookup"><span data-stu-id="7453e-118">Adding a data source control</span></span>
 * <span data-ttu-id="7453e-119">Добавление кода вручную</span><span class="sxs-lookup"><span data-stu-id="7453e-119">Adding code by hand</span></span>
 * <span data-ttu-id="7453e-120">Реализация привязки модели</span><span class="sxs-lookup"><span data-stu-id="7453e-120">Implementing model binding</span></span>

### <a name="use-a-data-source-control-to-bind-data"></a><span data-ttu-id="7453e-121">Используйте элемент управления источником данных для привязки данных</span><span class="sxs-lookup"><span data-stu-id="7453e-121">Use a data source control to bind data</span></span>

<span data-ttu-id="7453e-122">Добавление элемента управления источника данных связывает элемент управления источником данных элемент управления, который отображает данные.</span><span class="sxs-lookup"><span data-stu-id="7453e-122">Adding a data source control links the data source control to the control that displays the data.</span></span> <span data-ttu-id="7453e-123">При таком подходе вы можно декларативно, а не программным образом, подключение серверных элементов управления к источникам данных.</span><span class="sxs-lookup"><span data-stu-id="7453e-123">With this approach, you can declaratively, rather than programmatically, connect server-side controls to data sources.</span></span>

### <a name="code-by-hand-to-bind-data"></a><span data-ttu-id="7453e-124">Код вручную, для привязки данных</span><span class="sxs-lookup"><span data-stu-id="7453e-124">Code by hand to bind data</span></span>

<span data-ttu-id="7453e-125">Кодировании вручную включает в себя:</span><span class="sxs-lookup"><span data-stu-id="7453e-125">Coding by hand involves:</span></span>

1. <span data-ttu-id="7453e-126">Считывая значение</span><span class="sxs-lookup"><span data-stu-id="7453e-126">Reading a value</span></span>
2. <span data-ttu-id="7453e-127">Проверяется, является ли значение null</span><span class="sxs-lookup"><span data-stu-id="7453e-127">Checking if it's null</span></span>
3. <span data-ttu-id="7453e-128">Преобразование его в соответствующий тип</span><span class="sxs-lookup"><span data-stu-id="7453e-128">Converting it to an appropriate type</span></span>
4. <span data-ttu-id="7453e-129">Проверка успешного преобразования</span><span class="sxs-lookup"><span data-stu-id="7453e-129">Checking conversion success</span></span>
5. <span data-ttu-id="7453e-130">Что делает запрос с преобразованное значение</span><span class="sxs-lookup"><span data-stu-id="7453e-130">Making a query with the converted value</span></span> 

<span data-ttu-id="7453e-131">В этом случае у вас есть полный контроль над логику доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="7453e-131">With this approach, you have full control over your data-access logic.</span></span>

### <a name="use-model-binding-to-bind-data"></a><span data-ttu-id="7453e-132">Использование привязки модели для привязки данных</span><span class="sxs-lookup"><span data-stu-id="7453e-132">Use model binding to bind data</span></span>

<span data-ttu-id="7453e-133">С помощью привязки модели, привязать результаты гораздо меньше кода, и дает возможность повторно использовать функциональные возможности в приложении.</span><span class="sxs-lookup"><span data-stu-id="7453e-133">With model binding, you bind results with far less code and it gives you the ability to reuse the functionality throughout your application.</span></span> <span data-ttu-id="7453e-134">Она упрощает работу с логики доступа к данным на ориентированный на код по-прежнему предоставляя это богатый инфраструктура привязки данных.</span><span class="sxs-lookup"><span data-stu-id="7453e-134">It simplifies working with code-focused data-access logic while still providing a rich, data-binding framework.</span></span>

## <a name="display-products"></a><span data-ttu-id="7453e-135">Отобразить продукты</span><span class="sxs-lookup"><span data-stu-id="7453e-135">Display products</span></span>

<span data-ttu-id="7453e-136">В этом руководстве используется привязка модели для привязки данных.</span><span class="sxs-lookup"><span data-stu-id="7453e-136">In this tutorial, you use model binding to bind data.</span></span> <span data-ttu-id="7453e-137">Чтобы настроить элемент управления данные для использования привязки модели для выбора данных, значение элемента управления `SelectMethod` свойство к методу в коде страницы.</span><span class="sxs-lookup"><span data-stu-id="7453e-137">To configure a data control to use model binding to select data, you set the control's `SelectMethod` property to a method in the page's code.</span></span> <span data-ttu-id="7453e-138">Элемент управления данными вызывает метод в соответствующее время жизненного цикла страницы и автоматически связывает возвращаемых данных.</span><span class="sxs-lookup"><span data-stu-id="7453e-138">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="7453e-139">Нет необходимости явно вызвать `DataBind` метод.</span><span class="sxs-lookup"><span data-stu-id="7453e-139">There's no need to explicitly call the `DataBind` method.</span></span>

<span data-ttu-id="7453e-140">Работа следующие этапы внесения изменений в *ProductList.aspx* разметку для отображения продуктов.</span><span class="sxs-lookup"><span data-stu-id="7453e-140">Working through the following steps, you modify *ProductList.aspx* markup to display products.</span></span>

1. <span data-ttu-id="7453e-141">В **обозревателе решений**откройте *ProductList.aspx*.</span><span class="sxs-lookup"><span data-stu-id="7453e-141">In **Solution Explorer**, open *ProductList.aspx*.</span></span>

2. <span data-ttu-id="7453e-142">Замените существующую разметку следующей разметкой:</span><span class="sxs-lookup"><span data-stu-id="7453e-142">Replace the existing markup with the following markup:</span></span> 

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

<span data-ttu-id="7453e-143">Предыдущая разметка использует **ListView** управления с именем `productList` для отображения продуктов.</span><span class="sxs-lookup"><span data-stu-id="7453e-143">The preceding markup uses a **ListView** control named `productList` to display products.</span></span>

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

<span data-ttu-id="7453e-144">С помощью шаблонов и стилей, можно определить как **ListView** отображаются в элементе управления.</span><span class="sxs-lookup"><span data-stu-id="7453e-144">With templates and styles, you define how the **ListView** control displays data.</span></span> <span data-ttu-id="7453e-145">Это полезно для данных в любой повторяющейся структуре.</span><span class="sxs-lookup"><span data-stu-id="7453e-145">It's useful for data in any repeating structure.</span></span> <span data-ttu-id="7453e-146">Хотя это **ListView** примере просто отображает базы данных, кроме того, можно без кода, позволяют пользователям для редактирования, вставки и удаления данных и отсортировать и данные страницы.</span><span class="sxs-lookup"><span data-stu-id="7453e-146">Though this **ListView** example simply displays database data, you can also, without code, enable users to edit, insert, and delete data, and to sort and page data.</span></span>

<span data-ttu-id="7453e-147">При задании `ItemType` свойства в **ListView** управления, выражение привязки данных `Item` доступна и элемент управления становится строго типизированными.</span><span class="sxs-lookup"><span data-stu-id="7453e-147">When you set the `ItemType` property in the **ListView** control, the data-binding expression `Item` is available and the control becomes strongly typed.</span></span> <span data-ttu-id="7453e-148">Как упоминалось в предыдущем учебном курсе, можно выбрать сведения об элементе объекта с поддержкой технологии IntelliSense, например, указать `ProductName`:</span><span class="sxs-lookup"><span data-stu-id="7453e-148">As mentioned in the previous tutorial, you can select Item object details with IntelliSense, such as specifying the `ProductName`:</span></span>

![Отображение данных элементов и сведения о — IntelliSense](display_data_items_and_details/_static/image1.png)

<span data-ttu-id="7453e-150">С помощью привязки модели, при указании `SelectMethod` значение (`GetProducts`).</span><span class="sxs-lookup"><span data-stu-id="7453e-150">With model binding, you're specifying a `SelectMethod` value (`GetProducts`).</span></span> <span data-ttu-id="7453e-151">Это метод, добавьте в код запаздывает, для отображения продуктов на следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="7453e-151">This is the method you add to the code behind to display products in the next step.</span></span>

### <a name="add-code-to-display-products"></a><span data-ttu-id="7453e-152">Добавьте код для отображения продуктов</span><span class="sxs-lookup"><span data-stu-id="7453e-152">Add code to display products</span></span>

<span data-ttu-id="7453e-153">На этом шаге, добавьте код для заполнения **ListView** элемента управления данными продукта базы данных.</span><span class="sxs-lookup"><span data-stu-id="7453e-153">In this step, you add code to populate the **ListView** control with database product data.</span></span> <span data-ttu-id="7453e-154">Этот код поддерживает отображаются все продукты и отдельной категории продуктов.</span><span class="sxs-lookup"><span data-stu-id="7453e-154">The code supports showing all products and individual category products.</span></span>

1. <span data-ttu-id="7453e-155">В **обозревателе решений**, щелкните правой кнопкой мыши *ProductList.aspx* , а затем выберите **Просмотр кода**.</span><span class="sxs-lookup"><span data-stu-id="7453e-155">In **Solution Explorer**, right-click *ProductList.aspx* and then select **View Code**.</span></span>
2. <span data-ttu-id="7453e-156">Замените существующий код в *ProductList.aspx.cs* файл с этим:</span><span class="sxs-lookup"><span data-stu-id="7453e-156">Replace the existing code in the *ProductList.aspx.cs* file with this:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

<span data-ttu-id="7453e-157">В следующем коде показано `GetProducts` метод, **ListView** элемента управления `ItemType` ссылается на свойство в *ProductList.aspx*.</span><span class="sxs-lookup"><span data-stu-id="7453e-157">This code shows the `GetProducts` method that the **ListView** control's `ItemType` property references in *ProductList.aspx*.</span></span> <span data-ttu-id="7453e-158">Чтобы ограничить результаты для конкретной базы данных категории, код задает `categoryId` значение из строки запроса, передаваемые *ProductList.aspx*.</span><span class="sxs-lookup"><span data-stu-id="7453e-158">To limit the results to a specific database category, the code sets the `categoryId` value from the query string passed to *ProductList.aspx*.</span></span> <span data-ttu-id="7453e-159">`QueryStringAttribute` В класс `System.Web.ModelBinding` пространства имен используется для получения переменной строки запроса `id`от значения.</span><span class="sxs-lookup"><span data-stu-id="7453e-159">The `QueryStringAttribute` class in the `System.Web.ModelBinding` namespace is used to retrieve the query string variable `id`'s value.</span></span> <span data-ttu-id="7453e-160">Это указывает, что привязка модели, во время выполнения, привязать значения строки запроса для `categoryId` параметра.</span><span class="sxs-lookup"><span data-stu-id="7453e-160">This instructs model binding to, at run time, bind a query string value to the `categoryId` parameter.</span></span>

<span data-ttu-id="7453e-161">Когда допустимую категорию (`categoryId`) будет передан, результаты ограничены продукты этой категории баз данных.</span><span class="sxs-lookup"><span data-stu-id="7453e-161">When a valid category (`categoryId`) is passed, the results are limited to that category's database products.</span></span> <span data-ttu-id="7453e-162">Например если *ProductsList.aspx* это URL-адрес страницы:</span><span class="sxs-lookup"><span data-stu-id="7453e-162">For instance, if the *ProductsList.aspx* page URL is this:</span></span>

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

<span data-ttu-id="7453e-163">На странице отображаются только те продукты, где `categoryId` равно `1`.</span><span class="sxs-lookup"><span data-stu-id="7453e-163">The page displays only the products where the `categoryId` equals `1`.</span></span>

<span data-ttu-id="7453e-164">Все продукты отображаются в том случае, если передается строка запроса отсутствует.</span><span class="sxs-lookup"><span data-stu-id="7453e-164">All products are displayed if no query string is passed.</span></span>

<span data-ttu-id="7453e-165">Источники значений для этих методов вызываются *значение поставщики* (такие как `QueryString`), и вызываются атрибуты параметров, которые указывают, какой поставщик значений для использования *значение атрибуты поставщика* () Например, `id`).</span><span class="sxs-lookup"><span data-stu-id="7453e-165">The value sources for these methods are called *value providers* (such as `QueryString`), and the parameter attributes that indicate which value provider to use are called *value provider attributes* (such as `id`).</span></span> <span data-ttu-id="7453e-166">ASP.NET включает в себя поставщики значений и атрибутов для всех типичных веб-форм приложения пользователя источников входных данных.</span><span class="sxs-lookup"><span data-stu-id="7453e-166">ASP.NET includes value providers and attributes for all typical Web Forms application user input sources.</span></span> <span data-ttu-id="7453e-167">К ним относятся строки запроса, файлы cookie, значения формы, элементы управления, состояние представления, состояние сеанса и свойства профиля.</span><span class="sxs-lookup"><span data-stu-id="7453e-167">These include the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="7453e-168">Можно также написать пользовательские поставщики значений.</span><span class="sxs-lookup"><span data-stu-id="7453e-168">You can also write custom value providers.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="7453e-169">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="7453e-169">Run the application</span></span>

<span data-ttu-id="7453e-170">Запустите приложение и просмотреть все продукты или категории продуктов.</span><span class="sxs-lookup"><span data-stu-id="7453e-170">Run the application now to view all products or a category's products.</span></span>

1. <span data-ttu-id="7453e-171">В Visual Studio нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="7453e-171">In Visual Studio, press **F5** to run the application.</span></span>
 <span data-ttu-id="7453e-172">Откроется браузер и показывает *Default.aspx* страницы.</span><span class="sxs-lookup"><span data-stu-id="7453e-172">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="7453e-173">Выберите пункт категории продукта, **автомобили**.</span><span class="sxs-lookup"><span data-stu-id="7453e-173">From the product category menu, select **Cars**.</span></span>

   <span data-ttu-id="7453e-174">*ProductList.aspx* отображается страница, показывающая только продукты из **автомобили** категории.</span><span class="sxs-lookup"><span data-stu-id="7453e-174">The *ProductList.aspx* page appears, showing only products from the **Cars** category.</span></span> <span data-ttu-id="7453e-175">Далее в этом руководстве отображения сведений о продукте.</span><span class="sxs-lookup"><span data-stu-id="7453e-175">Later in this tutorial, you display product details.</span></span>

    ![Отображение данных элементов и сведения о — автомобили](display_data_items_and_details/_static/image2.png)

3. <span data-ttu-id="7453e-177">Выберите **продуктов** в верхнем меню.</span><span class="sxs-lookup"><span data-stu-id="7453e-177">Select **Products** from the top menu.</span></span>
 <span data-ttu-id="7453e-178">*ProductList.aspx* страницы теперь отображаются все продукты.</span><span class="sxs-lookup"><span data-stu-id="7453e-178">The *ProductList.aspx* page now displays all products.</span></span> 

    ![Отображение данных элементов и сведения о — продуктов](display_data_items_and_details/_static/image3.png)

4. <span data-ttu-id="7453e-180">Закройте браузер и вернуться в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7453e-180">Close the browser and return to Visual Studio.</span></span>

### <a name="add-a-data-control-to-display-product-details"></a><span data-ttu-id="7453e-181">Добавьте элемент управления данные для отображения сведений о продукте</span><span class="sxs-lookup"><span data-stu-id="7453e-181">Add a Data Control to display product details</span></span>

<span data-ttu-id="7453e-182">Изменить *ProductDetails.aspx* разметки, добавленный в предыдущем руководстве, чтобы отобразить сведения о конкретных продуктах:</span><span class="sxs-lookup"><span data-stu-id="7453e-182">Modify the *ProductDetails.aspx* markup that you added in the previous tutorial to display specific product information:</span></span>

1. <span data-ttu-id="7453e-183">В **обозревателе решений**откройте *ProductDetails.aspx*.</span><span class="sxs-lookup"><span data-stu-id="7453e-183">In **Solution Explorer**, open *ProductDetails.aspx*.</span></span>

2. <span data-ttu-id="7453e-184">Замените существующую разметку с этой разметкой:</span><span class="sxs-lookup"><span data-stu-id="7453e-184">Replace the existing markup with this markup:</span></span>

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

<span data-ttu-id="7453e-185">Эта разметка использует **FormView** управления для отображения сведений определенного продукта.</span><span class="sxs-lookup"><span data-stu-id="7453e-185">This markup uses a **FormView** control to display specific product details.</span></span> <span data-ttu-id="7453e-186">Он использует методы, например тех, которые используются для отображения данных в *ProductList.aspx*.</span><span class="sxs-lookup"><span data-stu-id="7453e-186">It uses methods like those used to display data in *ProductList.aspx*.</span></span> <span data-ttu-id="7453e-187">**FormView** элемент управления используется для отображения одной записи за раз из источника данных.</span><span class="sxs-lookup"><span data-stu-id="7453e-187">The **FormView** control is used to display a single record at a time from a data source.</span></span> <span data-ttu-id="7453e-188">При использовании **FormView** элемента управления, создавать шаблоны для отображения и редактирования значений с привязкой к данным.</span><span class="sxs-lookup"><span data-stu-id="7453e-188">When you use the **FormView** control, you create templates to display and edit data-bound values.</span></span> <span data-ttu-id="7453e-189">Эти шаблоны содержат элементы управления, выражения, привязки и форматирование, которое определить вид и функциональные возможности формы.</span><span class="sxs-lookup"><span data-stu-id="7453e-189">These templates contain controls, binding expressions, and formatting that define the form's look and functionality.</span></span>

<span data-ttu-id="7453e-190">Предыдущей разметки для подключения к базе данных требуется дополнительный код.</span><span class="sxs-lookup"><span data-stu-id="7453e-190">Connecting the previous markup to the database requires additional code.</span></span>

1. <span data-ttu-id="7453e-191">В **обозревателе решений**, щелкните правой кнопкой мыши *ProductDetails.aspx* , а затем выберите **Просмотр кода**.</span><span class="sxs-lookup"><span data-stu-id="7453e-191">In **Solution Explorer**, right-click *ProductDetails.aspx* and then select **View Code**.</span></span>  
   <span data-ttu-id="7453e-192">*ProductDetails.aspx.cs* отобразится файл.</span><span class="sxs-lookup"><span data-stu-id="7453e-192">The *ProductDetails.aspx.cs* file is displayed.</span></span>

2. <span data-ttu-id="7453e-193">Замените существующий код следующим:</span><span class="sxs-lookup"><span data-stu-id="7453e-193">Replace the existing code with this:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

<span data-ttu-id="7453e-194">Этот код проверяет наличие "`productID`" значения строки запроса.</span><span class="sxs-lookup"><span data-stu-id="7453e-194">This code checks for a "`productID`" query string value.</span></span> <span data-ttu-id="7453e-195">При обнаружении допустимое значение, отображается соответствующий продукт.</span><span class="sxs-lookup"><span data-stu-id="7453e-195">If a valid value is found, the matching product is displayed.</span></span> <span data-ttu-id="7453e-196">Если не найдено в строке запроса или его значение является недопустимым, программы не отображается.</span><span class="sxs-lookup"><span data-stu-id="7453e-196">If the query string isn't found, or its value isn't valid, no product is displayed.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="7453e-197">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="7453e-197">Run the application</span></span>

<span data-ttu-id="7453e-198">Теперь вы можете запустить приложение, чтобы просмотреть сведения о определенного продукта на основе идентификатора продукта.</span><span class="sxs-lookup"><span data-stu-id="7453e-198">Now you can run the application to see specific product details based on product ID.</span></span>

1. <span data-ttu-id="7453e-199">В Visual Studio нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="7453e-199">In Visual Studio, press **F5** to run the application.</span></span>  
 <span data-ttu-id="7453e-200">В браузере откроется *Default.aspx*.</span><span class="sxs-lookup"><span data-stu-id="7453e-200">The browser opens to *Default.aspx*.</span></span>

2. <span data-ttu-id="7453e-201">В меню «Категория» выберите **лодок**.</span><span class="sxs-lookup"><span data-stu-id="7453e-201">From the category menu, select **Boats**.</span></span>  
 <span data-ttu-id="7453e-202">*ProductList.aspx* откроется страница.</span><span class="sxs-lookup"><span data-stu-id="7453e-202">The *ProductList.aspx* page is displayed.</span></span>

3. <span data-ttu-id="7453e-203">Выберите **бумаги яхту**.</span><span class="sxs-lookup"><span data-stu-id="7453e-203">Select **Paper Boat**.</span></span>  
 <span data-ttu-id="7453e-204">*ProductDetails.aspx* откроется страница.</span><span class="sxs-lookup"><span data-stu-id="7453e-204">The *ProductDetails.aspx* page is displayed.</span></span>   

    ![Отображение данных элементов и сведения о — продуктов](display_data_items_and_details/_static/image4.png)

<span data-ttu-id="7453e-206">В следующем учебном курсе добавьте корзины для покупок в приложение Wingtip Toys.</span><span class="sxs-lookup"><span data-stu-id="7453e-206">In the next tutorial, you add a shopping cart to the Wingtip Toys application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7453e-207">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="7453e-207">Additional resources</span></span>

[<span data-ttu-id="7453e-208">Извлечение и отображение данных с помощью привязки модели и веб-форм</span><span class="sxs-lookup"><span data-stu-id="7453e-208">Retrieving and displaying data with model binding and web forms</span></span>](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> <span data-ttu-id="7453e-209">[Назад](ui_and_navigation.md)
> [Вперед](shopping-cart.md)</span><span class="sxs-lookup"><span data-stu-id="7453e-209">[Previous](ui_and_navigation.md)
[Next](shopping-cart.md)</span></span>
