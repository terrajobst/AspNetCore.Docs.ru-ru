---
title: Добавление проверки на страницу Razor в ASP.NET Core
author: rick-anderson
description: Практическое руководство по добавлению проверки на страницу Razor в ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/5/2018
uid: tutorials/razor-pages/validation
ms.openlocfilehash: 38e1fff9c7a212af992951dbf57e124cae69d36f
ms.sourcegitcommit: ccbb84ae307a5bc527441d3d509c20b5c1edde05
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/19/2019
ms.locfileid: "65874991"
---
# <a name="add-validation-to-an-aspnet-core-razor-page"></a><span data-ttu-id="d27dc-103">Добавление проверки на страницу Razor в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d27dc-103">Add validation to an ASP.NET Core Razor Page</span></span>

<span data-ttu-id="d27dc-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="d27dc-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d27dc-105">В этом разделе к модели `Movie` добавляется логика проверки.</span><span class="sxs-lookup"><span data-stu-id="d27dc-105">In this section, validation logic is added to the `Movie` model.</span></span> <span data-ttu-id="d27dc-106">Правила проверки применяются каждый раз, когда пользователь создает или редактирует фильм.</span><span class="sxs-lookup"><span data-stu-id="d27dc-106">The validation rules are enforced any time a user creates or edits a movie.</span></span>

## <a name="validation"></a><span data-ttu-id="d27dc-107">Проверка</span><span class="sxs-lookup"><span data-stu-id="d27dc-107">Validation</span></span>

<span data-ttu-id="d27dc-108">Ключевой принцип разработки программного обеспечения называется [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) (от английского "**D**on't **R**epeat **Y**ourself" — не повторяйся).</span><span class="sxs-lookup"><span data-stu-id="d27dc-108">A key tenet of software development is called [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**on't **R**epeat **Y**ourself").</span></span> <span data-ttu-id="d27dc-109">При разработке страниц Razor Pages рекомендуется задавать любые функциональные возможности лишь один раз и затем при необходимости отражать их в рамках всего приложения.</span><span class="sxs-lookup"><span data-stu-id="d27dc-109">Razor Pages encourages development where functionality is specified once, and it's reflected throughout the app.</span></span> <span data-ttu-id="d27dc-110">Принцип "Не повторяйся" может помочь:</span><span class="sxs-lookup"><span data-stu-id="d27dc-110">DRY can help:</span></span>

* <span data-ttu-id="d27dc-111">сократить объем кода в приложении;</span><span class="sxs-lookup"><span data-stu-id="d27dc-111">Reduce the amount of code in an app.</span></span>
* <span data-ttu-id="d27dc-112">снизить вероятность возникновения ошибки в коде и упростить его тестирование и поддержку.</span><span class="sxs-lookup"><span data-stu-id="d27dc-112">Make the code less error prone, and easier to test and maintain.</span></span>

<span data-ttu-id="d27dc-113">Ярким примером применения принципа "Не повторяйся" является поддержка проверки, реализуемая на страницах Razor и на платформе Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d27dc-113">The validation support provided by Razor Pages and Entity Framework is a good example of the DRY principle.</span></span> <span data-ttu-id="d27dc-114">Правила проверки декларативно определяются в одном месте (в классе модели) и затем применяются в рамках всего приложения.</span><span class="sxs-lookup"><span data-stu-id="d27dc-114">Validation rules are declaratively specified in one place (in the model class), and the rules are enforced everywhere in the app.</span></span>

[!INCLUDE[](~/includes/RP-MVC/validation.md)]

### <a name="validation-error-ui-in-razor-pages"></a><span data-ttu-id="d27dc-115">Пользовательский интерфейс проверки ошибок на страницах Razor</span><span class="sxs-lookup"><span data-stu-id="d27dc-115">Validation Error UI in Razor Pages</span></span>

<span data-ttu-id="d27dc-116">Запустите приложение и перейдите в раздел "Pages/Movies" (Страницы/фильмы).</span><span class="sxs-lookup"><span data-stu-id="d27dc-116">Run the app and navigate to Pages/Movies.</span></span>

<span data-ttu-id="d27dc-117">Щелкните ссылку **Create New** (Создать).</span><span class="sxs-lookup"><span data-stu-id="d27dc-117">Select the **Create New** link.</span></span> <span data-ttu-id="d27dc-118">Введите в форму какие-либо недопустимые значения.</span><span class="sxs-lookup"><span data-stu-id="d27dc-118">Complete the form with some invalid values.</span></span> <span data-ttu-id="d27dc-119">Если функция проверки jQuery на стороне клиента обнаруживает ошибку, сведения о ней отображаются в соответствующем сообщении.</span><span class="sxs-lookup"><span data-stu-id="d27dc-119">When jQuery client-side validation detects the error, it displays an error message.</span></span>

![Форма просмотра фильма с несколькими ошибками проверки jQuery на стороне клиента](validation/_static/val.png)

[!INCLUDE[](~/includes/currency.md)]

<span data-ttu-id="d27dc-121">Обратите внимание, что для каждого поля, содержащего недопустимое значение, в форме автоматически отображается сообщение об ошибке проверки.</span><span class="sxs-lookup"><span data-stu-id="d27dc-121">Notice how the form has automatically rendered a validation error message in each field containing an invalid value.</span></span> <span data-ttu-id="d27dc-122">Эти ошибки применяются как на стороне клиента (с помощью JavaScript и jQuery), так и на стороне сервера (если пользователь отключает JavaScript).</span><span class="sxs-lookup"><span data-stu-id="d27dc-122">The errors are enforced both client-side (using JavaScript and jQuery) and server-side (when a user has JavaScript disabled).</span></span>

<span data-ttu-id="d27dc-123">Основным преимуществом является то, что на страницах создания или редактирования **не требуется** изменять код.</span><span class="sxs-lookup"><span data-stu-id="d27dc-123">A significant benefit is that **no** code changes were necessary in the Create  or Edit pages.</span></span> <span data-ttu-id="d27dc-124">После применения класса DataAnnotations к модели активируется пользовательский интерфейс проверки.</span><span class="sxs-lookup"><span data-stu-id="d27dc-124">Once DataAnnotations were applied to the model, the validation UI was enabled.</span></span> <span data-ttu-id="d27dc-125">На страницах Razor, создаваемых в рамках этого руководства, автоматически применяются правила проверки (для этого к свойствам класса модели `Movie` применяются атрибуты).</span><span class="sxs-lookup"><span data-stu-id="d27dc-125">The Razor Pages created in this tutorial automatically picked up the validation rules (using validation attributes on the properties of the `Movie` model class).</span></span> <span data-ttu-id="d27dc-126">При проверке страницы редактирования применяются те же правила.</span><span class="sxs-lookup"><span data-stu-id="d27dc-126">Test validation using the Edit page, the same validation is applied.</span></span>

<span data-ttu-id="d27dc-127">Данные формы передаются на сервер только после того, как будут устранены все ошибки проверки на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="d27dc-127">The form data isn't posted to the server until there are no client-side validation errors.</span></span> <span data-ttu-id="d27dc-128">Чтобы убедиться, что данные формы не отправляются, используйте любой из следующих способов.</span><span class="sxs-lookup"><span data-stu-id="d27dc-128">Verify form data isn't posted by one or more of the following approaches:</span></span>

* <span data-ttu-id="d27dc-129">Поместите точку останова в метод `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="d27dc-129">Put a break point in the `OnPostAsync` method.</span></span> <span data-ttu-id="d27dc-130">Отправьте форму с помощью команды **Create** (Создать) или **Save** (Сохранить).</span><span class="sxs-lookup"><span data-stu-id="d27dc-130">Submit the form (select **Create** or **Save**).</span></span> <span data-ttu-id="d27dc-131">Точка останова не достигается ни при каких обстоятельствах.</span><span class="sxs-lookup"><span data-stu-id="d27dc-131">The break point is never hit.</span></span>
* <span data-ttu-id="d27dc-132">Используйте [инструмент Fiddler](http://www.telerik.com/fiddler).</span><span class="sxs-lookup"><span data-stu-id="d27dc-132">Use the [Fiddler tool](http://www.telerik.com/fiddler).</span></span>
* <span data-ttu-id="d27dc-133">Проследите сетевой трафик с помощью инструментов разработчика для браузера.</span><span class="sxs-lookup"><span data-stu-id="d27dc-133">Use the browser developer tools to monitor network traffic.</span></span>

### <a name="server-side-validation"></a><span data-ttu-id="d27dc-134">Проверка на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="d27dc-134">Server-side validation</span></span>

<span data-ttu-id="d27dc-135">Если в браузере отключен JavaScript, форма с ошибками отправляется на сервер.</span><span class="sxs-lookup"><span data-stu-id="d27dc-135">When JavaScript is disabled in the browser, submitting the form with errors will post to the server.</span></span>

<span data-ttu-id="d27dc-136">Реализация проверки на стороне сервера:</span><span class="sxs-lookup"><span data-stu-id="d27dc-136">Optional, test server-side validation:</span></span>

* <span data-ttu-id="d27dc-137">Отключите JavaScript в браузере.</span><span class="sxs-lookup"><span data-stu-id="d27dc-137">Disable JavaScript in the browser.</span></span> <span data-ttu-id="d27dc-138">Это можно сделать с помощью средств разработчика в браузере.</span><span class="sxs-lookup"><span data-stu-id="d27dc-138">You can do this using your browser's developer tools.</span></span> <span data-ttu-id="d27dc-139">Если сделать это не удается, попробуйте использовать другой браузер.</span><span class="sxs-lookup"><span data-stu-id="d27dc-139">If you can't disable JavaScript in the browser, try another browser.</span></span>
* <span data-ttu-id="d27dc-140">Поместите точку останова в метод `OnPostAsync` страниц создания или редактирования.</span><span class="sxs-lookup"><span data-stu-id="d27dc-140">Set a break point in the `OnPostAsync` method of the Create or Edit page.</span></span>
* <span data-ttu-id="d27dc-141">Отправьте форму с ошибками проверки.</span><span class="sxs-lookup"><span data-stu-id="d27dc-141">Submit a form with validation errors.</span></span>
* <span data-ttu-id="d27dc-142">Проверка недопустимого состояния модели:</span><span class="sxs-lookup"><span data-stu-id="d27dc-142">Verify the model state is invalid:</span></span>

  ```csharp
   if (!ModelState.IsValid)
   {
      return Page();
   }
  ```

<span data-ttu-id="d27dc-143">В следующем коде демонстрируется часть страницы *Create.cshtml*, сформированной ранее в рамках этого руководства.</span><span class="sxs-lookup"><span data-stu-id="d27dc-143">The following code shows a portion of the *Create.cshtml* page that you scaffolded earlier in the tutorial.</span></span> <span data-ttu-id="d27dc-144">Она используется на страницах создания и редактирования для отображения исходной формы и повторного вывода формы в случае ошибки.</span><span class="sxs-lookup"><span data-stu-id="d27dc-144">It's used by the Create and Edit pages to display the initial form and to redisplay the form in the event of an error.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]

<span data-ttu-id="d27dc-145">[Вспомогательная функция тега Input](xref:mvc/views/working-with-forms) использует атрибуты [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) и создает HTML-атрибуты, необходимые для проверки jQuery на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="d27dc-145">The [Input Tag Helper](xref:mvc/views/working-with-forms) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span> <span data-ttu-id="d27dc-146">[Вспомогательная функция тега Validation](xref:mvc/views/working-with-forms#the-validation-tag-helpers) отображает ошибки проверки.</span><span class="sxs-lookup"><span data-stu-id="d27dc-146">The [Validation Tag Helper](xref:mvc/views/working-with-forms#the-validation-tag-helpers) displays validation errors.</span></span> <span data-ttu-id="d27dc-147">Дополнительные сведения см. в разделе [Проверка](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="d27dc-147">See [Validation](xref:mvc/models/validation) for more information.</span></span>

<span data-ttu-id="d27dc-148">На страницах создания и редактирования не определены правила проверки.</span><span class="sxs-lookup"><span data-stu-id="d27dc-148">The Create and Edit pages have no validation rules in them.</span></span> <span data-ttu-id="d27dc-149">Правила проверки и строки ошибок указываются только в классе `Movie`.</span><span class="sxs-lookup"><span data-stu-id="d27dc-149">The validation rules and the error strings are specified only in the `Movie` class.</span></span> <span data-ttu-id="d27dc-150">Они автоматически применяются к страницам Razor, которые редактируют модель `Movie`.</span><span class="sxs-lookup"><span data-stu-id="d27dc-150">These validation rules are automatically applied to Razor Pages that edit the `Movie` model.</span></span>

<span data-ttu-id="d27dc-151">Любые необходимые изменения логики проверки осуществляются исключительно в модели.</span><span class="sxs-lookup"><span data-stu-id="d27dc-151">When validation logic needs to change, it's done only in the model.</span></span> <span data-ttu-id="d27dc-152">Проверка применяется согласованно на уровне всего приложения, для чего логика проверки определяется в одном месте.</span><span class="sxs-lookup"><span data-stu-id="d27dc-152">Validation is applied consistently throughout the application (validation logic is defined in one place).</span></span> <span data-ttu-id="d27dc-153">Такой подход позволяет максимально оптимизировать код и упростить его поддержку и обновление.</span><span class="sxs-lookup"><span data-stu-id="d27dc-153">Validation in one place helps keep the code clean, and makes it easier to maintain and update.</span></span>

## <a name="using-datatype-attributes"></a><span data-ttu-id="d27dc-154">Использование атрибутов DataType</span><span class="sxs-lookup"><span data-stu-id="d27dc-154">Using DataType Attributes</span></span>

<span data-ttu-id="d27dc-155">Проверьте класс `Movie`.</span><span class="sxs-lookup"><span data-stu-id="d27dc-155">Examine the `Movie` class.</span></span> <span data-ttu-id="d27dc-156">В пространстве имен `System.ComponentModel.DataAnnotations` в дополнение к набору встроенных атрибутов проверки предоставляются атрибуты форматирования.</span><span class="sxs-lookup"><span data-stu-id="d27dc-156">The `System.ComponentModel.DataAnnotations` namespace provides formatting attributes in addition to the built-in set of validation attributes.</span></span> <span data-ttu-id="d27dc-157">Атрибут `DataType` применяется к свойствам `ReleaseDate` и `Price`.</span><span class="sxs-lookup"><span data-stu-id="d27dc-157">The `DataType` attribute is applied to the `ReleaseDate` and `Price` properties.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

<span data-ttu-id="d27dc-158">Атрибуты `DataType` предоставляют модулю просмотра только рекомендации по форматированию данных, а также другие атрибуты, например `<a>` для URL-адресов и `<a href="mailto:EmailAddress.com">` для электронной почты.</span><span class="sxs-lookup"><span data-stu-id="d27dc-158">The `DataType` attributes only provide hints for the view engine to format the data (and supplies attributes such as `<a>` for URL's and `<a href="mailto:EmailAddress.com">` for email).</span></span> <span data-ttu-id="d27dc-159">Используйте атрибут `RegularExpression` для проверки формата данных.</span><span class="sxs-lookup"><span data-stu-id="d27dc-159">Use the `RegularExpression` attribute to validate the format of the data.</span></span> <span data-ttu-id="d27dc-160">Атрибут `DataType` позволяет указать тип данных с более точным определением по сравнению со встроенным типом базы данных.</span><span class="sxs-lookup"><span data-stu-id="d27dc-160">The `DataType` attribute is used to specify a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="d27dc-161">Атрибуты `DataType` не предназначены для проверки.</span><span class="sxs-lookup"><span data-stu-id="d27dc-161">`DataType` attributes are not validation attributes.</span></span> <span data-ttu-id="d27dc-162">В том же приложении отображается только дата (без времени).</span><span class="sxs-lookup"><span data-stu-id="d27dc-162">In the sample application, only the date is displayed, without time.</span></span>

<span data-ttu-id="d27dc-163">В перечислении `DataType` представлено множество типов данных, таких как Date, Time, PhoneNumber, Currency, EmailAddress и других.</span><span class="sxs-lookup"><span data-stu-id="d27dc-163">The `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="d27dc-164">Атрибут `DataType` также обеспечивает автоматическое предоставление функций для определенных типов в приложении.</span><span class="sxs-lookup"><span data-stu-id="d27dc-164">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="d27dc-165">Например, можно создавать ссылку `mailto:` для `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="d27dc-165">For example, a `mailto:` link can be created for `DataType.EmailAddress`.</span></span> <span data-ttu-id="d27dc-166">Для `DataType.Date` в браузерах с поддержкой HTML5 можно предоставить селектор даты.</span><span class="sxs-lookup"><span data-stu-id="d27dc-166">A date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="d27dc-167">Атрибут `DataType` создает атрибуты HTML 5 `data-`, которые используются браузерами с поддержкой HTML 5.</span><span class="sxs-lookup"><span data-stu-id="d27dc-167">The `DataType` attributes emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="d27dc-168">Атрибуты `DataType` **не предназначены** для проверки.</span><span class="sxs-lookup"><span data-stu-id="d27dc-168">The `DataType` attributes do **not** provide any validation.</span></span>

<span data-ttu-id="d27dc-169">`DataType.Date` не задает формат отображаемой даты.</span><span class="sxs-lookup"><span data-stu-id="d27dc-169">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="d27dc-170">По умолчанию поле данных отображается с использованием форматов, установленных в параметрах `CultureInfo` сервера.</span><span class="sxs-lookup"><span data-stu-id="d27dc-170">By default, the data field is displayed according to the default formats based on the server's `CultureInfo`.</span></span>

<span data-ttu-id="d27dc-171">Требуются заметки к данным `[Column(TypeName = "decimal(18, 2)")]`, чтобы Entity Framework Core корректно сопоставила `Price` с валютой в базе данных.</span><span class="sxs-lookup"><span data-stu-id="d27dc-171">The `[Column(TypeName = "decimal(18, 2)")]` data annotation is required so Entity Framework Core can correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="d27dc-172">Дополнительные сведения см. в разделе [Типы данных](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="d27dc-172">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

<span data-ttu-id="d27dc-173">С помощью атрибута `DisplayFormat` можно явно указать формат даты:</span><span class="sxs-lookup"><span data-stu-id="d27dc-173">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

<span data-ttu-id="d27dc-174">Параметр `ApplyFormatInEditMode` указывает, что формат должен применяться при отображении значения для редактирования.</span><span class="sxs-lookup"><span data-stu-id="d27dc-174">The `ApplyFormatInEditMode` setting specifies that the formatting should be applied when the value is displayed for editing.</span></span> <span data-ttu-id="d27dc-175">Для некоторых полей такое поведение нежелательно.</span><span class="sxs-lookup"><span data-stu-id="d27dc-175">You might not want that behavior for some fields.</span></span> <span data-ttu-id="d27dc-176">Например, в полях валюты в пользовательском интерфейсе редактирования использовать символ денежной единицы, как правило, не требуется.</span><span class="sxs-lookup"><span data-stu-id="d27dc-176">For example, in currency values, you probably don't want the currency symbol in the edit UI.</span></span>

<span data-ttu-id="d27dc-177">Атрибут `DisplayFormat` может использоваться отдельно, однако чаще всего его рекомендуется применять вместе с атрибутом `DataType`.</span><span class="sxs-lookup"><span data-stu-id="d27dc-177">The `DisplayFormat` attribute can be used by itself, but it's generally a good idea to use the `DataType` attribute.</span></span> <span data-ttu-id="d27dc-178">Атрибут `DataType` передает семантику данных (в отличие от способа их вывода на экран) и дает следующие преимущества по сравнению с атрибутом DisplayFormat:</span><span class="sxs-lookup"><span data-stu-id="d27dc-178">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with DisplayFormat:</span></span>

* <span data-ttu-id="d27dc-179">Поддержка функций HTML5 в браузере (отображение элемента управления календарем, соответствующего языковому стандарту символа валюты, ссылок электронной почты и т. д.)</span><span class="sxs-lookup"><span data-stu-id="d27dc-179">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, etc.)</span></span>
* <span data-ttu-id="d27dc-180">По умолчанию формат отображения данных в браузере определяется в соответствии с установленным языковым стандартом.</span><span class="sxs-lookup"><span data-stu-id="d27dc-180">By default, the browser will render data using the correct format based on your locale.</span></span>
* <span data-ttu-id="d27dc-181">Атрибут `DataType` обеспечивает поддержку платформы ASP.NET Core для выбора соответствующего шаблона поля, применяемого при отображении данных.</span><span class="sxs-lookup"><span data-stu-id="d27dc-181">The `DataType` attribute can enable the ASP.NET Core framework to choose the right field template to render the data.</span></span> <span data-ttu-id="d27dc-182">При отдельном использовании атрибут `DisplayFormat` базируется на строковом шаблоне.</span><span class="sxs-lookup"><span data-stu-id="d27dc-182">The `DisplayFormat` if used by itself uses the string template.</span></span>

<span data-ttu-id="d27dc-183">Примечание. Проверка jQuery не работает с атрибутом `Range` и `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="d27dc-183">Note: jQuery validation doesn't work with the `Range` attribute and `DateTime`.</span></span> <span data-ttu-id="d27dc-184">Например, следующий код всегда приводит к возникновению ошибки проверки на стороне клиента, даже если дата попадает в указанный диапазон:</span><span class="sxs-lookup"><span data-stu-id="d27dc-184">For example, the following code will always display a client-side validation error, even when the date is in the specified range:</span></span>

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

<span data-ttu-id="d27dc-185">Как правило, не рекомендуется компилировать модели с фиксированными датами, поэтому использовать атрибуты `Range` и `DateTime` следует крайне осторожно.</span><span class="sxs-lookup"><span data-stu-id="d27dc-185">It's generally not a good practice to compile hard dates in your models, so using the `Range` attribute and `DateTime` is discouraged.</span></span>

<span data-ttu-id="d27dc-186">В следующем коде демонстрируется объединение атрибутов в одной строке:</span><span class="sxs-lookup"><span data-stu-id="d27dc-186">The following code shows combining attributes on one line:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateRatingDAmult.cs?name=snippet1)]

<span data-ttu-id="d27dc-187">Дополнительные операции EF Core с Razor Pages см. в статье [Начало работы с Razor Pages и EF Core](xref:data/ef-rp/intro).</span><span class="sxs-lookup"><span data-stu-id="d27dc-187">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) shows advanced EF Core operations with Razor Pages.</span></span>

### <a name="publish-to-azure"></a><span data-ttu-id="d27dc-188">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="d27dc-188">Publish to Azure</span></span>

<span data-ttu-id="d27dc-189">Сведения о развертывании в Azure, см. в разделе [Учебник: Создание приложения ASP.NET в Azure с подключением к базе данных SQL](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase).</span><span class="sxs-lookup"><span data-stu-id="d27dc-189">For information on deploying to Azure, see [Tutorial: Build an ASP.NET app in Azure with SQL Database](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase).</span></span> <span data-ttu-id="d27dc-190">Эти инструкции приведены для приложения ASP.NET, а не ASP.NET Core, но шаги совпадают.</span><span class="sxs-lookup"><span data-stu-id="d27dc-190">These instructions are for an ASP.NET app, not an ASP.NET Core app, but the steps are the same.</span></span>

<span data-ttu-id="d27dc-191">Благодарим вас за изучение общих сведений о страницах Razor.</span><span class="sxs-lookup"><span data-stu-id="d27dc-191">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="d27dc-192">Отличным дополнением к этому руководству является руководство по [началу работы с Razor Pages и EF Core](xref:data/ef-rp/intro).</span><span class="sxs-lookup"><span data-stu-id="d27dc-192">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d27dc-193">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="d27dc-193">Additional resources</span></span>

* <xref:mvc/views/working-with-forms>
* <xref:fundamentals/localization>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/authoring>
* [<span data-ttu-id="d27dc-194">Версия руководства на YouTube</span><span class="sxs-lookup"><span data-stu-id="d27dc-194">YouTube version of this tutorial</span></span>](https://youtu.be/b63m66eu7us)

> [!div class="step-by-step"]
> [<span data-ttu-id="d27dc-195">Предыдущая статья. Добавление нового поля</span><span class="sxs-lookup"><span data-stu-id="d27dc-195">Previous: Adding a new field</span></span>](xref:tutorials/razor-pages/new-field)
