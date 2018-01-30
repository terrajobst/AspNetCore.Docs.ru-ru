---
title: "Страниц Razor с основными EF - модели данных — 5 8."
author: rick-anderson
description: "В этом учебнике добавляйте дополнительные сущности и связи и настроить модель данных, указав форматирование, проверки и правила сопоставления базы данных."
manager: wpickett
ms.author: riande
ms.date: 10/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: 58bb773ba16314827da84909def05a8ef370479b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="creating-a-complex-data-model---ef-core-with-razor-pages-tutorial-5-of-8"></a><span data-ttu-id="d1b4e-103">Создание сложных данных модели - Core EF учебнику страниц Razor (5 8)</span><span class="sxs-lookup"><span data-stu-id="d1b4e-103">Creating a complex data model - EF Core with Razor Pages tutorial (5 of 8)</span></span>

<span data-ttu-id="d1b4e-104">По [Tom Dykstra](https://github.com/tdykstra) и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d1b4e-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="d1b4e-105">Предыдущих учебниках работал с моделью данных, созданный из трех сущностей.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-105">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="d1b4e-106">В этом учебнике:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-106">In this tutorial:</span></span>

* <span data-ttu-id="d1b4e-107">Добавляются дополнительные сущности и связи.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-107">More entities and relationships are added.</span></span>
* <span data-ttu-id="d1b4e-108">Модель данных настраивается путем указания форматирования, проверки и правила сопоставления базы данных.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-108">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="d1b4e-109">Классы сущностей для завершенной модели показан на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-109">The entity classes for the completed data model is shown in the following illustration:</span></span>

![Схема Entity](complex-data-model/_static/diagram.png)

<span data-ttu-id="d1b4e-111">Если возникли проблемы, не удается устранить, загрузите [завершенное приложение для данного этапа](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).</span><span class="sxs-lookup"><span data-stu-id="d1b4e-111">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).</span></span>

## <a name="customize-the-data-model-with-attributes"></a><span data-ttu-id="d1b4e-112">Настройка модели данных с атрибутами</span><span class="sxs-lookup"><span data-stu-id="d1b4e-112">Customize the data model with attributes</span></span>

<span data-ttu-id="d1b4e-113">В этом разделе модели данных настраивается с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-113">In this section, the data model is customized using attributes.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="d1b4e-114">Атрибут типа данных</span><span class="sxs-lookup"><span data-stu-id="d1b4e-114">The DataType attribute</span></span>

<span data-ttu-id="d1b4e-115">Время страницы студента в настоящее время регистрации даты.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-115">The student pages currently displays the time of the enrollment date.</span></span> <span data-ttu-id="d1b4e-116">Как правило поля даты для отображения только даты и не времени.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-116">Typically, date fields show only the date and not the time.</span></span>

<span data-ttu-id="d1b4e-117">Обновление *Models/Student.cs* на следующий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-117">Update *Models/Student.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="d1b4e-118">[DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) атрибут задает тип данных, который является более точным определением, чем встроенный тип базы данных.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-118">The [DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="d1b4e-119">В этом случае следует отображать только дата не даты и времени.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-119">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="d1b4e-120">[Перечисление DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) предоставляет для многих типов данных, таких как дата, время, PhoneNumber, валюты, EmailAddress, и т. д. `DataType` Атрибута также позволяет приложению автоматически предоставлять функции для определенного типа.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-120">The [DataType Enumeration](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="d1b4e-121">Пример:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-121">For example:</span></span>

* <span data-ttu-id="d1b4e-122">`mailto:` Ссылка создается автоматически для `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-122">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="d1b4e-123">Средство выбора даты предоставляется для `DataType.Date` в большинстве браузеров.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-123">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="d1b4e-124">`DataType` Атрибут выдает HTML 5 `data-` (произносится данных тире) атрибутов, которые потребляют браузеров HTML 5.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-124">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="d1b4e-125">`DataType` Атрибуты не обеспечивают проверку.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-125">The `DataType` attributes don't provide validation.</span></span>

<span data-ttu-id="d1b4e-126">`DataType.Date`не указывает формат отображения даты.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-126">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="d1b4e-127">По умолчанию, поле «Дата» отображается согласно форматы по умолчанию на сервере [CultureInfo](https://docs.microsoft.com/aspnet/core/fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span><span class="sxs-lookup"><span data-stu-id="d1b4e-127">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](https://docs.microsoft.com/aspnet/core/fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="d1b4e-128">С помощью атрибута `DisplayFormat` можно явно указать формат даты:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-128">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="d1b4e-129">`ApplyFormatInEditMode` Параметр указывает, что форматирование должен также применяться к изменения пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-129">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="d1b4e-130">Некоторые поля не следует использовать `ApplyFormatInEditMode`.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-130">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="d1b4e-131">Например символ валюты обычно не должен отображаться в поле ввода текста.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-131">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="d1b4e-132">`DisplayFormat` Атрибут может использоваться сама по себе.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-132">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="d1b4e-133">Обычно рекомендуется использовать `DataType` атрибутом `DisplayFormat` атрибута.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-133">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="d1b4e-134">`DataType` Атрибут передает семантику данных, а не как отображать его на экране.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-134">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="d1b4e-135">`DataType` Атрибут предоставляет следующие преимущества, которые недоступны в `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-135">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="d1b4e-136">Браузер может включить функции HTML5.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-136">The browser can enable HTML5 features.</span></span> <span data-ttu-id="d1b4e-137">Например отображение элемента управления календаря, локализованными обозначение денежной единицы, ссылки электронной почты, входной проверки на стороне клиента, и т. д.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-137">For example, show a calendar control, the locale-appropriate currency symbol, email links, client-side input validation, etc.</span></span>
* <span data-ttu-id="d1b4e-138">По умолчанию браузер отображает данные, используя правильный формат на основе языкового стандарта.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-138">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="d1b4e-139">Дополнительные сведения см. в разделе [ \<ввода > Вспомогательные тега документации](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="d1b4e-139">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

<span data-ttu-id="d1b4e-140">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-140">Run the app.</span></span> <span data-ttu-id="d1b4e-141">Перейдите на страницу индекса учащихся.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-141">Navigate to the Students Index page.</span></span> <span data-ttu-id="d1b4e-142">Время больше не отображается.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-142">Times are no longer displayed.</span></span> <span data-ttu-id="d1b4e-143">Каждый представление, использующее `Student` модели отображается дата без времени.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-143">Every view that uses the `Student` model displays the date without time.</span></span>

![Отображение даты без времени страницы индекса студентов](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="d1b4e-145">Атрибут StringLength</span><span class="sxs-lookup"><span data-stu-id="d1b4e-145">The StringLength attribute</span></span>

<span data-ttu-id="d1b4e-146">С помощью атрибутов можно указать правила проверки данных и сообщений об ошибках проверки.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-146">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="d1b4e-147">[StringLength](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) атрибут указывает минимальную и максимальную длину символов, разрешенных в поле данных.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-147">The [StringLength](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="d1b4e-148">`StringLength` Атрибута также предоставляет проверки на стороне клиента и на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-148">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="d1b4e-149">Минимальное значение не оказывает влияния на схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-149">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="d1b4e-150">Обновление `Student` модели с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-150">Update the `Student` model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="d1b4e-151">Предыдущий код допускается использование не более 50 символов.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-151">The preceding code limits names to no more than 50 characters.</span></span> <span data-ttu-id="d1b4e-152">`StringLength` Атрибута не запрещает ввод символы-разделители для имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-152">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="d1b4e-153">[Регулярное выражение](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) атрибут используется для применения ограничений входных данных.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-153">The [RegularExpression](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute is used to apply restrictions to the input.</span></span> <span data-ttu-id="d1b4e-154">Например следующий код требует первого символа в записываются прописными буквами и остальные символы преобразуются в алфавитном порядке:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-154">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

<span data-ttu-id="d1b4e-155">Запуск приложения:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-155">Run the app:</span></span>

* <span data-ttu-id="d1b4e-156">Перейдите на страницу учащихся.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-156">Navigate to the Students page.</span></span>
* <span data-ttu-id="d1b4e-157">Выберите **создать новый**и введите имя длиной более 50 символов.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-157">Select **Create New**, and enter a name longer than 50 characters.</span></span>
* <span data-ttu-id="d1b4e-158">Выберите **создать**, проверка на стороне клиента показано сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-158">Select **Create**, client-side validation shows an error message.</span></span>

![Студенты индексная страница, отображающий строку ошибки длины](complex-data-model/_static/string-length-errors.png)

<span data-ttu-id="d1b4e-160">В **обозреватель объектов SQL Server** (SSOX), откройте в конструкторе таблиц учащихся, дважды щелкнув **студента** таблицы.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-160">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Таблица учащихся в SSOX до миграции](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="d1b4e-162">На предыдущем рисунке показана схемы для `Student` таблицы.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-162">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="d1b4e-163">Имя поля имеют тип `nvarchar(MAX)` , так как миграция не была запущена на базу данных.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-163">The name fields have type `nvarchar(MAX)` because migrations has not been run on the DB.</span></span> <span data-ttu-id="d1b4e-164">При выполнении миграции далее в этом учебнике, имя поля становятся `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-164">When migrations are run later in this tutorial, the name fields become `nvarchar(50)`.</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="d1b4e-165">Атрибут столбца</span><span class="sxs-lookup"><span data-stu-id="d1b4e-165">The Column attribute</span></span>

<span data-ttu-id="d1b4e-166">Атрибуты можно управлять как классы и свойства сопоставляются в базу данных.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-166">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="d1b4e-167">В этом разделе `Column` атрибут используется для сопоставления имени `FirstMidName` свойства «FirstName» в базе данных.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-167">In this section, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the DB.</span></span>

<span data-ttu-id="d1b4e-168">При создании базы данных, имена свойств в модели используются для имен столбцов (только если `Column` используется атрибут).</span><span class="sxs-lookup"><span data-stu-id="d1b4e-168">When the DB is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span>

<span data-ttu-id="d1b4e-169">`Student` Модель использует `FirstMidName` для имени первого поля, так как поле также может содержать отчество.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-169">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="d1b4e-170">Обновление *Student.cs* файл с следующий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-170">Update the *Student.cs* file with the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="d1b4e-171">С помощью предыдущего изменения `Student.FirstMidName` в приложении сопоставляется `FirstName` столбец `Student` таблицы.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-171">With the preceding change, `Student.FirstMidName` in the app maps to the `FirstName` column of the `Student` table.</span></span>

<span data-ttu-id="d1b4e-172">Добавление `Column` атрибут изменяет модель `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-172">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="d1b4e-173">Модель `SchoolContext` больше не соответствует базе данных.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-173">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="d1b4e-174">Если приложение запускается перед применением миграций, создается следующее исключение:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-174">If the app is run before applying migrations, the following exception is generated:</span></span>

```SQL
SqlException: Invalid column name 'FirstName'.
```
<span data-ttu-id="d1b4e-175">Обновление базы данных:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-175">To update the DB:</span></span>

* <span data-ttu-id="d1b4e-176">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-176">Build the project.</span></span>
* <span data-ttu-id="d1b4e-177">Откройте окно командной строки в папке проекта.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-177">Open a command window in the project folder.</span></span> <span data-ttu-id="d1b4e-178">Введите следующие команды для создания новой миграции и обновления базы данных:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-178">Enter the following commands to create a new migration and update the DB:</span></span>

    ```console
    dotnet ef migrations add ColumnFirstName
    dotnet ef database update
    ```

<span data-ttu-id="d1b4e-179">`dotnet ef migrations add ColumnFirstName` Команда создает сообщение об ошибке:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-179">The `dotnet ef migrations add ColumnFirstName` command generates the following warning message:</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

<span data-ttu-id="d1b4e-180">Создается предупреждение, так как имя поля, становятся более 50 символов.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-180">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="d1b4e-181">Если имя в базе данных имеет более 50 символов, 51 до последнего символа будет потеряна.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-181">If a name in the DB had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="d1b4e-182">Тестирование приложения.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-182">Test the app.</span></span>

<span data-ttu-id="d1b4e-183">Откройте таблицу студента в SSOX:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-183">Open the Student table in SSOX:</span></span>

![Студенты таблицы в SSOX после миграции](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="d1b4e-185">До применения миграции, имя столбцы были типа [nvarchar(MAX)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="d1b4e-185">Before migration was applied, the name columns were of type [nvarchar(MAX)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="d1b4e-186">Столбцы имен стали `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-186">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="d1b4e-187">Имя столбца отличается от `FirstMidName` для `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-187">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

> [!Note]
> <span data-ttu-id="d1b4e-188">В следующем разделе сборка приложения на некоторых этапах приводит к возникновению ошибки компилятора.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-188">In the following section, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="d1b4e-189">Укажите инструкции, когда для сборки приложения.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-189">The instructions specify when to build the app.</span></span>

## <a name="student-entity-update"></a><span data-ttu-id="d1b4e-190">Обновление сущности студента</span><span class="sxs-lookup"><span data-stu-id="d1b4e-190">Student entity update</span></span>

![Сущность Student](complex-data-model/_static/student-entity.png)

<span data-ttu-id="d1b4e-192">Обновление *Models/Student.cs* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-192">Update *Models/Student.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="d1b4e-193">Обязательный атрибут</span><span class="sxs-lookup"><span data-stu-id="d1b4e-193">The Required attribute</span></span>

<span data-ttu-id="d1b4e-194">`Required` Атрибут делает обязательные поля имя свойства.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-194">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="d1b4e-195">`Required` Атрибут не нужен для запретом типов, таких как типы значений (`DateTime`, `int`, `double`и т. д.).</span><span class="sxs-lookup"><span data-stu-id="d1b4e-195">The `Required` attribute isn't needed for non-nullable types such as value types (`DateTime`, `int`, `double`, etc.).</span></span> <span data-ttu-id="d1b4e-196">Типы, которые не может быть неопределенным автоматически обрабатываются как обязательные поля.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-196">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="d1b4e-197">`Required` Атрибута может быть заменено минимальную длину параметра в `StringLength` атрибута:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-197">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="d1b4e-198">Атрибут отображения</span><span class="sxs-lookup"><span data-stu-id="d1b4e-198">The Display attribute</span></span>

<span data-ttu-id="d1b4e-199">`Display` Атрибут указывает заголовок для текстовых полей таблицу «Имя», «Last Name», «Полное имя» и «Дата регистрации».</span><span class="sxs-lookup"><span data-stu-id="d1b4e-199">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="d1b4e-200">По умолчанию заголовки размером не разделение слов, например «Фамилия».</span><span class="sxs-lookup"><span data-stu-id="d1b4e-200">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="d1b4e-201">Свойство FullName вычисляется</span><span class="sxs-lookup"><span data-stu-id="d1b4e-201">The FullName calculated property</span></span>

<span data-ttu-id="d1b4e-202">`FullName`— Вычисляемое свойство, которое возвращает значение, которое создается путем объединения двух свойств.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-202">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="d1b4e-203">`FullName`не может быть задано, оно имеет только метод доступа get.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-203">`FullName` cannot be set, it has only a get accessor.</span></span> <span data-ttu-id="d1b4e-204">Не `FullName` столбец создается в базе данных.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-204">No `FullName` column is created in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="d1b4e-205">Создать сущность инструктора</span><span class="sxs-lookup"><span data-stu-id="d1b4e-205">Create the Instructor Entity</span></span>

![Сущность инструктора](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="d1b4e-207">Создание *Models/Instructor.cs* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-207">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

<span data-ttu-id="d1b4e-208">Обратите внимание, что некоторые свойства в `Student` и `Instructor` сущности.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-208">Notice that several properties are the same in the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="d1b4e-209">В этом учебнике реализация наследования далее в этой серии этот код оптимизирован во избежание избыточность.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-209">In the Implementing Inheritance tutorial later in this series, this code is refactored to eliminate the redundancy.</span></span>

<span data-ttu-id="d1b4e-210">Несколько атрибутов может быть на одной строке.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-210">Multiple attributes can be on one line.</span></span> <span data-ttu-id="d1b4e-211">`HireDate` Атрибутов может быть записан следующим образом:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-211">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="d1b4e-212">Свойства навигации CourseAssignments и OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="d1b4e-212">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="d1b4e-213">`CourseAssignments` И `OfficeAssignment` свойства — это свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-213">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="d1b4e-214">Инструктор можно обучить любое количество курсов, поэтому `CourseAssignments` определен как коллекция.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-214">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="d1b4e-215">Если свойство навигации содержит несколько сущностей:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-215">If a navigation property holds multiple entities:</span></span>

* <span data-ttu-id="d1b4e-216">Он должен быть типом списка, где записи может быть добавленные, удаленные и обновлены.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-216">It must be a list type where the entries can be added, deleted, and updated.</span></span>

<span data-ttu-id="d1b4e-217">Следующие типы свойств навигации.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-217">Navigation property types include:</span></span>

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

<span data-ttu-id="d1b4e-218">Если `ICollection<T>` указан, создает EF Core `HashSet<T>` коллекции по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-218">If `ICollection<T>` is specified, EF Core creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="d1b4e-219">`CourseAssignment` Сущности см. в разделе на связи, многие ко многим.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-219">The `CourseAssignment` entity is explained in the section on many-to-many relationships.</span></span>

<span data-ttu-id="d1b4e-220">Contoso университета бизнес-правил, инструктор может иметь не более одного офиса состояния.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-220">Contoso University business rules state that an instructor can have at most one office.</span></span> <span data-ttu-id="d1b4e-221">`OfficeAssignment` Свойство содержит один `OfficeAssignment` сущности.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-221">The `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="d1b4e-222">`OfficeAssignment`имеет значение null, если office не назначено.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-222">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="d1b4e-223">Создать сущность OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="d1b4e-223">Create the OfficeAssignment entity</span></span>

![OfficeAssignment сущности](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="d1b4e-225">Создание *Models/OfficeAssignment.cs* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-225">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="d1b4e-226">Ключевой атрибут</span><span class="sxs-lookup"><span data-stu-id="d1b4e-226">The Key attribute</span></span>

<span data-ttu-id="d1b4e-227">`[Key]` Атрибут используется для идентификации свойства как первичный ключ (PK) при имя свойства стоит Кроме classnameID или идентификатор.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-227">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="d1b4e-228">Имеется отношение "один к нулю или одному" между `Instructor` и `OfficeAssignment` сущности.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-228">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="d1b4e-229">Назначение office существует только относительно инструктора, которую он назначен.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-229">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="d1b4e-230">`OfficeAssignment` Первичного ключа является также его внешний ключ (FK) `Instructor` сущности.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-230">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span> <span data-ttu-id="d1b4e-231">Ядро EF не распознает автоматически `InstructorID` как PK из `OfficeAssignment` из-за:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-231">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because:</span></span>

* <span data-ttu-id="d1b4e-232">`InstructorID`не Следуйте соглашению об именах идентификатор или classnameID.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-232">`InstructorID` doesn't follow the ID or classnameID naming convention.</span></span>

<span data-ttu-id="d1b4e-233">Таким образом `Key` атрибут используется для идентификации `InstructorID` как первичного ключа:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-233">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="d1b4e-234">По умолчанию EF Core считает ключ без формирования базы данных, так как столбец используется идентифицирующего отношения.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-234">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="d1b4e-235">Свойство навигации инструктора</span><span class="sxs-lookup"><span data-stu-id="d1b4e-235">The Instructor navigation property</span></span>

<span data-ttu-id="d1b4e-236">`OfficeAssignment` Свойства навигации для `Instructor` сущности допускает значения NULL из-за:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-236">The `OfficeAssignment` navigation property for the `Instructor` entity is nullable because:</span></span>

* <span data-ttu-id="d1b4e-237">Ссылочных типов (такие, как классы, допускающие значение NULL).</span><span class="sxs-lookup"><span data-stu-id="d1b4e-237">Reference types (such as classes are nullable).</span></span>
* <span data-ttu-id="d1b4e-238">Инструктор не может иметь назначение office.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-238">An instructor might not have an office assignment.</span></span>


<span data-ttu-id="d1b4e-239">`OfficeAssignment` Сущность имеет не допускающий `Instructor` свойства навигации из-за:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-239">The `OfficeAssignment` entity has a non-nullable `Instructor` navigation property because:</span></span>

* <span data-ttu-id="d1b4e-240">`InstructorID`является запретом.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-240">`InstructorID` is non-nullable.</span></span>
* <span data-ttu-id="d1b4e-241">Назначение office не может существовать без инструктор.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-241">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="d1b4e-242">Когда `Instructor` сущность имеет связанный с ним `OfficeAssignment` сущностей, каждая сущность содержит ссылку на другую переменную его свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-242">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="d1b4e-243">`[Required]` Атрибут может применяться к `Instructor` свойство навигации:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-243">The `[Required]` attribute could be applied to the `Instructor` navigation property:</span></span>

```csharp
[Required]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="d1b4e-244">Предыдущий код указывает, что должен быть связанные инструктора.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-244">The preceding code specifies that there must be a related instructor.</span></span> <span data-ttu-id="d1b4e-245">Приведенный выше код не требуется из-за `InstructorID` внешний ключ (который также является PK) допускают значение NULL.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-245">The preceding code is unnecessary because the `InstructorID` foreign key (which is also the PK) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="d1b4e-246">Изменение сущности курса</span><span class="sxs-lookup"><span data-stu-id="d1b4e-246">Modify the Course Entity</span></span>

![Сущности курса](complex-data-model/_static/course-entity.png)

<span data-ttu-id="d1b4e-248">Обновление *Models/Course.cs* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-248">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="d1b4e-249">`Course` Сущность имеет внешний ключ (FK) свойство `DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-249">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="d1b4e-250">`DepartmentID`Указывает на связанный `Department` сущности.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-250">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="d1b4e-251">`Course` Сущность имеет `Department` свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-251">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="d1b4e-252">EF Core не требует свойства внешнего ключа для модели данных, если модель имеет свойство навигации для связанной сущности.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-252">EF Core doesn't require a FK property for a data model when the model has a navigation property for a related entity.</span></span>

<span data-ttu-id="d1b4e-253">Везде, где они понадобятся FKs автоматически создает EF ядра базы данных.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-253">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="d1b4e-254">Создает основной EF [затемнять свойства](https://docs.microsoft.com/ef/core/modeling/shadow-properties) для автоматически созданных FKs.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-254">EF Core creates [shadow properties](https://docs.microsoft.com/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="d1b4e-255">Наличие внешнего ключа в модели данных могут выполнять обновления проще и эффективнее.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-255">Having the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="d1b4e-256">Например, рассмотрим модель где свойства внешнего ключа `DepartmentID` — *не* включены.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-256">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="d1b4e-257">При выборке сущности курса для изменения:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-257">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="d1b4e-258">`Department` Сущности имеет значение null, если он явно не загружен.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-258">The `Department` entity is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="d1b4e-259">Для обновления сущности курса `Department` сущности должно быть выбрано.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-259">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="d1b4e-260">Если свойство внешнего ключа `DepartmentID` включено в модели данных, нет необходимости выбирать `Department` сущности перед обновлением.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-260">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="d1b4e-261">Атрибут DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="d1b4e-261">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="d1b4e-262">`[DatabaseGenerated(DatabaseGeneratedOption.None)]` Атрибут указывает, что первичного ключа является предоставляемый приложением, а не созданное базой данных.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-262">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="d1b4e-263">По умолчанию EF Core предполагается генерировать значений первичного ключа в базе данных.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-263">By default, EF Core assumes that PK values are generated by the DB.</span></span> <span data-ttu-id="d1b4e-264">DB созданный PK значений обычно является лучшим подходом.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-264">DB generated PK values is generally the best approach.</span></span> <span data-ttu-id="d1b4e-265">Для `Course` сущностей, пользователь указывает первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-265">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="d1b4e-266">Например курс число, например 1000 серии для отдела математические и 2000 для английского языка отдела.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-266">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="d1b4e-267">`DatabaseGenerated` Атрибут также может использоваться для создания значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-267">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="d1b4e-268">Например базы данных может автоматически создавать поля даты для записи даты строки был создан или обновлен.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-268">For example, the DB can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="d1b4e-269">Дополнительные сведения см. в разделе [созданные свойства](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="d1b4e-269">For more information, see [Generated Properties](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="d1b4e-270">Свойства внешнего ключа и навигации</span><span class="sxs-lookup"><span data-stu-id="d1b4e-270">Foreign key and navigation properties</span></span>

<span data-ttu-id="d1b4e-271">Внешний ключ (FK) свойства и свойства навигации в `Course` сущности отражают следующие связи:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-271">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="d1b4e-272">Курс назначается одного подразделения, то есть `DepartmentID` внешнего ключа и `Department` свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-272">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="d1b4e-273">Курс может иметь любое количество студентов, зарегистрированных в нем, так что `Enrollments` свойство навигации — это коллекция:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-273">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="d1b4e-274">Может обучения по нескольким инструкторов, поэтому `CourseAssignments` свойство навигации — это коллекция:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-274">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="d1b4e-275">`CourseAssignment`объясняется [позже](#many-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="d1b4e-275">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="create-the-department-entity"></a><span data-ttu-id="d1b4e-276">Создание сущности «отдел»</span><span class="sxs-lookup"><span data-stu-id="d1b4e-276">Create the Department entity</span></span>

![Сущность Department](complex-data-model/_static/department-entity.png)

<span data-ttu-id="d1b4e-278">Создание *Models/Department.cs* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-278">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="d1b4e-279">Атрибут столбца</span><span class="sxs-lookup"><span data-stu-id="d1b4e-279">The Column attribute</span></span>

<span data-ttu-id="d1b4e-280">Ранее `Column` атрибут был применен, чтобы изменить сопоставление имени столбца.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-280">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="d1b4e-281">В коде `Department` сущности, `Column` атрибута можно изменить сопоставление типов данных SQL.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-281">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="d1b4e-282">`Budget` Столбца определяется с помощью типа money SQL Server в базе данных:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-282">The `Budget` column is defined using the SQL Server money type in the DB:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="d1b4e-283">Сопоставление столбцов обычно не является обязательным.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-283">Column mapping is generally not required.</span></span> <span data-ttu-id="d1b4e-284">EF Core обычно выбирает соответствующий тип данных SQL Server, на основе типа CLR для свойства.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-284">EF Core generally chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="d1b4e-285">Среда CLR `decimal` сопоставляется SQL Server с типом `decimal` типа.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-285">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="d1b4e-286">`Budget`для денежных единиц и тип данных money больше подходит для валюты.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-286">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="d1b4e-287">Свойства внешнего ключа и навигации</span><span class="sxs-lookup"><span data-stu-id="d1b4e-287">Foreign key and navigation properties</span></span>

<span data-ttu-id="d1b4e-288">Свойства внешнего ключа и навигации отражают следующие связи:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-288">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="d1b4e-289">Отдел может или не может иметь администратора.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-289">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="d1b4e-290">Администратор всегда является инструктор.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-290">An administrator is always an instructor.</span></span> <span data-ttu-id="d1b4e-291">Поэтому `InstructorID` свойство включается в качестве внешнего ключа для `Instructor` сущности.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-291">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="d1b4e-292">Свойство навигации называется `Administrator` , но содержит `Instructor` сущности:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-292">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="d1b4e-293">Вопросительный знак (?) в приведенном выше коде указывает, что свойство имеет значение NULL.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-293">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="d1b4e-294">Отдел может иметь несколько курсов, то есть свойство навигации курсы:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-294">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="d1b4e-295">Примечание: По соглашению EF Core позволяет каскадное удаление для запретом FKs и многие ко многим связи.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-295">Note: By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="d1b4e-296">Каскадное удаление может привести к циклической каскадное удаление правил.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-296">Cascading delete can result in circular cascade delete rules.</span></span> <span data-ttu-id="d1b4e-297">Циклическая каскадного удаления правила вызывает исключение при добавлении миграции.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-297">Circular cascade delete rules causes an exception when a migration is added.</span></span>

<span data-ttu-id="d1b4e-298">Например если `Department.InstructorID` свойство не было определено как допускающая значение NULL:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-298">For example, if the `Department.InstructorID` property wasn't defined as nullable:</span></span>

* <span data-ttu-id="d1b4e-299">EF Core настраивает правила cascade delete для удаления инструктора при удалении подразделения.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-299">EF Core configures a cascade delete rule to delete the instructor when the department is deleted.</span></span>
* <span data-ttu-id="d1b4e-300">Удаление инструктора при удалении отделе режим не используется.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-300">Deleting the instructor when the department is deleted isn't the intended behavior.</span></span>

<span data-ttu-id="d1b4e-301">При необходимости бизнес-правила `InstructorID` свойства быть не значение NULL, используйте следующую инструкцию fluent API:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-301">If business rules required the `InstructorID` property be non-nullable, use the following fluent API statement:</span></span>

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

<span data-ttu-id="d1b4e-302">Предыдущий код отключает каскадное удаление в отношении инструктора отдела.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-302">The preceding code disables cascade delete on the department-instructor relationship.</span></span>

## <a name="update-the-enrollment-entity"></a><span data-ttu-id="d1b4e-303">Обновление сущности регистрации</span><span class="sxs-lookup"><span data-stu-id="d1b4e-303">Update the Enrollment entity</span></span>

<span data-ttu-id="d1b4e-304">Запись регистрации — для одного курса, выполняемое один студент.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-304">An enrollment record is for a one course taken by one student.</span></span>

![Сущности регистрации](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="d1b4e-306">Обновление *Models/Enrollment.cs* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-306">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="d1b4e-307">Свойства внешнего ключа и навигации</span><span class="sxs-lookup"><span data-stu-id="d1b4e-307">Foreign key and navigation properties</span></span>

<span data-ttu-id="d1b4e-308">Свойства внешнего ключа и свойства навигации отражают следующие связи:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-308">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="d1b4e-309">Для одного курса является запись регистрации, поэтому `CourseID` свойства внешнего ключа и `Course` свойство навигации:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-309">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="d1b4e-310">Для один студент является запись регистрации, поэтому `StudentID` свойства внешнего ключа и `Student` свойство навигации:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-310">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="d1b4e-311">Многие ко многим</span><span class="sxs-lookup"><span data-stu-id="d1b4e-311">Many-to-Many Relationships</span></span>

<span data-ttu-id="d1b4e-312">Имеется отношение "многие ко многим" между `Student` и `Course` сущности.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-312">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="d1b4e-313">`Enrollment` Сущности функционирует как многие ко многим Соединяемая таблица *с полезными данными* в базе данных.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-313">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="d1b4e-314">«С полезными данными» означает, что `Enrollment` таблица содержит дополнительные данные помимо FKs для соединяемых таблиц (в данном случае PK и `Grade`).</span><span class="sxs-lookup"><span data-stu-id="d1b4e-314">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="d1b4e-315">Ниже показано, как будут выглядеть эти связи в схеме сущности.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-315">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="d1b4e-316">(Эта схема была создана с помощью EF Power Tools для EF 6.x.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-316">(This diagram was generated using EF Power Tools for EF 6.x.</span></span> <span data-ttu-id="d1b4e-317">Создание схемы не является частью учебника.)</span><span class="sxs-lookup"><span data-stu-id="d1b4e-317">Creating the diagram isn't part of the tutorial.)</span></span>

![Курс студента много множественных связей](complex-data-model/_static/student-course.png)

<span data-ttu-id="d1b4e-319">Каждая линия связи имеет 1 на одном конце и звездочку (\*) в другом, указывающее один ко многим.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-319">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="d1b4e-320">Если `Enrollment` таблицы не были включены сведения об оценках, его потребуется только содержат два FKs (`CourseID` и `StudentID`).</span><span class="sxs-lookup"><span data-stu-id="d1b4e-320">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="d1b4e-321">Многие ко многим Соединяемая таблица без полезных данных иногда называют таблицы присоединения (ЧИСТАЯ).</span><span class="sxs-lookup"><span data-stu-id="d1b4e-321">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="d1b4e-322">`Instructor` И `Course` сущности имеют связь «многие ко многим» с помощью таблицы присоединения.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-322">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="d1b4e-323">Примечание: Неявное соединение таблиц для связи многие ко многим, но основные EF не поддерживает 6.x EF.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-323">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="d1b4e-324">Дополнительные сведения см. в разделе [многие ко многим связей в версии 2.0 основные EF](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span><span class="sxs-lookup"><span data-stu-id="d1b4e-324">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="d1b4e-325">Сущность CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="d1b4e-325">The CourseAssignment entity</span></span>

![CourseAssignment сущности](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="d1b4e-327">Создание *Models/CourseAssignment.cs* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-327">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a><span data-ttu-id="d1b4e-328">Инструктора курсов</span><span class="sxs-lookup"><span data-stu-id="d1b4e-328">Instructor-to-Courses</span></span>

![M:M инструктора курсов](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="d1b4e-330">Связь многие ко многим инструктора-курсы:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-330">The Instructor-to-Courses many-to-many relationship:</span></span>

* <span data-ttu-id="d1b4e-331">Требуется соединяемой таблице, который должен быть представлен набор сущностей.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-331">Requires a join table that must be represented by an entity set.</span></span>
* <span data-ttu-id="d1b4e-332">Является таблицы присоединения (таблицы без полезных данных).</span><span class="sxs-lookup"><span data-stu-id="d1b4e-332">Is a pure join table (table without payload).</span></span>

<span data-ttu-id="d1b4e-333">Обычно имя сущности объединения `EntityName1EntityName2`.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-333">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="d1b4e-334">Например, инструктора-курсы соединенной таблице с помощью этого шаблона является `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-334">For example, the Instructor-to-Courses join table using this pattern is `CourseInstructor`.</span></span> <span data-ttu-id="d1b4e-335">Тем не менее рекомендуется использовать имя, которое описывает связь.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-335">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="d1b4e-336">Модели создаются простой и увеличиваться.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-336">Data models start out simple and grow.</span></span> <span data-ttu-id="d1b4e-337">Для включения полезных данных часто изменяться полезных данных нет соединения (PJTs).</span><span class="sxs-lookup"><span data-stu-id="d1b4e-337">No-payload joins (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="d1b4e-338">С помощью сущности описательное имя, имя не нужно изменять при изменении таблицы соединения.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-338">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="d1b4e-339">В идеальном случае сущности объединения бы естественным имени (возможно одно слово) в бизнес-среде.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-339">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="d1b4e-340">Например книги и клиенты могут быть связаны с соединения сущности, называемой оценок.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-340">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="d1b4e-341">В отношении многие ко многим инструктора-курсы `CourseAssignment` предпочтительнее, чем `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-341">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="d1b4e-342">Составной ключ</span><span class="sxs-lookup"><span data-stu-id="d1b4e-342">Composite key</span></span>

<span data-ttu-id="d1b4e-343">FKs не допускают значения NULL.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-343">FKs are not nullable.</span></span> <span data-ttu-id="d1b4e-344">Два FKs в `CourseAssignment` (`InstructorID` и `CourseID`) совместно однозначно определяют каждую строку `CourseAssignment` таблицы.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-344">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="d1b4e-345">`CourseAssignment`не требует выделенной системы.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-345">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="d1b4e-346">`InstructorID` И `CourseID` свойства функционировать как составной первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-346">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="d1b4e-347">— Единственный способ для указания составных первичных EF Core с *fluent API*.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-347">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="d1b4e-348">В следующем разделе показано, как настроить составной первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-348">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="d1b4e-349">Обеспечивает составного ключа.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-349">The composite key ensures:</span></span>

* <span data-ttu-id="d1b4e-350">Для одного курса допускается несколько строк.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-350">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="d1b4e-351">Для одного инструктора допускается несколько строк.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-351">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="d1b4e-352">Несколько строк для одной и той же инструктора и курса не является допустимым.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-352">Multiple rows for the same instructor and course isn't allowed.</span></span>

<span data-ttu-id="d1b4e-353">`Enrollment` Сущности объединения определяет собственный первичного ключа, возможны повторяющиеся значения такого рода.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-353">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="d1b4e-354">Для предотвращения таких повторяющиеся значения:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-354">To prevent such duplicates:</span></span>

* <span data-ttu-id="d1b4e-355">Добавьте уникальный индекс по полям внешнего ключа, или</span><span class="sxs-lookup"><span data-stu-id="d1b4e-355">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="d1b4e-356">Настройка `Enrollment` с основной составного ключа, аналогично `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-356">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="d1b4e-357">Дополнительные сведения см. в разделе [индексы](https://docs.microsoft.com/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="d1b4e-357">For more information, see [Indexes](https://docs.microsoft.com/ef/core/modeling/indexes).</span></span>

## <a name="update-the-db-context"></a><span data-ttu-id="d1b4e-358">Обновить контекст базы данных</span><span class="sxs-lookup"><span data-stu-id="d1b4e-358">Update the DB context</span></span>

<span data-ttu-id="d1b4e-359">Добавьте следующий выделенный код в *Data/SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-359">Add the following highlighted code to *Data/SchoolContext.cs*:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="d1b4e-360">Предыдущий код добавляет новые сущности и настраивает `CourseAssignment` сущности составной первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-360">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="d1b4e-361">Fluent API вместо атрибутов</span><span class="sxs-lookup"><span data-stu-id="d1b4e-361">Fluent API alternative to attributes</span></span>

<span data-ttu-id="d1b4e-362">`OnModelCreating` Метод в предыдущем коде используется *fluent API* можно настроить поведение EF Core.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-362">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="d1b4e-363">API-Интерфейс называется «fluent», поскольку часто используемые проводить ряда вызовов метода вместе в одной инструкции.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-363">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="d1b4e-364">[После кода](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) является примером fluent API:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-364">The [following code](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="d1b4e-365">В этом учебнике fluent API используется только для сопоставления базы данных, которое не может выполняться с атрибутами.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-365">In this tutorial, the fluent API is used only for DB mapping that can't be done with attributes.</span></span> <span data-ttu-id="d1b4e-366">Однако fluent API можно указать большинство форматирование, проверки и правила сопоставления, которые можно сделать с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-366">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="d1b4e-367">Некоторые атрибуты, такие как `MinimumLength` не может применяться с помощью плавного API.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-367">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="d1b4e-368">`MinimumLength`не изменяйте схему, он применяется только минимальная длина правила проверки.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-368">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="d1b4e-369">Некоторые разработчики предпочитают использовать fluent API, они могут обновлять свои классы сущностей «очистить».</span><span class="sxs-lookup"><span data-stu-id="d1b4e-369">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="d1b4e-370">Можно одновременно атрибуты и fluent API.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-370">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="d1b4e-371">Существует несколько конфигураций, которые можно будет только с помощью плавного API (с указанием составного первичного ключа).</span><span class="sxs-lookup"><span data-stu-id="d1b4e-371">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="d1b4e-372">Существует несколько конфигураций, которые можно будет только с атрибутами (`MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="d1b4e-372">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="d1b4e-373">Рекомендуется с помощью плавного API или атрибуты:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-373">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="d1b4e-374">Выберите один из этих двух подходов.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-374">Choose one of these two approaches.</span></span>
* <span data-ttu-id="d1b4e-375">Выбранный подход постоянно максимально используйте.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-375">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="d1b4e-376">Некоторые атрибуты, используемые в этом учебнике используются для:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-376">Some of the attributes used in the this tutorial are used for:</span></span>

* <span data-ttu-id="d1b4e-377">Только для проверки (например, `MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="d1b4e-377">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="d1b4e-378">EF основная конфигурация (например, `HasKey`).</span><span class="sxs-lookup"><span data-stu-id="d1b4e-378">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="d1b4e-379">Проверка и EF основные конфигурации (например, `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="d1b4e-379">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="d1b4e-380">Дополнительные сведения об атрибутах и fluent API см. в разделе [методы конфигурации](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span><span class="sxs-lookup"><span data-stu-id="d1b4e-380">For more information about attributes vs. fluent API, see [Methods of configuration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="d1b4e-381">Отображение отношений объектов схемы</span><span class="sxs-lookup"><span data-stu-id="d1b4e-381">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="d1b4e-382">Ниже показана схема, создаваемая EF Power Tools для завершенной модели School.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-382">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Схема Entity](complex-data-model/_static/diagram.png)

<span data-ttu-id="d1b4e-384">На предыдущей схеме показаны:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-384">The preceding diagram shows:</span></span>

* <span data-ttu-id="d1b4e-385">Несколько строк один ко многим (от 1 до \*).</span><span class="sxs-lookup"><span data-stu-id="d1b4e-385">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="d1b4e-386">Линия связи один к нулю или одному (1 к нулю или одному) между `Instructor` и `OfficeAssignment` сущности.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-386">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="d1b4e-387">Линия связи нуль или один ко многим (от 0 до 1 для \*) между `Instructor` и `Department` сущности.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-387">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-db-with-test-data"></a><span data-ttu-id="d1b4e-388">Начальное значение DB тестовыми данными</span><span class="sxs-lookup"><span data-stu-id="d1b4e-388">Seed the DB with Test Data</span></span>

<span data-ttu-id="d1b4e-389">Обновление кода в *Data/DbInitializer.cs*:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-389">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="d1b4e-390">Предыдущий код предоставляет данные начальное значение для новых сущностей.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-390">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="d1b4e-391">Большая часть кода создает новые объекты сущности и загружает данные образца.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-391">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="d1b4e-392">Образец данных используется для проверки.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-392">The sample data is used for testing.</span></span> <span data-ttu-id="d1b4e-393">Приведенный выше код создает следующие связи многие ко многим:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-393">The preceding code creates the following many-to-many relationships:</span></span>

* `Enrollments`
* `CourseAssignment`

<span data-ttu-id="d1b4e-394">Примечание: [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) будет поддерживать [заполнение данными](https://github.com/aspnet/EntityFrameworkCore/issues/629).</span><span class="sxs-lookup"><span data-stu-id="d1b4e-394">Note: [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) will support [data seeding](https://github.com/aspnet/EntityFrameworkCore/issues/629).</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="d1b4e-395">Добавить миграции</span><span class="sxs-lookup"><span data-stu-id="d1b4e-395">Add a migration</span></span>

<span data-ttu-id="d1b4e-396">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-396">Build the project.</span></span> <span data-ttu-id="d1b4e-397">Откройте окно командной строки в папке проекта и введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-397">Open a command window in the project folder and enter the following command:</span></span>

```console
dotnet ef migrations add ComplexDataModel
```

<span data-ttu-id="d1b4e-398">Эта команда отображает предупреждение о возможной потере данных.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-398">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="d1b4e-399">Если `database update` команда выполняется, возникает следующая ошибка:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-399">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

<span data-ttu-id="d1b4e-400">При выполнении миграции с существующими данными, могут существовать ограничения внешнего ключа, которые не выполнены для существующих данных.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-400">When migrations are run with existing data, there may be FK constraints that are not satisfied with the exiting data.</span></span> <span data-ttu-id="d1b4e-401">В этом учебнике создается новый DB, поэтому отсутствие нарушений ограничений внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-401">For this tutorial, a new DB is created, so there are no FK constraint violations.</span></span> <span data-ttu-id="d1b4e-402">В разделе [исправление ограничения внешнего ключа с помощью предыдущих версий данных](#fk) инструкции для устранения нарушения внешнего ключа на текущей базы данных.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-402">See [Fixing foreign key constraints with legacy data](#fk) for instructions on how to fix the FK violations on the current DB.</span></span>

## <a name="change-the-connection-string-and-update-the-db"></a><span data-ttu-id="d1b4e-403">Измените строку подключения и обновления базы данных</span><span class="sxs-lookup"><span data-stu-id="d1b4e-403">Change the connection string and update the DB</span></span>

<span data-ttu-id="d1b4e-404">Код в обновленной `DbInitializer` добавляет данные начальное значение для новых сущностей.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-404">The code in the updated `DbInitializer` adds seed data for the new entities.</span></span> <span data-ttu-id="d1b4e-405">Чтобы принудительно EF основных компонентов для создания новой пустой базы данных:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-405">To force EF Core to create a new empty DB:</span></span>

* <span data-ttu-id="d1b4e-406">Измените имя строки подключения базы данных в *appsettings.json* для ContosoUniversity3.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-406">Change the DB connection string name in *appsettings.json* to ContosoUniversity3.</span></span> <span data-ttu-id="d1b4e-407">Новое имя должно быть имя, которое не было использовано на компьютере.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-407">The new name must be a name that hasn't been used on the computer.</span></span>

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
      },
    ```

* <span data-ttu-id="d1b4e-408">Кроме того удаление базы данных с помощью:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-408">Alternatively, delete the DB using:</span></span>

    * <span data-ttu-id="d1b4e-409">**Обозреватель объектов SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="d1b4e-409">**SQL Server Object Explorer** (SSOX).</span></span>
    * <span data-ttu-id="d1b4e-410">`database drop` Команду CLI:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-410">The `database drop` CLI command:</span></span>

   ```console
   dotnet ef database drop
   ```

<span data-ttu-id="d1b4e-411">Запустите `database update` в командную строку:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-411">Run `database update` in the command window:</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="d1b4e-412">Предыдущая команда выполняется миграция.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-412">The preceding command runs all the migrations.</span></span>

<span data-ttu-id="d1b4e-413">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-413">Run the app.</span></span> <span data-ttu-id="d1b4e-414">Приложение выполняется под управлением `DbInitializer.Initialize` метод.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-414">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="d1b4e-415">`DbInitializer.Initialize` Заполняет новой базы данных.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-415">The `DbInitializer.Initialize` populates the new DB.</span></span>

<span data-ttu-id="d1b4e-416">Откройте базу данных в SSOX:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-416">Open the DB in SSOX:</span></span>

* <span data-ttu-id="d1b4e-417">Разверните **таблиц** узла.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-417">Expand the **Tables** node.</span></span> <span data-ttu-id="d1b4e-418">Будут отображены созданные таблицы.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-418">The created tables are displayed.</span></span>
* <span data-ttu-id="d1b4e-419">Если ранее был открыт SSOX, нажмите кнопку **обновление** кнопки.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-419">If SSOX was opened previously, click the **Refresh** button.</span></span>

![Таблицы в SSOX](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="d1b4e-421">Изучите **CourseAssignment** таблицы:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-421">Examine the **CourseAssignment** table:</span></span>

* <span data-ttu-id="d1b4e-422">Щелкните правой кнопкой мыши **CourseAssignment** таблицы и выберите **данные представления**.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-422">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
* <span data-ttu-id="d1b4e-423">Проверьте **CourseAssignment** таблица содержит данные.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-423">Verify the **CourseAssignment** table contains data.</span></span>

![Данные CourseAssignment в SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="fk"></a>

## <a name="fixing-foreign-key-constraints-with-legacy-data"></a><span data-ttu-id="d1b4e-425">Исправление ограничения внешнего ключа с помощью данных из прежних версий</span><span class="sxs-lookup"><span data-stu-id="d1b4e-425">Fixing foreign key constraints with legacy data</span></span>

<span data-ttu-id="d1b4e-426">Этот раздел является необязательным.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-426">This section is optional.</span></span>

<span data-ttu-id="d1b4e-427">При выполнении миграции с существующими данными, могут существовать ограничения внешнего ключа, которые не выполнены для существующих данных.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-427">When migrations are run with existing data, there may be FK constraints that are not satisfied with the exiting data.</span></span> <span data-ttu-id="d1b4e-428">С рабочими данными необходимо выполнить действия для переноса существующих данных.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-428">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="d1b4e-429">В этом разделе приведен пример исправления нарушения ограничений внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-429">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="d1b4e-430">Не внести эти изменения кода без резервной копии.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-430">Don't make these code changes without a backup.</span></span> <span data-ttu-id="d1b4e-431">Не внести эти изменения кода, если выполнено предыдущего раздела и изменения в базу данных.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-431">Don't make these code changes if you completed the previous section and updated the database.</span></span>

<span data-ttu-id="d1b4e-432">*{Timestamp}_ComplexDataModel.cs* файл содержит следующий код:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-432">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="d1b4e-433">Предыдущий код добавляет не допускающий `DepartmentID` внешнего ключа для `Course` таблицы.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-433">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="d1b4e-434">Базы данных из предыдущего учебника содержит строки в `Course`, поэтому для этой таблицы не может быть обновлена миграции.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-434">The DB from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="d1b4e-435">Чтобы сделать `ComplexDataModel` миграции работы с существующими данными:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-435">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="d1b4e-436">Изменить код, чтобы предоставить новый столбец (`DepartmentID`) значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-436">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="d1b4e-437">Создайте фиктивный подразделение с именем «Temp» в качестве подразделения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-437">Create a fake department named "Temp" to act as the default department.</span></span>

### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="d1b4e-438">Исправьте ограничения внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="d1b4e-438">Fix the foreign key constraints</span></span>

<span data-ttu-id="d1b4e-439">Обновление `ComplexDataModel` классы `Up` метод:</span><span class="sxs-lookup"><span data-stu-id="d1b4e-439">Update the `ComplexDataModel` classes `Up` method:</span></span>

* <span data-ttu-id="d1b4e-440">Откройте *{timestamp}_ComplexDataModel.cs* файл.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-440">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="d1b4e-441">Закомментировать строку кода, который добавляет `DepartmentID` столбец `Course` таблицы.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-441">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="d1b4e-442">Добавьте следующий выделенный код.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-442">Add the following highlighted code.</span></span> <span data-ttu-id="d1b4e-443">Новый код идет после `.CreateTable( name: "Department"` блока:[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span><span class="sxs-lookup"><span data-stu-id="d1b4e-443">The new code goes after the `.CreateTable( name: "Department"` block: [!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span></span>

<span data-ttu-id="d1b4e-444">С предыдущие изменения, существующие `Course` строки будут связаны с отделом «Temp» после `ComplexDataModel` `Up` метода.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-444">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel` `Up` method runs.</span></span>

<span data-ttu-id="d1b4e-445">Так же производственного приложения.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-445">A production app would:</span></span>

* <span data-ttu-id="d1b4e-446">Включает код или сценарии, чтобы добавить `Department` строк и связанных с `Course` строк к новому `Department` строк.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-446">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="d1b4e-447">Не используйте отдела «Temp» или значение по умолчанию для `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-447">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

<span data-ttu-id="d1b4e-448">Далее в этом учебнике описывается взаимосвязанных данных.</span><span class="sxs-lookup"><span data-stu-id="d1b4e-448">The next tutorial covers related data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="d1b4e-449">[Назад](xref:data/ef-rp/migrations)
[Вперед](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="d1b4e-449">[Previous](xref:data/ef-rp/migrations)
[Next](xref:data/ef-rp/read-related-data)</span></span>
