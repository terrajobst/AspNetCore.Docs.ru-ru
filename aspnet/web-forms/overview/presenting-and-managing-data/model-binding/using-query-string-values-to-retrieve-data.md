---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: С помощью значения строки запроса для фильтрации данных с использованием привязки модели и веб-формы | Документы Microsoft
author: tfitzmac
description: Этот учебник ряд демонстрирует основные аспекты использования привязки модели в проекте веб-форм ASP.NET. Привязка модели позволяет взаимодействия с данными дополнительные прямые-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 03d20decf0eeff6062fbc6f8dd66f644b405c7cc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a><span data-ttu-id="4bb68-104">С помощью значения строки запроса для фильтрации данных с привязкой модели и веб-форм</span><span class="sxs-lookup"><span data-stu-id="4bb68-104">Using query string values to filter data with model binding and web forms</span></span>
====================
<span data-ttu-id="4bb68-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="4bb68-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="4bb68-106">Этот учебник ряд демонстрирует основные аспекты использования привязки модели в проекте веб-форм ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4bb68-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="4bb68-107">Привязка модели позволяет взаимодействия с данными более прост, чем работе с данными источника объектов (например, элемент управления ObjectDataSource или SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="4bb68-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="4bb68-108">Эта серия начинается с вводный материал и перемещает на следующих занятиях рассматривается более сложных понятиях.</span><span class="sxs-lookup"><span data-stu-id="4bb68-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="4bb68-109">Этого учебника показано, как передать значение в строку запроса и использовать это значение для получения данных через привязки модели.</span><span class="sxs-lookup"><span data-stu-id="4bb68-109">This tutorial shows how to pass a value in the query string and use that value to retrieve data through model binding.</span></span>
> 
> <span data-ttu-id="4bb68-110">Этот учебник построен на проект, созданный в [более ранних](retrieving-data.md) части ряда.</span><span class="sxs-lookup"><span data-stu-id="4bb68-110">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="4bb68-111">Вы можете [загрузки](https://go.microsoft.com/fwlink/?LinkId=286116) весь проект C# или Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="4bb68-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="4bb68-112">Загружаемый код работает с Visual Studio 2012 или Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="4bb68-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="4bb68-113">Она использует шаблон Visual Studio 2012, который немного отличается от шаблона Visual Studio 2013, показанными в данном руководстве.</span><span class="sxs-lookup"><span data-stu-id="4bb68-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="4bb68-114">Мы создадим</span><span class="sxs-lookup"><span data-stu-id="4bb68-114">What you'll build</span></span>

<span data-ttu-id="4bb68-115">В этом учебнике вы сможете:</span><span class="sxs-lookup"><span data-stu-id="4bb68-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="4bb68-116">Добавьте новую страницу, чтобы отобразить Зарегистрированные курсы для учащихся</span><span class="sxs-lookup"><span data-stu-id="4bb68-116">Add a new page to show the enrolled courses for a student</span></span>
2. <span data-ttu-id="4bb68-117">Получить зарегистрированных курсов для выбранного учащегося на основе значения в строке запроса</span><span class="sxs-lookup"><span data-stu-id="4bb68-117">Retrieve the enrolled courses for the selected student based on a value in the query string</span></span>
3. <span data-ttu-id="4bb68-118">Добавить гиперссылку со значением строки запроса из представления сетки на новую страницу</span><span class="sxs-lookup"><span data-stu-id="4bb68-118">Add a hyperlink with a query string value from the grid view to the new page</span></span>

<span data-ttu-id="4bb68-119">Действия, описанные в этом учебнике довольно аналогично тому, что вы делали в предыдущих [учебника](sorting-paging-and-filtering-data.md) для фильтрации отображаемых учащихся, основанному на выборе пользователя в раскрывающемся списке.</span><span class="sxs-lookup"><span data-stu-id="4bb68-119">The steps in this tutorial are fairly similar to what you did in the earlier [tutorial](sorting-paging-and-filtering-data.md) to filter the displayed students based on the user selection in a drop down list.</span></span> <span data-ttu-id="4bb68-120">В этом учебнике используется **управления** атрибута в методе select, чтобы указать, что значение параметра берется из элемента управления.</span><span class="sxs-lookup"><span data-stu-id="4bb68-120">In that tutorial, you used the **Control** attribute in the select method to specify that the parameter value comes from a control.</span></span> <span data-ttu-id="4bb68-121">В этом учебнике мы используем **QueryString** атрибута в методе select, чтобы указать, что значение параметра берется из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="4bb68-121">In this tutorial, you'll use the **QueryString** attribute in the select method to specify that the parameter value comes from the query string.</span></span>

## <a name="add-new-page-for-displaying-a-students-courses"></a><span data-ttu-id="4bb68-122">Добавить новую страницу для отображения Стьюдента курсов</span><span class="sxs-lookup"><span data-stu-id="4bb68-122">Add new page for displaying a student's courses</span></span>

<span data-ttu-id="4bb68-123">Добавление нового веб-формы, использующей главную страницу Site.master и назовите страницу **курсы**.</span><span class="sxs-lookup"><span data-stu-id="4bb68-123">Add a new web form that uses the Site.master master page, and name the page **Courses**.</span></span>

<span data-ttu-id="4bb68-124">В **Courses.aspx** файл, добавить представление сетки для отображения курсов для выбранного учащегося.</span><span class="sxs-lookup"><span data-stu-id="4bb68-124">In the **Courses.aspx** file, add a grid view to display the courses for the selected student.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a><span data-ttu-id="4bb68-125">Определение метода select</span><span class="sxs-lookup"><span data-stu-id="4bb68-125">Define the select method</span></span>

<span data-ttu-id="4bb68-126">В **Courses.aspx.cs**, вы добавите выберите метод с именем, указанным в представлении сетки **SelectMethod** свойство.</span><span class="sxs-lookup"><span data-stu-id="4bb68-126">In **Courses.aspx.cs**, you will add the select method with the name you specified in the grid view's **SelectMethod** property.</span></span> <span data-ttu-id="4bb68-127">В этом методе будет определить запрос для получения Стьюдента курсов, а также указать, что параметр берется из значение строки запроса с тем же именем в качестве параметра.</span><span class="sxs-lookup"><span data-stu-id="4bb68-127">In that method, you'll define the query for retrieving a student's courses, and specify that the parameter comes from a query string value with the same name as the parameter.</span></span>

<span data-ttu-id="4bb68-128">Во-первых, необходимо добавить следующие **с помощью** инструкции.</span><span class="sxs-lookup"><span data-stu-id="4bb68-128">First, you must add the following **using** statements.</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

<span data-ttu-id="4bb68-129">Затем добавьте следующий код Courses.aspx.cs:</span><span class="sxs-lookup"><span data-stu-id="4bb68-129">Then, add the following code to Courses.aspx.cs:</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

<span data-ttu-id="4bb68-130">Атрибута QueryString означает, что значение строки запроса с именем идентификатором StudentID автоматически назначается параметр этого метода.</span><span class="sxs-lookup"><span data-stu-id="4bb68-130">The QueryString attribute means that a query string value named StudentID is automatically assigned to the parameter in this method.</span></span>

## <a name="add-hyperlink-with-query-string-value"></a><span data-ttu-id="4bb68-131">Добавьте гиперссылку на значение строки запроса</span><span class="sxs-lookup"><span data-stu-id="4bb68-131">Add hyperlink with query string value</span></span>

<span data-ttu-id="4bb68-132">В представлении сетки на Students.aspx добавляется поле гиперссылки, ссылающуюся на новую страницу курсов.</span><span class="sxs-lookup"><span data-stu-id="4bb68-132">In the grid view on Students.aspx, you will add a hyperlink field that links to your new Courses page.</span></span> <span data-ttu-id="4bb68-133">Гиперссылка будет содержать значение строки запроса с идентификатором Стьюдента.</span><span class="sxs-lookup"><span data-stu-id="4bb68-133">The hyperlink will include a query string value with the student's id.</span></span>

<span data-ttu-id="4bb68-134">В Students.aspx добавьте следующее поле к столбцам представления сетки под поле для всего кредиты.</span><span class="sxs-lookup"><span data-stu-id="4bb68-134">In Students.aspx, add the following field to the grid view columns just below the field for Total Credits.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

<span data-ttu-id="4bb68-135">Запустите приложение и обратите внимание, представления сетки теперь содержит курсы ссылку.</span><span class="sxs-lookup"><span data-stu-id="4bb68-135">Run the application and notice that the grid view now includes the Courses link.</span></span>

![Добавление гиперссылки](using-query-string-values-to-retrieve-data/_static/image1.png)

<span data-ttu-id="4bb68-137">Если щелкнуть одну из ссылок, вы увидите этого студента зарегистрированных курсов.</span><span class="sxs-lookup"><span data-stu-id="4bb68-137">When you click one of the links, you'll see that student's enrolled courses.</span></span>

![Показать курсы](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="4bb68-139">Заключение</span><span class="sxs-lookup"><span data-stu-id="4bb68-139">Conclusion</span></span>

<span data-ttu-id="4bb68-140">В этом учебнике вы добавили ссылку со значением строки запроса.</span><span class="sxs-lookup"><span data-stu-id="4bb68-140">In this tutorial, you added a link with a query string value.</span></span> <span data-ttu-id="4bb68-141">Используется, значение строки запроса для значения параметра в методе select.</span><span class="sxs-lookup"><span data-stu-id="4bb68-141">You used that query string value for the parameter value in the select method.</span></span>

<span data-ttu-id="4bb68-142">В следующем [учебника](adding-business-logic-layer.md), будет переместить код из файлов кода в слой бизнес-логики и доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="4bb68-142">In the next [tutorial](adding-business-logic-layer.md), you will move the code from the code-behind files into a business logic layer and a data access layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4bb68-143">[Назад](integrating-jquery-ui.md)
> [Вперед](adding-business-logic-layer.md)</span><span class="sxs-lookup"><span data-stu-id="4bb68-143">[Previous](integrating-jquery-ui.md)
[Next](adding-business-logic-layer.md)</span></span>
