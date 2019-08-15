---
title: Добавление проверки на страницу Razor в ASP.NET Core
author: rick-anderson
description: Практическое руководство по добавлению проверки на страницу Razor в ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 7/23/2019
uid: tutorials/razor-pages/validation
ms.openlocfilehash: 5c5419eb6ccfbd9ddd8d6fadb24d688966d76c10
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022408"
---
# <a name="add-validation-to-an-aspnet-core-razor-page"></a><span data-ttu-id="1f9cc-103">Добавление проверки на страницу Razor в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1f9cc-103">Add validation to an ASP.NET Core Razor Page</span></span>

<span data-ttu-id="1f9cc-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="1f9cc-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1f9cc-105">В этом разделе к модели `Movie` добавляется логика проверки.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-105">In this section, validation logic is added to the `Movie` model.</span></span> <span data-ttu-id="1f9cc-106">Правила проверки применяются каждый раз, когда пользователь создает или редактирует фильм.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-106">The validation rules are enforced any time a user creates or edits a movie.</span></span>

## <a name="validation"></a><span data-ttu-id="1f9cc-107">Проверка</span><span class="sxs-lookup"><span data-stu-id="1f9cc-107">Validation</span></span>

<span data-ttu-id="1f9cc-108">Ключевой принцип разработки программного обеспечения называется [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) (от английского "**D**on't **R**epeat **Y**ourself" — не повторяйся).</span><span class="sxs-lookup"><span data-stu-id="1f9cc-108">A key tenet of software development is called [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**on't **R**epeat **Y**ourself").</span></span> <span data-ttu-id="1f9cc-109">При разработке страниц Razor Pages рекомендуется задавать любые функциональные возможности лишь один раз и затем при необходимости отражать их в рамках всего приложения.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-109">Razor Pages encourages development where functionality is specified once, and it's reflected throughout the app.</span></span> <span data-ttu-id="1f9cc-110">Принцип "Не повторяйся" может помочь:</span><span class="sxs-lookup"><span data-stu-id="1f9cc-110">DRY can help:</span></span>

* <span data-ttu-id="1f9cc-111">сократить объем кода в приложении;</span><span class="sxs-lookup"><span data-stu-id="1f9cc-111">Reduce the amount of code in an app.</span></span>
* <span data-ttu-id="1f9cc-112">снизить вероятность возникновения ошибки в коде и упростить его тестирование и поддержку.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-112">Make the code less error prone, and easier to test and maintain.</span></span>

<span data-ttu-id="1f9cc-113">Ярким примером применения принципа "Не повторяйся" является поддержка проверки, реализуемая на страницах Razor и на платформе Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-113">The validation support provided by Razor Pages and Entity Framework is a good example of the DRY principle.</span></span> <span data-ttu-id="1f9cc-114">Правила проверки декларативно определяются в одном месте (в классе модели) и затем применяются в рамках всего приложения.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-114">Validation rules are declaratively specified in one place (in the model class), and the rules are enforced everywhere in the app.</span></span>

## <a name="add-validation-rules-to-the-movie-model"></a><span data-ttu-id="1f9cc-115">Добавление правил проверки к модели фильма</span><span class="sxs-lookup"><span data-stu-id="1f9cc-115">Add validation rules to the movie model</span></span>

<span data-ttu-id="1f9cc-116">Пространство имен DataAnnotations предоставляет набор встроенных атрибутов проверки, которые декларативно применяются к классу или свойству.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-116">The DataAnnotations namespace provides a set of built-in validation attributes that are applied declaratively to a class or property.</span></span> <span data-ttu-id="1f9cc-117">Кроме того, DataAnnotations содержит атрибуты форматирования (такие как `DataType`), которые обеспечивают форматирование и не предназначены для проверки.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-117">DataAnnotations also contains formatting attributes like `DataType` that help with formatting and don't provide any validation.</span></span>

<span data-ttu-id="1f9cc-118">Обновите класс `Movie`, чтобы использовать преимущества встроенных атрибутов проверки `Required`, `StringLength`, `RegularExpression` и `Range`.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-118">Update the `Movie` class to take advantage of the built-in `Required`, `StringLength`, `RegularExpression`, and `Range` validation attributes.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRatingDA.cs?name=snippet1)]

<span data-ttu-id="1f9cc-119">Атрибуты проверки определяют поведение для свойств модели, к которым они применяются:</span><span class="sxs-lookup"><span data-stu-id="1f9cc-119">The validation attributes specify behavior that you want to enforce on the model properties they're applied to:</span></span>

* <span data-ttu-id="1f9cc-120">Атрибуты `Required` и `MinimumLength` указывают, что свойство должно иметь значение. Тем не менее, чтобы удовлетворить требованиям проверки, пользователю достаточно ввести пробел.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-120">The `Required` and `MinimumLength` attributes indicate that a property must have a value; but nothing prevents a user from entering white space to satisfy this validation.</span></span>
* <span data-ttu-id="1f9cc-121">Атрибут `RegularExpression` ограничивает набор допустимых для ввода символов.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-121">The `RegularExpression` attribute is used to limit what characters can be input.</span></span> <span data-ttu-id="1f9cc-122">В приведенном выше коде в Genre:</span><span class="sxs-lookup"><span data-stu-id="1f9cc-122">In the preceding code, "Genre":</span></span>

  * <span data-ttu-id="1f9cc-123">должны использоваться только буквы;</span><span class="sxs-lookup"><span data-stu-id="1f9cc-123">Must only use letters.</span></span>
  * <span data-ttu-id="1f9cc-124">первая буква должна быть прописной;</span><span class="sxs-lookup"><span data-stu-id="1f9cc-124">The first letter is required to be uppercase.</span></span> <span data-ttu-id="1f9cc-125">пробелы, цифры и специальные символы не допускаются.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-125">White space, numbers, and special characters are not allowed.</span></span>

* <span data-ttu-id="1f9cc-126">В `RegularExpression` Rating:</span><span class="sxs-lookup"><span data-stu-id="1f9cc-126">The `RegularExpression` "Rating":</span></span>

  * <span data-ttu-id="1f9cc-127">первый символ должен быть прописной буквой;</span><span class="sxs-lookup"><span data-stu-id="1f9cc-127">Requires that the first character be an uppercase letter.</span></span>
  * <span data-ttu-id="1f9cc-128">допускаются специальные символы и цифры, а также последующие пробелы.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-128">Allows special characters and numbers in  subsequent spaces.</span></span> <span data-ttu-id="1f9cc-129">Значение "PG-13" допустимо для рейтинга, но недопустимо для жанра.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-129">"PG-13" is valid for a rating, but fails for a "Genre".</span></span>

* <span data-ttu-id="1f9cc-130">Атрибут `Range` ограничивает диапазон значений.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-130">The `Range` attribute constrains a value to within a specified range.</span></span>
* <span data-ttu-id="1f9cc-131">Атрибут `StringLength` позволяет задать максимальную и при необходимости минимальную длину строкового свойства.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-131">The `StringLength` attribute lets you set the maximum length of a string property, and optionally its minimum length.</span></span>
* <span data-ttu-id="1f9cc-132">Типы значений (например, `decimal`, `int`, `float`, `DateTime`) по своей природе являются обязательными и не требуют атрибута `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-132">Value types (such as `decimal`, `int`, `float`, `DateTime`) are inherently required and don't need the `[Required]` attribute.</span></span>

<span data-ttu-id="1f9cc-133">Наличие правил проверки, которые автоматически применяются ASP.NET Core, помогает повысить степень надежности приложения.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-133">Having validation rules automatically enforced by ASP.NET Core helps make your app more robust.</span></span> <span data-ttu-id="1f9cc-134">Это также гарантирует, что в любом случае будут выполнены все проверки и в базе данных не будут случайно оставлены поврежденные данные.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-134">It also ensures that you can't forget to validate something and inadvertently let bad data into the database.</span></span>

### <a name="validation-error-ui-in-razor-pages"></a><span data-ttu-id="1f9cc-135">Пользовательский интерфейс проверки ошибок на страницах Razor</span><span class="sxs-lookup"><span data-stu-id="1f9cc-135">Validation Error UI in Razor Pages</span></span>

<span data-ttu-id="1f9cc-136">Запустите приложение и перейдите в раздел "Pages/Movies" (Страницы/фильмы).</span><span class="sxs-lookup"><span data-stu-id="1f9cc-136">Run the app and navigate to Pages/Movies.</span></span>

<span data-ttu-id="1f9cc-137">Щелкните ссылку **Create New** (Создать).</span><span class="sxs-lookup"><span data-stu-id="1f9cc-137">Select the **Create New** link.</span></span> <span data-ttu-id="1f9cc-138">Введите в форму какие-либо недопустимые значения.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-138">Complete the form with some invalid values.</span></span> <span data-ttu-id="1f9cc-139">Если функция проверки jQuery на стороне клиента обнаруживает ошибку, сведения о ней отображаются в соответствующем сообщении.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-139">When jQuery client-side validation detects the error, it displays an error message.</span></span>

![Форма просмотра фильма с несколькими ошибками проверки jQuery на стороне клиента](validation/_static/val.png)

[!INCLUDE[](~/includes/currency.md)]

<span data-ttu-id="1f9cc-141">Обратите внимание, что для каждого поля, содержащего недопустимое значение, в форме автоматически отображается сообщение об ошибке проверки.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-141">Notice how the form has automatically rendered a validation error message in each field containing an invalid value.</span></span> <span data-ttu-id="1f9cc-142">Эти ошибки применяются как на стороне клиента (с помощью JavaScript и jQuery), так и на стороне сервера (если пользователь отключает JavaScript).</span><span class="sxs-lookup"><span data-stu-id="1f9cc-142">The errors are enforced both client-side (using JavaScript and jQuery) and server-side (when a user has JavaScript disabled).</span></span>

<span data-ttu-id="1f9cc-143">Основным преимуществом является то, что на страницах создания или редактирования **не требуется** изменять код.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-143">A significant benefit is that **no** code changes were necessary in the Create  or Edit pages.</span></span> <span data-ttu-id="1f9cc-144">После применения класса DataAnnotations к модели активируется пользовательский интерфейс проверки.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-144">Once DataAnnotations were applied to the model, the validation UI was enabled.</span></span> <span data-ttu-id="1f9cc-145">На страницах Razor, создаваемых в рамках этого руководства, автоматически применяются правила проверки (для этого к свойствам класса модели `Movie` применяются атрибуты).</span><span class="sxs-lookup"><span data-stu-id="1f9cc-145">The Razor Pages created in this tutorial automatically picked up the validation rules (using validation attributes on the properties of the `Movie` model class).</span></span> <span data-ttu-id="1f9cc-146">При проверке страницы редактирования применяются те же правила.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-146">Test validation using the Edit page, the same validation is applied.</span></span>

<span data-ttu-id="1f9cc-147">Данные формы передаются на сервер только после того, как будут устранены все ошибки проверки на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-147">The form data isn't posted to the server until there are no client-side validation errors.</span></span> <span data-ttu-id="1f9cc-148">Чтобы убедиться, что данные формы не отправляются, используйте любой из следующих способов.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-148">Verify form data isn't posted by one or more of the following approaches:</span></span>

* <span data-ttu-id="1f9cc-149">Поместите точку останова в метод `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-149">Put a break point in the `OnPostAsync` method.</span></span> <span data-ttu-id="1f9cc-150">Отправьте форму с помощью команды **Create** (Создать) или **Save** (Сохранить).</span><span class="sxs-lookup"><span data-stu-id="1f9cc-150">Submit the form (select **Create** or **Save**).</span></span> <span data-ttu-id="1f9cc-151">Точка останова не достигается ни при каких обстоятельствах.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-151">The break point is never hit.</span></span>
* <span data-ttu-id="1f9cc-152">Используйте [инструмент Fiddler](https://www.telerik.com/fiddler).</span><span class="sxs-lookup"><span data-stu-id="1f9cc-152">Use the [Fiddler tool](https://www.telerik.com/fiddler).</span></span>
* <span data-ttu-id="1f9cc-153">Проследите сетевой трафик с помощью инструментов разработчика для браузера.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-153">Use the browser developer tools to monitor network traffic.</span></span>

### <a name="server-side-validation"></a><span data-ttu-id="1f9cc-154">Проверка на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="1f9cc-154">Server-side validation</span></span>

<span data-ttu-id="1f9cc-155">Если в браузере отключен JavaScript, форма с ошибками отправляется на сервер.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-155">When JavaScript is disabled in the browser, submitting the form with errors will post to the server.</span></span>

<span data-ttu-id="1f9cc-156">Реализация проверки на стороне сервера:</span><span class="sxs-lookup"><span data-stu-id="1f9cc-156">Optional, test server-side validation:</span></span>

* <span data-ttu-id="1f9cc-157">Отключите JavaScript в браузере.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-157">Disable JavaScript in the browser.</span></span> <span data-ttu-id="1f9cc-158">JavaScript можно отключить с помощью средств разработчика в браузере.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-158">You can disable JavaScript using browser's developer tools.</span></span> <span data-ttu-id="1f9cc-159">Если сделать это не удается, попробуйте использовать другой браузер.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-159">If you can't disable JavaScript in the browser, try another browser.</span></span>
* <span data-ttu-id="1f9cc-160">Поместите точку останова в метод `OnPostAsync` страниц создания или редактирования.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-160">Set a break point in the `OnPostAsync` method of the Create or Edit page.</span></span>
* <span data-ttu-id="1f9cc-161">Отправьте форму с недопустимыми данными.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-161">Submit a form with invalid data.</span></span>
* <span data-ttu-id="1f9cc-162">Проверка недопустимого состояния модели:</span><span class="sxs-lookup"><span data-stu-id="1f9cc-162">Verify the model state is invalid:</span></span>

  ```csharp
   if (!ModelState.IsValid)
   {
      return Page();
   }
  ```

<span data-ttu-id="1f9cc-163">В следующем коде демонстрируется часть страницы *Create.cshtml*, созданной ранее в рамках этого руководства.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-163">The following code shows a portion of the *Create.cshtml* page scaffolded earlier in the tutorial.</span></span> <span data-ttu-id="1f9cc-164">Она используется на страницах создания и редактирования для отображения исходной формы и повторного вывода формы в случае ошибки.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-164">It's used by the Create and Edit pages to display the initial form and to redisplay the form in the event of an error.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]

<span data-ttu-id="1f9cc-165">[Вспомогательная функция тега Input](xref:mvc/views/working-with-forms) использует атрибуты [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) и создает HTML-атрибуты, необходимые для проверки jQuery на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-165">The [Input Tag Helper](xref:mvc/views/working-with-forms) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span> <span data-ttu-id="1f9cc-166">[Вспомогательная функция тега Validation](xref:mvc/views/working-with-forms#the-validation-tag-helpers) отображает ошибки проверки.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-166">The [Validation Tag Helper](xref:mvc/views/working-with-forms#the-validation-tag-helpers) displays validation errors.</span></span> <span data-ttu-id="1f9cc-167">Дополнительные сведения см. в разделе [Проверка](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="1f9cc-167">See [Validation](xref:mvc/models/validation) for more information.</span></span>

<span data-ttu-id="1f9cc-168">На страницах создания и редактирования не определены правила проверки.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-168">The Create and Edit pages have no validation rules in them.</span></span> <span data-ttu-id="1f9cc-169">Правила проверки и строки ошибок указываются только в классе `Movie`.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-169">The validation rules and the error strings are specified only in the `Movie` class.</span></span> <span data-ttu-id="1f9cc-170">Они автоматически применяются к страницам Razor, которые редактируют модель `Movie`.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-170">These validation rules are automatically applied to Razor Pages that edit the `Movie` model.</span></span>

<span data-ttu-id="1f9cc-171">Любые необходимые изменения логики проверки осуществляются исключительно в модели.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-171">When validation logic needs to change, it's done only in the model.</span></span> <span data-ttu-id="1f9cc-172">Проверка применяется согласованно на уровне всего приложения, для чего логика проверки определяется в одном месте.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-172">Validation is applied consistently throughout the application (validation logic is defined in one place).</span></span> <span data-ttu-id="1f9cc-173">Такой подход позволяет максимально оптимизировать код и упростить его поддержку и обновление.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-173">Validation in one place helps keep the code clean, and makes it easier to maintain and update.</span></span>

## <a name="using-datatype-attributes"></a><span data-ttu-id="1f9cc-174">Использование атрибутов DataType</span><span class="sxs-lookup"><span data-stu-id="1f9cc-174">Using DataType Attributes</span></span>

<span data-ttu-id="1f9cc-175">Проверьте класс `Movie`.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-175">Examine the `Movie` class.</span></span> <span data-ttu-id="1f9cc-176">В пространстве имен `System.ComponentModel.DataAnnotations` в дополнение к набору встроенных атрибутов проверки предоставляются атрибуты форматирования.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-176">The `System.ComponentModel.DataAnnotations` namespace provides formatting attributes in addition to the built-in set of validation attributes.</span></span> <span data-ttu-id="1f9cc-177">Атрибут `DataType` применяется к свойствам `ReleaseDate` и `Price`.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-177">The `DataType` attribute is applied to the `ReleaseDate` and `Price` properties.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

<span data-ttu-id="1f9cc-178">Атрибуты `DataType` предоставляют модулю просмотра только рекомендации по форматированию данных, а также другие атрибуты, например `<a>` для URL-адресов и `<a href="mailto:EmailAddress.com">` для электронной почты.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-178">The `DataType` attributes only provide hints for the view engine to format the data (and supplies attributes such as `<a>` for URL's and `<a href="mailto:EmailAddress.com">` for email).</span></span> <span data-ttu-id="1f9cc-179">Используйте атрибут `RegularExpression` для проверки формата данных.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-179">Use the `RegularExpression` attribute to validate the format of the data.</span></span> <span data-ttu-id="1f9cc-180">Атрибут `DataType` позволяет указать тип данных с более точным определением по сравнению со встроенным типом базы данных.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-180">The `DataType` attribute is used to specify a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="1f9cc-181">Атрибуты `DataType` не предназначены для проверки.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-181">`DataType` attributes are not validation attributes.</span></span> <span data-ttu-id="1f9cc-182">В том же приложении отображается только дата (без времени).</span><span class="sxs-lookup"><span data-stu-id="1f9cc-182">In the sample application, only the date is displayed, without time.</span></span>

<span data-ttu-id="1f9cc-183">В перечислении `DataType` представлено множество типов данных, таких как Date, Time, PhoneNumber, Currency, EmailAddress и других.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-183">The `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="1f9cc-184">Атрибут `DataType` также обеспечивает автоматическое предоставление функций для определенных типов в приложении.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-184">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="1f9cc-185">Например, можно создавать ссылку `mailto:` для `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-185">For example, a `mailto:` link can be created for `DataType.EmailAddress`.</span></span> <span data-ttu-id="1f9cc-186">Для `DataType.Date` в браузерах с поддержкой HTML5 можно предоставить селектор даты.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-186">A date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="1f9cc-187">Атрибут `DataType` создает атрибуты HTML 5 `data-`, которые используются браузерами с поддержкой HTML 5.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-187">The `DataType` attributes emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="1f9cc-188">Атрибуты `DataType` **не предназначены** для проверки.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-188">The `DataType` attributes do **not** provide any validation.</span></span>

<span data-ttu-id="1f9cc-189">`DataType.Date` не задает формат отображаемой даты.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-189">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="1f9cc-190">По умолчанию поле данных отображается с использованием форматов, установленных в параметрах `CultureInfo` сервера.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-190">By default, the data field is displayed according to the default formats based on the server's `CultureInfo`.</span></span>

<span data-ttu-id="1f9cc-191">Требуются заметки к данным `[Column(TypeName = "decimal(18, 2)")]`, чтобы Entity Framework Core корректно сопоставила `Price` с валютой в базе данных.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-191">The `[Column(TypeName = "decimal(18, 2)")]` data annotation is required so Entity Framework Core can correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="1f9cc-192">Дополнительные сведения см. в разделе [Типы данных](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="1f9cc-192">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

<span data-ttu-id="1f9cc-193">С помощью атрибута `DisplayFormat` можно явно указать формат даты:</span><span class="sxs-lookup"><span data-stu-id="1f9cc-193">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

<span data-ttu-id="1f9cc-194">Параметр `ApplyFormatInEditMode` указывает, что формат должен применяться при отображении значения для редактирования.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-194">The `ApplyFormatInEditMode` setting specifies that the formatting should be applied when the value is displayed for editing.</span></span> <span data-ttu-id="1f9cc-195">Для некоторых полей такое поведение нежелательно.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-195">You might not want that behavior for some fields.</span></span> <span data-ttu-id="1f9cc-196">Например, в полях валюты в пользовательском интерфейсе редактирования использовать символ денежной единицы, как правило, не требуется.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-196">For example, in currency values, you probably don't want the currency symbol in the edit UI.</span></span>

<span data-ttu-id="1f9cc-197">Атрибут `DisplayFormat` может использоваться отдельно, однако чаще всего его рекомендуется применять вместе с атрибутом `DataType`.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-197">The `DisplayFormat` attribute can be used by itself, but it's generally a good idea to use the `DataType` attribute.</span></span> <span data-ttu-id="1f9cc-198">Атрибут `DataType` передает семантику данных (в отличие от способа их вывода на экран) и дает следующие преимущества по сравнению с атрибутом DisplayFormat:</span><span class="sxs-lookup"><span data-stu-id="1f9cc-198">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with DisplayFormat:</span></span>

* <span data-ttu-id="1f9cc-199">Поддержка функций HTML5 в браузере (отображение элемента управления календарем, соответствующего языковому стандарту символа валюты, ссылок электронной почты и т. д.)</span><span class="sxs-lookup"><span data-stu-id="1f9cc-199">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, etc.)</span></span>
* <span data-ttu-id="1f9cc-200">По умолчанию формат отображения данных в браузере определяется в соответствии с установленным языковым стандартом.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-200">By default, the browser will render data using the correct format based on your locale.</span></span>
* <span data-ttu-id="1f9cc-201">Атрибут `DataType` обеспечивает поддержку платформы ASP.NET Core для выбора соответствующего шаблона поля, применяемого при отображении данных.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-201">The `DataType` attribute can enable the ASP.NET Core framework to choose the right field template to render the data.</span></span> <span data-ttu-id="1f9cc-202">При отдельном использовании атрибут `DisplayFormat` базируется на строковом шаблоне.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-202">The `DisplayFormat` if used by itself uses the string template.</span></span>

<span data-ttu-id="1f9cc-203">Примечание. Проверка jQuery не работает с атрибутом `Range` и `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-203">Note: jQuery validation doesn't work with the `Range` attribute and `DateTime`.</span></span> <span data-ttu-id="1f9cc-204">Например, следующий код всегда приводит к возникновению ошибки проверки на стороне клиента, даже если дата попадает в указанный диапазон:</span><span class="sxs-lookup"><span data-stu-id="1f9cc-204">For example, the following code will always display a client-side validation error, even when the date is in the specified range:</span></span>

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

<span data-ttu-id="1f9cc-205">Как правило, не рекомендуется компилировать модели с фиксированными датами, поэтому использовать атрибуты `Range` и `DateTime` следует крайне осторожно.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-205">It's generally not a good practice to compile hard dates in your models, so using the `Range` attribute and `DateTime` is discouraged.</span></span>

<span data-ttu-id="1f9cc-206">В следующем коде демонстрируется объединение атрибутов в одной строке:</span><span class="sxs-lookup"><span data-stu-id="1f9cc-206">The following code shows combining attributes on one line:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRatingDAmult.cs?name=snippet1)]

<span data-ttu-id="1f9cc-207">Дополнительные операции EF Core с Razor Pages см. в статье [Начало работы с Razor Pages и EF Core](xref:data/ef-rp/intro).</span><span class="sxs-lookup"><span data-stu-id="1f9cc-207">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) shows advanced EF Core operations with Razor Pages.</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="1f9cc-208">Применение миграции</span><span class="sxs-lookup"><span data-stu-id="1f9cc-208">Apply migrations</span></span>

<span data-ttu-id="1f9cc-209">DataAnnotation, примененные к классу, изменяют схему.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-209">The DataAnnotations applied to the class change the schema.</span></span> <span data-ttu-id="1f9cc-210">Например, DataAnnotation, примеренные к полю `Title`, выполняют следующее:</span><span class="sxs-lookup"><span data-stu-id="1f9cc-210">For example, the DataAnnotations applied to the `Title` field:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRatingDA.cs?name=snippet11)]

* <span data-ttu-id="1f9cc-211">ограничивают число символов до 60;</span><span class="sxs-lookup"><span data-stu-id="1f9cc-211">Limits the characters to 60.</span></span>
* <span data-ttu-id="1f9cc-212">не допускают значение `null`.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-212">Doesn't allow a `null` value.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1f9cc-213">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1f9cc-213">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1f9cc-214">Сейчас таблица `Movie` имеет следующую схему:</span><span class="sxs-lookup"><span data-stu-id="1f9cc-214">The `Movie` table currently has the following schema:</span></span>

``` sql
CREATE TABLE [dbo].[Movie] (
    [ID]          INT             IDENTITY (1, 1) NOT NULL,
    [Title]       NVARCHAR (MAX)  NULL,
    [ReleaseDate] DATETIME2 (7)   NOT NULL,
    [Genre]       NVARCHAR (MAX)  NULL,
    [Price]       DECIMAL (18, 2) NOT NULL,
    [Rating]      NVARCHAR (MAX)  NULL,
    CONSTRAINT [PK_Movie] PRIMARY KEY CLUSTERED ([ID] ASC)
);
```

<span data-ttu-id="1f9cc-215">Предыдущие изменения схемы не приводят к созданию исключения EF.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-215">The preceding schema changes don't cause EF to throw an exception.</span></span> <span data-ttu-id="1f9cc-216">Но следует создать миграцию, чтобы схема соответствовала модели.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-216">However, create a migration so the schema is consistent with the model.</span></span>

<span data-ttu-id="1f9cc-217">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-217">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="1f9cc-218">В PMC введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="1f9cc-218">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration New_DataAnnotations
Update-Database
```

<span data-ttu-id="1f9cc-219">`Update-Database` выполняет методы `Up` класса `New_DataAnnotations`.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-219">`Update-Database` runs the `Up` methods of the `New_DataAnnotations` class.</span></span> <span data-ttu-id="1f9cc-220">Проверьте метод `Up`.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-220">Examine the `Up` method:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Migrations/20190724163003_New_DataAnnotations.cs?name=snippet)]

<span data-ttu-id="1f9cc-221">Обновленная таблица `Movie` имеет следующую схему:</span><span class="sxs-lookup"><span data-stu-id="1f9cc-221">The updated `Movie` table has the following schema:</span></span>

``` sql
CREATE TABLE [dbo].[Movie] (
    [ID]          INT             IDENTITY (1, 1) NOT NULL,
    [Title]       NVARCHAR (60)   NOT NULL,
    [ReleaseDate] DATETIME2 (7)   NOT NULL,
    [Genre]       NVARCHAR (30)   NOT NULL,
    [Price]       DECIMAL (18, 2) NOT NULL,
    [Rating]      NVARCHAR (5)    NOT NULL,
    CONSTRAINT [PK_Movie] PRIMARY KEY CLUSTERED ([ID] ASC)
);
```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="1f9cc-222">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="1f9cc-222">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="1f9cc-223">Для SQLite миграция не требуется.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-223">Migrations are not required for SQLite.</span></span>

---

### <a name="publish-to-azure"></a><span data-ttu-id="1f9cc-224">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="1f9cc-224">Publish to Azure</span></span>

<span data-ttu-id="1f9cc-225">Сведения о развертывании в Azure, см. в разделе [Учебник: Создание приложения ASP.NET Core в Azure с подключением к базе данных SQL](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb).</span><span class="sxs-lookup"><span data-stu-id="1f9cc-225">For information on deploying to Azure, see [Tutorial: Build an ASP.NET Core app in Azure with SQL Database](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb).</span></span>

<span data-ttu-id="1f9cc-226">Благодарим вас за изучение общих сведений о страницах Razor.</span><span class="sxs-lookup"><span data-stu-id="1f9cc-226">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="1f9cc-227">Отличным дополнением к этому руководству является руководство по [началу работы с Razor Pages и EF Core](xref:data/ef-rp/intro).</span><span class="sxs-lookup"><span data-stu-id="1f9cc-227">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1f9cc-228">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="1f9cc-228">Additional resources</span></span>

* <xref:mvc/views/working-with-forms>
* <xref:fundamentals/localization>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/authoring>
* [<span data-ttu-id="1f9cc-229">Версия руководства на YouTube</span><span class="sxs-lookup"><span data-stu-id="1f9cc-229">YouTube version of this tutorial</span></span>](https://youtu.be/b63m66eu7us)

> [!div class="step-by-step"]
> [<span data-ttu-id="1f9cc-230">Предыдущая статья. Добавление нового поля</span><span class="sxs-lookup"><span data-stu-id="1f9cc-230">Previous: Adding a new field</span></span>](xref:tutorials/razor-pages/new-field)
