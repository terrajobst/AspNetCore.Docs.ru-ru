---
title: Razor Pages с EF Core в ASP.NET Core — модель данных— 5 из 8
author: rick-anderson
description: В этом руководстве вы добавите дополнительные сущности и связи, а также настроите модель данных, указав правила форматирования, проверки и сопоставления.
ms.author: riande
ms.custom: mvc
ms.date: 07/22/2019
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: 2461bc398cd237dac04f4eb8832c70290663ff56
ms.sourcegitcommit: 7d3c6565dda6241eb13f9a8e1e1fd89b1cfe4d18
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2019
ms.locfileid: "72259486"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---data-model---5-of-8"></a><span data-ttu-id="fcdf8-103">Razor Pages с EF Core в ASP.NET Core — модель данных— 5 из 8</span><span class="sxs-lookup"><span data-stu-id="fcdf8-103">Razor Pages with EF Core in ASP.NET Core - Data Model - 5 of 8</span></span>

<span data-ttu-id="fcdf8-104">Авторы: [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="fcdf8-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fcdf8-105">Предыдущие руководства работали с базовой моделью данных, состоящей из трех сущностей.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-105">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="fcdf8-106">В этом руководстве:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-106">In this tutorial:</span></span>

* <span data-ttu-id="fcdf8-107">Добавляются дополнительные сущности и связи.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-107">More entities and relationships are added.</span></span>
* <span data-ttu-id="fcdf8-108">Настраивается модель данных с помощью указания правил для форматирования, проверки и сопоставления базы данных.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-108">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="fcdf8-109">Готовая модель данных показана на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-109">The completed data model is shown in the following illustration:</span></span>

![Схема сущностей](complex-data-model/_static/diagram.png)

## <a name="the-student-entity"></a><span data-ttu-id="fcdf8-111">Сущность Student</span><span class="sxs-lookup"><span data-stu-id="fcdf8-111">The Student entity</span></span>

![Сущность Student](complex-data-model/_static/student-entity.png)

<span data-ttu-id="fcdf8-113">Замените код в файле *Models/Student.cs* на следующий:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-113">Replace the code in *Models/Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Student.cs)]

<span data-ttu-id="fcdf8-114">В приведенном выше коде добавляется свойство `FullName` и добавляются следующие атрибуты к существующим свойствам:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-114">The preceding code adds a `FullName` property and adds the following attributes to existing properties:</span></span>

* `[DataType]`
* `[DisplayFormat]`
* `[StringLength]`
* `[Column]`
* `[Required]`
* `[Display]`

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="fcdf8-115">Вычисляемое свойство FullName</span><span class="sxs-lookup"><span data-stu-id="fcdf8-115">The FullName calculated property</span></span>

<span data-ttu-id="fcdf8-116">`FullName` — это вычисляемое свойство, которое возвращает значение, созданное путем объединения двух других свойств.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-116">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="fcdf8-117">`FullName` не может быть задано, поэтому имеет только метод доступа get.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-117">`FullName` can't be set, so it has only a get accessor.</span></span> <span data-ttu-id="fcdf8-118">В базе данных не создается столбец `FullName`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-118">No `FullName` column is created in the database.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="fcdf8-119">Атрибут DataType</span><span class="sxs-lookup"><span data-stu-id="fcdf8-119">The DataType attribute</span></span>

```csharp
[DataType(DataType.Date)]
```

<span data-ttu-id="fcdf8-120">Сейчас для дат зачисления учащихся на всех страницах отображаются время и дата, хотя важна только дата.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-120">For student enrollment dates, all of the pages currently display the time of day along with the date, although only the date is relevant.</span></span> <span data-ttu-id="fcdf8-121">Используя атрибуты заметок к данным, вы можете внести в код одно изменение, позволяющее исправить формат отображения на каждой странице, где отображаются эти данные.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-121">By using data annotation attributes, you can make one code change that will fix the display format in every page that shows the data.</span></span> 

<span data-ttu-id="fcdf8-122">Атрибут [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) указывает тип данных более точно по сравнению со встроенным типом базы данных.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-122">The [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="fcdf8-123">В этом случае следует отображать отобразить только дату, а не дату и время.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-123">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="fcdf8-124">В [перечислении DataType](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) представлено множество типов данных, таких как Date, Time, PhoneNumber, Currency, EmailAddress и других. Атрибут `DataType` также обеспечивает автоматическое предоставление функций для определенных типов в приложении.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-124">The [DataType Enumeration](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="fcdf8-125">Например:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-125">For example:</span></span>

* <span data-ttu-id="fcdf8-126">Ссылка `mailto:` для `DataType.EmailAddress` создается автоматически.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-126">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="fcdf8-127">Средство выбора даты предоставляется для `DataType.Date` в большинстве браузеров.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-127">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="fcdf8-128">Атрибут `DataType` создает атрибуты HTML 5 `data-`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-128">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes.</span></span> <span data-ttu-id="fcdf8-129">Атрибуты `DataType` не предназначены для проверки.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-129">The `DataType` attributes don't provide validation.</span></span>

### <a name="the-displayformat-attribute"></a><span data-ttu-id="fcdf8-130">Атрибут DisplayFormat</span><span class="sxs-lookup"><span data-stu-id="fcdf8-130">The DisplayFormat attribute</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="fcdf8-131">`DataType.Date` не задает формат отображаемой даты.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-131">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="fcdf8-132">По умолчанию поле даты отображается с использованием форматов, установленных в [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support) сервера.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-132">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="fcdf8-133">С помощью атрибута `DisplayFormat` можно явно указать формат даты:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-133">The `DisplayFormat` attribute is used to explicitly specify the date format.</span></span> <span data-ttu-id="fcdf8-134">Параметр `ApplyFormatInEditMode` указывает, что формат должен применяться к пользовательскому интерфейсу редактирования.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-134">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="fcdf8-135">Некоторым полям не следует использовать `ApplyFormatInEditMode`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-135">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="fcdf8-136">Например, обозначение денежной единицы в общем случае не должно отображаться в поле редактирования текста.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-136">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="fcdf8-137">Атрибут `DisplayFormat` можно использовать отдельно.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-137">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="fcdf8-138">Однако чаще всего `DataType` рекомендуется применять вместе с атрибутом `DisplayFormat`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-138">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="fcdf8-139">Атрибут `DataType` передает семантику данных (в отличие от способа их вывода на экран).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-139">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="fcdf8-140">Атрибут `DataType` дает следующие преимущества, недоступные в `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-140">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="fcdf8-141">Поддержка функций HTML5 в браузере.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-141">The browser can enable HTML5 features.</span></span> <span data-ttu-id="fcdf8-142">Например, отображение элемента управления календарем, соответствующего языковому стандарту обозначения денежной единицы, ссылок электронной почты, проверки входных данных на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-142">For example, show a calendar control, the locale-appropriate currency symbol, email links, and client-side input validation.</span></span>
* <span data-ttu-id="fcdf8-143">По умолчанию формат отображения данных в браузере определяется в соответствии с установленным языковым стандартом.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-143">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="fcdf8-144">Дополнительные сведения см. в документации по [вспомогательной функции тегов \<input>](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-144">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

### <a name="the-stringlength-attribute"></a><span data-ttu-id="fcdf8-145">Атрибут StringLength</span><span class="sxs-lookup"><span data-stu-id="fcdf8-145">The StringLength attribute</span></span>

```csharp
[StringLength(50, ErrorMessage = "First name cannot be longer than 50 characters.")]
```

<span data-ttu-id="fcdf8-146">С помощью атрибутов можно указать правила проверки данных и сообщения об ошибках проверки.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-146">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="fcdf8-147">Атрибут [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) указывает минимальную и максимальную длину символов, разрешенных в поле данных.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-147">The [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="fcdf8-148">Представленный код задает ограничение длины имен в 50 символов.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-148">The code shown limits names to no more than 50 characters.</span></span> <span data-ttu-id="fcdf8-149">Пример, в котором задается минимальная длина строки, приводится [далее](#the-required-attribute).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-149">An example that sets the minimum string length is shown [later](#the-required-attribute).</span></span>

<span data-ttu-id="fcdf8-150">Атрибут `StringLength` также обеспечивает проверку на стороне клиента и на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-150">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="fcdf8-151">Минимальное значение не оказывает влияния на схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-151">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="fcdf8-152">Атрибут `StringLength` не запрещает пользователю ввести пробел в качестве имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-152">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="fcdf8-153">Атрибут [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) можно использовать для применения ограничений к входным данным.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-153">The [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute can be used to apply restrictions to the input.</span></span> <span data-ttu-id="fcdf8-154">Например, следующий код требует, чтобы первый символ был прописной буквой, а остальные символы были буквенными:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-154">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fcdf8-155">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fcdf8-155">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fcdf8-156">В **обозревателе объектов SQL Server** (SSOX) откройте конструктор таблиц учащихся, дважды щелкнув таблицу **Student** (Учащийся).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-156">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Таблица учащихся в SSOX до миграций](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="fcdf8-158">На предыдущем изображении показана схемы для таблицы `Student`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-158">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="fcdf8-159">Поля имен имеют тип `nvarchar(MAX)`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-159">The name fields have type `nvarchar(MAX)`.</span></span> <span data-ttu-id="fcdf8-160">Когда далее в этом учебнике будет создана и применена миграция, поля имен станут `nvarchar(50)` из-за атрибутов длины строки.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-160">When a migration is created and applied later in this tutorial, the name fields become `nvarchar(50)` as a result of the string length attributes.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fcdf8-161">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fcdf8-161">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="fcdf8-162">В средстве SQLite изучите определения столбцов для таблицы `Student`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-162">In your SQLite tool, examine the column definitions for the `Student` table.</span></span> <span data-ttu-id="fcdf8-163">Поля имен имеют тип `Text`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-163">The name fields have type `Text`.</span></span> <span data-ttu-id="fcdf8-164">Обратите внимание, что первое поле имени называется `FirstMidName`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-164">Notice that the first name field is called `FirstMidName`.</span></span> <span data-ttu-id="fcdf8-165">В следующем разделе вы измените имя этого столбца на `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-165">In the next section, you change the name of that column to `FirstName`.</span></span>

---

### <a name="the-column-attribute"></a><span data-ttu-id="fcdf8-166">Атрибут Column</span><span class="sxs-lookup"><span data-stu-id="fcdf8-166">The Column attribute</span></span>

```csharp
[Column("FirstName")]
public string FirstMidName { get; set; }
```

<span data-ttu-id="fcdf8-167">Атрибуты могут управлять, как именно классы и свойства сопоставляются с базой данных.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-167">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="fcdf8-168">В модели `Student` атрибут `Column` используется для сопоставления имени свойства `FirstMidName` с "FirstName" в базе данных.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-168">In the `Student` model, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the database.</span></span>

<span data-ttu-id="fcdf8-169">При создании базы данных имена свойств в модели используются для имен столбцов (кроме случая, когда используется атрибут `Column`).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-169">When the database is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span> <span data-ttu-id="fcdf8-170">Модель `Student` использует `FirstMidName` для поля имени, так как это поле также может содержать отчество.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-170">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="fcdf8-171">С атрибутом `[Column]` поле `Student.FirstMidName` в модели данных сопоставляется со столбцом `FirstName` таблицы `Student`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-171">With the `[Column]` attribute, `Student.FirstMidName` in the data model maps to the `FirstName` column of the `Student` table.</span></span> <span data-ttu-id="fcdf8-172">Добавление атрибута `Column` изменяет модель для поддержки `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-172">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="fcdf8-173">Модель, поддерживающая `SchoolContext`, больше не соответствует базе данных.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-173">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="fcdf8-174">Это несоответствие будет устранено путем добавления миграции далее в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-174">That discrepancy will be resolved by adding a migration later in this tutorial.</span></span>

### <a name="the-required-attribute"></a><span data-ttu-id="fcdf8-175">Атрибут Required</span><span class="sxs-lookup"><span data-stu-id="fcdf8-175">The Required attribute</span></span>

```csharp
[Required]
```

<span data-ttu-id="fcdf8-176">Атрибут `Required` делает свойства имен обязательными полями.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-176">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="fcdf8-177">Атрибут `Required` не нужен для типов, не допускающих значения NULL, например для типов значений (таких как `DateTime`, `int` и `double`).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-177">The `Required` attribute isn't needed for non-nullable types such as value types (for example, `DateTime`, `int`, and `double`).</span></span> <span data-ttu-id="fcdf8-178">Типы, которые не могут принимать значение null, автоматически обрабатываются как обязательные поля.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-178">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="fcdf8-179">Для применения `MinimumLength` нужно использовать атрибут `Required` с `MinimumLength`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-179">The `Required` attribute must be used with `MinimumLength` for the `MinimumLength` to be enforced.</span></span>

```csharp
[Display(Name = "Last Name")]
[Required]
[StringLength(50, MinimumLength=2)]
public string LastName { get; set; }
```

<span data-ttu-id="fcdf8-180">`MinimumLength` и `Required` разрешают использовать пробелы при проверке.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-180">`MinimumLength` and `Required` allow whitespace to satisfy the validation.</span></span> <span data-ttu-id="fcdf8-181">Используйте атрибут `RegularExpression` для полного контроля над строкой.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-181">Use the `RegularExpression` attribute for full control over the string.</span></span>

### <a name="the-display-attribute"></a><span data-ttu-id="fcdf8-182">Атрибут Display</span><span class="sxs-lookup"><span data-stu-id="fcdf8-182">The Display attribute</span></span>

```csharp
[Display(Name = "Last Name")]
```

<span data-ttu-id="fcdf8-183">Атрибут `Display` указывает, что заголовки для текстовых полей должны иметь вид "First Name" (Имя), "Last Name" (Фамилия), "Full Name" (Полное имя) и "Enrollment Date" (Дата зачисления).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-183">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="fcdf8-184">По умолчанию в заголовках не использовался пробел для разделения слов, например "Lastname".</span><span class="sxs-lookup"><span data-stu-id="fcdf8-184">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="create-a-migration"></a><span data-ttu-id="fcdf8-185">Создание миграции</span><span class="sxs-lookup"><span data-stu-id="fcdf8-185">Create a migration</span></span>

<span data-ttu-id="fcdf8-186">Запустите приложение и перейдите на страницу Students.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-186">Run the app and go to the Students page.</span></span> <span data-ttu-id="fcdf8-187">Возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-187">An exception is thrown.</span></span> <span data-ttu-id="fcdf8-188">Атрибут `[Column]` приводит к тому, что EF ожидает столбец с именем `FirstName`, но имя столбца в базе данных по-прежнему `FirstMidName`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-188">The `[Column]` attribute causes EF to expect to find a column named `FirstName`, but the column name in the database is still `FirstMidName`.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fcdf8-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fcdf8-189">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fcdf8-190">Сообщение об ошибке подобно приведенному ниже.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-190">The error message is similar to the following example:</span></span>

```
SqlException: Invalid column name 'FirstName'.
```

* <span data-ttu-id="fcdf8-191">В PMC введите следующие команды для создания миграции и обновления базы данных:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-191">In the PMC, enter the following commands to create a new migration and update the database:</span></span>

  ```powershell
  Add-Migration ColumnFirstName
  Update-Database
  ```

  <span data-ttu-id="fcdf8-192">Первая из этих команд выдает следующее предупреждающее сообщение:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-192">The first of these commands generates the following warning message:</span></span>

  ```text
  An operation was scaffolded that may result in the loss of data.
  Please review the migration for accuracy.
  ```

  <span data-ttu-id="fcdf8-193">Это предупреждение вызвано тем, что поля имен теперь ограничены 50 символами.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-193">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="fcdf8-194">Если имя в базе данных имеет больше 50 символов, символы с 51-го до последнего будут потеряны.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-194">If a name in the database had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="fcdf8-195">Откройте таблицу "Student" (Учащийся) в окне SSOX:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-195">Open the Student table in SSOX:</span></span>

  ![Таблица учащихся в SSOX после миграций](complex-data-model/_static/ssox-after-migration.png)

  <span data-ttu-id="fcdf8-197">До применения миграции столбцы имен имели тип [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-197">Before the migration was applied, the name columns were of type [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="fcdf8-198">Теперь столбцы имен имеют тип `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-198">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="fcdf8-199">Имя столбца изменилось с `FirstMidName` на `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-199">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fcdf8-200">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fcdf8-200">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="fcdf8-201">Сообщение об ошибке подобно приведенному ниже.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-201">The error message is similar to the following example:</span></span>

```
SqliteException: SQLite Error 1: 'no such column: s.FirstName'.
```

* <span data-ttu-id="fcdf8-202">Откройте командное окно в папке проекта.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-202">Open a command window in the project folder.</span></span> <span data-ttu-id="fcdf8-203">Введите следующие команды для создания миграции и обновления базы данных:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-203">Enter the following commands to create a new migration and update the database:</span></span>

  ```dotnetcli
  dotnet ef migrations add ColumnFirstName
  dotnet ef database update
  ```

  <span data-ttu-id="fcdf8-204">Команда обновления базы данных выводит ошибку наподобие приведенной ниже.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-204">The database update command displays an error like the following example:</span></span>

  ```text
  SQLite does not support this migration operation ('AlterColumnOperation'). For more information, see http://go.microsoft.com/fwlink/?LinkId=723262.
  ```

<span data-ttu-id="fcdf8-205">В этом учебнике устранить ошибку можно, удалив и заново создав исходную миграцию.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-205">For this tutorial, the way to get past this error is to delete and re-create the initial migration.</span></span> <span data-ttu-id="fcdf8-206">Дополнительные сведения см. в предупреждении, касающемся SQLite, в начале [учебника по миграции](xref:data/ef-rp/migrations).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-206">For more information, see the SQLite warning note at the top of the [migrations tutorial](xref:data/ef-rp/migrations).</span></span>

* <span data-ttu-id="fcdf8-207">Удалите папку *Migrations*.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-207">Delete the *Migrations* folder.</span></span>
* <span data-ttu-id="fcdf8-208">Выполните следующие команды, чтобы удалить базу данных, создать новую исходную миграцию и применить ее:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-208">Run the following commands to drop the database, create a new initial migration, and apply the migration:</span></span>

  ```dotnetcli
  dotnet ef database drop --force
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

* <span data-ttu-id="fcdf8-209">Изучите таблицу Student в средстве SQLite.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-209">Examine the Student table in your SQLite tool.</span></span> <span data-ttu-id="fcdf8-210">Столбец, ранее называвшийся FirstMidName, теперь имеет имя FirstName.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-210">The column that was FirstMidName is now FirstName.</span></span>

---

* <span data-ttu-id="fcdf8-211">Запустите приложение и перейдите на страницу Students.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-211">Run the app and go to the Students page.</span></span>
* <span data-ttu-id="fcdf8-212">Обратите внимание, что значения времени не вводятся и не отображаются вместе с датами.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-212">Notice that times are not input or displayed along with dates.</span></span>
* <span data-ttu-id="fcdf8-213">Выберите **Create New** (Создать) и попробуйте ввести имя длиной более 50 символов.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-213">Select **Create New**, and try to enter a name longer than 50 characters.</span></span>

> [!Note]
> <span data-ttu-id="fcdf8-214">В следующих разделах сборка приложения на некоторых этапах приводит к возникновению ошибок компилятора.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-214">In the following sections, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="fcdf8-215">В инструкциях указано, когда производить сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-215">The instructions specify when to build the app.</span></span>

## <a name="the-instructor-entity"></a><span data-ttu-id="fcdf8-216">Сущность Instructor</span><span class="sxs-lookup"><span data-stu-id="fcdf8-216">The Instructor Entity</span></span>

![Сущность Instructor](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="fcdf8-218">Создайте файл *Models/Instructor.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-218">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Instructor.cs)]

<span data-ttu-id="fcdf8-219">На одной строке могут находиться несколько атрибутов.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-219">Multiple attributes can be on one line.</span></span> <span data-ttu-id="fcdf8-220">Атрибуты `HireDate` можно записать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-220">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="navigation-properties"></a><span data-ttu-id="fcdf8-221">Свойства навигации</span><span class="sxs-lookup"><span data-stu-id="fcdf8-221">Navigation properties</span></span>

<span data-ttu-id="fcdf8-222">`CourseAssignments` и `OfficeAssignment` — это свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-222">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="fcdf8-223">Преподаватель может проводить любое количество курсов, поэтому `CourseAssignments` определен как коллекция.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-223">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="fcdf8-224">Преподаватель может иметь не более одного кабинета, поэтому свойство `OfficeAssignment` содержит одну сущность `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-224">An instructor can have at most one office, so the `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="fcdf8-225">`OfficeAssignment` имеет значение null, если кабинет не назначен.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-225">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="the-officeassignment-entity"></a><span data-ttu-id="fcdf8-226">Сущность OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="fcdf8-226">The OfficeAssignment entity</span></span>

![Сущность OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="fcdf8-228">Создайте файл *Models/OfficeAssignment.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-228">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="fcdf8-229">Атрибут Key</span><span class="sxs-lookup"><span data-stu-id="fcdf8-229">The Key attribute</span></span>

<span data-ttu-id="fcdf8-230">Атрибут `[Key]` используется для идентификации свойства в качестве первичного ключа (PK), когда имя свойства отличается от classnameID или ID.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-230">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="fcdf8-231">Между сущностями `Instructor` и `OfficeAssignment` действует связь один к нулю или к одному.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-231">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="fcdf8-232">Назначение кабинета существует только в связи с преподавателем, которому оно назначено.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-232">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="fcdf8-233">Первичный ключ `OfficeAssignment` также является внешним ключом (FK) для сущности `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-233">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span>

<span data-ttu-id="fcdf8-234">EF Core не распознает `InstructorID` в качестве первичного ключа `OfficeAssignment` автоматически, так как `InstructorID` не соответствует соглашению об именовании ID или classnameID.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-234">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because `InstructorID` doesn't follow the ID or classnameID naming convention.</span></span> <span data-ttu-id="fcdf8-235">Таким образом, атрибут `Key` используется для определения `InstructorID` в качестве первичного ключа:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-235">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="fcdf8-236">По умолчанию EF Core считает ключ созданным не базой данных, так как столбец предназначен для идентифицирующего отношения.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-236">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="fcdf8-237">Свойство навигации Instructor</span><span class="sxs-lookup"><span data-stu-id="fcdf8-237">The Instructor navigation property</span></span>

<span data-ttu-id="fcdf8-238">Свойство навигации `Instructor.OfficeAssignment` может иметь значение NULL, так как строка `OfficeAssignment` для преподавателя может отсутствовать.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-238">The `Instructor.OfficeAssignment` navigation property can be null because there might not be an `OfficeAssignment` row for a given instructor.</span></span> <span data-ttu-id="fcdf8-239">Преподавателю может быть не назначен кабинет.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-239">An instructor might not have an office assignment.</span></span>

<span data-ttu-id="fcdf8-240">Свойство навигации `OfficeAssignment.Instructor` всегда будет иметь сущность Instructor, так как тип внешнего ключа `InstructorID` — это тип значения `int`, не допускающий значения NULL.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-240">The `OfficeAssignment.Instructor` navigation property will always have an instructor entity because the foreign key `InstructorID` type is `int`, a non-nullable value type.</span></span> <span data-ttu-id="fcdf8-241">Назначение кабинета не может существовать без преподавателя.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-241">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="fcdf8-242">Когда сущность `Instructor` имеет связанную сущность `OfficeAssignment`, каждая из них имеет ссылку на другую в своем свойстве навигации.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-242">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

## <a name="the-course-entity"></a><span data-ttu-id="fcdf8-243">Сущность Course</span><span class="sxs-lookup"><span data-stu-id="fcdf8-243">The Course Entity</span></span>

![Сущность Course](complex-data-model/_static/course-entity.png)

<span data-ttu-id="fcdf8-245">Обновите *Models/Course.cs*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-245">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Course.cs?highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="fcdf8-246">Сущность `Course` имеет свойство внешнего ключа (FK) `DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-246">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="fcdf8-247">`DepartmentID` указывает на связанную сущность `Department`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-247">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="fcdf8-248">Сущность `Course` имеет свойство навигации `Department`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-248">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="fcdf8-249">EF Core не требует свойства внешнего ключа для модели данных, если она имеет свойство навигации для связанной сущности.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-249">EF Core doesn't require a foreign key property for a data model when the model has a navigation property for a related entity.</span></span> <span data-ttu-id="fcdf8-250">EF Core автоматически создает внешние ключи в базе данных по мере необходимости.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-250">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="fcdf8-251">EF Core создает [теневые свойства](/ef/core/modeling/shadow-properties) для автоматически созданных внешних ключей.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-251">EF Core creates [shadow properties](/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="fcdf8-252">Однако явное включение внешнего ключа в модель данных позволяет сделать обновления проще и эффективнее.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-252">However, explicitly including the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="fcdf8-253">Например, рассмотрим модель, где свойство внешнего ключа `DepartmentID` *не* включено.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-253">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="fcdf8-254">При получении сущности курса для редактирования:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-254">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="fcdf8-255">свойство `Department` имеет значение NULL, если оно не загружено явно;</span><span class="sxs-lookup"><span data-stu-id="fcdf8-255">The `Department` property is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="fcdf8-256">для обновления сущности курса нужно сначала получить сущность `Department`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-256">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="fcdf8-257">Если свойство внешнего ключа `DepartmentID` включено в модель данных, получать сущность `Department` перед обновлением не нужно.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-257">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="fcdf8-258">Атрибут DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="fcdf8-258">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="fcdf8-259">Атрибут `[DatabaseGenerated(DatabaseGeneratedOption.None)]` указывает, что первичный ключ предоставляется приложением, а не создается базой данных.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-259">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="fcdf8-260">По умолчанию EF Core предполагает, что значения первичного ключа создаются базой данных.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-260">By default, EF Core assumes that PK values are generated by the database.</span></span> <span data-ttu-id="fcdf8-261">Обычно лучше всего использовать значения, созданные базой данных.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-261">Database-generated is generally the best approach.</span></span> <span data-ttu-id="fcdf8-262">Для сущностей `Course` пользователь указывает первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-262">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="fcdf8-263">Например, номер курса, такой как серия 1000 для кафедры математики и серия 2000 для кафедры английского языка.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-263">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="fcdf8-264">Атрибут `DatabaseGenerated` также может использоваться для создания значений по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-264">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="fcdf8-265">Например, база данных может автоматически создать поле даты для записи даты, когда строка была создана или изменена.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-265">For example, the database can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="fcdf8-266">Дополнительные сведения см. в разделе [Созданные свойства](/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-266">For more information, see [Generated Properties](/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="fcdf8-267">Свойства внешнего ключа и навигации</span><span class="sxs-lookup"><span data-stu-id="fcdf8-267">Foreign key and navigation properties</span></span>

<span data-ttu-id="fcdf8-268">Свойства внешнего ключа (FK) и свойства навигации в сущности `Course` отражают следующие связи:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-268">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="fcdf8-269">Курс назначается одной кафедре, поэтому имеется внешний ключ `DepartmentID` и свойство навигации `Department`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-269">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="fcdf8-270">На курс может быть зачислено любое количество учащихся, поэтому свойство навигации `Enrollments` является коллекцией:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-270">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="fcdf8-271">Курс могут вести несколько преподавателей, поэтому свойство навигации `CourseAssignments` является коллекцией:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-271">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="fcdf8-272">`CourseAssignment` описано [далее](#many-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-272">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="the-department-entity"></a><span data-ttu-id="fcdf8-273">Сущность Department</span><span class="sxs-lookup"><span data-stu-id="fcdf8-273">The Department entity</span></span>

![Сущность Department](complex-data-model/_static/department-entity.png)

<span data-ttu-id="fcdf8-275">Создайте файл *Models/Department.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-275">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Models/Department1.cs)]

### <a name="the-column-attribute"></a><span data-ttu-id="fcdf8-276">Атрибут Column</span><span class="sxs-lookup"><span data-stu-id="fcdf8-276">The Column attribute</span></span>

<span data-ttu-id="fcdf8-277">Ранее атрибут `Column` использовался, чтобы изменить сопоставление имени столбца.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-277">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="fcdf8-278">В коде для сущности `Department` атрибут `Column` можно использовать, чтобы изменить сопоставление типов данных SQL.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-278">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="fcdf8-279">Столбец `Budget` определяется с помощью типа money SQL Server в базе данных:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-279">The `Budget` column is defined using the SQL Server money type in the database:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="fcdf8-280">Сопоставление столбцов обычно не требуется.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-280">Column mapping is generally not required.</span></span> <span data-ttu-id="fcdf8-281">EF Core выбирает соответствующий тип данных SQL Server на основе типа среды CLR для свойства.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-281">EF Core chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="fcdf8-282">Тип `decimal` среды CLR сопоставляется с типом `decimal` SQL Server.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-282">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="fcdf8-283">`Budget` используется для денежных единиц, хотя для этого лучше подходит тип данных money.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-283">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="fcdf8-284">Свойства внешнего ключа и навигации</span><span class="sxs-lookup"><span data-stu-id="fcdf8-284">Foreign key and navigation properties</span></span>

<span data-ttu-id="fcdf8-285">Свойства внешнего ключа и навигации отражают следующие связи:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-285">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="fcdf8-286">Кафедра может иметь или не иметь администратора.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-286">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="fcdf8-287">Администратор всегда является преподавателем.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-287">An administrator is always an instructor.</span></span> <span data-ttu-id="fcdf8-288">Поэтому свойство `InstructorID` включается в качестве внешнего ключа в сущность `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-288">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="fcdf8-289">Свойство навигации называется `Administrator`, но содержит сущность `Instructor`:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-289">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="fcdf8-290">Вопросительный знак (?) в приведенном выше коде указывает, что свойство допускает значение null.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-290">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="fcdf8-291">Кафедра может иметь несколько курсов, поэтому доступно свойство навигации Courses:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-291">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="fcdf8-292">По соглашению EF Core разрешает каскадное удаление для внешних ключей, не допускающих значение null, и связей "многие ко многим".</span><span class="sxs-lookup"><span data-stu-id="fcdf8-292">By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="fcdf8-293">Это поведение по умолчанию может привести к циклическим правилам каскадного удаления.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-293">This default behavior can result in circular cascade delete rules.</span></span> <span data-ttu-id="fcdf8-294">Такие правила вызывают исключение при добавлении миграции.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-294">Circular cascade delete rules cause an exception when a migration is added.</span></span>

<span data-ttu-id="fcdf8-295">Например, если свойство `Department.InstructorID` было определено как не допускающее значения NULL, EF Core настроит правило каскадного удаления.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-295">For example, if the `Department.InstructorID` property was defined as non-nullable, EF Core would configure a cascade delete rule.</span></span> <span data-ttu-id="fcdf8-296">В этом случае кафедра будет удалена, если будет удален преподаватель, назначенный ее администратором.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-296">In that case, the department would be deleted when the instructor assigned as its administrator is deleted.</span></span> <span data-ttu-id="fcdf8-297">В такой ситуации правило ограничения будет более целесообразным.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-297">In this scenario, a restrict rule would make more sense.</span></span> <span data-ttu-id="fcdf8-298">Приведенный ниже [текучий API](#fluent-api-alternative-to-attributes) задает правило ограничения и отключает правило каскадного удаления.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-298">The following [fluent API](#fluent-api-alternative-to-attributes) would set a restrict rule and disable cascade delete.</span></span>

  ```csharp
  modelBuilder.Entity<Department>()
     .HasOne(d => d.Administrator)
     .WithMany()
     .OnDelete(DeleteBehavior.Restrict)
  ```

## <a name="the-enrollment-entity"></a><span data-ttu-id="fcdf8-299">Сущность Enrollment</span><span class="sxs-lookup"><span data-stu-id="fcdf8-299">The Enrollment entity</span></span>

<span data-ttu-id="fcdf8-300">Запись зачисления обозначает один курс, который проходит один учащийся.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-300">An enrollment record is for one course taken by one student.</span></span>

![Сущность Enrollment](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="fcdf8-302">Обновите *Models/Enrollment.cs*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-302">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Enrollment.cs?highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="fcdf8-303">Свойства внешнего ключа и навигации</span><span class="sxs-lookup"><span data-stu-id="fcdf8-303">Foreign key and navigation properties</span></span>

<span data-ttu-id="fcdf8-304">Свойства внешнего ключа и навигации отражают следующие связи:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-304">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="fcdf8-305">Запись зачисления предназначена для одного курса, поэтому доступно свойство первичного ключа `CourseID` и свойство навигации `Course`:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-305">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="fcdf8-306">Запись зачисления предназначена для одного учащегося, поэтому доступно свойство первичного ключа `StudentID` и свойство навигации `Student`:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-306">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="fcdf8-307">Связи многие ко многим</span><span class="sxs-lookup"><span data-stu-id="fcdf8-307">Many-to-Many Relationships</span></span>

<span data-ttu-id="fcdf8-308">Между сущностями `Student` и `Course` действует связь многие ко многим.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-308">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="fcdf8-309">Сущности `Enrollment` выступают в качестве таблицы соединения многие ко многим *с полезными данными* в базе данных.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-309">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="fcdf8-310">Фраза "с полезными данными" означает, что таблица `Enrollment` содержит дополнительные данные, кроме внешних ключей для присоединяемых таблиц (в данном случае — первичный ключ и `Grade`).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-310">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="fcdf8-311">На следующем рисунке показано, как выглядят эти связи на схеме сущностей.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-311">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="fcdf8-312">(Эта схема была создана с помощью [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) для EF 6.x.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-312">(This diagram was generated using [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) for EF 6.x.</span></span> <span data-ttu-id="fcdf8-313">Создание схемы не является частью руководства.)</span><span class="sxs-lookup"><span data-stu-id="fcdf8-313">Creating the diagram isn't part of the tutorial.)</span></span>

![Связь многие ко многим между Student и Course](complex-data-model/_static/student-course.png)

<span data-ttu-id="fcdf8-315">Каждая линия связи имеет 1 на одном конце и звездочку (\*) на другом, указывая характер один ко многим.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-315">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="fcdf8-316">Если таблица `Enrollment` не включала в себя сведения об оценках, ей потребуется содержать всего два внешних ключа (`CourseID` и `StudentID`).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-316">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="fcdf8-317">Таблицу соединения многие ко многим без полезных данных иногда называют чистой таблицей соединения (PJT).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-317">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="fcdf8-318">Сущности `Instructor` и `Course` имеют связь многие ко многим с использованием чистой таблицы соединения.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-318">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="fcdf8-319">Примечание. EF 6.x поддерживает неявные таблицы соединения для связей многие ко многим, но EF Core — нет.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-319">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="fcdf8-320">Дополнительные сведения см. в статье [Связи многие ко многим в EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-320">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="fcdf8-321">Сущность CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="fcdf8-321">The CourseAssignment entity</span></span>

![Сущность CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="fcdf8-323">Создайте файл *Models/CourseAssignment.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-323">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/CourseAssignment.cs)]

<span data-ttu-id="fcdf8-324">Для связи "многие ко многим" между Instructor и Courses нужна таблица соединения, сущностью для которой является CourseAssignment.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-324">The Instructor-to-Courses many-to-many relationship requires a join table, and the entity for that join table is CourseAssignment.</span></span>

![Связь многие ко многим между Instructor и Courses](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="fcdf8-326">Обычно для сущности соединения используется имя `EntityName1EntityName2`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-326">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="fcdf8-327">Например, таблицей соединения Instructor и Courses, использующей этот шаблон, будет `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-327">For example, the Instructor-to-Courses join table using this pattern would be `CourseInstructor`.</span></span> <span data-ttu-id="fcdf8-328">Однако рекомендуется использовать имя, которое описывает эту связь.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-328">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="fcdf8-329">Модели данных создаются простыми и разрастаются.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-329">Data models start out simple and grow.</span></span> <span data-ttu-id="fcdf8-330">Таблицы соединения без полезных данных (PJT) часто начинают включать их.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-330">Join tables without payload (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="fcdf8-331">Если в начале задать описательное имя сущности, его не нужно менять при изменениях таблицы соединения.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-331">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="fcdf8-332">Оптимально, если сущность соединения имеет собственное естественное имя (возможно, из одного слова) в бизнес-среде.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-332">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="fcdf8-333">Например, Books и Customers можно связать с сущностью соединения Ratings.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-333">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="fcdf8-334">Связь многие ко многим между Instructor и Courses `CourseAssignment` является более предпочтительным вариантом, чем `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-334">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="fcdf8-335">Составной ключ</span><span class="sxs-lookup"><span data-stu-id="fcdf8-335">Composite key</span></span>

<span data-ttu-id="fcdf8-336">Два внешних ключа в `CourseAssignment` (`InstructorID` и `CourseID`) совместно однозначно определяют каждую строку таблицы `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-336">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="fcdf8-337">`CourseAssignment` не требуется выделенный первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-337">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="fcdf8-338">Свойства `InstructorID` и `CourseID` выступают в качестве составного первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-338">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="fcdf8-339">Единственным способом указать составные первичные ключи для EF Core является *текучий API*.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-339">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="fcdf8-340">Следующий раздел описывает, как настроить составной первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-340">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="fcdf8-341">Составной ключ обеспечивает следующее:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-341">The composite key ensures that:</span></span>

* <span data-ttu-id="fcdf8-342">Для одного курса допускается несколько строк.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-342">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="fcdf8-343">Для одного преподавателя допускается несколько строк.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-343">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="fcdf8-344">Несколько строк для одного преподавателя и курса недопустимы.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-344">Multiple rows aren't allowed for the same instructor and course.</span></span>

<span data-ttu-id="fcdf8-345">Сущность соединения `Enrollment` определяет собственный первичный ключ, поэтому подобные дубликаты возможны.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-345">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="fcdf8-346">Для предотвращения подобных дубликатов:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-346">To prevent such duplicates:</span></span>

* <span data-ttu-id="fcdf8-347">добавьте уникальный индекс для полей внешнего ключа или</span><span class="sxs-lookup"><span data-stu-id="fcdf8-347">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="fcdf8-348">настройте `Enrollment` с первичным составным ключом аналогично `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-348">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="fcdf8-349">Дополнительные сведения см. в разделе [Индексы](/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-349">For more information, see [Indexes](/ef/core/modeling/indexes).</span></span>

## <a name="update-the-database-context"></a><span data-ttu-id="fcdf8-350">Обновление контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="fcdf8-350">Update the database context</span></span>

<span data-ttu-id="fcdf8-351">Обновите файл *Data/SchoolContext.cs*, добавив в него следующий код:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-351">Update *Data/SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Data/SchoolContext.cs?highlight=15-18,25-31)]

<span data-ttu-id="fcdf8-352">Предыдущий код добавляет новые сущности и настраивает составной первичный ключ сущности `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-352">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="fcdf8-353">Текучий API вместо атрибутов</span><span class="sxs-lookup"><span data-stu-id="fcdf8-353">Fluent API alternative to attributes</span></span>

<span data-ttu-id="fcdf8-354">Метод `OnModelCreating` в предыдущем коде использует для настройки поведения EF Core *текучий API*.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-354">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="fcdf8-355">Этот API называется "текучим", так как часто используется для объединения серии вызовов методов в один оператор.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-355">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="fcdf8-356">В [следующем коде](/ef/core/modeling/#use-fluent-api-to-configure-a-model) показан пример текучего API:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-356">The [following code](/ef/core/modeling/#use-fluent-api-to-configure-a-model) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="fcdf8-357">В этом учебнике текучий API используется только для сопоставления базы данных, которое невозможно выполнить с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-357">In this tutorial, the fluent API is used only for database mapping that can't be done with attributes.</span></span> <span data-ttu-id="fcdf8-358">Однако текучий API позволяет задать большинство правил форматирования, проверки и сопоставления, которые можно указать с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-358">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="fcdf8-359">Некоторые атрибуты, такие как `MinimumLength`, невозможно применить с текучим API.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-359">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="fcdf8-360">`MinimumLength` не изменяет схему, он лишь применяет правило проверки минимальной длины.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-360">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="fcdf8-361">Некоторые разработчики предпочитают использовать текучий API монопольно, чтобы оставить свои классы сущностей "чистыми".</span><span class="sxs-lookup"><span data-stu-id="fcdf8-361">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="fcdf8-362">Атрибуты и текучий API можно смешивать.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-362">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="fcdf8-363">Существует несколько конфигураций, которые можно реализовать только с помощью текучего API (с указанием составного первичного ключа).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-363">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="fcdf8-364">Существует несколько конфигураций, которые можно реализовать только с помощью атрибутов (`MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-364">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="fcdf8-365">Рекомендации по использованию текучего API и атрибутов:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-365">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="fcdf8-366">Выберите один из двух этих подходов.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-366">Choose one of these two approaches.</span></span>
* <span data-ttu-id="fcdf8-367">Используйте выбранный подход максимально согласованно.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-367">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="fcdf8-368">Некоторые атрибуты из этого руководства используются:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-368">Some of the attributes used in this tutorial are used for:</span></span>

* <span data-ttu-id="fcdf8-369">только для проверки (например, `MinimumLength`);</span><span class="sxs-lookup"><span data-stu-id="fcdf8-369">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="fcdf8-370">только для конфигурации EF Core (например, `HasKey`);</span><span class="sxs-lookup"><span data-stu-id="fcdf8-370">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="fcdf8-371">для проверки и конфигурации EF Core (например, `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-371">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="fcdf8-372">Дополнительные сведения о сравнении атрибутов и текучего API см. в разделе [Методы конфигурации](/ef/core/modeling/).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-372">For more information about attributes vs. fluent API, see [Methods of configuration](/ef/core/modeling/).</span></span>

## <a name="entity-diagram"></a><span data-ttu-id="fcdf8-373">Схема сущностей</span><span class="sxs-lookup"><span data-stu-id="fcdf8-373">Entity diagram</span></span>

<span data-ttu-id="fcdf8-374">Ниже показана схема, создаваемая средствами EF Power Tools для завершенной модели School.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-374">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Схема сущностей](complex-data-model/_static/diagram.png)

<span data-ttu-id="fcdf8-376">На предыдущей схеме показано следующее:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-376">The preceding diagram shows:</span></span>

* <span data-ttu-id="fcdf8-377">Несколько линий связи один ко многим (1 к \*).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-377">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="fcdf8-378">Линия связи один к нулю или одному (1 к 0..1) между сущностями `Instructor` и `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-378">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="fcdf8-379">Линия связи нуль или один ко многим (0..1 к \*) между сущностями `Instructor` и `Department`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-379">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-database"></a><span data-ttu-id="fcdf8-380">Заполнение базы данных</span><span class="sxs-lookup"><span data-stu-id="fcdf8-380">Seed the database</span></span>

<span data-ttu-id="fcdf8-381">Обновите код в файле *Data/DbInitializer.cs*:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-381">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[](intro/samples/cu30/Data/DbInitializer.cs)]

<span data-ttu-id="fcdf8-382">Предыдущий код предоставляет начальные данные для новых сущностей.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-382">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="fcdf8-383">Основная часть кода создает объекты сущностей и загружает демонстрационные данные.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-383">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="fcdf8-384">Демонстрационные данные используются для проверки.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-384">The sample data is used for testing.</span></span> <span data-ttu-id="fcdf8-385">См. `Enrollments` и `CourseAssignments`, чтобы ознакомиться с примерами заполнения данными таблиц соединения "многие ко многим".</span><span class="sxs-lookup"><span data-stu-id="fcdf8-385">See `Enrollments` and `CourseAssignments` for examples of how many-to-many join tables can be seeded.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="fcdf8-386">Добавление миграции</span><span class="sxs-lookup"><span data-stu-id="fcdf8-386">Add a migration</span></span>

<span data-ttu-id="fcdf8-387">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-387">Build the project.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fcdf8-388">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fcdf8-388">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fcdf8-389">В PMC выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-389">In PMC, run the following command.</span></span>

```powershell
Add-Migration ComplexDataModel
```

<span data-ttu-id="fcdf8-390">Предыдущая команда отображает предупреждение о возможной потере данных.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-390">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="fcdf8-391">При выполнении команды `database update` возникает следующая ошибка:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-391">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

<span data-ttu-id="fcdf8-392">В следующем разделе вы узнаете, что делать с этой ошибкой.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-392">In the next section, you see what to do about this error.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fcdf8-393">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fcdf8-393">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="fcdf8-394">Если добавить миграцию и выполнить команду `database update`, произойдет следующая ошибка:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-394">If you add a migration and run the `database update` command, the following error would be produced:</span></span>

```text
SQLite does not support this migration operation ('DropForeignKeyOperation').
For more information, see http://go.microsoft.com/fwlink/?LinkId=723262.
```

<span data-ttu-id="fcdf8-395">В следующем разделе вы узнаете, как ее избежать.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-395">In the next section, you see how to avoid this error.</span></span>

---

## <a name="apply-the-migration-or-drop-and-re-create"></a><span data-ttu-id="fcdf8-396">Применение миграции либо удаление и повторное создание</span><span class="sxs-lookup"><span data-stu-id="fcdf8-396">Apply the migration or drop and re-create</span></span>

<span data-ttu-id="fcdf8-397">Теперь у вас есть база данных, и пора подумать о том, как применять к ней изменения.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-397">Now that you have an existing database, you need to think about how to apply changes to it.</span></span> <span data-ttu-id="fcdf8-398">В этом руководстве показано два подхода:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-398">This tutorial shows two alternatives:</span></span>

* <span data-ttu-id="fcdf8-399">[Удаление и повторное создание базы данных](#drop).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-399">[Drop and re-create the database](#drop).</span></span> <span data-ttu-id="fcdf8-400">Выберите этот раздел, если вы используете SQLite.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-400">Choose this section if you're using SQLite.</span></span>
* <span data-ttu-id="fcdf8-401">[Применение миграции к существующей базе данных](#applyexisting).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-401">[Apply the migration to the existing database](#applyexisting).</span></span> <span data-ttu-id="fcdf8-402">Инструкции в этом разделе подходят только для SQL Server, но **не для SQLite**.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-402">The instructions in this section work for SQL Server only, **not for SQLite**.</span></span> 

<span data-ttu-id="fcdf8-403">Для SQL Server применимы оба подхода.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-403">Either choice works for SQL Server.</span></span> <span data-ttu-id="fcdf8-404">Хотя метод с применением миграции является более сложным и трудоемким, в реальной рабочей среде лучше использовать именно его.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-404">While the apply-migration method is more complex and time-consuming, it's the preferred approach for real-world, production environments.</span></span> 

<a name="drop"></a>

## <a name="drop-and-re-create-the-database"></a><span data-ttu-id="fcdf8-405">Удаление и повторное создание базы данных</span><span class="sxs-lookup"><span data-stu-id="fcdf8-405">Drop and re-create the database</span></span>

<span data-ttu-id="fcdf8-406">[Пропустите этот раздел](#apply-the-migration), если вы используете SQL Server и хотите применить миграцию в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-406">[Skip this section](#apply-the-migration) if you're using SQL Server and want to do the apply-migration approach in the following section.</span></span>

<span data-ttu-id="fcdf8-407">Чтобы в EF Core принудительно создать базу данных, удалить, а затем обновить ее, выполните указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-407">To force EF Core to create a new database, drop and update the database:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fcdf8-408">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fcdf8-408">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="fcdf8-409">Выполните следующие команды в **консоли диспетчера пакетов** (PMC):</span><span class="sxs-lookup"><span data-stu-id="fcdf8-409">In the **Package Manager Console** (PMC), run the following command:</span></span>

  ```powershell
  Drop-Database
  ```

* <span data-ttu-id="fcdf8-410">Удалите папку *Migrations*, а затем выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-410">Delete the *Migrations* folder, then run the following command:</span></span>

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fcdf8-411">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fcdf8-411">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="fcdf8-412">Откройте командное окно и перейдите в папку проекта.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-412">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="fcdf8-413">Папка проекта содержит файл *ContosoUniversity.csproj*.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-413">The project folder contains the *ContosoUniversity.csproj* file.</span></span>

* <span data-ttu-id="fcdf8-414">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-414">Run the following command:</span></span>

  ```dotnetcli
  dotnet ef database drop --force
  ```

* <span data-ttu-id="fcdf8-415">Удалите папку *Migrations*, а затем выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-415">Delete the *Migrations* folder, then run the following command:</span></span>

  ```dotnetcli
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

---

<span data-ttu-id="fcdf8-416">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-416">Run the app.</span></span> <span data-ttu-id="fcdf8-417">При запуске приложения выполняется метод `DbInitializer.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-417">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="fcdf8-418">`DbInitializer.Initialize` заполняет новую базу данных.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-418">The `DbInitializer.Initialize` populates the new database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fcdf8-419">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fcdf8-419">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fcdf8-420">Откройте базу данных в SSOX.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-420">Open the database in SSOX:</span></span>

* <span data-ttu-id="fcdf8-421">Если SSOX был открыт ранее, нажмите кнопку **Обновить**.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-421">If SSOX was opened previously, click the **Refresh** button.</span></span>
* <span data-ttu-id="fcdf8-422">Разверните узел **Таблицы**.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-422">Expand the **Tables** node.</span></span> <span data-ttu-id="fcdf8-423">Отображаются созданные таблицы.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-423">The created tables are displayed.</span></span>

  ![Таблицы в SSOX](complex-data-model/_static/ssox-tables.png)

* <span data-ttu-id="fcdf8-425">Изучите таблицу **CourseAssignment**:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-425">Examine the **CourseAssignment** table:</span></span>

  * <span data-ttu-id="fcdf8-426">Щелкните правой кнопкой мыши таблицу **CourseAssignment** и выберите пункт **Просмотреть данные**.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-426">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
  * <span data-ttu-id="fcdf8-427">Убедитесь, что таблица **CourseAssignment** содержит данные.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-427">Verify the **CourseAssignment** table contains data.</span></span>

  ![Данные CourseAssignment в SSOX](complex-data-model/_static/ssox-ci-data.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fcdf8-429">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fcdf8-429">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="fcdf8-430">Используйте средство SQLite для просмотра базы данных:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-430">Use your SQLite tool to examine the database:</span></span>

* <span data-ttu-id="fcdf8-431">новые таблицы и столбцы;</span><span class="sxs-lookup"><span data-stu-id="fcdf8-431">New tables and columns.</span></span>
* <span data-ttu-id="fcdf8-432">заполненные данные в таблицах, например в таблице **CourseAssignment**.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-432">Seeded data in tables, for example the **CourseAssignment** table.</span></span>

---

<a name="applyexisting"></a>

## <a name="apply-the-migration"></a><span data-ttu-id="fcdf8-433">Применение миграции</span><span class="sxs-lookup"><span data-stu-id="fcdf8-433">Apply the migration</span></span>

<span data-ttu-id="fcdf8-434">Этот раздел является необязательным.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-434">This section is optional.</span></span> <span data-ttu-id="fcdf8-435">Эти действия подходят только для SQL Server LocalDB и только в том случае, если вы пропустили предыдущий раздел [Удаление и повторное создание базы данных](#drop).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-435">These steps work only for SQL Server LocalDB and only if you skipped the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="fcdf8-436">При выполнении миграций с существующими данными могут действовать ограничения внешнего ключа, которым существующие данные не соответствуют.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-436">When migrations are run with existing data, there may be FK constraints that are not satisfied with the existing data.</span></span> <span data-ttu-id="fcdf8-437">Для рабочих данных нужно предпринять меры по переносу существующих данных.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-437">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="fcdf8-438">Этот раздел содержит пример того, как устранить нарушения ограничений внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-438">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="fcdf8-439">Вносите эти изменения кода только после создания резервной копии.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-439">Don't make these code changes without a backup.</span></span> <span data-ttu-id="fcdf8-440">Не вносите эти изменения в код, если вы выполнили инструкции из предыдущего раздела [Удаление и повторное создание базы данных](#drop).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-440">Don't make these code changes if you completed the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="fcdf8-441">Файл *{метка_времени}_ComplexDataModel.cs* содержит следующий код:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-441">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="fcdf8-442">Предыдущий код добавляет в таблицу `Course` внешний ключ `DepartmentID`, не допускающий значение null.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-442">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="fcdf8-443">База данных из предыдущего учебника содержит строки в `Course`, поэтому эту таблицу невозможно обновить с помощью миграций.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-443">The database from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="fcdf8-444">Чтобы заставить миграцию `ComplexDataModel` работать с существующими данными:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-444">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="fcdf8-445">Изменить код, чтобы присвоить новому столбцу (`DepartmentID`) значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-445">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="fcdf8-446">Создайте фиктивную кафедру с именем "Temp" для использования по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-446">Create a fake department named "Temp" to act as the default department.</span></span>

#### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="fcdf8-447">Устранение ограничений внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="fcdf8-447">Fix the foreign key constraints</span></span>

<span data-ttu-id="fcdf8-448">В классе миграции `Up` обновите метод `ComplexDataModel`:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-448">In the `ComplexDataModel` migration class, update the `Up` method:</span></span>

* <span data-ttu-id="fcdf8-449">Откройте файл *{метка_времени}_ComplexDataModel.cs*.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-449">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="fcdf8-450">Закомментируйте строку кода, которая добавляет столбец `DepartmentID` в таблицу `Course`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-450">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="fcdf8-451">Добавьте выделенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-451">Add the following highlighted code.</span></span> <span data-ttu-id="fcdf8-452">Новый код идет после блока `.CreateTable( name: "Department"`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-452">The new code goes after the `.CreateTable( name: "Department"` block:</span></span>

<span data-ttu-id="fcdf8-453">[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=23-31)]</span><span class="sxs-lookup"><span data-stu-id="fcdf8-453">[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=23-31)]</span></span>

<span data-ttu-id="fcdf8-454">После внесения описанных выше изменений существующие строки `Course` будут связаны с кафедрой "Temp" после выполнения метода `ComplexDataModel.Up`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-454">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel.Up` method runs.</span></span>

<span data-ttu-id="fcdf8-455">Инструкции на случай описанной здесь ситуации упрощены в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-455">The way of handling the situation shown here is simplified for this tutorial.</span></span> <span data-ttu-id="fcdf8-456">Рабочее приложение:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-456">A production app would:</span></span>

* <span data-ttu-id="fcdf8-457">включает код или сценарии, чтобы добавить строки `Department` и связанные строки `Course` к новым строкам `Department`;</span><span class="sxs-lookup"><span data-stu-id="fcdf8-457">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="fcdf8-458">не использует кафедру "Temp" или значение по умолчанию для `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-458">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fcdf8-459">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fcdf8-459">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="fcdf8-460">Выполните следующие команды в **консоли диспетчера пакетов** (PMC):</span><span class="sxs-lookup"><span data-stu-id="fcdf8-460">In the **Package Manager Console** (PMC), run the following command:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="fcdf8-461">Так как метод `DbInitializer.Initialize` предназначен для работы только с пустой базой данных, используйте SSOX для удаления всех строк в таблицах Student и Course.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-461">Because the `DbInitializer.Initialize` method is designed to work only with an empty database, use SSOX to delete all the rows in the Student and Course tables.</span></span> <span data-ttu-id="fcdf8-462">(К таблице Enrollment будет применено каскадное удаление.)</span><span class="sxs-lookup"><span data-stu-id="fcdf8-462">(Cascade delete will take care of the Enrollment table.)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fcdf8-463">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fcdf8-463">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="fcdf8-464">Если вы используете SQL Server LocalDB с Visual Studio Code, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-464">If you're using SQL Server LocalDB with Visual Studio Code, run the following command:</span></span>

  ```dotnetcli
  dotnet ef database update
  ```

---

<span data-ttu-id="fcdf8-465">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-465">Run the app.</span></span> <span data-ttu-id="fcdf8-466">При запуске приложения выполняется метод `DbInitializer.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-466">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="fcdf8-467">`DbInitializer.Initialize` заполняет новую базу данных.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-467">The `DbInitializer.Initialize` populates the new database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fcdf8-468">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="fcdf8-468">Next steps</span></span>

<span data-ttu-id="fcdf8-469">В следующих двух учебниках рассказывается о том, как считывать и обновлять связанные данные.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-469">The next two tutorials show how to read and update related data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fcdf8-470">[Предыдущий учебник](xref:data/ef-rp/migrations)
> [Следующий учебник](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="fcdf8-470">[Previous tutorial](xref:data/ef-rp/migrations)
[Next tutorial](xref:data/ef-rp/read-related-data)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="fcdf8-471">Предыдущие руководства работали с базовой моделью данных, состоящей из трех сущностей.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-471">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="fcdf8-472">В этом руководстве:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-472">In this tutorial:</span></span>

* <span data-ttu-id="fcdf8-473">Добавляются дополнительные сущности и связи.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-473">More entities and relationships are added.</span></span>
* <span data-ttu-id="fcdf8-474">Настраивается модель данных с помощью указания правил для форматирования, проверки и сопоставления базы данных.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-474">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="fcdf8-475">Классы сущностей для готовой модели данных показаны на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-475">The entity classes for the completed data model are shown in the following illustration:</span></span>

![Схема сущностей](complex-data-model/_static/diagram.png)

<span data-ttu-id="fcdf8-477">При возникновении проблем, которые вам не удается устранить, скачайте [готовое приложение](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-477">If you run into problems you can't solve, download the [completed app](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span>

## <a name="customize-the-data-model-with-attributes"></a><span data-ttu-id="fcdf8-478">Настройка модели данных с использованием атрибутов</span><span class="sxs-lookup"><span data-stu-id="fcdf8-478">Customize the data model with attributes</span></span>

<span data-ttu-id="fcdf8-479">В этом разделе модель данных настраивается с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-479">In this section, the data model is customized using attributes.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="fcdf8-480">Атрибут DataType</span><span class="sxs-lookup"><span data-stu-id="fcdf8-480">The DataType attribute</span></span>

<span data-ttu-id="fcdf8-481">Страницы учащихся сейчас отображают время и дату зачисления.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-481">The student pages currently displays the time of the enrollment date.</span></span> <span data-ttu-id="fcdf8-482">Как правило, поля даты отображают только дату без времени.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-482">Typically, date fields show only the date and not the time.</span></span>

<span data-ttu-id="fcdf8-483">Обновите *Models/Student.cs*, используя следующий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-483">Update *Models/Student.cs* with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="fcdf8-484">Атрибут [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) указывает тип данных более точно по сравнению со встроенным типом базы данных.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-484">The [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="fcdf8-485">В этом случае следует отображать отобразить только дату, а не дату и время.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-485">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="fcdf8-486">В [перечислении DataType](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) представлено множество типов данных, таких как Date, Time, PhoneNumber, Currency, EmailAddress и других. Атрибут `DataType` также обеспечивает автоматическое предоставление функций для определенных типов в приложении.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-486">The [DataType Enumeration](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="fcdf8-487">Например:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-487">For example:</span></span>

* <span data-ttu-id="fcdf8-488">Ссылка `mailto:` для `DataType.EmailAddress` создается автоматически.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-488">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="fcdf8-489">Средство выбора даты предоставляется для `DataType.Date` в большинстве браузеров.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-489">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="fcdf8-490">Атрибут `DataType` создает атрибуты HTML 5 `data-`, которые используются браузерами с поддержкой HTML 5.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-490">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="fcdf8-491">Атрибуты `DataType` не предназначены для проверки.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-491">The `DataType` attributes don't provide validation.</span></span>

<span data-ttu-id="fcdf8-492">`DataType.Date` не задает формат отображаемой даты.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-492">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="fcdf8-493">По умолчанию поле даты отображается с использованием форматов, установленных в [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support) сервера.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-493">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="fcdf8-494">С помощью атрибута `DisplayFormat` можно явно указать формат даты:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-494">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="fcdf8-495">Параметр `ApplyFormatInEditMode` указывает, что формат должен применяться к пользовательскому интерфейсу редактирования.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-495">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="fcdf8-496">Некоторым полям не следует использовать `ApplyFormatInEditMode`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-496">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="fcdf8-497">Например, обозначение денежной единицы в общем случае не должно отображаться в поле редактирования текста.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-497">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="fcdf8-498">Атрибут `DisplayFormat` можно использовать отдельно.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-498">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="fcdf8-499">Однако чаще всего `DataType` рекомендуется применять вместе с атрибутом `DisplayFormat`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-499">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="fcdf8-500">Атрибут `DataType` передает семантику данных (в отличие от способа их вывода на экран).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-500">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="fcdf8-501">Атрибут `DataType` дает следующие преимущества, недоступные в `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-501">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="fcdf8-502">Поддержка функций HTML5 в браузере.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-502">The browser can enable HTML5 features.</span></span> <span data-ttu-id="fcdf8-503">Например, отображение элемента управления календарем, соответствующего языковому стандарту обозначения денежной единицы, ссылок электронной почты, проверки входных данных на стороне клиента и т. д.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-503">For example, show a calendar control, the locale-appropriate currency symbol, email links, client-side input validation, etc.</span></span>
* <span data-ttu-id="fcdf8-504">По умолчанию формат отображения данных в браузере определяется в соответствии с установленным языковым стандартом.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-504">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="fcdf8-505">Дополнительные сведения см. в документации по [вспомогательной функции тегов \<input>](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-505">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

<span data-ttu-id="fcdf8-506">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-506">Run the app.</span></span> <span data-ttu-id="fcdf8-507">Перейдите на страницу индекса учащихся.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-507">Navigate to the Students Index page.</span></span> <span data-ttu-id="fcdf8-508">Время больше не отображается.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-508">Times are no longer displayed.</span></span> <span data-ttu-id="fcdf8-509">Каждое представление, использующее модель `Student`, отображает дату без времени.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-509">Every view that uses the `Student` model displays the date without time.</span></span>

![Страница указателя учащихся с датами без времени](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="fcdf8-511">Атрибут StringLength</span><span class="sxs-lookup"><span data-stu-id="fcdf8-511">The StringLength attribute</span></span>

<span data-ttu-id="fcdf8-512">С помощью атрибутов можно указать правила проверки данных и сообщения об ошибках проверки.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-512">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="fcdf8-513">Атрибут [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) указывает минимальную и максимальную длину символов, разрешенных в поле данных.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-513">The [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="fcdf8-514">Атрибут `StringLength` также обеспечивает проверку на стороне клиента и на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-514">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="fcdf8-515">Минимальное значение не оказывает влияния на схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-515">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="fcdf8-516">Обновите модель`Student`, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-516">Update the `Student` model with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="fcdf8-517">Предыдущий код задает ограничение длины имен в 50 символов.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-517">The preceding code limits names to no more than 50 characters.</span></span> <span data-ttu-id="fcdf8-518">Атрибут `StringLength` не запрещает пользователю ввести пробел в качестве имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-518">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="fcdf8-519">Атрибут [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) используется для применения ограничений к входным данным.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-519">The [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute is used to apply restrictions to the input.</span></span> <span data-ttu-id="fcdf8-520">Например, следующий код требует, чтобы первый символ был прописной буквой, а остальные символы были буквенными:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-520">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

<span data-ttu-id="fcdf8-521">Запустите приложение:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-521">Run the app:</span></span>

* <span data-ttu-id="fcdf8-522">Перейдите на страницу учащихся.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-522">Navigate to the Students page.</span></span>
* <span data-ttu-id="fcdf8-523">Выберите **Create New** (Создать) и введите имя длиной более 50 символов.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-523">Select **Create New**, and enter a name longer than 50 characters.</span></span>
* <span data-ttu-id="fcdf8-524">Выберите **Create** (Создать), проверка на стороне клиента отображает сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-524">Select **Create**, client-side validation shows an error message.</span></span>

![Страница указателя учащихся с ошибками длины строки](complex-data-model/_static/string-length-errors.png)

<span data-ttu-id="fcdf8-526">В **обозревателе объектов SQL Server** (SSOX) откройте конструктор таблиц учащихся, дважды щелкнув таблицу **Student** (Учащийся).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-526">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Таблица учащихся в SSOX до миграций](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="fcdf8-528">На предыдущем изображении показана схемы для таблицы `Student`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-528">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="fcdf8-529">Поля имен имеют тип `nvarchar(MAX)`, так как миграции для базы данных не выполнялись.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-529">The name fields have type `nvarchar(MAX)` because migrations has not been run on the DB.</span></span> <span data-ttu-id="fcdf8-530">Когда в дальнейшем миграции будут выполнены, поля имен станут `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-530">When migrations are run later in this tutorial, the name fields become `nvarchar(50)`.</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="fcdf8-531">Атрибут Column</span><span class="sxs-lookup"><span data-stu-id="fcdf8-531">The Column attribute</span></span>

<span data-ttu-id="fcdf8-532">Атрибуты могут управлять, как именно классы и свойства сопоставляются с базой данных.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-532">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="fcdf8-533">В этом разделе атрибут `Column` используется для сопоставления имени свойства `FirstMidName` с "FirstName" в базе данных.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-533">In this section, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the DB.</span></span>

<span data-ttu-id="fcdf8-534">При создании базы данных имена свойств в модели используются для имен столбцов (кроме случая, когда используется атрибут `Column`).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-534">When the DB is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span>

<span data-ttu-id="fcdf8-535">Модель `Student` использует `FirstMidName` для поля имени, так как это поле также может содержать отчество.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-535">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="fcdf8-536">Обновите файл *Student.cs*, используя следующий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-536">Update the *Student.cs* file with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="fcdf8-537">С учетом предыдущего изменения `Student.FirstMidName` в приложении сопоставляется со столбцом `FirstName` таблицы `Student`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-537">With the preceding change, `Student.FirstMidName` in the app maps to the `FirstName` column of the `Student` table.</span></span>

<span data-ttu-id="fcdf8-538">Добавление атрибута `Column` изменяет модель для поддержки `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-538">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="fcdf8-539">Модель, поддерживающая `SchoolContext`, больше не соответствует базе данных.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-539">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="fcdf8-540">Если приложение запускается перед применением миграций, возникает следующее исключение:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-540">If the app is run before applying migrations, the following exception is generated:</span></span>

```SQL
SqlException: Invalid column name 'FirstName'.
```

<span data-ttu-id="fcdf8-541">Для обновления базы данных сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-541">To update the DB:</span></span>

* <span data-ttu-id="fcdf8-542">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-542">Build the project.</span></span>
* <span data-ttu-id="fcdf8-543">Откройте командное окно в папке проекта.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-543">Open a command window in the project folder.</span></span> <span data-ttu-id="fcdf8-544">Введите следующие команды для создания миграции и обновления базы данных:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-544">Enter the following commands to create a new migration and update the DB:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fcdf8-545">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fcdf8-545">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration ColumnFirstName
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fcdf8-546">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fcdf8-546">Visual Studio Code</span></span>](#tab/visual-studio-code)

```dotnetcli
dotnet ef migrations add ColumnFirstName
dotnet ef database update
```

---

<span data-ttu-id="fcdf8-547">Команда `migrations add ColumnFirstName` выдает следующее предупреждающее сообщение:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-547">The `migrations add ColumnFirstName` command generates the following warning message:</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

<span data-ttu-id="fcdf8-548">Это предупреждение вызвано тем, что поля имен теперь ограничены 50 символами.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-548">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="fcdf8-549">Если имя в базе данных имеет больше 50 символов, символы с 51 до последнего будут потеряны.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-549">If a name in the DB had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="fcdf8-550">Проверьте работу приложения.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-550">Test the app.</span></span>

<span data-ttu-id="fcdf8-551">Откройте таблицу "Student" (Учащийся) в окне SSOX:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-551">Open the Student table in SSOX:</span></span>

![Таблица учащихся в SSOX после миграций](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="fcdf8-553">До применения миграции столбцы имен имели тип [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-553">Before migration was applied, the name columns were of type [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="fcdf8-554">Теперь столбцы имен имеют тип `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-554">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="fcdf8-555">Имя столбца изменилось с `FirstMidName` на `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-555">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

> [!Note]
> <span data-ttu-id="fcdf8-556">В следующем разделе сборка приложения на некоторых этапах приводит к возникновению ошибок компилятора.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-556">In the following section, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="fcdf8-557">В инструкциях указано, когда производить сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-557">The instructions specify when to build the app.</span></span>

## <a name="student-entity-update"></a><span data-ttu-id="fcdf8-558">Обновление сущности Student</span><span class="sxs-lookup"><span data-stu-id="fcdf8-558">Student entity update</span></span>

![Сущность Student](complex-data-model/_static/student-entity.png)

<span data-ttu-id="fcdf8-560">Обновите *Models/Student.cs*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-560">Update *Models/Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="fcdf8-561">Атрибут Required</span><span class="sxs-lookup"><span data-stu-id="fcdf8-561">The Required attribute</span></span>

<span data-ttu-id="fcdf8-562">Атрибут `Required` делает свойства имен обязательными полями.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-562">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="fcdf8-563">Атрибут `Required` не нужен для типов, не допускающих значения null, например для типов значений (`DateTime`, `int`, `double` и т. д.).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-563">The `Required` attribute isn't needed for non-nullable types such as value types (`DateTime`, `int`, `double`, etc.).</span></span> <span data-ttu-id="fcdf8-564">Типы, которые не могут принимать значение null, автоматически обрабатываются как обязательные поля.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-564">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="fcdf8-565">Атрибут `Required` можно заменить параметром минимальной длины в атрибуте `StringLength`:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-565">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="fcdf8-566">Атрибут Display</span><span class="sxs-lookup"><span data-stu-id="fcdf8-566">The Display attribute</span></span>

<span data-ttu-id="fcdf8-567">Атрибут `Display` указывает, что заголовки для текстовых полей должны иметь вид "First Name" (Имя), "Last Name" (Фамилия), "Full Name" (Полное имя) и "Enrollment Date" (Дата зачисления).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-567">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="fcdf8-568">По умолчанию в заголовках не использовался пробел для разделения слов, например "Lastname".</span><span class="sxs-lookup"><span data-stu-id="fcdf8-568">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="fcdf8-569">Вычисляемое свойство FullName</span><span class="sxs-lookup"><span data-stu-id="fcdf8-569">The FullName calculated property</span></span>

<span data-ttu-id="fcdf8-570">`FullName` — это вычисляемое свойство, которое возвращает значение, созданное путем объединения двух других свойств.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-570">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="fcdf8-571">`FullName` не может быть задано, оно имеет только метод доступа get.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-571">`FullName` cannot be set, it has only a get accessor.</span></span> <span data-ttu-id="fcdf8-572">В базе данных не создается столбец `FullName`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-572">No `FullName` column is created in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="fcdf8-573">Создание сущности Instructor</span><span class="sxs-lookup"><span data-stu-id="fcdf8-573">Create the Instructor Entity</span></span>

![Сущность Instructor](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="fcdf8-575">Создайте файл *Models/Instructor.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-575">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Instructor.cs)]

<span data-ttu-id="fcdf8-576">На одной строке могут находиться несколько атрибутов.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-576">Multiple attributes can be on one line.</span></span> <span data-ttu-id="fcdf8-577">Атрибуты `HireDate` можно записать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-577">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="fcdf8-578">Свойства навигации CourseAssignments и OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="fcdf8-578">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="fcdf8-579">`CourseAssignments` и `OfficeAssignment` — это свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-579">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="fcdf8-580">Преподаватель может проводить любое количество курсов, поэтому `CourseAssignments` определен как коллекция.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-580">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="fcdf8-581">Если свойство навигации содержит несколько сущностей:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-581">If a navigation property holds multiple entities:</span></span>

* <span data-ttu-id="fcdf8-582">Оно должно быть типом списка, где можно добавлять, удалять и обновлять записи.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-582">It must be a list type where the entries can be added, deleted, and updated.</span></span>

<span data-ttu-id="fcdf8-583">К типам свойств навигации относятся следующие:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-583">Navigation property types include:</span></span>

* `ICollection<T>`
* `List<T>`
* `HashSet<T>`

<span data-ttu-id="fcdf8-584">Если указан тип `ICollection<T>`, платформа EF Core по умолчанию создает коллекцию `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-584">If `ICollection<T>` is specified, EF Core creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="fcdf8-585">Сущность `CourseAssignment` описана в разделе по связям многие ко многим.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-585">The `CourseAssignment` entity is explained in the section on many-to-many relationships.</span></span>

<span data-ttu-id="fcdf8-586">Бизнес-правила университета Contoso указывают, что преподаватель может иметь не более одного кабинета.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-586">Contoso University business rules state that an instructor can have at most one office.</span></span> <span data-ttu-id="fcdf8-587">Свойство `OfficeAssignment` содержит отдельную сущность `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-587">The `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="fcdf8-588">`OfficeAssignment` имеет значение null, если кабинет не назначен.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-588">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="fcdf8-589">Создание сущности OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="fcdf8-589">Create the OfficeAssignment entity</span></span>

![Сущность OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="fcdf8-591">Создайте файл *Models/OfficeAssignment.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-591">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="fcdf8-592">Атрибут Key</span><span class="sxs-lookup"><span data-stu-id="fcdf8-592">The Key attribute</span></span>

<span data-ttu-id="fcdf8-593">Атрибут `[Key]` используется для идентификации свойства в качестве первичного ключа (PK), когда имя свойства отличается от classnameID или ID.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-593">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="fcdf8-594">Между сущностями `Instructor` и `OfficeAssignment` действует связь один к нулю или к одному.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-594">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="fcdf8-595">Назначение кабинета существует только в связи с преподавателем, которому оно назначено.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-595">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="fcdf8-596">Первичный ключ `OfficeAssignment` также является внешним ключом (FK) для сущности `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-596">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span> <span data-ttu-id="fcdf8-597">EF Core не распознает автоматически `InstructorID` как первичный ключ объекта `OfficeAssignment` по следующей причине:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-597">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because:</span></span>

* <span data-ttu-id="fcdf8-598">`InstructorID` не соблюдает соглашение об именовании ID или classnameID.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-598">`InstructorID` doesn't follow the ID or classnameID naming convention.</span></span>

<span data-ttu-id="fcdf8-599">Таким образом, атрибут `Key` используется для определения `InstructorID` в качестве первичного ключа:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-599">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="fcdf8-600">По умолчанию EF Core считает ключ созданным не базой данных, так как столбец предназначен для идентифицирующего отношения.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-600">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="fcdf8-601">Свойство навигации Instructor</span><span class="sxs-lookup"><span data-stu-id="fcdf8-601">The Instructor navigation property</span></span>

<span data-ttu-id="fcdf8-602">Свойство навигации `OfficeAssignment` для сущности `Instructor` допускает значение null по следующим причинам:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-602">The `OfficeAssignment` navigation property for the `Instructor` entity is nullable because:</span></span>

* <span data-ttu-id="fcdf8-603">Ссылочные типы (например, классы) допускают значение null.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-603">Reference types (such as classes are nullable).</span></span>
* <span data-ttu-id="fcdf8-604">Преподавателю может быть не назначен кабинет.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-604">An instructor might not have an office assignment.</span></span>

<span data-ttu-id="fcdf8-605">Сущность `OfficeAssignment` имеет свойство навигации `Instructor`, не допускающее значение null, по следующим причинам:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-605">The `OfficeAssignment` entity has a non-nullable `Instructor` navigation property because:</span></span>

* <span data-ttu-id="fcdf8-606">`InstructorID` не допускает значение null.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-606">`InstructorID` is non-nullable.</span></span>
* <span data-ttu-id="fcdf8-607">Назначение кабинета не может существовать без преподавателя.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-607">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="fcdf8-608">Когда сущность `Instructor` имеет связанную сущность `OfficeAssignment`, каждая из них имеет ссылку на другую в своем свойстве навигации.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-608">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="fcdf8-609">Атрибут `[Required]` можно применить к свойству навигации `Instructor`:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-609">The `[Required]` attribute could be applied to the `Instructor` navigation property:</span></span>

```csharp
[Required]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="fcdf8-610">Предыдущий код указывает, что должен существовать связанный преподаватель.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-610">The preceding code specifies that there must be a related instructor.</span></span> <span data-ttu-id="fcdf8-611">Этот код не нужен, так как внешний ключ `InstructorID` (который также является первичным) не допускает значение null.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-611">The preceding code is unnecessary because the `InstructorID` foreign key (which is also the PK) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="fcdf8-612">Изменение сущности Course</span><span class="sxs-lookup"><span data-stu-id="fcdf8-612">Modify the Course Entity</span></span>

![Сущность Course](complex-data-model/_static/course-entity.png)

<span data-ttu-id="fcdf8-614">Обновите *Models/Course.cs*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-614">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="fcdf8-615">Сущность `Course` имеет свойство внешнего ключа (FK) `DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-615">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="fcdf8-616">`DepartmentID` указывает на связанную сущность `Department`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-616">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="fcdf8-617">Сущность `Course` имеет свойство навигации `Department`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-617">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="fcdf8-618">EF Core не требует свойство внешнего ключа для модели данных, если она имеет свойство навигации для связанной сущности.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-618">EF Core doesn't require a FK property for a data model when the model has a navigation property for a related entity.</span></span>

<span data-ttu-id="fcdf8-619">EF Core автоматически создает внешние ключи в базе данных по мере необходимости.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-619">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="fcdf8-620">EF Core создает [теневые свойства](/ef/core/modeling/shadow-properties) для автоматически созданных внешних ключей.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-620">EF Core creates [shadow properties](/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="fcdf8-621">Наличие внешнего ключа в модели данных позволяет сделать обновления проще и эффективнее.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-621">Having the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="fcdf8-622">Например, рассмотрим модель, где свойство внешнего ключа `DepartmentID` *не* включено.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-622">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="fcdf8-623">При получении сущности курса для редактирования:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-623">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="fcdf8-624">сущность `Department` имеет значение null, если она не загружена явно;</span><span class="sxs-lookup"><span data-stu-id="fcdf8-624">The `Department` entity is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="fcdf8-625">для обновления сущности курса нужно сначала получить сущность `Department`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-625">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="fcdf8-626">Если свойство внешнего ключа `DepartmentID` включено в модель данных, получать сущность `Department` перед обновлением не нужно.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-626">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="fcdf8-627">Атрибут DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="fcdf8-627">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="fcdf8-628">Атрибут `[DatabaseGenerated(DatabaseGeneratedOption.None)]` указывает, что первичный ключ предоставляется приложением, а не создается базой данных.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-628">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="fcdf8-629">По умолчанию EF Core предполагает, что значения первичного ключа создаются базой данных.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-629">By default, EF Core assumes that PK values are generated by the DB.</span></span> <span data-ttu-id="fcdf8-630">Обычно лучше всего использовать значения первичного ключа, созданные базой данных.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-630">DB generated PK values is generally the best approach.</span></span> <span data-ttu-id="fcdf8-631">Для сущностей `Course` пользователь указывает первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-631">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="fcdf8-632">Например, номер курса, такой как серия 1000 для кафедры математики и серия 2000 для кафедры английского языка.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-632">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="fcdf8-633">Атрибут `DatabaseGenerated` также может использоваться для создания значений по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-633">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="fcdf8-634">Например, база данных может автоматически создать поле даты для записи даты, когда строка была создана или изменена.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-634">For example, the DB can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="fcdf8-635">Дополнительные сведения см. в разделе [Созданные свойства](/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-635">For more information, see [Generated Properties](/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="fcdf8-636">Свойства внешнего ключа и навигации</span><span class="sxs-lookup"><span data-stu-id="fcdf8-636">Foreign key and navigation properties</span></span>

<span data-ttu-id="fcdf8-637">Свойства внешнего ключа (FK) и свойства навигации в сущности `Course` отражают следующие связи:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-637">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="fcdf8-638">Курс назначается одной кафедре, поэтому имеется внешний ключ `DepartmentID` и свойство навигации `Department`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-638">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="fcdf8-639">На курс может быть зачислено любое количество учащихся, поэтому свойство навигации `Enrollments` является коллекцией:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-639">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="fcdf8-640">Курс могут вести несколько преподавателей, поэтому свойство навигации `CourseAssignments` является коллекцией:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-640">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="fcdf8-641">`CourseAssignment` описано [далее](#many-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-641">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="create-the-department-entity"></a><span data-ttu-id="fcdf8-642">Создание сущности Department</span><span class="sxs-lookup"><span data-stu-id="fcdf8-642">Create the Department entity</span></span>

![Сущность Department](complex-data-model/_static/department-entity.png)

<span data-ttu-id="fcdf8-644">Создайте файл *Models/Department.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-644">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="fcdf8-645">Атрибут Column</span><span class="sxs-lookup"><span data-stu-id="fcdf8-645">The Column attribute</span></span>

<span data-ttu-id="fcdf8-646">Ранее атрибут `Column` использовался, чтобы изменить сопоставление имени столбца.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-646">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="fcdf8-647">В коде для сущности `Department` атрибут `Column` можно использовать, чтобы изменить сопоставление типов данных SQL.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-647">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="fcdf8-648">Столбец `Budget` определяется с помощью типа money SQL Server в базе данных:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-648">The `Budget` column is defined using the SQL Server money type in the DB:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="fcdf8-649">Сопоставление столбцов обычно не требуется.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-649">Column mapping is generally not required.</span></span> <span data-ttu-id="fcdf8-650">В общем случае EF Core выбирает соответствующий тип данных SQL Server на основе типа среды CLR для свойства.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-650">EF Core generally chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="fcdf8-651">Тип `decimal` среды CLR сопоставляется с типом `decimal` SQL Server.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-651">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="fcdf8-652">`Budget` используется для денежных единиц, хотя для этого лучше подходит тип данных money.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-652">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="fcdf8-653">Свойства внешнего ключа и навигации</span><span class="sxs-lookup"><span data-stu-id="fcdf8-653">Foreign key and navigation properties</span></span>

<span data-ttu-id="fcdf8-654">Свойства внешнего ключа и навигации отражают следующие связи:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-654">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="fcdf8-655">Кафедра может иметь или не иметь администратора.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-655">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="fcdf8-656">Администратор всегда является преподавателем.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-656">An administrator is always an instructor.</span></span> <span data-ttu-id="fcdf8-657">Поэтому свойство `InstructorID` включается в качестве внешнего ключа в сущность `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-657">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="fcdf8-658">Свойство навигации называется `Administrator`, но содержит сущность `Instructor`:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-658">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="fcdf8-659">Вопросительный знак (?) в приведенном выше коде указывает, что свойство допускает значение null.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-659">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="fcdf8-660">Кафедра может иметь несколько курсов, поэтому доступно свойство навигации Courses:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-660">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="fcdf8-661">Примечание. По соглашению EF Core разрешает каскадное удаление для внешних ключей, не допускающих значение null, и связей "многие ко многим".</span><span class="sxs-lookup"><span data-stu-id="fcdf8-661">Note: By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="fcdf8-662">Каскадное удаление может привести к циклическим правилам каскадного удаления.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-662">Cascading delete can result in circular cascade delete rules.</span></span> <span data-ttu-id="fcdf8-663">Такие правила вызывают исключение при добавлении миграции.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-663">Circular cascade delete rules causes an exception when a migration is added.</span></span>

<span data-ttu-id="fcdf8-664">Например, если свойство `Department.InstructorID` было определено как не допускающее значения NULL:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-664">For example, if the `Department.InstructorID` property was defined as non-nullable:</span></span>

* <span data-ttu-id="fcdf8-665">EF Core настраивает правило каскадного удаления для удаления кафедры при удалении преподавателя.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-665">EF Core configures a cascade delete rule to delete the department when the instructor is deleted.</span></span>
* <span data-ttu-id="fcdf8-666">Удаление кафедры при удалении преподавателя не является запланированным поведением.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-666">Deleting the department when the instructor is deleted isn't the intended behavior.</span></span>
* <span data-ttu-id="fcdf8-667">Следующий [текучий API](#fluent-api-alternative-to-attributes) будет задавать правило ограничения вместо каскада.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-667">The following [fluent API](#fluent-api-alternative-to-attributes) would set a restrict rule instead of cascade.</span></span>

   ```csharp
   modelBuilder.Entity<Department>()
      .HasOne(d => d.Administrator)
      .WithMany()
      .OnDelete(DeleteBehavior.Restrict)
  ```

<span data-ttu-id="fcdf8-668">Предыдущий код отключает каскадное удаление для связи кафедры и преподавателя.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-668">The preceding code disables cascade delete on the department-instructor relationship.</span></span>

## <a name="update-the-enrollment-entity"></a><span data-ttu-id="fcdf8-669">Обновление сущности Enrollment</span><span class="sxs-lookup"><span data-stu-id="fcdf8-669">Update the Enrollment entity</span></span>

<span data-ttu-id="fcdf8-670">Запись зачисления обозначает один курс, который проходит один учащийся.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-670">An enrollment record is for one course taken by one student.</span></span>

![Сущность Enrollment](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="fcdf8-672">Обновите *Models/Enrollment.cs*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-672">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="fcdf8-673">Свойства внешнего ключа и навигации</span><span class="sxs-lookup"><span data-stu-id="fcdf8-673">Foreign key and navigation properties</span></span>

<span data-ttu-id="fcdf8-674">Свойства внешнего ключа и навигации отражают следующие связи:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-674">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="fcdf8-675">Запись зачисления предназначена для одного курса, поэтому доступно свойство первичного ключа `CourseID` и свойство навигации `Course`:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-675">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="fcdf8-676">Запись зачисления предназначена для одного учащегося, поэтому доступно свойство первичного ключа `StudentID` и свойство навигации `Student`:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-676">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="fcdf8-677">Связи многие ко многим</span><span class="sxs-lookup"><span data-stu-id="fcdf8-677">Many-to-Many Relationships</span></span>

<span data-ttu-id="fcdf8-678">Между сущностями `Student` и `Course` действует связь многие ко многим.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-678">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="fcdf8-679">Сущности `Enrollment` выступают в качестве таблицы соединения многие ко многим *с полезными данными* в базе данных.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-679">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="fcdf8-680">Фраза "с полезными данными" означает, что таблица `Enrollment` содержит дополнительные данные, кроме внешних ключей для присоединяемых таблиц (в данном случае — первичный ключ и `Grade`).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-680">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="fcdf8-681">На следующем рисунке показано, как выглядят эти связи на схеме сущностей.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-681">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="fcdf8-682">(Эта схема была создана с помощью [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) для EF 6.x.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-682">(This diagram was generated using [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) for EF 6.x.</span></span> <span data-ttu-id="fcdf8-683">Создание схемы не является частью руководства.)</span><span class="sxs-lookup"><span data-stu-id="fcdf8-683">Creating the diagram isn't part of the tutorial.)</span></span>

![Связь многие ко многим между Student и Course](complex-data-model/_static/student-course.png)

<span data-ttu-id="fcdf8-685">Каждая линия связи имеет 1 на одном конце и звездочку (\*) на другом, указывая характер один ко многим.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-685">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="fcdf8-686">Если таблица `Enrollment` не включала в себя сведения об оценках, ей потребуется содержать всего два внешних ключа (`CourseID` и `StudentID`).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-686">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="fcdf8-687">Таблицу соединения многие ко многим без полезных данных иногда называют чистой таблицей соединения (PJT).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-687">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="fcdf8-688">Сущности `Instructor` и `Course` имеют связь многие ко многим с использованием чистой таблицы соединения.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-688">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="fcdf8-689">Примечание. EF 6.x поддерживает неявные таблицы соединения для связей многие ко многим, но EF Core — нет.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-689">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="fcdf8-690">Дополнительные сведения см. в статье [Связи многие ко многим в EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-690">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="fcdf8-691">Сущность CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="fcdf8-691">The CourseAssignment entity</span></span>

![Сущность CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="fcdf8-693">Создайте файл *Models/CourseAssignment.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-693">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a><span data-ttu-id="fcdf8-694">Связь между Instructor и Courses</span><span class="sxs-lookup"><span data-stu-id="fcdf8-694">Instructor-to-Courses</span></span>

![Связь многие ко многим между Instructor и Courses](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="fcdf8-696">Связь многие ко многим между Instructor и Courses:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-696">The Instructor-to-Courses many-to-many relationship:</span></span>

* <span data-ttu-id="fcdf8-697">Нуждается в таблице соединения, которая должна быть представлена набором сущностей.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-697">Requires a join table that must be represented by an entity set.</span></span>
* <span data-ttu-id="fcdf8-698">Является чистой таблицей соединения (таблицей без полезных данных).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-698">Is a pure join table (table without payload).</span></span>

<span data-ttu-id="fcdf8-699">Обычно для сущности соединения используется имя `EntityName1EntityName2`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-699">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="fcdf8-700">Например, таблицей соединения Instructor и Courses, использующей этот шаблон, является `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-700">For example, the Instructor-to-Courses join table using this pattern is `CourseInstructor`.</span></span> <span data-ttu-id="fcdf8-701">Однако рекомендуется использовать имя, которое описывает эту связь.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-701">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="fcdf8-702">Модели данных создаются простыми и разрастаются.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-702">Data models start out simple and grow.</span></span> <span data-ttu-id="fcdf8-703">Соединения без полезных данных (PJT) часто начинают включать их.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-703">No-payload joins (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="fcdf8-704">Если в начале задать описательное имя сущности, его не нужно менять при изменениях таблицы соединения.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-704">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="fcdf8-705">Оптимально, если сущность соединения имеет собственное естественное имя (возможно, из одного слова) в бизнес-среде.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-705">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="fcdf8-706">Например, Books и Customers можно связать с сущностью соединения Ratings.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-706">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="fcdf8-707">Связь многие ко многим между Instructor и Courses `CourseAssignment` является более предпочтительным вариантом, чем `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-707">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="fcdf8-708">Составной ключ</span><span class="sxs-lookup"><span data-stu-id="fcdf8-708">Composite key</span></span>

<span data-ttu-id="fcdf8-709">Внешние ключи не допускают значение null.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-709">FKs are not nullable.</span></span> <span data-ttu-id="fcdf8-710">Два внешних ключа в `CourseAssignment` (`InstructorID` и `CourseID`) совместно однозначно определяют каждую строку таблицы `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-710">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="fcdf8-711">`CourseAssignment` не требуется выделенный первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-711">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="fcdf8-712">Свойства `InstructorID` и `CourseID` выступают в качестве составного первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-712">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="fcdf8-713">Единственным способом указать составные первичные ключи для EF Core является *текучий API*.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-713">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="fcdf8-714">Следующий раздел описывает, как настроить составной первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-714">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="fcdf8-715">Составной ключ обеспечивает следующее:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-715">The composite key ensures:</span></span>

* <span data-ttu-id="fcdf8-716">Для одного курса допускается несколько строк.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-716">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="fcdf8-717">Для одного преподавателя допускается несколько строк.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-717">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="fcdf8-718">Несколько строк для одного преподавателя и курса недопустимы.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-718">Multiple rows for the same instructor and course isn't allowed.</span></span>

<span data-ttu-id="fcdf8-719">Сущность соединения `Enrollment` определяет собственный первичный ключ, поэтому подобные дубликаты возможны.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-719">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="fcdf8-720">Для предотвращения подобных дубликатов:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-720">To prevent such duplicates:</span></span>

* <span data-ttu-id="fcdf8-721">добавьте уникальный индекс для полей внешнего ключа или</span><span class="sxs-lookup"><span data-stu-id="fcdf8-721">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="fcdf8-722">настройте `Enrollment` с первичным составным ключом аналогично `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-722">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="fcdf8-723">Дополнительные сведения см. в разделе [Индексы](/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-723">For more information, see [Indexes](/ef/core/modeling/indexes).</span></span>

## <a name="update-the-db-context"></a><span data-ttu-id="fcdf8-724">Изменение контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="fcdf8-724">Update the DB context</span></span>

<span data-ttu-id="fcdf8-725">Добавьте выделенный ниже код в файл *Data/SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-725">Add the following highlighted code to *Data/SchoolContext.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="fcdf8-726">Предыдущий код добавляет новые сущности и настраивает составной первичный ключ сущности `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-726">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="fcdf8-727">Текучий API вместо атрибутов</span><span class="sxs-lookup"><span data-stu-id="fcdf8-727">Fluent API alternative to attributes</span></span>

<span data-ttu-id="fcdf8-728">Метод `OnModelCreating` в предыдущем коде использует для настройки поведения EF Core *текучий API*.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-728">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="fcdf8-729">Этот API называется "текучим", так как часто используется для объединения серии вызовов методов в один оператор.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-729">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="fcdf8-730">В [следующем коде](/ef/core/modeling/#use-fluent-api-to-configure-a-model) показан пример текучего API:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-730">The [following code](/ef/core/modeling/#use-fluent-api-to-configure-a-model) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="fcdf8-731">В этом руководстве текучий API используется только для сопоставления базы данных, которое невозможно выполнить с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-731">In this tutorial, the fluent API is used only for DB mapping that can't be done with attributes.</span></span> <span data-ttu-id="fcdf8-732">Однако текучий API позволяет задать большинство правил форматирования, проверки и сопоставления, которые можно указать с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-732">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="fcdf8-733">Некоторые атрибуты, такие как `MinimumLength`, невозможно применить с текучим API.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-733">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="fcdf8-734">`MinimumLength` не изменяет схему, он лишь применяет правило проверки минимальной длины.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-734">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="fcdf8-735">Некоторые разработчики предпочитают использовать текучий API монопольно, чтобы оставить свои классы сущностей "чистыми".</span><span class="sxs-lookup"><span data-stu-id="fcdf8-735">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="fcdf8-736">Атрибуты и текучий API можно смешивать.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-736">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="fcdf8-737">Существует несколько конфигураций, которые можно реализовать только с помощью текучего API (с указанием составного первичного ключа).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-737">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="fcdf8-738">Существует несколько конфигураций, которые можно реализовать только с помощью атрибутов (`MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-738">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="fcdf8-739">Рекомендации по использованию текучего API и атрибутов:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-739">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="fcdf8-740">Выберите один из двух этих подходов.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-740">Choose one of these two approaches.</span></span>
* <span data-ttu-id="fcdf8-741">Используйте выбранный подход максимально согласованно.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-741">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="fcdf8-742">Некоторые атрибуты из этого руководства используются:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-742">Some of the attributes used in the this tutorial are used for:</span></span>

* <span data-ttu-id="fcdf8-743">только для проверки (например, `MinimumLength`);</span><span class="sxs-lookup"><span data-stu-id="fcdf8-743">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="fcdf8-744">только для конфигурации EF Core (например, `HasKey`);</span><span class="sxs-lookup"><span data-stu-id="fcdf8-744">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="fcdf8-745">для проверки и конфигурации EF Core (например, `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-745">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="fcdf8-746">Дополнительные сведения о сравнении атрибутов и текучего API см. в разделе [Методы конфигурации](/ef/core/modeling/).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-746">For more information about attributes vs. fluent API, see [Methods of configuration](/ef/core/modeling/).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="fcdf8-747">Схема сущностей, показывающая связи</span><span class="sxs-lookup"><span data-stu-id="fcdf8-747">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="fcdf8-748">Ниже показана схема, создаваемая средствами EF Power Tools для завершенной модели School.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-748">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Схема сущностей](complex-data-model/_static/diagram.png)

<span data-ttu-id="fcdf8-750">На предыдущей схеме показано следующее:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-750">The preceding diagram shows:</span></span>

* <span data-ttu-id="fcdf8-751">Несколько линий связи один ко многим (1 к \*).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-751">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="fcdf8-752">Линия связи один к нулю или одному (1 к 0..1) между сущностями `Instructor` и `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-752">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="fcdf8-753">Линия связи нуль или один ко многим (0..1 к \*) между сущностями `Instructor` и `Department`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-753">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-db-with-test-data"></a><span data-ttu-id="fcdf8-754">Заполнение базы данных тестовыми данными</span><span class="sxs-lookup"><span data-stu-id="fcdf8-754">Seed the DB with Test Data</span></span>

<span data-ttu-id="fcdf8-755">Обновите код в файле *Data/DbInitializer.cs*:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-755">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="fcdf8-756">Предыдущий код предоставляет начальные данные для новых сущностей.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-756">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="fcdf8-757">Основная часть кода создает объекты сущностей и загружает демонстрационные данные.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-757">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="fcdf8-758">Демонстрационные данные используются для проверки.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-758">The sample data is used for testing.</span></span> <span data-ttu-id="fcdf8-759">См. `Enrollments` и `CourseAssignments`, чтобы ознакомиться с примерами заполнения данными таблиц соединения "многие ко многим".</span><span class="sxs-lookup"><span data-stu-id="fcdf8-759">See `Enrollments` and `CourseAssignments` for examples of how many-to-many join tables can be seeded.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="fcdf8-760">Добавление миграции</span><span class="sxs-lookup"><span data-stu-id="fcdf8-760">Add a migration</span></span>

<span data-ttu-id="fcdf8-761">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-761">Build the project.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fcdf8-762">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fcdf8-762">Visual Studio</span></span>](#tab/visual-studio)

```powershell
Add-Migration ComplexDataModel
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fcdf8-763">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fcdf8-763">Visual Studio Code</span></span>](#tab/visual-studio-code)

```dotnetcli
dotnet ef migrations add ComplexDataModel
```

---

<span data-ttu-id="fcdf8-764">Предыдущая команда отображает предупреждение о возможной потере данных.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-764">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="fcdf8-765">При выполнении команды `database update` возникает следующая ошибка:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-765">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

## <a name="apply-the-migration"></a><span data-ttu-id="fcdf8-766">Применение миграции</span><span class="sxs-lookup"><span data-stu-id="fcdf8-766">Apply the migration</span></span>

<span data-ttu-id="fcdf8-767">Теперь у вас есть база данных, и пора подумать о том, как применять к ней будущие изменения.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-767">Now that you have an existing database, you need to think about how to apply future changes to it.</span></span> <span data-ttu-id="fcdf8-768">В этом руководстве показано два подхода:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-768">This tutorial shows two approaches:</span></span>

* [<span data-ttu-id="fcdf8-769">Удаление и повторное создание базы данных</span><span class="sxs-lookup"><span data-stu-id="fcdf8-769">Drop and re-create the database</span></span>](#drop)
* <span data-ttu-id="fcdf8-770">[Применение миграции к существующей базе данных](#applyexisting).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-770">[Apply the migration to the existing database](#applyexisting).</span></span> <span data-ttu-id="fcdf8-771">Хотя этот метод является более сложным и трудоемким, в реальной рабочей среде лучше использовать именно его.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-771">While this method is more complex and time-consuming, it's the preferred approach for real-world, production environments.</span></span> <span data-ttu-id="fcdf8-772">**Примечание**. Это необязательный раздел руководства.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-772">**Note**: This is an optional section of the tutorial.</span></span> <span data-ttu-id="fcdf8-773">Вы можете выполнить удаление и повторное создание и пропустить этот раздел.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-773">You can do the drop and re-create steps and skip this section.</span></span> <span data-ttu-id="fcdf8-774">Если вы хотите выполнить инструкции в этом разделе, не выполняйте удаление и повторное создание.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-774">If you do want to follow the steps in this section, don't do the drop and re-create steps.</span></span> 

<a name="drop"></a>

### <a name="drop-and-re-create-the-database"></a><span data-ttu-id="fcdf8-775">Удаление и повторное создание базы данных</span><span class="sxs-lookup"><span data-stu-id="fcdf8-775">Drop and re-create the database</span></span>

<span data-ttu-id="fcdf8-776">Код в обновленном `DbInitializer` предоставляет начальные данные для новых сущностей.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-776">The code in the updated `DbInitializer` adds seed data for the new entities.</span></span> <span data-ttu-id="fcdf8-777">Чтобы выполнить в EF Core принудительное создание новой базы данных, удаление и обновление базы данных:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-777">To force EF Core to create a new  DB, drop and update the DB:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fcdf8-778">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fcdf8-778">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fcdf8-779">Выполните следующие команды в **консоли диспетчера пакетов** (PMC):</span><span class="sxs-lookup"><span data-stu-id="fcdf8-779">In the **Package Manager Console** (PMC), run the following command:</span></span>

```PMC
Drop-Database
Update-Database
```

<span data-ttu-id="fcdf8-780">Чтобы просмотреть справочную информацию, выполните команду `Get-Help about_EntityFrameworkCore` в PMC.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-780">Run `Get-Help about_EntityFrameworkCore` from the PMC to get help information.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fcdf8-781">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fcdf8-781">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="fcdf8-782">Откройте командное окно и перейдите в папку проекта.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-782">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="fcdf8-783">Папка проекта содержит файл *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-783">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="fcdf8-784">Введите в командном окне следующее:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-784">Enter the following in the command window:</span></span>

```dotnetcli
dotnet ef database drop
dotnet ef database update
```

---

<span data-ttu-id="fcdf8-785">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-785">Run the app.</span></span> <span data-ttu-id="fcdf8-786">При запуске приложения выполняется метод `DbInitializer.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-786">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="fcdf8-787">`DbInitializer.Initialize` заполняет новую базу данных.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-787">The `DbInitializer.Initialize` populates the new DB.</span></span>

<span data-ttu-id="fcdf8-788">Откройте базу данных в SSOX:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-788">Open the DB in SSOX:</span></span>

* <span data-ttu-id="fcdf8-789">Если SSOX был открыт ранее, нажмите кнопку **Обновить**.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-789">If SSOX was opened previously, click the **Refresh** button.</span></span>
* <span data-ttu-id="fcdf8-790">Разверните узел **Таблицы**.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-790">Expand the **Tables** node.</span></span> <span data-ttu-id="fcdf8-791">Отображаются созданные таблицы.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-791">The created tables are displayed.</span></span>

![Таблицы в SSOX](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="fcdf8-793">Изучите таблицу **CourseAssignment**:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-793">Examine the **CourseAssignment** table:</span></span>

* <span data-ttu-id="fcdf8-794">Щелкните правой кнопкой мыши таблицу **CourseAssignment** и выберите пункт **Просмотреть данные**.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-794">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
* <span data-ttu-id="fcdf8-795">Убедитесь, что таблица **CourseAssignment** содержит данные.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-795">Verify the **CourseAssignment** table contains data.</span></span>

![Данные CourseAssignment в SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="applyexisting"></a>

### <a name="apply-the-migration-to-the-existing-database"></a><span data-ttu-id="fcdf8-797">Применение миграции к существующей базе данных</span><span class="sxs-lookup"><span data-stu-id="fcdf8-797">Apply the migration to the existing database</span></span>

<span data-ttu-id="fcdf8-798">Этот раздел является необязательным.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-798">This section is optional.</span></span> <span data-ttu-id="fcdf8-799">Эти действия подходят только в том случае, если вы пропустили предыдущий раздел [Удаление и повторное создание базы данных](#drop).</span><span class="sxs-lookup"><span data-stu-id="fcdf8-799">These steps work only if you skipped the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="fcdf8-800">При выполнении миграций с существующими данными могут действовать ограничения внешнего ключа, которым существующие данные не соответствуют.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-800">When migrations are run with existing data, there may be FK constraints that are not satisfied with the existing data.</span></span> <span data-ttu-id="fcdf8-801">Для рабочих данных нужно предпринять меры по переносу существующих данных.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-801">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="fcdf8-802">Этот раздел содержит пример того, как устранить нарушения ограничений внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-802">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="fcdf8-803">Вносите эти изменения кода только после создания резервной копии.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-803">Don't make these code changes without a backup.</span></span> <span data-ttu-id="fcdf8-804">Не вносите эти изменения кода, если вы выполнили инструкции из предыдущего раздела и обновили базу данных.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-804">Don't make these code changes if you completed the previous section and updated the database.</span></span>

<span data-ttu-id="fcdf8-805">Файл *{метка_времени}_ComplexDataModel.cs* содержит следующий код:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-805">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="fcdf8-806">Предыдущий код добавляет в таблицу `Course` внешний ключ `DepartmentID`, не допускающий значение null.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-806">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="fcdf8-807">База данных из предыдущего руководства содержит строки в `Course`, поэтому эту таблицу невозможно обновить с помощью миграций.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-807">The DB from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="fcdf8-808">Чтобы заставить миграцию `ComplexDataModel` работать с существующими данными:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-808">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="fcdf8-809">Изменить код, чтобы присвоить новому столбцу (`DepartmentID`) значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-809">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="fcdf8-810">Создайте фиктивную кафедру с именем "Temp" для использования по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-810">Create a fake department named "Temp" to act as the default department.</span></span>

#### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="fcdf8-811">Устранение ограничений внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="fcdf8-811">Fix the foreign key constraints</span></span>

<span data-ttu-id="fcdf8-812">Обновите метод `Up` в классах `ComplexDataModel`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-812">Update the `ComplexDataModel` classes `Up` method:</span></span>

* <span data-ttu-id="fcdf8-813">Откройте файл *{метка_времени}_ComplexDataModel.cs*.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-813">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="fcdf8-814">Закомментируйте строку кода, которая добавляет столбец `DepartmentID` в таблицу `Course`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-814">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="fcdf8-815">Добавьте выделенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-815">Add the following highlighted code.</span></span> <span data-ttu-id="fcdf8-816">Новый код идет после блока `.CreateTable( name: "Department"`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-816">The new code goes after the `.CreateTable( name: "Department"` block:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

<span data-ttu-id="fcdf8-817">После внесения описанных выше изменений существующие строки `Course` будут связаны с кафедрой "Temp" после выполнения метода `ComplexDataModel` `Up`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-817">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel` `Up` method runs.</span></span>

<span data-ttu-id="fcdf8-818">Рабочее приложение:</span><span class="sxs-lookup"><span data-stu-id="fcdf8-818">A production app would:</span></span>

* <span data-ttu-id="fcdf8-819">включает код или сценарии, чтобы добавить строки `Department` и связанные строки `Course` к новым строкам `Department`;</span><span class="sxs-lookup"><span data-stu-id="fcdf8-819">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="fcdf8-820">не использует кафедру "Temp" или значение по умолчанию для `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-820">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

<span data-ttu-id="fcdf8-821">Следующее руководство посвящено связанным данным.</span><span class="sxs-lookup"><span data-stu-id="fcdf8-821">The next tutorial covers related data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fcdf8-822">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="fcdf8-822">Additional resources</span></span>

* [<span data-ttu-id="fcdf8-823">Версия руководства на YouTube (часть 1)</span><span class="sxs-lookup"><span data-stu-id="fcdf8-823">YouTube version of this tutorial(Part 1)</span></span>](https://www.youtube.com/watch?v=0n2f0ObgCoA)
* [<span data-ttu-id="fcdf8-824">Версия руководства на YouTube (часть 2)</span><span class="sxs-lookup"><span data-stu-id="fcdf8-824">YouTube version of this tutorial(Part 2)</span></span>](https://www.youtube.com/watch?v=Je0Z5K1TNmY)

> [!div class="step-by-step"]
> <span data-ttu-id="fcdf8-825">[Назад](xref:data/ef-rp/migrations)
> [Вперед](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="fcdf8-825">[Previous](xref:data/ef-rp/migrations)
[Next](xref:data/ef-rp/read-related-data)</span></span>

::: moniker-end
