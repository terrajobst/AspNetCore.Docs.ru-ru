---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Использование строковых значений запросов для фильтрации данных с помощью привязки модели и веб-формы | Документация Майкрософт
author: tfitzmac
description: В этой серии руководств показано основными аспектами с помощью привязки модели с проектом веб-форм ASP.NET. Привязка модели позволяет взаимодействие с данными более прямой-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 495489479ef912afcb89c267b56fb11e07f959ec
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374734"
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a><span data-ttu-id="0e1cf-104">Использование строковых значений запросов для фильтрации данных с помощью привязки модели и веб-форм</span><span class="sxs-lookup"><span data-stu-id="0e1cf-104">Using query string values to filter data with model binding and web forms</span></span>
====================
<span data-ttu-id="0e1cf-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="0e1cf-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="0e1cf-106">В этой серии руководств показано основными аспектами с помощью привязки модели с проектом веб-форм ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0e1cf-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="0e1cf-107">Привязка модели упрощает взаимодействие с данными более эффективную чем работа с данными объектов источника (например, элемент управления ObjectDataSource и SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="0e1cf-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="0e1cf-108">Эта серия начинается с вводный материал и перемещает до более продвинутых в последующих руководствах.</span><span class="sxs-lookup"><span data-stu-id="0e1cf-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="0e1cf-109">Этом руководстве показано, как передать значение в строке запроса и использовать его для получения данных через привязки модели.</span><span class="sxs-lookup"><span data-stu-id="0e1cf-109">This tutorial shows how to pass a value in the query string and use that value to retrieve data through model binding.</span></span>
> 
> <span data-ttu-id="0e1cf-110">Этот учебник основан на проект, созданный в [более ранних](retrieving-data.md) части серии.</span><span class="sxs-lookup"><span data-stu-id="0e1cf-110">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="0e1cf-111">Вы можете [загрузить](https://go.microsoft.com/fwlink/?LinkId=286116) весь проект полностью на C# или Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="0e1cf-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="0e1cf-112">Загружаемый код работает с Visual Studio 2012 или Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="0e1cf-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="0e1cf-113">Он использует шаблон Visual Studio 2012, который немного отличается от шаблона Visual Studio 2013, в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="0e1cf-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="0e1cf-114">Вы создадите</span><span class="sxs-lookup"><span data-stu-id="0e1cf-114">What you'll build</span></span>

<span data-ttu-id="0e1cf-115">В этом руководстве вам потребуется:</span><span class="sxs-lookup"><span data-stu-id="0e1cf-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="0e1cf-116">Добавьте новую страницу для отображения зарегистрированных курсов для учащегося</span><span class="sxs-lookup"><span data-stu-id="0e1cf-116">Add a new page to show the enrolled courses for a student</span></span>
2. <span data-ttu-id="0e1cf-117">Получить зарегистрированных курсов для выбранного учащегося на основе значения в строке запроса</span><span class="sxs-lookup"><span data-stu-id="0e1cf-117">Retrieve the enrolled courses for the selected student based on a value in the query string</span></span>
3. <span data-ttu-id="0e1cf-118">Добавить гиперссылку с значения строки запроса из представления сетки на новую страницу</span><span class="sxs-lookup"><span data-stu-id="0e1cf-118">Add a hyperlink with a query string value from the grid view to the new page</span></span>

<span data-ttu-id="0e1cf-119">Действия, описанные в этом руководстве довольно похожи на что вы сделали в более ранней [руководстве](sorting-paging-and-filtering-data.md) для фильтрации отображаемых учащихся, в зависимости от выбора пользователя в раскрывающемся списке.</span><span class="sxs-lookup"><span data-stu-id="0e1cf-119">The steps in this tutorial are fairly similar to what you did in the earlier [tutorial](sorting-paging-and-filtering-data.md) to filter the displayed students based on the user selection in a drop down list.</span></span> <span data-ttu-id="0e1cf-120">В этом учебнике вы использовали **управления** атрибут в метод select, чтобы указать, что значение параметра поступает из элемента управления.</span><span class="sxs-lookup"><span data-stu-id="0e1cf-120">In that tutorial, you used the **Control** attribute in the select method to specify that the parameter value comes from a control.</span></span> <span data-ttu-id="0e1cf-121">В этом руководстве мы воспользуемся **QueryString** атрибут в метод select, чтобы указать, что значение параметра поступает из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="0e1cf-121">In this tutorial, you'll use the **QueryString** attribute in the select method to specify that the parameter value comes from the query string.</span></span>

## <a name="add-new-page-for-displaying-a-students-courses"></a><span data-ttu-id="0e1cf-122">Добавить новую страницу для отображения курсов учащихся</span><span class="sxs-lookup"><span data-stu-id="0e1cf-122">Add new page for displaying a student's courses</span></span>

<span data-ttu-id="0e1cf-123">Добавление новой веб-формы, который использует страницей Site.master и назовите страницу **курсы**.</span><span class="sxs-lookup"><span data-stu-id="0e1cf-123">Add a new web form that uses the Site.master master page, and name the page **Courses**.</span></span>

<span data-ttu-id="0e1cf-124">В **Courses.aspx** добавьте представление сетки для отображения курсов для выбранного учащегося.</span><span class="sxs-lookup"><span data-stu-id="0e1cf-124">In the **Courses.aspx** file, add a grid view to display the courses for the selected student.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a><span data-ttu-id="0e1cf-125">Определите метод select</span><span class="sxs-lookup"><span data-stu-id="0e1cf-125">Define the select method</span></span>

<span data-ttu-id="0e1cf-126">В **Courses.aspx.cs**, добавляемые с помощью метода select с именем, указанным в представлении сетки **SelectMethod** свойство.</span><span class="sxs-lookup"><span data-stu-id="0e1cf-126">In **Courses.aspx.cs**, you will add the select method with the name you specified in the grid view's **SelectMethod** property.</span></span> <span data-ttu-id="0e1cf-127">В этот метод вы определите запрос для извлечения Стьюдента курсов и указать, что параметр берется из значения строки запроса с тем же именем, в качестве параметра.</span><span class="sxs-lookup"><span data-stu-id="0e1cf-127">In that method, you'll define the query for retrieving a student's courses, and specify that the parameter comes from a query string value with the same name as the parameter.</span></span>

<span data-ttu-id="0e1cf-128">Во-первых, необходимо добавить следующие **с помощью** инструкций.</span><span class="sxs-lookup"><span data-stu-id="0e1cf-128">First, you must add the following **using** statements.</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

<span data-ttu-id="0e1cf-129">Затем добавьте следующий код к Courses.aspx.cs:</span><span class="sxs-lookup"><span data-stu-id="0e1cf-129">Then, add the following code to Courses.aspx.cs:</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

<span data-ttu-id="0e1cf-130">Атрибут строки запроса означает, что значения строки запроса с именем StudentID автоматически назначается параметр в этом методе.</span><span class="sxs-lookup"><span data-stu-id="0e1cf-130">The QueryString attribute means that a query string value named StudentID is automatically assigned to the parameter in this method.</span></span>

## <a name="add-hyperlink-with-query-string-value"></a><span data-ttu-id="0e1cf-131">Добавление гиперссылки с помощью значения строки запроса</span><span class="sxs-lookup"><span data-stu-id="0e1cf-131">Add hyperlink with query string value</span></span>

<span data-ttu-id="0e1cf-132">В представлении в виде сетки на Students.aspx вы добавите поле гиперссылки, ссылающейся на новую страницу курсов.</span><span class="sxs-lookup"><span data-stu-id="0e1cf-132">In the grid view on Students.aspx, you will add a hyperlink field that links to your new Courses page.</span></span> <span data-ttu-id="0e1cf-133">Гиперссылка будет включать значения строки запроса с идентификатором учащегося.</span><span class="sxs-lookup"><span data-stu-id="0e1cf-133">The hyperlink will include a query string value with the student's id.</span></span>

<span data-ttu-id="0e1cf-134">В Students.aspx добавьте следующее поле к столбцам представления сетки сразу же после поля для суммарные кредиты.</span><span class="sxs-lookup"><span data-stu-id="0e1cf-134">In Students.aspx, add the following field to the grid view columns just below the field for Total Credits.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

<span data-ttu-id="0e1cf-135">Запустите приложение и обратите внимание на то, что в представлении в виде сетки теперь включает ссылку курсов.</span><span class="sxs-lookup"><span data-stu-id="0e1cf-135">Run the application and notice that the grid view now includes the Courses link.</span></span>

![Добавление гиперссылки](using-query-string-values-to-retrieve-data/_static/image1.png)

<span data-ttu-id="0e1cf-137">Если щелкнуть одну из ссылок, вы увидите зарегистрированных курсов этого учащегося.</span><span class="sxs-lookup"><span data-stu-id="0e1cf-137">When you click one of the links, you'll see that student's enrolled courses.</span></span>

![Показать курсы](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="0e1cf-139">Заключение</span><span class="sxs-lookup"><span data-stu-id="0e1cf-139">Conclusion</span></span>

<span data-ttu-id="0e1cf-140">В этом руководстве вы добавили ссылку со значением строки запроса.</span><span class="sxs-lookup"><span data-stu-id="0e1cf-140">In this tutorial, you added a link with a query string value.</span></span> <span data-ttu-id="0e1cf-141">Вы использовали, значение строки запроса для значения параметра в метод select.</span><span class="sxs-lookup"><span data-stu-id="0e1cf-141">You used that query string value for the parameter value in the select method.</span></span>

<span data-ttu-id="0e1cf-142">В следующем [руководстве](adding-business-logic-layer.md), будет выполнено перемещение кода из файлов кода в слой бизнес-логики и доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="0e1cf-142">In the next [tutorial](adding-business-logic-layer.md), you will move the code from the code-behind files into a business logic layer and a data access layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0e1cf-143">[Назад](integrating-jquery-ui.md)
> [Вперед](adding-business-logic-layer.md)</span><span class="sxs-lookup"><span data-stu-id="0e1cf-143">[Previous](integrating-jquery-ui.md)
[Next](adding-business-logic-layer.md)</span></span>
