---
title: "Основные ASP.NET MVC с основными EF - модели данных — 5, 10"
author: tdykstra
description: "В этом учебнике добавляйте дополнительные сущности и связи и настроить модель данных, указав форматирование, проверки и правила сопоставления базы данных."
keywords: "Заметок к данным ASP.NET Core, Entity Framework Core,"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 0dd63913-a041-48b6-96a4-3aeaedbdf5d0
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/complex-data-model
ms.openlocfilehash: a9e255040c300bc5ce55a356e17e6912dbaeaf88
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
# <a name="creating-a-complex-data-model---ef-core-with-aspnet-core-mvc-tutorial-5-of-10"></a><span data-ttu-id="eca80-104">Создание сложных данных модели - Core EF учебнику ASP.NET Core MVC (5, 10)</span><span class="sxs-lookup"><span data-stu-id="eca80-104">Creating a complex data model - EF Core with ASP.NET Core MVC tutorial (5 of 10)</span></span>

<span data-ttu-id="eca80-105">По [Tom Dykstra](https://github.com/tdykstra) и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="eca80-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="eca80-106">Contoso университета примера веб-приложения показано, как создавать веб-приложения ASP.NET MVC ядра с помощью основного Entity Framework и Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="eca80-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="eca80-107">Сведения о учебника серии см [в первом учебнике ряда](intro.md).</span><span class="sxs-lookup"><span data-stu-id="eca80-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="eca80-108">В предыдущих занятий вы работали с простая модель данных, созданный из трех сущностей.</span><span class="sxs-lookup"><span data-stu-id="eca80-108">In the previous tutorials, you worked with a simple data model that was composed of three entities.</span></span> <span data-ttu-id="eca80-109">В этом учебнике вы добавите несколько сущностей и связей, путем указания форматирования, проверки и правила сопоставления базы данных будет настроить модель данных.</span><span class="sxs-lookup"><span data-stu-id="eca80-109">In this tutorial, you'll add more entities and relationships and you'll customize the data model by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="eca80-110">После завершения классы сущностей составляют модель данных, завершенные, показан на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="eca80-110">When you're finished, the entity classes will make up the completed data model that's shown in the following illustration:</span></span>

![Схема Entity](complex-data-model/_static/diagram.png)

## <a name="customize-the-data-model-by-using-attributes"></a><span data-ttu-id="eca80-112">Настройка модели данных с помощью атрибутов</span><span class="sxs-lookup"><span data-stu-id="eca80-112">Customize the Data Model by Using Attributes</span></span>

<span data-ttu-id="eca80-113">В этом разделе вы увидите, как настроить модель данных с помощью атрибутов, укажите параметры форматирования, проверки и правила сопоставления базы данных.</span><span class="sxs-lookup"><span data-stu-id="eca80-113">In this section you'll see how to customize the data model by using attributes that specify formatting, validation, and database mapping rules.</span></span> <span data-ttu-id="eca80-114">Затем в некоторых из следующих разделов, который вы создаете всю модель данных школе путем добавления атрибутов к классам вы уже создали и создание новых классов для остальных типов сущностей в модели.</span><span class="sxs-lookup"><span data-stu-id="eca80-114">Then in several of the following sections you'll create the complete School data model by adding attributes to the classes you already created and creating new classes for the remaining entity types in the model.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="eca80-115">Атрибут типа данных</span><span class="sxs-lookup"><span data-stu-id="eca80-115">The DataType attribute</span></span>

<span data-ttu-id="eca80-116">Для дат регистрации студентов все веб-страниц в настоящее время отображения времени и даты, несмотря на то, что все, что важна для этого поля является датой.</span><span class="sxs-lookup"><span data-stu-id="eca80-116">For student enrollment dates, all of the web pages currently display the time along with the date, although all you care about for this field is the date.</span></span> <span data-ttu-id="eca80-117">Используя атрибуты данных заметок, ее можно создать, позволяющим формат отображения в каждого представления, отображающего данные изменения кода.</span><span class="sxs-lookup"><span data-stu-id="eca80-117">By using data annotation attributes, you can make one code change that will fix the display format in every view that shows the data.</span></span> <span data-ttu-id="eca80-118">Пример этого процесса, что вы добавите атрибут `EnrollmentDate` свойства `Student` класса.</span><span class="sxs-lookup"><span data-stu-id="eca80-118">To see an example of how to do that, you'll add an attribute to the `EnrollmentDate` property in the `Student` class.</span></span>

<span data-ttu-id="eca80-119">В *Models/Student.cs*, добавьте `using` инструкции для `System.ComponentModel.DataAnnotations` пространства имен и добавьте `DataType` и `DisplayFormat` атрибуты `EnrollmentDate` свойства, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="eca80-119">In *Models/Student.cs*, add a `using` statement for the `System.ComponentModel.DataAnnotations` namespace and add `DataType` and `DisplayFormat` attributes to the `EnrollmentDate` property, as shown in the following example:</span></span>

<span data-ttu-id="eca80-120">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]</span><span class="sxs-lookup"><span data-stu-id="eca80-120">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]</span></span>

<span data-ttu-id="eca80-121">`DataType` Атрибут используется для указания типа данных, который является более точным определением, чем встроенный тип базы данных.</span><span class="sxs-lookup"><span data-stu-id="eca80-121">The `DataType` attribute is used to specify a data type that is more specific than the database intrinsic type.</span></span> <span data-ttu-id="eca80-122">В этом случае нам нужен только для отслеживания даты, не даты и времени.</span><span class="sxs-lookup"><span data-stu-id="eca80-122">In this case we only want to keep track of the date, not the date and time.</span></span> <span data-ttu-id="eca80-123">`DataType` Перечисление предоставляет для многих типов данных, таких как дата, время, PhoneNumber, валюты, EmailAddress и многое другое.</span><span class="sxs-lookup"><span data-stu-id="eca80-123">The  `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="eca80-124">`DataType` Атрибута также позволяет приложению автоматически предоставляют функции определенного типа.</span><span class="sxs-lookup"><span data-stu-id="eca80-124">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="eca80-125">Например `mailto:` связи могут создаваться для `DataType.EmailAddress`, и элемент выбора даты, которые могут быть предоставлены для `DataType.Date` в браузерах, поддерживающих HTML5.</span><span class="sxs-lookup"><span data-stu-id="eca80-125">For example, a `mailto:` link can be created for `DataType.EmailAddress`, and a date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="eca80-126">`DataType` Атрибут выдает HTML 5 `data-` (произносится данных тире) атрибутов, которые можно понять браузеров HTML 5.</span><span class="sxs-lookup"><span data-stu-id="eca80-126">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers can understand.</span></span> <span data-ttu-id="eca80-127">`DataType` Атрибуты не имеют каких-либо проверок.</span><span class="sxs-lookup"><span data-stu-id="eca80-127">The `DataType` attributes do not provide any validation.</span></span>

<span data-ttu-id="eca80-128">`DataType.Date`Задает формат отображения даты.</span><span class="sxs-lookup"><span data-stu-id="eca80-128">`DataType.Date` does not specify the format of the date that is displayed.</span></span> <span data-ttu-id="eca80-129">По умолчанию поле данных отображается в соответствии с форматы по умолчанию, в зависимости от сервера CultureInfo.</span><span class="sxs-lookup"><span data-stu-id="eca80-129">By default, the data field is displayed according to the default formats based on the server's CultureInfo.</span></span>

<span data-ttu-id="eca80-130">`DisplayFormat` Атрибута можно явно указать формат даты:</span><span class="sxs-lookup"><span data-stu-id="eca80-130">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="eca80-131">`ApplyFormatInEditMode` Параметр указывает, что форматирование должен также быть применяется, когда значение отображается в текстовом поле для редактирования.</span><span class="sxs-lookup"><span data-stu-id="eca80-131">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied when the value is displayed in a text box for editing.</span></span> <span data-ttu-id="eca80-132">(Может не потребоваться, для некоторых полей — например, для значений денежных сумм не можно обозначение денежной единицы в текстовом поле для изменения.)</span><span class="sxs-lookup"><span data-stu-id="eca80-132">(You might not want that for some fields -- for example, for currency values, you might not want the currency symbol in the text box for editing.)</span></span>

<span data-ttu-id="eca80-133">Можно использовать `DisplayFormat` атрибут сам, но обычно имеет смысл использовать `DataType` также атрибут.</span><span class="sxs-lookup"><span data-stu-id="eca80-133">You can use the `DisplayFormat` attribute by itself, but it's generally a good idea to use the `DataType` attribute also.</span></span> <span data-ttu-id="eca80-134">`DataType` Атрибут передает семантику данных, а не как отображать его на экране и предоставляет следующие преимущества, которые вы не получаете с `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="eca80-134">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with `DisplayFormat`:</span></span>

* <span data-ttu-id="eca80-135">Браузер может включить функции HTML5 (например, для отображения элемента управления календаря, локализованными обозначение денежной единицы, отправка ссылок, некоторые клиентские ввода проверки и т. д.).</span><span class="sxs-lookup"><span data-stu-id="eca80-135">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, some client-side input validation, etc.).</span></span>

* <span data-ttu-id="eca80-136">По умолчанию браузер будет отображаться данные, используя правильный формат, соответствующий применяемой локали.</span><span class="sxs-lookup"><span data-stu-id="eca80-136">By default, the browser will render data using the correct format based on your locale.</span></span>

<span data-ttu-id="eca80-137">Дополнительные сведения см. в разделе [ \<ввода > тег документации вспомогательный](../../mvc/views/working-with-forms.md#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="eca80-137">For more information, see the [\<input> tag helper documentation](../../mvc/views/working-with-forms.md#the-input-tag-helper).</span></span>

<span data-ttu-id="eca80-138">Снова запустите страницу индекса студентов и обратите внимание раз больше не отображаются для дат регистрации.</span><span class="sxs-lookup"><span data-stu-id="eca80-138">Run the Students Index page again and notice that times are no longer displayed for the enrollment dates.</span></span> <span data-ttu-id="eca80-139">Также будет иметь значение true для любого представления, использующего модель студента.</span><span class="sxs-lookup"><span data-stu-id="eca80-139">The same will be true for any view that uses the Student model.</span></span>

![Отображение даты без времени страницы индекса студентов](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="eca80-141">Атрибут StringLength</span><span class="sxs-lookup"><span data-stu-id="eca80-141">The StringLength attribute</span></span>

<span data-ttu-id="eca80-142">Также можно указать правила проверки данных и сообщений об ошибках проверки с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="eca80-142">You can also specify data validation rules and validation error messages using attributes.</span></span> <span data-ttu-id="eca80-143">`StringLength` Атрибут задает максимальную длину в базе данных и предоставляет на стороне клиента и на стороне сервера проверки ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="eca80-143">The `StringLength` attribute sets the maximum length  in the database and provides client side and server side validation for ASP.NET MVC.</span></span> <span data-ttu-id="eca80-144">Можно также указать минимальной длины строки в этом атрибуте, но минимальное значение не оказывает влияния на схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="eca80-144">You can also specify the minimum string length in this attribute, but the minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="eca80-145">Предположим, что вы хотите убедиться, что пользователь не ввел более 50 символов для имени.</span><span class="sxs-lookup"><span data-stu-id="eca80-145">Suppose you want to ensure that users don't enter more than 50 characters for a name.</span></span> <span data-ttu-id="eca80-146">Чтобы добавить это ограничение, добавьте `StringLength` атрибуты `LastName` и `FirstMidName` свойства, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="eca80-146">To add this limitation, add `StringLength` attributes to the `LastName` and `FirstMidName` properties, as shown in the following example:</span></span>

<span data-ttu-id="eca80-147">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]</span><span class="sxs-lookup"><span data-stu-id="eca80-147">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]</span></span>

<span data-ttu-id="eca80-148">`StringLength` Атрибут не предотвратить ввода пробелы в имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="eca80-148">The `StringLength` attribute won't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="eca80-149">Можно использовать `RegularExpression` атрибутов для применения ограничений входных данных.</span><span class="sxs-lookup"><span data-stu-id="eca80-149">You can use the `RegularExpression` attribute to apply restrictions to the input.</span></span> <span data-ttu-id="eca80-150">Например следующий код требует первого символа в записываются прописными буквами и остальные символы преобразуются в алфавитном порядке:</span><span class="sxs-lookup"><span data-stu-id="eca80-150">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z''-'\s]*$")]
```

<span data-ttu-id="eca80-151">`MaxLength` Атрибут предоставляет функциональность, аналогичную `StringLength` атрибута, но не предоставляет клиентской проверки.</span><span class="sxs-lookup"><span data-stu-id="eca80-151">The `MaxLength` attribute provides functionality similar to the `StringLength` attribute but doesn't provide client side validation.</span></span>

<span data-ttu-id="eca80-152">Модель базы данных изменилось в результате которого требует изменения в схеме базы данных.</span><span class="sxs-lookup"><span data-stu-id="eca80-152">The database model has now changed in a way that requires a change in the database schema.</span></span> <span data-ttu-id="eca80-153">Миграция будет использоваться для обновления схемы без потери данных, могут добавления в базу данных с помощью пользовательского интерфейса приложения.</span><span class="sxs-lookup"><span data-stu-id="eca80-153">You'll use migrations to update the schema without losing any data that you may have added to the database by using the application UI.</span></span>

<span data-ttu-id="eca80-154">Сохраните изменения и выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="eca80-154">Save your changes and build the project.</span></span> <span data-ttu-id="eca80-155">Затем откройте командную строку в папке проекта и введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="eca80-155">Then open the command window in the project folder and enter the following commands:</span></span>

```console
dotnet ef migrations add MaxLengthOnNames
```

```console
dotnet ef database update
```

<span data-ttu-id="eca80-156">`migrations add` Команда выдает предупреждение, может привести к потере данных, поскольку это изменение делает максимальную длину более короткие для двух столбцов.</span><span class="sxs-lookup"><span data-stu-id="eca80-156">The `migrations add` command warns that data loss may occur, because the change makes the maximum length shorter for two columns.</span></span>  <span data-ttu-id="eca80-157">Миграция создает файл с именем * \<timeStamp > _MaxLengthOnNames.cs*.</span><span class="sxs-lookup"><span data-stu-id="eca80-157">Migrations creates a file named *\<timeStamp>_MaxLengthOnNames.cs*.</span></span> <span data-ttu-id="eca80-158">Этот файл содержит код в `Up` метод, который будет обновлять базу данных в соответствии с текущей моделью данных.</span><span class="sxs-lookup"><span data-stu-id="eca80-158">This file contains code in the `Up` method that will update the database to match the current data model.</span></span> <span data-ttu-id="eca80-159">`database update` Этот код выполнялась команда.</span><span class="sxs-lookup"><span data-stu-id="eca80-159">The `database update` command ran that code.</span></span>

<span data-ttu-id="eca80-160">Отметка времени, в качестве префикса к имени файла миграции используется платформой Entity Framework для упорядочения для миграции.</span><span class="sxs-lookup"><span data-stu-id="eca80-160">The timestamp prefixed to the migrations file name is used by Entity Framework to order the migrations.</span></span> <span data-ttu-id="eca80-161">Можно создать несколько миграции перед выполнением команды update-database, и затем все миграций применяются в порядке, в котором они были созданы.</span><span class="sxs-lookup"><span data-stu-id="eca80-161">You can create multiple migrations before running the update-database command, and then all of the migrations are applied in the order in which they were created.</span></span>

<span data-ttu-id="eca80-162">Откройте страницу создать и введите имя длиной более 50 символов.</span><span class="sxs-lookup"><span data-stu-id="eca80-162">Run the Create page, and enter either name longer than 50 characters.</span></span> <span data-ttu-id="eca80-163">Нажмите кнопку "Создать", клиентская проверка отобразится сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="eca80-163">When you click Create, client side validation shows an error message.</span></span>

![Студенты индексная страница, отображающий строку ошибки длины](complex-data-model/_static/string-length-errors.png)

### <a name="the-column-attribute"></a><span data-ttu-id="eca80-165">Атрибут столбца</span><span class="sxs-lookup"><span data-stu-id="eca80-165">The Column attribute</span></span>

<span data-ttu-id="eca80-166">Атрибуты используются для управления как классы и свойства сопоставляются в базу данных.</span><span class="sxs-lookup"><span data-stu-id="eca80-166">You can also use attributes to control how your classes and properties are mapped to the database.</span></span> <span data-ttu-id="eca80-167">Предположим, что вы использовали имя `FirstMidName` -имя поля, так как поле также может содержать отчество.</span><span class="sxs-lookup"><span data-stu-id="eca80-167">Suppose you had used the name `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span> <span data-ttu-id="eca80-168">Но необходимо сделать столбец базы данных могут называться `FirstName`, так как в это имя привыкшим пользователей, которые будет записывать нерегламентированные запросы к базе данных.</span><span class="sxs-lookup"><span data-stu-id="eca80-168">But you want the database column to be named `FirstName`, because users who will be writing ad-hoc queries against the database are accustomed to that name.</span></span> <span data-ttu-id="eca80-169">Чтобы это сопоставление, можно использовать `Column` атрибута.</span><span class="sxs-lookup"><span data-stu-id="eca80-169">To make this mapping, you can use the `Column` attribute.</span></span>

<span data-ttu-id="eca80-170">`Column` Атрибут указывает, что при создании базы данных в столбце `Student` таблицу, которая сопоставляет `FirstMidName` свойство с именем `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="eca80-170">The `Column` attribute specifies that when the database is created, the column of the `Student` table that maps to the `FirstMidName` property will be named `FirstName`.</span></span> <span data-ttu-id="eca80-171">Другими словами, когда ваш код ссылается `Student.FirstMidName`, данные получаются из или обновлены в `FirstName` столбец `Student` таблицы.</span><span class="sxs-lookup"><span data-stu-id="eca80-171">In other words, when your code refers to `Student.FirstMidName`, the data will come from or be updated in the `FirstName` column of the `Student` table.</span></span> <span data-ttu-id="eca80-172">Если не указать имена столбцов, они являются присваивается то же имя, как и имя свойства.</span><span class="sxs-lookup"><span data-stu-id="eca80-172">If you don't specify column names, they are given the same name as the property name.</span></span>

<span data-ttu-id="eca80-173">В *Student.cs* файл, добавьте `using` инструкции для `System.ComponentModel.DataAnnotations.Schema` и добавьте атрибут имени столбца для `FirstMidName` свойства, как показано в следующий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="eca80-173">In the *Student.cs* file, add a `using` statement for `System.ComponentModel.DataAnnotations.Schema` and add the column name attribute to the `FirstMidName` property, as shown in the following highlighted code:</span></span>

<span data-ttu-id="eca80-174">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]</span><span class="sxs-lookup"><span data-stu-id="eca80-174">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]</span></span>

<span data-ttu-id="eca80-175">Добавление `Column` атрибут изменяет модель `SchoolContext`, поэтому он не будет соответствовать базе данных.</span><span class="sxs-lookup"><span data-stu-id="eca80-175">The addition of the `Column` attribute changes the model backing the `SchoolContext`, so it won't match the database.</span></span>

<span data-ttu-id="eca80-176">Сохраните изменения и выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="eca80-176">Save your changes and build the project.</span></span> <span data-ttu-id="eca80-177">Затем откройте командную строку в папке проекта и введите следующие команды, чтобы создать еще один процесс миграции:</span><span class="sxs-lookup"><span data-stu-id="eca80-177">Then open the command window in the project folder and enter the following commands to create another migration:</span></span>

```console
dotnet ef migrations add ColumnFirstName
```

```console
dotnet ef database update
```

<span data-ttu-id="eca80-178">В **обозреватель объектов SQL Server**, откройте в конструкторе таблиц учащихся, дважды щелкнув **студента** таблицы.</span><span class="sxs-lookup"><span data-stu-id="eca80-178">In **SQL Server Object Explorer**, open the Student table designer by double-clicking the **Student** table.</span></span>

![Студенты таблицы в SSOX после миграции](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="eca80-180">Перед применением миграции первых двух столбцов имен были типа nvarchar(max).</span><span class="sxs-lookup"><span data-stu-id="eca80-180">Before you applied the first two migrations, the name columns were of type nvarchar(MAX).</span></span> <span data-ttu-id="eca80-181">Они теперь nvarchar(50) и имя столбца был изменен с FirstMidName на «имя».</span><span class="sxs-lookup"><span data-stu-id="eca80-181">They are now nvarchar(50) and the column name has changed from FirstMidName to FirstName.</span></span>

> [!Note]
> <span data-ttu-id="eca80-182">При попытке скомпилировать до завершения создания всех классов сущностей в следующих разделах, могут возникнуть ошибки компилятора.</span><span class="sxs-lookup"><span data-stu-id="eca80-182">If you try to compile before you finish creating all of the entity classes in the following sections, you might get compiler errors.</span></span>

## <a name="final-changes-to-the-student-entity"></a><span data-ttu-id="eca80-183">Окончательных изменений в сущность Student</span><span class="sxs-lookup"><span data-stu-id="eca80-183">Final changes to the Student entity</span></span>

![Сущность Student](complex-data-model/_static/student-entity.png)

<span data-ttu-id="eca80-185">В *Models/Student.cs*, замените код, добавленный ранее следующий код.</span><span class="sxs-lookup"><span data-stu-id="eca80-185">In *Models/Student.cs*, replace the code you added earlier with the following code.</span></span> <span data-ttu-id="eca80-186">Изменения выделены.</span><span class="sxs-lookup"><span data-stu-id="eca80-186">The changes are highlighted.</span></span>

<span data-ttu-id="eca80-187">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]</span><span class="sxs-lookup"><span data-stu-id="eca80-187">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]</span></span>

### <a name="the-required-attribute"></a><span data-ttu-id="eca80-188">Обязательный атрибут</span><span class="sxs-lookup"><span data-stu-id="eca80-188">The Required attribute</span></span>

<span data-ttu-id="eca80-189">`Required` Атрибут делает обязательные поля имя свойства.</span><span class="sxs-lookup"><span data-stu-id="eca80-189">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="eca80-190">`Required` Атрибут не нужен для запретом типов, таких как типы значений (DateTime, int, дважды, число с плавающей запятой, и т. д.).</span><span class="sxs-lookup"><span data-stu-id="eca80-190">The `Required` attribute is not needed for non-nullable types such as value types (DateTime, int, double, float, etc.).</span></span> <span data-ttu-id="eca80-191">Типы, которые не может быть неопределенным автоматически обрабатываются как обязательные поля.</span><span class="sxs-lookup"><span data-stu-id="eca80-191">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="eca80-192">Можно удалить `Required` атрибута и замените его минимальную длину параметра `StringLength` атрибута:</span><span class="sxs-lookup"><span data-stu-id="eca80-192">You could remove the `Required` attribute and replace it with a minimum length parameter for the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="eca80-193">Атрибут отображения</span><span class="sxs-lookup"><span data-stu-id="eca80-193">The Display attribute</span></span>

<span data-ttu-id="eca80-194">`Display` Атрибут указывает, должен быть заголовок для поля «Имя», «Фамилия», «Полное имя» и «Регистрации Date», а не имя свойства в каждом экземпляре (которая имеет место не разделение слов).</span><span class="sxs-lookup"><span data-stu-id="eca80-194">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date" instead of the property name in each instance (which has no space dividing the words).</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="eca80-195">Свойство FullName вычисляется</span><span class="sxs-lookup"><span data-stu-id="eca80-195">The FullName calculated property</span></span>

<span data-ttu-id="eca80-196">`FullName`— Вычисляемое свойство, которое возвращает значение, которое создается путем объединения двух свойств.</span><span class="sxs-lookup"><span data-stu-id="eca80-196">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="eca80-197">Поэтому он имеет только метод доступа get и нет `FullName` столбец создается в базе данных.</span><span class="sxs-lookup"><span data-stu-id="eca80-197">Therefore it has only a get accessor, and no `FullName` column will be generated in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="eca80-198">Создать сущность инструктора</span><span class="sxs-lookup"><span data-stu-id="eca80-198">Create the Instructor Entity</span></span>

![Сущность инструктора](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="eca80-200">Создание *Models/Instructor.cs*, заменив шаблон код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="eca80-200">Create *Models/Instructor.cs*, replacing the template code with the following code:</span></span>

<span data-ttu-id="eca80-201">[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]</span><span class="sxs-lookup"><span data-stu-id="eca80-201">[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]</span></span>

<span data-ttu-id="eca80-202">Обратите внимание, что некоторые свойства в сущности учащихся и инструкторов совпадают.</span><span class="sxs-lookup"><span data-stu-id="eca80-202">Notice that several properties are the same in the Student and Instructor entities.</span></span> <span data-ttu-id="eca80-203">В [реализации наследования](inheritance.md) учебника далее в этой серии будет оптимизацию данного кода, чтобы исключить избыточность.</span><span class="sxs-lookup"><span data-stu-id="eca80-203">In the [Implementing Inheritance](inheritance.md) tutorial later in this series, you'll refactor this code to eliminate the redundancy.</span></span>

<span data-ttu-id="eca80-204">Можно поместить несколько атрибутов в одной строке, поэтому можно также написать `HireDate` атрибуты следующим образом:</span><span class="sxs-lookup"><span data-stu-id="eca80-204">You can put multiple attributes on one line, so you could also write the `HireDate` attributes as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="eca80-205">Свойства навигации CourseAssignments и OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="eca80-205">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="eca80-206">`CourseAssignments` И `OfficeAssignment` свойства — это свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="eca80-206">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="eca80-207">Инструктор можно обучить любое количество курсов, поэтому `CourseAssignments` определен как коллекция.</span><span class="sxs-lookup"><span data-stu-id="eca80-207">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="eca80-208">Если свойство навигации может содержать несколько сущностей, его тип должен быть список, в котором записи могут быть добавлены, удаленные и обновлены.</span><span class="sxs-lookup"><span data-stu-id="eca80-208">If a navigation property can hold multiple entities, its type must be a list in which entries can be added, deleted, and updated.</span></span>  <span data-ttu-id="eca80-209">Можно указать `ICollection<T>` или тип, такой как `List<T>` или `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="eca80-209">You can specify `ICollection<T>` or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="eca80-210">При указании `ICollection<T>`, создает EF `HashSet<T>` коллекции по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="eca80-210">If you specify `ICollection<T>`, EF creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="eca80-211">Причина, почему они являются `CourseAssignment` сущностями описывается ниже в разделе о связях многие ко многим.</span><span class="sxs-lookup"><span data-stu-id="eca80-211">The reason why these are `CourseAssignment` entities is explained below in the section about many-to-many relationships.</span></span>

<span data-ttu-id="eca80-212">Состояние университета Contoso бизнес-правила, инструктор только может иметь не более одного офиса, поэтому `OfficeAssignment` свойство содержит одну сущность OfficeAssignment (который может иметь значение null, если office не назначено).</span><span class="sxs-lookup"><span data-stu-id="eca80-212">Contoso University business rules state that an instructor can only have at most one office, so the `OfficeAssignment` property holds a single OfficeAssignment entity (which may be null if no office is assigned).</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="eca80-213">Создать сущность OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="eca80-213">Create the OfficeAssignment entity</span></span>

![OfficeAssignment сущности](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="eca80-215">Создание *Models/OfficeAssignment.cs* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="eca80-215">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

<span data-ttu-id="eca80-216">[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]</span><span class="sxs-lookup"><span data-stu-id="eca80-216">[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]</span></span>

### <a name="the-key-attribute"></a><span data-ttu-id="eca80-217">Ключевой атрибут</span><span class="sxs-lookup"><span data-stu-id="eca80-217">The Key attribute</span></span>

<span data-ttu-id="eca80-218">Имеется один к нулю или одному связь между инструктора и OfficeAssignment сущностей.</span><span class="sxs-lookup"><span data-stu-id="eca80-218">There's a one-to-zero-or-one relationship  between the Instructor and the OfficeAssignment entities.</span></span> <span data-ttu-id="eca80-219">Office назначения существует только относительно инструктора, которую он назначен, и поэтому ее первичный ключ также является его внешний ключ к сущности инструктора.</span><span class="sxs-lookup"><span data-stu-id="eca80-219">An office assignment only exists in relation to the instructor it's assigned to, and therefore its primary key is also its foreign key to the Instructor entity.</span></span> <span data-ttu-id="eca80-220">Однако платформа Entity Framework не удалось распознать автоматически InstructorID как первичный ключ сущности, так как его имя не соответствовать соглашению об именовании идентификатор или classnameID.</span><span class="sxs-lookup"><span data-stu-id="eca80-220">But the Entity Framework can't automatically recognize InstructorID as the primary key of this entity because its name doesn't follow the ID or classnameID naming convention.</span></span> <span data-ttu-id="eca80-221">Таким образом `Key` атрибут используется для идентификации в качестве ключа:</span><span class="sxs-lookup"><span data-stu-id="eca80-221">Therefore, the `Key` attribute is used to identify it as the key:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="eca80-222">Можно также использовать `Key` атрибута, если сущность имеет собственный первичный ключ, но требуется свойство name что-то отличное от classnameID или идентификатор.</span><span class="sxs-lookup"><span data-stu-id="eca80-222">You can also use the `Key` attribute if the entity does have its own primary key but you want to name the property something other than classnameID or ID.</span></span>

<span data-ttu-id="eca80-223">По умолчанию EF обрабатывает ключ как без формирования базы данных, так как столбец для идентифицирующего отношения.</span><span class="sxs-lookup"><span data-stu-id="eca80-223">By default, EF treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="eca80-224">Свойство навигации инструктора</span><span class="sxs-lookup"><span data-stu-id="eca80-224">The Instructor navigation property</span></span>

<span data-ttu-id="eca80-225">Сущность инструктора имеет значение NULL `OfficeAssignment` свойство навигации (поскольку инструктор не может иметь назначение office), и имеет не допускающий сущность OfficeAssignment `Instructor` свойство навигации (так как не может присваиваться office существует без инструктор-- `InstructorID` не допускает значение NULL).</span><span class="sxs-lookup"><span data-stu-id="eca80-225">The Instructor entity has a nullable `OfficeAssignment` navigation property (because an instructor might not have an office assignment), and the OfficeAssignment entity has a non-nullable `Instructor` navigation property (because an office assignment can't exist without an instructor -- `InstructorID` is non-nullable).</span></span> <span data-ttu-id="eca80-226">Сущность инструктора имеет связанную сущность OfficeAssignment, каждая сущность получите ссылку на другую переменную в свойстве навигации.</span><span class="sxs-lookup"><span data-stu-id="eca80-226">When an Instructor entity has a related OfficeAssignment entity, each entity will have a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="eca80-227">Можно поместить `[Required]` атрибут для свойства навигации инструктора, чтобы указать, должен быть связанные инструктора, что не нужно делать, так как `InstructorID` внешний ключ (который также является ключом к этой таблице) допускают значение NULL.</span><span class="sxs-lookup"><span data-stu-id="eca80-227">You could put a `[Required]` attribute on the Instructor navigation property to specify that there must be a related instructor, but you don't have to do that because the `InstructorID` foreign key (which is also the key to this table) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="eca80-228">Изменение сущности курса</span><span class="sxs-lookup"><span data-stu-id="eca80-228">Modify the Course Entity</span></span>

![Сущности курса](complex-data-model/_static/course-entity.png)

<span data-ttu-id="eca80-230">В *Models/Course.cs*, замените код, добавленный ранее следующий код.</span><span class="sxs-lookup"><span data-stu-id="eca80-230">In *Models/Course.cs*, replace the code you added earlier with the following code.</span></span> <span data-ttu-id="eca80-231">Изменения выделены.</span><span class="sxs-lookup"><span data-stu-id="eca80-231">The changes are highlighted.</span></span>

<span data-ttu-id="eca80-232">[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]</span><span class="sxs-lookup"><span data-stu-id="eca80-232">[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]</span></span>

<span data-ttu-id="eca80-233">Сущность курс имеет свойство внешнего ключа `DepartmentID` указывающая связанной сущности отдела и он имеет `Department` свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="eca80-233">The course entity has a foreign key property `DepartmentID` which points to the related Department entity and it has a `Department` navigation property.</span></span>

<span data-ttu-id="eca80-234">Платформа Entity Framework не требуется добавлять свойство внешнего ключа в модель данных при наличии свойства навигации для связанной сущности.</span><span class="sxs-lookup"><span data-stu-id="eca80-234">The Entity Framework doesn't require you to add a foreign key property to your data model when you have a navigation property for a related entity.</span></span>  <span data-ttu-id="eca80-235">EF автоматически создает внешние ключи в базе данных, где они необходимы и создает [затемнять свойства](https://docs.microsoft.com/ef/core/modeling/shadow-properties) для них.</span><span class="sxs-lookup"><span data-stu-id="eca80-235">EF automatically creates foreign keys in the database wherever they are needed and creates [shadow properties](https://docs.microsoft.com/ef/core/modeling/shadow-properties) for them.</span></span> <span data-ttu-id="eca80-236">Однако наличие внешнего ключа в модели данных могут выполнять обновления проще и эффективнее.</span><span class="sxs-lookup"><span data-stu-id="eca80-236">But having the foreign key in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="eca80-237">Например, при извлечения сущностью курса для редактирования сущности «отдел» имеет значение null, если он не загружается, таким образом при обновлении сущности курса, будет необходимо сначала извлечь сущности «отдел».</span><span class="sxs-lookup"><span data-stu-id="eca80-237">For example, when you fetch a course entity to edit, the  Department entity is null if you don't load it, so when you update the course entity, you would have to first fetch the Department entity.</span></span> <span data-ttu-id="eca80-238">Если свойство внешнего ключа `DepartmentID` включено в модель данных, не нужно извлечь сущности «отдел» перед обновлением.</span><span class="sxs-lookup"><span data-stu-id="eca80-238">When the foreign key property `DepartmentID` is included in the data model, you don't need to fetch the Department entity before you update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="eca80-239">Атрибут DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="eca80-239">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="eca80-240">`DatabaseGenerated` Атрибутом `None` параметр на `CourseID` свойство указывает, что значения первичного ключа предоставленного пользователем, а не созданное базой данных.</span><span class="sxs-lookup"><span data-stu-id="eca80-240">The `DatabaseGenerated` attribute with the `None` parameter on the `CourseID` property specifies that primary key values are provided by the user rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="eca80-241">По умолчанию Entity Framework предполагает генерировать значений первичного ключа в базе данных.</span><span class="sxs-lookup"><span data-stu-id="eca80-241">By default, Entity Framework assumes that primary key values are generated by the database.</span></span> <span data-ttu-id="eca80-242">Это требуется для большинства сценариев.</span><span class="sxs-lookup"><span data-stu-id="eca80-242">That's what you want in most scenarios.</span></span> <span data-ttu-id="eca80-243">Однако для набора сущностей, будет использовать курса определяемый пользователем номер серии 1000 для одного подразделения ряд 2000 для разных отделов и так далее.</span><span class="sxs-lookup"><span data-stu-id="eca80-243">However, for Course entities, you'll use a user-specified course number such as a 1000 series for one department, a 2000 series for another department, and so on.</span></span>

<span data-ttu-id="eca80-244">`DatabaseGenerated` Атрибут также может использоваться для создания значения по умолчанию, как в случае столбцов базы данных, используемых для записи даты строки был создан или обновлен.</span><span class="sxs-lookup"><span data-stu-id="eca80-244">The `DatabaseGenerated` attribute can also be used to generate default values, as in the case of database columns used to record the date a row was created or updated.</span></span>  <span data-ttu-id="eca80-245">Дополнительные сведения см. в разделе [созданные свойства](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="eca80-245">For more information, see [Generated Properties](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="eca80-246">Свойства внешнего ключа и навигации</span><span class="sxs-lookup"><span data-stu-id="eca80-246">Foreign key and navigation properties</span></span>

<span data-ttu-id="eca80-247">Свойства внешнего ключа и свойств навигации сущности курса отражают следующие связи:</span><span class="sxs-lookup"><span data-stu-id="eca80-247">The foreign key properties and navigation properties in the Course entity reflect the following relationships:</span></span>

<span data-ttu-id="eca80-248">Курс назначается одного подразделения, то есть `DepartmentID` внешнего ключа и `Department` свойства навигации по причинам, перечисленным выше.</span><span class="sxs-lookup"><span data-stu-id="eca80-248">A course is assigned to one department, so there's a `DepartmentID` foreign key and a `Department` navigation property for the reasons mentioned above.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="eca80-249">Курс может иметь любое количество студентов, зарегистрированных в нем, так что `Enrollments` свойство навигации — это коллекция:</span><span class="sxs-lookup"><span data-stu-id="eca80-249">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="eca80-250">Может обучения по нескольким инструкторов, поэтому `CourseAssignments` свойство навигации — это коллекция (тип `CourseAssignment` объясняется [позже](#many-to-many-relationships)):</span><span class="sxs-lookup"><span data-stu-id="eca80-250">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection (the type `CourseAssignment` is explained [later](#many-to-many-relationships)):</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

## <a name="create-the-department-entity"></a><span data-ttu-id="eca80-251">Создание сущности «отдел»</span><span class="sxs-lookup"><span data-stu-id="eca80-251">Create the Department entity</span></span>

![Сущность Department](complex-data-model/_static/department-entity.png)


<span data-ttu-id="eca80-253">Создание *Models/Department.cs* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="eca80-253">Create *Models/Department.cs* with the following code:</span></span>

<span data-ttu-id="eca80-254">[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]</span><span class="sxs-lookup"><span data-stu-id="eca80-254">[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="eca80-255">Атрибут столбца</span><span class="sxs-lookup"><span data-stu-id="eca80-255">The Column attribute</span></span>

<span data-ttu-id="eca80-256">Ранее вы использовали `Column` атрибут, чтобы изменить сопоставление имени столбца.</span><span class="sxs-lookup"><span data-stu-id="eca80-256">Earlier you used the `Column` attribute to change column name mapping.</span></span> <span data-ttu-id="eca80-257">В коде для сущности «отдел» `Column` атрибут используется для изменения SQL сопоставления типов данных, чтобы столбец будет определен с помощью типа money SQL Server в базе данных:</span><span class="sxs-lookup"><span data-stu-id="eca80-257">In the code for the Department entity, the `Column` attribute is being used to change SQL data type mapping so that the column will be defined using the SQL Server money type in the database:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="eca80-258">Сопоставление столбцов обычно не является обязательным, поскольку Entity Framework выбирает соответствующий тип данных SQL Server, на основе типа CLR, определяемый для свойства.</span><span class="sxs-lookup"><span data-stu-id="eca80-258">Column mapping is generally not required, because the Entity Framework chooses the appropriate SQL Server data type based on the CLR type that you define for the property.</span></span> <span data-ttu-id="eca80-259">Среда CLR `decimal` сопоставляется SQL Server с типом `decimal` типа.</span><span class="sxs-lookup"><span data-stu-id="eca80-259">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="eca80-260">Но в этом случае вы знать, что столбец будет удерживать денежные суммы больше подходит для, тип данных money.</span><span class="sxs-lookup"><span data-stu-id="eca80-260">But in this case you know that the column will be holding currency amounts, and the money data type is more appropriate for that.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="eca80-261">Свойства внешнего ключа и навигации</span><span class="sxs-lookup"><span data-stu-id="eca80-261">Foreign key and navigation properties</span></span>

<span data-ttu-id="eca80-262">Свойства внешнего ключа и навигации отражают следующие связи:</span><span class="sxs-lookup"><span data-stu-id="eca80-262">The foreign key and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="eca80-263">Отдел может или не может быть администратор и администратор всегда инструктор.</span><span class="sxs-lookup"><span data-stu-id="eca80-263">A department may or may not have an administrator, and an administrator is always an instructor.</span></span> <span data-ttu-id="eca80-264">Поэтому `InstructorID` свойство включается в качестве внешнего ключа к сущности инструктора и знак вопроса добавляется после `int` введите обозначение, — отмечает свойство как допускающие значение NULL.</span><span class="sxs-lookup"><span data-stu-id="eca80-264">Therefore the `InstructorID` property is included as the foreign key to the Instructor entity, and a question mark is added after the `int` type designation to mark the property as nullable.</span></span> <span data-ttu-id="eca80-265">Свойство навигации называется `Administrator` , но содержит сущности инструктора:</span><span class="sxs-lookup"><span data-stu-id="eca80-265">The navigation property is named `Administrator` but holds an Instructor entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="eca80-266">Отдел может иметь несколько курсов, то есть свойство навигации курсы:</span><span class="sxs-lookup"><span data-stu-id="eca80-266">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

> [!NOTE]
> <span data-ttu-id="eca80-267">По соглашению платформа Entity Framework позволяет каскадное удаление для внешних ключей, не допускающие значения NULL, а также для связи многие ко многим.</span><span class="sxs-lookup"><span data-stu-id="eca80-267">By convention, the Entity Framework enables cascade delete for non-nullable foreign keys and for many-to-many relationships.</span></span> <span data-ttu-id="eca80-268">Это может привести к циклической cascade delete правила, которые вызывает исключение при попытке добавить миграции.</span><span class="sxs-lookup"><span data-stu-id="eca80-268">This can result in circular cascade delete rules, which will cause an exception when you try to add a migration.</span></span> <span data-ttu-id="eca80-269">Например если свойство Department.InstructorID не определено как допускающие значение NULL, EF бы настроить правила cascade delete для удаления инструктора при удалении отдел, не нужны нужно иметь.</span><span class="sxs-lookup"><span data-stu-id="eca80-269">For example, if you didn't define the Department.InstructorID property as nullable, EF would configure a cascade delete rule to delete the instructor when you delete the department, which is not what you want to have happen.</span></span> <span data-ttu-id="eca80-270">При необходимости бизнес-правила `InstructorID` значение отличное от NULL, необходимо выполнить инструкцию fluent API для отключения каскадное удаление в отношении:</span><span class="sxs-lookup"><span data-stu-id="eca80-270">If your business rules required the `InstructorID` property to be non-nullable, you would have to use the following fluent API statement to disable cascade delete on the relationship:</span></span>
> ```csharp
> modelBuilder.Entity<Department>()
>    .HasOne(d => d.Administrator)
>    .WithMany()
>    .OnDelete(DeleteBehavior.Restrict)
> ```

## <a name="modify-the-enrollment-entity"></a><span data-ttu-id="eca80-271">Изменение сущности регистрации</span><span class="sxs-lookup"><span data-stu-id="eca80-271">Modify the Enrollment entity</span></span>

![Сущности регистрации](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="eca80-273">В *Models/Enrollment.cs*, замените код, добавленный ранее следующий код:</span><span class="sxs-lookup"><span data-stu-id="eca80-273">In *Models/Enrollment.cs*, replace the code you added earlier with the following code:</span></span>

<span data-ttu-id="eca80-274">[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]</span><span class="sxs-lookup"><span data-stu-id="eca80-274">[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="eca80-275">Свойства внешнего ключа и навигации</span><span class="sxs-lookup"><span data-stu-id="eca80-275">Foreign key and navigation properties</span></span>

<span data-ttu-id="eca80-276">Свойства внешнего ключа, а также свойства навигации отражают следующие связи:</span><span class="sxs-lookup"><span data-stu-id="eca80-276">The foreign key properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="eca80-277">Для одного курса, является запись регистрации, поэтому `CourseID` свойство внешнего ключа и `Course` свойство навигации:</span><span class="sxs-lookup"><span data-stu-id="eca80-277">An enrollment record is for a single course, so there's a `CourseID` foreign key property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="eca80-278">Для одного студента, является запись регистрации, поэтому `StudentID` свойство внешнего ключа и `Student` свойство навигации:</span><span class="sxs-lookup"><span data-stu-id="eca80-278">An enrollment record is for a single student, so there's a `StudentID` foreign key property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="eca80-279">Многие ко многим</span><span class="sxs-lookup"><span data-stu-id="eca80-279">Many-to-Many Relationships</span></span>

<span data-ttu-id="eca80-280">Имеется отношение многие ко многим между сущностями студентов и курс и регистрации сущности функционирует как многие ко многим Соединяемая таблица *с полезными данными* в базе данных.</span><span class="sxs-lookup"><span data-stu-id="eca80-280">There's a many-to-many relationship between the Student and Course entities, and the Enrollment entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="eca80-281">«С полезными данными» означает, что в таблице регистрации содержит дополнительные данные помимо внешние ключи для соединяемых таблиц (в данном случае первичный ключ и свойство уровень).</span><span class="sxs-lookup"><span data-stu-id="eca80-281">"With payload" means that the Enrollment table contains additional data besides foreign keys for the joined tables (in this case, a primary key and a Grade property).</span></span>

<span data-ttu-id="eca80-282">Ниже показано, как будут выглядеть эти связи в схеме сущности.</span><span class="sxs-lookup"><span data-stu-id="eca80-282">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="eca80-283">(Эта схема был создан с помощью средств управления питанием Entity Framework для EF 6.x; Создание схемы не является частью учебника, он просто используется здесь как пример.)</span><span class="sxs-lookup"><span data-stu-id="eca80-283">(This diagram was generated using the Entity Framework Power Tools for EF 6.x; creating the diagram isn't part of the tutorial, it's just being used here as an illustration.)</span></span>

![Курс студента много множественных связей](complex-data-model/_static/student-course.png)

<span data-ttu-id="eca80-285">Каждая линия связи имеет 1 на одном конце и звездочку (*) в другом, указывающее один ко многим.</span><span class="sxs-lookup"><span data-stu-id="eca80-285">Each relationship line has a 1 at one end and an asterisk (*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="eca80-286">Если в таблице регистрации не были включены сведения об оценках, его потребуется только содержат два внешних ключа CourseID и идентификатором StudentID.</span><span class="sxs-lookup"><span data-stu-id="eca80-286">If the Enrollment table didn't include grade information, it would only need to contain the two foreign keys CourseID and StudentID.</span></span> <span data-ttu-id="eca80-287">В этом случае было бы многие ко многим Соединяемая таблица без полезных данных (или таблицы присоединения) в базе данных.</span><span class="sxs-lookup"><span data-stu-id="eca80-287">In that case, it would be a many-to-many join table without payload (or a pure join table) in the database.</span></span> <span data-ttu-id="eca80-288">Сущности инструктора и Course имеют этот тип связи "многие ко многим" и следующим шагом является создание класса сущности в качестве соединения таблицы без полезных данных.</span><span class="sxs-lookup"><span data-stu-id="eca80-288">The Instructor and Course entities have that kind of many-to-many relationship, and your next step is to create an entity class to function as a join table without payload.</span></span>

<span data-ttu-id="eca80-289">(Поддерживает 6.x EF неявное соединение таблиц для связи многие ко многим, но основные EF не поддерживает.</span><span class="sxs-lookup"><span data-stu-id="eca80-289">(EF 6.x supports implicit join tables for many-to-many relationships, but EF Core does not.</span></span> <span data-ttu-id="eca80-290">Дополнительные сведения см. в разделе [обсуждения в репозитории EF Core GitHub](https://github.com/aspnet/EntityFramework/issues/1368).)</span><span class="sxs-lookup"><span data-stu-id="eca80-290">For more information, see the [discussion in the EF Core GitHub repository](https://github.com/aspnet/EntityFramework/issues/1368).)</span></span> 

## <a name="the-courseassignment-entity"></a><span data-ttu-id="eca80-291">Сущность CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="eca80-291">The CourseAssignment entity</span></span>

![CourseAssignment сущности](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="eca80-293">Создание *Models/CourseAssignment.cs* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="eca80-293">Create *Models/CourseAssignment.cs* with the following code:</span></span>

<span data-ttu-id="eca80-294">[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]</span><span class="sxs-lookup"><span data-stu-id="eca80-294">[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]</span></span>

### <a name="join-entity-names"></a><span data-ttu-id="eca80-295">Присоединение имена сущностей</span><span class="sxs-lookup"><span data-stu-id="eca80-295">Join entity names</span></span>

<span data-ttu-id="eca80-296">Соединяемая таблица является обязательным в базе данных для связи, многие ко многим инструктора-курсов, и он должен быть представлен набор сущностей.</span><span class="sxs-lookup"><span data-stu-id="eca80-296">A join table is required in the database for the Instructor-to-Courses many-to-many relationship, and it has to be represented by an entity set.</span></span> <span data-ttu-id="eca80-297">Обычно имя сущности объединения `EntityName1EntityName2`, который в этом случае будет `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="eca80-297">It's common to name a join entity `EntityName1EntityName2`, which in this case would be `CourseInstructor`.</span></span> <span data-ttu-id="eca80-298">Тем не менее рекомендуется выбрать имя, описывающее связь.</span><span class="sxs-lookup"><span data-stu-id="eca80-298">However, we recommend that you choose a name that describes the relationship.</span></span> <span data-ttu-id="eca80-299">Модели создаются простой и увеличиваться с соединениями полезных данных нет, часто получение полезных данных более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="eca80-299">Data models start out simple and grow, with no-payload joins frequently getting payloads later.</span></span> <span data-ttu-id="eca80-300">При запуске с именем описания сущности, не придется изменить имя позже.</span><span class="sxs-lookup"><span data-stu-id="eca80-300">If you start with a descriptive entity name, you won't have to change the name later.</span></span> <span data-ttu-id="eca80-301">В идеальном случае сущности объединения бы естественным имени (возможно одно слово) в бизнес-среде.</span><span class="sxs-lookup"><span data-stu-id="eca80-301">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="eca80-302">Например книги и клиенты могут быть связаны через оценок.</span><span class="sxs-lookup"><span data-stu-id="eca80-302">For example, Books and Customers could be linked through Ratings.</span></span> <span data-ttu-id="eca80-303">Для этого отношения `CourseAssignment` является более предпочтительной, чем `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="eca80-303">For this relationship, `CourseAssignment` is a better choice than `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="eca80-304">Составной ключ</span><span class="sxs-lookup"><span data-stu-id="eca80-304">Composite key</span></span>

<span data-ttu-id="eca80-305">Поскольку внешние ключи, не допускающие значения NULL и вместе уникально идентифицировать каждую строку таблицы, нет необходимости для отдельных первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="eca80-305">Since the foreign keys are not nullable and together uniquely identify each row of the table, there is no need for a separate primary key.</span></span> <span data-ttu-id="eca80-306">*InstructorID* и *CourseID* свойства должны работать в качестве составного первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="eca80-306">The *InstructorID* and *CourseID* properties should function as a composite primary key.</span></span> <span data-ttu-id="eca80-307">Единственный способ идентификации составных первичных ключей в EF, — с помощью *fluent API* (он не может выполняться с помощью атрибутов).</span><span class="sxs-lookup"><span data-stu-id="eca80-307">The only way to identify composite primary keys to EF is by using the *fluent API* (it can't be done by using attributes).</span></span> <span data-ttu-id="eca80-308">Вы увидите, как настроить составного первичного ключа в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="eca80-308">You'll see how to configure the composite primary key in the next section.</span></span>

<span data-ttu-id="eca80-309">Составной ключ гарантирует, что при наличии нескольких строк для одного курса и несколько строк для одного инструктора не может иметь несколько строк для одного инструктора и курса.</span><span class="sxs-lookup"><span data-stu-id="eca80-309">The composite key ensures that while you can have multiple rows for one course, and multiple rows for one instructor, you can't have multiple rows for the same instructor and course.</span></span> <span data-ttu-id="eca80-310">`Enrollment` Сущности объединения определяет собственный первичный ключ, возможны повторяющиеся значения такого рода.</span><span class="sxs-lookup"><span data-stu-id="eca80-310">The `Enrollment` join entity defines its own primary key, so duplicates of this sort are possible.</span></span> <span data-ttu-id="eca80-311">Для предотвращения таких дубликаты, может добавить уникальный индекс полей внешних ключей, или настроить `Enrollment` с основной составного ключа, аналогично `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="eca80-311">To prevent such duplicates, you could add a unique index on the foreign key fields, or configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="eca80-312">Дополнительные сведения см. в разделе [индексы](https://docs.microsoft.com/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="eca80-312">For more information, see [Indexes](https://docs.microsoft.com/ef/core/modeling/indexes).</span></span>

## <a name="update-the-database-context"></a><span data-ttu-id="eca80-313">Обновление контекст базы данных</span><span class="sxs-lookup"><span data-stu-id="eca80-313">Update the database context</span></span>

<span data-ttu-id="eca80-314">Добавьте следующий выделенный код в *Data/SchoolContext.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="eca80-314">Add the following highlighted code to the *Data/SchoolContext.cs* file:</span></span>

<span data-ttu-id="eca80-315">[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]</span><span class="sxs-lookup"><span data-stu-id="eca80-315">[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]</span></span>

<span data-ttu-id="eca80-316">Этот код добавляет новые сущности и настраивает сущности CourseAssignment составного первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="eca80-316">This code adds the new entities and configures the CourseAssignment entity's composite primary key.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="eca80-317">Fluent API вместо атрибутов</span><span class="sxs-lookup"><span data-stu-id="eca80-317">Fluent API alternative to attributes</span></span>

<span data-ttu-id="eca80-318">Код в `OnModelCreating` метод `DbContext` класс использует *fluent API* можно настроить поведение EF.</span><span class="sxs-lookup"><span data-stu-id="eca80-318">The code in the `OnModelCreating` method of the `DbContext` class uses the *fluent API* to configure EF behavior.</span></span> <span data-ttu-id="eca80-319">API-Интерфейс называется «fluent», поскольку часто используемые проводить ряда вызовов метода вместе в одной инструкции, как показано в примере из [EF основная документация](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):</span><span class="sxs-lookup"><span data-stu-id="eca80-319">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement, as in this example from the [EF Core documentation](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="eca80-320">В этом учебнике вы используете fluent API только для сопоставления с базой данных, нельзя выполнить с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="eca80-320">In this tutorial, you're using the fluent API only for database mapping that you can't do with attributes.</span></span> <span data-ttu-id="eca80-321">Тем не менее можно также использовать fluent API для указания форматирования, проверки и правила сопоставления, которые можно сделать с помощью атрибутов, большинство.</span><span class="sxs-lookup"><span data-stu-id="eca80-321">However, you can also use the fluent API to specify most of the formatting, validation, and mapping rules that you can do by using attributes.</span></span> <span data-ttu-id="eca80-322">Некоторые атрибуты, такие как `MinimumLength` не может применяться с помощью плавного API.</span><span class="sxs-lookup"><span data-stu-id="eca80-322">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="eca80-323">Как упоминалось ранее, `MinimumLength` не изменяет схему, применяется правило проверки стороны клиента и сервера.</span><span class="sxs-lookup"><span data-stu-id="eca80-323">As mentioned previously, `MinimumLength` doesn't change the schema, it only applies a client and server side validation rule.</span></span>

<span data-ttu-id="eca80-324">Некоторые разработчики предпочитают использовать fluent API, они могут обновлять свои классы сущностей «очистить».</span><span class="sxs-lookup"><span data-stu-id="eca80-324">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="eca80-325">Могут быть использованы смешанные атрибуты и fluent API, если требуется, и существует ряд настроек, можно только с помощью плавного API, но в целом рекомендуется выбрать один из этих двух подходов и использовать согласованно, насколько возможно.</span><span class="sxs-lookup"><span data-stu-id="eca80-325">You can mix attributes and fluent API if you want, and there are a few customizations that can only be done by using fluent API, but in general the recommended practice is to choose one of these two approaches and use that consistently as much as possible.</span></span> <span data-ttu-id="eca80-326">Если вы используете оба, обратите внимание, что везде, где возникает конфликт, переопределяет атрибуты Fluent API.</span><span class="sxs-lookup"><span data-stu-id="eca80-326">If you do use both, note that wherever there is a conflict, Fluent API overrides attributes.</span></span>

<span data-ttu-id="eca80-327">Дополнительные сведения об атрибутах и fluent API см. в разделе [методы конфигурации](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span><span class="sxs-lookup"><span data-stu-id="eca80-327">For more information about attributes vs. fluent API, see [Methods of configuration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="eca80-328">Отображение отношений объектов схемы</span><span class="sxs-lookup"><span data-stu-id="eca80-328">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="eca80-329">Ниже показана схема, создаваемая Entity Framework Power Tools для завершенной модели School.</span><span class="sxs-lookup"><span data-stu-id="eca80-329">The following illustration shows the diagram that the Entity Framework Power Tools create for the completed School model.</span></span>

![Схема Entity](complex-data-model/_static/diagram.png)

<span data-ttu-id="eca80-331">Помимо линии связей один ко многим (от 1 до \*), вы видите линию связи один к нулю или одному (1 к нулю или одному) между сущности инструктора и OfficeAssignment и линию связи нуль или один ко многим (от 0 до 1 для *) между Сущности инструктора и отдел.</span><span class="sxs-lookup"><span data-stu-id="eca80-331">Besides the one-to-many relationship lines (1 to \*), you can see here the one-to-zero-or-one relationship line (1 to 0..1) between the Instructor and OfficeAssignment entities and the zero-or-one-to-many relationship line (0..1 to *) between the Instructor and Department entities.</span></span>

## <a name="seed-the-database-with-test-data"></a><span data-ttu-id="eca80-332">Начальное значение базы данных с тестовыми данными</span><span class="sxs-lookup"><span data-stu-id="eca80-332">Seed the Database with Test Data</span></span>

<span data-ttu-id="eca80-333">Замените код в *Data/DbInitializer.cs* файла следующим кодом для предоставления данных начального значения для новых сущностей, которые вы создали.</span><span class="sxs-lookup"><span data-stu-id="eca80-333">Replace the code in the *Data/DbInitializer.cs* file with the following code in order to provide seed data for the new entities you've created.</span></span>

<span data-ttu-id="eca80-334">[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]</span><span class="sxs-lookup"><span data-stu-id="eca80-334">[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]</span></span>

<span data-ttu-id="eca80-335">Как видно в первом руководстве, большая часть кода просто создает новые объекты сущности и загружает данные в свойства, необходимые для тестирования.</span><span class="sxs-lookup"><span data-stu-id="eca80-335">As you saw in the first tutorial, most of this code simply creates new entity objects and loads sample data into properties as required for testing.</span></span> <span data-ttu-id="eca80-336">Обратите внимание на то, как обрабатываются связи многие ко многим: код создает связи путем создания сущностей в `Enrollments` и `CourseAssignment` соединения наборов сущностей.</span><span class="sxs-lookup"><span data-stu-id="eca80-336">Notice how the many-to-many relationships are handled: the code creates relationships by creating entities in the `Enrollments` and `CourseAssignment` join entity sets.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="eca80-337">Добавить миграции</span><span class="sxs-lookup"><span data-stu-id="eca80-337">Add a migration</span></span>

<span data-ttu-id="eca80-338">Сохраните изменения и выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="eca80-338">Save your changes and build the project.</span></span> <span data-ttu-id="eca80-339">Затем откройте командную строку в папке проекта и введите `migrations add` команду (не выполнять команду update-database еще):</span><span class="sxs-lookup"><span data-stu-id="eca80-339">Then open the command window in the project folder and enter the `migrations add` command (don't do the update-database command yet):</span></span>

```console
dotnet ef migrations add ComplexDataModel
```

<span data-ttu-id="eca80-340">Вы получаете предупреждение о возможной потере данных.</span><span class="sxs-lookup"><span data-stu-id="eca80-340">You get a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data. Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="eca80-341">При попытке запуска `database update` команды на этом этапе (не еще), будет получена следующая ошибка:</span><span class="sxs-lookup"><span data-stu-id="eca80-341">If you tried to run the `database update` command at this point (don't do it yet), you would get the following error:</span></span>

> <span data-ttu-id="eca80-342">Конфликт инструкции ALTER TABLE с ограничением FOREIGN KEY «FK_dbo. Course_dbo. Department_DepartmentID».</span><span class="sxs-lookup"><span data-stu-id="eca80-342">The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID".</span></span> <span data-ttu-id="eca80-343">Конфликт произошел в таблицу в базе данных «ContosoUniversity», «dbo. Отдела», столбец «DepartmentID».</span><span class="sxs-lookup"><span data-stu-id="eca80-343">The conflict occurred in database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.</span></span>

<span data-ttu-id="eca80-344">Иногда при выполнении миграции с существующими данными, необходимо вставить данные заглушки в базу данных для удовлетворения ограничения внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="eca80-344">Sometimes when you execute migrations with existing data, you need to insert stub data into the database to satisfy foreign key constraints.</span></span> <span data-ttu-id="eca80-345">Код, созданный в `Up` метод добавляет запретом DepartmentID внешний ключ в таблицу Course.</span><span class="sxs-lookup"><span data-stu-id="eca80-345">The generated code in the `Up` method adds a non-nullable DepartmentID foreign key to the Course table.</span></span> <span data-ttu-id="eca80-346">Если уже есть строк в таблицу Course при выполнении кода, `AddColumn` операция завершается неудачей, так как SQL Server не знает, какое значение в столбце, который не может иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="eca80-346">If there are already rows in the Course table when the code runs, the `AddColumn` operation fails because SQL Server doesn't know what value to put in the column that can't be null.</span></span> <span data-ttu-id="eca80-347">В этом учебнике вы будете запускать миграции на новую базу данных, но в реальном приложении необходимо сделать обрабатывать существующие данные, поэтому следующие инструкции показан пример того, как для этого переноса.</span><span class="sxs-lookup"><span data-stu-id="eca80-347">For this tutorial you'll run the migration on a new database, but in a production application you'd have to make the migration handle existing data, so the following directions show an example of how to do that.</span></span>

<span data-ttu-id="eca80-348">Чтобы сделать этот вид миграции работать с существующими данными, необходимо изменить код, чтобы предоставить значения по умолчанию для нового столбца и создания заглушки отдел с именем «Temp» в качестве подразделения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="eca80-348">To make this migration work with existing data you have to change the code to give the new column a default value, and create a stub department named "Temp" to act as the default department.</span></span> <span data-ttu-id="eca80-349">В результате существующие строки курса будут все связаны с отделом «Temp» после `Up` метода.</span><span class="sxs-lookup"><span data-stu-id="eca80-349">As a result, existing Course rows will all be related to the "Temp" department after the `Up` method runs.</span></span>

* <span data-ttu-id="eca80-350">Откройте *{timestamp}_ComplexDataModel.cs* файл.</span><span class="sxs-lookup"><span data-stu-id="eca80-350">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span> 

* <span data-ttu-id="eca80-351">Закомментировать строку кода, который добавляет столбец DepartmentID в таблицу Course.</span><span class="sxs-lookup"><span data-stu-id="eca80-351">Comment out the line of code that adds the DepartmentID column to the Course table.</span></span>

  <span data-ttu-id="eca80-352">[!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]</span><span class="sxs-lookup"><span data-stu-id="eca80-352">[!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]</span></span>

* <span data-ttu-id="eca80-353">Добавьте следующий выделенный код после кода, создающую таблицу отдел:</span><span class="sxs-lookup"><span data-stu-id="eca80-353">Add the following highlighted code after the code that creates the Department table:</span></span>

  <span data-ttu-id="eca80-354">[!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span><span class="sxs-lookup"><span data-stu-id="eca80-354">[!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span></span>

<span data-ttu-id="eca80-355">В реальном приложении вы пишете код или сценарии для добавления строк отдела, а также связанные строки курса новым строкам отдела.</span><span class="sxs-lookup"><span data-stu-id="eca80-355">In a production application, you would write code or scripts to add Department rows and relate Course rows to the new Department rows.</span></span> <span data-ttu-id="eca80-356">Затем больше не потребуется отдела «Temp» или значение по умолчанию для столбца Course.DepartmentID.</span><span class="sxs-lookup"><span data-stu-id="eca80-356">You would then no longer need the "Temp" department or the default value on the Course.DepartmentID column.</span></span>

<span data-ttu-id="eca80-357">Сохраните изменения и выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="eca80-357">Save your changes and build the project.</span></span>

## <a name="change-the-connection-string-and-update-the-database"></a><span data-ttu-id="eca80-358">Измените строку подключения и обновления базы данных</span><span class="sxs-lookup"><span data-stu-id="eca80-358">Change the connection string and update the database</span></span>

<span data-ttu-id="eca80-359">Теперь у вас есть новый код `DbInitializer` класс, который добавляет данные начальное значение для новых сущностей для пустой базы данных.</span><span class="sxs-lookup"><span data-stu-id="eca80-359">You now have new code in the `DbInitializer` class that adds seed data for the new entities to an empty database.</span></span> <span data-ttu-id="eca80-360">Чтобы создать новую пустую базу данных EF, измените имя базы данных в строке подключения в *appsettings.json* ContosoUniversity3 или другое имя, вы не использовали на компьютере, который вы используете.</span><span class="sxs-lookup"><span data-stu-id="eca80-360">To make EF create a new empty database, change the name of the database in the connection string in *appsettings.json* to ContosoUniversity3 or some other name that you haven't used on the computer you're using.</span></span>

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
```

<span data-ttu-id="eca80-361">Сохраните изменения в *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="eca80-361">Save your change to *appsettings.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="eca80-362">В качестве альтернативы для изменения имени базы данных можно удалить базы данных.</span><span class="sxs-lookup"><span data-stu-id="eca80-362">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="eca80-363">Используйте **обозреватель объектов SQL Server** (SSOX) или `database drop` команду CLI:</span><span class="sxs-lookup"><span data-stu-id="eca80-363">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```

<span data-ttu-id="eca80-364">После изменения имени базы данных или удалить базу данных, запустите `database update` команду в окне команд для выполнения миграции.</span><span class="sxs-lookup"><span data-stu-id="eca80-364">After you have changed the database name or deleted the database, run the `database update` command in the command window to execute the migrations.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="eca80-365">Запустите приложение, чтобы `DbInitializer.Initialize` метод для запуска и заполнение новой базы данных.</span><span class="sxs-lookup"><span data-stu-id="eca80-365">Run the app to cause the `DbInitializer.Initialize` method to run and populate the new database.</span></span>

<span data-ttu-id="eca80-366">Открыть базу данных в SSOX, как это было ранее, а затем разверните **таблиц** узел, чтобы увидеть, что все таблицы были созданы.</span><span class="sxs-lookup"><span data-stu-id="eca80-366">Open the database in SSOX as you did earlier, and expand the **Tables** node to see that all of the tables have been created.</span></span> <span data-ttu-id="eca80-367">(Если по-прежнему имеется SSOX открыть в более ранних времени, щелкните "Обновить".)</span><span class="sxs-lookup"><span data-stu-id="eca80-367">(If you still have SSOX open from the earlier time, click the Refresh button.)</span></span>

![Таблицы в SSOX](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="eca80-369">Запустите приложение для запуска инициализаторов код, который заполняет базу данных.</span><span class="sxs-lookup"><span data-stu-id="eca80-369">Run the application to trigger the initializer code that seeds the database.</span></span>

<span data-ttu-id="eca80-370">Щелкните правой кнопкой мыши **CourseAssignment** таблицы и выберите **данные представления** для убедитесь в наличии данных в ней.</span><span class="sxs-lookup"><span data-stu-id="eca80-370">Right-click the **CourseAssignment** table and select **View Data** to verify that it has data in it.</span></span>

![Данные CourseAssignment в SSOX](complex-data-model/_static/ssox-ci-data.png)

## <a name="summary"></a><span data-ttu-id="eca80-372">Сводка</span><span class="sxs-lookup"><span data-stu-id="eca80-372">Summary</span></span>

<span data-ttu-id="eca80-373">Теперь у вас есть более сложные модели данных и соответствующей базой данных.</span><span class="sxs-lookup"><span data-stu-id="eca80-373">You now have a more complex data model and corresponding database.</span></span> <span data-ttu-id="eca80-374">В этом руководстве вы узнаете о более подробную информацию для доступа к взаимосвязанных данных.</span><span class="sxs-lookup"><span data-stu-id="eca80-374">In the following tutorial, you'll learn more about how to access related data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="eca80-375">[Назад](migrations.md)
[Вперед](read-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="eca80-375">[Previous](migrations.md)
[Next](read-related-data.md)</span></span>  
