---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: "Использование Entity Framework 4.0 и управления ObjectDataSource, часть 3: сортировка и фильтрация | Документы Microsoft"
author: tdykstra
description: "Этот учебник ряд строится на веб-приложение Contoso университета, созданный Приступая к работе с рядами учебника Entity Framework 4.0. Я..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 4accd3381a66bde1f87f0dc7dd95beeb54fcc6a2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a><span data-ttu-id="69800-104">Использование Entity Framework 4.0 и управления ObjectDataSource, часть 3: сортировка и фильтрация</span><span class="sxs-lookup"><span data-stu-id="69800-104">Using the Entity Framework 4.0 and the ObjectDataSource Control, Part 3: Sorting and Filtering</span></span>
====================
<span data-ttu-id="69800-105">По [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="69800-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="69800-106">Этот учебник ряд основан на веб-приложение Contoso университета, созданный [Приступая к работе с Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) учебника рядов.</span><span class="sxs-lookup"><span data-stu-id="69800-106">This tutorial series builds on the Contoso University web application that is created by the [Getting Started with the Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) tutorial series.</span></span> <span data-ttu-id="69800-107">Если не была завершена ранее учебники, в качестве отправной точки для этого учебника вы можете [загрузить приложение](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , будет создана.</span><span class="sxs-lookup"><span data-stu-id="69800-107">If you didn't complete the earlier tutorials, as a starting point for this tutorial you can [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) that you would have created.</span></span> <span data-ttu-id="69800-108">Вы также можете [загрузить приложение](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , создаваемый для завершения учебника ряда.</span><span class="sxs-lookup"><span data-stu-id="69800-108">You can also [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) that is created by the complete tutorial series.</span></span> <span data-ttu-id="69800-109">Если у вас есть вопросы о учебники, их можно разместить [форум по ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).</span><span class="sxs-lookup"><span data-stu-id="69800-109">If you have questions about the tutorials, you can post them to the [ASP.NET Entity Framework forum](https://forums.asp.net/1227.aspx).</span></span>


<span data-ttu-id="69800-110">В предыдущем учебнике реализован шаблон репозитория в n уровневые веб-приложения, использующего платформу Entity Framework и `ObjectDataSource` элемента управления.</span><span class="sxs-lookup"><span data-stu-id="69800-110">In the previous tutorial you implemented the repository pattern in an n-tier web application that uses the Entity Framework and the `ObjectDataSource` control.</span></span> <span data-ttu-id="69800-111">Этого учебника показано, как выполнять сортировку и фильтрацию и обработать основной-подробности.</span><span class="sxs-lookup"><span data-stu-id="69800-111">This tutorial shows how to do sorting and filtering and handle master-detail scenarios.</span></span> <span data-ttu-id="69800-112">Вам предстоит добавить следующие усовершенствования для *Departments.aspx* страницы:</span><span class="sxs-lookup"><span data-stu-id="69800-112">You'll add the following enhancements to the *Departments.aspx* page:</span></span>

- <span data-ttu-id="69800-113">Текстовое поле, чтобы разрешить пользователям выбирать подразделения по имени.</span><span class="sxs-lookup"><span data-stu-id="69800-113">A text box to allow users to select departments by name.</span></span>
- <span data-ttu-id="69800-114">Список курсов для каждого отдела, который отображается в сетке.</span><span class="sxs-lookup"><span data-stu-id="69800-114">A list of courses for each department that's shown in the grid.</span></span>
- <span data-ttu-id="69800-115">Возможность сортировать, щелкая заголовки столбцов.</span><span class="sxs-lookup"><span data-stu-id="69800-115">The ability to sort by clicking column headings.</span></span>

<span data-ttu-id="69800-116">[!["Image01"](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="69800-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span></span>

## <a name="adding-the-ability-to-sort-gridview-columns"></a><span data-ttu-id="69800-117">Добавление возможности сортировки столбцов GridView</span><span class="sxs-lookup"><span data-stu-id="69800-117">Adding the Ability to Sort GridView Columns</span></span>

<span data-ttu-id="69800-118">Откройте *Departments.aspx* и добавьте `SortParameterName="sortExpression"` атрибут `ObjectDataSource` управления с именем `DepartmentsObjectDataSource`.</span><span class="sxs-lookup"><span data-stu-id="69800-118">Open the *Departments.aspx* page and add a `SortParameterName="sortExpression"` attribute to the `ObjectDataSource` control named `DepartmentsObjectDataSource`.</span></span> <span data-ttu-id="69800-119">(Позже вы создадите `GetDepartments` метод, который принимает параметр с именем `sortExpression`.) Разметку для открывающего тега элемента управления теперь приведенной в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="69800-119">(Later you'll create a `GetDepartments` method that takes a parameter named `sortExpression`.) The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

<span data-ttu-id="69800-120">Добавить `AllowSorting="true"` для открывающего тега элемента атрибута `GridView` элемента управления.</span><span class="sxs-lookup"><span data-stu-id="69800-120">Add the `AllowSorting="true"` attribute to the opening tag of the `GridView` control.</span></span> <span data-ttu-id="69800-121">Разметку для открывающего тега элемента управления теперь приведенной в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="69800-121">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

<span data-ttu-id="69800-122">В *Departments.aspx.cs*, задать порядок сортировки по умолчанию, вызвав `GridView` элемента управления `Sort` метод `Page_Load` метод:</span><span class="sxs-lookup"><span data-stu-id="69800-122">In *Departments.aspx.cs*, set the default sort order by calling the `GridView` control's `Sort` method from the `Page_Load` method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

<span data-ttu-id="69800-123">Можно добавить код, который сортирует или фильтрует бизнес логики класса или класса репозитория.</span><span class="sxs-lookup"><span data-stu-id="69800-123">You can add code that sorts or filters in either the business logic class or the repository class.</span></span> <span data-ttu-id="69800-124">Если сделать это в классе логику, сортировке или фильтрации рабочих выполняются после извлечения данных из базы данных, так как логика классе работает с `IEnumerable` объект, возвращаемый в репозиторий.</span><span class="sxs-lookup"><span data-stu-id="69800-124">If you do it in the business logic class, the sorting or filtering work will be done after the data is retrieved from the database, because the business logic class is working with an `IEnumerable` object returned by the repository.</span></span> <span data-ttu-id="69800-125">Если добавить сортировку и фильтрацию кода в классе репозитория и можно сделать перед выражение LINQ или запроса объектов был преобразован в `IEnumerable` объекта команды будут передаваться в базу данных для обработки, который обычно более эффективно.</span><span class="sxs-lookup"><span data-stu-id="69800-125">If you add sorting and filtering code in the repository class and you do it before a LINQ expression or object query has been converted to an `IEnumerable` object, your commands will be passed through to the database for processing, which is typically more efficient.</span></span> <span data-ttu-id="69800-126">В этом учебнике будет реализован сортировки и фильтрации, в результате которого вызывает обработку, которые необходимо выполнить в базе данных, то есть в репозитории.</span><span class="sxs-lookup"><span data-stu-id="69800-126">In this tutorial you'll implement sorting and filtering in a way that causes the processing to be done by the database — that is, in the repository.</span></span>

<span data-ttu-id="69800-127">Чтобы добавить функцию сортировки, необходимо добавить новый метод в репозиторий интерфейс и классы репозитория также относительно бизнес логики класса.</span><span class="sxs-lookup"><span data-stu-id="69800-127">To add sorting capability, you must add a new method to the repository interface and repository classes as well as to the business logic class.</span></span> <span data-ttu-id="69800-128">В *ISchoolRepository.cs* файл, добавьте новый `GetDepartments` метода, принимающего `sortExpression` параметр, который будет использоваться для сортировки списка отделов, возвращается:</span><span class="sxs-lookup"><span data-stu-id="69800-128">In the *ISchoolRepository.cs* file, add a new `GetDepartments` method that takes a `sortExpression` parameter that will be used to sort the list of departments that's returned:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

<span data-ttu-id="69800-129">`sortExpression` Параметр будет задавать столбец для сортировки и направление сортировки.</span><span class="sxs-lookup"><span data-stu-id="69800-129">The `sortExpression` parameter will specify the column to sort on and the sort direction.</span></span>

<span data-ttu-id="69800-130">Добавьте код для нового метода для *SchoolRepository.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="69800-130">Add code for the new method to the *SchoolRepository.cs* file:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

<span data-ttu-id="69800-131">Изменение существующих параметров `GetDepartments` для вызова нового метода:</span><span class="sxs-lookup"><span data-stu-id="69800-131">Change the existing parameterless `GetDepartments` method to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

<span data-ttu-id="69800-132">В тестовом проекте, добавьте следующий новый метод *MockSchoolRepository.cs*:</span><span class="sxs-lookup"><span data-stu-id="69800-132">In the test project, add the following new method to *MockSchoolRepository.cs*:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

<span data-ttu-id="69800-133">Если планируется создать модульные тесты, зависят от этого метода возврат отсортированного списка, необходимо отсортировать список перед возвращением.</span><span class="sxs-lookup"><span data-stu-id="69800-133">If you were going to create any unit tests that depended on this method returning a sorted list, you would need to sort the list before returning it.</span></span> <span data-ttu-id="69800-134">Вы не будет создавать тесты так, в этом учебнике, этот метод может возвращать только неупорядоченного списка отделов.</span><span class="sxs-lookup"><span data-stu-id="69800-134">You won't be creating tests like that in this tutorial, so the method can just return the unsorted list of departments.</span></span>

<span data-ttu-id="69800-135">В *SchoolBL.cs* файл, добавьте следующий новый метод в классе логики:</span><span class="sxs-lookup"><span data-stu-id="69800-135">In the *SchoolBL.cs* file, add the following new method to the business logic class:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

<span data-ttu-id="69800-136">Этот код передает параметр сортировки методу репозитория.</span><span class="sxs-lookup"><span data-stu-id="69800-136">This code passes the sort parameter to the repository method.</span></span>

<span data-ttu-id="69800-137">Запустите *Departments.aspx* страницы.</span><span class="sxs-lookup"><span data-stu-id="69800-137">Run the *Departments.aspx* page.</span></span>

<span data-ttu-id="69800-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="69800-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span></span>

<span data-ttu-id="69800-139">Теперь можно щелкнуть любой заголовок столбца для сортировки по этому столбцу.</span><span class="sxs-lookup"><span data-stu-id="69800-139">You can now click any column heading to sort by that column.</span></span> <span data-ttu-id="69800-140">Если столбец уже отсортированы, щелкнув заголовок обращает направление сортировки.</span><span class="sxs-lookup"><span data-stu-id="69800-140">If the column is already sorted, clicking the heading reverses the sort direction.</span></span>

## <a name="adding-a-search-box"></a><span data-ttu-id="69800-141">Добавление поля поиска</span><span class="sxs-lookup"><span data-stu-id="69800-141">Adding a Search Box</span></span>

<span data-ttu-id="69800-142">В этом разделе предстоит добавить текстовое поле поиска, связывание его с `ObjectDataSource` управлять с помощью параметра управления и добавить метод в классе логику для поддержки фильтрации.</span><span class="sxs-lookup"><span data-stu-id="69800-142">In this section you'll add a search text box, link it to the `ObjectDataSource` control using a control parameter, and add a method to the business logic class to support filtering.</span></span>

<span data-ttu-id="69800-143">Откройте *Departments.aspx* и добавьте следующую разметку между заголовком и первым `ObjectDataSource` управления:</span><span class="sxs-lookup"><span data-stu-id="69800-143">Open the *Departments.aspx* page and add the following markup between the heading and the first `ObjectDataSource` control:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

<span data-ttu-id="69800-144">В `ObjectDataSource` управления с именем `DepartmentsObjectDataSource`, выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="69800-144">In the `ObjectDataSource` control named `DepartmentsObjectDataSource`, do the following:</span></span>

- <span data-ttu-id="69800-145">Добавить `SelectParameters` элемент для параметра с именем `nameSearchString` , возвращает значение, введенное в `SearchTextBox` элемента управления.</span><span class="sxs-lookup"><span data-stu-id="69800-145">Add a `SelectParameters` element for a parameter named `nameSearchString` that gets the value entered in the `SearchTextBox` control.</span></span>
- <span data-ttu-id="69800-146">Изменение `SelectMethod` значение для атрибута `GetDepartmentsByName`.</span><span class="sxs-lookup"><span data-stu-id="69800-146">Change the `SelectMethod` attribute value to `GetDepartmentsByName`.</span></span> <span data-ttu-id="69800-147">(Вы создадите этот метод более поздней версии.)</span><span class="sxs-lookup"><span data-stu-id="69800-147">(You'll create this method later.)</span></span>

<span data-ttu-id="69800-148">Разметка для `ObjectDataSource` теперь элемент управления похож на приведенный ниже:</span><span class="sxs-lookup"><span data-stu-id="69800-148">The markup for the `ObjectDataSource` control now resembles the following example:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

<span data-ttu-id="69800-149">В *ISchoolRepository.cs*, добавьте `GetDepartmentsByName` метода, принимающего оба `sortExpression` и `nameSearchString` параметры:</span><span class="sxs-lookup"><span data-stu-id="69800-149">In *ISchoolRepository.cs*, add a `GetDepartmentsByName` method that takes both `sortExpression` and `nameSearchString` parameters:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

<span data-ttu-id="69800-150">В *SchoolRepository.cs*, добавьте следующий новый метод:</span><span class="sxs-lookup"><span data-stu-id="69800-150">In *SchoolRepository.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

<span data-ttu-id="69800-151">Этот код использует `Where` метод, чтобы выбрать элементы, которые содержат строку поиска.</span><span class="sxs-lookup"><span data-stu-id="69800-151">This code uses a `Where` method to select items that contain the search string.</span></span> <span data-ttu-id="69800-152">Если строка поиска пуста, будет выбрано все записи.</span><span class="sxs-lookup"><span data-stu-id="69800-152">If the search string is empty, all records will be selected.</span></span> <span data-ttu-id="69800-153">Обратите внимание, что при указании вызовы методов вместе в одной инструкции следующим образом (`Include`, затем `OrderBy`, затем `Where`), `Where` метод всегда должен быть последним.</span><span class="sxs-lookup"><span data-stu-id="69800-153">Note that when you specify method calls together in one statement like this (`Include`, then `OrderBy`, then `Where`), the `Where` method must always be last.</span></span>

<span data-ttu-id="69800-154">Изменить существующий `GetDepartments` метода, принимающего `sortExpression` параметров для вызова нового метода:</span><span class="sxs-lookup"><span data-stu-id="69800-154">Change the existing `GetDepartments` method that takes a `sortExpression` parameter to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

<span data-ttu-id="69800-155">В *MockSchoolRepository.cs* в тестовом проекте, добавьте следующий новый метод:</span><span class="sxs-lookup"><span data-stu-id="69800-155">In *MockSchoolRepository.cs* in the test project, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

<span data-ttu-id="69800-156">В *SchoolBL.cs*, добавьте следующий новый метод:</span><span class="sxs-lookup"><span data-stu-id="69800-156">In *SchoolBL.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

<span data-ttu-id="69800-157">Запустите *Departments.aspx* страницы и введите строку поиска, чтобы убедиться в том, что работает логику выбора.</span><span class="sxs-lookup"><span data-stu-id="69800-157">Run the *Departments.aspx* page and enter a search string to make sure that the selection logic works.</span></span> <span data-ttu-id="69800-158">Оставьте поле пустым и повторите поиск, чтобы убедиться в том, что возвращаются все записи.</span><span class="sxs-lookup"><span data-stu-id="69800-158">Leave the text box empty and try a search to make sure that all records are returned.</span></span>

<span data-ttu-id="69800-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="69800-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span></span>

## <a name="adding-a-details-column-for-each-grid-row"></a><span data-ttu-id="69800-160">Добавление столбца сведений для каждой строки сетки</span><span class="sxs-lookup"><span data-stu-id="69800-160">Adding a Details Column for Each Grid Row</span></span>

<span data-ttu-id="69800-161">Теперь можно увидеть все курсов для каждого отдела, отображаемых в правую ячейку сетки.</span><span class="sxs-lookup"><span data-stu-id="69800-161">Next, you want to see all of the courses for each department displayed in the right-hand cell of the grid.</span></span> <span data-ttu-id="69800-162">Для этого потребуется использовать вложенную `GridView` управления и привязки его к данным из `Courses` свойство навигации `Department` сущности.</span><span class="sxs-lookup"><span data-stu-id="69800-162">To do this, you'll use a nested `GridView` control and databind it to data from the `Courses` navigation property of the `Department` entity.</span></span>

<span data-ttu-id="69800-163">Откройте *Departments.aspx* и разметки `GridView` управления, указать обработчик для `RowDataBound` события.</span><span class="sxs-lookup"><span data-stu-id="69800-163">Open *Departments.aspx* and in the markup for the `GridView` control, specify a handler for the `RowDataBound` event.</span></span> <span data-ttu-id="69800-164">Разметку для открывающего тега элемента управления теперь приведенной в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="69800-164">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

<span data-ttu-id="69800-165">Добавьте новый `TemplateField` элемента после `Administrator` поле шаблона:</span><span class="sxs-lookup"><span data-stu-id="69800-165">Add a new `TemplateField` element after the `Administrator` template field:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

<span data-ttu-id="69800-166">Эта разметка создает вложенную `GridView` управления, отображающий номер и название списка курсов.</span><span class="sxs-lookup"><span data-stu-id="69800-166">This markup creates a nested `GridView` control that shows the course number and title of a list of courses.</span></span> <span data-ttu-id="69800-167">Не указывает источник данных, так как вы будете databind его в коде в `RowDataBound` обработчика.</span><span class="sxs-lookup"><span data-stu-id="69800-167">It does not specify a data source because you'll databind it in code in the `RowDataBound` handler.</span></span>

<span data-ttu-id="69800-168">Откройте *Departments.aspx.cs* и добавьте следующий обработчик `RowDataBound` событий:</span><span class="sxs-lookup"><span data-stu-id="69800-168">Open *Departments.aspx.cs* and add the following handler for the `RowDataBound` event:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

<span data-ttu-id="69800-169">Этот код получает `Department` преобразует сущности из аргументов события `Courses` свойство навигации для `List` коллекции, а также осуществляет вложенного `GridView` в коллекцию.</span><span class="sxs-lookup"><span data-stu-id="69800-169">This code gets the `Department` entity from the event arguments, converts the `Courses` navigation property to a `List` collection, and databinds the nested `GridView` to the collection.</span></span>

<span data-ttu-id="69800-170">Откройте *SchoolRepository.cs* файла и укажите упреждающую для `Courses` свойство навигации, вызвав `Include` метод в запросе объекта, создаваемого в `GetDepartmentsByName` метод.</span><span class="sxs-lookup"><span data-stu-id="69800-170">Open the *SchoolRepository.cs* file and specify eager loading for the `Courses` navigation property by calling the `Include` method in the object query that you create in the `GetDepartmentsByName` method.</span></span> <span data-ttu-id="69800-171">`return` Инструкции в `GetDepartmentsByName` метод теперь приведенной в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="69800-171">The `return` statement in the `GetDepartmentsByName` method now resembles the following example.</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

<span data-ttu-id="69800-172">Запустите страницу.</span><span class="sxs-lookup"><span data-stu-id="69800-172">Run the page.</span></span> <span data-ttu-id="69800-173">Помимо фильтрации и сортировки возможностей, который был добавлен ранее элемента управления GridView теперь отображаются сведения о вложенных курса для каждого отдела.</span><span class="sxs-lookup"><span data-stu-id="69800-173">In addition to the sorting and filtering capability that you added earlier, the GridView control now shows nested course details for each department.</span></span>

<span data-ttu-id="69800-174">[!["Image01"](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="69800-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span></span>

<span data-ttu-id="69800-175">Введение в сценарии сортировки, фильтрации и подробный завершена.</span><span class="sxs-lookup"><span data-stu-id="69800-175">This completes the introduction to sorting, filtering, and master-detail scenarios.</span></span> <span data-ttu-id="69800-176">В следующем уроке вы увидите, как обрабатывать параллелизма.</span><span class="sxs-lookup"><span data-stu-id="69800-176">In the next tutorial, you'll see how to handle concurrency.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="69800-177">[Назад](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
[Вперед](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span><span class="sxs-lookup"><span data-stu-id="69800-177">[Previous](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
[Next](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span></span>
