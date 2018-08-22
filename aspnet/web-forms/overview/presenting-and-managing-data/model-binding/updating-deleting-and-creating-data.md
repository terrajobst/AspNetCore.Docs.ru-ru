---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: Обновление, удаление и создание данных с помощью привязки модели и веб-форм | Документация Майкрософт
author: tfitzmac
description: В этой серии руководств показано основными аспектами с помощью привязки модели с проектом веб-форм ASP.NET. Привязка модели позволяет взаимодействие с данными более прямой-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: a3e098d7b10ba6218ffa1818ccf8fc8df6912a9e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827986"
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a><span data-ttu-id="3f3f5-104">Обновление, удаление и создание данных с помощью привязки модели и веб-форм</span><span class="sxs-lookup"><span data-stu-id="3f3f5-104">Updating, deleting, and creating data with model binding and web forms</span></span>
====================
<span data-ttu-id="3f3f5-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3f3f5-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="3f3f5-106">В этой серии руководств показано основными аспектами с помощью привязки модели с проектом веб-форм ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="3f3f5-107">Привязка модели упрощает взаимодействие с данными более эффективную чем работа с данными объектов источника (например, элемент управления ObjectDataSource и SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="3f3f5-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="3f3f5-108">Эта серия начинается с вводный материал и перемещает до более продвинутых в последующих руководствах.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="3f3f5-109">Этом руководстве показано, как создание, обновление и удаление данных с помощью привязки модели.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-109">This tutorial shows how to create, update, and delete data with model binding.</span></span> <span data-ttu-id="3f3f5-110">Будут заданы следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="3f3f5-110">You will set the following properties:</span></span>
> 
> - <span data-ttu-id="3f3f5-111">DeleteMethod</span><span class="sxs-lookup"><span data-stu-id="3f3f5-111">DeleteMethod</span></span>
> - <span data-ttu-id="3f3f5-112">InsertMethod</span><span class="sxs-lookup"><span data-stu-id="3f3f5-112">InsertMethod</span></span>
> - <span data-ttu-id="3f3f5-113">UpdateMethod</span><span class="sxs-lookup"><span data-stu-id="3f3f5-113">UpdateMethod</span></span>
> 
> <span data-ttu-id="3f3f5-114">Эти свойства получают имя метода, который обрабатывает соответствующей операции.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-114">These properties receive the name of the method that handles the corresponding operation.</span></span> <span data-ttu-id="3f3f5-115">В этом методе обеспечивают логику для взаимодействия с данными.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-115">Within that method, you provide the logic for interacting with the data.</span></span>
> 
> <span data-ttu-id="3f3f5-116">Этот учебник основан на проект, созданный в первом [часть](retrieving-data.md) ряда.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-116">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="3f3f5-117">Вы можете [загрузить](https://go.microsoft.com/fwlink/?LinkId=286116) весь проект полностью на C# или Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-117">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="3f3f5-118">Загружаемый код работает с Visual Studio 2012 или Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-118">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="3f3f5-119">Он использует шаблон Visual Studio 2012, который немного отличается от шаблона Visual Studio 2013, в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-119">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="3f3f5-120">Вы создадите</span><span class="sxs-lookup"><span data-stu-id="3f3f5-120">What you'll build</span></span>

<span data-ttu-id="3f3f5-121">В этом руководстве вам потребуется:</span><span class="sxs-lookup"><span data-stu-id="3f3f5-121">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="3f3f5-122">Добавление шаблонов динамических данных</span><span class="sxs-lookup"><span data-stu-id="3f3f5-122">Add dynamic data templates</span></span>
2. <span data-ttu-id="3f3f5-123">Включить обновление и удаление данных через методы привязки модели</span><span class="sxs-lookup"><span data-stu-id="3f3f5-123">Enable updating and deleting data through model binding methods</span></span>
3. <span data-ttu-id="3f3f5-124">Применение правил проверки данных — Включение создания новой записи в базе данных</span><span class="sxs-lookup"><span data-stu-id="3f3f5-124">Apply data validation rules - Enable creating a new record in the database</span></span>

## <a name="add-dynamic-data-templates"></a><span data-ttu-id="3f3f5-125">Добавление шаблонов динамических данных</span><span class="sxs-lookup"><span data-stu-id="3f3f5-125">Add dynamic data templates</span></span>

<span data-ttu-id="3f3f5-126">Для обеспечения удобства работы пользователей и сократить повторы в коде, будет использовать шаблоны динамических данных.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-126">To provide the best user experience and minimize code repetition, you will use dynamic data templates.</span></span> <span data-ttu-id="3f3f5-127">Вы легко можете интегрировать шаблоны готовых динамических данных в существующий узел, установив пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-127">You can easily integrate pre-built dynamic data templates into your existing site by installing a NuGet package.</span></span>

<span data-ttu-id="3f3f5-128">Из **управление пакетами NuGet**, установить **DynamicDataTemplatesCS**.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-128">From the **Manage NuGet Packages**, install the **DynamicDataTemplatesCS**.</span></span>

![шаблоны динамических данных](updating-deleting-and-creating-data/_static/image1.png)

<span data-ttu-id="3f3f5-130">Обратите внимание на то, что проект сейчас содержит папку с именем **DynamicData**.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-130">Notice that your project now includes a folder named **DynamicData**.</span></span> <span data-ttu-id="3f3f5-131">В этой папке вы найдете шаблоны, которые автоматически применяются к динамическим элементам управления в web forms.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-131">In that folder, you will find the templates that are automatically applied to dynamic controls in your web forms.</span></span>

![Папка динамических данных](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a><span data-ttu-id="3f3f5-133">Включение обновления и удаления</span><span class="sxs-lookup"><span data-stu-id="3f3f5-133">Enable updating and deleting</span></span>

<span data-ttu-id="3f3f5-134">Предоставление пользователям возможности обновления и удаления записей в базе данных очень похожа на процедуру для извлечения данных.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-134">Enabling users to update and delete records in the database is very similar to the process for retrieving data.</span></span> <span data-ttu-id="3f3f5-135">В **UpdateMethod** и **DeleteMethod** свойства, укажите имена методов, которые выполняют эти операции.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-135">In the **UpdateMethod** and **DeleteMethod** properties, you specify the names of the methods that perform those operations.</span></span> <span data-ttu-id="3f3f5-136">С элементом управления GridView можно также указать автоматическое создание перегруженных вариантов редактирования и удалить кнопки.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-136">With a GridView control, you can also specify the automatic generation of edit and delete buttons.</span></span> <span data-ttu-id="3f3f5-137">Следующий выделенный код показывает дополнения кода GridView.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-137">The following highlighted code shows the additions to the GridView code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

<span data-ttu-id="3f3f5-138">В файле кода, добавьте с помощью инструкции для **System.Data.Entity.Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-138">In the code-behind file, add a using statement for **System.Data.Entity.Infrastructure**.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

<span data-ttu-id="3f3f5-139">Затем добавьте следующее обновление и удаление методов.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-139">Then, add the following update and delete methods.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

<span data-ttu-id="3f3f5-140">**TryUpdateModel** метод применим совпадающих значений привязкой к данным из веб-формы к элементу данных.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-140">The **TryUpdateModel** method applies the matching data-bound values from the web form to the data item.</span></span> <span data-ttu-id="3f3f5-141">Элемент данных извлекаются на основе значения параметра id.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-141">The data item is retrieved based on the value of the id parameter.</span></span>

## <a name="enforce-validation-requirements"></a><span data-ttu-id="3f3f5-142">Принудительно применить требования к проверке</span><span class="sxs-lookup"><span data-stu-id="3f3f5-142">Enforce validation requirements</span></span>

<span data-ttu-id="3f3f5-143">Атрибуты проверки, которые были применены к свойствам класса Student FirstName, LastName и года будут применяться автоматически при обновлении данных.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-143">The validation attributes that you applied to the FirstName, LastName, and Year properties in the Student class are automatically enforced when updating the data.</span></span> <span data-ttu-id="3f3f5-144">Элементы управления DynamicField добавить клиентские и серверные проверяющих элементов управления на основе атрибутов проверки.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-144">The DynamicField controls add client and server validators based on the validation attributes.</span></span> <span data-ttu-id="3f3f5-145">Свойства FirstName и LastName, как требуется.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-145">The FirstName and LastName properties are both required.</span></span> <span data-ttu-id="3f3f5-146">Имя не может превышать 20 символов и LastName не может превышать 40 символов.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-146">FirstName cannot exceed 20 characters in length, and LastName cannot exceed 40 characters.</span></span> <span data-ttu-id="3f3f5-147">Год должен быть является допустимым значением для перечисления AcademicYear.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-147">Year must be a valid value for the AcademicYear enumeration.</span></span>

<span data-ttu-id="3f3f5-148">Если он нарушает одно из требований проверки, обновление не продолжит работу.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-148">If the user violates one of the validation requirements, the update does not proceed.</span></span> <span data-ttu-id="3f3f5-149">Чтобы увидеть сообщение об ошибке, добавьте элемент управления ValidationSummary над элементом управления GridView.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-149">To see the error message, add a ValidationSummary control above the GridView.</span></span> <span data-ttu-id="3f3f5-150">Для отображения ошибок проверки из привязки модели, задайте **ShowModelStateErrors** свойство значение **true**.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-150">To display the validation errors from model binding, set the **ShowModelStateErrors** property set to **true**.</span></span> 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

<span data-ttu-id="3f3f5-151">Запустите веб-приложение и обновление и удаление всех записей.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-151">Run the web application, and update and delete any of the records.</span></span>

![обновление данных](updating-deleting-and-creating-data/_static/image3.png)

<span data-ttu-id="3f3f5-153">Обратите внимание, что когда в режиме правки значение для свойства год автоматически отображается в виде раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-153">Notice that when in the edit mode the value for the Year property is automatically rendered as a drop down list.</span></span> <span data-ttu-id="3f3f5-154">Свойство Year является значением перечисления, а шаблон динамических данных для значения перечисления указывает раскрывающийся список для редактирования.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-154">The Year property is an enumeration value, and the dynamic data template for an enumeration value specifies a drop down list for editing.</span></span> <span data-ttu-id="3f3f5-155">Этот шаблон можно найти, открыв **перечисления\_Edit.ascx платформы** файл **DynamicData**/**FieldTemplates** папки.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-155">You can find that template by opening the **Enumeration\_Edit.ascx** file in the **DynamicData**/**FieldTemplates** folder.</span></span>

<span data-ttu-id="3f3f5-156">Если указать допустимые значения, обновление завершается успешно.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-156">If you provide valid values, the update completes successfully.</span></span> <span data-ttu-id="3f3f5-157">Если Вы нарушаете одно из требований проверки, обновление не запускается, и сообщение об ошибке отображается над сеткой.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-157">If you violate one of the validation requirements, the update does not proceed and an error message is displayed above the grid.</span></span>

![Сообщение об ошибке](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a><span data-ttu-id="3f3f5-159">Добавление новых записей</span><span class="sxs-lookup"><span data-stu-id="3f3f5-159">Add new records</span></span>

<span data-ttu-id="3f3f5-160">Элемент управления GridView не включает **InsertMethod** свойство и поэтому не может использоваться для добавления новой записи с помощью привязки модели.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-160">The GridView control does not include the **InsertMethod** property and therefore cannot be used for adding a new record with model binding.</span></span> <span data-ttu-id="3f3f5-161">Можно найти свойство InsertMethod в **FormView**, **DetailsView**, или **ListView** элементов управления.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-161">You can find the InsertMethod property in the **FormView**, **DetailsView**, or **ListView** controls.</span></span> <span data-ttu-id="3f3f5-162">В этом руководстве используется элемент управления FormView для добавления новой записи.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-162">In this tutorial, you will use a FormView control to add a new record.</span></span>

<span data-ttu-id="3f3f5-163">Во-первых необходимо добавьте ссылку на новую страницу, которая создается для добавления новой записи.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-163">First, add a link to the new page you will create for adding a new record.</span></span> <span data-ttu-id="3f3f5-164">Добавьте над ValidationSummary:</span><span class="sxs-lookup"><span data-stu-id="3f3f5-164">Above the ValidationSummary, add:</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

<span data-ttu-id="3f3f5-165">Новая ссылка появится в верхней части содержимого на страницу учащихся.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-165">The new link will appear at the top of the content for the Students page.</span></span>

![Новая ссылка](updating-deleting-and-creating-data/_static/image5.png)

<span data-ttu-id="3f3f5-167">Затем добавьте новую веб-форму, с помощью главной страницы и назовите его **AddStudent**.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-167">Then, add a new web form using a master page, and name it **AddStudent**.</span></span> <span data-ttu-id="3f3f5-168">Выберите в качестве главной странице Site.Master.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-168">Select Site.Master as the master page.</span></span>

<span data-ttu-id="3f3f5-169">Отрисовка поля для добавления нового учащегося с помощью **DynamicEntity** элемента управления.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-169">You will render the fields for adding a new student by using a **DynamicEntity** control.</span></span> <span data-ttu-id="3f3f5-170">Элемент управления DynamicEntity визуализирует, редактируемых свойств в классе, указанном в свойстве ItemType.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-170">The DynamicEntity control renders that editable properties in the class specified in the ItemType property.</span></span> <span data-ttu-id="3f3f5-171">Свойство StudentID был помечен атрибутом **[ScaffoldColumn(false)]** атрибут, поэтому он не отображается.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-171">The StudentID property was marked with the **[ScaffoldColumn(false)]** attribute so it is not rendered.</span></span> <span data-ttu-id="3f3f5-172">В заполнителе MainContent AddStudent страницы добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-172">In the MainContent placeholder of the AddStudent page, add the following code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

<span data-ttu-id="3f3f5-173">Добавьте в файл кода (AddStudent.aspx.cs), **с помощью** инструкции для **ContosoUniversityModelBinding.Models** пространства имен.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-173">In the code-behind file (AddStudent.aspx.cs), add a **using** statement for the **ContosoUniversityModelBinding.Models** namespace.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

<span data-ttu-id="3f3f5-174">Затем добавьте следующие методы для вставки новой записи и обработчик событий для кнопки "Отмена".</span><span class="sxs-lookup"><span data-stu-id="3f3f5-174">Then, add the following methods to specify how to insert a new record and an event handler for the cancel button.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

<span data-ttu-id="3f3f5-175">Сохраните все изменения.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-175">Save all of the changes.</span></span>

<span data-ttu-id="3f3f5-176">Запустите веб-приложения и создания нового учащегося.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-176">Run the web application and create a new student.</span></span>

![Добавление нового учащегося](updating-deleting-and-creating-data/_static/image6.png)

<span data-ttu-id="3f3f5-178">Нажмите кнопку **вставить** и обратите внимание на то, будет создана нового учащегося.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-178">Click **Insert** and notice the new student has been created.</span></span>

![Отображение нового учащегося](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a><span data-ttu-id="3f3f5-180">Заключение</span><span class="sxs-lookup"><span data-stu-id="3f3f5-180">Conclusion</span></span>

<span data-ttu-id="3f3f5-181">В этом руководстве вы включили, обновление, удаление и создание данных.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-181">In this tutorial, you enabled updating, deleting, and creating data.</span></span> <span data-ttu-id="3f3f5-182">Вы уверены в том, что правила проверки применяются при взаимодействии с данными.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-182">You ensured validation rules are applied when interacting with the data.</span></span>

<span data-ttu-id="3f3f5-183">В следующем [руководстве](sorting-paging-and-filtering-data.md) в этой серии, вы включите сортировка, разбиение по страницам и фильтрация данных.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-183">In the next [tutorial](sorting-paging-and-filtering-data.md) in this series, you will enable sorting, paging, and filtering data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3f3f5-184">[Назад](retrieving-data.md)
> [Вперед](sorting-paging-and-filtering-data.md)</span><span class="sxs-lookup"><span data-stu-id="3f3f5-184">[Previous](retrieving-data.md)
[Next](sorting-paging-and-filtering-data.md)</span></span>
