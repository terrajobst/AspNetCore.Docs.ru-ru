---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: Проверка вводимых пользователем данных в ASP.NET Web Pages сайтов (Razor) | Документация Майкрософт
author: Rick-Anderson
description: В этой статье рассматриваются способы проверки информации, полученной от пользователей &mdash; то есть, убедитесь, что пользователи вводят допустимые сведения в формате HTML forms в AS...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 8f049adce33e452896b5e2a444635ff30d18e480
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021525"
---
<a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="7e821-103">Проверка пользовательского ввода на сайтах ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="7e821-103">Validating User Input in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="7e821-104">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="7e821-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="7e821-105">В этой статье рассматриваются способы проверки информации, полученной от пользователей &mdash; то есть, убедитесь, что пользователи вводят допустимые сведения в формате HTML forms на сайте веб-страниц ASP.NET (Razor).</span><span class="sxs-lookup"><span data-stu-id="7e821-105">This article discusses how to validate information you get from users &mdash; that is, to make sure that users enter valid information in HTML forms in an ASP.NET Web Pages (Razor) site.</span></span>
> 
> <span data-ttu-id="7e821-106">Вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="7e821-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="7e821-107">Как проверить, что пользовательского ввода соответствуют критериям проверки, определенных вами.</span><span class="sxs-lookup"><span data-stu-id="7e821-107">How to check that a user's input matches validation criteria that you define.</span></span>
> - <span data-ttu-id="7e821-108">Как определить, прошли ли все проверочные тесты.</span><span class="sxs-lookup"><span data-stu-id="7e821-108">How to determine whether all validation tests have passed.</span></span>
> - <span data-ttu-id="7e821-109">Способ отображения ошибок проверки (и форматировать их).</span><span class="sxs-lookup"><span data-stu-id="7e821-109">How to display validation errors (and how to format them).</span></span>
> - <span data-ttu-id="7e821-110">Как проверить данные, которые не поступают непосредственно из пользователей.</span><span class="sxs-lookup"><span data-stu-id="7e821-110">How to validate data that doesn't come directly from users.</span></span>
> 
> <span data-ttu-id="7e821-111">Ниже приведены основные понятия, представленные в этой статье программирования ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7e821-111">These are the ASP.NET programming concepts introduced in the article:</span></span>
> 
> - <span data-ttu-id="7e821-112">`Validation` Вспомогательный.</span><span class="sxs-lookup"><span data-stu-id="7e821-112">The `Validation` helper.</span></span>
> - <span data-ttu-id="7e821-113">`Html.ValidationSummary` И `Html.ValidationMessage` методы.</span><span class="sxs-lookup"><span data-stu-id="7e821-113">The `Html.ValidationSummary` and `Html.ValidationMessage` methods.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="7e821-114">Версии программного обеспечения, используемые в этом руководстве</span><span class="sxs-lookup"><span data-stu-id="7e821-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="7e821-115">Веб-страниц ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="7e821-115">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="7e821-116">Этот учебник также работает с ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="7e821-116">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="7e821-117">В этом разделе содержатся следующие подразделы:</span><span class="sxs-lookup"><span data-stu-id="7e821-117">This article contains the following sections:</span></span>

- [<span data-ttu-id="7e821-118">Обзор проверка введенных пользователем данных</span><span class="sxs-lookup"><span data-stu-id="7e821-118">Overview of User Input Validation</span></span>](#Overview_of_User_Input_Validation)
- [<span data-ttu-id="7e821-119">Проверка вводимых пользователем данных</span><span class="sxs-lookup"><span data-stu-id="7e821-119">Validating User Input</span></span>](#Validating_User_Input)
- [<span data-ttu-id="7e821-120">Добавление проверки на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="7e821-120">Adding Client-Side Validation</span></span>](#Adding_Client-Side_Validation)
- [<span data-ttu-id="7e821-121">Форматирование ошибки проверки</span><span class="sxs-lookup"><span data-stu-id="7e821-121">Formatting Validation Errors</span></span>](#Formatting_Validation_Errors)
- [<span data-ttu-id="7e821-122">Проверка данных, который не поступают непосредственно из пользователей</span><span class="sxs-lookup"><span data-stu-id="7e821-122">Validating Data That Doesn't Come Directly from Users</span></span>](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a><span data-ttu-id="7e821-123">Обзор проверка введенных пользователем данных</span><span class="sxs-lookup"><span data-stu-id="7e821-123">Overview of User Input Validation</span></span>

<span data-ttu-id="7e821-124">Если вы запрос на ввод сведений на странице — например, в форму, очень важно, чтобы убедиться в том, что значения, которые они вводят являются допустимыми.</span><span class="sxs-lookup"><span data-stu-id="7e821-124">If you ask users to enter information in a page — for example, into a form — it's important to make sure that the values that they enter are valid.</span></span> <span data-ttu-id="7e821-125">Например вы не хотите обрабатывать формы, в которой отсутствуют критически важные сведения.</span><span class="sxs-lookup"><span data-stu-id="7e821-125">For example, you don't want to process a form that's missing critical information.</span></span>

<span data-ttu-id="7e821-126">При вводе значений в HTML-форму, они вводят значения строки.</span><span class="sxs-lookup"><span data-stu-id="7e821-126">When users enter values into an HTML form, the values that they enter are strings.</span></span> <span data-ttu-id="7e821-127">Во многих случаях необходимые значения других типов данных, таких как целые числа или даты.</span><span class="sxs-lookup"><span data-stu-id="7e821-127">In many cases, the values you need are some other data types, like integers or dates.</span></span> <span data-ttu-id="7e821-128">Таким образом необходимо также убедиться, что значения, которые пользователи вводят может быть правильно преобразован в соответствующие типы данных.</span><span class="sxs-lookup"><span data-stu-id="7e821-128">Therefore, you also have to make sure that the values that users enter can be correctly converted to the appropriate data types.</span></span>

<span data-ttu-id="7e821-129">Кроме того, возможны некоторые ограничения на значения.</span><span class="sxs-lookup"><span data-stu-id="7e821-129">You might also have certain restrictions on the values.</span></span> <span data-ttu-id="7e821-130">Даже если пользователи неправильно вводят целое число, например, может потребоваться убедиться, что значение находится в пределах определенного диапазона.</span><span class="sxs-lookup"><span data-stu-id="7e821-130">Even if users correctly enter an integer, for example, you might need to make sure that the value falls within a certain range.</span></span>

![Ошибки проверки, которые используют классы стилей CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> <span data-ttu-id="7e821-132">**Важные** проверка вводимых пользователем данных также важна для обеспечения безопасности.</span><span class="sxs-lookup"><span data-stu-id="7e821-132">**Important** Validating user input is also important for security.</span></span> <span data-ttu-id="7e821-133">Если вы ограничите значения, которые пользователи могут вводить в формах, снижается вероятность того, что кто-то можно ввести значение, которое может скомпрометировать безопасность веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="7e821-133">When you restrict the values that users can enter in forms, you reduce the chance that someone can enter a value that can compromise the security of your site.</span></span>


<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a><span data-ttu-id="7e821-134">Проверка вводимых пользователем данных</span><span class="sxs-lookup"><span data-stu-id="7e821-134">Validating User Input</span></span>

<span data-ttu-id="7e821-135">В ASP.NET Web Pages 2 можно использовать `Validator` вспомогательный метод для поля ввода.</span><span class="sxs-lookup"><span data-stu-id="7e821-135">In ASP.NET Web Pages 2, you can use the `Validator` helper to test user input.</span></span> <span data-ttu-id="7e821-136">Основной подход заключается в том, выполнив следующие действия:</span><span class="sxs-lookup"><span data-stu-id="7e821-136">The basic approach is to do the following:</span></span>

1. <span data-ttu-id="7e821-137">Определите, какие входные элементы (поля), которые требуется проверить.</span><span class="sxs-lookup"><span data-stu-id="7e821-137">Determine which input elements (fields) you want to validate.</span></span>

    <span data-ttu-id="7e821-138">Вы обычно проверяете значения в `<input>` элементов в форму.</span><span class="sxs-lookup"><span data-stu-id="7e821-138">You typically validate values in `<input>` elements in a form.</span></span> <span data-ttu-id="7e821-139">Тем не менее, рекомендуется проверяйте весь ввод, даже входные данные, поступающие от ограниченного элемент, подобный `<select>` списка.</span><span class="sxs-lookup"><span data-stu-id="7e821-139">However, it's a good practice to validate all input, even input that comes from a constrained element like a `<select>` list.</span></span> <span data-ttu-id="7e821-140">Это позволяет убедиться, что пользователи не обхода элементов управления на странице и отправить форму.</span><span class="sxs-lookup"><span data-stu-id="7e821-140">This helps to make sure that users don't bypass the controls on a page and submit a form.</span></span>
2. <span data-ttu-id="7e821-141">В коде страницы добавьте отдельные проверки для каждого входного элемента с помощью методов класса `Validation` вспомогательный.</span><span class="sxs-lookup"><span data-stu-id="7e821-141">In the page code, add individual validation checks for each input element by using methods of the `Validation` helper.</span></span>

    <span data-ttu-id="7e821-142">Чтобы проверить наличие обязательных полей, используйте `Validation.RequireField(field, [error message])` (для отдельных полей) или `Validation.RequireFields(field1, field2, ...))` (для получения списка полей).</span><span class="sxs-lookup"><span data-stu-id="7e821-142">To check for required fields, use `Validation.RequireField(field, [error message])` (for an individual field) or `Validation.RequireFields(field1, field2, ...))` (for a list of fields).</span></span> <span data-ttu-id="7e821-143">Другие виды проверок используйте `Validation.Add(field, ValidationType)`.</span><span class="sxs-lookup"><span data-stu-id="7e821-143">For other types of validation, use `Validation.Add(field, ValidationType)`.</span></span> <span data-ttu-id="7e821-144">Для `ValidationType`, можно использовать следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="7e821-144">For `ValidationType`, you can use these options:</span></span>

    `Validator.DateTime ([error message])`  
   `Validator.Decimal([error message])`  
   `Validator.EqualsTo(otherField [, error message])`  
   `Validator.Float([error message])`  
   `Validator.Integer([error message])`  
   `Validator.Range(min, max [, error message])`  
   `Validator.RegEx(pattern [, error message])`  
   `Validator.Required([error message])`  
   `Validator.StringLength(length)`  
   `Validator.Url([error message])`
3. <span data-ttu-id="7e821-145">При отправке страницы, проверить, прошел ли проверку путем проверки `Validation.IsValid`:</span><span class="sxs-lookup"><span data-stu-id="7e821-145">When the page is submitted, check whether validation has passed by checking `Validation.IsValid`:</span></span>

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    <span data-ttu-id="7e821-146">При возникновении ошибок проверки, то пропустить обработки обычных страниц.</span><span class="sxs-lookup"><span data-stu-id="7e821-146">If there are any validation errors, you skip normal page processing.</span></span> <span data-ttu-id="7e821-147">Например если страница предназначена для обновления базы данных, этого не сделать, пока не будут исправлены все ошибки проверки.</span><span class="sxs-lookup"><span data-stu-id="7e821-147">For example, if the purpose of the page is to update a database, you don't do that until all validation errors have been fixed.</span></span>
4. <span data-ttu-id="7e821-148">При возникновении ошибки проверки, отображение сообщения об ошибках в разметке страницы с помощью `Html.ValidationSummary` или `Html.ValidationMessage`, или оба.</span><span class="sxs-lookup"><span data-stu-id="7e821-148">If there are validation errors, display error messages in the page's markup by using `Html.ValidationSummary` or `Html.ValidationMessage`, or both.</span></span>

<span data-ttu-id="7e821-149">В следующем примере страница, на которой показаны все эти шаги.</span><span class="sxs-lookup"><span data-stu-id="7e821-149">The following example shows a page that illustrates these steps.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

<span data-ttu-id="7e821-150">Чтобы увидеть, как работает проверка, запустите эту страницу и намеренно совершают ошибки.</span><span class="sxs-lookup"><span data-stu-id="7e821-150">To see how validation works, run this page and deliberately make mistakes.</span></span> <span data-ttu-id="7e821-151">Например, вот как если вы забыли ввести название курса, если ввести выглядит страницы, и если ввести недопустимую дату:</span><span class="sxs-lookup"><span data-stu-id="7e821-151">For example, here's what the page looks like if you forget to enter a course name, if you enter an, and if you enter an invalid date:</span></span>

![Ошибки проверки в отображаемой странице](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a><span data-ttu-id="7e821-153">Добавление проверки на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="7e821-153">Adding Client-Side Validation</span></span>

<span data-ttu-id="7e821-154">По умолчанию проверяется ввод данных пользователем, после отправки страницы — то есть проверка выполняется в серверном коде.</span><span class="sxs-lookup"><span data-stu-id="7e821-154">By default, user input is validated after users submit the page — that is, the validation is performed in server code.</span></span> <span data-ttu-id="7e821-155">Недостаток этого подхода является то, что пользователи не знают, что они сделали ошибку до после их отправки страницы.</span><span class="sxs-lookup"><span data-stu-id="7e821-155">A disadvantage of this approach is that users don't know that they've made an error until after they submit the page.</span></span> <span data-ttu-id="7e821-156">Если форма является длинное или сложное, отчетов об ошибках только в том случае, после отправки страницы может быть неудобно для пользователя.</span><span class="sxs-lookup"><span data-stu-id="7e821-156">If a form is long or complex, reporting errors only after the page is submitted can be inconvenient to the user.</span></span>

<span data-ttu-id="7e821-157">Вы можете добавить поддержку для выполнения проверки из клиентского скрипта.</span><span class="sxs-lookup"><span data-stu-id="7e821-157">You can add support to perform validation in client script.</span></span> <span data-ttu-id="7e821-158">В этом случае проверка выполняется при работе в браузере.</span><span class="sxs-lookup"><span data-stu-id="7e821-158">In that case, the validation is performed as users work in the browser.</span></span> <span data-ttu-id="7e821-159">Например предположим, что можно указать, что значение должно быть целым.</span><span class="sxs-lookup"><span data-stu-id="7e821-159">For example, suppose you specify that a value should be an integer.</span></span> <span data-ttu-id="7e821-160">Если пользователь вводит нецелочисленное значение, ошибка, как только пользователь уходит из поля ввода.</span><span class="sxs-lookup"><span data-stu-id="7e821-160">If a user enters a non-integer value, the error is reported as soon as the user leaves the entry field.</span></span> <span data-ttu-id="7e821-161">Пользователям получать немедленные отзывы, что удобно для них.</span><span class="sxs-lookup"><span data-stu-id="7e821-161">Users get immediate feedback, which is convenient for them.</span></span> <span data-ttu-id="7e821-162">Проверка на основе клиента можно также уменьшить количество раз, которые пользователь должен отправить форму, чтобы устранить несколько ошибок.</span><span class="sxs-lookup"><span data-stu-id="7e821-162">Client-based validation can also reduce the number of times that the user has to submit the form to correct multiple errors.</span></span>

> [!NOTE]
> <span data-ttu-id="7e821-163">Даже если вы используете клиентскую проверку, в серверном коде всегда также выполняется проверка.</span><span class="sxs-lookup"><span data-stu-id="7e821-163">Even if you use client-side validation, validation is always also performed in server code.</span></span> <span data-ttu-id="7e821-164">Выполнение проверки в серверном коде является мерой безопасности, в случае, если пользователи пропустить проверку на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="7e821-164">Performing validation in server code is a security measure, in case users bypass client-based validation.</span></span>


1. <span data-ttu-id="7e821-165">Зарегистрируйте следующие библиотеки JavaScript на странице:</span><span class="sxs-lookup"><span data-stu-id="7e821-165">Register the following JavaScript libraries in the page:</span></span>  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   <span data-ttu-id="7e821-166">Двух библиотек может быть загружено из сети доставки содержимого (CDN), поэтому вам не обязательно отобразить их на компьютере или сервере.</span><span class="sxs-lookup"><span data-stu-id="7e821-166">Two of the libraries are loadable from a content delivery network (CDN), so you don't necessarily have to have them on your computer or server.</span></span> <span data-ttu-id="7e821-167">Тем не менее, необходимо иметь локальную копию *jquery.validate.unobtrusive.js*.</span><span class="sxs-lookup"><span data-stu-id="7e821-167">However, you must have a local copy of *jquery.validate.unobtrusive.js*.</span></span> <span data-ttu-id="7e821-168">Если вы уже работаете не с помощью WebMatrix шаблона (например **начального сайта** ), содержит библиотеку, создание веб-страниц сайта, который основан на **начального сайта**.</span><span class="sxs-lookup"><span data-stu-id="7e821-168">If you are not already working with a WebMatrix template (like **Starter Site** ) that includes the library, create a Web Pages site that's based on **Starter Site**.</span></span> <span data-ttu-id="7e821-169">Затем скопируйте *.js* файла для текущего веб-узла.</span><span class="sxs-lookup"><span data-stu-id="7e821-169">Then copy the *.js* file to your current site.</span></span>
2. <span data-ttu-id="7e821-170">В разметке, для каждого элемента, который проверка, добавьте вызов `Validation.For(field)`.</span><span class="sxs-lookup"><span data-stu-id="7e821-170">In markup, for each element that you're validating, add a call to `Validation.For(field)`.</span></span> <span data-ttu-id="7e821-171">Этот метод создает атрибуты, используемые для проверки на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="7e821-171">This method emits attributes that are used by client-side validation.</span></span> <span data-ttu-id="7e821-172">(Вместо того чтобы выпуска фактического кода JavaScript, метод выдает атрибутов, таких как `data-val-...`.</span><span class="sxs-lookup"><span data-stu-id="7e821-172">(Rather than emitting actual JavaScript code, the method emits attributes like `data-val-...`.</span></span> <span data-ttu-id="7e821-173">Эти атрибуты поддерживают ненавязчивой клиентской проверки, который использует jQuery для выполнения работы.)</span><span class="sxs-lookup"><span data-stu-id="7e821-173">These attributes support unobtrusive client validation that uses jQuery to do the work.)</span></span>

<span data-ttu-id="7e821-174">Следующая страница демонстрирует добавление функции проверки клиента для примера, приведенного ранее.</span><span class="sxs-lookup"><span data-stu-id="7e821-174">The following page shows how to add client validation features to the example shown earlier.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

<span data-ttu-id="7e821-175">Не все проверки запуска на клиенте.</span><span class="sxs-lookup"><span data-stu-id="7e821-175">Not all validation checks run on the client.</span></span> <span data-ttu-id="7e821-176">В частности проверка типа данных (целое число, Дата и т. д.) не выполняются на клиенте.</span><span class="sxs-lookup"><span data-stu-id="7e821-176">In particular, data-type validation (integer, date, and so on) don't run on the client.</span></span> <span data-ttu-id="7e821-177">На клиенте и сервере работают следующие проверки:</span><span class="sxs-lookup"><span data-stu-id="7e821-177">The following checks work on both the client and server:</span></span>

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

<span data-ttu-id="7e821-178">В этом примере теста для допустимой датой не будет работать в клиентском коде.</span><span class="sxs-lookup"><span data-stu-id="7e821-178">In this example, the test for a valid date won't work in client code.</span></span> <span data-ttu-id="7e821-179">Тем не менее тест будет выполняться в серверном коде.</span><span class="sxs-lookup"><span data-stu-id="7e821-179">However, the test will be performed in server code.</span></span>

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a><span data-ttu-id="7e821-180">Форматирование ошибки проверки</span><span class="sxs-lookup"><span data-stu-id="7e821-180">Formatting Validation Errors</span></span>

<span data-ttu-id="7e821-181">Можно задать способ отображения ошибок проверки, определяя классы CSS, которые имеют следующие зарезервированные имена:</span><span class="sxs-lookup"><span data-stu-id="7e821-181">You can control how validation errors are displayed by defining CSS classes that have the following reserved names:</span></span>

- <span data-ttu-id="7e821-182">`field-validation-error`.</span><span class="sxs-lookup"><span data-stu-id="7e821-182">`field-validation-error`.</span></span> <span data-ttu-id="7e821-183">Определяет выходные данные `Html.ValidationMessage` метод, когда он отображает ошибку.</span><span class="sxs-lookup"><span data-stu-id="7e821-183">Defines the output of the `Html.ValidationMessage` method when it's displaying an error.</span></span>
- <span data-ttu-id="7e821-184">`field-validation-valid`.</span><span class="sxs-lookup"><span data-stu-id="7e821-184">`field-validation-valid`.</span></span> <span data-ttu-id="7e821-185">Определяет выходные данные `Html.ValidationMessage` метод при отсутствии ошибок.</span><span class="sxs-lookup"><span data-stu-id="7e821-185">Defines the output of the `Html.ValidationMessage` method when there is no error.</span></span>
- <span data-ttu-id="7e821-186">`input-validation-error`.</span><span class="sxs-lookup"><span data-stu-id="7e821-186">`input-validation-error`.</span></span> <span data-ttu-id="7e821-187">Определяет как `<input>` элементы отображаются в том случае, когда возникает ошибка.</span><span class="sxs-lookup"><span data-stu-id="7e821-187">Defines how `<input>` elements are rendered when there's an error.</span></span> <span data-ttu-id="7e821-188">(Например, этот класс можно использовать, чтобы задать цвет фона &lt;входной&gt; элемент на другой цвет, если его значение является недопустимым.) Этот класс CSS используется только во время проверки клиента (в ASP.NET Web Pages 2).</span><span class="sxs-lookup"><span data-stu-id="7e821-188">(For example, you can use this class to set the background color of an &lt;input&gt; element to a different color if its value is invalid.) This CSS class is used only during client validation (in ASP.NET Web Pages 2).</span></span>
- <span data-ttu-id="7e821-189">`input-validation-valid`.</span><span class="sxs-lookup"><span data-stu-id="7e821-189">`input-validation-valid`.</span></span> <span data-ttu-id="7e821-190">Определяет внешний вид `<input>` элементов при отсутствии ошибок.</span><span class="sxs-lookup"><span data-stu-id="7e821-190">Defines the appearance of `<input>` elements when there is no error.</span></span>
- <span data-ttu-id="7e821-191">`validation-summary-errors`.</span><span class="sxs-lookup"><span data-stu-id="7e821-191">`validation-summary-errors`.</span></span> <span data-ttu-id="7e821-192">Определяет выходные данные `Html.ValidationSummary` метод, он отображает список ошибок.</span><span class="sxs-lookup"><span data-stu-id="7e821-192">Defines the output of the `Html.ValidationSummary` method it's displaying a list of errors.</span></span>
- <span data-ttu-id="7e821-193">`validation-summary-valid`.</span><span class="sxs-lookup"><span data-stu-id="7e821-193">`validation-summary-valid`.</span></span> <span data-ttu-id="7e821-194">Определяет выходные данные `Html.ValidationSummary` метод при отсутствии ошибок.</span><span class="sxs-lookup"><span data-stu-id="7e821-194">Defines the output of the `Html.ValidationSummary` method when there is no error.</span></span>

<span data-ttu-id="7e821-195">Следующие `<style>` блока отображаются правила для условий ошибок.</span><span class="sxs-lookup"><span data-stu-id="7e821-195">The following `<style>` block shows rules for error conditions.</span></span>

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

<span data-ttu-id="7e821-196">Если включить этот блок стиля в примере страницы из ранее в этой статье, отображение ошибок будет выглядеть как на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="7e821-196">If you include this style block in the example pages from earlier in the article, the error display will look like the following illustration:</span></span>

![Ошибки проверки, которые используют классы стилей CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="7e821-198">Если вы не используете клиентская проверка в ASP.NET Web Pages 2, CSS классы для `<input>` элементы (`input-validation-error` и `input-validation-valid` не оказывает никакого воздействия.</span><span class="sxs-lookup"><span data-stu-id="7e821-198">If you're not using client validation in ASP.NET Web Pages 2, the CSS classes for the `<input>` elements (`input-validation-error` and `input-validation-valid` don't have any effect.</span></span>


### <a name="static-and-dynamic-error-display"></a><span data-ttu-id="7e821-199">Отображение статических и динамических ошибок</span><span class="sxs-lookup"><span data-stu-id="7e821-199">Static and Dynamic Error Display</span></span>

<span data-ttu-id="7e821-200">Правила CSS возникают попарно, например `validation-summary-errors` и `validation-summary-valid`.</span><span class="sxs-lookup"><span data-stu-id="7e821-200">The CSS rules come in pairs, such as `validation-summary-errors` and `validation-summary-valid`.</span></span> <span data-ttu-id="7e821-201">Эти пары позволяют определить правила для обоих условий: ошибки и условие «Обычная работа» (без ошибок).</span><span class="sxs-lookup"><span data-stu-id="7e821-201">These pairs let you define rules for both conditions: an error condition and a "normal" (non-error) condition.</span></span> <span data-ttu-id="7e821-202">Важно понимать, что разметка для отображения ошибок отображается всегда, даже если ошибок нет.</span><span class="sxs-lookup"><span data-stu-id="7e821-202">It's important to understand that the markup for the error display is always rendered, even if there are no errors.</span></span> <span data-ttu-id="7e821-203">Например, если страница содержит `Html.ValidationSummary` метод в разметке, исходный код страницы будет содержать следующую разметку даже в том случае, если страница запрашивается в первый раз:</span><span class="sxs-lookup"><span data-stu-id="7e821-203">For example, if a page has an `Html.ValidationSummary` method in the markup, the page source will contain the following markup even when the page is requested for the first time:</span></span>

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

<span data-ttu-id="7e821-204">Другими словами `Html.ValidationSummary` метод всегда отображает `<div>` элемента и списка, даже если в списке ошибок является пустым.</span><span class="sxs-lookup"><span data-stu-id="7e821-204">In other words, the `Html.ValidationSummary` method always renders a `<div>` element and a list, even if the error list is empty.</span></span> <span data-ttu-id="7e821-205">Аналогичным образом `Html.ValidationMessage` метод всегда отображает `<span>` элемент как заполнитель для ошибку отдельного поля, даже в том случае, если ошибки отсутствуют.</span><span class="sxs-lookup"><span data-stu-id="7e821-205">Similarly, the `Html.ValidationMessage` method always renders a `<span>` element as a placeholder for an individual field error, even if there is no error.</span></span>

<span data-ttu-id="7e821-206">В некоторых ситуациях, отображая сообщение об ошибке может привести к странице, чтобы перекомпоновать и может привести к элементов на странице, чтобы перемещать.</span><span class="sxs-lookup"><span data-stu-id="7e821-206">In some situations, displaying an error message can cause the page to reflow and can cause elements on the page to move around.</span></span> <span data-ttu-id="7e821-207">Правила CSS, которые заканчиваются на `-valid` позволяют определить макет, который может помочь предотвратить возникновение этой проблемы.</span><span class="sxs-lookup"><span data-stu-id="7e821-207">The CSS rules that end in `-valid` let you define a layout that can help prevent this problem.</span></span> <span data-ttu-id="7e821-208">Например, можно определить `field-validation-error` и `field-validation-valid` на оба имеют же фиксированный размер.</span><span class="sxs-lookup"><span data-stu-id="7e821-208">For example, you can define `field-validation-error` and `field-validation-valid` to both have the same fixed size.</span></span> <span data-ttu-id="7e821-209">Таким образом, отображаемую область для поля является статическим и поток страниц не изменится, если отображается сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="7e821-209">That way, the display area for the field is static and won't change the page flow if an error message is displayed.</span></span>

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a><span data-ttu-id="7e821-210">Проверка данных, который не поступают непосредственно из пользователей</span><span class="sxs-lookup"><span data-stu-id="7e821-210">Validating Data That Doesn't Come Directly from Users</span></span>

<span data-ttu-id="7e821-211">Иногда необходимо проверить сведения, которые не поступают непосредственно из формы HTML.</span><span class="sxs-lookup"><span data-stu-id="7e821-211">Sometimes you have to validate information that doesn't come directly from an HTML form.</span></span> <span data-ttu-id="7e821-212">Типичным примером является страницей, где значение передается в строке запроса, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="7e821-212">A typical example is a page where a value is passed in a query string, as in the following example:</span></span>

`http://server/myapp/EditClassInformation?classid=1022`

<span data-ttu-id="7e821-213">В этом случае необходимо убедиться, что значение, которое передается на страницу (здесь 1022 значения `classid`) является допустимым.</span><span class="sxs-lookup"><span data-stu-id="7e821-213">In this case, you want to make sure that the value that's passed to the page (here, 1022 for the value of `classid`) is valid.</span></span> <span data-ttu-id="7e821-214">Нельзя напрямую использовать `Validation` вспомогательный метод для выполнения этой проверки.</span><span class="sxs-lookup"><span data-stu-id="7e821-214">You can't directly use the `Validation` helper to perform this validation.</span></span> <span data-ttu-id="7e821-215">Тем не менее можно использовать другие функции проверки системы, такие как возможность отображения сообщений об ошибках проверки.</span><span class="sxs-lookup"><span data-stu-id="7e821-215">However, you can use other features of the validation system, like the ability to display validation error messages.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="7e821-216">**Важные** всегда проверяйте значения, получаемые из *любой* источника, включая значения поля формы, значения строки запроса и значений файла cookie.</span><span class="sxs-lookup"><span data-stu-id="7e821-216">**Important** Always validate values that you get from *any* source, including form-field values, query-string values, and cookie values.</span></span> <span data-ttu-id="7e821-217">Это легко изменять эти значения (возможно злонамеренных целях).</span><span class="sxs-lookup"><span data-stu-id="7e821-217">It's easy for people to change these values (perhaps for malicious purposes).</span></span> <span data-ttu-id="7e821-218">Поэтому необходимо проверить эти значения для защиты приложения.</span><span class="sxs-lookup"><span data-stu-id="7e821-218">So you must check these values in order to protect your application.</span></span>


<span data-ttu-id="7e821-219">В следующем примере показано, как может проверить значение, которое передается в строке запроса.</span><span class="sxs-lookup"><span data-stu-id="7e821-219">The following example shows how you might validate a value that's passed in a query string.</span></span> <span data-ttu-id="7e821-220">Код проверяет, что значение не пустое, и что это целое число.</span><span class="sxs-lookup"><span data-stu-id="7e821-220">The code tests that the value is not empty and that it's an integer.</span></span>

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

<span data-ttu-id="7e821-221">Обратите внимание на то, что данная проверка выполняется, если запрос не является отправка формы (`if(!IsPost)`).</span><span class="sxs-lookup"><span data-stu-id="7e821-221">Notice that the test is performed when the request is not a form submission (`if(!IsPost)`).</span></span> <span data-ttu-id="7e821-222">Этот тест будет передан при первом запросе страницы, но не в том случае, если запрос имеет отправки формы.</span><span class="sxs-lookup"><span data-stu-id="7e821-222">This test would pass the first time that the page is requested, but not when the request is a form submission.</span></span>

<span data-ttu-id="7e821-223">Чтобы отобразить эту ошибку, можно добавить ошибки к списку ошибок проверки, вызвав `Validation.AddFormError("message")`.</span><span class="sxs-lookup"><span data-stu-id="7e821-223">To display this error, you can add the error to the list of validation errors by calling `Validation.AddFormError("message")`.</span></span> <span data-ttu-id="7e821-224">Если страница содержит вызов `Html.ValidationSummary` метод, ошибка отображается как ошибка проверки входных данных пользователя.</span><span class="sxs-lookup"><span data-stu-id="7e821-224">If the page contains a call to the `Html.ValidationSummary` method, the error is displayed there, just like a user-input validation error.</span></span>

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="7e821-225">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="7e821-225">Additional Resources</span></span>

[<span data-ttu-id="7e821-226">Работа с HTML-форм в ASP.NET веб-страниц узлов</span><span class="sxs-lookup"><span data-stu-id="7e821-226">Working with HTML Forms in ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkID=202892)
