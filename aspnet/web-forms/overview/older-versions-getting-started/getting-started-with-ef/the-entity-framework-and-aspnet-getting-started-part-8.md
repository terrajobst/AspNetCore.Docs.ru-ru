---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: Приступая к работе с базой данных Entity Framework 4.0 сначала и ASP.NET 4, веб-формы - часть 8 | Документы Microsoft
author: tdykstra
description: Contoso университета примера веб-приложения показано, как для создания приложений веб-форм ASP.NET, использующий Entity Framework. Это образец приложения...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: 035cce022d1b3697b825a96487529dbc9675d90e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a><span data-ttu-id="87cd7-104">Приступая к работе с базой данных Entity Framework 4.0 сначала и форм ASP.NET 4 веб - часть 8</span><span class="sxs-lookup"><span data-stu-id="87cd7-104">Getting Started with Entity Framework 4.0 Database First and ASP.NET 4 Web Forms - Part 8</span></span>
====================
<span data-ttu-id="87cd7-105">по [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="87cd7-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="87cd7-106">Contoso университета примера веб-приложения показано, как для создания приложений веб-форм ASP.NET, с помощью Entity Framework 4.0 и Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="87cd7-106">The Contoso University sample web application demonstrates how to create ASP.NET Web Forms applications using the Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="87cd7-107">Сведения о учебника серии см [в первом учебнике ряда](the-entity-framework-and-aspnet-getting-started-part-1.md)</span><span class="sxs-lookup"><span data-stu-id="87cd7-107">For information about the tutorial series, see [the first tutorial in the series](the-entity-framework-and-aspnet-getting-started-part-1.md)</span></span>


## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a><span data-ttu-id="87cd7-108">С помощью функций платформы динамических данных для форматирования и проверки данных</span><span class="sxs-lookup"><span data-stu-id="87cd7-108">Using Dynamic Data Functionality to Format and Validate Data</span></span>

<span data-ttu-id="87cd7-109">В предыдущем учебнике реализации хранимых процедур.</span><span class="sxs-lookup"><span data-stu-id="87cd7-109">In the previous tutorial you implemented stored procedures.</span></span> <span data-ttu-id="87cd7-110">Этот учебник будет показано, как функциональные возможности платформы динамических данных может предоставить следующие преимущества:</span><span class="sxs-lookup"><span data-stu-id="87cd7-110">This tutorial will show you how Dynamic Data functionality can provide the following benefits:</span></span>

- <span data-ttu-id="87cd7-111">Поля форматируются автоматически для отображения на основе их типа данных.</span><span class="sxs-lookup"><span data-stu-id="87cd7-111">Fields are automatically formatted for display based on their data type.</span></span>
- <span data-ttu-id="87cd7-112">Поля автоматически проверяются на основе их типа данных.</span><span class="sxs-lookup"><span data-stu-id="87cd7-112">Fields are automatically validated based on their data type.</span></span>
- <span data-ttu-id="87cd7-113">Метаданные можно добавить в модель данных для настройки поведения форматирования и проверки.</span><span class="sxs-lookup"><span data-stu-id="87cd7-113">You can add metadata to the data model to customize formatting and validation behavior.</span></span> <span data-ttu-id="87cd7-114">Если это сделать, можно добавить правила форматирования и проверки в одном месте, и они автоматически выполняется применяться везде доступ к поля, с помощью элементов управления платформы динамических данных.</span><span class="sxs-lookup"><span data-stu-id="87cd7-114">When you do this, you can add the formatting and validation rules in just one place, and they're automatically applied everywhere you access the fields using Dynamic Data controls.</span></span>

<span data-ttu-id="87cd7-115">Чтобы увидеть, как это работает, изменим элементов управления, использовать для отображения и редактирования поля в существующем *Students.aspx* страницы и вы добавите форматирования и проверки метаданных поля имя и дата `Student` типа сущности.</span><span class="sxs-lookup"><span data-stu-id="87cd7-115">To see how this works, you'll change the controls you use to display and edit fields in the existing *Students.aspx* page, and you'll add formatting and validation metadata to the name and date fields of the `Student` entity type.</span></span>

<span data-ttu-id="87cd7-116">[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="87cd7-116">[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)</span></span>

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a><span data-ttu-id="87cd7-117">С помощью DynamicField и элементы управления DynamicControl</span><span class="sxs-lookup"><span data-stu-id="87cd7-117">Using DynamicField and DynamicControl Controls</span></span>

<span data-ttu-id="87cd7-118">Откройте *Students.aspx* страницы и в `StudentsGridView` управления замены **имя** и **Дата регистрации** `TemplateField` элементы следующей разметкой:</span><span class="sxs-lookup"><span data-stu-id="87cd7-118">Open the *Students.aspx* page and in the `StudentsGridView` control replace the **Name** and **Enrollment Date** `TemplateField` elements with the following markup:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

<span data-ttu-id="87cd7-119">Эта разметка использует `DynamicControl` управляет вместо `TextBox` и `Label` элементов управления в ученик имя шаблона поля, а также `DynamicField` управления для даты регистрации.</span><span class="sxs-lookup"><span data-stu-id="87cd7-119">This markup uses `DynamicControl` controls in place of `TextBox` and `Label` controls in the student name template field, and it uses a `DynamicField` control for the enrollment date.</span></span> <span data-ttu-id="87cd7-120">Указаны никакие строки формата.</span><span class="sxs-lookup"><span data-stu-id="87cd7-120">No format strings are specified.</span></span>

<span data-ttu-id="87cd7-121">Добавить `ValidationSummary` элемента управления после `StudentsGridView` элемента управления.</span><span class="sxs-lookup"><span data-stu-id="87cd7-121">Add a `ValidationSummary` control after the `StudentsGridView` control.</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

<span data-ttu-id="87cd7-122">В `SearchGridView` управления замените существующую разметку для **имя** и **Дата регистрации** столбцы, что вы делали на `StudentsGridView` управления, за исключением опустить `EditItemTemplate` элемента.</span><span class="sxs-lookup"><span data-stu-id="87cd7-122">In the `SearchGridView` control replace the markup for the **Name** and **Enrollment Date** columns as you did in the `StudentsGridView` control, except omit the `EditItemTemplate` element.</span></span> <span data-ttu-id="87cd7-123">`Columns` Элемент `SearchGridView` управления теперь содержит следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="87cd7-123">The `Columns` element of the `SearchGridView` control now contains the following markup:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

<span data-ttu-id="87cd7-124">Откройте *Students.aspx.cs* и добавьте следующее `using` инструкции:</span><span class="sxs-lookup"><span data-stu-id="87cd7-124">Open *Students.aspx.cs* and add the following `using` statement:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

<span data-ttu-id="87cd7-125">Добавление обработчика для этой страницы `Init` событий:</span><span class="sxs-lookup"><span data-stu-id="87cd7-125">Add a handler for the page's `Init` event:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

<span data-ttu-id="87cd7-126">Этот код указывает, что обеспечит динамических данных, форматирования и проверки в этих элементах управления с привязкой к данным для полей `Student` сущности.</span><span class="sxs-lookup"><span data-stu-id="87cd7-126">This code specifies that Dynamic Data will provide formatting and validation in these data-bound controls for fields of the `Student` entity.</span></span> <span data-ttu-id="87cd7-127">Если вы получаете сообщение об ошибке, как в следующем примере при выполнении страницы, обычно это значит, вы забыли вызвать `EnableDynamicData` метод в `Page_Init`:</span><span class="sxs-lookup"><span data-stu-id="87cd7-127">If you get an error message like the following example when you run the page, it typically means you've forgotten to call the `EnableDynamicData` method in `Page_Init`:</span></span>

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

<span data-ttu-id="87cd7-128">Запустите страницу.</span><span class="sxs-lookup"><span data-stu-id="87cd7-128">Run the page.</span></span>

<span data-ttu-id="87cd7-129">[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="87cd7-129">[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)</span></span>

<span data-ttu-id="87cd7-130">В **Дата регистрации** столбец отображения времени и даты, так как тип свойства `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="87cd7-130">In the **Enrollment Date** column, the time is displayed along with the date because the property type is `DateTime`.</span></span> <span data-ttu-id="87cd7-131">Это будет исправлено позже.</span><span class="sxs-lookup"><span data-stu-id="87cd7-131">You'll fix that later.</span></span>

<span data-ttu-id="87cd7-132">Теперь обратите внимание, что динамических данных обеспечивает автоматическую проверку данных.</span><span class="sxs-lookup"><span data-stu-id="87cd7-132">For now, notice that Dynamic Data automatically provides basic data validation.</span></span> <span data-ttu-id="87cd7-133">Например, щелкните **изменить**очистить поля «Дата», нажмите кнопку **обновление**, и вы увидите, что динамических данных автоматически делает это обязательное поле потому, что значение не допускает значения NULL в модели данных.</span><span class="sxs-lookup"><span data-stu-id="87cd7-133">For example, click **Edit**, clear the date field, click **Update**, and you see that Dynamic Data automatically makes this a required field because the value is not nullable in the data model.</span></span> <span data-ttu-id="87cd7-134">На странице отображается звездочка после поля и сообщение об ошибке в `ValidationSummary` управления:</span><span class="sxs-lookup"><span data-stu-id="87cd7-134">The page displays an asterisk after the field and an error message in the `ValidationSummary` control:</span></span>

<span data-ttu-id="87cd7-135">[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="87cd7-135">[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)</span></span>

<span data-ttu-id="87cd7-136">Можно опустить `ValidationSummary` поскольку элемент управления, также можно навести указатель мыши на звездочку, чтобы просмотреть сообщение об ошибке:</span><span class="sxs-lookup"><span data-stu-id="87cd7-136">You could omit the `ValidationSummary` control, because you can also hold the mouse pointer over the asterisk to see the error message:</span></span>

<span data-ttu-id="87cd7-137">[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="87cd7-137">[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)</span></span>

<span data-ttu-id="87cd7-138">Платформа динамических данных также будет проверять данные, введенные в **Дата регистрации** поле представляет собой допустимую дату:</span><span class="sxs-lookup"><span data-stu-id="87cd7-138">Dynamic Data will also validate that data entered in the **Enrollment Date** field is a valid date:</span></span>

<span data-ttu-id="87cd7-139">[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="87cd7-139">[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)</span></span>

<span data-ttu-id="87cd7-140">Как видите, это универсальное сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="87cd7-140">As you can see, this is a generic error message.</span></span> <span data-ttu-id="87cd7-141">В следующем разделе вы увидите, как настроить сообщения, а также проверки и правила форматирования.</span><span class="sxs-lookup"><span data-stu-id="87cd7-141">In the next section you'll see how to customize messages as well as validation and formatting rules.</span></span>

## <a name="adding-metadata-to-the-data-model"></a><span data-ttu-id="87cd7-142">Добавление метаданных в модель данных</span><span class="sxs-lookup"><span data-stu-id="87cd7-142">Adding Metadata to the Data Model</span></span>

<span data-ttu-id="87cd7-143">Как правило требуется настроить функциональные возможности, предоставляемые платформой динамических данных.</span><span class="sxs-lookup"><span data-stu-id="87cd7-143">Typically, you want to customize the functionality provided by Dynamic Data.</span></span> <span data-ttu-id="87cd7-144">Например можно изменить способ отображения данных и содержимое сообщения об ошибках.</span><span class="sxs-lookup"><span data-stu-id="87cd7-144">For example, you might change how data is displayed and the content of error messages.</span></span> <span data-ttu-id="87cd7-145">Обычно приходится также настраивать правила проверки данных, чтобы предоставить больше возможностей, чем предоставляет платформа динамических данных автоматически на основе типов данных.</span><span class="sxs-lookup"><span data-stu-id="87cd7-145">You typically also customize data validation rules to provide more functionality than what Dynamic Data provides automatically based on data types.</span></span> <span data-ttu-id="87cd7-146">Чтобы сделать это, создайте разделяемых классов, которые соответствуют типам сущностей.</span><span class="sxs-lookup"><span data-stu-id="87cd7-146">To do this, you create partial classes that correspond to entity types.</span></span>

<span data-ttu-id="87cd7-147">В **обозревателе решений**, щелкните правой кнопкой мыши **ContosoUniversity** проекта, выберите **добавить ссылку**и добавьте ссылку на `System.ComponentModel.DataAnnotations`.</span><span class="sxs-lookup"><span data-stu-id="87cd7-147">In **Solution Explorer**, right-click the **ContosoUniversity** project, select **Add Reference**, and add a reference to `System.ComponentModel.DataAnnotations`.</span></span>

<span data-ttu-id="87cd7-148">[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="87cd7-148">[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)</span></span>

<span data-ttu-id="87cd7-149">В *DAL* папки, создайте новый файл класса, назовите его *Student.cs*и замените код шаблона в него следующий код.</span><span class="sxs-lookup"><span data-stu-id="87cd7-149">In the *DAL* folder, create a new class file, name it *Student.cs*, and replace the template code in it with the following code.</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

<span data-ttu-id="87cd7-150">Этот код создает разделяемый класс для `Student` сущности.</span><span class="sxs-lookup"><span data-stu-id="87cd7-150">This code creates a partial class for the `Student` entity.</span></span> <span data-ttu-id="87cd7-151">`MetadataType` Атрибут, примененный к этот разделяемый класс определяет класс, который используется для задания метаданных.</span><span class="sxs-lookup"><span data-stu-id="87cd7-151">The `MetadataType` attribute applied to this partial class identifies the class that you're using to specify metadata.</span></span> <span data-ttu-id="87cd7-152">Класс метаданных может иметь любое имя, но с помощью имя сущности, а также «Метаданные» — это распространенная практика.</span><span class="sxs-lookup"><span data-stu-id="87cd7-152">The metadata class can have any name, but using the entity name plus "Metadata" is a common practice.</span></span>

<span data-ttu-id="87cd7-153">Атрибуты, применяемые к свойствам в классе метаданных указать форматирование, проверки, правила и сообщения об ошибках.</span><span class="sxs-lookup"><span data-stu-id="87cd7-153">The attributes applied to properties in the metadata class specify formatting, validation, rules, and error messages.</span></span> <span data-ttu-id="87cd7-154">Атрибуты, приведенные здесь имеет следующие результаты:</span><span class="sxs-lookup"><span data-stu-id="87cd7-154">The attributes shown here will have the following results:</span></span>

- <span data-ttu-id="87cd7-155">`EnrollmentDate` отобразится дата (без времени).</span><span class="sxs-lookup"><span data-stu-id="87cd7-155">`EnrollmentDate` will display as a date (without a time).</span></span>
- <span data-ttu-id="87cd7-156">Оба имени поля должны быть 25 символов или меньше и сообщения о пользовательской ошибке предоставляется.</span><span class="sxs-lookup"><span data-stu-id="87cd7-156">Both name fields must be 25 characters or less in length, and a custom error message is provided.</span></span>
- <span data-ttu-id="87cd7-157">Оба имени поля являются обязательными, и предоставляется пользовательское сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="87cd7-157">Both name fields are required, and a custom error message is provided.</span></span>

<span data-ttu-id="87cd7-158">Запустите *Students.aspx* страницу еще раз и разделе, теперь отображаются даты без времени:</span><span class="sxs-lookup"><span data-stu-id="87cd7-158">Run the *Students.aspx* page again, and you see that the dates are now displayed without times:</span></span>

<span data-ttu-id="87cd7-159">[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="87cd7-159">[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)</span></span>

<span data-ttu-id="87cd7-160">Изменить строку и попытайтесь удалить значения в полях имен.</span><span class="sxs-lookup"><span data-stu-id="87cd7-160">Edit a row and try to clear the values in the name fields.</span></span> <span data-ttu-id="87cd7-161">Звездочки, указывающие на ошибки поля отображаются по мере оставить поле, прежде чем нажимать кнопку **обновление**.</span><span class="sxs-lookup"><span data-stu-id="87cd7-161">The asterisks indicating field errors appear as soon as you leave a field, before you click **Update**.</span></span> <span data-ttu-id="87cd7-162">При нажатии кнопки **обновление**, на странице отображается указанное сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="87cd7-162">When you click **Update**, the page displays the error message text you specified.</span></span>

<span data-ttu-id="87cd7-163">[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="87cd7-163">[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)</span></span>

<span data-ttu-id="87cd7-164">Попробуйте ввести имена, которые не более 25 символов, нажмите кнопку **обновление**, и на странице отображается указанное сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="87cd7-164">Try to enter names that are longer than 25 characters, click **Update**, and the page displays the error message text you specified.</span></span>

<span data-ttu-id="87cd7-165">[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="87cd7-165">[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)</span></span>

<span data-ttu-id="87cd7-166">Теперь, когда вы настроили эти правила форматирования и проверки в метаданных модели данных, правила будут автоматически применяться на каждой странице, который будет отображать или изменения этих полей позволяет при условии, что вы используете `DynamicControl` или `DynamicField` элементов управления.</span><span class="sxs-lookup"><span data-stu-id="87cd7-166">Now that you've set up these formatting and validation rules in the data model metadata, the rules will automatically be applied on every page that displays or allows changes to these fields, so long as you use `DynamicControl` or `DynamicField` controls.</span></span> <span data-ttu-id="87cd7-167">Это уменьшает объем избыточный код, для записи, благодаря чему программирования и тестирования, и гарантирует, что данные форматирования и проверки согласованы во всем приложении.</span><span class="sxs-lookup"><span data-stu-id="87cd7-167">This reduces the amount of redundant code you have to write, which makes programming and testing easier, and it ensures that data formatting and validation are consistent throughout an application.</span></span>

## <a name="more-information"></a><span data-ttu-id="87cd7-168">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="87cd7-168">More Information</span></span>

<span data-ttu-id="87cd7-169">Это заключительный шаг ряд учебники по началу работы с платформой Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="87cd7-169">This concludes this series of tutorials on Getting Started with the Entity Framework.</span></span> <span data-ttu-id="87cd7-170">Дополнительные ресурсы помогут вам освоить использование Entity Framework продолжить работу с [в первом учебнике следующей последовательности учебника Entity Framework](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) или посетите следующие сайты:</span><span class="sxs-lookup"><span data-stu-id="87cd7-170">For more resources to help you learn how to use the Entity Framework, continue with [the first tutorial in the next Entity Framework tutorial series](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) or visit the following sites:</span></span>

- [<span data-ttu-id="87cd7-171">Entity Framework вопросы и ответы</span><span class="sxs-lookup"><span data-stu-id="87cd7-171">Entity Framework FAQ</span></span>](http://www.ef-faq.org/introduction.html)
- [<span data-ttu-id="87cd7-172">Блог команды Entity Framework</span><span class="sxs-lookup"><span data-stu-id="87cd7-172">The Entity Framework Team Blog</span></span>](https://blogs.msdn.com/b/adonet/)
- [<span data-ttu-id="87cd7-173">Entity Framework в библиотеке MSDN</span><span class="sxs-lookup"><span data-stu-id="87cd7-173">Entity Framework in the MSDN Library</span></span>](https://msdn.microsoft.com/library/bb399572.aspx)
- [<span data-ttu-id="87cd7-174">Entity Framework в центре разработчиков MSDN данных</span><span class="sxs-lookup"><span data-stu-id="87cd7-174">Entity Framework in the MSDN Data Developer Center</span></span>](https://msdn.microsoft.com/data/ef.aspx)
- [<span data-ttu-id="87cd7-175">Обзор элемента управления EntityDataSource веб-сервера в библиотеке MSDN</span><span class="sxs-lookup"><span data-stu-id="87cd7-175">EntityDataSource Web Server Control Overview in the MSDN Library</span></span>](https://msdn.microsoft.com/library/cc488502.aspx)
- [<span data-ttu-id="87cd7-176">Элемент управления EntityDataSource Справочник по API в библиотеке MSDN</span><span class="sxs-lookup"><span data-stu-id="87cd7-176">EntityDataSource control API reference in the MSDN Library</span></span>](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [<span data-ttu-id="87cd7-177">Форумы Entity Framework в MSDN</span><span class="sxs-lookup"><span data-stu-id="87cd7-177">Entity Framework Forums on MSDN</span></span>](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [<span data-ttu-id="87cd7-178">Блог Джули Лерман</span><span class="sxs-lookup"><span data-stu-id="87cd7-178">Julie Lerman's blog</span></span>](http://thedatafarm.com/blog/)

> [!div class="step-by-step"]
> [<span data-ttu-id="87cd7-179">Назад</span><span class="sxs-lookup"><span data-stu-id="87cd7-179">Previous</span></span>](the-entity-framework-and-aspnet-getting-started-part-7.md)
