---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Добавление бизнес-логики для проекта, использующего привязки модели и веб-форм | Документы Microsoft
author: tfitzmac
description: Этот учебник ряд демонстрирует основные аспекты использования привязки модели в проекте веб-форм ASP.NET. Привязка модели позволяет взаимодействия с данными дополнительные прямые-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: 25e887bdc316abf65c780bb6c8d075e938e85064
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a><span data-ttu-id="7b830-104">Добавление бизнес-логики для проекта, использующего привязки модели и веб-форм</span><span class="sxs-lookup"><span data-stu-id="7b830-104">Adding business logic layer to a project that uses model binding and web forms</span></span>
====================
<span data-ttu-id="7b830-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="7b830-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="7b830-106">Этот учебник ряд демонстрирует основные аспекты использования привязки модели в проекте веб-форм ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7b830-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="7b830-107">Привязка модели позволяет взаимодействия с данными более прост, чем работе с данными источника объектов (например, элемент управления ObjectDataSource или SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="7b830-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="7b830-108">Эта серия начинается с вводный материал и перемещает на следующих занятиях рассматривается более сложных понятиях.</span><span class="sxs-lookup"><span data-stu-id="7b830-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="7b830-109">Этот учебник демонстрируется использование привязки модели с слой бизнес-логики.</span><span class="sxs-lookup"><span data-stu-id="7b830-109">This tutorial shows how to use model binding with a business logic layer.</span></span> <span data-ttu-id="7b830-110">Необходимо задать элемент OnCallingDataMethods, чтобы указать, что объект, отличный от текущей странице используется для вызова методов работы с данными.</span><span class="sxs-lookup"><span data-stu-id="7b830-110">You will set the OnCallingDataMethods member to specify that an object other than the current page is used to call the data methods.</span></span>
> 
> <span data-ttu-id="7b830-111">Этот учебник построен на проект, созданный в [более ранних](retrieving-data.md) части ряда.</span><span class="sxs-lookup"><span data-stu-id="7b830-111">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="7b830-112">Вы можете [загрузки](https://go.microsoft.com/fwlink/?LinkId=286116) весь проект C# или Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="7b830-112">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="7b830-113">Загружаемый код работает с Visual Studio 2012 или Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="7b830-113">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="7b830-114">Она использует шаблон Visual Studio 2012, который немного отличается от шаблона Visual Studio 2013, показанными в данном руководстве.</span><span class="sxs-lookup"><span data-stu-id="7b830-114">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="7b830-115">Мы создадим</span><span class="sxs-lookup"><span data-stu-id="7b830-115">What you'll build</span></span>

<span data-ttu-id="7b830-116">Привязка модели позволяет помещать код взаимодействия данных в файл кода для веб-страницы или в класс отдельные бизнес-логики.</span><span class="sxs-lookup"><span data-stu-id="7b830-116">Model binding enables you to put your data interaction code in either the code-behind file for a web page or in a separate business logic class.</span></span> <span data-ttu-id="7b830-117">Предыдущих учебниках показано, как использовать файлы кода для кода взаимодействия данных.</span><span class="sxs-lookup"><span data-stu-id="7b830-117">The previous tutorials have shown how to use the code-behind files for data interaction code.</span></span> <span data-ttu-id="7b830-118">Этот способ подходит для небольших сайтов, но он может привести к кода повторов и затруднение при поддержке больших сайта.</span><span class="sxs-lookup"><span data-stu-id="7b830-118">This approach works for small sites but it can lead to code repetition and greater difficulty when maintaining a large site.</span></span> <span data-ttu-id="7b830-119">Также может быть очень сложно программным путем тестирования кода, который находится в коде программной части файлов, так как нет уровень абстракции.</span><span class="sxs-lookup"><span data-stu-id="7b830-119">It can also be very difficult to programmatically test code that resides in code behind files because there is no abstraction layer.</span></span>

<span data-ttu-id="7b830-120">Для централизации кода взаимодействия данных, можно создать слой бизнес-логики, который содержит всю необходимую логику для взаимодействия с данными.</span><span class="sxs-lookup"><span data-stu-id="7b830-120">To centralize the data interaction code, you can create a business logic layer that contains all of the logic for interacting with data.</span></span> <span data-ttu-id="7b830-121">Затем вызовите уровня бизнес-логики от веб-страниц.</span><span class="sxs-lookup"><span data-stu-id="7b830-121">You then call the business logic layer from your web pages.</span></span> <span data-ttu-id="7b830-122">Этого учебника показано, как переместить весь код, написанный на предыдущих уроках в слой бизнес-логики, а затем использовать этот код из страниц.</span><span class="sxs-lookup"><span data-stu-id="7b830-122">This tutorial shows how to move all of the code you have written in the previous tutorials into a business logic layer, and then use that code from the pages.</span></span>

<span data-ttu-id="7b830-123">В этом учебнике вы сможете:</span><span class="sxs-lookup"><span data-stu-id="7b830-123">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="7b830-124">Переместить код из файлов кода в слой бизнес-логики</span><span class="sxs-lookup"><span data-stu-id="7b830-124">Move the code from code-behind files to a business logic layer</span></span>
2. <span data-ttu-id="7b830-125">Изменение элементов управления с привязкой к данным для вызова методов в бизнес-логики</span><span class="sxs-lookup"><span data-stu-id="7b830-125">Change your data bound controls to call the methods in the business logic layer</span></span>

## <a name="create-business-logic-layer"></a><span data-ttu-id="7b830-126">Создание уровня бизнес-логики</span><span class="sxs-lookup"><span data-stu-id="7b830-126">Create business logic layer</span></span>

<span data-ttu-id="7b830-127">Теперь необходимо создать класс, вызываемой из веб-страниц.</span><span class="sxs-lookup"><span data-stu-id="7b830-127">Now, you will create the class that is called from the web pages.</span></span> <span data-ttu-id="7b830-128">Методы в этом классе выглядеть аналогично методах, использованных в предыдущих учебниках и включать атрибуты поставщика значения.</span><span class="sxs-lookup"><span data-stu-id="7b830-128">The methods in this class look similar to the methods you used in the previous tutorials and include the value provider attributes.</span></span>

<span data-ttu-id="7b830-129">Сначала добавьте новую папку с именем **уровень бизнес-ЛОГИКИ**.</span><span class="sxs-lookup"><span data-stu-id="7b830-129">First, add a new folder called **BLL**.</span></span>

![Добавить папку](adding-business-logic-layer/_static/image1.png)

<span data-ttu-id="7b830-131">В папке уровень бизнес-ЛОГИКИ, создайте новый класс с именем **SchoolBL.cs**.</span><span class="sxs-lookup"><span data-stu-id="7b830-131">In the BLL folder, create a new class named **SchoolBL.cs**.</span></span> <span data-ttu-id="7b830-132">Он содержит список всех операций с данными, которые первоначально находились в файлах кода.</span><span class="sxs-lookup"><span data-stu-id="7b830-132">It will contain all of the data operations that originally resided in code-behind files.</span></span> <span data-ttu-id="7b830-133">Методы схожих как методы в файле кода, но будет содержать некоторые изменения.</span><span class="sxs-lookup"><span data-stu-id="7b830-133">The methods are almost the same as the methods in the code-behind file, but will include some changes.</span></span>

<span data-ttu-id="7b830-134">Наиболее важное изменение, следует отметить, — больше не выполняются в экземпляре код из **страницы** класса.</span><span class="sxs-lookup"><span data-stu-id="7b830-134">The most important change to note is that you are no longer executing the code from within an instance of **Page** class.</span></span> <span data-ttu-id="7b830-135">Содержит класс страницы **TryUpdateModel** метод и **ModelState** свойство.</span><span class="sxs-lookup"><span data-stu-id="7b830-135">The Page class contains the **TryUpdateModel** method and the **ModelState** property.</span></span> <span data-ttu-id="7b830-136">Этот код был перемещен слой бизнес-логики, больше не нужна экземпляр класса страницы для вызова этих членов.</span><span class="sxs-lookup"><span data-stu-id="7b830-136">When this code is moved to a business logic layer, you no longer have an instance of the Page class to call these members.</span></span> <span data-ttu-id="7b830-137">Чтобы обойти эту проблему, необходимо добавить **ModelMethodContext** параметр с любым методом, который обращается к TryUpdateModel или ModelState.</span><span class="sxs-lookup"><span data-stu-id="7b830-137">To get around this issue, you must add a **ModelMethodContext** parameter to any method that accesses TryUpdateModel or ModelState.</span></span> <span data-ttu-id="7b830-138">Этот параметр ModelMethodContext используется для вызова TryUpdateModel или получить ModelState.</span><span class="sxs-lookup"><span data-stu-id="7b830-138">You use this ModelMethodContext parameter to call TryUpdateModel or retrieve ModelState.</span></span> <span data-ttu-id="7b830-139">Необходимо вносит изменения в веб-страницы с учетом этого нового параметра.</span><span class="sxs-lookup"><span data-stu-id="7b830-139">You do not need to change anything in the web page to account for this new parameter.</span></span>

<span data-ttu-id="7b830-140">Замените код в SchoolBL.cs следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="7b830-140">Replace the code in SchoolBL.cs with the following code.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a><span data-ttu-id="7b830-141">Изменения существующих страниц для извлечения данных из уровня бизнес-логики</span><span class="sxs-lookup"><span data-stu-id="7b830-141">Revise existing pages to retrieve data from business logic layer</span></span>

<span data-ttu-id="7b830-142">Наконец преобразование страниц Students.aspx, AddStudent.aspx и Courses.aspx с помощью запросов в файл кода с помощью бизнес-логики.</span><span class="sxs-lookup"><span data-stu-id="7b830-142">Finally, you will convert the pages Students.aspx, AddStudent.aspx, and Courses.aspx from using queries in the code-behind file to using the business logic layer.</span></span>

<span data-ttu-id="7b830-143">В файлах кода для учащихся, AddStudent и курсы удалите или закомментируйте следующие методы запроса:</span><span class="sxs-lookup"><span data-stu-id="7b830-143">In the code-behind files for Students, AddStudent, and Courses, delete or comment out the following query methods:</span></span>

- <span data-ttu-id="7b830-144">studentsGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="7b830-144">studentsGrid\_GetData</span></span>
- <span data-ttu-id="7b830-145">studentsGrid\_UpdateItem</span><span class="sxs-lookup"><span data-stu-id="7b830-145">studentsGrid\_UpdateItem</span></span>
- <span data-ttu-id="7b830-146">studentsGrid\_DeleteItem</span><span class="sxs-lookup"><span data-stu-id="7b830-146">studentsGrid\_DeleteItem</span></span>
- <span data-ttu-id="7b830-147">addStudentForm\_InsertItem</span><span class="sxs-lookup"><span data-stu-id="7b830-147">addStudentForm\_InsertItem</span></span>
- <span data-ttu-id="7b830-148">coursesGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="7b830-148">coursesGrid\_GetData</span></span>

<span data-ttu-id="7b830-149">Теперь следует не имеется кода в файле кода, относящиеся к операции с данными.</span><span class="sxs-lookup"><span data-stu-id="7b830-149">You now should have no code in the code-behind file that pertains to data operations.</span></span>

<span data-ttu-id="7b830-150">**OnCallingDataMethods** обработчик событий можно указать объект, используемый для методов работы с данными.</span><span class="sxs-lookup"><span data-stu-id="7b830-150">The **OnCallingDataMethods** event handler enables you to specify an object to use for the data methods.</span></span> <span data-ttu-id="7b830-151">В Students.aspx добавьте значение для этого обработчика событий и измените имена методов работы с данными на имена методов в классе логику.</span><span class="sxs-lookup"><span data-stu-id="7b830-151">In Students.aspx, add a value for that event handler and change the names of the data methods to the names of the methods in the business logic class.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

<span data-ttu-id="7b830-152">В файле кода программной части для Students.aspx Определите обработчик событий для события CallingDataMethods.</span><span class="sxs-lookup"><span data-stu-id="7b830-152">In the code-behind file for Students.aspx, define the event handler for the CallingDataMethods event.</span></span> <span data-ttu-id="7b830-153">В этом обработчике событий задать классе логику для операций с данными.</span><span class="sxs-lookup"><span data-stu-id="7b830-153">In this event handler, you specify the business logic class for data operations.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

<span data-ttu-id="7b830-154">В AddStudent.aspx внести соответствующие изменения.</span><span class="sxs-lookup"><span data-stu-id="7b830-154">In AddStudent.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

<span data-ttu-id="7b830-155">В Courses.aspx внести соответствующие изменения.</span><span class="sxs-lookup"><span data-stu-id="7b830-155">In Courses.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

<span data-ttu-id="7b830-156">Запустите приложение и обратите внимание, что все страницы работать так, как они имели ранее.</span><span class="sxs-lookup"><span data-stu-id="7b830-156">Run the application and notice that all of the pages function as they had previously.</span></span> <span data-ttu-id="7b830-157">Логика проверки также работает правильно.</span><span class="sxs-lookup"><span data-stu-id="7b830-157">The validation logic also works correctly.</span></span>

## <a name="conclusion"></a><span data-ttu-id="7b830-158">Заключение</span><span class="sxs-lookup"><span data-stu-id="7b830-158">Conclusion</span></span>

<span data-ttu-id="7b830-159">В этом учебнике повторно структуру приложения для использования уровня доступа к данным и бизнес-логики.</span><span class="sxs-lookup"><span data-stu-id="7b830-159">In this tutorial, you re-structured your application to use a data access layer and business logic layer.</span></span> <span data-ttu-id="7b830-160">Вы указали, что элементы управления данными использовать объект, который не является текущей страницы для операций с данными.</span><span class="sxs-lookup"><span data-stu-id="7b830-160">You specified that the data controls use an object that is not the current page for data operations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7b830-161">Назад</span><span class="sxs-lookup"><span data-stu-id="7b830-161">Previous</span></span>](using-query-string-values-to-retrieve-data.md)
