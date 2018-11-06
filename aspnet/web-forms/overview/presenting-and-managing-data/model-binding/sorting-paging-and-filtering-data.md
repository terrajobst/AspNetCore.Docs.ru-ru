---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Сортировка, разбиение по страницам и фильтрация данных с помощью привязки модели и веб-форм | Документация Майкрософт
author: Rick-Anderson
description: В этой серии руководств показано основными аспектами с помощью привязки модели с проектом веб-форм ASP.NET. Привязка модели позволяет взаимодействие с данными более прямой-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: 624f98cea6030e0b7b022f86c4c1aa37f1db9726
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020958"
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a><span data-ttu-id="d48b4-104">Сортировка, разбиение по страницам и фильтрация данных с помощью привязки модели и веб-форм</span><span class="sxs-lookup"><span data-stu-id="d48b4-104">Sorting, paging, and filtering data with model binding and web forms</span></span>
====================
<span data-ttu-id="d48b4-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d48b4-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d48b4-106">В этой серии руководств показано основными аспектами с помощью привязки модели с проектом веб-форм ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d48b4-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="d48b4-107">Привязка модели упрощает взаимодействие с данными более эффективную чем работа с данными объектов источника (например, элемент управления ObjectDataSource и SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="d48b4-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="d48b4-108">Эта серия начинается с вводный материал и перемещает до более продвинутых в последующих руководствах.</span><span class="sxs-lookup"><span data-stu-id="d48b4-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="d48b4-109">Этот учебник демонстрирует добавление сортировка, разбиение по страницам и фильтрацию данных с помощью привязки модели.</span><span class="sxs-lookup"><span data-stu-id="d48b4-109">This tutorial shows how to add sorting, paging, and filtering of the data through model binding.</span></span>
> 
> <span data-ttu-id="d48b4-110">Этот учебник основан на проект, созданный в первом [часть](retrieving-data.md) ряда.</span><span class="sxs-lookup"><span data-stu-id="d48b4-110">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="d48b4-111">Вы можете [загрузить](https://go.microsoft.com/fwlink/?LinkId=286116) весь проект полностью на C# или Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="d48b4-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="d48b4-112">Загружаемый код работает с Visual Studio 2012 или Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="d48b4-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="d48b4-113">Он использует шаблон Visual Studio 2012, который немного отличается от шаблона Visual Studio 2013, в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="d48b4-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="d48b4-114">Вы создадите</span><span class="sxs-lookup"><span data-stu-id="d48b4-114">What you'll build</span></span>

<span data-ttu-id="d48b4-115">В этом руководстве вам потребуется:</span><span class="sxs-lookup"><span data-stu-id="d48b4-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="d48b4-116">Включить сортировку и разбиение по страницам данных</span><span class="sxs-lookup"><span data-stu-id="d48b4-116">Enable sorting and paging of the data</span></span>
2. <span data-ttu-id="d48b4-117">Включить фильтрацию данных на основе выбранного пользователем</span><span class="sxs-lookup"><span data-stu-id="d48b4-117">Enable filtering of the data based on a selection by the user</span></span>

## <a name="add-sorting"></a><span data-ttu-id="d48b4-118">Добавление сортировки</span><span class="sxs-lookup"><span data-stu-id="d48b4-118">Add sorting</span></span>

<span data-ttu-id="d48b4-119">Включение сортировки в GridView очень проста.</span><span class="sxs-lookup"><span data-stu-id="d48b4-119">Enabling sorting in the GridView is very easy.</span></span> <span data-ttu-id="d48b4-120">В файле Student.aspx, просто установите **AllowSorting** для **true** в GridView.</span><span class="sxs-lookup"><span data-stu-id="d48b4-120">In the Student.aspx file, simply set **AllowSorting** to **true** in the GridView.</span></span> <span data-ttu-id="d48b4-121">Необходимо задать **SortExpression** значение для каждого столбца, как DataField используется автоматически.</span><span class="sxs-lookup"><span data-stu-id="d48b4-121">You do not need to set a **SortExpression** value for each column as the DataField is automatically used.</span></span> <span data-ttu-id="d48b4-122">GridView изменяет запрос, чтобы включить упорядочение данных по выбранному значению.</span><span class="sxs-lookup"><span data-stu-id="d48b4-122">The GridView modifies the query to include ordering the data by the selected value.</span></span> <span data-ttu-id="d48b4-123">Выделенный ниже код показано добавление, что необходимо сделать включить сортировку.</span><span class="sxs-lookup"><span data-stu-id="d48b4-123">The highlighted code below shows the addition you need to make to enable sorting.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

<span data-ttu-id="d48b4-124">Запустите веб-приложение и протестируйте сортировки записи учащихся по значениям в разных столбцах.</span><span class="sxs-lookup"><span data-stu-id="d48b4-124">Run the web application, and test sorting student records by the values in different columns.</span></span>

![Сортировка учащихся](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a><span data-ttu-id="d48b4-126">Добавление разбиения по страницам</span><span class="sxs-lookup"><span data-stu-id="d48b4-126">Add paging</span></span>

<span data-ttu-id="d48b4-127">Включение разбиения на страницы тоже очень просто.</span><span class="sxs-lookup"><span data-stu-id="d48b4-127">Enabling paging is also very easy.</span></span> <span data-ttu-id="d48b4-128">В случае с GridView, задайте **AllowPaging** свойства **true** и задайте **PageSize** количество записей, которое должно отображаться на каждой странице.</span><span class="sxs-lookup"><span data-stu-id="d48b4-128">In the GridView, set the **AllowPaging** property to **true** and set the **PageSize** property to the number of records you wish to display on each page.</span></span> <span data-ttu-id="d48b4-129">В этом руководстве можно разместить его до 4.</span><span class="sxs-lookup"><span data-stu-id="d48b4-129">In this tutorial, you can set it to 4.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

<span data-ttu-id="d48b4-130">Запустите веб-приложения и обратите внимание, что теперь записи разделяются на несколько страниц с не более 4 записей, отображаемых на одной странице.</span><span class="sxs-lookup"><span data-stu-id="d48b4-130">Run the web application, and notice that now the records are divided over multiple pages with no more than 4 records displayed on a single page.</span></span>

![Добавление разбиения по страницам](sorting-paging-and-filtering-data/_static/image4.png)

<span data-ttu-id="d48b4-132">Отложенное выполнение запросов повышает эффективность работы приложения.</span><span class="sxs-lookup"><span data-stu-id="d48b4-132">Deferred query execution improves the application efficiency.</span></span> <span data-ttu-id="d48b4-133">Вместо извлечения весь набор данных, GridView изменяет запрос, чтобы получить только те записи, для текущей страницы.</span><span class="sxs-lookup"><span data-stu-id="d48b4-133">Instead of retrieving the entire data set, the GridView modifies the query to retrieve only the records for the current page.</span></span>

## <a name="filter-records-by-user-selection"></a><span data-ttu-id="d48b4-134">Отфильтровать путем выбора пользователя</span><span class="sxs-lookup"><span data-stu-id="d48b4-134">Filter records by user selection</span></span>

<span data-ttu-id="d48b4-135">Привязка модели добавляет несколько атрибутов, которые позволяют вам самостоятельно определить, как задать значение для параметра в методе привязки модели.</span><span class="sxs-lookup"><span data-stu-id="d48b4-135">Model binding adds several attributes which enable you to designate how to set the value for a parameter in a model binding method.</span></span> <span data-ttu-id="d48b4-136">Эти атрибуты принадлежат **System.Web.ModelBinding** пространства имен.</span><span class="sxs-lookup"><span data-stu-id="d48b4-136">These attributes are in the **System.Web.ModelBinding** namespace.</span></span> <span data-ttu-id="d48b4-137">В их число входят следующее.</span><span class="sxs-lookup"><span data-stu-id="d48b4-137">They include:</span></span>

- <span data-ttu-id="d48b4-138">Элемент управления</span><span class="sxs-lookup"><span data-stu-id="d48b4-138">Control</span></span>
- <span data-ttu-id="d48b4-139">Куки-файл</span><span class="sxs-lookup"><span data-stu-id="d48b4-139">Cookie</span></span>
- <span data-ttu-id="d48b4-140">Form</span><span class="sxs-lookup"><span data-stu-id="d48b4-140">Form</span></span>
- <span data-ttu-id="d48b4-141">Профиль</span><span class="sxs-lookup"><span data-stu-id="d48b4-141">Profile</span></span>
- <span data-ttu-id="d48b4-142">QueryString</span><span class="sxs-lookup"><span data-stu-id="d48b4-142">QueryString</span></span>
- <span data-ttu-id="d48b4-143">RouteData</span><span class="sxs-lookup"><span data-stu-id="d48b4-143">RouteData</span></span>
- <span data-ttu-id="d48b4-144">Сеанс</span><span class="sxs-lookup"><span data-stu-id="d48b4-144">Session</span></span>
- <span data-ttu-id="d48b4-145">UserProfile</span><span class="sxs-lookup"><span data-stu-id="d48b4-145">UserProfile</span></span>
- <span data-ttu-id="d48b4-146">ViewState</span><span class="sxs-lookup"><span data-stu-id="d48b4-146">ViewState</span></span>

<span data-ttu-id="d48b4-147">В этом руководстве используется значение элемента управления для фильтрации отображаемых записей в GridView.</span><span class="sxs-lookup"><span data-stu-id="d48b4-147">In this tutorial, you will use a control's value to filter which records are displayed in the GridView.</span></span> <span data-ttu-id="d48b4-148">Вы добавите **управления** атрибут методу запроса, который вы создали ранее.</span><span class="sxs-lookup"><span data-stu-id="d48b4-148">You will add the **Control** attribute to the query method you had created earlier.</span></span> <span data-ttu-id="d48b4-149">В [позже](using-query-string-values-to-retrieve-data.md) руководстве, будет применяться **QueryString** атрибут к параметру, чтобы указать, что значение параметра поступает из значения строки запроса.</span><span class="sxs-lookup"><span data-stu-id="d48b4-149">In a [later](using-query-string-values-to-retrieve-data.md) tutorial, you will apply the **QueryString** attribute to a parameter to specify that the parameter value comes from a query string value.</span></span>

<span data-ttu-id="d48b4-150">Во-первых добавьте над ValidationSummary, раскрывающийся список для фильтрации отображаются какие учащихся.</span><span class="sxs-lookup"><span data-stu-id="d48b4-150">First, above the ValidationSummary, add a drop down list for filtering which students are shown.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

<span data-ttu-id="d48b4-151">В файле кода изменить метод select для получения значения из элемента управления и задайте имя параметра, имя элемента управления, который предоставляет значение.</span><span class="sxs-lookup"><span data-stu-id="d48b4-151">In the code-behind file, modify the select method to receive a value from the control, and set the name of the parameter to the name of the control that provides the value.</span></span>

<span data-ttu-id="d48b4-152">Необходимо добавить **с помощью** инструкции для **System.Web.ModelBinding** пространство имен необходимо разрешить атрибут элемента управления.</span><span class="sxs-lookup"><span data-stu-id="d48b4-152">You must add a **using** statement for the **System.Web.ModelBinding** namespace to resolve the Control attribute.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

<span data-ttu-id="d48b4-153">В следующем коде показано метода select, повторно работал для фильтрации возвращаемых данных, на основе значения из раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="d48b4-153">The following code shows the select method re-worked to filter the returned data based on the value of the drop down list.</span></span> <span data-ttu-id="d48b4-154">Добавление атрибута элемента управления, прежде, чем параметр указывает, что значение для этого параметра берется из элемента управления с таким именем.</span><span class="sxs-lookup"><span data-stu-id="d48b4-154">Adding a control attribute before a parameter specifies that the value for this parameter comes from a control with the same name.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

<span data-ttu-id="d48b4-155">Запустите веб-приложение и выберите другие значения в раскрывающемся списке, чтобы отфильтровать список учащихся.</span><span class="sxs-lookup"><span data-stu-id="d48b4-155">Run the web application and select different values from the drop down list to filter the list of students.</span></span>

![Фильтр учащихся](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a><span data-ttu-id="d48b4-157">Заключение</span><span class="sxs-lookup"><span data-stu-id="d48b4-157">Conclusion</span></span>

<span data-ttu-id="d48b4-158">В этом руководстве вы включили, сортировка и разбиение по страницам данных.</span><span class="sxs-lookup"><span data-stu-id="d48b4-158">In this tutorial, you enabled sorting and paging of the data.</span></span> <span data-ttu-id="d48b4-159">Также вы включили, фильтруя данные по значению свойства элемента управления.</span><span class="sxs-lookup"><span data-stu-id="d48b4-159">You also enabled filtering the data by the value of a control.</span></span>

<span data-ttu-id="d48b4-160">В следующем [руководстве](integrating-jquery-ui.md) поможет повысить эффективность пользовательского интерфейса путем интеграции пользовательского интерфейса JQuery мини-приложения в шаблон динамических данных.</span><span class="sxs-lookup"><span data-stu-id="d48b4-160">In the next [tutorial](integrating-jquery-ui.md) you will enhance the UI by integrating a JQuery UI widget into the dynamic data template.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d48b4-161">[Назад](updating-deleting-and-creating-data.md)
> [Вперед](integrating-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="d48b4-161">[Previous](updating-deleting-and-creating-data.md)
[Next](integrating-jquery-ui.md)</span></span>
