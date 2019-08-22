---
title: Razor Pages с EF Core в ASP.NET Core — модель данных— 5 из 8
author: tdykstra
description: В этом руководстве вы добавите дополнительные сущности и связи, а также настроите модель данных, указав правила форматирования, проверки и сопоставления.
ms.author: riande
ms.custom: mvc
ms.date: 07/22/2019
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: 8a1c0759453b02f4ce1c45471a8f93da626f8261
ms.sourcegitcommit: 257cc3fe8c1d61341aa3b07e5bc0fa3d1c1c1d1c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/19/2019
ms.locfileid: "69583282"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---data-model---5-of-8"></a><span data-ttu-id="e1ba1-103">Razor Pages с EF Core в ASP.NET Core — модель данных— 5 из 8</span><span class="sxs-lookup"><span data-stu-id="e1ba1-103">Razor Pages with EF Core in ASP.NET Core - Data Model - 5 of 8</span></span>

<span data-ttu-id="e1ba1-104">Авторы: [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="e1ba1-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e1ba1-105">Предыдущие руководства работали с базовой моделью данных, состоящей из трех сущностей.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-105">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="e1ba1-106">В этом руководстве:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-106">In this tutorial:</span></span>

* <span data-ttu-id="e1ba1-107">Добавляются дополнительные сущности и связи.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-107">More entities and relationships are added.</span></span>
* <span data-ttu-id="e1ba1-108">Настраивается модель данных с помощью указания правил для форматирования, проверки и сопоставления базы данных.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-108">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="e1ba1-109">Готовая модель данных показана на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-109">The completed data model is shown in the following illustration:</span></span>

![Схема сущностей](complex-data-model/_static/diagram.png)

## <a name="the-student-entity"></a><span data-ttu-id="e1ba1-111">Сущность Student</span><span class="sxs-lookup"><span data-stu-id="e1ba1-111">The Student entity</span></span>

![Сущность Student](complex-data-model/_static/student-entity.png)

<span data-ttu-id="e1ba1-113">Замените код в файле *Models/Student.cs* на следующий:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-113">Replace the code in *Models/Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Student.cs)]

<span data-ttu-id="e1ba1-114">В приведенном выше коде добавляется свойство `FullName` и добавляются следующие атрибуты к существующим свойствам:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-114">The preceding code adds a `FullName` property and adds the following attributes to existing properties:</span></span>

* `[DataType]`
* `[DisplayFormat]`
* `[StringLength]`
* `[Column]`
* `[Required]`
* `[Display]`

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="e1ba1-115">Вычисляемое свойство FullName</span><span class="sxs-lookup"><span data-stu-id="e1ba1-115">The FullName calculated property</span></span>

<span data-ttu-id="e1ba1-116">`FullName` — это вычисляемое свойство, которое возвращает значение, созданное путем объединения двух других свойств.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-116">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="e1ba1-117">`FullName` не может быть задано, поэтому имеет только метод доступа get.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-117">`FullName` can't be set, so it has only a get accessor.</span></span> <span data-ttu-id="e1ba1-118">В базе данных не создается столбец `FullName`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-118">No `FullName` column is created in the database.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="e1ba1-119">Атрибут DataType</span><span class="sxs-lookup"><span data-stu-id="e1ba1-119">The DataType attribute</span></span>

```csharp
[DataType(DataType.Date)]
```

<span data-ttu-id="e1ba1-120">Сейчас для дат зачисления учащихся на всех страницах отображаются время и дата, хотя важна только дата.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-120">For student enrollment dates, all of the pages currently display the time of day along with the date, although only the date is relevant.</span></span> <span data-ttu-id="e1ba1-121">Используя атрибуты заметок к данным, вы можете внести в код одно изменение, позволяющее исправить формат отображения на каждой странице, где отображаются эти данные.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-121">By using data annotation attributes, you can make one code change that will fix the display format in every page that shows the data.</span></span> 

<span data-ttu-id="e1ba1-122">Атрибут [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) указывает тип данных более точно по сравнению со встроенным типом базы данных.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-122">The [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="e1ba1-123">В этом случае следует отображать отобразить только дату, а не дату и время.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-123">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="e1ba1-124">В [перечислении DataType](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) представлено множество типов данных, таких как Date, Time, PhoneNumber, Currency, EmailAddress и других. Атрибут `DataType` также обеспечивает автоматическое предоставление функций для определенных типов в приложении.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-124">The [DataType Enumeration](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="e1ba1-125">Например:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-125">For example:</span></span>

* <span data-ttu-id="e1ba1-126">Ссылка `mailto:` для `DataType.EmailAddress` создается автоматически.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-126">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="e1ba1-127">Средство выбора даты предоставляется для `DataType.Date` в большинстве браузеров.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-127">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="e1ba1-128">Атрибут `DataType` создает атрибуты HTML 5 `data-`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-128">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes.</span></span> <span data-ttu-id="e1ba1-129">Атрибуты `DataType` не предназначены для проверки.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-129">The `DataType` attributes don't provide validation.</span></span>

### <a name="the-displayformat-attribute"></a><span data-ttu-id="e1ba1-130">Атрибут DisplayFormat</span><span class="sxs-lookup"><span data-stu-id="e1ba1-130">The DisplayFormat attribute</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="e1ba1-131">`DataType.Date` не задает формат отображаемой даты.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-131">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="e1ba1-132">По умолчанию поле даты отображается с использованием форматов, установленных в [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support) сервера.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-132">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="e1ba1-133">С помощью атрибута `DisplayFormat` можно явно указать формат даты:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-133">The `DisplayFormat` attribute is used to explicitly specify the date format.</span></span> <span data-ttu-id="e1ba1-134">Параметр `ApplyFormatInEditMode` указывает, что формат должен применяться к пользовательскому интерфейсу редактирования.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-134">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="e1ba1-135">Некоторым полям не следует использовать `ApplyFormatInEditMode`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-135">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="e1ba1-136">Например, обозначение денежной единицы в общем случае не должно отображаться в поле редактирования текста.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-136">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="e1ba1-137">Атрибут `DisplayFormat` можно использовать отдельно.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-137">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="e1ba1-138">Однако чаще всего `DataType` рекомендуется применять вместе с атрибутом `DisplayFormat`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-138">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="e1ba1-139">Атрибут `DataType` передает семантику данных (в отличие от способа их вывода на экран).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-139">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="e1ba1-140">Атрибут `DataType` дает следующие преимущества, недоступные в `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-140">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="e1ba1-141">Поддержка функций HTML5 в браузере.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-141">The browser can enable HTML5 features.</span></span> <span data-ttu-id="e1ba1-142">Например, отображение элемента управления календарем, соответствующего языковому стандарту обозначения денежной единицы, ссылок электронной почты, проверки входных данных на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-142">For example, show a calendar control, the locale-appropriate currency symbol, email links, and client-side input validation.</span></span>
* <span data-ttu-id="e1ba1-143">По умолчанию формат отображения данных в браузере определяется в соответствии с установленным языковым стандартом.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-143">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="e1ba1-144">Дополнительные сведения см. в документации по [вспомогательной функции тегов \<input>](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-144">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

### <a name="the-stringlength-attribute"></a><span data-ttu-id="e1ba1-145">Атрибут StringLength</span><span class="sxs-lookup"><span data-stu-id="e1ba1-145">The StringLength attribute</span></span>

```csharp
[StringLength(50, ErrorMessage = "First name cannot be longer than 50 characters.")]
```

<span data-ttu-id="e1ba1-146">С помощью атрибутов можно указать правила проверки данных и сообщения об ошибках проверки.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-146">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="e1ba1-147">Атрибут [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) указывает минимальную и максимальную длину символов, разрешенных в поле данных.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-147">The [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="e1ba1-148">Представленный код задает ограничение длины имен в 50 символов.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-148">The code shown limits names to no more than 50 characters.</span></span> <span data-ttu-id="e1ba1-149">Пример, в котором задается минимальная длина строки, приводится [далее](#the-required-attribute).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-149">An example that sets the minimum string length is shown [later](#the-required-attribute).</span></span>

<span data-ttu-id="e1ba1-150">Атрибут `StringLength` также обеспечивает проверку на стороне клиента и на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-150">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="e1ba1-151">Минимальное значение не оказывает влияния на схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-151">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="e1ba1-152">Атрибут `StringLength` не запрещает пользователю ввести пробел в качестве имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-152">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="e1ba1-153">Атрибут [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) можно использовать для применения ограничений к входным данным.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-153">The [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute can be used to apply restrictions to the input.</span></span> <span data-ttu-id="e1ba1-154">Например, следующий код требует, чтобы первый символ был прописной буквой, а остальные символы были буквенными:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-154">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1ba1-155">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1ba1-155">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e1ba1-156">В **обозревателе объектов SQL Server** (SSOX) откройте конструктор таблиц учащихся, дважды щелкнув таблицу **Student** (Учащийся).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-156">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Таблица учащихся в SSOX до миграций](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="e1ba1-158">На предыдущем изображении показана схемы для таблицы `Student`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-158">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="e1ba1-159">Поля имен имеют тип `nvarchar(MAX)`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-159">The name fields have type `nvarchar(MAX)`.</span></span> <span data-ttu-id="e1ba1-160">Когда далее в этом учебнике будет создана и применена миграция, поля имен станут `nvarchar(50)` из-за атрибутов длины строки.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-160">When a migration is created and applied later in this tutorial, the name fields become `nvarchar(50)` as a result of the string length attributes.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e1ba1-161">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-161">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e1ba1-162">В средстве SQLite изучите определения столбцов для таблицы `Student`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-162">In your SQLite tool, examine the column definitions for the `Student` table.</span></span> <span data-ttu-id="e1ba1-163">Поля имен имеют тип `Text`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-163">The name fields have type `Text`.</span></span> <span data-ttu-id="e1ba1-164">Обратите внимание, что первое поле имени называется `FirstMidName`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-164">Notice that the first name field is called `FirstMidName`.</span></span> <span data-ttu-id="e1ba1-165">В следующем разделе вы измените имя этого столбца на `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-165">In the next section, you change the name of that column to `FirstName`.</span></span>

---

### <a name="the-column-attribute"></a><span data-ttu-id="e1ba1-166">Атрибут Column</span><span class="sxs-lookup"><span data-stu-id="e1ba1-166">The Column attribute</span></span>

```csharp
[Column("FirstName")]
public string FirstMidName { get; set; }
```

<span data-ttu-id="e1ba1-167">Атрибуты могут управлять, как именно классы и свойства сопоставляются с базой данных.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-167">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="e1ba1-168">В модели `Student` атрибут `Column` используется для сопоставления имени свойства `FirstMidName` с "FirstName" в базе данных.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-168">In the `Student` model, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the database.</span></span>

<span data-ttu-id="e1ba1-169">При создании базы данных имена свойств в модели используются для имен столбцов (кроме случая, когда используется атрибут `Column`).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-169">When the database is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span> <span data-ttu-id="e1ba1-170">Модель `Student` использует `FirstMidName` для поля имени, так как это поле также может содержать отчество.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-170">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="e1ba1-171">С атрибутом `[Column]` поле `Student.FirstMidName` в модели данных сопоставляется со столбцом `FirstName` таблицы `Student`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-171">With the `[Column]` attribute, `Student.FirstMidName` in the data model maps to the `FirstName` column of the `Student` table.</span></span> <span data-ttu-id="e1ba1-172">Добавление атрибута `Column` изменяет модель для поддержки `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-172">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="e1ba1-173">Модель, поддерживающая `SchoolContext`, больше не соответствует базе данных.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-173">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="e1ba1-174">Это несоответствие будет устранено путем добавления миграции далее в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-174">That discrepancy will be resolved by adding a migration later in this tutorial.</span></span>

### <a name="the-required-attribute"></a><span data-ttu-id="e1ba1-175">Атрибут Required</span><span class="sxs-lookup"><span data-stu-id="e1ba1-175">The Required attribute</span></span>

```csharp
[Required]
```

<span data-ttu-id="e1ba1-176">Атрибут `Required` делает свойства имен обязательными полями.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-176">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="e1ba1-177">Атрибут `Required` не нужен для типов, не допускающих значения NULL, например для типов значений (таких как `DateTime`, `int` и `double`).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-177">The `Required` attribute isn't needed for non-nullable types such as value types (for example, `DateTime`, `int`, and `double`).</span></span> <span data-ttu-id="e1ba1-178">Типы, которые не могут принимать значение null, автоматически обрабатываются как обязательные поля.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-178">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="e1ba1-179">Атрибут `Required` можно заменить параметром минимальной длины в атрибуте `StringLength`:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-179">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="e1ba1-180">Атрибут Display</span><span class="sxs-lookup"><span data-stu-id="e1ba1-180">The Display attribute</span></span>

```csharp
[Display(Name = "Last Name")]
```

<span data-ttu-id="e1ba1-181">Атрибут `Display` указывает, что заголовки для текстовых полей должны иметь вид "First Name" (Имя), "Last Name" (Фамилия), "Full Name" (Полное имя) и "Enrollment Date" (Дата зачисления).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-181">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="e1ba1-182">По умолчанию в заголовках не использовался пробел для разделения слов, например "Lastname".</span><span class="sxs-lookup"><span data-stu-id="e1ba1-182">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="create-a-migration"></a><span data-ttu-id="e1ba1-183">Создание миграции</span><span class="sxs-lookup"><span data-stu-id="e1ba1-183">Create a migration</span></span>

<span data-ttu-id="e1ba1-184">Запустите приложение и перейдите на страницу Students.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-184">Run the app and go to the Students page.</span></span> <span data-ttu-id="e1ba1-185">Возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-185">An exception is thrown.</span></span> <span data-ttu-id="e1ba1-186">Атрибут `[Column]` приводит к тому, что EF ожидает столбец с именем `FirstName`, но имя столбца в базе данных по-прежнему `FirstMidName`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-186">The `[Column]` attribute causes EF to expect to find a column named `FirstName`, but the column name in the database is still `FirstMidName`.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1ba1-187">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1ba1-187">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e1ba1-188">Сообщение об ошибке подобно приведенному ниже.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-188">The error message is similar to the following example:</span></span>

```
SqlException: Invalid column name 'FirstName'.
```

* <span data-ttu-id="e1ba1-189">В PMC введите следующие команды для создания миграции и обновления базы данных:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-189">In the PMC, enter the following commands to create a new migration and update the database:</span></span>

  ```powershell
  Add-Migration ColumnFirstName
  Update-Database
  ```

  <span data-ttu-id="e1ba1-190">Первая из этих команд выдает следующее предупреждающее сообщение:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-190">The first of these commands generates the following warning message:</span></span>

  ```text
  An operation was scaffolded that may result in the loss of data.
  Please review the migration for accuracy.
  ```

  <span data-ttu-id="e1ba1-191">Это предупреждение вызвано тем, что поля имен теперь ограничены 50 символами.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-191">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="e1ba1-192">Если имя в базе данных имеет больше 50 символов, символы с 51-го до последнего будут потеряны.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-192">If a name in the database had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="e1ba1-193">Откройте таблицу "Student" (Учащийся) в окне SSOX:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-193">Open the Student table in SSOX:</span></span>

  ![Таблица учащихся в SSOX после миграций](complex-data-model/_static/ssox-after-migration.png)

  <span data-ttu-id="e1ba1-195">До применения миграции столбцы имен имели тип [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-195">Before the migration was applied, the name columns were of type [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="e1ba1-196">Теперь столбцы имен имеют тип `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-196">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="e1ba1-197">Имя столбца изменилось с `FirstMidName` на `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-197">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e1ba1-198">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-198">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e1ba1-199">Сообщение об ошибке подобно приведенному ниже.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-199">The error message is similar to the following example:</span></span>

```
SqliteException: SQLite Error 1: 'no such column: s.FirstName'.
```

* <span data-ttu-id="e1ba1-200">Откройте командное окно в папке проекта.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-200">Open a command window in the project folder.</span></span> <span data-ttu-id="e1ba1-201">Введите следующие команды для создания миграции и обновления базы данных:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-201">Enter the following commands to create a new migration and update the database:</span></span>

  ```console
  dotnet ef migrations add ColumnFirstName
  dotnet ef database update
  ```

  <span data-ttu-id="e1ba1-202">Команда обновления базы данных выводит ошибку наподобие приведенной ниже.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-202">The database update command displays an error like the following example:</span></span>

  ```text
  SQLite does not support this migration operation ('AlterColumnOperation'). For more information, see http://go.microsoft.com/fwlink/?LinkId=723262.
  ```

<span data-ttu-id="e1ba1-203">В этом учебнике устранить ошибку можно, удалив и заново создав исходную миграцию.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-203">For this tutorial, the way to get past this error is to delete and re-create the initial migration.</span></span> <span data-ttu-id="e1ba1-204">Дополнительные сведения см. в предупреждении, касающемся SQLite, в начале [учебника по миграции](xref:data/ef-rp/migrations).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-204">For more information, see the SQLite warning note at the top of the [migrations tutorial](xref:data/ef-rp/migrations).</span></span>

* <span data-ttu-id="e1ba1-205">Удалите папку *Migrations*.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-205">Delete the *Migrations* folder.</span></span>
* <span data-ttu-id="e1ba1-206">Выполните следующие команды, чтобы удалить базу данных, создать новую исходную миграцию и применить ее:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-206">Run the following commands to drop the database, create a new initial migration, and apply the migration:</span></span>

  ```console
  dotnet ef database drop --force
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

* <span data-ttu-id="e1ba1-207">Изучите таблицу Student в средстве SQLite.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-207">Examine the Student table in your SQLite tool.</span></span> <span data-ttu-id="e1ba1-208">Столбец, ранее называвшийся FirstMidName, теперь имеет имя FirstName.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-208">The column that was FirstMidName is now FirstName.</span></span>

---

* <span data-ttu-id="e1ba1-209">Запустите приложение и перейдите на страницу Students.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-209">Run the app and go to the Students page.</span></span>
* <span data-ttu-id="e1ba1-210">Обратите внимание, что значения времени не вводятся и не отображаются вместе с датами.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-210">Notice that times are not input or displayed along with dates.</span></span>
* <span data-ttu-id="e1ba1-211">Выберите **Create New** (Создать) и попробуйте ввести имя длиной более 50 символов.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-211">Select **Create New**, and try to enter a name longer than 50 characters.</span></span>

> [!Note]
> <span data-ttu-id="e1ba1-212">В следующих разделах сборка приложения на некоторых этапах приводит к возникновению ошибок компилятора.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-212">In the following sections, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="e1ba1-213">В инструкциях указано, когда производить сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-213">The instructions specify when to build the app.</span></span>

## <a name="the-instructor-entity"></a><span data-ttu-id="e1ba1-214">Сущность Instructor</span><span class="sxs-lookup"><span data-stu-id="e1ba1-214">The Instructor Entity</span></span>

![Сущность Instructor](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="e1ba1-216">Создайте файл *Models/Instructor.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-216">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Instructor.cs)]

<span data-ttu-id="e1ba1-217">На одной строке могут находиться несколько атрибутов.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-217">Multiple attributes can be on one line.</span></span> <span data-ttu-id="e1ba1-218">Атрибуты `HireDate` можно записать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-218">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="navigation-properties"></a><span data-ttu-id="e1ba1-219">Свойства навигации</span><span class="sxs-lookup"><span data-stu-id="e1ba1-219">Navigation properties</span></span>

<span data-ttu-id="e1ba1-220">`CourseAssignments` и `OfficeAssignment` — это свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-220">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="e1ba1-221">Преподаватель может проводить любое количество курсов, поэтому `CourseAssignments` определен как коллекция.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-221">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="e1ba1-222">Преподаватель может иметь не более одного кабинета, поэтому свойство `OfficeAssignment` содержит одну сущность `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-222">An instructor can have at most one office, so the `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="e1ba1-223">`OfficeAssignment` имеет значение null, если кабинет не назначен.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-223">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="the-officeassignment-entity"></a><span data-ttu-id="e1ba1-224">Сущность OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="e1ba1-224">The OfficeAssignment entity</span></span>

![Сущность OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="e1ba1-226">Создайте файл *Models/OfficeAssignment.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-226">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="e1ba1-227">Атрибут Key</span><span class="sxs-lookup"><span data-stu-id="e1ba1-227">The Key attribute</span></span>

<span data-ttu-id="e1ba1-228">Атрибут `[Key]` используется для идентификации свойства в качестве первичного ключа (PK), когда имя свойства отличается от classnameID или ID.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-228">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="e1ba1-229">Между сущностями `Instructor` и `OfficeAssignment` действует связь один к нулю или к одному.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-229">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="e1ba1-230">Назначение кабинета существует только в связи с преподавателем, которому оно назначено.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-230">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="e1ba1-231">Первичный ключ `OfficeAssignment` также является внешним ключом (FK) для сущности `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-231">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span>

<span data-ttu-id="e1ba1-232">EF Core не распознает `InstructorID` в качестве первичного ключа `OfficeAssignment` автоматически, так как `InstructorID` не соответствует соглашению об именовании ID или classnameID.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-232">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because `InstructorID` doesn't follow the ID or classnameID naming convention.</span></span> <span data-ttu-id="e1ba1-233">Таким образом, атрибут `Key` используется для определения `InstructorID` в качестве первичного ключа:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-233">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="e1ba1-234">По умолчанию EF Core считает ключ созданным не базой данных, так как столбец предназначен для идентифицирующего отношения.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-234">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="e1ba1-235">Свойство навигации Instructor</span><span class="sxs-lookup"><span data-stu-id="e1ba1-235">The Instructor navigation property</span></span>

<span data-ttu-id="e1ba1-236">Свойство навигации `Instructor.OfficeAssignment` может иметь значение NULL, так как строка `OfficeAssignment` для преподавателя может отсутствовать.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-236">The `Instructor.OfficeAssignment` navigation property can be null because there might not be an `OfficeAssignment` row for a given instructor.</span></span> <span data-ttu-id="e1ba1-237">Преподавателю может быть не назначен кабинет.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-237">An instructor might not have an office assignment.</span></span>

<span data-ttu-id="e1ba1-238">Свойство навигации `OfficeAssignment.Instructor` всегда будет иметь сущность Instructor, так как тип внешнего ключа `InstructorID` — это тип значения `int`, не допускающий значения NULL.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-238">The `OfficeAssignment.Instructor` navigation property will always have an instructor entity because the foreign key `InstructorID` type is `int`, a non-nullable value type.</span></span> <span data-ttu-id="e1ba1-239">Назначение кабинета не может существовать без преподавателя.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-239">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="e1ba1-240">Когда сущность `Instructor` имеет связанную сущность `OfficeAssignment`, каждая из них имеет ссылку на другую в своем свойстве навигации.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-240">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

## <a name="the-course-entity"></a><span data-ttu-id="e1ba1-241">Сущность Course</span><span class="sxs-lookup"><span data-stu-id="e1ba1-241">The Course Entity</span></span>

![Сущность Course](complex-data-model/_static/course-entity.png)

<span data-ttu-id="e1ba1-243">Обновите *Models/Course.cs*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-243">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Course.cs?highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="e1ba1-244">Сущность `Course` имеет свойство внешнего ключа (FK) `DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-244">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="e1ba1-245">`DepartmentID` указывает на связанную сущность `Department`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-245">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="e1ba1-246">Сущность `Course` имеет свойство навигации `Department`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-246">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="e1ba1-247">EF Core не требует свойства внешнего ключа для модели данных, если она имеет свойство навигации для связанной сущности.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-247">EF Core doesn't require a foreign key property for a data model when the model has a navigation property for a related entity.</span></span> <span data-ttu-id="e1ba1-248">EF Core автоматически создает внешние ключи в базе данных по мере необходимости.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-248">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="e1ba1-249">EF Core создает [теневые свойства](/ef/core/modeling/shadow-properties) для автоматически созданных внешних ключей.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-249">EF Core creates [shadow properties](/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="e1ba1-250">Однако явное включение внешнего ключа в модель данных позволяет сделать обновления проще и эффективнее.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-250">However, explicitly including the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="e1ba1-251">Например, рассмотрим модель, где свойство внешнего ключа `DepartmentID` *не* включено.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-251">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="e1ba1-252">При получении сущности курса для редактирования:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-252">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="e1ba1-253">свойство `Department` имеет значение NULL, если оно не загружено явно;</span><span class="sxs-lookup"><span data-stu-id="e1ba1-253">The `Department` property is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="e1ba1-254">для обновления сущности курса нужно сначала получить сущность `Department`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-254">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="e1ba1-255">Если свойство внешнего ключа `DepartmentID` включено в модель данных, получать сущность `Department` перед обновлением не нужно.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-255">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="e1ba1-256">Атрибут DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="e1ba1-256">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="e1ba1-257">Атрибут `[DatabaseGenerated(DatabaseGeneratedOption.None)]` указывает, что первичный ключ предоставляется приложением, а не создается базой данных.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-257">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="e1ba1-258">По умолчанию EF Core предполагает, что значения первичного ключа создаются базой данных.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-258">By default, EF Core assumes that PK values are generated by the database.</span></span> <span data-ttu-id="e1ba1-259">Обычно лучше всего использовать значения, созданные базой данных.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-259">Database-generated is generally the best approach.</span></span> <span data-ttu-id="e1ba1-260">Для сущностей `Course` пользователь указывает первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-260">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="e1ba1-261">Например, номер курса, такой как серия 1000 для кафедры математики и серия 2000 для кафедры английского языка.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-261">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="e1ba1-262">Атрибут `DatabaseGenerated` также может использоваться для создания значений по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-262">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="e1ba1-263">Например, база данных может автоматически создать поле даты для записи даты, когда строка была создана или изменена.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-263">For example, the database can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="e1ba1-264">Дополнительные сведения см. в разделе [Созданные свойства](/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-264">For more information, see [Generated Properties](/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="e1ba1-265">Свойства внешнего ключа и навигации</span><span class="sxs-lookup"><span data-stu-id="e1ba1-265">Foreign key and navigation properties</span></span>

<span data-ttu-id="e1ba1-266">Свойства внешнего ключа (FK) и свойства навигации в сущности `Course` отражают следующие связи:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-266">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="e1ba1-267">Курс назначается одной кафедре, поэтому имеется внешний ключ `DepartmentID` и свойство навигации `Department`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-267">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="e1ba1-268">На курс может быть зачислено любое количество учащихся, поэтому свойство навигации `Enrollments` является коллекцией:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-268">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="e1ba1-269">Курс могут вести несколько преподавателей, поэтому свойство навигации `CourseAssignments` является коллекцией:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-269">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="e1ba1-270">`CourseAssignment` описано [далее](#many-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-270">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="the-department-entity"></a><span data-ttu-id="e1ba1-271">Сущность Department</span><span class="sxs-lookup"><span data-stu-id="e1ba1-271">The Department entity</span></span>

![Сущность Department](complex-data-model/_static/department-entity.png)

<span data-ttu-id="e1ba1-273">Создайте файл *Models/Department.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-273">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Models/Department1.cs)]

### <a name="the-column-attribute"></a><span data-ttu-id="e1ba1-274">Атрибут Column</span><span class="sxs-lookup"><span data-stu-id="e1ba1-274">The Column attribute</span></span>

<span data-ttu-id="e1ba1-275">Ранее атрибут `Column` использовался, чтобы изменить сопоставление имени столбца.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-275">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="e1ba1-276">В коде для сущности `Department` атрибут `Column` можно использовать, чтобы изменить сопоставление типов данных SQL.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-276">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="e1ba1-277">Столбец `Budget` определяется с помощью типа money SQL Server в базе данных:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-277">The `Budget` column is defined using the SQL Server money type in the database:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="e1ba1-278">Сопоставление столбцов обычно не требуется.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-278">Column mapping is generally not required.</span></span> <span data-ttu-id="e1ba1-279">EF Core выбирает соответствующий тип данных SQL Server на основе типа среды CLR для свойства.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-279">EF Core chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="e1ba1-280">Тип `decimal` среды CLR сопоставляется с типом `decimal` SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-280">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="e1ba1-281">`Budget` используется для денежных единиц, хотя для этого лучше подходит тип данных money.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-281">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="e1ba1-282">Свойства внешнего ключа и навигации</span><span class="sxs-lookup"><span data-stu-id="e1ba1-282">Foreign key and navigation properties</span></span>

<span data-ttu-id="e1ba1-283">Свойства внешнего ключа и навигации отражают следующие связи:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-283">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="e1ba1-284">Кафедра может иметь или не иметь администратора.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-284">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="e1ba1-285">Администратор всегда является преподавателем.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-285">An administrator is always an instructor.</span></span> <span data-ttu-id="e1ba1-286">Поэтому свойство `InstructorID` включается в качестве внешнего ключа в сущность `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-286">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="e1ba1-287">Свойство навигации называется `Administrator`, но содержит сущность `Instructor`:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-287">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="e1ba1-288">Вопросительный знак (?) в приведенном выше коде указывает, что свойство допускает значение null.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-288">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="e1ba1-289">Кафедра может иметь несколько курсов, поэтому доступно свойство навигации Courses:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-289">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="e1ba1-290">По соглашению EF Core разрешает каскадное удаление для внешних ключей, не допускающих значение null, и связей "многие ко многим".</span><span class="sxs-lookup"><span data-stu-id="e1ba1-290">By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="e1ba1-291">Это поведение по умолчанию может привести к циклическим правилам каскадного удаления.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-291">This default behavior can result in circular cascade delete rules.</span></span> <span data-ttu-id="e1ba1-292">Такие правила вызывают исключение при добавлении миграции.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-292">Circular cascade delete rules cause an exception when a migration is added.</span></span>

<span data-ttu-id="e1ba1-293">Например, если свойство `Department.InstructorID` было определено как не допускающее значения NULL, EF Core настроит правило каскадного удаления.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-293">For example, if the `Department.InstructorID` property was defined as non-nullable, EF Core would configure a cascade delete rule.</span></span> <span data-ttu-id="e1ba1-294">В этом случае кафедра будет удалена, если будет удален преподаватель, назначенный ее администратором.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-294">In that case, the department would be deleted when the instructor assigned as its administrator is deleted.</span></span> <span data-ttu-id="e1ba1-295">В такой ситуации правило ограничения будет более целесообразным.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-295">In this scenario, a restrict rule would make more sense.</span></span> <span data-ttu-id="e1ba1-296">Приведенный ниже текучий API задает правило ограничения и отключает правило каскадного удаления.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-296">The following fluent API would set a restrict rule and disable cascade delete.</span></span>

  ```csharp
  modelBuilder.Entity<Department>()
     .HasOne(d => d.Administrator)
     .WithMany()
     .OnDelete(DeleteBehavior.Restrict)
  ```

## <a name="the-enrollment-entity"></a><span data-ttu-id="e1ba1-297">Сущность Enrollment</span><span class="sxs-lookup"><span data-stu-id="e1ba1-297">The Enrollment entity</span></span>

<span data-ttu-id="e1ba1-298">Запись зачисления обозначает один курс, который проходит один учащийся.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-298">An enrollment record is for one course taken by one student.</span></span>

![Сущность Enrollment](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="e1ba1-300">Обновите *Models/Enrollment.cs*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-300">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Enrollment.cs?highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="e1ba1-301">Свойства внешнего ключа и навигации</span><span class="sxs-lookup"><span data-stu-id="e1ba1-301">Foreign key and navigation properties</span></span>

<span data-ttu-id="e1ba1-302">Свойства внешнего ключа и навигации отражают следующие связи:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-302">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="e1ba1-303">Запись зачисления предназначена для одного курса, поэтому доступно свойство первичного ключа `CourseID` и свойство навигации `Course`:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-303">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="e1ba1-304">Запись зачисления предназначена для одного учащегося, поэтому доступно свойство первичного ключа `StudentID` и свойство навигации `Student`:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-304">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="e1ba1-305">Связи многие ко многим</span><span class="sxs-lookup"><span data-stu-id="e1ba1-305">Many-to-Many Relationships</span></span>

<span data-ttu-id="e1ba1-306">Между сущностями `Student` и `Course` действует связь многие ко многим.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-306">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="e1ba1-307">Сущности `Enrollment` выступают в качестве таблицы соединения многие ко многим *с полезными данными* в базе данных.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-307">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="e1ba1-308">Фраза "с полезными данными" означает, что таблица `Enrollment` содержит дополнительные данные, кроме внешних ключей для присоединяемых таблиц (в данном случае — первичный ключ и `Grade`).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-308">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="e1ba1-309">На следующем рисунке показано, как выглядят эти связи на схеме сущностей.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-309">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="e1ba1-310">(Эта схема была создана с помощью [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) для EF 6.x.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-310">(This diagram was generated using [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) for EF 6.x.</span></span> <span data-ttu-id="e1ba1-311">Создание схемы не является частью руководства.)</span><span class="sxs-lookup"><span data-stu-id="e1ba1-311">Creating the diagram isn't part of the tutorial.)</span></span>

![Связь многие ко многим между Student и Course](complex-data-model/_static/student-course.png)

<span data-ttu-id="e1ba1-313">Каждая линия связи имеет 1 на одном конце и звездочку (\*) на другом, указывая характер один ко многим.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-313">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="e1ba1-314">Если таблица `Enrollment` не включала в себя сведения об оценках, ей потребуется содержать всего два внешних ключа (`CourseID` и `StudentID`).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-314">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="e1ba1-315">Таблицу соединения многие ко многим без полезных данных иногда называют чистой таблицей соединения (PJT).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-315">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="e1ba1-316">Сущности `Instructor` и `Course` имеют связь многие ко многим с использованием чистой таблицы соединения.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-316">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="e1ba1-317">Примечание. EF 6.x поддерживает неявные таблицы соединения для связей многие ко многим, но EF Core — нет.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-317">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="e1ba1-318">Дополнительные сведения см. в статье [Связи многие ко многим в EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-318">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="e1ba1-319">Сущность CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="e1ba1-319">The CourseAssignment entity</span></span>

![Сущность CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="e1ba1-321">Создайте файл *Models/CourseAssignment.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-321">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/CourseAssignment.cs)]

<span data-ttu-id="e1ba1-322">Для связи "многие ко многим" между Instructor и Courses нужна таблица соединения, сущностью для которой является CourseAssignment.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-322">The Instructor-to-Courses many-to-many relationship requires a join table, and the entity for that join table is CourseAssignment.</span></span>

![Связь многие ко многим между Instructor и Courses](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="e1ba1-324">Обычно для сущности соединения используется имя `EntityName1EntityName2`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-324">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="e1ba1-325">Например, таблицей соединения Instructor и Courses, использующей этот шаблон, будет `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-325">For example, the Instructor-to-Courses join table using this pattern would be `CourseInstructor`.</span></span> <span data-ttu-id="e1ba1-326">Однако рекомендуется использовать имя, которое описывает эту связь.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-326">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="e1ba1-327">Модели данных создаются простыми и разрастаются.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-327">Data models start out simple and grow.</span></span> <span data-ttu-id="e1ba1-328">Таблицы соединения без полезных данных (PJT) часто начинают включать их.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-328">Join tables without payload (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="e1ba1-329">Если в начале задать описательное имя сущности, его не нужно менять при изменениях таблицы соединения.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-329">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="e1ba1-330">Оптимально, если сущность соединения имеет собственное естественное имя (возможно, из одного слова) в бизнес-среде.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-330">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="e1ba1-331">Например, Books и Customers можно связать с сущностью соединения Ratings.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-331">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="e1ba1-332">Связь многие ко многим между Instructor и Courses `CourseAssignment` является более предпочтительным вариантом, чем `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-332">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="e1ba1-333">Составной ключ</span><span class="sxs-lookup"><span data-stu-id="e1ba1-333">Composite key</span></span>

<span data-ttu-id="e1ba1-334">Два внешних ключа в `CourseAssignment` (`InstructorID` и `CourseID`) совместно однозначно определяют каждую строку таблицы `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-334">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="e1ba1-335">`CourseAssignment` не требуется выделенный первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-335">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="e1ba1-336">Свойства `InstructorID` и `CourseID` выступают в качестве составного первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-336">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="e1ba1-337">Единственным способом указать составные первичные ключи для EF Core является *текучий API*.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-337">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="e1ba1-338">Следующий раздел описывает, как настроить составной первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-338">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="e1ba1-339">Составной ключ обеспечивает следующее:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-339">The composite key ensures that:</span></span>

* <span data-ttu-id="e1ba1-340">Для одного курса допускается несколько строк.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-340">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="e1ba1-341">Для одного преподавателя допускается несколько строк.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-341">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="e1ba1-342">Несколько строк для одного преподавателя и курса недопустимы.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-342">Multiple rows aren't allowed for the same instructor and course.</span></span>

<span data-ttu-id="e1ba1-343">Сущность соединения `Enrollment` определяет собственный первичный ключ, поэтому подобные дубликаты возможны.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-343">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="e1ba1-344">Для предотвращения подобных дубликатов:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-344">To prevent such duplicates:</span></span>

* <span data-ttu-id="e1ba1-345">добавьте уникальный индекс для полей внешнего ключа или</span><span class="sxs-lookup"><span data-stu-id="e1ba1-345">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="e1ba1-346">настройте `Enrollment` с первичным составным ключом аналогично `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-346">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="e1ba1-347">Дополнительные сведения см. в разделе [Индексы](/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-347">For more information, see [Indexes](/ef/core/modeling/indexes).</span></span>

## <a name="update-the-database-context"></a><span data-ttu-id="e1ba1-348">Обновление контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="e1ba1-348">Update the database context</span></span>

<span data-ttu-id="e1ba1-349">Обновите файл *Data/SchoolContext.cs*, добавив в него следующий код:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-349">Update *Data/SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Data/SchoolContext.cs?highlight=15-18,25-31)]

<span data-ttu-id="e1ba1-350">Предыдущий код добавляет новые сущности и настраивает составной первичный ключ сущности `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-350">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="e1ba1-351">Текучий API вместо атрибутов</span><span class="sxs-lookup"><span data-stu-id="e1ba1-351">Fluent API alternative to attributes</span></span>

<span data-ttu-id="e1ba1-352">Метод `OnModelCreating` в предыдущем коде использует для настройки поведения EF Core *текучий API*.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-352">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="e1ba1-353">Этот API называется "текучим", так как часто используется для объединения серии вызовов методов в один оператор.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-353">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="e1ba1-354">В [следующем коде](/ef/core/modeling/#use-fluent-api-to-configure-a-model) показан пример текучего API:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-354">The [following code](/ef/core/modeling/#use-fluent-api-to-configure-a-model) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="e1ba1-355">В этом учебнике текучий API используется только для сопоставления базы данных, которое невозможно выполнить с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-355">In this tutorial, the fluent API is used only for database mapping that can't be done with attributes.</span></span> <span data-ttu-id="e1ba1-356">Однако текучий API позволяет задать большинство правил форматирования, проверки и сопоставления, которые можно указать с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-356">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="e1ba1-357">Некоторые атрибуты, такие как `MinimumLength`, невозможно применить с текучим API.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-357">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="e1ba1-358">`MinimumLength` не изменяет схему, он лишь применяет правило проверки минимальной длины.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-358">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="e1ba1-359">Некоторые разработчики предпочитают использовать текучий API монопольно, чтобы оставить свои классы сущностей "чистыми".</span><span class="sxs-lookup"><span data-stu-id="e1ba1-359">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="e1ba1-360">Атрибуты и текучий API можно смешивать.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-360">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="e1ba1-361">Существует несколько конфигураций, которые можно реализовать только с помощью текучего API (с указанием составного первичного ключа).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-361">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="e1ba1-362">Существует несколько конфигураций, которые можно реализовать только с помощью атрибутов (`MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-362">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="e1ba1-363">Рекомендации по использованию текучего API и атрибутов:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-363">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="e1ba1-364">Выберите один из двух этих подходов.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-364">Choose one of these two approaches.</span></span>
* <span data-ttu-id="e1ba1-365">Используйте выбранный подход максимально согласованно.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-365">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="e1ba1-366">Некоторые атрибуты из этого руководства используются:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-366">Some of the attributes used in this tutorial are used for:</span></span>

* <span data-ttu-id="e1ba1-367">только для проверки (например, `MinimumLength`);</span><span class="sxs-lookup"><span data-stu-id="e1ba1-367">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="e1ba1-368">только для конфигурации EF Core (например, `HasKey`);</span><span class="sxs-lookup"><span data-stu-id="e1ba1-368">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="e1ba1-369">для проверки и конфигурации EF Core (например, `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-369">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="e1ba1-370">Дополнительные сведения о сравнении атрибутов и текучего API см. в разделе [Методы конфигурации](/ef/core/modeling/).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-370">For more information about attributes vs. fluent API, see [Methods of configuration](/ef/core/modeling/).</span></span>

## <a name="entity-diagram"></a><span data-ttu-id="e1ba1-371">Схема сущностей</span><span class="sxs-lookup"><span data-stu-id="e1ba1-371">Entity diagram</span></span>

<span data-ttu-id="e1ba1-372">Ниже показана схема, создаваемая средствами EF Power Tools для завершенной модели School.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-372">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Схема сущностей](complex-data-model/_static/diagram.png)

<span data-ttu-id="e1ba1-374">На предыдущей схеме показано следующее:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-374">The preceding diagram shows:</span></span>

* <span data-ttu-id="e1ba1-375">Несколько линий связи один ко многим (1 к \*).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-375">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="e1ba1-376">Линия связи один к нулю или одному (1 к 0..1) между сущностями `Instructor` и `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-376">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="e1ba1-377">Линия связи нуль или один ко многим (0..1 к \*) между сущностями `Instructor` и `Department`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-377">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-database"></a><span data-ttu-id="e1ba1-378">Заполнение базы данных</span><span class="sxs-lookup"><span data-stu-id="e1ba1-378">Seed the database</span></span>

<span data-ttu-id="e1ba1-379">Обновите код в файле *Data/DbInitializer.cs*:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-379">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[](intro/samples/cu30/Data/DbInitializer.cs)]

<span data-ttu-id="e1ba1-380">Предыдущий код предоставляет начальные данные для новых сущностей.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-380">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="e1ba1-381">Основная часть кода создает объекты сущностей и загружает демонстрационные данные.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-381">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="e1ba1-382">Демонстрационные данные используются для проверки.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-382">The sample data is used for testing.</span></span> <span data-ttu-id="e1ba1-383">См. `Enrollments` и `CourseAssignments`, чтобы ознакомиться с примерами заполнения данными таблиц соединения "многие ко многим".</span><span class="sxs-lookup"><span data-stu-id="e1ba1-383">See `Enrollments` and `CourseAssignments` for examples of how many-to-many join tables can be seeded.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="e1ba1-384">Добавление миграции</span><span class="sxs-lookup"><span data-stu-id="e1ba1-384">Add a migration</span></span>

<span data-ttu-id="e1ba1-385">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-385">Build the project.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1ba1-386">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1ba1-386">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e1ba1-387">В PMC выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-387">In PMC, run the following command.</span></span>

```powershell
Add-Migration ComplexDataModel
```

<span data-ttu-id="e1ba1-388">Предыдущая команда отображает предупреждение о возможной потере данных.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-388">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="e1ba1-389">При выполнении команды `database update` возникает следующая ошибка:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-389">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

<span data-ttu-id="e1ba1-390">В следующем разделе вы узнаете, что делать с этой ошибкой.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-390">In the next section, you see what to do about this error.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e1ba1-391">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-391">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e1ba1-392">Если добавить миграцию и выполнить команду `database update`, произойдет следующая ошибка:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-392">If you add a migration and run the `database update` command, the following error would be produced:</span></span>

```text
SQLite does not support this migration operation ('DropForeignKeyOperation').
For more information, see http://go.microsoft.com/fwlink/?LinkId=723262.
```

<span data-ttu-id="e1ba1-393">В следующем разделе вы узнаете, как ее избежать.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-393">In the next section, you see how to avoid this error.</span></span>

---

## <a name="apply-the-migration-or-drop-and-re-create"></a><span data-ttu-id="e1ba1-394">Применение миграции либо удаление и повторное создание</span><span class="sxs-lookup"><span data-stu-id="e1ba1-394">Apply the migration or drop and re-create</span></span>

<span data-ttu-id="e1ba1-395">Теперь у вас есть база данных, и пора подумать о том, как применять к ней изменения.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-395">Now that you have an existing database, you need to think about how to apply changes to it.</span></span> <span data-ttu-id="e1ba1-396">В этом руководстве показано два подхода:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-396">This tutorial shows two alternatives:</span></span>

* <span data-ttu-id="e1ba1-397">[Удаление и повторное создание базы данных](#drop).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-397">[Drop and re-create the database](#drop).</span></span> <span data-ttu-id="e1ba1-398">Выберите этот раздел, если вы используете SQLite.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-398">Choose this section if you're using SQLite.</span></span>
* <span data-ttu-id="e1ba1-399">[Применение миграции к существующей базе данных](#applyexisting).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-399">[Apply the migration to the existing database](#applyexisting).</span></span> <span data-ttu-id="e1ba1-400">Инструкции в этом разделе подходят только для SQL Server, но **не для SQLite**.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-400">The instructions in this section work for SQL Server only, **not for SQLite**.</span></span> 

<span data-ttu-id="e1ba1-401">Для SQL Server применимы оба подхода.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-401">Either choice works for SQL Server.</span></span> <span data-ttu-id="e1ba1-402">Хотя метод с применением миграции является более сложным и трудоемким, в реальной рабочей среде лучше использовать именно его.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-402">While the apply-migration method is more complex and time-consuming, it's the preferred approach for real-world, production environments.</span></span> 

<a name="drop"></a>

## <a name="drop-and-re-create-the-database"></a><span data-ttu-id="e1ba1-403">Удаление и повторное создание базы данных</span><span class="sxs-lookup"><span data-stu-id="e1ba1-403">Drop and re-create the database</span></span>

<span data-ttu-id="e1ba1-404">[Пропустите этот раздел](#apply-the-migration), если вы используете SQL Server и хотите применить миграцию в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-404">[Skip this section](#apply-the-migration) if you're using SQL Server and want to do the apply-migration approach in the following section.</span></span>

<span data-ttu-id="e1ba1-405">Чтобы в EF Core принудительно создать базу данных, удалить, а затем обновить ее, выполните указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-405">To force EF Core to create a new database, drop and update the database:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1ba1-406">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1ba1-406">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e1ba1-407">Выполните следующие команды в **консоли диспетчера пакетов** (PMC):</span><span class="sxs-lookup"><span data-stu-id="e1ba1-407">In the **Package Manager Console** (PMC), run the following command:</span></span>

  ```powershell
  Drop-Database
  ```

* <span data-ttu-id="e1ba1-408">Удалите папку *Migrations*, а затем выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-408">Delete the *Migrations* folder, then run the following command:</span></span>

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e1ba1-409">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-409">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e1ba1-410">Откройте командное окно и перейдите в папку проекта.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-410">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="e1ba1-411">Папка проекта содержит файл *ContosoUniversity.csproj*.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-411">The project folder contains the *ContosoUniversity.csproj* file.</span></span>

* <span data-ttu-id="e1ba1-412">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-412">Run the following command:</span></span>

  ```console
  dotnet ef database drop --force
  ```

* <span data-ttu-id="e1ba1-413">Удалите папку *Migrations*, а затем выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-413">Delete the *Migrations* folder, then run the following command:</span></span>

  ```console
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

---

<span data-ttu-id="e1ba1-414">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-414">Run the app.</span></span> <span data-ttu-id="e1ba1-415">При запуске приложения выполняется метод `DbInitializer.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-415">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="e1ba1-416">`DbInitializer.Initialize` заполняет новую базу данных.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-416">The `DbInitializer.Initialize` populates the new database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1ba1-417">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1ba1-417">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e1ba1-418">Откройте базу данных в SSOX.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-418">Open the database in SSOX:</span></span>

* <span data-ttu-id="e1ba1-419">Если SSOX был открыт ранее, нажмите кнопку **Обновить**.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-419">If SSOX was opened previously, click the **Refresh** button.</span></span>
* <span data-ttu-id="e1ba1-420">Разверните узел **Таблицы**.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-420">Expand the **Tables** node.</span></span> <span data-ttu-id="e1ba1-421">Отображаются созданные таблицы.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-421">The created tables are displayed.</span></span>

  ![Таблицы в SSOX](complex-data-model/_static/ssox-tables.png)

* <span data-ttu-id="e1ba1-423">Изучите таблицу **CourseAssignment**:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-423">Examine the **CourseAssignment** table:</span></span>

  * <span data-ttu-id="e1ba1-424">Щелкните правой кнопкой мыши таблицу **CourseAssignment** и выберите пункт **Просмотреть данные**.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-424">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
  * <span data-ttu-id="e1ba1-425">Убедитесь, что таблица **CourseAssignment** содержит данные.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-425">Verify the **CourseAssignment** table contains data.</span></span>

  ![Данные CourseAssignment в SSOX](complex-data-model/_static/ssox-ci-data.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e1ba1-427">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-427">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e1ba1-428">Используйте средство SQLite для просмотра базы данных:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-428">Use your SQLite tool to examine the database:</span></span>

* <span data-ttu-id="e1ba1-429">новые таблицы и столбцы;</span><span class="sxs-lookup"><span data-stu-id="e1ba1-429">New tables and columns.</span></span>
* <span data-ttu-id="e1ba1-430">заполненные данные в таблицах, например в таблице **CourseAssignment**.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-430">Seeded data in tables, for example the **CourseAssignment** table.</span></span>

---

<a name="applyexisting"></a>

## <a name="apply-the-migration"></a><span data-ttu-id="e1ba1-431">Применение миграции</span><span class="sxs-lookup"><span data-stu-id="e1ba1-431">Apply the migration</span></span>

<span data-ttu-id="e1ba1-432">Этот раздел является необязательным.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-432">This section is optional.</span></span> <span data-ttu-id="e1ba1-433">Эти действия подходят только для SQL Server LocalDB и только в том случае, если вы пропустили предыдущий раздел [Удаление и повторное создание базы данных](#drop).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-433">These steps work only for SQL Server LocalDB and only if you skipped the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="e1ba1-434">При выполнении миграций с существующими данными могут действовать ограничения внешнего ключа, которым существующие данные не соответствуют.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-434">When migrations are run with existing data, there may be FK constraints that are not satisfied with the existing data.</span></span> <span data-ttu-id="e1ba1-435">Для рабочих данных нужно предпринять меры по переносу существующих данных.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-435">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="e1ba1-436">Этот раздел содержит пример того, как устранить нарушения ограничений внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-436">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="e1ba1-437">Вносите эти изменения кода только после создания резервной копии.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-437">Don't make these code changes without a backup.</span></span> <span data-ttu-id="e1ba1-438">Не вносите эти изменения в код, если вы выполнили инструкции из предыдущего раздела [Удаление и повторное создание базы данных](#drop).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-438">Don't make these code changes if you completed the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="e1ba1-439">Файл *{метка_времени}_ComplexDataModel.cs* содержит следующий код:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-439">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="e1ba1-440">Предыдущий код добавляет в таблицу `Course` внешний ключ `DepartmentID`, не допускающий значение null.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-440">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="e1ba1-441">База данных из предыдущего учебника содержит строки в `Course`, поэтому эту таблицу невозможно обновить с помощью миграций.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-441">The database from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="e1ba1-442">Чтобы заставить миграцию `ComplexDataModel` работать с существующими данными:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-442">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="e1ba1-443">Изменить код, чтобы присвоить новому столбцу (`DepartmentID`) значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-443">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="e1ba1-444">Создайте фиктивную кафедру с именем "Temp" для использования по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-444">Create a fake department named "Temp" to act as the default department.</span></span>

#### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="e1ba1-445">Устранение ограничений внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="e1ba1-445">Fix the foreign key constraints</span></span>

<span data-ttu-id="e1ba1-446">В классе миграции `Up` обновите метод `ComplexDataModel`:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-446">In the `ComplexDataModel` migration class, update the `Up` method:</span></span>

* <span data-ttu-id="e1ba1-447">Откройте файл *{метка_времени}_ComplexDataModel.cs*.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-447">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="e1ba1-448">Закомментируйте строку кода, которая добавляет столбец `DepartmentID` в таблицу `Course`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-448">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="e1ba1-449">Добавьте выделенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-449">Add the following highlighted code.</span></span> <span data-ttu-id="e1ba1-450">Новый код идет после блока `.CreateTable( name: "Department"`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-450">The new code goes after the `.CreateTable( name: "Department"` block:</span></span>

<span data-ttu-id="e1ba1-451">[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=23-31)]</span><span class="sxs-lookup"><span data-stu-id="e1ba1-451">[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=23-31)]</span></span>

<span data-ttu-id="e1ba1-452">После внесения описанных выше изменений существующие строки `Course` будут связаны с кафедрой "Temp" после выполнения метода `ComplexDataModel.Up`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-452">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel.Up` method runs.</span></span>

<span data-ttu-id="e1ba1-453">Инструкции на случай описанной здесь ситуации упрощены в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-453">The way of handling the situation shown here is simplified for this tutorial.</span></span> <span data-ttu-id="e1ba1-454">Рабочее приложение:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-454">A production app would:</span></span>

* <span data-ttu-id="e1ba1-455">включает код или сценарии, чтобы добавить строки `Department` и связанные строки `Course` к новым строкам `Department`;</span><span class="sxs-lookup"><span data-stu-id="e1ba1-455">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="e1ba1-456">не использует кафедру "Temp" или значение по умолчанию для `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-456">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1ba1-457">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1ba1-457">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e1ba1-458">Выполните следующие команды в **консоли диспетчера пакетов** (PMC):</span><span class="sxs-lookup"><span data-stu-id="e1ba1-458">In the **Package Manager Console** (PMC), run the following command:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="e1ba1-459">Так как метод `DbInitializer.Initialize` предназначен для работы только с пустой базой данных, используйте SSOX для удаления всех строк в таблицах Student и Course.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-459">Because the `DbInitializer.Initialize` method is designed to work only with an empty database, use SSOX to delete all the rows in the Student and Course tables.</span></span> <span data-ttu-id="e1ba1-460">(К таблице Enrollment будет применено каскадное удаление.)</span><span class="sxs-lookup"><span data-stu-id="e1ba1-460">(Cascade delete will take care of the Enrollment table.)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e1ba1-461">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-461">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e1ba1-462">Если вы используете SQL Server LocalDB с Visual Studio Code, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-462">If you're using SQL Server LocalDB with Visual Studio Code, run the following command:</span></span>

  ```console
  dotnet ef database update
  ```

---

<span data-ttu-id="e1ba1-463">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-463">Run the app.</span></span> <span data-ttu-id="e1ba1-464">При запуске приложения выполняется метод `DbInitializer.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-464">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="e1ba1-465">`DbInitializer.Initialize` заполняет новую базу данных.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-465">The `DbInitializer.Initialize` populates the new database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e1ba1-466">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="e1ba1-466">Next steps</span></span>

<span data-ttu-id="e1ba1-467">В следующих двух учебниках рассказывается о том, как считывать и обновлять связанные данные.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-467">The next two tutorials show how to read and update related data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e1ba1-468">[Предыдущий учебник](xref:data/ef-rp/migrations)
> [Следующий учебник](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="e1ba1-468">[Previous tutorial](xref:data/ef-rp/migrations)
[Next tutorial](xref:data/ef-rp/read-related-data)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e1ba1-469">Предыдущие руководства работали с базовой моделью данных, состоящей из трех сущностей.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-469">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="e1ba1-470">В этом руководстве:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-470">In this tutorial:</span></span>

* <span data-ttu-id="e1ba1-471">Добавляются дополнительные сущности и связи.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-471">More entities and relationships are added.</span></span>
* <span data-ttu-id="e1ba1-472">Настраивается модель данных с помощью указания правил для форматирования, проверки и сопоставления базы данных.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-472">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="e1ba1-473">Классы сущностей для готовой модели данных показаны на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-473">The entity classes for the completed data model are shown in the following illustration:</span></span>

![Схема сущностей](complex-data-model/_static/diagram.png)

<span data-ttu-id="e1ba1-475">При возникновении проблем, которые вам не удается устранить, скачайте [готовое приложение](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-475">If you run into problems you can't solve, download the [completed app](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span>

## <a name="customize-the-data-model-with-attributes"></a><span data-ttu-id="e1ba1-476">Настройка модели данных с использованием атрибутов</span><span class="sxs-lookup"><span data-stu-id="e1ba1-476">Customize the data model with attributes</span></span>

<span data-ttu-id="e1ba1-477">В этом разделе модель данных настраивается с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-477">In this section, the data model is customized using attributes.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="e1ba1-478">Атрибут DataType</span><span class="sxs-lookup"><span data-stu-id="e1ba1-478">The DataType attribute</span></span>

<span data-ttu-id="e1ba1-479">Страницы учащихся сейчас отображают время и дату зачисления.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-479">The student pages currently displays the time of the enrollment date.</span></span> <span data-ttu-id="e1ba1-480">Как правило, поля даты отображают только дату без времени.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-480">Typically, date fields show only the date and not the time.</span></span>

<span data-ttu-id="e1ba1-481">Обновите *Models/Student.cs*, используя следующий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-481">Update *Models/Student.cs* with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="e1ba1-482">Атрибут [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) указывает тип данных более точно по сравнению со встроенным типом базы данных.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-482">The [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="e1ba1-483">В этом случае следует отображать отобразить только дату, а не дату и время.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-483">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="e1ba1-484">В [перечислении DataType](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) представлено множество типов данных, таких как Date, Time, PhoneNumber, Currency, EmailAddress и других. Атрибут `DataType` также обеспечивает автоматическое предоставление функций для определенных типов в приложении.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-484">The [DataType Enumeration](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="e1ba1-485">Например:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-485">For example:</span></span>

* <span data-ttu-id="e1ba1-486">Ссылка `mailto:` для `DataType.EmailAddress` создается автоматически.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-486">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="e1ba1-487">Средство выбора даты предоставляется для `DataType.Date` в большинстве браузеров.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-487">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="e1ba1-488">Атрибут `DataType` создает атрибуты HTML 5 `data-`, которые используются браузерами с поддержкой HTML 5.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-488">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="e1ba1-489">Атрибуты `DataType` не предназначены для проверки.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-489">The `DataType` attributes don't provide validation.</span></span>

<span data-ttu-id="e1ba1-490">`DataType.Date` не задает формат отображаемой даты.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-490">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="e1ba1-491">По умолчанию поле даты отображается с использованием форматов, установленных в [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support) сервера.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-491">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="e1ba1-492">С помощью атрибута `DisplayFormat` можно явно указать формат даты:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-492">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="e1ba1-493">Параметр `ApplyFormatInEditMode` указывает, что формат должен применяться к пользовательскому интерфейсу редактирования.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-493">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="e1ba1-494">Некоторым полям не следует использовать `ApplyFormatInEditMode`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-494">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="e1ba1-495">Например, обозначение денежной единицы в общем случае не должно отображаться в поле редактирования текста.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-495">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="e1ba1-496">Атрибут `DisplayFormat` можно использовать отдельно.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-496">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="e1ba1-497">Однако чаще всего `DataType` рекомендуется применять вместе с атрибутом `DisplayFormat`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-497">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="e1ba1-498">Атрибут `DataType` передает семантику данных (в отличие от способа их вывода на экран).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-498">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="e1ba1-499">Атрибут `DataType` дает следующие преимущества, недоступные в `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-499">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="e1ba1-500">Поддержка функций HTML5 в браузере.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-500">The browser can enable HTML5 features.</span></span> <span data-ttu-id="e1ba1-501">Например, отображение элемента управления календарем, соответствующего языковому стандарту обозначения денежной единицы, ссылок электронной почты, проверки входных данных на стороне клиента и т. д.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-501">For example, show a calendar control, the locale-appropriate currency symbol, email links, client-side input validation, etc.</span></span>
* <span data-ttu-id="e1ba1-502">По умолчанию формат отображения данных в браузере определяется в соответствии с установленным языковым стандартом.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-502">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="e1ba1-503">Дополнительные сведения см. в документации по [вспомогательной функции тегов \<input>](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-503">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

<span data-ttu-id="e1ba1-504">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-504">Run the app.</span></span> <span data-ttu-id="e1ba1-505">Перейдите на страницу индекса учащихся.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-505">Navigate to the Students Index page.</span></span> <span data-ttu-id="e1ba1-506">Время больше не отображается.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-506">Times are no longer displayed.</span></span> <span data-ttu-id="e1ba1-507">Каждое представление, использующее модель `Student`, отображает дату без времени.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-507">Every view that uses the `Student` model displays the date without time.</span></span>

![Страница указателя учащихся с датами без времени](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="e1ba1-509">Атрибут StringLength</span><span class="sxs-lookup"><span data-stu-id="e1ba1-509">The StringLength attribute</span></span>

<span data-ttu-id="e1ba1-510">С помощью атрибутов можно указать правила проверки данных и сообщения об ошибках проверки.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-510">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="e1ba1-511">Атрибут [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) указывает минимальную и максимальную длину символов, разрешенных в поле данных.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-511">The [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="e1ba1-512">Атрибут `StringLength` также обеспечивает проверку на стороне клиента и на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-512">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="e1ba1-513">Минимальное значение не оказывает влияния на схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-513">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="e1ba1-514">Обновите модель`Student`, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-514">Update the `Student` model with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="e1ba1-515">Предыдущий код задает ограничение длины имен в 50 символов.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-515">The preceding code limits names to no more than 50 characters.</span></span> <span data-ttu-id="e1ba1-516">Атрибут `StringLength` не запрещает пользователю ввести пробел в качестве имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-516">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="e1ba1-517">Атрибут [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) используется для применения ограничений к входным данным.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-517">The [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute is used to apply restrictions to the input.</span></span> <span data-ttu-id="e1ba1-518">Например, следующий код требует, чтобы первый символ был прописной буквой, а остальные символы были буквенными:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-518">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

<span data-ttu-id="e1ba1-519">Запустите приложение:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-519">Run the app:</span></span>

* <span data-ttu-id="e1ba1-520">Перейдите на страницу учащихся.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-520">Navigate to the Students page.</span></span>
* <span data-ttu-id="e1ba1-521">Выберите **Create New** (Создать) и введите имя длиной более 50 символов.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-521">Select **Create New**, and enter a name longer than 50 characters.</span></span>
* <span data-ttu-id="e1ba1-522">Выберите **Create** (Создать), проверка на стороне клиента отображает сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-522">Select **Create**, client-side validation shows an error message.</span></span>

![Страница указателя учащихся с ошибками длины строки](complex-data-model/_static/string-length-errors.png)

<span data-ttu-id="e1ba1-524">В **обозревателе объектов SQL Server** (SSOX) откройте конструктор таблиц учащихся, дважды щелкнув таблицу **Student** (Учащийся).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-524">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Таблица учащихся в SSOX до миграций](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="e1ba1-526">На предыдущем изображении показана схемы для таблицы `Student`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-526">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="e1ba1-527">Поля имен имеют тип `nvarchar(MAX)`, так как миграции для базы данных не выполнялись.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-527">The name fields have type `nvarchar(MAX)` because migrations has not been run on the DB.</span></span> <span data-ttu-id="e1ba1-528">Когда в дальнейшем миграции будут выполнены, поля имен станут `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-528">When migrations are run later in this tutorial, the name fields become `nvarchar(50)`.</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="e1ba1-529">Атрибут Column</span><span class="sxs-lookup"><span data-stu-id="e1ba1-529">The Column attribute</span></span>

<span data-ttu-id="e1ba1-530">Атрибуты могут управлять, как именно классы и свойства сопоставляются с базой данных.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-530">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="e1ba1-531">В этом разделе атрибут `Column` используется для сопоставления имени свойства `FirstMidName` с "FirstName" в базе данных.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-531">In this section, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the DB.</span></span>

<span data-ttu-id="e1ba1-532">При создании базы данных имена свойств в модели используются для имен столбцов (кроме случая, когда используется атрибут `Column`).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-532">When the DB is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span>

<span data-ttu-id="e1ba1-533">Модель `Student` использует `FirstMidName` для поля имени, так как это поле также может содержать отчество.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-533">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="e1ba1-534">Обновите файл *Student.cs*, используя следующий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-534">Update the *Student.cs* file with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="e1ba1-535">С учетом предыдущего изменения `Student.FirstMidName` в приложении сопоставляется со столбцом `FirstName` таблицы `Student`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-535">With the preceding change, `Student.FirstMidName` in the app maps to the `FirstName` column of the `Student` table.</span></span>

<span data-ttu-id="e1ba1-536">Добавление атрибута `Column` изменяет модель для поддержки `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-536">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="e1ba1-537">Модель, поддерживающая `SchoolContext`, больше не соответствует базе данных.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-537">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="e1ba1-538">Если приложение запускается перед применением миграций, возникает следующее исключение:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-538">If the app is run before applying migrations, the following exception is generated:</span></span>

```SQL
SqlException: Invalid column name 'FirstName'.
```

<span data-ttu-id="e1ba1-539">Для обновления базы данных сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-539">To update the DB:</span></span>

* <span data-ttu-id="e1ba1-540">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-540">Build the project.</span></span>
* <span data-ttu-id="e1ba1-541">Откройте командное окно в папке проекта.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-541">Open a command window in the project folder.</span></span> <span data-ttu-id="e1ba1-542">Введите следующие команды для создания миграции и обновления базы данных:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-542">Enter the following commands to create a new migration and update the DB:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1ba1-543">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1ba1-543">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration ColumnFirstName
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e1ba1-544">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-544">Visual Studio Code</span></span>](#tab/visual-studio-code)

```console
dotnet ef migrations add ColumnFirstName
dotnet ef database update
```

---

<span data-ttu-id="e1ba1-545">Команда `migrations add ColumnFirstName` выдает следующее предупреждающее сообщение:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-545">The `migrations add ColumnFirstName` command generates the following warning message:</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

<span data-ttu-id="e1ba1-546">Это предупреждение вызвано тем, что поля имен теперь ограничены 50 символами.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-546">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="e1ba1-547">Если имя в базе данных имеет больше 50 символов, символы с 51 до последнего будут потеряны.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-547">If a name in the DB had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="e1ba1-548">Проверьте работу приложения.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-548">Test the app.</span></span>

<span data-ttu-id="e1ba1-549">Откройте таблицу "Student" (Учащийся) в окне SSOX:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-549">Open the Student table in SSOX:</span></span>

![Таблица учащихся в SSOX после миграций](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="e1ba1-551">До применения миграции столбцы имен имели тип [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-551">Before migration was applied, the name columns were of type [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="e1ba1-552">Теперь столбцы имен имеют тип `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-552">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="e1ba1-553">Имя столбца изменилось с `FirstMidName` на `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-553">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

> [!Note]
> <span data-ttu-id="e1ba1-554">В следующем разделе сборка приложения на некоторых этапах приводит к возникновению ошибок компилятора.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-554">In the following section, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="e1ba1-555">В инструкциях указано, когда производить сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-555">The instructions specify when to build the app.</span></span>

## <a name="student-entity-update"></a><span data-ttu-id="e1ba1-556">Обновление сущности Student</span><span class="sxs-lookup"><span data-stu-id="e1ba1-556">Student entity update</span></span>

![Сущность Student](complex-data-model/_static/student-entity.png)

<span data-ttu-id="e1ba1-558">Обновите *Models/Student.cs*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-558">Update *Models/Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="e1ba1-559">Атрибут Required</span><span class="sxs-lookup"><span data-stu-id="e1ba1-559">The Required attribute</span></span>

<span data-ttu-id="e1ba1-560">Атрибут `Required` делает свойства имен обязательными полями.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-560">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="e1ba1-561">Атрибут `Required` не нужен для типов, не допускающих значения null, например для типов значений (`DateTime`, `int`, `double` и т. д.).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-561">The `Required` attribute isn't needed for non-nullable types such as value types (`DateTime`, `int`, `double`, etc.).</span></span> <span data-ttu-id="e1ba1-562">Типы, которые не могут принимать значение null, автоматически обрабатываются как обязательные поля.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-562">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="e1ba1-563">Атрибут `Required` можно заменить параметром минимальной длины в атрибуте `StringLength`:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-563">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="e1ba1-564">Атрибут Display</span><span class="sxs-lookup"><span data-stu-id="e1ba1-564">The Display attribute</span></span>

<span data-ttu-id="e1ba1-565">Атрибут `Display` указывает, что заголовки для текстовых полей должны иметь вид "First Name" (Имя), "Last Name" (Фамилия), "Full Name" (Полное имя) и "Enrollment Date" (Дата зачисления).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-565">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="e1ba1-566">По умолчанию в заголовках не использовался пробел для разделения слов, например "Lastname".</span><span class="sxs-lookup"><span data-stu-id="e1ba1-566">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="e1ba1-567">Вычисляемое свойство FullName</span><span class="sxs-lookup"><span data-stu-id="e1ba1-567">The FullName calculated property</span></span>

<span data-ttu-id="e1ba1-568">`FullName` — это вычисляемое свойство, которое возвращает значение, созданное путем объединения двух других свойств.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-568">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="e1ba1-569">`FullName` не может быть задано, оно имеет только метод доступа get.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-569">`FullName` cannot be set, it has only a get accessor.</span></span> <span data-ttu-id="e1ba1-570">В базе данных не создается столбец `FullName`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-570">No `FullName` column is created in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="e1ba1-571">Создание сущности Instructor</span><span class="sxs-lookup"><span data-stu-id="e1ba1-571">Create the Instructor Entity</span></span>

![Сущность Instructor](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="e1ba1-573">Создайте файл *Models/Instructor.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-573">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Instructor.cs)]

<span data-ttu-id="e1ba1-574">На одной строке могут находиться несколько атрибутов.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-574">Multiple attributes can be on one line.</span></span> <span data-ttu-id="e1ba1-575">Атрибуты `HireDate` можно записать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-575">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="e1ba1-576">Свойства навигации CourseAssignments и OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="e1ba1-576">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="e1ba1-577">`CourseAssignments` и `OfficeAssignment` — это свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-577">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="e1ba1-578">Преподаватель может проводить любое количество курсов, поэтому `CourseAssignments` определен как коллекция.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-578">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="e1ba1-579">Если свойство навигации содержит несколько сущностей:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-579">If a navigation property holds multiple entities:</span></span>

* <span data-ttu-id="e1ba1-580">Оно должно быть типом списка, где можно добавлять, удалять и обновлять записи.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-580">It must be a list type where the entries can be added, deleted, and updated.</span></span>

<span data-ttu-id="e1ba1-581">К типам свойств навигации относятся следующие:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-581">Navigation property types include:</span></span>

* `ICollection<T>`
* `List<T>`
* `HashSet<T>`

<span data-ttu-id="e1ba1-582">Если указан тип `ICollection<T>`, платформа EF Core по умолчанию создает коллекцию `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-582">If `ICollection<T>` is specified, EF Core creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="e1ba1-583">Сущность `CourseAssignment` описана в разделе по связям многие ко многим.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-583">The `CourseAssignment` entity is explained in the section on many-to-many relationships.</span></span>

<span data-ttu-id="e1ba1-584">Бизнес-правила университета Contoso указывают, что преподаватель может иметь не более одного кабинета.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-584">Contoso University business rules state that an instructor can have at most one office.</span></span> <span data-ttu-id="e1ba1-585">Свойство `OfficeAssignment` содержит отдельную сущность `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-585">The `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="e1ba1-586">`OfficeAssignment` имеет значение null, если кабинет не назначен.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-586">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="e1ba1-587">Создание сущности OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="e1ba1-587">Create the OfficeAssignment entity</span></span>

![Сущность OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="e1ba1-589">Создайте файл *Models/OfficeAssignment.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-589">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="e1ba1-590">Атрибут Key</span><span class="sxs-lookup"><span data-stu-id="e1ba1-590">The Key attribute</span></span>

<span data-ttu-id="e1ba1-591">Атрибут `[Key]` используется для идентификации свойства в качестве первичного ключа (PK), когда имя свойства отличается от classnameID или ID.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-591">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="e1ba1-592">Между сущностями `Instructor` и `OfficeAssignment` действует связь один к нулю или к одному.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-592">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="e1ba1-593">Назначение кабинета существует только в связи с преподавателем, которому оно назначено.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-593">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="e1ba1-594">Первичный ключ `OfficeAssignment` также является внешним ключом (FK) для сущности `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-594">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span> <span data-ttu-id="e1ba1-595">EF Core не распознает автоматически `InstructorID` как первичный ключ объекта `OfficeAssignment` по следующей причине:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-595">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because:</span></span>

* <span data-ttu-id="e1ba1-596">`InstructorID` не соблюдает соглашение об именовании ID или classnameID.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-596">`InstructorID` doesn't follow the ID or classnameID naming convention.</span></span>

<span data-ttu-id="e1ba1-597">Таким образом, атрибут `Key` используется для определения `InstructorID` в качестве первичного ключа:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-597">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="e1ba1-598">По умолчанию EF Core считает ключ созданным не базой данных, так как столбец предназначен для идентифицирующего отношения.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-598">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="e1ba1-599">Свойство навигации Instructor</span><span class="sxs-lookup"><span data-stu-id="e1ba1-599">The Instructor navigation property</span></span>

<span data-ttu-id="e1ba1-600">Свойство навигации `OfficeAssignment` для сущности `Instructor` допускает значение null по следующим причинам:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-600">The `OfficeAssignment` navigation property for the `Instructor` entity is nullable because:</span></span>

* <span data-ttu-id="e1ba1-601">Ссылочные типы (например, классы) допускают значение null.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-601">Reference types (such as classes are nullable).</span></span>
* <span data-ttu-id="e1ba1-602">Преподавателю может быть не назначен кабинет.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-602">An instructor might not have an office assignment.</span></span>

<span data-ttu-id="e1ba1-603">Сущность `OfficeAssignment` имеет свойство навигации `Instructor`, не допускающее значение null, по следующим причинам:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-603">The `OfficeAssignment` entity has a non-nullable `Instructor` navigation property because:</span></span>

* <span data-ttu-id="e1ba1-604">`InstructorID` не допускает значение null.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-604">`InstructorID` is non-nullable.</span></span>
* <span data-ttu-id="e1ba1-605">Назначение кабинета не может существовать без преподавателя.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-605">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="e1ba1-606">Когда сущность `Instructor` имеет связанную сущность `OfficeAssignment`, каждая из них имеет ссылку на другую в своем свойстве навигации.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-606">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="e1ba1-607">Атрибут `[Required]` можно применить к свойству навигации `Instructor`:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-607">The `[Required]` attribute could be applied to the `Instructor` navigation property:</span></span>

```csharp
[Required]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="e1ba1-608">Предыдущий код указывает, что должен существовать связанный преподаватель.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-608">The preceding code specifies that there must be a related instructor.</span></span> <span data-ttu-id="e1ba1-609">Этот код не нужен, так как внешний ключ `InstructorID` (который также является первичным) не допускает значение null.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-609">The preceding code is unnecessary because the `InstructorID` foreign key (which is also the PK) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="e1ba1-610">Изменение сущности Course</span><span class="sxs-lookup"><span data-stu-id="e1ba1-610">Modify the Course Entity</span></span>

![Сущность Course](complex-data-model/_static/course-entity.png)

<span data-ttu-id="e1ba1-612">Обновите *Models/Course.cs*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-612">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="e1ba1-613">Сущность `Course` имеет свойство внешнего ключа (FK) `DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-613">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="e1ba1-614">`DepartmentID` указывает на связанную сущность `Department`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-614">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="e1ba1-615">Сущность `Course` имеет свойство навигации `Department`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-615">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="e1ba1-616">EF Core не требует свойство внешнего ключа для модели данных, если она имеет свойство навигации для связанной сущности.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-616">EF Core doesn't require a FK property for a data model when the model has a navigation property for a related entity.</span></span>

<span data-ttu-id="e1ba1-617">EF Core автоматически создает внешние ключи в базе данных по мере необходимости.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-617">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="e1ba1-618">EF Core создает [теневые свойства](/ef/core/modeling/shadow-properties) для автоматически созданных внешних ключей.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-618">EF Core creates [shadow properties](/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="e1ba1-619">Наличие внешнего ключа в модели данных позволяет сделать обновления проще и эффективнее.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-619">Having the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="e1ba1-620">Например, рассмотрим модель, где свойство внешнего ключа `DepartmentID` *не* включено.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-620">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="e1ba1-621">При получении сущности курса для редактирования:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-621">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="e1ba1-622">сущность `Department` имеет значение null, если она не загружена явно;</span><span class="sxs-lookup"><span data-stu-id="e1ba1-622">The `Department` entity is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="e1ba1-623">для обновления сущности курса нужно сначала получить сущность `Department`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-623">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="e1ba1-624">Если свойство внешнего ключа `DepartmentID` включено в модель данных, получать сущность `Department` перед обновлением не нужно.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-624">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="e1ba1-625">Атрибут DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="e1ba1-625">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="e1ba1-626">Атрибут `[DatabaseGenerated(DatabaseGeneratedOption.None)]` указывает, что первичный ключ предоставляется приложением, а не создается базой данных.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-626">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="e1ba1-627">По умолчанию EF Core предполагает, что значения первичного ключа создаются базой данных.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-627">By default, EF Core assumes that PK values are generated by the DB.</span></span> <span data-ttu-id="e1ba1-628">Обычно лучше всего использовать значения первичного ключа, созданные базой данных.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-628">DB generated PK values is generally the best approach.</span></span> <span data-ttu-id="e1ba1-629">Для сущностей `Course` пользователь указывает первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-629">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="e1ba1-630">Например, номер курса, такой как серия 1000 для кафедры математики и серия 2000 для кафедры английского языка.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-630">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="e1ba1-631">Атрибут `DatabaseGenerated` также может использоваться для создания значений по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-631">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="e1ba1-632">Например, база данных может автоматически создать поле даты для записи даты, когда строка была создана или изменена.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-632">For example, the DB can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="e1ba1-633">Дополнительные сведения см. в разделе [Созданные свойства](/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-633">For more information, see [Generated Properties](/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="e1ba1-634">Свойства внешнего ключа и навигации</span><span class="sxs-lookup"><span data-stu-id="e1ba1-634">Foreign key and navigation properties</span></span>

<span data-ttu-id="e1ba1-635">Свойства внешнего ключа (FK) и свойства навигации в сущности `Course` отражают следующие связи:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-635">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="e1ba1-636">Курс назначается одной кафедре, поэтому имеется внешний ключ `DepartmentID` и свойство навигации `Department`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-636">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="e1ba1-637">На курс может быть зачислено любое количество учащихся, поэтому свойство навигации `Enrollments` является коллекцией:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-637">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="e1ba1-638">Курс могут вести несколько преподавателей, поэтому свойство навигации `CourseAssignments` является коллекцией:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-638">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="e1ba1-639">`CourseAssignment` описано [далее](#many-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-639">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="create-the-department-entity"></a><span data-ttu-id="e1ba1-640">Создание сущности Department</span><span class="sxs-lookup"><span data-stu-id="e1ba1-640">Create the Department entity</span></span>

![Сущность Department](complex-data-model/_static/department-entity.png)

<span data-ttu-id="e1ba1-642">Создайте файл *Models/Department.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-642">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="e1ba1-643">Атрибут Column</span><span class="sxs-lookup"><span data-stu-id="e1ba1-643">The Column attribute</span></span>

<span data-ttu-id="e1ba1-644">Ранее атрибут `Column` использовался, чтобы изменить сопоставление имени столбца.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-644">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="e1ba1-645">В коде для сущности `Department` атрибут `Column` можно использовать, чтобы изменить сопоставление типов данных SQL.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-645">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="e1ba1-646">Столбец `Budget` определяется с помощью типа money SQL Server в базе данных:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-646">The `Budget` column is defined using the SQL Server money type in the DB:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="e1ba1-647">Сопоставление столбцов обычно не требуется.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-647">Column mapping is generally not required.</span></span> <span data-ttu-id="e1ba1-648">В общем случае EF Core выбирает соответствующий тип данных SQL Server на основе типа среды CLR для свойства.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-648">EF Core generally chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="e1ba1-649">Тип `decimal` среды CLR сопоставляется с типом `decimal` SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-649">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="e1ba1-650">`Budget` используется для денежных единиц, хотя для этого лучше подходит тип данных money.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-650">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="e1ba1-651">Свойства внешнего ключа и навигации</span><span class="sxs-lookup"><span data-stu-id="e1ba1-651">Foreign key and navigation properties</span></span>

<span data-ttu-id="e1ba1-652">Свойства внешнего ключа и навигации отражают следующие связи:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-652">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="e1ba1-653">Кафедра может иметь или не иметь администратора.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-653">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="e1ba1-654">Администратор всегда является преподавателем.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-654">An administrator is always an instructor.</span></span> <span data-ttu-id="e1ba1-655">Поэтому свойство `InstructorID` включается в качестве внешнего ключа в сущность `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-655">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="e1ba1-656">Свойство навигации называется `Administrator`, но содержит сущность `Instructor`:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-656">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="e1ba1-657">Вопросительный знак (?) в приведенном выше коде указывает, что свойство допускает значение null.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-657">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="e1ba1-658">Кафедра может иметь несколько курсов, поэтому доступно свойство навигации Courses:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-658">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="e1ba1-659">Примечание. По соглашению EF Core разрешает каскадное удаление для внешних ключей, не допускающих значение null, и связей "многие ко многим".</span><span class="sxs-lookup"><span data-stu-id="e1ba1-659">Note: By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="e1ba1-660">Каскадное удаление может привести к циклическим правилам каскадного удаления.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-660">Cascading delete can result in circular cascade delete rules.</span></span> <span data-ttu-id="e1ba1-661">Такие правила вызывают исключение при добавлении миграции.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-661">Circular cascade delete rules causes an exception when a migration is added.</span></span>

<span data-ttu-id="e1ba1-662">Например, если свойство `Department.InstructorID` было определено как не допускающее значения NULL:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-662">For example, if the `Department.InstructorID` property was defined as non-nullable:</span></span>

* <span data-ttu-id="e1ba1-663">EF Core настраивает правило каскадного удаления для удаления кафедры при удалении преподавателя.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-663">EF Core configures a cascade delete rule to delete the department when the instructor is deleted.</span></span>
* <span data-ttu-id="e1ba1-664">Удаление кафедры при удалении преподавателя не является запланированным поведением.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-664">Deleting the department when the instructor is deleted isn't the intended behavior.</span></span>
* <span data-ttu-id="e1ba1-665">Следующий текучий API будет задать правило ограничения вместо каскада.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-665">The following fluent API would set a restrict rule instead of cascade.</span></span>

   ```csharp
   modelBuilder.Entity<Department>()
      .HasOne(d => d.Administrator)
      .WithMany()
      .OnDelete(DeleteBehavior.Restrict)
  ```

<span data-ttu-id="e1ba1-666">Предыдущий код отключает каскадное удаление для связи кафедры и преподавателя.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-666">The preceding code disables cascade delete on the department-instructor relationship.</span></span>

## <a name="update-the-enrollment-entity"></a><span data-ttu-id="e1ba1-667">Обновление сущности Enrollment</span><span class="sxs-lookup"><span data-stu-id="e1ba1-667">Update the Enrollment entity</span></span>

<span data-ttu-id="e1ba1-668">Запись зачисления обозначает один курс, который проходит один учащийся.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-668">An enrollment record is for one course taken by one student.</span></span>

![Сущность Enrollment](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="e1ba1-670">Обновите *Models/Enrollment.cs*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-670">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="e1ba1-671">Свойства внешнего ключа и навигации</span><span class="sxs-lookup"><span data-stu-id="e1ba1-671">Foreign key and navigation properties</span></span>

<span data-ttu-id="e1ba1-672">Свойства внешнего ключа и навигации отражают следующие связи:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-672">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="e1ba1-673">Запись зачисления предназначена для одного курса, поэтому доступно свойство первичного ключа `CourseID` и свойство навигации `Course`:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-673">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="e1ba1-674">Запись зачисления предназначена для одного учащегося, поэтому доступно свойство первичного ключа `StudentID` и свойство навигации `Student`:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-674">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="e1ba1-675">Связи многие ко многим</span><span class="sxs-lookup"><span data-stu-id="e1ba1-675">Many-to-Many Relationships</span></span>

<span data-ttu-id="e1ba1-676">Между сущностями `Student` и `Course` действует связь многие ко многим.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-676">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="e1ba1-677">Сущности `Enrollment` выступают в качестве таблицы соединения многие ко многим *с полезными данными* в базе данных.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-677">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="e1ba1-678">Фраза "с полезными данными" означает, что таблица `Enrollment` содержит дополнительные данные, кроме внешних ключей для присоединяемых таблиц (в данном случае — первичный ключ и `Grade`).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-678">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="e1ba1-679">На следующем рисунке показано, как выглядят эти связи на схеме сущностей.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-679">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="e1ba1-680">(Эта схема была создана с помощью [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) для EF 6.x.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-680">(This diagram was generated using [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) for EF 6.x.</span></span> <span data-ttu-id="e1ba1-681">Создание схемы не является частью руководства.)</span><span class="sxs-lookup"><span data-stu-id="e1ba1-681">Creating the diagram isn't part of the tutorial.)</span></span>

![Связь многие ко многим между Student и Course](complex-data-model/_static/student-course.png)

<span data-ttu-id="e1ba1-683">Каждая линия связи имеет 1 на одном конце и звездочку (\*) на другом, указывая характер один ко многим.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-683">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="e1ba1-684">Если таблица `Enrollment` не включала в себя сведения об оценках, ей потребуется содержать всего два внешних ключа (`CourseID` и `StudentID`).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-684">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="e1ba1-685">Таблицу соединения многие ко многим без полезных данных иногда называют чистой таблицей соединения (PJT).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-685">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="e1ba1-686">Сущности `Instructor` и `Course` имеют связь многие ко многим с использованием чистой таблицы соединения.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-686">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="e1ba1-687">Примечание. EF 6.x поддерживает неявные таблицы соединения для связей многие ко многим, но EF Core — нет.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-687">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="e1ba1-688">Дополнительные сведения см. в статье [Связи многие ко многим в EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-688">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="e1ba1-689">Сущность CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="e1ba1-689">The CourseAssignment entity</span></span>

![Сущность CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="e1ba1-691">Создайте файл *Models/CourseAssignment.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-691">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a><span data-ttu-id="e1ba1-692">Связь между Instructor и Courses</span><span class="sxs-lookup"><span data-stu-id="e1ba1-692">Instructor-to-Courses</span></span>

![Связь многие ко многим между Instructor и Courses](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="e1ba1-694">Связь многие ко многим между Instructor и Courses:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-694">The Instructor-to-Courses many-to-many relationship:</span></span>

* <span data-ttu-id="e1ba1-695">Нуждается в таблице соединения, которая должна быть представлена набором сущностей.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-695">Requires a join table that must be represented by an entity set.</span></span>
* <span data-ttu-id="e1ba1-696">Является чистой таблицей соединения (таблицей без полезных данных).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-696">Is a pure join table (table without payload).</span></span>

<span data-ttu-id="e1ba1-697">Обычно для сущности соединения используется имя `EntityName1EntityName2`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-697">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="e1ba1-698">Например, таблицей соединения Instructor и Courses, использующей этот шаблон, является `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-698">For example, the Instructor-to-Courses join table using this pattern is `CourseInstructor`.</span></span> <span data-ttu-id="e1ba1-699">Однако рекомендуется использовать имя, которое описывает эту связь.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-699">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="e1ba1-700">Модели данных создаются простыми и разрастаются.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-700">Data models start out simple and grow.</span></span> <span data-ttu-id="e1ba1-701">Соединения без полезных данных (PJT) часто начинают включать их.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-701">No-payload joins (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="e1ba1-702">Если в начале задать описательное имя сущности, его не нужно менять при изменениях таблицы соединения.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-702">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="e1ba1-703">Оптимально, если сущность соединения имеет собственное естественное имя (возможно, из одного слова) в бизнес-среде.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-703">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="e1ba1-704">Например, Books и Customers можно связать с сущностью соединения Ratings.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-704">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="e1ba1-705">Связь многие ко многим между Instructor и Courses `CourseAssignment` является более предпочтительным вариантом, чем `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-705">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="e1ba1-706">Составной ключ</span><span class="sxs-lookup"><span data-stu-id="e1ba1-706">Composite key</span></span>

<span data-ttu-id="e1ba1-707">Внешние ключи не допускают значение null.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-707">FKs are not nullable.</span></span> <span data-ttu-id="e1ba1-708">Два внешних ключа в `CourseAssignment` (`InstructorID` и `CourseID`) совместно однозначно определяют каждую строку таблицы `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-708">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="e1ba1-709">`CourseAssignment` не требуется выделенный первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-709">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="e1ba1-710">Свойства `InstructorID` и `CourseID` выступают в качестве составного первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-710">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="e1ba1-711">Единственным способом указать составные первичные ключи для EF Core является *текучий API*.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-711">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="e1ba1-712">Следующий раздел описывает, как настроить составной первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-712">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="e1ba1-713">Составной ключ обеспечивает следующее:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-713">The composite key ensures:</span></span>

* <span data-ttu-id="e1ba1-714">Для одного курса допускается несколько строк.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-714">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="e1ba1-715">Для одного преподавателя допускается несколько строк.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-715">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="e1ba1-716">Несколько строк для одного преподавателя и курса недопустимы.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-716">Multiple rows for the same instructor and course isn't allowed.</span></span>

<span data-ttu-id="e1ba1-717">Сущность соединения `Enrollment` определяет собственный первичный ключ, поэтому подобные дубликаты возможны.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-717">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="e1ba1-718">Для предотвращения подобных дубликатов:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-718">To prevent such duplicates:</span></span>

* <span data-ttu-id="e1ba1-719">добавьте уникальный индекс для полей внешнего ключа или</span><span class="sxs-lookup"><span data-stu-id="e1ba1-719">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="e1ba1-720">настройте `Enrollment` с первичным составным ключом аналогично `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-720">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="e1ba1-721">Дополнительные сведения см. в разделе [Индексы](/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-721">For more information, see [Indexes](/ef/core/modeling/indexes).</span></span>

## <a name="update-the-db-context"></a><span data-ttu-id="e1ba1-722">Изменение контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="e1ba1-722">Update the DB context</span></span>

<span data-ttu-id="e1ba1-723">Добавьте выделенный ниже код в файл *Data/SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-723">Add the following highlighted code to *Data/SchoolContext.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="e1ba1-724">Предыдущий код добавляет новые сущности и настраивает составной первичный ключ сущности `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-724">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="e1ba1-725">Текучий API вместо атрибутов</span><span class="sxs-lookup"><span data-stu-id="e1ba1-725">Fluent API alternative to attributes</span></span>

<span data-ttu-id="e1ba1-726">Метод `OnModelCreating` в предыдущем коде использует для настройки поведения EF Core *текучий API*.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-726">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="e1ba1-727">Этот API называется "текучим", так как часто используется для объединения серии вызовов методов в один оператор.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-727">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="e1ba1-728">В [следующем коде](/ef/core/modeling/#use-fluent-api-to-configure-a-model) показан пример текучего API:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-728">The [following code](/ef/core/modeling/#use-fluent-api-to-configure-a-model) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="e1ba1-729">В этом руководстве текучий API используется только для сопоставления базы данных, которое невозможно выполнить с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-729">In this tutorial, the fluent API is used only for DB mapping that can't be done with attributes.</span></span> <span data-ttu-id="e1ba1-730">Однако текучий API позволяет задать большинство правил форматирования, проверки и сопоставления, которые можно указать с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-730">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="e1ba1-731">Некоторые атрибуты, такие как `MinimumLength`, невозможно применить с текучим API.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-731">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="e1ba1-732">`MinimumLength` не изменяет схему, он лишь применяет правило проверки минимальной длины.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-732">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="e1ba1-733">Некоторые разработчики предпочитают использовать текучий API монопольно, чтобы оставить свои классы сущностей "чистыми".</span><span class="sxs-lookup"><span data-stu-id="e1ba1-733">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="e1ba1-734">Атрибуты и текучий API можно смешивать.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-734">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="e1ba1-735">Существует несколько конфигураций, которые можно реализовать только с помощью текучего API (с указанием составного первичного ключа).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-735">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="e1ba1-736">Существует несколько конфигураций, которые можно реализовать только с помощью атрибутов (`MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-736">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="e1ba1-737">Рекомендации по использованию текучего API и атрибутов:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-737">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="e1ba1-738">Выберите один из двух этих подходов.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-738">Choose one of these two approaches.</span></span>
* <span data-ttu-id="e1ba1-739">Используйте выбранный подход максимально согласованно.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-739">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="e1ba1-740">Некоторые атрибуты из этого руководства используются:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-740">Some of the attributes used in the this tutorial are used for:</span></span>

* <span data-ttu-id="e1ba1-741">только для проверки (например, `MinimumLength`);</span><span class="sxs-lookup"><span data-stu-id="e1ba1-741">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="e1ba1-742">только для конфигурации EF Core (например, `HasKey`);</span><span class="sxs-lookup"><span data-stu-id="e1ba1-742">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="e1ba1-743">для проверки и конфигурации EF Core (например, `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-743">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="e1ba1-744">Дополнительные сведения о сравнении атрибутов и текучего API см. в разделе [Методы конфигурации](/ef/core/modeling/).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-744">For more information about attributes vs. fluent API, see [Methods of configuration](/ef/core/modeling/).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="e1ba1-745">Схема сущностей, показывающая связи</span><span class="sxs-lookup"><span data-stu-id="e1ba1-745">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="e1ba1-746">Ниже показана схема, создаваемая средствами EF Power Tools для завершенной модели School.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-746">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Схема сущностей](complex-data-model/_static/diagram.png)

<span data-ttu-id="e1ba1-748">На предыдущей схеме показано следующее:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-748">The preceding diagram shows:</span></span>

* <span data-ttu-id="e1ba1-749">Несколько линий связи один ко многим (1 к \*).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-749">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="e1ba1-750">Линия связи один к нулю или одному (1 к 0..1) между сущностями `Instructor` и `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-750">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="e1ba1-751">Линия связи нуль или один ко многим (0..1 к \*) между сущностями `Instructor` и `Department`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-751">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-db-with-test-data"></a><span data-ttu-id="e1ba1-752">Заполнение базы данных тестовыми данными</span><span class="sxs-lookup"><span data-stu-id="e1ba1-752">Seed the DB with Test Data</span></span>

<span data-ttu-id="e1ba1-753">Обновите код в файле *Data/DbInitializer.cs*:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-753">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="e1ba1-754">Предыдущий код предоставляет начальные данные для новых сущностей.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-754">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="e1ba1-755">Основная часть кода создает объекты сущностей и загружает демонстрационные данные.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-755">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="e1ba1-756">Демонстрационные данные используются для проверки.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-756">The sample data is used for testing.</span></span> <span data-ttu-id="e1ba1-757">См. `Enrollments` и `CourseAssignments`, чтобы ознакомиться с примерами заполнения данными таблиц соединения "многие ко многим".</span><span class="sxs-lookup"><span data-stu-id="e1ba1-757">See `Enrollments` and `CourseAssignments` for examples of how many-to-many join tables can be seeded.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="e1ba1-758">Добавление миграции</span><span class="sxs-lookup"><span data-stu-id="e1ba1-758">Add a migration</span></span>

<span data-ttu-id="e1ba1-759">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-759">Build the project.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1ba1-760">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1ba1-760">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration ComplexDataModel
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e1ba1-761">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-761">Visual Studio Code</span></span>](#tab/visual-studio-code)

```console
dotnet ef migrations add ComplexDataModel
```

---

<span data-ttu-id="e1ba1-762">Предыдущая команда отображает предупреждение о возможной потере данных.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-762">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="e1ba1-763">При выполнении команды `database update` возникает следующая ошибка:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-763">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

## <a name="apply-the-migration"></a><span data-ttu-id="e1ba1-764">Применение миграции</span><span class="sxs-lookup"><span data-stu-id="e1ba1-764">Apply the migration</span></span>

<span data-ttu-id="e1ba1-765">Теперь у вас есть база данных, и пора подумать о том, как применять к ней будущие изменения.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-765">Now that you have an existing database, you need to think about how to apply future changes to it.</span></span> <span data-ttu-id="e1ba1-766">В этом руководстве показано два подхода:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-766">This tutorial shows two approaches:</span></span>

* [<span data-ttu-id="e1ba1-767">Удаление и повторное создание базы данных</span><span class="sxs-lookup"><span data-stu-id="e1ba1-767">Drop and re-create the database</span></span>](#drop)
* <span data-ttu-id="e1ba1-768">[Применение миграции к существующей базе данных](#applyexisting).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-768">[Apply the migration to the existing database](#applyexisting).</span></span> <span data-ttu-id="e1ba1-769">Хотя этот метод является более сложным и трудоемким, в реальной рабочей среде лучше использовать именно его.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-769">While this method is more complex and time-consuming, it's the preferred approach for real-world, production environments.</span></span> <span data-ttu-id="e1ba1-770">**Примечание**. Это необязательный раздел руководства.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-770">**Note**: This is an optional section of the tutorial.</span></span> <span data-ttu-id="e1ba1-771">Вы можете выполнить удаление и повторное создание и пропустить этот раздел.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-771">You can do the drop and re-create steps and skip this section.</span></span> <span data-ttu-id="e1ba1-772">Если вы хотите выполнить инструкции в этом разделе, не выполняйте удаление и повторное создание.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-772">If you do want to follow the steps in this section, don't do the drop and re-create steps.</span></span> 

<a name="drop"></a>

### <a name="drop-and-re-create-the-database"></a><span data-ttu-id="e1ba1-773">Удаление и повторное создание базы данных</span><span class="sxs-lookup"><span data-stu-id="e1ba1-773">Drop and re-create the database</span></span>

<span data-ttu-id="e1ba1-774">Код в обновленном `DbInitializer` предоставляет начальные данные для новых сущностей.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-774">The code in the updated `DbInitializer` adds seed data for the new entities.</span></span> <span data-ttu-id="e1ba1-775">Чтобы выполнить в EF Core принудительное создание новой базы данных, удаление и обновление базы данных:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-775">To force EF Core to create a new  DB, drop and update the DB:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1ba1-776">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1ba1-776">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e1ba1-777">Выполните следующие команды в **консоли диспетчера пакетов** (PMC):</span><span class="sxs-lookup"><span data-stu-id="e1ba1-777">In the **Package Manager Console** (PMC), run the following command:</span></span>

```PMC
Drop-Database
Update-Database
```

<span data-ttu-id="e1ba1-778">Чтобы просмотреть справочную информацию, выполните команду `Get-Help about_EntityFrameworkCore` в PMC.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-778">Run `Get-Help about_EntityFrameworkCore` from the PMC to get help information.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e1ba1-779">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-779">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e1ba1-780">Откройте командное окно и перейдите в папку проекта.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-780">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="e1ba1-781">Папка проекта содержит файл *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-781">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="e1ba1-782">Введите в командном окне следующее:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-782">Enter the following in the command window:</span></span>

 ```console
 dotnet ef database drop
dotnet ef database update
 ```

---

<span data-ttu-id="e1ba1-783">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-783">Run the app.</span></span> <span data-ttu-id="e1ba1-784">При запуске приложения выполняется метод `DbInitializer.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-784">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="e1ba1-785">`DbInitializer.Initialize` заполняет новую базу данных.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-785">The `DbInitializer.Initialize` populates the new DB.</span></span>

<span data-ttu-id="e1ba1-786">Откройте базу данных в SSOX:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-786">Open the DB in SSOX:</span></span>

* <span data-ttu-id="e1ba1-787">Если SSOX был открыт ранее, нажмите кнопку **Обновить**.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-787">If SSOX was opened previously, click the **Refresh** button.</span></span>
* <span data-ttu-id="e1ba1-788">Разверните узел **Таблицы**.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-788">Expand the **Tables** node.</span></span> <span data-ttu-id="e1ba1-789">Отображаются созданные таблицы.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-789">The created tables are displayed.</span></span>

![Таблицы в SSOX](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="e1ba1-791">Изучите таблицу **CourseAssignment**:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-791">Examine the **CourseAssignment** table:</span></span>

* <span data-ttu-id="e1ba1-792">Щелкните правой кнопкой мыши таблицу **CourseAssignment** и выберите пункт **Просмотреть данные**.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-792">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
* <span data-ttu-id="e1ba1-793">Убедитесь, что таблица **CourseAssignment** содержит данные.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-793">Verify the **CourseAssignment** table contains data.</span></span>

![Данные CourseAssignment в SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="applyexisting"></a>

### <a name="apply-the-migration-to-the-existing-database"></a><span data-ttu-id="e1ba1-795">Применение миграции к существующей базе данных</span><span class="sxs-lookup"><span data-stu-id="e1ba1-795">Apply the migration to the existing database</span></span>

<span data-ttu-id="e1ba1-796">Этот раздел является необязательным.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-796">This section is optional.</span></span> <span data-ttu-id="e1ba1-797">Эти действия подходят только в том случае, если вы пропустили предыдущий раздел [Удаление и повторное создание базы данных](#drop).</span><span class="sxs-lookup"><span data-stu-id="e1ba1-797">These steps work only if you skipped the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="e1ba1-798">При выполнении миграций с существующими данными могут действовать ограничения внешнего ключа, которым существующие данные не соответствуют.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-798">When migrations are run with existing data, there may be FK constraints that are not satisfied with the existing data.</span></span> <span data-ttu-id="e1ba1-799">Для рабочих данных нужно предпринять меры по переносу существующих данных.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-799">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="e1ba1-800">Этот раздел содержит пример того, как устранить нарушения ограничений внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-800">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="e1ba1-801">Вносите эти изменения кода только после создания резервной копии.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-801">Don't make these code changes without a backup.</span></span> <span data-ttu-id="e1ba1-802">Не вносите эти изменения кода, если вы выполнили инструкции из предыдущего раздела и обновили базу данных.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-802">Don't make these code changes if you completed the previous section and updated the database.</span></span>

<span data-ttu-id="e1ba1-803">Файл *{метка_времени}_ComplexDataModel.cs* содержит следующий код:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-803">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="e1ba1-804">Предыдущий код добавляет в таблицу `Course` внешний ключ `DepartmentID`, не допускающий значение null.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-804">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="e1ba1-805">База данных из предыдущего руководства содержит строки в `Course`, поэтому эту таблицу невозможно обновить с помощью миграций.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-805">The DB from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="e1ba1-806">Чтобы заставить миграцию `ComplexDataModel` работать с существующими данными:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-806">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="e1ba1-807">Изменить код, чтобы присвоить новому столбцу (`DepartmentID`) значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-807">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="e1ba1-808">Создайте фиктивную кафедру с именем "Temp" для использования по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-808">Create a fake department named "Temp" to act as the default department.</span></span>

#### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="e1ba1-809">Устранение ограничений внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="e1ba1-809">Fix the foreign key constraints</span></span>

<span data-ttu-id="e1ba1-810">Обновите метод `Up` в классах `ComplexDataModel`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-810">Update the `ComplexDataModel` classes `Up` method:</span></span>

* <span data-ttu-id="e1ba1-811">Откройте файл *{метка_времени}_ComplexDataModel.cs*.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-811">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="e1ba1-812">Закомментируйте строку кода, которая добавляет столбец `DepartmentID` в таблицу `Course`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-812">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="e1ba1-813">Добавьте выделенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-813">Add the following highlighted code.</span></span> <span data-ttu-id="e1ba1-814">Новый код идет после блока `.CreateTable( name: "Department"`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-814">The new code goes after the `.CreateTable( name: "Department"` block:</span></span>

 [!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

<span data-ttu-id="e1ba1-815">После внесения описанных выше изменений существующие строки `Course` будут связаны с кафедрой "Temp" после выполнения метода `ComplexDataModel` `Up`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-815">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel` `Up` method runs.</span></span>

<span data-ttu-id="e1ba1-816">Рабочее приложение:</span><span class="sxs-lookup"><span data-stu-id="e1ba1-816">A production app would:</span></span>

* <span data-ttu-id="e1ba1-817">включает код или сценарии, чтобы добавить строки `Department` и связанные строки `Course` к новым строкам `Department`;</span><span class="sxs-lookup"><span data-stu-id="e1ba1-817">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="e1ba1-818">не использует кафедру "Temp" или значение по умолчанию для `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-818">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

<span data-ttu-id="e1ba1-819">Следующее руководство посвящено связанным данным.</span><span class="sxs-lookup"><span data-stu-id="e1ba1-819">The next tutorial covers related data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e1ba1-820">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="e1ba1-820">Additional resources</span></span>

* [<span data-ttu-id="e1ba1-821">Версия руководства на YouTube (часть 1)</span><span class="sxs-lookup"><span data-stu-id="e1ba1-821">YouTube version of this tutorial(Part 1)</span></span>](https://www.youtube.com/watch?v=0n2f0ObgCoA)
* [<span data-ttu-id="e1ba1-822">Версия руководства на YouTube (часть 2)</span><span class="sxs-lookup"><span data-stu-id="e1ba1-822">YouTube version of this tutorial(Part 2)</span></span>](https://www.youtube.com/watch?v=Je0Z5K1TNmY)



> [!div class="step-by-step"]
> <span data-ttu-id="e1ba1-823">[Назад](xref:data/ef-rp/migrations)
> [Вперед](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="e1ba1-823">[Previous](xref:data/ef-rp/migrations)
[Next](xref:data/ef-rp/read-related-data)</span></span>

::: moniker-end