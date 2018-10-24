---
title: Razor Pages с EF Core в ASP.NET Core — модель данных— 5 из 8
author: rick-anderson
description: В этом руководстве вы добавите дополнительные сущности и связи, а также настроите модель данных, указав правила форматирования, проверки и сопоставления.
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: b81918cbd74200f0672f3002f916523fb4a9a914
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477661"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---data-model---5-of-8"></a><span data-ttu-id="af47f-103">Razor Pages с EF Core в ASP.NET Core — модель данных— 5 из 8</span><span class="sxs-lookup"><span data-stu-id="af47f-103">Razor Pages with EF Core in ASP.NET Core - Data Model - 5 of 8</span></span>

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="af47f-104">Авторы: [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="af47f-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

<span data-ttu-id="af47f-105">Предыдущие руководства работали с базовой моделью данных, состоящей из трех сущностей.</span><span class="sxs-lookup"><span data-stu-id="af47f-105">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="af47f-106">В этом руководстве:</span><span class="sxs-lookup"><span data-stu-id="af47f-106">In this tutorial:</span></span>

* <span data-ttu-id="af47f-107">Добавляются дополнительные сущности и связи.</span><span class="sxs-lookup"><span data-stu-id="af47f-107">More entities and relationships are added.</span></span>
* <span data-ttu-id="af47f-108">Настраивается модель данных с помощью указания правил для форматирования, проверки и сопоставления базы данных.</span><span class="sxs-lookup"><span data-stu-id="af47f-108">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="af47f-109">Классы сущностей для готовой модели данных показаны на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="af47f-109">The entity classes for the completed data model is shown in the following illustration:</span></span>

![Схема сущностей](complex-data-model/_static/diagram.png)

<span data-ttu-id="af47f-111">При возникновении проблем, которые вам не удается устранить, скачайте [готовое приложение](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="af47f-111">If you run into problems you can't solve, download the [completed app](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span>

## <a name="customize-the-data-model-with-attributes"></a><span data-ttu-id="af47f-112">Настройка модели данных с использованием атрибутов</span><span class="sxs-lookup"><span data-stu-id="af47f-112">Customize the data model with attributes</span></span>

<span data-ttu-id="af47f-113">В этом разделе модель данных настраивается с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="af47f-113">In this section, the data model is customized using attributes.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="af47f-114">Атрибут DataType</span><span class="sxs-lookup"><span data-stu-id="af47f-114">The DataType attribute</span></span>

<span data-ttu-id="af47f-115">Страницы учащихся сейчас отображают время и дату зачисления.</span><span class="sxs-lookup"><span data-stu-id="af47f-115">The student pages currently displays the time of the enrollment date.</span></span> <span data-ttu-id="af47f-116">Как правило, поля даты отображают только дату без времени.</span><span class="sxs-lookup"><span data-stu-id="af47f-116">Typically, date fields show only the date and not the time.</span></span>

<span data-ttu-id="af47f-117">Обновите *Models/Student.cs*, используя следующий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="af47f-117">Update *Models/Student.cs* with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="af47f-118">Атрибут [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) указывает тип данных более точно по сравнению со встроенным типом базы данных.</span><span class="sxs-lookup"><span data-stu-id="af47f-118">The [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="af47f-119">В этом случае следует отображать отобразить только дату, а не дату и время.</span><span class="sxs-lookup"><span data-stu-id="af47f-119">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="af47f-120">В [перечислении DataType](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) представлено множество типов данных, таких как Date, Time, PhoneNumber, Currency, EmailAddress и других. Атрибут `DataType` также обеспечивает автоматическое предоставление функций для определенных типов в приложении.</span><span class="sxs-lookup"><span data-stu-id="af47f-120">The [DataType Enumeration](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="af47f-121">Пример:</span><span class="sxs-lookup"><span data-stu-id="af47f-121">For example:</span></span>

* <span data-ttu-id="af47f-122">Ссылка `mailto:` для `DataType.EmailAddress` создается автоматически.</span><span class="sxs-lookup"><span data-stu-id="af47f-122">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="af47f-123">Средство выбора даты предоставляется для `DataType.Date` в большинстве браузеров.</span><span class="sxs-lookup"><span data-stu-id="af47f-123">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="af47f-124">Атрибут `DataType` создает атрибуты HTML 5 `data-`, которые используются браузерами с поддержкой HTML 5.</span><span class="sxs-lookup"><span data-stu-id="af47f-124">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="af47f-125">Атрибуты `DataType` не предназначены для проверки.</span><span class="sxs-lookup"><span data-stu-id="af47f-125">The `DataType` attributes don't provide validation.</span></span>

<span data-ttu-id="af47f-126">`DataType.Date` не задает формат отображаемой даты.</span><span class="sxs-lookup"><span data-stu-id="af47f-126">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="af47f-127">По умолчанию поле даты отображается с использованием форматов, установленных в [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support) сервера.</span><span class="sxs-lookup"><span data-stu-id="af47f-127">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="af47f-128">С помощью атрибута `DisplayFormat` можно явно указать формат даты:</span><span class="sxs-lookup"><span data-stu-id="af47f-128">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="af47f-129">Параметр `ApplyFormatInEditMode` указывает, что формат должен применяться к пользовательскому интерфейсу редактирования.</span><span class="sxs-lookup"><span data-stu-id="af47f-129">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="af47f-130">Некоторым полям не следует использовать `ApplyFormatInEditMode`.</span><span class="sxs-lookup"><span data-stu-id="af47f-130">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="af47f-131">Например, обозначение денежной единицы в общем случае не должно отображаться в поле редактирования текста.</span><span class="sxs-lookup"><span data-stu-id="af47f-131">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="af47f-132">Атрибут `DisplayFormat` можно использовать отдельно.</span><span class="sxs-lookup"><span data-stu-id="af47f-132">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="af47f-133">Однако чаще всего `DataType` рекомендуется применять вместе с атрибутом `DisplayFormat`.</span><span class="sxs-lookup"><span data-stu-id="af47f-133">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="af47f-134">Атрибут `DataType` передает семантику данных (в отличие от способа их вывода на экран).</span><span class="sxs-lookup"><span data-stu-id="af47f-134">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="af47f-135">Атрибут `DataType` дает следующие преимущества, недоступные в `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="af47f-135">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="af47f-136">Поддержка функций HTML5 в браузере.</span><span class="sxs-lookup"><span data-stu-id="af47f-136">The browser can enable HTML5 features.</span></span> <span data-ttu-id="af47f-137">Например, отображение элемента управления календарем, соответствующего языковому стандарту обозначения денежной единицы, ссылок электронной почты, проверки входных данных на стороне клиента и т. д.</span><span class="sxs-lookup"><span data-stu-id="af47f-137">For example, show a calendar control, the locale-appropriate currency symbol, email links, client-side input validation, etc.</span></span>
* <span data-ttu-id="af47f-138">По умолчанию формат отображения данных в браузере определяется в соответствии с установленным языковым стандартом.</span><span class="sxs-lookup"><span data-stu-id="af47f-138">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="af47f-139">Дополнительные сведения см. в документации по [вспомогательной функции тегов \<input>](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="af47f-139">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

<span data-ttu-id="af47f-140">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="af47f-140">Run the app.</span></span> <span data-ttu-id="af47f-141">Перейдите на страницу индекса учащихся.</span><span class="sxs-lookup"><span data-stu-id="af47f-141">Navigate to the Students Index page.</span></span> <span data-ttu-id="af47f-142">Время больше не отображается.</span><span class="sxs-lookup"><span data-stu-id="af47f-142">Times are no longer displayed.</span></span> <span data-ttu-id="af47f-143">Каждое представление, использующее модель `Student`, отображает дату без времени.</span><span class="sxs-lookup"><span data-stu-id="af47f-143">Every view that uses the `Student` model displays the date without time.</span></span>

![Страница указателя учащихся с датами без времени](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="af47f-145">Атрибут StringLength</span><span class="sxs-lookup"><span data-stu-id="af47f-145">The StringLength attribute</span></span>

<span data-ttu-id="af47f-146">С помощью атрибутов можно указать правила проверки данных и сообщения об ошибках проверки.</span><span class="sxs-lookup"><span data-stu-id="af47f-146">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="af47f-147">Атрибут [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) указывает минимальную и максимальную длину символов, разрешенных в поле данных.</span><span class="sxs-lookup"><span data-stu-id="af47f-147">The [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="af47f-148">Атрибут `StringLength` также обеспечивает проверку на стороне клиента и на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="af47f-148">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="af47f-149">Минимальное значение не оказывает влияния на схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="af47f-149">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="af47f-150">Обновите модель`Student`, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="af47f-150">Update the `Student` model with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="af47f-151">Предыдущий код задает ограничение длины имен в 50 символов.</span><span class="sxs-lookup"><span data-stu-id="af47f-151">The preceding code limits names to no more than 50 characters.</span></span> <span data-ttu-id="af47f-152">Атрибут `StringLength` не запрещает пользователю ввести пробел в качестве имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="af47f-152">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="af47f-153">Атрибут [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) используется для применения ограничений к входным данным.</span><span class="sxs-lookup"><span data-stu-id="af47f-153">The [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute is used to apply restrictions to the input.</span></span> <span data-ttu-id="af47f-154">Например, следующий код требует, чтобы первый символ был прописной буквой, а остальные символы были буквенными:</span><span class="sxs-lookup"><span data-stu-id="af47f-154">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

<span data-ttu-id="af47f-155">Запустите приложение:</span><span class="sxs-lookup"><span data-stu-id="af47f-155">Run the app:</span></span>

* <span data-ttu-id="af47f-156">Перейдите на страницу учащихся.</span><span class="sxs-lookup"><span data-stu-id="af47f-156">Navigate to the Students page.</span></span>
* <span data-ttu-id="af47f-157">Выберите **Create New** (Создать) и введите имя длиной более 50 символов.</span><span class="sxs-lookup"><span data-stu-id="af47f-157">Select **Create New**, and enter a name longer than 50 characters.</span></span>
* <span data-ttu-id="af47f-158">Выберите **Create** (Создать), проверка на стороне клиента отображает сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="af47f-158">Select **Create**, client-side validation shows an error message.</span></span>

![Страница указателя учащихся с ошибками длины строки](complex-data-model/_static/string-length-errors.png)

<span data-ttu-id="af47f-160">В **обозревателе объектов SQL Server** (SSOX) откройте конструктор таблиц учащихся, дважды щелкнув таблицу **Student** (Учащийся).</span><span class="sxs-lookup"><span data-stu-id="af47f-160">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Таблица учащихся в SSOX до миграций](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="af47f-162">На предыдущем изображении показана схемы для таблицы `Student`.</span><span class="sxs-lookup"><span data-stu-id="af47f-162">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="af47f-163">Поля имен имеют тип `nvarchar(MAX)`, так как миграции для базы данных не выполнялись.</span><span class="sxs-lookup"><span data-stu-id="af47f-163">The name fields have type `nvarchar(MAX)` because migrations has not been run on the DB.</span></span> <span data-ttu-id="af47f-164">Когда в дальнейшем миграции будут выполнены, поля имен станут `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="af47f-164">When migrations are run later in this tutorial, the name fields become `nvarchar(50)`.</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="af47f-165">Атрибут Column</span><span class="sxs-lookup"><span data-stu-id="af47f-165">The Column attribute</span></span>

<span data-ttu-id="af47f-166">Атрибуты могут управлять, как именно классы и свойства сопоставляются с базой данных.</span><span class="sxs-lookup"><span data-stu-id="af47f-166">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="af47f-167">В этом разделе атрибут `Column` используется для сопоставления имени свойства `FirstMidName` с "FirstName" в базе данных.</span><span class="sxs-lookup"><span data-stu-id="af47f-167">In this section, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the DB.</span></span>

<span data-ttu-id="af47f-168">При создании базы данных имена свойств в модели используются для имен столбцов (кроме случая, когда используется атрибут `Column`).</span><span class="sxs-lookup"><span data-stu-id="af47f-168">When the DB is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span>

<span data-ttu-id="af47f-169">Модель `Student` использует `FirstMidName` для поля имени, так как это поле также может содержать отчество.</span><span class="sxs-lookup"><span data-stu-id="af47f-169">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="af47f-170">Обновите файл *Student.cs*, используя следующий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="af47f-170">Update the *Student.cs* file with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="af47f-171">С учетом предыдущего изменения `Student.FirstMidName` в приложении сопоставляется со столбцом `FirstName` таблицы `Student`.</span><span class="sxs-lookup"><span data-stu-id="af47f-171">With the preceding change, `Student.FirstMidName` in the app maps to the `FirstName` column of the `Student` table.</span></span>

<span data-ttu-id="af47f-172">Добавление атрибута `Column` изменяет модель для поддержки `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="af47f-172">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="af47f-173">Модель, поддерживающая `SchoolContext`, больше не соответствует базе данных.</span><span class="sxs-lookup"><span data-stu-id="af47f-173">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="af47f-174">Если приложение запускается перед применением миграций, возникает следующее исключение:</span><span class="sxs-lookup"><span data-stu-id="af47f-174">If the app is run before applying migrations, the following exception is generated:</span></span>

```SQL
SqlException: Invalid column name 'FirstName'.
```
<span data-ttu-id="af47f-175">Для обновления базы данных сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="af47f-175">To update the DB:</span></span>

* <span data-ttu-id="af47f-176">Выполните сборку проекта.</span><span class="sxs-lookup"><span data-stu-id="af47f-176">Build the project.</span></span>
* <span data-ttu-id="af47f-177">Откройте командное окно в папке проекта.</span><span class="sxs-lookup"><span data-stu-id="af47f-177">Open a command window in the project folder.</span></span> <span data-ttu-id="af47f-178">Введите следующие команды для создания миграции и обновления базы данных:</span><span class="sxs-lookup"><span data-stu-id="af47f-178">Enter the following commands to create a new migration and update the DB:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="af47f-179">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="af47f-179">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration ColumnFirstName
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="af47f-180">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="af47f-180">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations add ColumnFirstName
dotnet ef database update
```

------

<span data-ttu-id="af47f-181">Команда `migrations add ColumnFirstName` выдает следующее предупреждающее сообщение:</span><span class="sxs-lookup"><span data-stu-id="af47f-181">The `migrations add ColumnFirstName` command generates the following warning message:</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

<span data-ttu-id="af47f-182">Это предупреждение вызвано тем, что поля имен теперь ограничены 50 символами.</span><span class="sxs-lookup"><span data-stu-id="af47f-182">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="af47f-183">Если имя в базе данных имеет больше 50 символов, символы с 51 до последнего будут потеряны.</span><span class="sxs-lookup"><span data-stu-id="af47f-183">If a name in the DB had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="af47f-184">Проверьте работу приложения.</span><span class="sxs-lookup"><span data-stu-id="af47f-184">Test the app.</span></span>

<span data-ttu-id="af47f-185">Откройте таблицу "Student" (Учащийся) в окне SSOX:</span><span class="sxs-lookup"><span data-stu-id="af47f-185">Open the Student table in SSOX:</span></span>

![Таблица учащихся в SSOX после миграций](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="af47f-187">До применения миграции столбцы имен имели тип [nvarchar(MAX)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="af47f-187">Before migration was applied, the name columns were of type [nvarchar(MAX)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="af47f-188">Теперь столбцы имен имеют тип `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="af47f-188">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="af47f-189">Имя столбца изменилось с `FirstMidName` на `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="af47f-189">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

> [!Note]
> <span data-ttu-id="af47f-190">В следующем разделе сборка приложения на некоторых этапах приводит к возникновению ошибок компилятора.</span><span class="sxs-lookup"><span data-stu-id="af47f-190">In the following section, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="af47f-191">В инструкциях указано, когда производить сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="af47f-191">The instructions specify when to build the app.</span></span>

## <a name="student-entity-update"></a><span data-ttu-id="af47f-192">Обновление сущности Student</span><span class="sxs-lookup"><span data-stu-id="af47f-192">Student entity update</span></span>

![Сущность Student](complex-data-model/_static/student-entity.png)

<span data-ttu-id="af47f-194">Обновите *Models/Student.cs*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="af47f-194">Update *Models/Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="af47f-195">Атрибут Required</span><span class="sxs-lookup"><span data-stu-id="af47f-195">The Required attribute</span></span>

<span data-ttu-id="af47f-196">Атрибут `Required` делает свойства имен обязательными полями.</span><span class="sxs-lookup"><span data-stu-id="af47f-196">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="af47f-197">Атрибут `Required` не нужен для типов, не допускающих значения null, например для типов значений (`DateTime`, `int`, `double` и т. д.).</span><span class="sxs-lookup"><span data-stu-id="af47f-197">The `Required` attribute isn't needed for non-nullable types such as value types (`DateTime`, `int`, `double`, etc.).</span></span> <span data-ttu-id="af47f-198">Типы, которые не могут принимать значение null, автоматически обрабатываются как обязательные поля.</span><span class="sxs-lookup"><span data-stu-id="af47f-198">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="af47f-199">Атрибут `Required` можно заменить параметром минимальной длины в атрибуте `StringLength`:</span><span class="sxs-lookup"><span data-stu-id="af47f-199">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="af47f-200">Атрибут Display</span><span class="sxs-lookup"><span data-stu-id="af47f-200">The Display attribute</span></span>

<span data-ttu-id="af47f-201">Атрибут `Display` указывает, что заголовки для текстовых полей должны иметь вид "First Name" (Имя), "Last Name" (Фамилия), "Full Name" (Полное имя) и "Enrollment Date" (Дата зачисления).</span><span class="sxs-lookup"><span data-stu-id="af47f-201">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="af47f-202">По умолчанию в заголовках не использовался пробел для разделения слов, например "Lastname".</span><span class="sxs-lookup"><span data-stu-id="af47f-202">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="af47f-203">Вычисляемое свойство FullName</span><span class="sxs-lookup"><span data-stu-id="af47f-203">The FullName calculated property</span></span>

<span data-ttu-id="af47f-204">`FullName` — это вычисляемое свойство, которое возвращает значение, созданное путем объединения двух других свойств.</span><span class="sxs-lookup"><span data-stu-id="af47f-204">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="af47f-205">`FullName` не может быть задано, оно имеет только метод доступа get.</span><span class="sxs-lookup"><span data-stu-id="af47f-205">`FullName` cannot be set, it has only a get accessor.</span></span> <span data-ttu-id="af47f-206">В базе данных не создается столбец `FullName`.</span><span class="sxs-lookup"><span data-stu-id="af47f-206">No `FullName` column is created in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="af47f-207">Создание сущности Instructor</span><span class="sxs-lookup"><span data-stu-id="af47f-207">Create the Instructor Entity</span></span>

![Сущность Instructor](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="af47f-209">Создайте файл *Models/Instructor.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="af47f-209">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Instructor.cs)]

<span data-ttu-id="af47f-210">На одной строке могут находиться несколько атрибутов.</span><span class="sxs-lookup"><span data-stu-id="af47f-210">Multiple attributes can be on one line.</span></span> <span data-ttu-id="af47f-211">Атрибуты `HireDate` можно записать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="af47f-211">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="af47f-212">Свойства навигации CourseAssignments и OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="af47f-212">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="af47f-213">`CourseAssignments` и `OfficeAssignment` — это свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="af47f-213">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="af47f-214">Преподаватель может проводить любое количество курсов, поэтому `CourseAssignments` определен как коллекция.</span><span class="sxs-lookup"><span data-stu-id="af47f-214">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="af47f-215">Если свойство навигации содержит несколько сущностей:</span><span class="sxs-lookup"><span data-stu-id="af47f-215">If a navigation property holds multiple entities:</span></span>

* <span data-ttu-id="af47f-216">Оно должно быть типом списка, где можно добавлять, удалять и обновлять записи.</span><span class="sxs-lookup"><span data-stu-id="af47f-216">It must be a list type where the entries can be added, deleted, and updated.</span></span>

<span data-ttu-id="af47f-217">К типам свойств навигации относятся следующие:</span><span class="sxs-lookup"><span data-stu-id="af47f-217">Navigation property types include:</span></span>

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

<span data-ttu-id="af47f-218">Если указан тип `ICollection<T>`, платформа EF Core по умолчанию создает коллекцию `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="af47f-218">If `ICollection<T>` is specified, EF Core creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="af47f-219">Сущность `CourseAssignment` описана в разделе по связям многие ко многим.</span><span class="sxs-lookup"><span data-stu-id="af47f-219">The `CourseAssignment` entity is explained in the section on many-to-many relationships.</span></span>

<span data-ttu-id="af47f-220">Бизнес-правила университета Contoso указывают, что преподаватель может иметь не более одного кабинета.</span><span class="sxs-lookup"><span data-stu-id="af47f-220">Contoso University business rules state that an instructor can have at most one office.</span></span> <span data-ttu-id="af47f-221">Свойство `OfficeAssignment` содержит отдельную сущность `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="af47f-221">The `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="af47f-222">`OfficeAssignment` имеет значение null, если кабинет не назначен.</span><span class="sxs-lookup"><span data-stu-id="af47f-222">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="af47f-223">Создание сущности OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="af47f-223">Create the OfficeAssignment entity</span></span>

![Сущность OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="af47f-225">Создайте файл *Models/OfficeAssignment.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="af47f-225">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="af47f-226">Атрибут Key</span><span class="sxs-lookup"><span data-stu-id="af47f-226">The Key attribute</span></span>

<span data-ttu-id="af47f-227">Атрибут `[Key]` используется для идентификации свойства в качестве первичного ключа (PK), когда имя свойства отличается от classnameID или ID.</span><span class="sxs-lookup"><span data-stu-id="af47f-227">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="af47f-228">Между сущностями `Instructor` и `OfficeAssignment` действует связь один к нулю или к одному.</span><span class="sxs-lookup"><span data-stu-id="af47f-228">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="af47f-229">Назначение кабинета существует только в связи с преподавателем, которому оно назначено.</span><span class="sxs-lookup"><span data-stu-id="af47f-229">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="af47f-230">Первичный ключ `OfficeAssignment` также является внешним ключом (FK) для сущности `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="af47f-230">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span> <span data-ttu-id="af47f-231">EF Core не распознает автоматически `InstructorID` как первичный ключ объекта `OfficeAssignment` по следующей причине:</span><span class="sxs-lookup"><span data-stu-id="af47f-231">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because:</span></span>

* <span data-ttu-id="af47f-232">`InstructorID` не соблюдает соглашение об именовании ID или classnameID.</span><span class="sxs-lookup"><span data-stu-id="af47f-232">`InstructorID` doesn't follow the ID or classnameID naming convention.</span></span>

<span data-ttu-id="af47f-233">Таким образом, атрибут `Key` используется для определения `InstructorID` в качестве первичного ключа:</span><span class="sxs-lookup"><span data-stu-id="af47f-233">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="af47f-234">По умолчанию EF Core считает ключ созданным не базой данных, так как столбец предназначен для идентифицирующего отношения.</span><span class="sxs-lookup"><span data-stu-id="af47f-234">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="af47f-235">Свойство навигации Instructor</span><span class="sxs-lookup"><span data-stu-id="af47f-235">The Instructor navigation property</span></span>

<span data-ttu-id="af47f-236">Свойство навигации `OfficeAssignment` для сущности `Instructor` допускает значение null по следующим причинам:</span><span class="sxs-lookup"><span data-stu-id="af47f-236">The `OfficeAssignment` navigation property for the `Instructor` entity is nullable because:</span></span>

* <span data-ttu-id="af47f-237">Ссылочные типы (например, классы) допускают значение null.</span><span class="sxs-lookup"><span data-stu-id="af47f-237">Reference types (such as classes are nullable).</span></span>
* <span data-ttu-id="af47f-238">Преподавателю может быть не назначен кабинет.</span><span class="sxs-lookup"><span data-stu-id="af47f-238">An instructor might not have an office assignment.</span></span>


<span data-ttu-id="af47f-239">Сущность `OfficeAssignment` имеет свойство навигации `Instructor`, не допускающее значение null, по следующим причинам:</span><span class="sxs-lookup"><span data-stu-id="af47f-239">The `OfficeAssignment` entity has a non-nullable `Instructor` navigation property because:</span></span>

* <span data-ttu-id="af47f-240">`InstructorID` не допускает значение null.</span><span class="sxs-lookup"><span data-stu-id="af47f-240">`InstructorID` is non-nullable.</span></span>
* <span data-ttu-id="af47f-241">Назначение кабинета не может существовать без преподавателя.</span><span class="sxs-lookup"><span data-stu-id="af47f-241">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="af47f-242">Когда сущность `Instructor` имеет связанную сущность `OfficeAssignment`, каждая из них имеет ссылку на другую в своем свойстве навигации.</span><span class="sxs-lookup"><span data-stu-id="af47f-242">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="af47f-243">Атрибут `[Required]` можно применить к свойству навигации `Instructor`:</span><span class="sxs-lookup"><span data-stu-id="af47f-243">The `[Required]` attribute could be applied to the `Instructor` navigation property:</span></span>

```csharp
[Required]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="af47f-244">Предыдущий код указывает, что должен существовать связанный преподаватель.</span><span class="sxs-lookup"><span data-stu-id="af47f-244">The preceding code specifies that there must be a related instructor.</span></span> <span data-ttu-id="af47f-245">Этот код не нужен, так как внешний ключ `InstructorID` (который также является первичным) не допускает значение null.</span><span class="sxs-lookup"><span data-stu-id="af47f-245">The preceding code is unnecessary because the `InstructorID` foreign key (which is also the PK) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="af47f-246">Изменение сущности Course</span><span class="sxs-lookup"><span data-stu-id="af47f-246">Modify the Course Entity</span></span>

![Сущность Course](complex-data-model/_static/course-entity.png)

<span data-ttu-id="af47f-248">Обновите *Models/Course.cs*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="af47f-248">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="af47f-249">Сущность `Course` имеет свойство внешнего ключа (FK) `DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="af47f-249">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="af47f-250">`DepartmentID` указывает на связанную сущность `Department`.</span><span class="sxs-lookup"><span data-stu-id="af47f-250">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="af47f-251">Сущность `Course` имеет свойство навигации `Department`.</span><span class="sxs-lookup"><span data-stu-id="af47f-251">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="af47f-252">EF Core не требует свойство внешнего ключа для модели данных, если она имеет свойство навигации для связанной сущности.</span><span class="sxs-lookup"><span data-stu-id="af47f-252">EF Core doesn't require a FK property for a data model when the model has a navigation property for a related entity.</span></span>

<span data-ttu-id="af47f-253">EF Core автоматически создает внешние ключи в базе данных по мере необходимости.</span><span class="sxs-lookup"><span data-stu-id="af47f-253">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="af47f-254">EF Core создает [теневые свойства](https://docs.microsoft.com/ef/core/modeling/shadow-properties) для автоматически созданных внешних ключей.</span><span class="sxs-lookup"><span data-stu-id="af47f-254">EF Core creates [shadow properties](https://docs.microsoft.com/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="af47f-255">Наличие внешнего ключа в модели данных позволяет сделать обновления проще и эффективнее.</span><span class="sxs-lookup"><span data-stu-id="af47f-255">Having the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="af47f-256">Например, рассмотрим модель, где свойство внешнего ключа `DepartmentID` *не* включено.</span><span class="sxs-lookup"><span data-stu-id="af47f-256">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="af47f-257">При получении сущности курса для редактирования:</span><span class="sxs-lookup"><span data-stu-id="af47f-257">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="af47f-258">сущность `Department` имеет значение null, если она не загружена явно;</span><span class="sxs-lookup"><span data-stu-id="af47f-258">The `Department` entity is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="af47f-259">для обновления сущности курса нужно сначала получить сущность `Department`.</span><span class="sxs-lookup"><span data-stu-id="af47f-259">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="af47f-260">Если свойство внешнего ключа `DepartmentID` включено в модель данных, получать сущность `Department` перед обновлением не нужно.</span><span class="sxs-lookup"><span data-stu-id="af47f-260">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="af47f-261">Атрибут DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="af47f-261">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="af47f-262">Атрибут `[DatabaseGenerated(DatabaseGeneratedOption.None)]` указывает, что первичный ключ предоставляется приложением, а не создается базой данных.</span><span class="sxs-lookup"><span data-stu-id="af47f-262">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="af47f-263">По умолчанию EF Core предполагает, что значения первичного ключа создаются базой данных.</span><span class="sxs-lookup"><span data-stu-id="af47f-263">By default, EF Core assumes that PK values are generated by the DB.</span></span> <span data-ttu-id="af47f-264">Обычно лучше всего использовать значения первичного ключа, созданные базой данных.</span><span class="sxs-lookup"><span data-stu-id="af47f-264">DB generated PK values is generally the best approach.</span></span> <span data-ttu-id="af47f-265">Для сущностей `Course` пользователь указывает первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="af47f-265">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="af47f-266">Например, номер курса, такой как серия 1000 для кафедры математики и серия 2000 для кафедры английского языка.</span><span class="sxs-lookup"><span data-stu-id="af47f-266">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="af47f-267">Атрибут `DatabaseGenerated` также может использоваться для создания значений по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="af47f-267">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="af47f-268">Например, база данных может автоматически создать поле даты для записи даты, когда строка была создана или изменена.</span><span class="sxs-lookup"><span data-stu-id="af47f-268">For example, the DB can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="af47f-269">Дополнительные сведения см. в разделе [Созданные свойства](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="af47f-269">For more information, see [Generated Properties](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="af47f-270">Свойства внешнего ключа и навигации</span><span class="sxs-lookup"><span data-stu-id="af47f-270">Foreign key and navigation properties</span></span>

<span data-ttu-id="af47f-271">Свойства внешнего ключа (FK) и свойства навигации в сущности `Course` отражают следующие связи:</span><span class="sxs-lookup"><span data-stu-id="af47f-271">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="af47f-272">Курс назначается одной кафедре, поэтому имеется внешний ключ `DepartmentID` и свойство навигации `Department`.</span><span class="sxs-lookup"><span data-stu-id="af47f-272">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="af47f-273">На курс может быть зачислено любое количество учащихся, поэтому свойство навигации `Enrollments` является коллекцией:</span><span class="sxs-lookup"><span data-stu-id="af47f-273">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="af47f-274">Курс могут вести несколько преподавателей, поэтому свойство навигации `CourseAssignments` является коллекцией:</span><span class="sxs-lookup"><span data-stu-id="af47f-274">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="af47f-275">`CourseAssignment` описано [далее](#many-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="af47f-275">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="create-the-department-entity"></a><span data-ttu-id="af47f-276">Создание сущности Department</span><span class="sxs-lookup"><span data-stu-id="af47f-276">Create the Department entity</span></span>

![Сущность Department](complex-data-model/_static/department-entity.png)

<span data-ttu-id="af47f-278">Создайте файл *Models/Department.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="af47f-278">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="af47f-279">Атрибут Column</span><span class="sxs-lookup"><span data-stu-id="af47f-279">The Column attribute</span></span>

<span data-ttu-id="af47f-280">Ранее атрибут `Column` использовался, чтобы изменить сопоставление имени столбца.</span><span class="sxs-lookup"><span data-stu-id="af47f-280">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="af47f-281">В коде для сущности `Department` атрибут `Column` можно использовать, чтобы изменить сопоставление типов данных SQL.</span><span class="sxs-lookup"><span data-stu-id="af47f-281">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="af47f-282">Столбец `Budget` определяется с помощью типа money SQL Server в базе данных:</span><span class="sxs-lookup"><span data-stu-id="af47f-282">The `Budget` column is defined using the SQL Server money type in the DB:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="af47f-283">Сопоставление столбцов обычно не требуется.</span><span class="sxs-lookup"><span data-stu-id="af47f-283">Column mapping is generally not required.</span></span> <span data-ttu-id="af47f-284">В общем случае EF Core выбирает соответствующий тип данных SQL Server на основе типа среды CLR для свойства.</span><span class="sxs-lookup"><span data-stu-id="af47f-284">EF Core generally chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="af47f-285">Тип `decimal` среды CLR сопоставляется с типом `decimal` SQL Server.</span><span class="sxs-lookup"><span data-stu-id="af47f-285">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="af47f-286">`Budget` используется для денежных единиц, хотя для этого лучше подходит тип данных money.</span><span class="sxs-lookup"><span data-stu-id="af47f-286">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="af47f-287">Свойства внешнего ключа и навигации</span><span class="sxs-lookup"><span data-stu-id="af47f-287">Foreign key and navigation properties</span></span>

<span data-ttu-id="af47f-288">Свойства внешнего ключа и навигации отражают следующие связи:</span><span class="sxs-lookup"><span data-stu-id="af47f-288">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="af47f-289">Кафедра может иметь или не иметь администратора.</span><span class="sxs-lookup"><span data-stu-id="af47f-289">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="af47f-290">Администратор всегда является преподавателем.</span><span class="sxs-lookup"><span data-stu-id="af47f-290">An administrator is always an instructor.</span></span> <span data-ttu-id="af47f-291">Поэтому свойство `InstructorID` включается в качестве внешнего ключа в сущность `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="af47f-291">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="af47f-292">Свойство навигации называется `Administrator`, но содержит сущность `Instructor`:</span><span class="sxs-lookup"><span data-stu-id="af47f-292">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="af47f-293">Вопросительный знак (?) в приведенном выше коде указывает, что свойство допускает значение null.</span><span class="sxs-lookup"><span data-stu-id="af47f-293">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="af47f-294">Кафедра может иметь несколько курсов, поэтому доступно свойство навигации Courses:</span><span class="sxs-lookup"><span data-stu-id="af47f-294">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="af47f-295">Примечание. По соглашению EF Core разрешает каскадное удаление для внешних ключей, не допускающих значение null, и связей многие ко многим.</span><span class="sxs-lookup"><span data-stu-id="af47f-295">Note: By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="af47f-296">Каскадное удаление может привести к циклическим правилам каскадного удаления.</span><span class="sxs-lookup"><span data-stu-id="af47f-296">Cascading delete can result in circular cascade delete rules.</span></span> <span data-ttu-id="af47f-297">Такие правила вызывают исключение при добавлении миграции.</span><span class="sxs-lookup"><span data-stu-id="af47f-297">Circular cascade delete rules causes an exception when a migration is added.</span></span>

<span data-ttu-id="af47f-298">Например, если свойство `Department.InstructorID` не было определено как допускающее значение null:</span><span class="sxs-lookup"><span data-stu-id="af47f-298">For example, if the `Department.InstructorID` property wasn't defined as nullable:</span></span>

* <span data-ttu-id="af47f-299">EF Core настраивает правило каскадного удаления для удаления преподавателя при удалении кафедры.</span><span class="sxs-lookup"><span data-stu-id="af47f-299">EF Core configures a cascade delete rule to delete the instructor when the department is deleted.</span></span>
* <span data-ttu-id="af47f-300">Удаление преподавателя при удалении кафедры не является запланированным поведением.</span><span class="sxs-lookup"><span data-stu-id="af47f-300">Deleting the instructor when the department is deleted isn't the intended behavior.</span></span>

<span data-ttu-id="af47f-301">Если бизнес-правила требуют, чтобы свойство `InstructorID` не допускало значение null, используйте следующий оператор текучего API:</span><span class="sxs-lookup"><span data-stu-id="af47f-301">If business rules required the `InstructorID` property be non-nullable, use the following fluent API statement:</span></span>

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

<span data-ttu-id="af47f-302">Предыдущий код отключает каскадное удаление для связи кафедры и преподавателя.</span><span class="sxs-lookup"><span data-stu-id="af47f-302">The preceding code disables cascade delete on the department-instructor relationship.</span></span>

## <a name="update-the-enrollment-entity"></a><span data-ttu-id="af47f-303">Обновление сущности Enrollment</span><span class="sxs-lookup"><span data-stu-id="af47f-303">Update the Enrollment entity</span></span>

<span data-ttu-id="af47f-304">Запись зачисления обозначает один курс, который проходит один учащийся.</span><span class="sxs-lookup"><span data-stu-id="af47f-304">An enrollment record is for one course taken by one student.</span></span>

![Сущность Enrollment](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="af47f-306">Обновите *Models/Enrollment.cs*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="af47f-306">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="af47f-307">Свойства внешнего ключа и навигации</span><span class="sxs-lookup"><span data-stu-id="af47f-307">Foreign key and navigation properties</span></span>

<span data-ttu-id="af47f-308">Свойства внешнего ключа и навигации отражают следующие связи:</span><span class="sxs-lookup"><span data-stu-id="af47f-308">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="af47f-309">Запись зачисления предназначена для одного курса, поэтому доступно свойство первичного ключа `CourseID` и свойство навигации `Course`:</span><span class="sxs-lookup"><span data-stu-id="af47f-309">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="af47f-310">Запись зачисления предназначена для одного учащегося, поэтому доступно свойство первичного ключа `StudentID` и свойство навигации `Student`:</span><span class="sxs-lookup"><span data-stu-id="af47f-310">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="af47f-311">Связи многие ко многим</span><span class="sxs-lookup"><span data-stu-id="af47f-311">Many-to-Many Relationships</span></span>

<span data-ttu-id="af47f-312">Между сущностями `Student` и `Course` действует связь многие ко многим.</span><span class="sxs-lookup"><span data-stu-id="af47f-312">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="af47f-313">Сущности `Enrollment` выступают в качестве таблицы соединения многие ко многим *с полезными данными* в базе данных.</span><span class="sxs-lookup"><span data-stu-id="af47f-313">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="af47f-314">Фраза "с полезными данными" означает, что таблица `Enrollment` содержит дополнительные данные, кроме внешних ключей для присоединяемых таблиц (в данном случае — первичный ключ и `Grade`).</span><span class="sxs-lookup"><span data-stu-id="af47f-314">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="af47f-315">На следующем рисунке показано, как выглядят эти связи на схеме сущностей.</span><span class="sxs-lookup"><span data-stu-id="af47f-315">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="af47f-316">(Эта схема была создана с помощью [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) для EF 6.x.</span><span class="sxs-lookup"><span data-stu-id="af47f-316">(This diagram was generated using [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) for EF 6.x.</span></span> <span data-ttu-id="af47f-317">Создание схемы не является частью руководства.)</span><span class="sxs-lookup"><span data-stu-id="af47f-317">Creating the diagram isn't part of the tutorial.)</span></span>

![Связь многие ко многим между Student и Course](complex-data-model/_static/student-course.png)

<span data-ttu-id="af47f-319">Каждая линия связи имеет 1 на одном конце и звездочку (\*) на другом, указывая характер один ко многим.</span><span class="sxs-lookup"><span data-stu-id="af47f-319">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="af47f-320">Если таблица `Enrollment` не включала в себя сведения об оценках, ей потребуется содержать всего два внешних ключа (`CourseID` и `StudentID`).</span><span class="sxs-lookup"><span data-stu-id="af47f-320">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="af47f-321">Таблицу соединения многие ко многим без полезных данных иногда называют чистой таблицей соединения (PJT).</span><span class="sxs-lookup"><span data-stu-id="af47f-321">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="af47f-322">Сущности `Instructor` и `Course` имеют связь многие ко многим с использованием чистой таблицы соединения.</span><span class="sxs-lookup"><span data-stu-id="af47f-322">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="af47f-323">Примечание. EF 6.x поддерживает неявные таблицы соединения для связей многие ко многим, но EF Core — нет.</span><span class="sxs-lookup"><span data-stu-id="af47f-323">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="af47f-324">Дополнительные сведения см. в статье [Связи многие ко многим в EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span><span class="sxs-lookup"><span data-stu-id="af47f-324">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="af47f-325">Сущность CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="af47f-325">The CourseAssignment entity</span></span>

![Сущность CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="af47f-327">Создайте файл *Models/CourseAssignment.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="af47f-327">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a><span data-ttu-id="af47f-328">Связь между Instructor и Courses</span><span class="sxs-lookup"><span data-stu-id="af47f-328">Instructor-to-Courses</span></span>

![Связь многие ко многим между Instructor и Courses](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="af47f-330">Связь многие ко многим между Instructor и Courses:</span><span class="sxs-lookup"><span data-stu-id="af47f-330">The Instructor-to-Courses many-to-many relationship:</span></span>

* <span data-ttu-id="af47f-331">Нуждается в таблице соединения, которая должна быть представлена набором сущностей.</span><span class="sxs-lookup"><span data-stu-id="af47f-331">Requires a join table that must be represented by an entity set.</span></span>
* <span data-ttu-id="af47f-332">Является чистой таблицей соединения (таблицей без полезных данных).</span><span class="sxs-lookup"><span data-stu-id="af47f-332">Is a pure join table (table without payload).</span></span>

<span data-ttu-id="af47f-333">Обычно для сущности соединения используется имя `EntityName1EntityName2`.</span><span class="sxs-lookup"><span data-stu-id="af47f-333">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="af47f-334">Например, таблицей соединения Instructor и Courses, использующей этот шаблон, является `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="af47f-334">For example, the Instructor-to-Courses join table using this pattern is `CourseInstructor`.</span></span> <span data-ttu-id="af47f-335">Однако рекомендуется использовать имя, которое описывает эту связь.</span><span class="sxs-lookup"><span data-stu-id="af47f-335">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="af47f-336">Модели данных создаются простыми и разрастаются.</span><span class="sxs-lookup"><span data-stu-id="af47f-336">Data models start out simple and grow.</span></span> <span data-ttu-id="af47f-337">Соединения без полезных данных (PJT) часто начинают включать их.</span><span class="sxs-lookup"><span data-stu-id="af47f-337">No-payload joins (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="af47f-338">Если в начале задать описательное имя сущности, его не нужно менять при изменениях таблицы соединения.</span><span class="sxs-lookup"><span data-stu-id="af47f-338">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="af47f-339">Оптимально, если сущность соединения имеет собственное естественное имя (возможно, из одного слова) в бизнес-среде.</span><span class="sxs-lookup"><span data-stu-id="af47f-339">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="af47f-340">Например, Books и Customers можно связать с сущностью соединения Ratings.</span><span class="sxs-lookup"><span data-stu-id="af47f-340">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="af47f-341">Связь многие ко многим между Instructor и Courses `CourseAssignment` является более предпочтительным вариантом, чем `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="af47f-341">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="af47f-342">Составной ключ</span><span class="sxs-lookup"><span data-stu-id="af47f-342">Composite key</span></span>

<span data-ttu-id="af47f-343">Внешние ключи не допускают значение null.</span><span class="sxs-lookup"><span data-stu-id="af47f-343">FKs are not nullable.</span></span> <span data-ttu-id="af47f-344">Два внешних ключа в `CourseAssignment` (`InstructorID` и `CourseID`) совместно однозначно определяют каждую строку таблицы `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="af47f-344">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="af47f-345">`CourseAssignment` не требуется выделенный первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="af47f-345">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="af47f-346">Свойства `InstructorID` и `CourseID` выступают в качестве составного первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="af47f-346">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="af47f-347">Единственным способом указать составные первичные ключи для EF Core является *текучий API*.</span><span class="sxs-lookup"><span data-stu-id="af47f-347">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="af47f-348">Следующий раздел описывает, как настроить составной первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="af47f-348">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="af47f-349">Составной ключ обеспечивает следующее:</span><span class="sxs-lookup"><span data-stu-id="af47f-349">The composite key ensures:</span></span>

* <span data-ttu-id="af47f-350">Для одного курса допускается несколько строк.</span><span class="sxs-lookup"><span data-stu-id="af47f-350">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="af47f-351">Для одного преподавателя допускается несколько строк.</span><span class="sxs-lookup"><span data-stu-id="af47f-351">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="af47f-352">Несколько строк для одного преподавателя и курса недопустимы.</span><span class="sxs-lookup"><span data-stu-id="af47f-352">Multiple rows for the same instructor and course isn't allowed.</span></span>

<span data-ttu-id="af47f-353">Сущность соединения `Enrollment` определяет собственный первичный ключ, поэтому подобные дубликаты возможны.</span><span class="sxs-lookup"><span data-stu-id="af47f-353">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="af47f-354">Для предотвращения подобных дубликатов:</span><span class="sxs-lookup"><span data-stu-id="af47f-354">To prevent such duplicates:</span></span>

* <span data-ttu-id="af47f-355">добавьте уникальный индекс для полей внешнего ключа или</span><span class="sxs-lookup"><span data-stu-id="af47f-355">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="af47f-356">настройте `Enrollment` с первичным составным ключом аналогично `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="af47f-356">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="af47f-357">Дополнительные сведения см. в разделе [Индексы](https://docs.microsoft.com/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="af47f-357">For more information, see [Indexes](https://docs.microsoft.com/ef/core/modeling/indexes).</span></span>

## <a name="update-the-db-context"></a><span data-ttu-id="af47f-358">Изменение контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="af47f-358">Update the DB context</span></span>

<span data-ttu-id="af47f-359">Добавьте выделенный ниже код в файл *Data/SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="af47f-359">Add the following highlighted code to *Data/SchoolContext.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="af47f-360">Предыдущий код добавляет новые сущности и настраивает составной первичный ключ сущности `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="af47f-360">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="af47f-361">Текучий API вместо атрибутов</span><span class="sxs-lookup"><span data-stu-id="af47f-361">Fluent API alternative to attributes</span></span>

<span data-ttu-id="af47f-362">Метод `OnModelCreating` в предыдущем коде использует для настройки поведения EF Core *текучий API*.</span><span class="sxs-lookup"><span data-stu-id="af47f-362">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="af47f-363">Этот API называется "текучим", так как часто используется для объединения серии вызовов методов в один оператор.</span><span class="sxs-lookup"><span data-stu-id="af47f-363">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="af47f-364">В [следующем коде](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) показан пример текучего API:</span><span class="sxs-lookup"><span data-stu-id="af47f-364">The [following code](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="af47f-365">В этом руководстве текучий API используется только для сопоставления базы данных, которое невозможно выполнить с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="af47f-365">In this tutorial, the fluent API is used only for DB mapping that can't be done with attributes.</span></span> <span data-ttu-id="af47f-366">Однако текучий API позволяет задать большинство правил форматирования, проверки и сопоставления, которые можно указать с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="af47f-366">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="af47f-367">Некоторые атрибуты, такие как `MinimumLength`, невозможно применить с текучим API.</span><span class="sxs-lookup"><span data-stu-id="af47f-367">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="af47f-368">`MinimumLength` не изменяет схему, он лишь применяет правило проверки минимальной длины.</span><span class="sxs-lookup"><span data-stu-id="af47f-368">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="af47f-369">Некоторые разработчики предпочитают использовать текучий API монопольно, чтобы оставить свои классы сущностей "чистыми".</span><span class="sxs-lookup"><span data-stu-id="af47f-369">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="af47f-370">Атрибуты и текучий API можно смешивать.</span><span class="sxs-lookup"><span data-stu-id="af47f-370">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="af47f-371">Существует несколько конфигураций, которые можно реализовать только с помощью текучего API (с указанием составного первичного ключа).</span><span class="sxs-lookup"><span data-stu-id="af47f-371">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="af47f-372">Существует несколько конфигураций, которые можно реализовать только с помощью атрибутов (`MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="af47f-372">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="af47f-373">Рекомендации по использованию текучего API и атрибутов:</span><span class="sxs-lookup"><span data-stu-id="af47f-373">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="af47f-374">Выберите один из двух этих подходов.</span><span class="sxs-lookup"><span data-stu-id="af47f-374">Choose one of these two approaches.</span></span>
* <span data-ttu-id="af47f-375">Используйте выбранный подход максимально согласованно.</span><span class="sxs-lookup"><span data-stu-id="af47f-375">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="af47f-376">Некоторые атрибуты из этого руководства используются:</span><span class="sxs-lookup"><span data-stu-id="af47f-376">Some of the attributes used in the this tutorial are used for:</span></span>

* <span data-ttu-id="af47f-377">только для проверки (например, `MinimumLength`);</span><span class="sxs-lookup"><span data-stu-id="af47f-377">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="af47f-378">только для конфигурации EF Core (например, `HasKey`);</span><span class="sxs-lookup"><span data-stu-id="af47f-378">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="af47f-379">для проверки и конфигурации EF Core (например, `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="af47f-379">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="af47f-380">Дополнительные сведения о сравнении атрибутов и текучего API см. в разделе [Методы конфигурации](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span><span class="sxs-lookup"><span data-stu-id="af47f-380">For more information about attributes vs. fluent API, see [Methods of configuration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="af47f-381">Схема сущностей, показывающая связи</span><span class="sxs-lookup"><span data-stu-id="af47f-381">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="af47f-382">Ниже показана схема, создаваемая средствами EF Power Tools для завершенной модели School.</span><span class="sxs-lookup"><span data-stu-id="af47f-382">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Схема сущностей](complex-data-model/_static/diagram.png)

<span data-ttu-id="af47f-384">На предыдущей схеме показано следующее:</span><span class="sxs-lookup"><span data-stu-id="af47f-384">The preceding diagram shows:</span></span>

* <span data-ttu-id="af47f-385">Несколько линий связи один ко многим (1 к \*).</span><span class="sxs-lookup"><span data-stu-id="af47f-385">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="af47f-386">Линия связи один к нулю или одному (1 к 0..1) между сущностями `Instructor` и `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="af47f-386">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="af47f-387">Линия связи нуль или один ко многим (0..1 к \*) между сущностями `Instructor` и `Department`.</span><span class="sxs-lookup"><span data-stu-id="af47f-387">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-db-with-test-data"></a><span data-ttu-id="af47f-388">Заполнение базы данных тестовыми данными</span><span class="sxs-lookup"><span data-stu-id="af47f-388">Seed the DB with Test Data</span></span>

<span data-ttu-id="af47f-389">Обновите код в файле *Data/DbInitializer.cs*:</span><span class="sxs-lookup"><span data-stu-id="af47f-389">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="af47f-390">Предыдущий код предоставляет начальные данные для новых сущностей.</span><span class="sxs-lookup"><span data-stu-id="af47f-390">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="af47f-391">Основная часть кода создает объекты сущностей и загружает демонстрационные данные.</span><span class="sxs-lookup"><span data-stu-id="af47f-391">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="af47f-392">Демонстрационные данные используются для проверки.</span><span class="sxs-lookup"><span data-stu-id="af47f-392">The sample data is used for testing.</span></span> <span data-ttu-id="af47f-393">Приведенный выше код создает следующие связи многие ко многим:</span><span class="sxs-lookup"><span data-stu-id="af47f-393">The preceding code creates the following many-to-many relationships:</span></span>

* `Enrollments`
* `CourseAssignment`

## <a name="add-a-migration"></a><span data-ttu-id="af47f-394">Добавление миграции</span><span class="sxs-lookup"><span data-stu-id="af47f-394">Add a migration</span></span>

<span data-ttu-id="af47f-395">Выполните сборку проекта.</span><span class="sxs-lookup"><span data-stu-id="af47f-395">Build the project.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="af47f-396">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="af47f-396">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration ComplexDataModel
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="af47f-397">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="af47f-397">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations add ComplexDataModel
```

------

<span data-ttu-id="af47f-398">Предыдущая команда отображает предупреждение о возможной потере данных.</span><span class="sxs-lookup"><span data-stu-id="af47f-398">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="af47f-399">При выполнении команды `database update` возникает следующая ошибка:</span><span class="sxs-lookup"><span data-stu-id="af47f-399">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

## <a name="apply-the-migration"></a><span data-ttu-id="af47f-400">Применение миграции</span><span class="sxs-lookup"><span data-stu-id="af47f-400">Apply the migration</span></span>

<span data-ttu-id="af47f-401">Теперь у вас есть база данных, и пора подумать о том, как применять к ней будущие изменения.</span><span class="sxs-lookup"><span data-stu-id="af47f-401">Now that you have an existing database, you need to think about how to apply future changes to it.</span></span> <span data-ttu-id="af47f-402">В этом руководстве показано два подхода:</span><span class="sxs-lookup"><span data-stu-id="af47f-402">This tutorial shows two approaches:</span></span>
* [<span data-ttu-id="af47f-403">Удаление и повторное создание базы данных</span><span class="sxs-lookup"><span data-stu-id="af47f-403">Drop and re-create the database</span></span>](#drop)
* <span data-ttu-id="af47f-404">[Применение миграции к существующей базе данных](#applyexisting).</span><span class="sxs-lookup"><span data-stu-id="af47f-404">[Apply the migration to the existing database](#applyexisting).</span></span> <span data-ttu-id="af47f-405">Хотя этот метод является более сложным и трудоемким, в реальной рабочей среде лучше использовать именно его.</span><span class="sxs-lookup"><span data-stu-id="af47f-405">While this method is more complex and time-consuming, it's the preferred approach for real-world, production environments.</span></span> <span data-ttu-id="af47f-406">**Примечание**. Это необязательный раздел руководства.</span><span class="sxs-lookup"><span data-stu-id="af47f-406">**Note**: This is an optional section of the tutorial.</span></span> <span data-ttu-id="af47f-407">Вы можете выполнить удаление и повторное создание и пропустить этот раздел.</span><span class="sxs-lookup"><span data-stu-id="af47f-407">You can do the drop and re-create steps and skip this section.</span></span> <span data-ttu-id="af47f-408">Если вы хотите выполнить инструкции в этом разделе, не выполняйте удаление и повторное создание.</span><span class="sxs-lookup"><span data-stu-id="af47f-408">If you do want to follow the steps in this section, don't do the drop and re-create steps.</span></span> 

<a name="drop"></a>

### <a name="drop-and-re-create-the-database"></a><span data-ttu-id="af47f-409">Удаление и повторное создание базы данных</span><span class="sxs-lookup"><span data-stu-id="af47f-409">Drop and re-create the database</span></span>

<span data-ttu-id="af47f-410">Код в обновленном `DbInitializer` предоставляет начальные данные для новых сущностей.</span><span class="sxs-lookup"><span data-stu-id="af47f-410">The code in the updated `DbInitializer` adds seed data for the new entities.</span></span> <span data-ttu-id="af47f-411">Чтобы выполнить в EF Core принудительное создание новой базы данных, удаление и обновление базы данных:</span><span class="sxs-lookup"><span data-stu-id="af47f-411">To force EF Core to create a new  DB, drop and update the DB:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="af47f-412">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="af47f-412">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="af47f-413">Выполните следующие команды в **консоли диспетчера пакетов** (PMC):</span><span class="sxs-lookup"><span data-stu-id="af47f-413">In the **Package Manager Console** (PMC), run the following command:</span></span>

```PMC
Drop-Database
Update-Database
```

<span data-ttu-id="af47f-414">Чтобы просмотреть справочную информацию, выполните команду `Get-Help about_EntityFrameworkCore` в PMC.</span><span class="sxs-lookup"><span data-stu-id="af47f-414">Run `Get-Help about_EntityFrameworkCore` from the PMC to get help information.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="af47f-415">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="af47f-415">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="af47f-416">Откройте командное окно и перейдите в папку проекта.</span><span class="sxs-lookup"><span data-stu-id="af47f-416">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="af47f-417">Папка проекта содержит файл *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="af47f-417">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="af47f-418">Введите в командном окне следующее:</span><span class="sxs-lookup"><span data-stu-id="af47f-418">Enter the following in the command window:</span></span>

 ```console
 dotnet ef database drop
dotnet ef database update
 ```

------

<span data-ttu-id="af47f-419">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="af47f-419">Run the app.</span></span> <span data-ttu-id="af47f-420">При запуске приложения выполняется метод `DbInitializer.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="af47f-420">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="af47f-421">`DbInitializer.Initialize` заполняет новую базу данных.</span><span class="sxs-lookup"><span data-stu-id="af47f-421">The `DbInitializer.Initialize` populates the new DB.</span></span>

<span data-ttu-id="af47f-422">Откройте базу данных в SSOX:</span><span class="sxs-lookup"><span data-stu-id="af47f-422">Open the DB in SSOX:</span></span>

* <span data-ttu-id="af47f-423">Если SSOX был открыт ранее, нажмите кнопку **Обновить**.</span><span class="sxs-lookup"><span data-stu-id="af47f-423">If SSOX was opened previously, click the **Refresh** button.</span></span>
* <span data-ttu-id="af47f-424">Разверните узел **Таблицы**.</span><span class="sxs-lookup"><span data-stu-id="af47f-424">Expand the **Tables** node.</span></span> <span data-ttu-id="af47f-425">Отображаются созданные таблицы.</span><span class="sxs-lookup"><span data-stu-id="af47f-425">The created tables are displayed.</span></span>

![Таблицы в SSOX](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="af47f-427">Изучите таблицу **CourseAssignment**:</span><span class="sxs-lookup"><span data-stu-id="af47f-427">Examine the **CourseAssignment** table:</span></span>

* <span data-ttu-id="af47f-428">Щелкните правой кнопкой мыши таблицу **CourseAssignment** и выберите пункт **Просмотреть данные**.</span><span class="sxs-lookup"><span data-stu-id="af47f-428">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
* <span data-ttu-id="af47f-429">Убедитесь, что таблица **CourseAssignment** содержит данные.</span><span class="sxs-lookup"><span data-stu-id="af47f-429">Verify the **CourseAssignment** table contains data.</span></span>

![Данные CourseAssignment в SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="applyexisting"></a>

### <a name="apply-the-migration-to-the-existing-database"></a><span data-ttu-id="af47f-431">Применение миграции к существующей базе данных</span><span class="sxs-lookup"><span data-stu-id="af47f-431">Apply the migration to the existing database</span></span>

<span data-ttu-id="af47f-432">Этот раздел является необязательным.</span><span class="sxs-lookup"><span data-stu-id="af47f-432">This section is optional.</span></span> <span data-ttu-id="af47f-433">Эти действия подходят только в том случае, если вы пропустили предыдущий раздел [Удаление и повторное создание базы данных](#drop).</span><span class="sxs-lookup"><span data-stu-id="af47f-433">These steps work only if you skipped the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="af47f-434">При выполнении миграций с существующими данными могут действовать ограничения внешнего ключа, которым существующие данные не соответствуют.</span><span class="sxs-lookup"><span data-stu-id="af47f-434">When migrations are run with existing data, there may be FK constraints that are not satisfied with the existing data.</span></span> <span data-ttu-id="af47f-435">Для рабочих данных нужно предпринять меры по переносу существующих данных.</span><span class="sxs-lookup"><span data-stu-id="af47f-435">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="af47f-436">Этот раздел содержит пример того, как устранить нарушения ограничений внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="af47f-436">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="af47f-437">Вносите эти изменения кода только после создания резервной копии.</span><span class="sxs-lookup"><span data-stu-id="af47f-437">Don't make these code changes without a backup.</span></span> <span data-ttu-id="af47f-438">Не вносите эти изменения кода, если вы выполнили инструкции из предыдущего раздела и обновили базу данных.</span><span class="sxs-lookup"><span data-stu-id="af47f-438">Don't make these code changes if you completed the previous section and updated the database.</span></span>

<span data-ttu-id="af47f-439">Файл *{метка_времени}_ComplexDataModel.cs* содержит следующий код:</span><span class="sxs-lookup"><span data-stu-id="af47f-439">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="af47f-440">Предыдущий код добавляет в таблицу `Course` внешний ключ `DepartmentID`, не допускающий значение null.</span><span class="sxs-lookup"><span data-stu-id="af47f-440">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="af47f-441">База данных из предыдущего руководства содержит строки в `Course`, поэтому эту таблицу невозможно обновить с помощью миграций.</span><span class="sxs-lookup"><span data-stu-id="af47f-441">The DB from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="af47f-442">Чтобы заставить миграцию `ComplexDataModel` работать с существующими данными:</span><span class="sxs-lookup"><span data-stu-id="af47f-442">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="af47f-443">Изменить код, чтобы присвоить новому столбцу (`DepartmentID`) значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="af47f-443">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="af47f-444">Создайте фиктивную кафедру с именем "Temp" для использования по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="af47f-444">Create a fake department named "Temp" to act as the default department.</span></span>

#### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="af47f-445">Устранение ограничений внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="af47f-445">Fix the foreign key constraints</span></span>

<span data-ttu-id="af47f-446">Обновите метод `Up` в классах `ComplexDataModel`.</span><span class="sxs-lookup"><span data-stu-id="af47f-446">Update the `ComplexDataModel` classes `Up` method:</span></span>

* <span data-ttu-id="af47f-447">Откройте файл *{метка_времени}_ComplexDataModel.cs*.</span><span class="sxs-lookup"><span data-stu-id="af47f-447">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="af47f-448">Закомментируйте строку кода, которая добавляет столбец `DepartmentID` в таблицу `Course`.</span><span class="sxs-lookup"><span data-stu-id="af47f-448">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="af47f-449">Добавьте выделенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="af47f-449">Add the following highlighted code.</span></span> <span data-ttu-id="af47f-450">Новый код идет после блока `.CreateTable( name: "Department"`: [!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span><span class="sxs-lookup"><span data-stu-id="af47f-450">The new code goes after the `.CreateTable( name: "Department"` block: [!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span></span>

<span data-ttu-id="af47f-451">После внесения описанных выше изменений существующие строки `Course` будут связаны с кафедрой "Temp" после выполнения метода `ComplexDataModel` `Up`.</span><span class="sxs-lookup"><span data-stu-id="af47f-451">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel` `Up` method runs.</span></span>

<span data-ttu-id="af47f-452">Рабочее приложение:</span><span class="sxs-lookup"><span data-stu-id="af47f-452">A production app would:</span></span>

* <span data-ttu-id="af47f-453">включает код или сценарии, чтобы добавить строки `Department` и связанные строки `Course` к новым строкам `Department`;</span><span class="sxs-lookup"><span data-stu-id="af47f-453">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="af47f-454">не использует кафедру "Temp" или значение по умолчанию для `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="af47f-454">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

<span data-ttu-id="af47f-455">Следующее руководство посвящено связанным данным.</span><span class="sxs-lookup"><span data-stu-id="af47f-455">The next tutorial covers related data.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="af47f-456">[Назад](xref:data/ef-rp/migrations)
> [Вперед](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="af47f-456">[Previous](xref:data/ef-rp/migrations)
[Next](xref:data/ef-rp/read-related-data)</span></span>
