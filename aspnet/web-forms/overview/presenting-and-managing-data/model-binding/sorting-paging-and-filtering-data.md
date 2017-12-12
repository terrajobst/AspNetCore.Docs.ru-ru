---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: "Сортировка, разбиение по страницам и фильтрация данных с помощью модели привязки и веб-форм | Документы Microsoft"
author: tfitzmac
description: "Этот учебник ряд демонстрирует основные аспекты использования привязки модели в проекте веб-форм ASP.NET. Привязка модели позволяет взаимодействия с данными дополнительные прямые-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: 94fc84533be5fcbcf0612fcdcabea7dee738d89b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a><span data-ttu-id="02832-104">Сортировка, разбиение по страницам и фильтрация данных с помощью модели привязки и веб-форм</span><span class="sxs-lookup"><span data-stu-id="02832-104">Sorting, paging, and filtering data with model binding and web forms</span></span>
====================
<span data-ttu-id="02832-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="02832-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="02832-106">Этот учебник ряд демонстрирует основные аспекты использования привязки модели в проекте веб-форм ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="02832-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="02832-107">Привязка модели позволяет взаимодействия с данными более прост, чем работе с данными источника объектов (например, элемент управления ObjectDataSource или SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="02832-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="02832-108">Эта серия начинается с вводный материал и перемещает на следующих занятиях рассматривается более сложных понятиях.</span><span class="sxs-lookup"><span data-stu-id="02832-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="02832-109">Этот учебник демонстрирует добавление сортировка, разбиение по страницам и фильтровать данные до привязки модели.</span><span class="sxs-lookup"><span data-stu-id="02832-109">This tutorial shows how to add sorting, paging, and filtering of the data through model binding.</span></span>
> 
> <span data-ttu-id="02832-110">Этот учебник построен на проект, созданный в первом [часть](retrieving-data.md) ряда.</span><span class="sxs-lookup"><span data-stu-id="02832-110">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="02832-111">Вы можете [загрузки](https://go.microsoft.com/fwlink/?LinkId=286116) весь проект C# или Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="02832-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="02832-112">Загружаемый код работает с Visual Studio 2012 или Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="02832-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="02832-113">Она использует шаблон Visual Studio 2012, который немного отличается от шаблона Visual Studio 2013, показанными в данном руководстве.</span><span class="sxs-lookup"><span data-stu-id="02832-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="02832-114">Мы создадим</span><span class="sxs-lookup"><span data-stu-id="02832-114">What you'll build</span></span>

<span data-ttu-id="02832-115">В этом учебнике вы сможете:</span><span class="sxs-lookup"><span data-stu-id="02832-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="02832-116">Включить сортировку и разбиение по страницам данных</span><span class="sxs-lookup"><span data-stu-id="02832-116">Enable sorting and paging of the data</span></span>
2. <span data-ttu-id="02832-117">Включить фильтрацию данных на основе выбора пользователя</span><span class="sxs-lookup"><span data-stu-id="02832-117">Enable filtering of the data based on a selection by the user</span></span>

## <a name="add-sorting"></a><span data-ttu-id="02832-118">Добавление сортировки</span><span class="sxs-lookup"><span data-stu-id="02832-118">Add sorting</span></span>

<span data-ttu-id="02832-119">Включение сортировки в GridView является очень простым.</span><span class="sxs-lookup"><span data-stu-id="02832-119">Enabling sorting in the GridView is very easy.</span></span> <span data-ttu-id="02832-120">В файле Student.aspx задать **AllowSorting** для **true** в GridView.</span><span class="sxs-lookup"><span data-stu-id="02832-120">In the Student.aspx file, simply set **AllowSorting** to **true** in the GridView.</span></span> <span data-ttu-id="02832-121">Не нужно задавать **SortExpression** значение для каждого столбца, как DataField используется автоматически.</span><span class="sxs-lookup"><span data-stu-id="02832-121">You do not need to set a **SortExpression** value for each column as the DataField is automatically used.</span></span> <span data-ttu-id="02832-122">GridView изменяет запрос для включения упорядочение данных по выбранному значению.</span><span class="sxs-lookup"><span data-stu-id="02832-122">The GridView modifies the query to include ordering the data by the selected value.</span></span> <span data-ttu-id="02832-123">Выделенный код ниже показано добавление, что необходимо внести разрешить сортировку.</span><span class="sxs-lookup"><span data-stu-id="02832-123">The highlighted code below shows the addition you need to make to enable sorting.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

<span data-ttu-id="02832-124">Запустите веб-приложения и тестирования сортировки записей студентов по значениям в разные столбцы.</span><span class="sxs-lookup"><span data-stu-id="02832-124">Run the web application, and test sorting student records by the values in different columns.</span></span>

![Студенты сортировки](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a><span data-ttu-id="02832-126">Добавить разбиение на страницы</span><span class="sxs-lookup"><span data-stu-id="02832-126">Add paging</span></span>

<span data-ttu-id="02832-127">Включение разбиения также является очень простым.</span><span class="sxs-lookup"><span data-stu-id="02832-127">Enabling paging is also very easy.</span></span> <span data-ttu-id="02832-128">В GridView, задайте **AllowPaging** свойства **true** и задайте **PageSize** количество записей, которые нужно отобразить на каждой странице.</span><span class="sxs-lookup"><span data-stu-id="02832-128">In the GridView, set the **AllowPaging** property to **true** and set the **PageSize** property to the number of records you wish to display on each page.</span></span> <span data-ttu-id="02832-129">В этом учебнике можно задать значение 4.</span><span class="sxs-lookup"><span data-stu-id="02832-129">In this tutorial, you can set it to 4.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

<span data-ttu-id="02832-130">Запустите веб-приложение и обратите внимание, что теперь записи разделяются на несколько страниц с не более 4 записей, отображаемых на одной странице.</span><span class="sxs-lookup"><span data-stu-id="02832-130">Run the web application, and notice that now the records are divided over multiple pages with no more than 4 records displayed on a single page.</span></span>

![добавить разбиение на страницы](sorting-paging-and-filtering-data/_static/image4.png)

<span data-ttu-id="02832-132">Отложенное выполнение запросов повышает эффективность работы приложения.</span><span class="sxs-lookup"><span data-stu-id="02832-132">Deferred query execution improves the application efficiency.</span></span> <span data-ttu-id="02832-133">Вместо извлечения весь набор данных, GridView изменяет запрос таким образом, чтобы получить только те записи, для текущей страницы.</span><span class="sxs-lookup"><span data-stu-id="02832-133">Instead of retrieving the entire data set, the GridView modifies the query to retrieve only the records for the current page.</span></span>

## <a name="filter-records-by-user-selection"></a><span data-ttu-id="02832-134">Отбор записей путем выбора пользователя</span><span class="sxs-lookup"><span data-stu-id="02832-134">Filter records by user selection</span></span>

<span data-ttu-id="02832-135">Привязка модели добавляет несколько атрибутов, которые позволяют определить, как задать значение для параметра в методе привязки модели.</span><span class="sxs-lookup"><span data-stu-id="02832-135">Model binding adds several attributes which enable you to designate how to set the value for a parameter in a model binding method.</span></span> <span data-ttu-id="02832-136">Эти атрибуты являются в **System.Web.ModelBinding** пространства имен.</span><span class="sxs-lookup"><span data-stu-id="02832-136">These attributes are in the **System.Web.ModelBinding** namespace.</span></span> <span data-ttu-id="02832-137">В их число входят следующее.</span><span class="sxs-lookup"><span data-stu-id="02832-137">They include:</span></span>

- <span data-ttu-id="02832-138">Control</span><span class="sxs-lookup"><span data-stu-id="02832-138">Control</span></span>
- <span data-ttu-id="02832-139">Куки-файл</span><span class="sxs-lookup"><span data-stu-id="02832-139">Cookie</span></span>
- <span data-ttu-id="02832-140">Form</span><span class="sxs-lookup"><span data-stu-id="02832-140">Form</span></span>
- <span data-ttu-id="02832-141">Профиль</span><span class="sxs-lookup"><span data-stu-id="02832-141">Profile</span></span>
- <span data-ttu-id="02832-142">QueryString</span><span class="sxs-lookup"><span data-stu-id="02832-142">QueryString</span></span>
- <span data-ttu-id="02832-143">RouteData</span><span class="sxs-lookup"><span data-stu-id="02832-143">RouteData</span></span>
- <span data-ttu-id="02832-144">Сеанс</span><span class="sxs-lookup"><span data-stu-id="02832-144">Session</span></span>
- <span data-ttu-id="02832-145">Профиль пользователя</span><span class="sxs-lookup"><span data-stu-id="02832-145">UserProfile</span></span>
- <span data-ttu-id="02832-146">Состояние просмотра</span><span class="sxs-lookup"><span data-stu-id="02832-146">ViewState</span></span>

<span data-ttu-id="02832-147">В этом учебнике будет использоваться значение элемента управления для фильтрации отображаемых записей в GridView.</span><span class="sxs-lookup"><span data-stu-id="02832-147">In this tutorial, you will use a control's value to filter which records are displayed in the GridView.</span></span> <span data-ttu-id="02832-148">Вы добавите **управления** атрибут для метода запроса, который был создан ранее.</span><span class="sxs-lookup"><span data-stu-id="02832-148">You will add the **Control** attribute to the query method you had created earlier.</span></span> <span data-ttu-id="02832-149">В [позже](using-query-string-values-to-retrieve-data.md) учебника вы научитесь применять **QueryString** атрибут к параметру, чтобы указать, что значение параметра берется из значение строки запроса.</span><span class="sxs-lookup"><span data-stu-id="02832-149">In a [later](using-query-string-values-to-retrieve-data.md) tutorial, you will apply the **QueryString** attribute to a parameter to specify that the parameter value comes from a query string value.</span></span>

<span data-ttu-id="02832-150">Во-первых над ValidationSummary, добавьте раскрывающегося списка для фильтрации показаны какие учащихся.</span><span class="sxs-lookup"><span data-stu-id="02832-150">First, above the ValidationSummary, add a drop down list for filtering which students are shown.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

<span data-ttu-id="02832-151">В файле кода измените метод select для получения значения из элемента управления и задайте имя параметра имя элемента управления, которое предоставляет значение.</span><span class="sxs-lookup"><span data-stu-id="02832-151">In the code-behind file, modify the select method to receive a value from the control, and set the name of the parameter to the name of the control that provides the value.</span></span>

<span data-ttu-id="02832-152">Необходимо добавить **с помощью** инструкции для **System.Web.ModelBinding** пространство имен для разрешения атрибута элемента управления.</span><span class="sxs-lookup"><span data-stu-id="02832-152">You must add a **using** statement for the **System.Web.ModelBinding** namespace to resolve the Control attribute.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

<span data-ttu-id="02832-153">Ниже приведен метод select повторно работал для фильтрации возвращаемых данных на основе значения из раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="02832-153">The following code shows the select method re-worked to filter the returned data based on the value of the drop down list.</span></span> <span data-ttu-id="02832-154">Добавление атрибута элемента управления перед параметр указывает, что значение для этого параметра берется из элемента управления с тем же именем.</span><span class="sxs-lookup"><span data-stu-id="02832-154">Adding a control attribute before a parameter specifies that the value for this parameter comes from a control with the same name.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

<span data-ttu-id="02832-155">Запустите веб-приложение и выберите из раскрывающегося списка для фильтрации списка учащихся разные значения.</span><span class="sxs-lookup"><span data-stu-id="02832-155">Run the web application and select different values from the drop down list to filter the list of students.</span></span>

![Студенты фильтра](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a><span data-ttu-id="02832-157">Заключение</span><span class="sxs-lookup"><span data-stu-id="02832-157">Conclusion</span></span>

<span data-ttu-id="02832-158">В этом учебнике вы включили сортировки и разбиения на страницы данных.</span><span class="sxs-lookup"><span data-stu-id="02832-158">In this tutorial, you enabled sorting and paging of the data.</span></span> <span data-ttu-id="02832-159">Вы также включена фильтрация данных на основе значения элемента управления.</span><span class="sxs-lookup"><span data-stu-id="02832-159">You also enabled filtering the data by the value of a control.</span></span>

<span data-ttu-id="02832-160">В следующем [учебника](integrating-jquery-ui.md) будет улучшения пользовательского интерфейса интегрируя графического пользовательского интерфейса JQuery в шаблон динамических данных.</span><span class="sxs-lookup"><span data-stu-id="02832-160">In the next [tutorial](integrating-jquery-ui.md) you will enhance the UI by integrating a JQuery UI widget into the dynamic data template.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="02832-161">[Назад](updating-deleting-and-creating-data.md)
[Вперед](integrating-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="02832-161">[Previous](updating-deleting-and-creating-data.md)
[Next](integrating-jquery-ui.md)</span></span>
