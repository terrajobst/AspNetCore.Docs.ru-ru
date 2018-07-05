---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Добавление уровня бизнес-логики в проект, использующий привязки модели и веб-форм | Документация Майкрософт
author: tfitzmac
description: В этой серии руководств показано основными аспектами с помощью привязки модели с проектом веб-форм ASP.NET. Привязка модели позволяет взаимодействие с данными более прямой-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: be69bf56c6a1c5bf601a47e90e4e1c67c48a760f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386504"
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a><span data-ttu-id="7124c-104">Добавление уровня бизнес-логики в проект, использующий привязки модели и веб-форм</span><span class="sxs-lookup"><span data-stu-id="7124c-104">Adding business logic layer to a project that uses model binding and web forms</span></span>
====================
<span data-ttu-id="7124c-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="7124c-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="7124c-106">В этой серии руководств показано основными аспектами с помощью привязки модели с проектом веб-форм ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7124c-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="7124c-107">Привязка модели упрощает взаимодействие с данными более эффективную чем работа с данными объектов источника (например, элемент управления ObjectDataSource и SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="7124c-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="7124c-108">Эта серия начинается с вводный материал и перемещает до более продвинутых в последующих руководствах.</span><span class="sxs-lookup"><span data-stu-id="7124c-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="7124c-109">Этом руководстве показано, как использовать привязки модели с слой бизнес-логики.</span><span class="sxs-lookup"><span data-stu-id="7124c-109">This tutorial shows how to use model binding with a business logic layer.</span></span> <span data-ttu-id="7124c-110">Необходимо задать элемент OnCallingDataMethods, чтобы указать, что объект, отличный от текущей страницы используется для вызова методов данных.</span><span class="sxs-lookup"><span data-stu-id="7124c-110">You will set the OnCallingDataMethods member to specify that an object other than the current page is used to call the data methods.</span></span>
> 
> <span data-ttu-id="7124c-111">Этот учебник основан на проект, созданный в [более ранних](retrieving-data.md) части серии.</span><span class="sxs-lookup"><span data-stu-id="7124c-111">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="7124c-112">Вы можете [загрузить](https://go.microsoft.com/fwlink/?LinkId=286116) весь проект полностью на C# или Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="7124c-112">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="7124c-113">Загружаемый код работает с Visual Studio 2012 или Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="7124c-113">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="7124c-114">Он использует шаблон Visual Studio 2012, который немного отличается от шаблона Visual Studio 2013, в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="7124c-114">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="7124c-115">Вы создадите</span><span class="sxs-lookup"><span data-stu-id="7124c-115">What you'll build</span></span>

<span data-ttu-id="7124c-116">Привязка модели позволяет размещать код взаимодействия данных, либо в файле кода для веб-страницы или в класс отдельные бизнес-логики.</span><span class="sxs-lookup"><span data-stu-id="7124c-116">Model binding enables you to put your data interaction code in either the code-behind file for a web page or in a separate business logic class.</span></span> <span data-ttu-id="7124c-117">Предыдущих руководствах показано, как использовать файлы с выделенным кодом для взаимодействия кода данных.</span><span class="sxs-lookup"><span data-stu-id="7124c-117">The previous tutorials have shown how to use the code-behind files for data interaction code.</span></span> <span data-ttu-id="7124c-118">Этот метод подходит для небольших узлов, но это может привести к кода повторения, а также более сложные при поддержке большой узел.</span><span class="sxs-lookup"><span data-stu-id="7124c-118">This approach works for small sites but it can lead to code repetition and greater difficulty when maintaining a large site.</span></span> <span data-ttu-id="7124c-119">Он также может быть очень сложно программным образом проверить код, который находится в коде программной части файлов, так как ни один уровень абстракции.</span><span class="sxs-lookup"><span data-stu-id="7124c-119">It can also be very difficult to programmatically test code that resides in code behind files because there is no abstraction layer.</span></span>

<span data-ttu-id="7124c-120">Для централизации кода взаимодействия данных, можно создать слой бизнес-логики, который содержит всю логику для взаимодействия с данными.</span><span class="sxs-lookup"><span data-stu-id="7124c-120">To centralize the data interaction code, you can create a business logic layer that contains all of the logic for interacting with data.</span></span> <span data-ttu-id="7124c-121">Затем вызовите уровня бизнес-логики от веб-страниц.</span><span class="sxs-lookup"><span data-stu-id="7124c-121">You then call the business logic layer from your web pages.</span></span> <span data-ttu-id="7124c-122">Этом руководстве показано, как переместить весь код, написанный в предыдущих руководствах в слой бизнес-логики, а затем использовать этот код на страницах.</span><span class="sxs-lookup"><span data-stu-id="7124c-122">This tutorial shows how to move all of the code you have written in the previous tutorials into a business logic layer, and then use that code from the pages.</span></span>

<span data-ttu-id="7124c-123">В этом руководстве вам потребуется:</span><span class="sxs-lookup"><span data-stu-id="7124c-123">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="7124c-124">Переместить код из файлов кода в слой бизнес-логики</span><span class="sxs-lookup"><span data-stu-id="7124c-124">Move the code from code-behind files to a business logic layer</span></span>
2. <span data-ttu-id="7124c-125">Изменение элементов управления с привязкой к данным для вызова методов на уровне бизнес-логики</span><span class="sxs-lookup"><span data-stu-id="7124c-125">Change your data bound controls to call the methods in the business logic layer</span></span>

## <a name="create-business-logic-layer"></a><span data-ttu-id="7124c-126">Создание уровня бизнес-логики</span><span class="sxs-lookup"><span data-stu-id="7124c-126">Create business logic layer</span></span>

<span data-ttu-id="7124c-127">Теперь вы создадите класс, который вызывается из веб-страниц.</span><span class="sxs-lookup"><span data-stu-id="7124c-127">Now, you will create the class that is called from the web pages.</span></span> <span data-ttu-id="7124c-128">Методы в этом классе выглядеть методы, которые вы использовали в предыдущих учебных курсах и включают в себя значение поставщика атрибуты.</span><span class="sxs-lookup"><span data-stu-id="7124c-128">The methods in this class look similar to the methods you used in the previous tutorials and include the value provider attributes.</span></span>

<span data-ttu-id="7124c-129">Во-первых, добавьте новую папку с именем **BLL**.</span><span class="sxs-lookup"><span data-stu-id="7124c-129">First, add a new folder called **BLL**.</span></span>

![Добавить папку](adding-business-logic-layer/_static/image1.png)

<span data-ttu-id="7124c-131">В папку BLL, создайте новый класс с именем **SchoolBL.cs**.</span><span class="sxs-lookup"><span data-stu-id="7124c-131">In the BLL folder, create a new class named **SchoolBL.cs**.</span></span> <span data-ttu-id="7124c-132">Он будет содержать все операции с данными, которые первоначально находились в файлы с выделенным кодом.</span><span class="sxs-lookup"><span data-stu-id="7124c-132">It will contain all of the data operations that originally resided in code-behind files.</span></span> <span data-ttu-id="7124c-133">Методы являются почти одинаковыми, как методы в файле кода, но будет включать некоторые изменения.</span><span class="sxs-lookup"><span data-stu-id="7124c-133">The methods are almost the same as the methods in the code-behind file, but will include some changes.</span></span>

<span data-ttu-id="7124c-134">Самое важное изменение, следует отметить, — это больше не выполняются в экземпляре код из **страницы** класса.</span><span class="sxs-lookup"><span data-stu-id="7124c-134">The most important change to note is that you are no longer executing the code from within an instance of **Page** class.</span></span> <span data-ttu-id="7124c-135">Класс Page содержит **TryUpdateModel** метод и **ModelState** свойство.</span><span class="sxs-lookup"><span data-stu-id="7124c-135">The Page class contains the **TryUpdateModel** method and the **ModelState** property.</span></span> <span data-ttu-id="7124c-136">Этот код при перемещении в слой бизнес-логики, вы больше не иметь экземпляр класса страницы для вызова этих членов.</span><span class="sxs-lookup"><span data-stu-id="7124c-136">When this code is moved to a business logic layer, you no longer have an instance of the Page class to call these members.</span></span> <span data-ttu-id="7124c-137">Чтобы обойти эту проблему, необходимо добавить **ModelMethodContext** параметра к любому методу, который обращается к TryUpdateModel или ModelState.</span><span class="sxs-lookup"><span data-stu-id="7124c-137">To get around this issue, you must add a **ModelMethodContext** parameter to any method that accesses TryUpdateModel or ModelState.</span></span> <span data-ttu-id="7124c-138">Используйте этот параметр ModelMethodContext для вызова TryUpdateModel или получить ModelState.</span><span class="sxs-lookup"><span data-stu-id="7124c-138">You use this ModelMethodContext parameter to call TryUpdateModel or retrieve ModelState.</span></span> <span data-ttu-id="7124c-139">Не требуется вносить изменения в веб-страницы, чтобы учесть этот новый параметр.</span><span class="sxs-lookup"><span data-stu-id="7124c-139">You do not need to change anything in the web page to account for this new parameter.</span></span>

<span data-ttu-id="7124c-140">Замените код в SchoolBL.cs следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="7124c-140">Replace the code in SchoolBL.cs with the following code.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a><span data-ttu-id="7124c-141">Изменить существующие страницы для получения данных из уровня бизнес-логики</span><span class="sxs-lookup"><span data-stu-id="7124c-141">Revise existing pages to retrieve data from business logic layer</span></span>

<span data-ttu-id="7124c-142">Наконец страницы Students.aspx AddStudent.aspx и Courses.aspx преобразование из с помощью запросов в файл с выделенным кодом с помощью уровня бизнес-логики.</span><span class="sxs-lookup"><span data-stu-id="7124c-142">Finally, you will convert the pages Students.aspx, AddStudent.aspx, and Courses.aspx from using queries in the code-behind file to using the business logic layer.</span></span>

<span data-ttu-id="7124c-143">В файлы с выделенным кодом для учащихся, AddStudent и курсы удалите или закомментируйте следующие методы запроса:</span><span class="sxs-lookup"><span data-stu-id="7124c-143">In the code-behind files for Students, AddStudent, and Courses, delete or comment out the following query methods:</span></span>

- <span data-ttu-id="7124c-144">studentsGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="7124c-144">studentsGrid\_GetData</span></span>
- <span data-ttu-id="7124c-145">studentsGrid\_UpdateItem</span><span class="sxs-lookup"><span data-stu-id="7124c-145">studentsGrid\_UpdateItem</span></span>
- <span data-ttu-id="7124c-146">studentsGrid\_DeleteItem</span><span class="sxs-lookup"><span data-stu-id="7124c-146">studentsGrid\_DeleteItem</span></span>
- <span data-ttu-id="7124c-147">addStudentForm\_InsertItem</span><span class="sxs-lookup"><span data-stu-id="7124c-147">addStudentForm\_InsertItem</span></span>
- <span data-ttu-id="7124c-148">coursesGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="7124c-148">coursesGrid\_GetData</span></span>

<span data-ttu-id="7124c-149">Теперь требуется без кода в файл с выделенным кодом, относящиеся к операции с данными.</span><span class="sxs-lookup"><span data-stu-id="7124c-149">You now should have no code in the code-behind file that pertains to data operations.</span></span>

<span data-ttu-id="7124c-150">**OnCallingDataMethods** обработчик событий позволяет указать объект, используемый для методов данных.</span><span class="sxs-lookup"><span data-stu-id="7124c-150">The **OnCallingDataMethods** event handler enables you to specify an object to use for the data methods.</span></span> <span data-ttu-id="7124c-151">В Students.aspx добавьте значение для этого обработчика событий и измените имена методов данных на имена методов в класс бизнес-логики.</span><span class="sxs-lookup"><span data-stu-id="7124c-151">In Students.aspx, add a value for that event handler and change the names of the data methods to the names of the methods in the business logic class.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

<span data-ttu-id="7124c-152">В файле с выделенным кодом для Students.aspx Определите обработчик событий для события CallingDataMethods.</span><span class="sxs-lookup"><span data-stu-id="7124c-152">In the code-behind file for Students.aspx, define the event handler for the CallingDataMethods event.</span></span> <span data-ttu-id="7124c-153">В этом обработчике событий укажите класс бизнес-логики для данных операций.</span><span class="sxs-lookup"><span data-stu-id="7124c-153">In this event handler, you specify the business logic class for data operations.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

<span data-ttu-id="7124c-154">В AddStudent.aspx внести аналогичные изменения.</span><span class="sxs-lookup"><span data-stu-id="7124c-154">In AddStudent.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

<span data-ttu-id="7124c-155">В Courses.aspx внести аналогичные изменения.</span><span class="sxs-lookup"><span data-stu-id="7124c-155">In Courses.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

<span data-ttu-id="7124c-156">Запустите приложение и обратите внимание на то, что все страницы работать так, как у него есть ранее.</span><span class="sxs-lookup"><span data-stu-id="7124c-156">Run the application and notice that all of the pages function as they had previously.</span></span> <span data-ttu-id="7124c-157">Логика проверки также работает правильно.</span><span class="sxs-lookup"><span data-stu-id="7124c-157">The validation logic also works correctly.</span></span>

## <a name="conclusion"></a><span data-ttu-id="7124c-158">Заключение</span><span class="sxs-lookup"><span data-stu-id="7124c-158">Conclusion</span></span>

<span data-ttu-id="7124c-159">В этом руководстве повторно структуру приложения для использования уровня доступа к данным и уровня бизнес-логики.</span><span class="sxs-lookup"><span data-stu-id="7124c-159">In this tutorial, you re-structured your application to use a data access layer and business logic layer.</span></span> <span data-ttu-id="7124c-160">Вы указали, что элементы управления данными использовать объект, который не является текущей страницы для операции с данными.</span><span class="sxs-lookup"><span data-stu-id="7124c-160">You specified that the data controls use an object that is not the current page for data operations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7124c-161">Назад</span><span class="sxs-lookup"><span data-stu-id="7124c-161">Previous</span></span>](using-query-string-values-to-retrieve-data.md)
