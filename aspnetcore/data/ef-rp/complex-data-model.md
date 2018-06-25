---
title: Razor Pages с EF Core в ASP.NET Core — модель данных— 5 из 8
author: rick-anderson
description: В этом руководстве вы добавите дополнительные сущности и связи, а также настроите модель данных, указав правила форматирования, проверки и сопоставления.
ms.author: riande
ms.date: 10/25/2017
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: a885809205f13e1090a957496710cc0d9c7257c0
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274545"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---data-model---5-of-8"></a><span data-ttu-id="5cde8-103">Razor Pages с EF Core в ASP.NET Core — модель данных— 5 из 8</span><span class="sxs-lookup"><span data-stu-id="5cde8-103">Razor Pages with EF Core in ASP.NET Core - Data Model - 5 of 8</span></span>

<span data-ttu-id="5cde8-104">Авторы: [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="5cde8-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="5cde8-105">Предыдущие руководства работали с базовой моделью данных, состоящей из трех сущностей.</span><span class="sxs-lookup"><span data-stu-id="5cde8-105">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="5cde8-106">В этом руководстве:</span><span class="sxs-lookup"><span data-stu-id="5cde8-106">In this tutorial:</span></span>

* <span data-ttu-id="5cde8-107">Добавляются дополнительные сущности и связи.</span><span class="sxs-lookup"><span data-stu-id="5cde8-107">More entities and relationships are added.</span></span>
* <span data-ttu-id="5cde8-108">Настраивается модель данных с помощью указания правил для форматирования, проверки и сопоставления базы данных.</span><span class="sxs-lookup"><span data-stu-id="5cde8-108">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="5cde8-109">Классы сущностей для готовой модели данных показаны на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="5cde8-109">The entity classes for the completed data model is shown in the following illustration:</span></span>

![Схема сущностей](complex-data-model/_static/diagram.png)

<span data-ttu-id="5cde8-111">При возникновении проблем, которые вам не удается устранить, скачайте [готовое приложение для этого этапа](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).</span><span class="sxs-lookup"><span data-stu-id="5cde8-111">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).</span></span>

## <a name="customize-the-data-model-with-attributes"></a><span data-ttu-id="5cde8-112">Настройка модели данных с использованием атрибутов</span><span class="sxs-lookup"><span data-stu-id="5cde8-112">Customize the data model with attributes</span></span>

<span data-ttu-id="5cde8-113">В этом разделе модель данных настраивается с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="5cde8-113">In this section, the data model is customized using attributes.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="5cde8-114">Атрибут DataType</span><span class="sxs-lookup"><span data-stu-id="5cde8-114">The DataType attribute</span></span>

<span data-ttu-id="5cde8-115">Страницы учащихся сейчас отображают время и дату зачисления.</span><span class="sxs-lookup"><span data-stu-id="5cde8-115">The student pages currently displays the time of the enrollment date.</span></span> <span data-ttu-id="5cde8-116">Как правило, поля даты отображают только дату без времени.</span><span class="sxs-lookup"><span data-stu-id="5cde8-116">Typically, date fields show only the date and not the time.</span></span>

<span data-ttu-id="5cde8-117">Обновите *Models/Student.cs*, используя следующий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="5cde8-117">Update *Models/Student.cs* with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="5cde8-118">Атрибут [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) указывает тип данных более точно по сравнению со встроенным типом базы данных.</span><span class="sxs-lookup"><span data-stu-id="5cde8-118">The [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="5cde8-119">В этом случае следует отображать отобразить только дату, а не дату и время.</span><span class="sxs-lookup"><span data-stu-id="5cde8-119">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="5cde8-120">В [перечислении DataType](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) представлено множество типов данных, таких как Date, Time, PhoneNumber, Currency, EmailAddress и других. Атрибут `DataType` также обеспечивает автоматическое предоставление функций для определенных типов в приложении.</span><span class="sxs-lookup"><span data-stu-id="5cde8-120">The [DataType Enumeration](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="5cde8-121">Пример:</span><span class="sxs-lookup"><span data-stu-id="5cde8-121">For example:</span></span>

* <span data-ttu-id="5cde8-122">Ссылка `mailto:` для `DataType.EmailAddress` создается автоматически.</span><span class="sxs-lookup"><span data-stu-id="5cde8-122">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="5cde8-123">Средство выбора даты предоставляется для `DataType.Date` в большинстве браузеров.</span><span class="sxs-lookup"><span data-stu-id="5cde8-123">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="5cde8-124">Атрибут `DataType` создает атрибуты HTML 5 `data-`, которые используются браузерами с поддержкой HTML 5.</span><span class="sxs-lookup"><span data-stu-id="5cde8-124">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="5cde8-125">Атрибуты `DataType` не предназначены для проверки.</span><span class="sxs-lookup"><span data-stu-id="5cde8-125">The `DataType` attributes don't provide validation.</span></span>

<span data-ttu-id="5cde8-126">`DataType.Date` не задает формат отображаемой даты.</span><span class="sxs-lookup"><span data-stu-id="5cde8-126">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="5cde8-127">По умолчанию поле даты отображается с использованием форматов, установленных в [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support) сервера.</span><span class="sxs-lookup"><span data-stu-id="5cde8-127">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="5cde8-128">С помощью атрибута `DisplayFormat` можно явно указать формат даты:</span><span class="sxs-lookup"><span data-stu-id="5cde8-128">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="5cde8-129">Параметр `ApplyFormatInEditMode` указывает, что формат должен применяться к пользовательскому интерфейсу редактирования.</span><span class="sxs-lookup"><span data-stu-id="5cde8-129">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="5cde8-130">Некоторым полям не следует использовать `ApplyFormatInEditMode`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-130">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="5cde8-131">Например, обозначение денежной единицы в общем случае не должно отображаться в поле редактирования текста.</span><span class="sxs-lookup"><span data-stu-id="5cde8-131">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="5cde8-132">Атрибут `DisplayFormat` можно использовать отдельно.</span><span class="sxs-lookup"><span data-stu-id="5cde8-132">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="5cde8-133">Однако чаще всего `DataType` рекомендуется применять вместе с атрибутом `DisplayFormat`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-133">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="5cde8-134">Атрибут `DataType` передает семантику данных (в отличие от способа их вывода на экран).</span><span class="sxs-lookup"><span data-stu-id="5cde8-134">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="5cde8-135">Атрибут `DataType` дает следующие преимущества, недоступные в `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="5cde8-135">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="5cde8-136">Поддержка функций HTML5 в браузере.</span><span class="sxs-lookup"><span data-stu-id="5cde8-136">The browser can enable HTML5 features.</span></span> <span data-ttu-id="5cde8-137">Например, отображение элемента управления календарем, соответствующего языковому стандарту обозначения денежной единицы, ссылок электронной почты, проверки входных данных на стороне клиента и т. д.</span><span class="sxs-lookup"><span data-stu-id="5cde8-137">For example, show a calendar control, the locale-appropriate currency symbol, email links, client-side input validation, etc.</span></span>
* <span data-ttu-id="5cde8-138">По умолчанию формат отображения данных в браузере определяется в соответствии с установленным языковым стандартом.</span><span class="sxs-lookup"><span data-stu-id="5cde8-138">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="5cde8-139">Дополнительные сведения см. в документации по [вспомогательной функции тегов \<input>](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="5cde8-139">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

<span data-ttu-id="5cde8-140">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="5cde8-140">Run the app.</span></span> <span data-ttu-id="5cde8-141">Перейдите на страницу индекса учащихся.</span><span class="sxs-lookup"><span data-stu-id="5cde8-141">Navigate to the Students Index page.</span></span> <span data-ttu-id="5cde8-142">Время больше не отображается.</span><span class="sxs-lookup"><span data-stu-id="5cde8-142">Times are no longer displayed.</span></span> <span data-ttu-id="5cde8-143">Каждое представление, использующее модель `Student`, отображает дату без времени.</span><span class="sxs-lookup"><span data-stu-id="5cde8-143">Every view that uses the `Student` model displays the date without time.</span></span>

![Страница указателя учащихся с датами без времени](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="5cde8-145">Атрибут StringLength</span><span class="sxs-lookup"><span data-stu-id="5cde8-145">The StringLength attribute</span></span>

<span data-ttu-id="5cde8-146">С помощью атрибутов можно указать правила проверки данных и сообщения об ошибках проверки.</span><span class="sxs-lookup"><span data-stu-id="5cde8-146">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="5cde8-147">Атрибут [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) указывает минимальную и максимальную длину символов, разрешенных в поле данных.</span><span class="sxs-lookup"><span data-stu-id="5cde8-147">The [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="5cde8-148">Атрибут `StringLength` также обеспечивает проверку на стороне клиента и на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="5cde8-148">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="5cde8-149">Минимальное значение не оказывает влияния на схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="5cde8-149">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="5cde8-150">Обновите модель`Student`, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="5cde8-150">Update the `Student` model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="5cde8-151">Предыдущий код задает ограничение длины имен в 50 символов.</span><span class="sxs-lookup"><span data-stu-id="5cde8-151">The preceding code limits names to no more than 50 characters.</span></span> <span data-ttu-id="5cde8-152">Атрибут `StringLength` не запрещает пользователю ввести пробел в качестве имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="5cde8-152">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="5cde8-153">Атрибут [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) используется для применения ограничений к входным данным.</span><span class="sxs-lookup"><span data-stu-id="5cde8-153">The [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute is used to apply restrictions to the input.</span></span> <span data-ttu-id="5cde8-154">Например, следующий код требует, чтобы первый символ был прописной буквой, а остальные символы были буквенными:</span><span class="sxs-lookup"><span data-stu-id="5cde8-154">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

<span data-ttu-id="5cde8-155">Запустите приложение:</span><span class="sxs-lookup"><span data-stu-id="5cde8-155">Run the app:</span></span>

* <span data-ttu-id="5cde8-156">Перейдите на страницу учащихся.</span><span class="sxs-lookup"><span data-stu-id="5cde8-156">Navigate to the Students page.</span></span>
* <span data-ttu-id="5cde8-157">Выберите **Create New** (Создать) и введите имя длиной более 50 символов.</span><span class="sxs-lookup"><span data-stu-id="5cde8-157">Select **Create New**, and enter a name longer than 50 characters.</span></span>
* <span data-ttu-id="5cde8-158">Выберите **Create** (Создать), проверка на стороне клиента отображает сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="5cde8-158">Select **Create**, client-side validation shows an error message.</span></span>

![Страница указателя учащихся с ошибками длины строки](complex-data-model/_static/string-length-errors.png)

<span data-ttu-id="5cde8-160">В **обозревателе объектов SQL Server** (SSOX) откройте конструктор таблиц учащихся, дважды щелкнув таблицу **Student** (Учащийся).</span><span class="sxs-lookup"><span data-stu-id="5cde8-160">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Таблица учащихся в SSOX до миграций](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="5cde8-162">На предыдущем изображении показана схемы для таблицы `Student`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-162">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="5cde8-163">Поля имен имеют тип `nvarchar(MAX)`, так как миграции для базы данных не выполнялись.</span><span class="sxs-lookup"><span data-stu-id="5cde8-163">The name fields have type `nvarchar(MAX)` because migrations has not been run on the DB.</span></span> <span data-ttu-id="5cde8-164">Когда в дальнейшем миграции будут выполнены, поля имен станут `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-164">When migrations are run later in this tutorial, the name fields become `nvarchar(50)`.</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="5cde8-165">Атрибут Column</span><span class="sxs-lookup"><span data-stu-id="5cde8-165">The Column attribute</span></span>

<span data-ttu-id="5cde8-166">Атрибуты могут управлять, как именно классы и свойства сопоставляются с базой данных.</span><span class="sxs-lookup"><span data-stu-id="5cde8-166">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="5cde8-167">В этом разделе атрибут `Column` используется для сопоставления имени свойства `FirstMidName` с "FirstName" в базе данных.</span><span class="sxs-lookup"><span data-stu-id="5cde8-167">In this section, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the DB.</span></span>

<span data-ttu-id="5cde8-168">При создании базы данных имена свойств в модели используются для имен столбцов (кроме случая, когда используется атрибут `Column`).</span><span class="sxs-lookup"><span data-stu-id="5cde8-168">When the DB is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span>

<span data-ttu-id="5cde8-169">Модель `Student` использует `FirstMidName` для поля имени, так как это поле также может содержать отчество.</span><span class="sxs-lookup"><span data-stu-id="5cde8-169">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="5cde8-170">Обновите файл *Student.cs*, используя следующий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="5cde8-170">Update the *Student.cs* file with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="5cde8-171">С учетом предыдущего изменения `Student.FirstMidName` в приложении сопоставляется со столбцом `FirstName` таблицы `Student`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-171">With the preceding change, `Student.FirstMidName` in the app maps to the `FirstName` column of the `Student` table.</span></span>

<span data-ttu-id="5cde8-172">Добавление атрибута `Column` изменяет модель для поддержки `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-172">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="5cde8-173">Модель, поддерживающая `SchoolContext`, больше не соответствует базе данных.</span><span class="sxs-lookup"><span data-stu-id="5cde8-173">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="5cde8-174">Если приложение запускается перед применением миграций, возникает следующее исключение:</span><span class="sxs-lookup"><span data-stu-id="5cde8-174">If the app is run before applying migrations, the following exception is generated:</span></span>

```SQL
SqlException: Invalid column name 'FirstName'.
```
<span data-ttu-id="5cde8-175">Для обновления базы данных сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="5cde8-175">To update the DB:</span></span>

* <span data-ttu-id="5cde8-176">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="5cde8-176">Build the project.</span></span>
* <span data-ttu-id="5cde8-177">Откройте командное окно в папке проекта.</span><span class="sxs-lookup"><span data-stu-id="5cde8-177">Open a command window in the project folder.</span></span> <span data-ttu-id="5cde8-178">Введите следующие команды для создания миграции и обновления базы данных:</span><span class="sxs-lookup"><span data-stu-id="5cde8-178">Enter the following commands to create a new migration and update the DB:</span></span>

    ```console
    dotnet ef migrations add ColumnFirstName
    dotnet ef database update
    ```

<span data-ttu-id="5cde8-179">Команда `dotnet ef migrations add ColumnFirstName` выдает следующее предупреждающее сообщение:</span><span class="sxs-lookup"><span data-stu-id="5cde8-179">The `dotnet ef migrations add ColumnFirstName` command generates the following warning message:</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

<span data-ttu-id="5cde8-180">Это предупреждение вызвано тем, что поля имен теперь ограничены 50 символами.</span><span class="sxs-lookup"><span data-stu-id="5cde8-180">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="5cde8-181">Если имя в базе данных имеет больше 50 символов, символы с 51 до последнего будут потеряны.</span><span class="sxs-lookup"><span data-stu-id="5cde8-181">If a name in the DB had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="5cde8-182">Проверьте работу приложения.</span><span class="sxs-lookup"><span data-stu-id="5cde8-182">Test the app.</span></span>

<span data-ttu-id="5cde8-183">Откройте таблицу "Student" (Учащийся) в окне SSOX:</span><span class="sxs-lookup"><span data-stu-id="5cde8-183">Open the Student table in SSOX:</span></span>

![Таблица учащихся в SSOX после миграций](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="5cde8-185">До применения миграции столбцы имен имели тип [nvarchar(MAX)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="5cde8-185">Before migration was applied, the name columns were of type [nvarchar(MAX)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="5cde8-186">Теперь столбцы имен имеют тип `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-186">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="5cde8-187">Имя столбца изменилось с `FirstMidName` на `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-187">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

> [!Note]
> <span data-ttu-id="5cde8-188">В следующем разделе сборка приложения на некоторых этапах приводит к возникновению ошибок компилятора.</span><span class="sxs-lookup"><span data-stu-id="5cde8-188">In the following section, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="5cde8-189">В инструкциях указано, когда производить сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="5cde8-189">The instructions specify when to build the app.</span></span>

## <a name="student-entity-update"></a><span data-ttu-id="5cde8-190">Обновление сущности Student</span><span class="sxs-lookup"><span data-stu-id="5cde8-190">Student entity update</span></span>

![Сущность Student](complex-data-model/_static/student-entity.png)

<span data-ttu-id="5cde8-192">Обновите *Models/Student.cs*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="5cde8-192">Update *Models/Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="5cde8-193">Атрибут Required</span><span class="sxs-lookup"><span data-stu-id="5cde8-193">The Required attribute</span></span>

<span data-ttu-id="5cde8-194">Атрибут `Required` делает свойства имен обязательными полями.</span><span class="sxs-lookup"><span data-stu-id="5cde8-194">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="5cde8-195">Атрибут `Required` не нужен для типов, не допускающих значения null, например для типов значений (`DateTime`, `int`, `double` и т. д.).</span><span class="sxs-lookup"><span data-stu-id="5cde8-195">The `Required` attribute isn't needed for non-nullable types such as value types (`DateTime`, `int`, `double`, etc.).</span></span> <span data-ttu-id="5cde8-196">Типы, которые не могут принимать значение null, автоматически обрабатываются как обязательные поля.</span><span class="sxs-lookup"><span data-stu-id="5cde8-196">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="5cde8-197">Атрибут `Required` можно заменить параметром минимальной длины в атрибуте `StringLength`:</span><span class="sxs-lookup"><span data-stu-id="5cde8-197">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="5cde8-198">Атрибут Display</span><span class="sxs-lookup"><span data-stu-id="5cde8-198">The Display attribute</span></span>

<span data-ttu-id="5cde8-199">Атрибут `Display` указывает, что заголовки для текстовых полей должны иметь вид "First Name" (Имя), "Last Name" (Фамилия), "Full Name" (Полное имя) и "Enrollment Date" (Дата зачисления).</span><span class="sxs-lookup"><span data-stu-id="5cde8-199">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="5cde8-200">По умолчанию в заголовках не использовался пробел для разделения слов, например "Lastname".</span><span class="sxs-lookup"><span data-stu-id="5cde8-200">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="5cde8-201">Вычисляемое свойство FullName</span><span class="sxs-lookup"><span data-stu-id="5cde8-201">The FullName calculated property</span></span>

<span data-ttu-id="5cde8-202">`FullName` — это вычисляемое свойство, которое возвращает значение, созданное путем объединения двух других свойств.</span><span class="sxs-lookup"><span data-stu-id="5cde8-202">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="5cde8-203">`FullName` не может быть задано, оно имеет только метод доступа get.</span><span class="sxs-lookup"><span data-stu-id="5cde8-203">`FullName` cannot be set, it has only a get accessor.</span></span> <span data-ttu-id="5cde8-204">В базе данных не создается столбец `FullName`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-204">No `FullName` column is created in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="5cde8-205">Создание сущности Instructor</span><span class="sxs-lookup"><span data-stu-id="5cde8-205">Create the Instructor Entity</span></span>

![Сущность Instructor](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="5cde8-207">Создайте файл *Models/Instructor.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="5cde8-207">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

<span data-ttu-id="5cde8-208">Обратите внимание, что некоторые свойства являются одинаковыми в сущностях `Student` и `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-208">Notice that several properties are the same in the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="5cde8-209">В руководстве по реализации наследования далее в этой серии выполняется рефакторинг данного кода, чтобы устранить избыточность.</span><span class="sxs-lookup"><span data-stu-id="5cde8-209">In the Implementing Inheritance tutorial later in this series, this code is refactored to eliminate the redundancy.</span></span>

<span data-ttu-id="5cde8-210">На одной строке могут находиться несколько атрибутов.</span><span class="sxs-lookup"><span data-stu-id="5cde8-210">Multiple attributes can be on one line.</span></span> <span data-ttu-id="5cde8-211">Атрибуты `HireDate` можно записать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="5cde8-211">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="5cde8-212">Свойства навигации CourseAssignments и OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="5cde8-212">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="5cde8-213">`CourseAssignments` и `OfficeAssignment` — это свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="5cde8-213">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="5cde8-214">Преподаватель может проводить любое количество курсов, поэтому `CourseAssignments` определен как коллекция.</span><span class="sxs-lookup"><span data-stu-id="5cde8-214">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="5cde8-215">Если свойство навигации содержит несколько сущностей:</span><span class="sxs-lookup"><span data-stu-id="5cde8-215">If a navigation property holds multiple entities:</span></span>

* <span data-ttu-id="5cde8-216">Оно должно быть типом списка, где можно добавлять, удалять и обновлять записи.</span><span class="sxs-lookup"><span data-stu-id="5cde8-216">It must be a list type where the entries can be added, deleted, and updated.</span></span>

<span data-ttu-id="5cde8-217">К типам свойств навигации относятся следующие:</span><span class="sxs-lookup"><span data-stu-id="5cde8-217">Navigation property types include:</span></span>

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

<span data-ttu-id="5cde8-218">Если указан тип `ICollection<T>`, платформа EF Core по умолчанию создает коллекцию `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-218">If `ICollection<T>` is specified, EF Core creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="5cde8-219">Сущность `CourseAssignment` описана в разделе по связям многие ко многим.</span><span class="sxs-lookup"><span data-stu-id="5cde8-219">The `CourseAssignment` entity is explained in the section on many-to-many relationships.</span></span>

<span data-ttu-id="5cde8-220">Бизнес-правила университета Contoso указывают, что преподаватель может иметь не более одного кабинета.</span><span class="sxs-lookup"><span data-stu-id="5cde8-220">Contoso University business rules state that an instructor can have at most one office.</span></span> <span data-ttu-id="5cde8-221">Свойство `OfficeAssignment` содержит отдельную сущность `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-221">The `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="5cde8-222">`OfficeAssignment` имеет значение null, если кабинет не назначен.</span><span class="sxs-lookup"><span data-stu-id="5cde8-222">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="5cde8-223">Создание сущности OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="5cde8-223">Create the OfficeAssignment entity</span></span>

![Сущность OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="5cde8-225">Создайте файл *Models/OfficeAssignment.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="5cde8-225">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="5cde8-226">Атрибут Key</span><span class="sxs-lookup"><span data-stu-id="5cde8-226">The Key attribute</span></span>

<span data-ttu-id="5cde8-227">Атрибут `[Key]` используется для идентификации свойства в качестве первичного ключа (PK), когда имя свойства отличается от classnameID или ID.</span><span class="sxs-lookup"><span data-stu-id="5cde8-227">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="5cde8-228">Между сущностями `Instructor` и `OfficeAssignment` действует связь один к нулю или к одному.</span><span class="sxs-lookup"><span data-stu-id="5cde8-228">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="5cde8-229">Назначение кабинета существует только в связи с преподавателем, которому оно назначено.</span><span class="sxs-lookup"><span data-stu-id="5cde8-229">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="5cde8-230">Первичный ключ `OfficeAssignment` также является внешним ключом (FK) для сущности `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-230">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span> <span data-ttu-id="5cde8-231">EF Core не распознает автоматически `InstructorID` как первичный ключ объекта `OfficeAssignment` по следующей причине:</span><span class="sxs-lookup"><span data-stu-id="5cde8-231">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because:</span></span>

* <span data-ttu-id="5cde8-232">`InstructorID` не соблюдает соглашение об именовании ID или classnameID.</span><span class="sxs-lookup"><span data-stu-id="5cde8-232">`InstructorID` doesn't follow the ID or classnameID naming convention.</span></span>

<span data-ttu-id="5cde8-233">Таким образом, атрибут `Key` используется для определения `InstructorID` в качестве первичного ключа:</span><span class="sxs-lookup"><span data-stu-id="5cde8-233">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="5cde8-234">По умолчанию EF Core считает ключ созданным не базой данных, так как столбец предназначен для идентифицирующего отношения.</span><span class="sxs-lookup"><span data-stu-id="5cde8-234">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="5cde8-235">Свойство навигации Instructor</span><span class="sxs-lookup"><span data-stu-id="5cde8-235">The Instructor navigation property</span></span>

<span data-ttu-id="5cde8-236">Свойство навигации `OfficeAssignment` для сущности `Instructor` допускает значение null по следующим причинам:</span><span class="sxs-lookup"><span data-stu-id="5cde8-236">The `OfficeAssignment` navigation property for the `Instructor` entity is nullable because:</span></span>

* <span data-ttu-id="5cde8-237">Ссылочные типы (например, классы) допускают значение null.</span><span class="sxs-lookup"><span data-stu-id="5cde8-237">Reference types (such as classes are nullable).</span></span>
* <span data-ttu-id="5cde8-238">Преподавателю может быть не назначен кабинет.</span><span class="sxs-lookup"><span data-stu-id="5cde8-238">An instructor might not have an office assignment.</span></span>


<span data-ttu-id="5cde8-239">Сущность `OfficeAssignment` имеет свойство навигации `Instructor`, не допускающее значение null, по следующим причинам:</span><span class="sxs-lookup"><span data-stu-id="5cde8-239">The `OfficeAssignment` entity has a non-nullable `Instructor` navigation property because:</span></span>

* <span data-ttu-id="5cde8-240">`InstructorID` не допускает значение null.</span><span class="sxs-lookup"><span data-stu-id="5cde8-240">`InstructorID` is non-nullable.</span></span>
* <span data-ttu-id="5cde8-241">Назначение кабинета не может существовать без преподавателя.</span><span class="sxs-lookup"><span data-stu-id="5cde8-241">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="5cde8-242">Когда сущность `Instructor` имеет связанную сущность `OfficeAssignment`, каждая из них имеет ссылку на другую в своем свойстве навигации.</span><span class="sxs-lookup"><span data-stu-id="5cde8-242">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="5cde8-243">Атрибут `[Required]` можно применить к свойству навигации `Instructor`:</span><span class="sxs-lookup"><span data-stu-id="5cde8-243">The `[Required]` attribute could be applied to the `Instructor` navigation property:</span></span>

```csharp
[Required]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="5cde8-244">Предыдущий код указывает, что должен существовать связанный преподаватель.</span><span class="sxs-lookup"><span data-stu-id="5cde8-244">The preceding code specifies that there must be a related instructor.</span></span> <span data-ttu-id="5cde8-245">Этот код не нужен, так как внешний ключ `InstructorID` (который также является первичным) не допускает значение null.</span><span class="sxs-lookup"><span data-stu-id="5cde8-245">The preceding code is unnecessary because the `InstructorID` foreign key (which is also the PK) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="5cde8-246">Изменение сущности Course</span><span class="sxs-lookup"><span data-stu-id="5cde8-246">Modify the Course Entity</span></span>

![Сущность Course](complex-data-model/_static/course-entity.png)

<span data-ttu-id="5cde8-248">Обновите *Models/Course.cs*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="5cde8-248">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="5cde8-249">Сущность `Course` имеет свойство внешнего ключа (FK) `DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-249">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="5cde8-250">`DepartmentID` указывает на связанную сущность `Department`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-250">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="5cde8-251">Сущность `Course` имеет свойство навигации `Department`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-251">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="5cde8-252">EF Core не требует свойство внешнего ключа для модели данных, если она имеет свойство навигации для связанной сущности.</span><span class="sxs-lookup"><span data-stu-id="5cde8-252">EF Core doesn't require a FK property for a data model when the model has a navigation property for a related entity.</span></span>

<span data-ttu-id="5cde8-253">EF Core автоматически создает внешние ключи в базе данных по мере необходимости.</span><span class="sxs-lookup"><span data-stu-id="5cde8-253">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="5cde8-254">EF Core создает [теневые свойства](https://docs.microsoft.com/ef/core/modeling/shadow-properties) для автоматически созданных внешних ключей.</span><span class="sxs-lookup"><span data-stu-id="5cde8-254">EF Core creates [shadow properties](https://docs.microsoft.com/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="5cde8-255">Наличие внешнего ключа в модели данных позволяет сделать обновления проще и эффективнее.</span><span class="sxs-lookup"><span data-stu-id="5cde8-255">Having the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="5cde8-256">Например, рассмотрим модель, где свойство внешнего ключа `DepartmentID` *не* включено.</span><span class="sxs-lookup"><span data-stu-id="5cde8-256">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="5cde8-257">При получении сущности курса для редактирования:</span><span class="sxs-lookup"><span data-stu-id="5cde8-257">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="5cde8-258">сущность `Department` имеет значение null, если она не загружена явно;</span><span class="sxs-lookup"><span data-stu-id="5cde8-258">The `Department` entity is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="5cde8-259">для обновления сущности курса нужно сначала получить сущность `Department`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-259">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="5cde8-260">Если свойство внешнего ключа `DepartmentID` включено в модель данных, получать сущность `Department` перед обновлением не нужно.</span><span class="sxs-lookup"><span data-stu-id="5cde8-260">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="5cde8-261">Атрибут DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="5cde8-261">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="5cde8-262">Атрибут `[DatabaseGenerated(DatabaseGeneratedOption.None)]` указывает, что первичный ключ предоставляется приложением, а не создается базой данных.</span><span class="sxs-lookup"><span data-stu-id="5cde8-262">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="5cde8-263">По умолчанию EF Core предполагает, что значения первичного ключа создаются базой данных.</span><span class="sxs-lookup"><span data-stu-id="5cde8-263">By default, EF Core assumes that PK values are generated by the DB.</span></span> <span data-ttu-id="5cde8-264">Обычно лучше всего использовать значения первичного ключа, созданные базой данных.</span><span class="sxs-lookup"><span data-stu-id="5cde8-264">DB generated PK values is generally the best approach.</span></span> <span data-ttu-id="5cde8-265">Для сущностей `Course` пользователь указывает первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="5cde8-265">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="5cde8-266">Например, номер курса, такой как серия 1000 для кафедры математики и серия 2000 для кафедры английского языка.</span><span class="sxs-lookup"><span data-stu-id="5cde8-266">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="5cde8-267">Атрибут `DatabaseGenerated` также может использоваться для создания значений по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="5cde8-267">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="5cde8-268">Например, база данных может автоматически создать поле даты для записи даты, когда строка была создана или изменена.</span><span class="sxs-lookup"><span data-stu-id="5cde8-268">For example, the DB can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="5cde8-269">Дополнительные сведения см. в разделе [Созданные свойства](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="5cde8-269">For more information, see [Generated Properties](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="5cde8-270">Свойства внешнего ключа и навигации</span><span class="sxs-lookup"><span data-stu-id="5cde8-270">Foreign key and navigation properties</span></span>

<span data-ttu-id="5cde8-271">Свойства внешнего ключа (FK) и свойства навигации в сущности `Course` отражают следующие связи:</span><span class="sxs-lookup"><span data-stu-id="5cde8-271">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="5cde8-272">Курс назначается одной кафедре, поэтому имеется внешний ключ `DepartmentID` и свойство навигации `Department`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-272">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="5cde8-273">На курс может быть зачислено любое количество учащихся, поэтому свойство навигации `Enrollments` является коллекцией:</span><span class="sxs-lookup"><span data-stu-id="5cde8-273">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="5cde8-274">Курс могут вести несколько преподавателей, поэтому свойство навигации `CourseAssignments` является коллекцией:</span><span class="sxs-lookup"><span data-stu-id="5cde8-274">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="5cde8-275">`CourseAssignment` описано [далее](#many-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="5cde8-275">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="create-the-department-entity"></a><span data-ttu-id="5cde8-276">Создание сущности Department</span><span class="sxs-lookup"><span data-stu-id="5cde8-276">Create the Department entity</span></span>

![Сущность Department](complex-data-model/_static/department-entity.png)

<span data-ttu-id="5cde8-278">Создайте файл *Models/Department.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="5cde8-278">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="5cde8-279">Атрибут Column</span><span class="sxs-lookup"><span data-stu-id="5cde8-279">The Column attribute</span></span>

<span data-ttu-id="5cde8-280">Ранее атрибут `Column` использовался, чтобы изменить сопоставление имени столбца.</span><span class="sxs-lookup"><span data-stu-id="5cde8-280">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="5cde8-281">В коде для сущности `Department` атрибут `Column` можно использовать, чтобы изменить сопоставление типов данных SQL.</span><span class="sxs-lookup"><span data-stu-id="5cde8-281">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="5cde8-282">Столбец `Budget` определяется с помощью типа money SQL Server в базе данных:</span><span class="sxs-lookup"><span data-stu-id="5cde8-282">The `Budget` column is defined using the SQL Server money type in the DB:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="5cde8-283">Сопоставление столбцов обычно не требуется.</span><span class="sxs-lookup"><span data-stu-id="5cde8-283">Column mapping is generally not required.</span></span> <span data-ttu-id="5cde8-284">В общем случае EF Core выбирает соответствующий тип данных SQL Server на основе типа среды CLR для свойства.</span><span class="sxs-lookup"><span data-stu-id="5cde8-284">EF Core generally chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="5cde8-285">Тип `decimal` среды CLR сопоставляется с типом `decimal` SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5cde8-285">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="5cde8-286">`Budget` используется для денежных единиц, хотя для этого лучше подходит тип данных money.</span><span class="sxs-lookup"><span data-stu-id="5cde8-286">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="5cde8-287">Свойства внешнего ключа и навигации</span><span class="sxs-lookup"><span data-stu-id="5cde8-287">Foreign key and navigation properties</span></span>

<span data-ttu-id="5cde8-288">Свойства внешнего ключа и навигации отражают следующие связи:</span><span class="sxs-lookup"><span data-stu-id="5cde8-288">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="5cde8-289">Кафедра может иметь или не иметь администратора.</span><span class="sxs-lookup"><span data-stu-id="5cde8-289">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="5cde8-290">Администратор всегда является преподавателем.</span><span class="sxs-lookup"><span data-stu-id="5cde8-290">An administrator is always an instructor.</span></span> <span data-ttu-id="5cde8-291">Поэтому свойство `InstructorID` включается в качестве внешнего ключа в сущность `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-291">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="5cde8-292">Свойство навигации называется `Administrator`, но содержит сущность `Instructor`:</span><span class="sxs-lookup"><span data-stu-id="5cde8-292">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="5cde8-293">Вопросительный знак (?) в приведенном выше коде указывает, что свойство допускает значение null.</span><span class="sxs-lookup"><span data-stu-id="5cde8-293">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="5cde8-294">Кафедра может иметь несколько курсов, поэтому доступно свойство навигации Courses:</span><span class="sxs-lookup"><span data-stu-id="5cde8-294">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="5cde8-295">Примечание. По соглашению EF Core разрешает каскадное удаление для внешних ключей, не допускающих значение null, и связей многие ко многим.</span><span class="sxs-lookup"><span data-stu-id="5cde8-295">Note: By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="5cde8-296">Каскадное удаление может привести к циклическим правилам каскадного удаления.</span><span class="sxs-lookup"><span data-stu-id="5cde8-296">Cascading delete can result in circular cascade delete rules.</span></span> <span data-ttu-id="5cde8-297">Такие правила вызывают исключение при добавлении миграции.</span><span class="sxs-lookup"><span data-stu-id="5cde8-297">Circular cascade delete rules causes an exception when a migration is added.</span></span>

<span data-ttu-id="5cde8-298">Например, если свойство `Department.InstructorID` не было определено как допускающее значение null:</span><span class="sxs-lookup"><span data-stu-id="5cde8-298">For example, if the `Department.InstructorID` property wasn't defined as nullable:</span></span>

* <span data-ttu-id="5cde8-299">EF Core настраивает правило каскадного удаления для удаления преподавателя при удалении кафедры.</span><span class="sxs-lookup"><span data-stu-id="5cde8-299">EF Core configures a cascade delete rule to delete the instructor when the department is deleted.</span></span>
* <span data-ttu-id="5cde8-300">Удаление преподавателя при удалении кафедры не является запланированным поведением.</span><span class="sxs-lookup"><span data-stu-id="5cde8-300">Deleting the instructor when the department is deleted isn't the intended behavior.</span></span>

<span data-ttu-id="5cde8-301">Если бизнес-правила требуют, чтобы свойство `InstructorID` не допускало значение null, используйте следующий оператор текучего API:</span><span class="sxs-lookup"><span data-stu-id="5cde8-301">If business rules required the `InstructorID` property be non-nullable, use the following fluent API statement:</span></span>

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

<span data-ttu-id="5cde8-302">Предыдущий код отключает каскадное удаление для связи кафедры и преподавателя.</span><span class="sxs-lookup"><span data-stu-id="5cde8-302">The preceding code disables cascade delete on the department-instructor relationship.</span></span>

## <a name="update-the-enrollment-entity"></a><span data-ttu-id="5cde8-303">Обновление сущности Enrollment</span><span class="sxs-lookup"><span data-stu-id="5cde8-303">Update the Enrollment entity</span></span>

<span data-ttu-id="5cde8-304">Запись зачисления обозначает один курс, который проходит один учащийся.</span><span class="sxs-lookup"><span data-stu-id="5cde8-304">An enrollment record is for a one course taken by one student.</span></span>

![Сущность Enrollment](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="5cde8-306">Обновите *Models/Enrollment.cs*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="5cde8-306">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="5cde8-307">Свойства внешнего ключа и навигации</span><span class="sxs-lookup"><span data-stu-id="5cde8-307">Foreign key and navigation properties</span></span>

<span data-ttu-id="5cde8-308">Свойства внешнего ключа и навигации отражают следующие связи:</span><span class="sxs-lookup"><span data-stu-id="5cde8-308">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="5cde8-309">Запись зачисления предназначена для одного курса, поэтому доступно свойство первичного ключа `CourseID` и свойство навигации `Course`:</span><span class="sxs-lookup"><span data-stu-id="5cde8-309">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="5cde8-310">Запись зачисления предназначена для одного учащегося, поэтому доступно свойство первичного ключа `StudentID` и свойство навигации `Student`:</span><span class="sxs-lookup"><span data-stu-id="5cde8-310">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="5cde8-311">Связи многие ко многим</span><span class="sxs-lookup"><span data-stu-id="5cde8-311">Many-to-Many Relationships</span></span>

<span data-ttu-id="5cde8-312">Между сущностями `Student` и `Course` действует связь многие ко многим.</span><span class="sxs-lookup"><span data-stu-id="5cde8-312">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="5cde8-313">Сущности `Enrollment` выступают в качестве таблицы соединения многие ко многим *с полезными данными* в базе данных.</span><span class="sxs-lookup"><span data-stu-id="5cde8-313">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="5cde8-314">Фраза "с полезными данными" означает, что таблица `Enrollment` содержит дополнительные данные, кроме внешних ключей для присоединяемых таблиц (в данном случае — первичный ключ и `Grade`).</span><span class="sxs-lookup"><span data-stu-id="5cde8-314">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="5cde8-315">На следующем рисунке показано, как выглядят эти связи на схеме сущностей.</span><span class="sxs-lookup"><span data-stu-id="5cde8-315">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="5cde8-316">(Эта схема была создана с помощью EF Power Tools для EF 6.x.</span><span class="sxs-lookup"><span data-stu-id="5cde8-316">(This diagram was generated using EF Power Tools for EF 6.x.</span></span> <span data-ttu-id="5cde8-317">Создание схемы не является частью руководства.)</span><span class="sxs-lookup"><span data-stu-id="5cde8-317">Creating the diagram isn't part of the tutorial.)</span></span>

![Связь многие ко многим между Student и Course](complex-data-model/_static/student-course.png)

<span data-ttu-id="5cde8-319">Каждая линия связи имеет 1 на одном конце и звездочку (\*) на другом, указывая характер один ко многим.</span><span class="sxs-lookup"><span data-stu-id="5cde8-319">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="5cde8-320">Если таблица `Enrollment` не включала в себя сведения об оценках, ей потребуется содержать всего два внешних ключа (`CourseID` и `StudentID`).</span><span class="sxs-lookup"><span data-stu-id="5cde8-320">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="5cde8-321">Таблицу соединения многие ко многим без полезных данных иногда называют чистой таблицей соединения (PJT).</span><span class="sxs-lookup"><span data-stu-id="5cde8-321">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="5cde8-322">Сущности `Instructor` и `Course` имеют связь многие ко многим с использованием чистой таблицы соединения.</span><span class="sxs-lookup"><span data-stu-id="5cde8-322">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="5cde8-323">Примечание. EF 6.x поддерживает неявные таблицы соединения для связей многие ко многим, но EF Core — нет.</span><span class="sxs-lookup"><span data-stu-id="5cde8-323">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="5cde8-324">Дополнительные сведения см. в статье [Связи многие ко многим в EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span><span class="sxs-lookup"><span data-stu-id="5cde8-324">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="5cde8-325">Сущность CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="5cde8-325">The CourseAssignment entity</span></span>

![Сущность CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="5cde8-327">Создайте файл *Models/CourseAssignment.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="5cde8-327">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a><span data-ttu-id="5cde8-328">Связь между Instructor и Courses</span><span class="sxs-lookup"><span data-stu-id="5cde8-328">Instructor-to-Courses</span></span>

![Связь многие ко многим между Instructor и Courses](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="5cde8-330">Связь многие ко многим между Instructor и Courses:</span><span class="sxs-lookup"><span data-stu-id="5cde8-330">The Instructor-to-Courses many-to-many relationship:</span></span>

* <span data-ttu-id="5cde8-331">Нуждается в таблице соединения, которая должна быть представлена набором сущностей.</span><span class="sxs-lookup"><span data-stu-id="5cde8-331">Requires a join table that must be represented by an entity set.</span></span>
* <span data-ttu-id="5cde8-332">Является чистой таблицей соединения (таблицей без полезных данных).</span><span class="sxs-lookup"><span data-stu-id="5cde8-332">Is a pure join table (table without payload).</span></span>

<span data-ttu-id="5cde8-333">Обычно для сущности соединения используется имя `EntityName1EntityName2`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-333">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="5cde8-334">Например, таблицей соединения Instructor и Courses, использующей этот шаблон, является `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-334">For example, the Instructor-to-Courses join table using this pattern is `CourseInstructor`.</span></span> <span data-ttu-id="5cde8-335">Однако рекомендуется использовать имя, которое описывает эту связь.</span><span class="sxs-lookup"><span data-stu-id="5cde8-335">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="5cde8-336">Модели данных создаются простыми и разрастаются.</span><span class="sxs-lookup"><span data-stu-id="5cde8-336">Data models start out simple and grow.</span></span> <span data-ttu-id="5cde8-337">Соединения без полезных данных (PJT) часто начинают включать их.</span><span class="sxs-lookup"><span data-stu-id="5cde8-337">No-payload joins (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="5cde8-338">Если в начале задать описательное имя сущности, его не нужно менять при изменениях таблицы соединения.</span><span class="sxs-lookup"><span data-stu-id="5cde8-338">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="5cde8-339">Оптимально, если сущность соединения имеет собственное естественное имя (возможно, из одного слова) в бизнес-среде.</span><span class="sxs-lookup"><span data-stu-id="5cde8-339">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="5cde8-340">Например, Books и Customers можно связать с сущностью соединения Ratings.</span><span class="sxs-lookup"><span data-stu-id="5cde8-340">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="5cde8-341">Связь многие ко многим между Instructor и Courses `CourseAssignment` является более предпочтительным вариантом, чем `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-341">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="5cde8-342">Составной ключ</span><span class="sxs-lookup"><span data-stu-id="5cde8-342">Composite key</span></span>

<span data-ttu-id="5cde8-343">Внешние ключи не допускают значение null.</span><span class="sxs-lookup"><span data-stu-id="5cde8-343">FKs are not nullable.</span></span> <span data-ttu-id="5cde8-344">Два внешних ключа в `CourseAssignment` (`InstructorID` и `CourseID`) совместно однозначно определяют каждую строку таблицы `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-344">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="5cde8-345">`CourseAssignment` не требуется выделенный первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="5cde8-345">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="5cde8-346">Свойства `InstructorID` и `CourseID` выступают в качестве составного первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="5cde8-346">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="5cde8-347">Единственным способом указать составные первичные ключи для EF Core является *текучий API*.</span><span class="sxs-lookup"><span data-stu-id="5cde8-347">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="5cde8-348">Следующий раздел описывает, как настроить составной первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="5cde8-348">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="5cde8-349">Составной ключ обеспечивает следующее:</span><span class="sxs-lookup"><span data-stu-id="5cde8-349">The composite key ensures:</span></span>

* <span data-ttu-id="5cde8-350">Для одного курса допускается несколько строк.</span><span class="sxs-lookup"><span data-stu-id="5cde8-350">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="5cde8-351">Для одного преподавателя допускается несколько строк.</span><span class="sxs-lookup"><span data-stu-id="5cde8-351">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="5cde8-352">Несколько строк для одного преподавателя и курса недопустимы.</span><span class="sxs-lookup"><span data-stu-id="5cde8-352">Multiple rows for the same instructor and course isn't allowed.</span></span>

<span data-ttu-id="5cde8-353">Сущность соединения `Enrollment` определяет собственный первичный ключ, поэтому подобные дубликаты возможны.</span><span class="sxs-lookup"><span data-stu-id="5cde8-353">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="5cde8-354">Для предотвращения подобных дубликатов:</span><span class="sxs-lookup"><span data-stu-id="5cde8-354">To prevent such duplicates:</span></span>

* <span data-ttu-id="5cde8-355">добавьте уникальный индекс для полей внешнего ключа или</span><span class="sxs-lookup"><span data-stu-id="5cde8-355">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="5cde8-356">настройте `Enrollment` с первичным составным ключом аналогично `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-356">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="5cde8-357">Дополнительные сведения см. в разделе [Индексы](https://docs.microsoft.com/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="5cde8-357">For more information, see [Indexes](https://docs.microsoft.com/ef/core/modeling/indexes).</span></span>

## <a name="update-the-db-context"></a><span data-ttu-id="5cde8-358">Изменение контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="5cde8-358">Update the DB context</span></span>

<span data-ttu-id="5cde8-359">Добавьте выделенный ниже код в файл *Data/SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="5cde8-359">Add the following highlighted code to *Data/SchoolContext.cs*:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="5cde8-360">Предыдущий код добавляет новые сущности и настраивает составной первичный ключ сущности `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-360">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="5cde8-361">Текучий API вместо атрибутов</span><span class="sxs-lookup"><span data-stu-id="5cde8-361">Fluent API alternative to attributes</span></span>

<span data-ttu-id="5cde8-362">Метод `OnModelCreating` в предыдущем коде использует для настройки поведения EF Core *текучий API*.</span><span class="sxs-lookup"><span data-stu-id="5cde8-362">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="5cde8-363">Этот API называется "текучим", так как часто используется для объединения серии вызовов методов в один оператор.</span><span class="sxs-lookup"><span data-stu-id="5cde8-363">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="5cde8-364">В [следующем коде](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) показан пример текучего API:</span><span class="sxs-lookup"><span data-stu-id="5cde8-364">The [following code](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="5cde8-365">В этом руководстве текучий API используется только для сопоставления базы данных, которое невозможно выполнить с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="5cde8-365">In this tutorial, the fluent API is used only for DB mapping that can't be done with attributes.</span></span> <span data-ttu-id="5cde8-366">Однако текучий API позволяет задать большинство правил форматирования, проверки и сопоставления, которые можно указать с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="5cde8-366">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="5cde8-367">Некоторые атрибуты, такие как `MinimumLength`, невозможно применить с текучим API.</span><span class="sxs-lookup"><span data-stu-id="5cde8-367">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="5cde8-368">`MinimumLength` не изменяет схему, он лишь применяет правило проверки минимальной длины.</span><span class="sxs-lookup"><span data-stu-id="5cde8-368">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="5cde8-369">Некоторые разработчики предпочитают использовать текучий API монопольно, чтобы оставить свои классы сущностей "чистыми".</span><span class="sxs-lookup"><span data-stu-id="5cde8-369">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="5cde8-370">Атрибуты и текучий API можно смешивать.</span><span class="sxs-lookup"><span data-stu-id="5cde8-370">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="5cde8-371">Существует несколько конфигураций, которые можно реализовать только с помощью текучего API (с указанием составного первичного ключа).</span><span class="sxs-lookup"><span data-stu-id="5cde8-371">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="5cde8-372">Существует несколько конфигураций, которые можно реализовать только с помощью атрибутов (`MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="5cde8-372">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="5cde8-373">Рекомендации по использованию текучего API и атрибутов:</span><span class="sxs-lookup"><span data-stu-id="5cde8-373">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="5cde8-374">Выберите один из двух этих подходов.</span><span class="sxs-lookup"><span data-stu-id="5cde8-374">Choose one of these two approaches.</span></span>
* <span data-ttu-id="5cde8-375">Используйте выбранный подход максимально согласованно.</span><span class="sxs-lookup"><span data-stu-id="5cde8-375">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="5cde8-376">Некоторые атрибуты из этого руководства используются:</span><span class="sxs-lookup"><span data-stu-id="5cde8-376">Some of the attributes used in the this tutorial are used for:</span></span>

* <span data-ttu-id="5cde8-377">только для проверки (например, `MinimumLength`);</span><span class="sxs-lookup"><span data-stu-id="5cde8-377">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="5cde8-378">только для конфигурации EF Core (например, `HasKey`);</span><span class="sxs-lookup"><span data-stu-id="5cde8-378">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="5cde8-379">для проверки и конфигурации EF Core (например, `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="5cde8-379">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="5cde8-380">Дополнительные сведения о сравнении атрибутов и текучего API см. в разделе [Методы конфигурации](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span><span class="sxs-lookup"><span data-stu-id="5cde8-380">For more information about attributes vs. fluent API, see [Methods of configuration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="5cde8-381">Схема сущностей, показывающая связи</span><span class="sxs-lookup"><span data-stu-id="5cde8-381">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="5cde8-382">Ниже показана схема, создаваемая средствами EF Power Tools для завершенной модели School.</span><span class="sxs-lookup"><span data-stu-id="5cde8-382">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Схема сущностей](complex-data-model/_static/diagram.png)

<span data-ttu-id="5cde8-384">На предыдущей схеме показано следующее:</span><span class="sxs-lookup"><span data-stu-id="5cde8-384">The preceding diagram shows:</span></span>

* <span data-ttu-id="5cde8-385">Несколько линий связи один ко многим (1 к \*).</span><span class="sxs-lookup"><span data-stu-id="5cde8-385">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="5cde8-386">Линия связи один к нулю или одному (1 к 0..1) между сущностями `Instructor` и `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-386">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="5cde8-387">Линия связи нуль или один ко многим (0..1 к \*) между сущностями `Instructor` и `Department`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-387">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-db-with-test-data"></a><span data-ttu-id="5cde8-388">Заполнение базы данных тестовыми данными</span><span class="sxs-lookup"><span data-stu-id="5cde8-388">Seed the DB with Test Data</span></span>

<span data-ttu-id="5cde8-389">Обновите код в файле *Data/DbInitializer.cs*:</span><span class="sxs-lookup"><span data-stu-id="5cde8-389">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="5cde8-390">Предыдущий код предоставляет начальные данные для новых сущностей.</span><span class="sxs-lookup"><span data-stu-id="5cde8-390">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="5cde8-391">Основная часть кода создает объекты сущностей и загружает демонстрационные данные.</span><span class="sxs-lookup"><span data-stu-id="5cde8-391">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="5cde8-392">Демонстрационные данные используются для проверки.</span><span class="sxs-lookup"><span data-stu-id="5cde8-392">The sample data is used for testing.</span></span> <span data-ttu-id="5cde8-393">Приведенный выше код создает следующие связи многие ко многим:</span><span class="sxs-lookup"><span data-stu-id="5cde8-393">The preceding code creates the following many-to-many relationships:</span></span>

* `Enrollments`
* `CourseAssignment`

<span data-ttu-id="5cde8-394">Примечание. [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) будет поддерживать [заполнение данными](https://github.com/aspnet/EntityFrameworkCore/issues/629).</span><span class="sxs-lookup"><span data-stu-id="5cde8-394">Note: [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) will support [data seeding](https://github.com/aspnet/EntityFrameworkCore/issues/629).</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="5cde8-395">Добавление миграции</span><span class="sxs-lookup"><span data-stu-id="5cde8-395">Add a migration</span></span>

<span data-ttu-id="5cde8-396">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="5cde8-396">Build the project.</span></span> <span data-ttu-id="5cde8-397">Откройте командное окно в папке проекта и введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="5cde8-397">Open a command window in the project folder and enter the following command:</span></span>

```console
dotnet ef migrations add ComplexDataModel
```

<span data-ttu-id="5cde8-398">Предыдущая команда отображает предупреждение о возможной потере данных.</span><span class="sxs-lookup"><span data-stu-id="5cde8-398">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="5cde8-399">При выполнении команды `database update` возникает следующая ошибка:</span><span class="sxs-lookup"><span data-stu-id="5cde8-399">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

<span data-ttu-id="5cde8-400">При выполнении миграций с существующими данными могут действовать ограничения внешнего ключа, которым существующие данные не соответствуют.</span><span class="sxs-lookup"><span data-stu-id="5cde8-400">When migrations are run with existing data, there may be FK constraints that are not satisfied with the exiting data.</span></span> <span data-ttu-id="5cde8-401">В этом руководстве создается новая база данных, поэтому нарушения ограничений внешнего ключа отсутствуют.</span><span class="sxs-lookup"><span data-stu-id="5cde8-401">For this tutorial, a new DB is created, so there are no FK constraint violations.</span></span> <span data-ttu-id="5cde8-402">Инструкции по устранению нарушений внешнего ключа для текущей базы данных см. в разделе [Устранение ограничений внешнего ключа с устаревшими данными](#fk).</span><span class="sxs-lookup"><span data-stu-id="5cde8-402">See [Fixing foreign key constraints with legacy data](#fk) for instructions on how to fix the FK violations on the current DB.</span></span>

## <a name="change-the-connection-string-and-update-the-db"></a><span data-ttu-id="5cde8-403">Изменение строки подключения и обновление базы данных</span><span class="sxs-lookup"><span data-stu-id="5cde8-403">Change the connection string and update the DB</span></span>

<span data-ttu-id="5cde8-404">Код в обновленном `DbInitializer` предоставляет начальные данные для новых сущностей.</span><span class="sxs-lookup"><span data-stu-id="5cde8-404">The code in the updated `DbInitializer` adds seed data for the new entities.</span></span> <span data-ttu-id="5cde8-405">Чтобы велеть EF Core создать пустую базу данных, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="5cde8-405">To force EF Core to create a new empty DB:</span></span>

* <span data-ttu-id="5cde8-406">Измените имя строки подключения базы данных в файле *appsettings.json* для ContosoUniversity3.</span><span class="sxs-lookup"><span data-stu-id="5cde8-406">Change the DB connection string name in *appsettings.json* to ContosoUniversity3.</span></span> <span data-ttu-id="5cde8-407">Новое имя не должно использоваться на компьютере ранее.</span><span class="sxs-lookup"><span data-stu-id="5cde8-407">The new name must be a name that hasn't been used on the computer.</span></span>

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
      },
    ```

* <span data-ttu-id="5cde8-408">Кроме того, можно удалить базу данных с помощью:</span><span class="sxs-lookup"><span data-stu-id="5cde8-408">Alternatively, delete the DB using:</span></span>

  * <span data-ttu-id="5cde8-409">**обозревателя объектов SQL Server** (SSOX) или</span><span class="sxs-lookup"><span data-stu-id="5cde8-409">**SQL Server Object Explorer** (SSOX).</span></span>
  * <span data-ttu-id="5cde8-410">команды интерфейса командной строки `database drop`:</span><span class="sxs-lookup"><span data-stu-id="5cde8-410">The `database drop` CLI command:</span></span>

    ```console
    dotnet ef database drop
    ```

<span data-ttu-id="5cde8-411">Запустите `database update` в командном окне:</span><span class="sxs-lookup"><span data-stu-id="5cde8-411">Run `database update` in the command window:</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="5cde8-412">Предыдущая команда выполняет все миграции.</span><span class="sxs-lookup"><span data-stu-id="5cde8-412">The preceding command runs all the migrations.</span></span>

<span data-ttu-id="5cde8-413">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="5cde8-413">Run the app.</span></span> <span data-ttu-id="5cde8-414">При запуске приложения выполняется метод `DbInitializer.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-414">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="5cde8-415">`DbInitializer.Initialize` заполняет новую базу данных.</span><span class="sxs-lookup"><span data-stu-id="5cde8-415">The `DbInitializer.Initialize` populates the new DB.</span></span>

<span data-ttu-id="5cde8-416">Откройте базу данных в SSOX:</span><span class="sxs-lookup"><span data-stu-id="5cde8-416">Open the DB in SSOX:</span></span>

* <span data-ttu-id="5cde8-417">Разверните узел **Таблицы**.</span><span class="sxs-lookup"><span data-stu-id="5cde8-417">Expand the **Tables** node.</span></span> <span data-ttu-id="5cde8-418">Отображаются созданные таблицы.</span><span class="sxs-lookup"><span data-stu-id="5cde8-418">The created tables are displayed.</span></span>
* <span data-ttu-id="5cde8-419">Если SSOX был открыт ранее, нажмите кнопку **Обновить**.</span><span class="sxs-lookup"><span data-stu-id="5cde8-419">If SSOX was opened previously, click the **Refresh** button.</span></span>

![Таблицы в SSOX](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="5cde8-421">Изучите таблицу **CourseAssignment**:</span><span class="sxs-lookup"><span data-stu-id="5cde8-421">Examine the **CourseAssignment** table:</span></span>

* <span data-ttu-id="5cde8-422">Щелкните правой кнопкой мыши таблицу **CourseAssignment** и выберите пункт **Просмотреть данные**.</span><span class="sxs-lookup"><span data-stu-id="5cde8-422">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
* <span data-ttu-id="5cde8-423">Убедитесь, что таблица **CourseAssignment** содержит данные.</span><span class="sxs-lookup"><span data-stu-id="5cde8-423">Verify the **CourseAssignment** table contains data.</span></span>

![Данные CourseAssignment в SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="fk"></a>

## <a name="fixing-foreign-key-constraints-with-legacy-data"></a><span data-ttu-id="5cde8-425">Устранение ограничений внешнего ключа с устаревшими данными</span><span class="sxs-lookup"><span data-stu-id="5cde8-425">Fixing foreign key constraints with legacy data</span></span>

<span data-ttu-id="5cde8-426">Этот раздел является необязательным.</span><span class="sxs-lookup"><span data-stu-id="5cde8-426">This section is optional.</span></span>

<span data-ttu-id="5cde8-427">При выполнении миграций с существующими данными могут действовать ограничения внешнего ключа, которым существующие данные не соответствуют.</span><span class="sxs-lookup"><span data-stu-id="5cde8-427">When migrations are run with existing data, there may be FK constraints that are not satisfied with the exiting data.</span></span> <span data-ttu-id="5cde8-428">Для рабочих данных нужно предпринять меры по переносу существующих данных.</span><span class="sxs-lookup"><span data-stu-id="5cde8-428">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="5cde8-429">Этот раздел содержит пример того, как устранить нарушения ограничений внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="5cde8-429">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="5cde8-430">Вносите эти изменения кода только после создания резервной копии.</span><span class="sxs-lookup"><span data-stu-id="5cde8-430">Don't make these code changes without a backup.</span></span> <span data-ttu-id="5cde8-431">Не вносите эти изменения кода, если вы выполнили инструкции из предыдущего раздела и обновили базу данных.</span><span class="sxs-lookup"><span data-stu-id="5cde8-431">Don't make these code changes if you completed the previous section and updated the database.</span></span>

<span data-ttu-id="5cde8-432">Файл *{метка_времени}_ComplexDataModel.cs* содержит следующий код:</span><span class="sxs-lookup"><span data-stu-id="5cde8-432">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="5cde8-433">Предыдущий код добавляет в таблицу `Course` внешний ключ `DepartmentID`, не допускающий значение null.</span><span class="sxs-lookup"><span data-stu-id="5cde8-433">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="5cde8-434">База данных из предыдущего руководства содержит строки в `Course`, поэтому эту таблицу невозможно обновить с помощью миграций.</span><span class="sxs-lookup"><span data-stu-id="5cde8-434">The DB from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="5cde8-435">Чтобы заставить миграцию `ComplexDataModel` работать с существующими данными:</span><span class="sxs-lookup"><span data-stu-id="5cde8-435">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="5cde8-436">Изменить код, чтобы присвоить новому столбцу (`DepartmentID`) значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="5cde8-436">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="5cde8-437">Создайте фиктивную кафедру с именем "Temp" для использования по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="5cde8-437">Create a fake department named "Temp" to act as the default department.</span></span>

### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="5cde8-438">Устранение ограничений внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="5cde8-438">Fix the foreign key constraints</span></span>

<span data-ttu-id="5cde8-439">Обновите метод `Up` в классах `ComplexDataModel`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-439">Update the `ComplexDataModel` classes `Up` method:</span></span>

* <span data-ttu-id="5cde8-440">Откройте файл *{метка_времени}_ComplexDataModel.cs*.</span><span class="sxs-lookup"><span data-stu-id="5cde8-440">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="5cde8-441">Закомментируйте строку кода, которая добавляет столбец `DepartmentID` в таблицу `Course`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-441">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="5cde8-442">Добавьте выделенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="5cde8-442">Add the following highlighted code.</span></span> <span data-ttu-id="5cde8-443">Новый код идет после блока `.CreateTable( name: "Department"`: [!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span><span class="sxs-lookup"><span data-stu-id="5cde8-443">The new code goes after the `.CreateTable( name: "Department"` block: [!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span></span>

<span data-ttu-id="5cde8-444">После внесения описанных выше изменений существующие строки `Course` будут связаны с кафедрой "Temp" после выполнения метода `ComplexDataModel` `Up`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-444">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel` `Up` method runs.</span></span>

<span data-ttu-id="5cde8-445">Рабочее приложение:</span><span class="sxs-lookup"><span data-stu-id="5cde8-445">A production app would:</span></span>

* <span data-ttu-id="5cde8-446">включает код или сценарии, чтобы добавить строки `Department` и связанные строки `Course` к новым строкам `Department`;</span><span class="sxs-lookup"><span data-stu-id="5cde8-446">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="5cde8-447">не использует кафедру "Temp" или значение по умолчанию для `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="5cde8-447">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

<span data-ttu-id="5cde8-448">Следующее руководство посвящено связанным данным.</span><span class="sxs-lookup"><span data-stu-id="5cde8-448">The next tutorial covers related data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5cde8-449">[Назад](xref:data/ef-rp/migrations)
> [Вперед](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="5cde8-449">[Previous](xref:data/ef-rp/migrations)
[Next](xref:data/ef-rp/read-related-data)</span></span>
