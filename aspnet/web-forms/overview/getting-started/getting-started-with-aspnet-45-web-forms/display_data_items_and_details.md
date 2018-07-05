---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Отображение элементов данных и сведений | Документация Майкрософт
author: Erikre
description: В этой серии руководств будет основы создания приложений веб-форм ASP.NET с помощью ASP.NET 4.5 и Microsoft Visual Studio Express 2013 для мы...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 08efafc1ae076bcf481c5c7d8aea2bbaa704af17
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379742"
---
<a name="display-data-items-and-details"></a><span data-ttu-id="308d0-103">Отображение элементов данных и сведений</span><span class="sxs-lookup"><span data-stu-id="308d0-103">Display Data Items and Details</span></span>
====================
<span data-ttu-id="308d0-104">по [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="308d0-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="308d0-105">[Скачайте пример проекта Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) или [скачайте электронную книгу (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="308d0-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="308d0-106">В этой серии руководств будет основы создания приложений веб-форм ASP.NET с помощью ASP.NET 4.5 и Microsoft Visual Studio Express 2013 для Web.</span><span class="sxs-lookup"><span data-stu-id="308d0-106">This tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="308d0-107">Visual Studio 2013 [проект с исходным кодом C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) доступен на следующей странице этой серии руководств.</span><span class="sxs-lookup"><span data-stu-id="308d0-107">A Visual Studio 2013 [project with C# source code](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) is available to accompany this tutorial series.</span></span>


<span data-ttu-id="308d0-108">Это руководство содержит инструкции для отображения элементов данных и сведений об элементе данных, с помощью веб-форм ASP.NET и Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="308d0-108">This tutorial describes how to display data items and data item details using ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="308d0-109">Этот учебник основан на предыдущем учебном курсе «И навигация по пользовательскому Интерфейсу» и является частью серии руководств Store Toy Wingtip.</span><span class="sxs-lookup"><span data-stu-id="308d0-109">This tutorial builds on the previous tutorial "UI and Navigation" and is part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="308d0-110">Если после завершения данного учебника, вы сможете см. в разделе продуктов на *ProductsList.aspx* страницы, а также сведения о отдельному продукту на *ProductDetails.aspx* страницы.</span><span class="sxs-lookup"><span data-stu-id="308d0-110">When you've completed this tutorial, you'll be able to see products on the *ProductsList.aspx* page and details about an individual product on the *ProductDetails.aspx* page.</span></span>

## <a name="what-youll-learn"></a><span data-ttu-id="308d0-111">Вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="308d0-111">What you'll learn:</span></span>

- <span data-ttu-id="308d0-112">Как добавить элемент управления данные для отображения продуктов из базы данных.</span><span class="sxs-lookup"><span data-stu-id="308d0-112">How to add a data control to display products from the database.</span></span>
- <span data-ttu-id="308d0-113">Как подключить элемент управления данные для выбранных данных.</span><span class="sxs-lookup"><span data-stu-id="308d0-113">How to connect a data control to the selected data.</span></span>
- <span data-ttu-id="308d0-114">Как добавить элемент управления данные для отображения сведений о продукте из базы данных.</span><span class="sxs-lookup"><span data-stu-id="308d0-114">How to add a data control to display product details from the database.</span></span>
- <span data-ttu-id="308d0-115">Как извлечь значение из строки запроса и использовать это значение, чтобы ограничить данные, которые извлекаются из базы данных.</span><span class="sxs-lookup"><span data-stu-id="308d0-115">How to retrieve a value from the query string and use that value to limit the data that's retrieved from the database.</span></span>

### <a name="these-are-the-features-introduced-in-the-tutorial"></a><span data-ttu-id="308d0-116">Ниже приведены функции, представленные в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="308d0-116">These are the features introduced in the tutorial:</span></span>

- <span data-ttu-id="308d0-117">Привязка модели</span><span class="sxs-lookup"><span data-stu-id="308d0-117">Model Binding</span></span>
- <span data-ttu-id="308d0-118">Поставщики значений</span><span class="sxs-lookup"><span data-stu-id="308d0-118">Value providers</span></span>

## <a name="adding-a-data-control-to-display-products"></a><span data-ttu-id="308d0-119">Добавление данных элемента управления для отображения продуктов</span><span class="sxs-lookup"><span data-stu-id="308d0-119">Adding a Data Control to Display Products</span></span>

<span data-ttu-id="308d0-120">Во время привязки данных к серверному элементу управления, существует несколько вариантов, которые можно использовать.</span><span class="sxs-lookup"><span data-stu-id="308d0-120">When binding data to a server control, there are a few different options you can use.</span></span> <span data-ttu-id="308d0-121">Основные параметры включают добавление элемента управления источником данных, добавление кода вручную или с помощью привязки модели.</span><span class="sxs-lookup"><span data-stu-id="308d0-121">The most common options include adding a data source control, adding code by hand, or using model binding.</span></span>

### <a name="using-a-data-source-control-to-bind-data"></a><span data-ttu-id="308d0-122">Использование элемента управления источником данных для привязки данных</span><span class="sxs-lookup"><span data-stu-id="308d0-122">Using a Data Source Control to Bind Data</span></span>

<span data-ttu-id="308d0-123">Добавление элемента управления источника данных позволяет связать элемент управления источником данных элемент управления, который отображает данные.</span><span class="sxs-lookup"><span data-stu-id="308d0-123">Adding a data source control allows you to link the data source control to the control that displays the data.</span></span> <span data-ttu-id="308d0-124">Такой подход позволяет декларативно серверных элементов управления напрямую подключаться к источникам данных, а не используя программный подход.</span><span class="sxs-lookup"><span data-stu-id="308d0-124">This approach allows you to declaratively connect server-side controls directly to data sources, rather than using a programmatic approach.</span></span>

### <a name="coding-by-hand-to-bind-data"></a><span data-ttu-id="308d0-125">Кодировании вручную для привязки данных</span><span class="sxs-lookup"><span data-stu-id="308d0-125">Coding By Hand to Bind Data</span></span>

<span data-ttu-id="308d0-126">Добавление кода вручную включает в себя считывания значения, проверки на наличие значения null, попытка преобразовать его к соответствующему типу, проверка, успешно ли выполнено преобразование и наконец, используя значение в запросе.</span><span class="sxs-lookup"><span data-stu-id="308d0-126">Adding code by hand involves reading the value, checking for a null value, attempting to convert it to the appropriate type, checking whether the conversion was successful, and finally, using the value in the query.</span></span> <span data-ttu-id="308d0-127">Этот подход следует использовать, когда необходимо сохранить полный контроль над логику доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="308d0-127">You would use this approach when you need to retain full control over your data-access logic.</span></span>

### <a name="using-model-binding-to-bind-data"></a><span data-ttu-id="308d0-128">С помощью модели привязки для привязки данных</span><span class="sxs-lookup"><span data-stu-id="308d0-128">Using Model Binding to Bind Data</span></span>

<span data-ttu-id="308d0-129">С помощью привязки модели позволяет привязать результаты, используя гораздо меньше кода и дает возможность повторно использовать функциональные возможности во всем приложении.</span><span class="sxs-lookup"><span data-stu-id="308d0-129">Using model binding allows you to bind results using far less code and gives you the ability to reuse the functionality throughout your application.</span></span> <span data-ttu-id="308d0-130">Модель привязки, которая упрощают работу с логики доступа к данным на ориентированный на код, не теряя преимуществ платформы многофункциональных, привязки данных.</span><span class="sxs-lookup"><span data-stu-id="308d0-130">Model binding aims to simplify working with code-focused data-access logic while still retaining the benefits of a rich, data-binding framework.</span></span>

## <a name="displaying-products"></a><span data-ttu-id="308d0-131">Отображение продуктов</span><span class="sxs-lookup"><span data-stu-id="308d0-131">Displaying Products</span></span>

<span data-ttu-id="308d0-132">В этом руководстве вы используете привязки модели для привязки данных.</span><span class="sxs-lookup"><span data-stu-id="308d0-132">In this tutorial, you'll use model binding to bind data.</span></span> <span data-ttu-id="308d0-133">Чтобы настроить элемент управления данные для использования привязки модели для выбора данных, значение элемента управления `SelectMethod` имя метода в код страницы.</span><span class="sxs-lookup"><span data-stu-id="308d0-133">To configure a data control to use model binding to select data, you set the control's `SelectMethod` property to the name of a method in the page's code.</span></span> <span data-ttu-id="308d0-134">Элемент управления данными вызывает метод в соответствующее время жизненного цикла страницы и автоматически связывает возвращаемых данных.</span><span class="sxs-lookup"><span data-stu-id="308d0-134">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="308d0-135">Нет необходимости явно вызвать `DataBind` метод.</span><span class="sxs-lookup"><span data-stu-id="308d0-135">There's no need to explicitly call the `DataBind` method.</span></span>

<span data-ttu-id="308d0-136">Выполнив следующие действия, вы измените разметку в *ProductList.aspx* странице таким образом, чтобы на странице может отображаться продукты.</span><span class="sxs-lookup"><span data-stu-id="308d0-136">Using the steps below, you'll modify the markup in the *ProductList.aspx* page so that the page can display products.</span></span>

1. <span data-ttu-id="308d0-137">В **обозревателе решений**откройте *ProductList.aspx* страницы.</span><span class="sxs-lookup"><span data-stu-id="308d0-137">In **Solution Explorer**, open the *ProductList.aspx* page.</span></span>
2. <span data-ttu-id="308d0-138">Замените существующую разметку следующей разметкой:</span><span class="sxs-lookup"><span data-stu-id="308d0-138">Replace the existing markup with the following markup:</span></span>   

    [!code-aspx[Main](display_data_items_and_details/samples/sample1.aspx)]

<span data-ttu-id="308d0-139">Этот код использует **ListView** управления с именем «productList» для отображения продуктов.</span><span class="sxs-lookup"><span data-stu-id="308d0-139">This code uses a **ListView** control named "productList" to display the products.</span></span>

[!code-aspx[Main](display_data_items_and_details/samples/sample2.aspx)]

<span data-ttu-id="308d0-140">**ListView** элемент управления отображает данные в формате, который определяется с помощью шаблонов и стилей.</span><span class="sxs-lookup"><span data-stu-id="308d0-140">The **ListView** control displays data in a format that you define by using templates and styles.</span></span> <span data-ttu-id="308d0-141">Это полезно для данных в любой повторяющейся структуре.</span><span class="sxs-lookup"><span data-stu-id="308d0-141">It is useful for data in any repeating structure.</span></span> <span data-ttu-id="308d0-142">Это **ListView** примере просто отображает данные из базы данных, тем не менее можно разрешить пользователям для редактирования, вставки и удаления данных, а также для сортировки и страницы данных, без написания кода.</span><span class="sxs-lookup"><span data-stu-id="308d0-142">This **ListView** example simply shows data from the database, however you can enable users to edit, insert, and delete data, and to sort and page data, all without code.</span></span>

<span data-ttu-id="308d0-143">Задав `ItemType` свойство в **ListView** управления, выражение привязки данных `Item` доступна и элемент управления становится строго типизированными.</span><span class="sxs-lookup"><span data-stu-id="308d0-143">By setting the `ItemType` property in the **ListView** control, the data-binding expression `Item` is available and the control becomes strongly typed.</span></span> <span data-ttu-id="308d0-144">Как упоминалось в предыдущем учебном курсе, можно выбрать сведения о элемента объекта, с помощью IntelliSense, например, указать `ProductName`:</span><span class="sxs-lookup"><span data-stu-id="308d0-144">As mentioned in the previous tutorial, you can select details of the Item object using IntelliSense, such as specifying the `ProductName`:</span></span>

![Отображение данных элементов и сведения о — IntelliSense](display_data_items_and_details/_static/image1.png)

<span data-ttu-id="308d0-146">Кроме того, при использовании привязки модели для указания `SelectMethod` значение.</span><span class="sxs-lookup"><span data-stu-id="308d0-146">In addition, you are using model binding to specify a `SelectMethod` value.</span></span> <span data-ttu-id="308d0-147">Это значение (`GetProducts`) будет соответствовать методу, который вы добавите код для отображения продуктов на следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="308d0-147">This value (`GetProducts`) will correspond to the method that you will add to the code behind to display products in the next step.</span></span>

### <a name="adding-code-to-display-products"></a><span data-ttu-id="308d0-148">Добавление кода для отображения продуктов</span><span class="sxs-lookup"><span data-stu-id="308d0-148">Adding Code to Display Products</span></span>

<span data-ttu-id="308d0-149">На этом шаге вы добавите код для заполнения **ListView** элемента управления с данными о продуктах из базы данных.</span><span class="sxs-lookup"><span data-stu-id="308d0-149">In this step, you'll add code to populate the **ListView** control with product data from the database.</span></span> <span data-ttu-id="308d0-150">Код будет поддерживать отображение продуктов по отдельной категории, отображающий все продукты.</span><span class="sxs-lookup"><span data-stu-id="308d0-150">The code will support showing products by individual category, as well as showing all products.</span></span>

1. <span data-ttu-id="308d0-151">В **обозревателе решений**, щелкните правой кнопкой мыши *ProductList.aspx* и нажмите кнопку **Просмотр кода**.</span><span class="sxs-lookup"><span data-stu-id="308d0-151">In **Solution Explorer**, right-click *ProductList.aspx* and then click **View Code**.</span></span>
2. <span data-ttu-id="308d0-152">Замените существующий код в *ProductList.aspx.cs* файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="308d0-152">Replace the existing code in the *ProductList.aspx.cs* file with the following code:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

<span data-ttu-id="308d0-153">В следующем коде показано `GetProducts` метод, на который ссылается `ItemType` свойство **ListView** контролировать *ProductList.aspx* страницы.</span><span class="sxs-lookup"><span data-stu-id="308d0-153">This code shows the `GetProducts` method that's referenced by the `ItemType` property of the **ListView** control in the *ProductList.aspx* page.</span></span> <span data-ttu-id="308d0-154">Чтобы ограничить результаты для определенной категории в базе данных, код задает `categoryId` значение из значения строки запроса, передаваемые *ProductList.aspx* страницы при *ProductList.aspx* страница Переход к.</span><span class="sxs-lookup"><span data-stu-id="308d0-154">To limit the results to a specific category in the database, the code sets the `categoryId` value from the query string value passed to the *ProductList.aspx* page when the *ProductList.aspx* page is navigated to.</span></span> <span data-ttu-id="308d0-155">`QueryStringAttribute` В класс `System.Web.ModelBinding` пространство имен используется для извлечения значения идентификатор переменной строки запроса. Это указывает, что привязка модели к попытке привязать значение из строки запроса для `categoryId` параметра во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="308d0-155">The `QueryStringAttribute` class in the `System.Web.ModelBinding` namespace is used to retrieve the value of the query string variable id. This instructs model binding to try to bind a value from the query string to the `categoryId` parameter at run time.</span></span>

<span data-ttu-id="308d0-156">При передаче допустимую категорию как строку запроса на страницу, результаты запроса ограничиваются продуктов в базе данных, которые соответствуют `categoryId` значение.</span><span class="sxs-lookup"><span data-stu-id="308d0-156">When a valid category is passed as a query string to the page, the results of the query are limited to those products in the database that match the `categoryId` value.</span></span> <span data-ttu-id="308d0-157">Например если URL-адрес *ProductsList.aspx* страницы представляет собой следующее:</span><span class="sxs-lookup"><span data-stu-id="308d0-157">For instance, if the URL to the *ProductsList.aspx* page is the following:</span></span>

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

<span data-ttu-id="308d0-158">На странице отображаются только те продукты, где `category` равно `1`.</span><span class="sxs-lookup"><span data-stu-id="308d0-158">The page displays only the products where the `category` equals `1`.</span></span>

<span data-ttu-id="308d0-159">Если строка запроса не включен, при переходе к *ProductList.aspx* странице отображаются все продукты.</span><span class="sxs-lookup"><span data-stu-id="308d0-159">If no query string is included when navigating to the *ProductList.aspx* page, all products will be displayed.</span></span>

<span data-ttu-id="308d0-160">Источники значений для этих методов, называются *значение поставщики* (такие как *QueryString*), и параметр атрибуты, которые указывают, какой поставщик значений для использования, рассматриваются как значение поставщик атрибутов (например, "`id`«).</span><span class="sxs-lookup"><span data-stu-id="308d0-160">The sources of values for these methods are referred to as *value providers* (such as *QueryString*), and the parameter attributes that indicate which value provider to use are referred to as value provider attributes (such as "`id`").</span></span> <span data-ttu-id="308d0-161">ASP.NET включает в себя поставщики значений и соответствующих атрибутов для всех типичных источников данных, введенных пользователем в приложении Web Forms, такие как строка запроса, файлы cookie, значения формы, элементы управления, состояние представления, состояние сеанса и свойства профиля.</span><span class="sxs-lookup"><span data-stu-id="308d0-161">ASP.NET includes value providers and corresponding attributes for all of the typical sources of user input in a Web Forms application, such as the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="308d0-162">Можно также написать пользовательские поставщики значений.</span><span class="sxs-lookup"><span data-stu-id="308d0-162">You can also write custom value providers.</span></span>

### <a name="running-the-application"></a><span data-ttu-id="308d0-163">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="308d0-163">Running the Application</span></span>

<span data-ttu-id="308d0-164">Запустите приложение и увидеть, как можно просмотреть все продукты или просто набор продуктов, ограниченные по категориям.</span><span class="sxs-lookup"><span data-stu-id="308d0-164">Run the application now to see how you can view all of the products or just a set of products limited by category.</span></span>

1. <span data-ttu-id="308d0-165">В **обозревателе решений**, щелкните правой кнопкой мыши *Default.aspx* и укажите **просмотреть в браузере**.</span><span class="sxs-lookup"><span data-stu-id="308d0-165">In the **Solution Explorer**, right-click the *Default.aspx* page and select **View in Browser**.</span></span>  
 <span data-ttu-id="308d0-166">Браузер и отобразится *Default.aspx* страницы.</span><span class="sxs-lookup"><span data-stu-id="308d0-166">The browser will open and show the *Default.aspx* page.</span></span>
2. <span data-ttu-id="308d0-167">Выберите **автомобили** меню навигации категории продукта.</span><span class="sxs-lookup"><span data-stu-id="308d0-167">Select **Cars** from the product category navigation menu.</span></span>  
 <span data-ttu-id="308d0-168">*ProductList.aspx* откроется страница отображается только продукты, включенные в категории «Автомобили».</span><span class="sxs-lookup"><span data-stu-id="308d0-168">The *ProductList.aspx* page is displayed showing only products included in the "Cars" category.</span></span> <span data-ttu-id="308d0-169">Далее в этом руководстве будут отображаться сведения о продукте.</span><span class="sxs-lookup"><span data-stu-id="308d0-169">Later in this tutorial, you will display product details.</span></span>  

    ![Отображение данных элементов и сведения о — автомобили](display_data_items_and_details/_static/image2.png)
3. <span data-ttu-id="308d0-171">Выберите **продуктов** меню навигации вверху.</span><span class="sxs-lookup"><span data-stu-id="308d0-171">Select **Products** from the navigation menu at the top.</span></span>  
 <span data-ttu-id="308d0-172">Опять же *ProductList.aspx* страница отображается, однако в настоящее время он показан весь список продуктов.</span><span class="sxs-lookup"><span data-stu-id="308d0-172">Again, the *ProductList.aspx* page is displayed, however this time it shows the entire list of products.</span></span>   

    ![Отображение данных элементов и сведения о — продуктов](display_data_items_and_details/_static/image3.png)
4. <span data-ttu-id="308d0-174">Закройте браузер и вернуться в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="308d0-174">Close the browser and return to Visual Studio.</span></span>

### <a name="adding-a-data-control-to-display-product-details"></a><span data-ttu-id="308d0-175">Добавление данных элемента управления для отображения сведений о продукте</span><span class="sxs-lookup"><span data-stu-id="308d0-175">Adding a Data Control to Display Product Details</span></span>

<span data-ttu-id="308d0-176">Затем вы измените разметку в *ProductDetails.aspx* страницы, добавленный в предыдущем учебном курсе, что страницы можно отображать сведения о отдельному продукту.</span><span class="sxs-lookup"><span data-stu-id="308d0-176">Next, you'll modify the markup in the *ProductDetails.aspx* page that you added in the previous tutorial so that the page can display information about an individual product.</span></span>

1. <span data-ttu-id="308d0-177">В **обозревателе решений**откройте *ProductDetails.aspx* страницы.</span><span class="sxs-lookup"><span data-stu-id="308d0-177">In **Solution Explorer**, open the *ProductDetails.aspx* page.</span></span>
2. <span data-ttu-id="308d0-178">Замените существующую разметку следующей разметкой:</span><span class="sxs-lookup"><span data-stu-id="308d0-178">Replace the existing markup with the following markup:</span></span>   

    [!code-aspx[Main](display_data_items_and_details/samples/sample5.aspx)]

<span data-ttu-id="308d0-179">Этот код использует **FormView** управления для отображения сведений о отдельному продукту.</span><span class="sxs-lookup"><span data-stu-id="308d0-179">This code uses a **FormView** control to display details about an individual product.</span></span> <span data-ttu-id="308d0-180">Эта разметка использует методы, такие как те, которые используются для отображения данных в *ProductList.aspx* страницы.</span><span class="sxs-lookup"><span data-stu-id="308d0-180">This markup uses methods like those that are used to display data in the *ProductList.aspx* page.</span></span> <span data-ttu-id="308d0-181">**FormView** элемент управления используется для отображения одной записи за раз из источника данных.</span><span class="sxs-lookup"><span data-stu-id="308d0-181">The **FormView** control is used to display a single record at a time from a data source.</span></span> <span data-ttu-id="308d0-182">При использовании **FormView** элемента управления, создавать шаблоны для отображения и редактирования значений с привязкой к данным.</span><span class="sxs-lookup"><span data-stu-id="308d0-182">When you use the **FormView** control, you create templates to display and edit data-bound values.</span></span> <span data-ttu-id="308d0-183">Шаблоны содержат элементы управления, выражения привязки и форматирования, которые определяют внешний вид и функциональность формы.</span><span class="sxs-lookup"><span data-stu-id="308d0-183">The templates contain controls, binding expressions, and formatting that define the look and functionality of the form.</span></span>

<span data-ttu-id="308d0-184">Приведенная выше разметка подключиться к базе данных, необходимо добавить дополнительный код для *ProductDetails.aspx* кода.</span><span class="sxs-lookup"><span data-stu-id="308d0-184">To connect the above markup to the database, you must add additional code to the *ProductDetails.aspx* code.</span></span>

1. <span data-ttu-id="308d0-185">В **обозревателе решений**, щелкните правой кнопкой мыши *ProductDetails.aspx* и нажмите кнопку **Просмотр кода**.</span><span class="sxs-lookup"><span data-stu-id="308d0-185">In **Solution Explorer**, right-click *ProductDetails.aspx* and then click **View Code**.</span></span>  
   <span data-ttu-id="308d0-186">*ProductDetails.aspx.cs* будет отображен файл.</span><span class="sxs-lookup"><span data-stu-id="308d0-186">The *ProductDetails.aspx.cs* file will be displayed.</span></span>
2. <span data-ttu-id="308d0-187">Замените существующий код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="308d0-187">Replace the existing code with the following code:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

<span data-ttu-id="308d0-188">Этот код проверяет наличие "`productID`" значения строки запроса.</span><span class="sxs-lookup"><span data-stu-id="308d0-188">This code checks for a "`productID`" query-string value.</span></span> <span data-ttu-id="308d0-189">Если значение допустимым строка запроса обнаруживается, отображается соответствующий продукт.</span><span class="sxs-lookup"><span data-stu-id="308d0-189">If a valid query-string value is found, the matching product is displayed.</span></span> <span data-ttu-id="308d0-190">Если найдена строка запроса отсутствует или недопустимое значение строки запроса, программы не отображается на *ProductDetails.aspx* страницы.</span><span class="sxs-lookup"><span data-stu-id="308d0-190">If no query-string is found, or the query-string value is not valid, no product is displayed on the *ProductDetails.aspx* page.</span></span>

### <a name="running-the-application"></a><span data-ttu-id="308d0-191">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="308d0-191">Running the Application</span></span>

<span data-ttu-id="308d0-192">Теперь вы можете запустить приложение, чтобы просмотреть отдельные продукта, отображаемое на основе идентификатора продукта.</span><span class="sxs-lookup"><span data-stu-id="308d0-192">Now you can run the application to see an individual product displayed based on the id of the product.</span></span>

1. <span data-ttu-id="308d0-193">Нажмите клавишу **F5** при работе в Visual Studio для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="308d0-193">Press **F5** while in Visual Studio to run the application.</span></span>  
 <span data-ttu-id="308d0-194">Браузер и отобразится *Default.aspx* страницы.</span><span class="sxs-lookup"><span data-stu-id="308d0-194">The browser will open and show the *Default.aspx* page.</span></span>
2. <span data-ttu-id="308d0-195">Выберите «Лодки» в меню навигации категории.</span><span class="sxs-lookup"><span data-stu-id="308d0-195">Select "Boats" from the category navigation menu.</span></span>  
 <span data-ttu-id="308d0-196">*ProductList.aspx* откроется страница.</span><span class="sxs-lookup"><span data-stu-id="308d0-196">The *ProductList.aspx* page is displayed.</span></span>
3. <span data-ttu-id="308d0-197">Выберите продукт «Яхту бумаги» из списка продуктов.</span><span class="sxs-lookup"><span data-stu-id="308d0-197">Select the "Paper Boat" product from the product list.</span></span>  
 <span data-ttu-id="308d0-198">*ProductDetails.aspx* откроется страница.</span><span class="sxs-lookup"><span data-stu-id="308d0-198">The *ProductDetails.aspx* page is displayed.</span></span>   

    ![Отображение данных элементов и сведения о — продуктов](display_data_items_and_details/_static/image4.png)
4. <span data-ttu-id="308d0-200">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="308d0-200">Close the browser.</span></span>

## <a name="summary"></a><span data-ttu-id="308d0-201">Сводка</span><span class="sxs-lookup"><span data-stu-id="308d0-201">Summary</span></span>

<span data-ttu-id="308d0-202">В этом руководстве из серии имеют добавьте разметку и код для отображения списка продуктов и отображения сведений о продукте.</span><span class="sxs-lookup"><span data-stu-id="308d0-202">In this tutorial of the series you have add markup and code to display a product list and to display product details.</span></span> <span data-ttu-id="308d0-203">Во время этого процесса вы узнали о строго типизированные элементы управления данными, привязка модели и поставщики значений.</span><span class="sxs-lookup"><span data-stu-id="308d0-203">During this process you have learned about strongly typed data controls, model binding, and value providers.</span></span> <span data-ttu-id="308d0-204">В следующем руководстве вы добавите корзины для покупок в пример приложения Wingtip Toys.</span><span class="sxs-lookup"><span data-stu-id="308d0-204">In the next tutorial, you'll add a shopping cart to the Wingtip Toys sample application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="308d0-205">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="308d0-205">Additional Resources</span></span>

[<span data-ttu-id="308d0-206">Извлечение и отображение данных с помощью привязки модели и веб-форм</span><span class="sxs-lookup"><span data-stu-id="308d0-206">Retrieving and displaying data with model binding and web forms</span></span>](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> <span data-ttu-id="308d0-207">[Назад](ui_and_navigation.md)
> [Вперед](shopping-cart.md)</span><span class="sxs-lookup"><span data-stu-id="308d0-207">[Previous](ui_and_navigation.md)
[Next](shopping-cart.md)</span></span>
