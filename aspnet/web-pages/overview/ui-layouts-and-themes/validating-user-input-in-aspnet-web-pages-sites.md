---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: Проверка пользовательского ввода в ASP.NET Web Pages (Razor) узлов | Документы Microsoft
author: tfitzmac
description: В этой статье описывается, как для проверки сведений, получаемых от пользователей &mdash; то есть, убедитесь, что пользователь ввел допустимый сведения в формате HTML форм в как...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 34f703e6db70ac79c22f4a50d4cfd4e2326b4c74
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30899182"
---
<a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="ce5e7-103">Проверка пользовательского ввода в веб-страницы (Razor) узлов ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ce5e7-103">Validating User Input in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="ce5e7-104">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="ce5e7-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="ce5e7-105">В этой статье описывается, как для проверки сведений, получаемых от пользователей &mdash; то есть, убедитесь, что пользователь ввел допустимый сведения в формате HTML форм на сайте ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="ce5e7-105">This article discusses how to validate information you get from users &mdash; that is, to make sure that users enter valid information in HTML forms in an ASP.NET Web Pages (Razor) site.</span></span>
> 
> <span data-ttu-id="ce5e7-106">Что вы узнаете следующее.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="ce5e7-107">Как проверить, что ввода пользователя соответствует критериям проверки.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-107">How to check that a user's input matches validation criteria that you define.</span></span>
> - <span data-ttu-id="ce5e7-108">Как определить, является ли все проверочные тесты были пройдены.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-108">How to determine whether all validation tests have passed.</span></span>
> - <span data-ttu-id="ce5e7-109">Способ отображения ошибок проверки (и форматировать их).</span><span class="sxs-lookup"><span data-stu-id="ce5e7-109">How to display validation errors (and how to format them).</span></span>
> - <span data-ttu-id="ce5e7-110">Как проверить данные, не поступать из пользователей.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-110">How to validate data that doesn't come directly from users.</span></span>
> 
> <span data-ttu-id="ce5e7-111">Ниже перечислены ASP.NET программирования концепции, реализованные в статье:</span><span class="sxs-lookup"><span data-stu-id="ce5e7-111">These are the ASP.NET programming concepts introduced in the article:</span></span>
> 
> - <span data-ttu-id="ce5e7-112">`Validation` Вспомогательные.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-112">The `Validation` helper.</span></span>
> - <span data-ttu-id="ce5e7-113">`Html.ValidationSummary` И `Html.ValidationMessage` методы.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-113">The `Html.ValidationSummary` and `Html.ValidationMessage` methods.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ce5e7-114">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="ce5e7-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="ce5e7-115">Веб-страниц ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="ce5e7-115">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="ce5e7-116">Этот учебник также работает с веб-страницы ASP.NET 2.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-116">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="ce5e7-117">В этом разделе содержатся следующие подразделы:</span><span class="sxs-lookup"><span data-stu-id="ce5e7-117">This article contains the following sections:</span></span>

- [<span data-ttu-id="ce5e7-118">Общие сведения о проверка введенных пользователем данных</span><span class="sxs-lookup"><span data-stu-id="ce5e7-118">Overview of User Input Validation</span></span>](#Overview_of_User_Input_Validation)
- [<span data-ttu-id="ce5e7-119">Проверка пользовательского ввода</span><span class="sxs-lookup"><span data-stu-id="ce5e7-119">Validating User Input</span></span>](#Validating_User_Input)
- [<span data-ttu-id="ce5e7-120">Добавление проверки на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="ce5e7-120">Adding Client-Side Validation</span></span>](#Adding_Client-Side_Validation)
- [<span data-ttu-id="ce5e7-121">Форматирование ошибки проверки</span><span class="sxs-lookup"><span data-stu-id="ce5e7-121">Formatting Validation Errors</span></span>](#Formatting_Validation_Errors)
- [<span data-ttu-id="ce5e7-122">Проверка данных, не поступать из пользователей</span><span class="sxs-lookup"><span data-stu-id="ce5e7-122">Validating Data That Doesn't Come Directly from Users</span></span>](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a><span data-ttu-id="ce5e7-123">Общие сведения о проверка введенных пользователем данных</span><span class="sxs-lookup"><span data-stu-id="ce5e7-123">Overview of User Input Validation</span></span>

<span data-ttu-id="ce5e7-124">Если вы запрос на ввод сведений на странице — например, в форму, очень важно, чтобы убедиться в том, что допустимы значения, которые они вводят.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-124">If you ask users to enter information in a page — for example, into a form — it's important to make sure that the values that they enter are valid.</span></span> <span data-ttu-id="ce5e7-125">Например не нужно выполнять формы, в которой отсутствуют критически важной информации.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-125">For example, you don't want to process a form that's missing critical information.</span></span>

<span data-ttu-id="ce5e7-126">Когда пользователь вводит значения в HTML-формы, значения, которые они вводят представляют собой строки.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-126">When users enter values into an HTML form, the values that they enter are strings.</span></span> <span data-ttu-id="ce5e7-127">Во многих случаях нужные значения других типов данных, такие как целые числа или даты.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-127">In many cases, the values you need are some other data types, like integers or dates.</span></span> <span data-ttu-id="ce5e7-128">Таким образом необходимо также убедиться, что значения, которые пользователи вводят может правильно преобразован в соответствующий тип данных.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-128">Therefore, you also have to make sure that the values that users enter can be correctly converted to the appropriate data types.</span></span>

<span data-ttu-id="ce5e7-129">Может также имеют определенные ограничения на значения.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-129">You might also have certain restrictions on the values.</span></span> <span data-ttu-id="ce5e7-130">Даже если пользователи неправильно введите целое число, например, может потребоваться убедитесь в том, что значение находится в пределах определенного диапазона.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-130">Even if users correctly enter an integer, for example, you might need to make sure that the value falls within a certain range.</span></span>

![Ошибки проверки, которые используют классы стилей CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> <span data-ttu-id="ce5e7-132">**Важные** проверка пользовательского ввода также важна для обеспечения безопасности.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-132">**Important** Validating user input is also important for security.</span></span> <span data-ttu-id="ce5e7-133">Если ограничить значения, которые пользователи могут вводить в формах, снижается вероятность того, что кто-то можно ввести значение, которое может скомпрометировать безопасность веб-узла.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-133">When you restrict the values that users can enter in forms, you reduce the chance that someone can enter a value that can compromise the security of your site.</span></span>


<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a><span data-ttu-id="ce5e7-134">Проверка пользовательского ввода</span><span class="sxs-lookup"><span data-stu-id="ce5e7-134">Validating User Input</span></span>

<span data-ttu-id="ce5e7-135">В ASP.NET Web Pages 2 можно использовать `Validator` вспомогательный метод для поля ввода.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-135">In ASP.NET Web Pages 2, you can use the `Validator` helper to test user input.</span></span> <span data-ttu-id="ce5e7-136">Основной подход заключается в том, чтобы сделать следующее:</span><span class="sxs-lookup"><span data-stu-id="ce5e7-136">The basic approach is to do the following:</span></span>

1. <span data-ttu-id="ce5e7-137">Определите, какие входные элементы (поля), который нужно проверить.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-137">Determine which input elements (fields) you want to validate.</span></span>

    <span data-ttu-id="ce5e7-138">Как правило проверки значений в `<input>` элементы в форме.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-138">You typically validate values in `<input>` elements in a form.</span></span> <span data-ttu-id="ce5e7-139">Однако это рекомендуется проверять все входные данные, даже ввода, поступающие от элемента, ограниченного как `<select>` списка.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-139">However, it's a good practice to validate all input, even input that comes from a constrained element like a `<select>` list.</span></span> <span data-ttu-id="ce5e7-140">Это помогает обеспечить, что пользователи не обхода элементов управления на странице и отправить форму.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-140">This helps to make sure that users don't bypass the controls on a page and submit a form.</span></span>
2. <span data-ttu-id="ce5e7-141">В коде страницы добавьте отдельные проверки каждого входного элемента с помощью методов `Validation` вспомогательный.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-141">In the page code, add individual validation checks for each input element by using methods of the `Validation` helper.</span></span>

    <span data-ttu-id="ce5e7-142">Чтобы проверить наличие обязательных полей, используйте `Validation.RequireField(field, [error message])` (для отдельных полей) или `Validation.RequireFields(field1, field2, ...))` (для получения списка полей).</span><span class="sxs-lookup"><span data-stu-id="ce5e7-142">To check for required fields, use `Validation.RequireField(field, [error message])` (for an individual field) or `Validation.RequireFields(field1, field2, ...))` (for a list of fields).</span></span> <span data-ttu-id="ce5e7-143">Для других типов проверки, используйте `Validation.Add(field, ValidationType)`.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-143">For other types of validation, use `Validation.Add(field, ValidationType)`.</span></span> <span data-ttu-id="ce5e7-144">Для `ValidationType`, можно использовать следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="ce5e7-144">For `ValidationType`, you can use these options:</span></span>

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
3. <span data-ttu-id="ce5e7-145">При отправке страницы проверить, достиг ли проверка, проверив `Validation.IsValid`:</span><span class="sxs-lookup"><span data-stu-id="ce5e7-145">When the page is submitted, check whether validation has passed by checking `Validation.IsValid`:</span></span>

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    <span data-ttu-id="ce5e7-146">В случае ошибки проверки, можно пропустить обработки обычных страниц.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-146">If there are any validation errors, you skip normal page processing.</span></span> <span data-ttu-id="ce5e7-147">Например если страница предназначена для обновления базы данных, этого не сделать, пока не будут исправлены все ошибки проверки.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-147">For example, if the purpose of the page is to update a database, you don't do that until all validation errors have been fixed.</span></span>
4. <span data-ttu-id="ce5e7-148">Если имеются ошибки проверки, отображать сообщения об ошибках в разметке страницы с помощью `Html.ValidationSummary` или `Html.ValidationMessage`, или оба.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-148">If there are validation errors, display error messages in the page's markup by using `Html.ValidationSummary` or `Html.ValidationMessage`, or both.</span></span>

<span data-ttu-id="ce5e7-149">Пример страницы, который иллюстрирует следующие действия.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-149">The following example shows a page that illustrates these steps.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

<span data-ttu-id="ce5e7-150">Чтобы увидеть, как работает проверка, запустите эту страницу и намеренно делают ошибки.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-150">To see how validation works, run this page and deliberately make mistakes.</span></span> <span data-ttu-id="ce5e7-151">Например, вот как если вы забыли ввести название курса, если ввести выглядит страницы, а при вводе недопустимую дату:</span><span class="sxs-lookup"><span data-stu-id="ce5e7-151">For example, here's what the page looks like if you forget to enter a course name, if you enter an, and if you enter an invalid date:</span></span>

![Ошибки проверки в отображаемой странице](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a><span data-ttu-id="ce5e7-153">Добавление проверки на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="ce5e7-153">Adding Client-Side Validation</span></span>

<span data-ttu-id="ce5e7-154">По умолчанию проверяется ввод данных пользователем после отправки страницы — то есть проверка выполняется в серверном коде.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-154">By default, user input is validated after users submit the page — that is, the validation is performed in server code.</span></span> <span data-ttu-id="ce5e7-155">Недостатком этого подхода является, что пользователи не знают, что они внесли ошибку до их после отправки страницы.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-155">A disadvantage of this approach is that users don't know that they've made an error until after they submit the page.</span></span> <span data-ttu-id="ce5e7-156">Если форма является длинное или сложное, отчетов об ошибках только после отправки страницы может быть неудобным для пользователя.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-156">If a form is long or complex, reporting errors only after the page is submitted can be inconvenient to the user.</span></span>

<span data-ttu-id="ce5e7-157">Вы можете добавить поддержку для выполнения проверки в клиентского скрипта.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-157">You can add support to perform validation in client script.</span></span> <span data-ttu-id="ce5e7-158">В этом случае проверка выполняется при работе в браузере.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-158">In that case, the validation is performed as users work in the browser.</span></span> <span data-ttu-id="ce5e7-159">Например предположим, что можно указать, что значение должно быть целым.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-159">For example, suppose you specify that a value should be an integer.</span></span> <span data-ttu-id="ce5e7-160">Если пользователь вводит значение не целое число, как только пользователь покидает поле запись об ошибке.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-160">If a user enters a non-integer value, the error is reported as soon as the user leaves the entry field.</span></span> <span data-ttu-id="ce5e7-161">Пользователи получают мгновенно узнавать, что удобно при работе с ними.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-161">Users get immediate feedback, which is convenient for them.</span></span> <span data-ttu-id="ce5e7-162">Проверка на основе клиента можно также уменьшить количество раз, которые есть у пользователя для отправки формы, чтобы устранить несколько ошибок.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-162">Client-based validation can also reduce the number of times that the user has to submit the form to correct multiple errors.</span></span>

> [!NOTE]
> <span data-ttu-id="ce5e7-163">Даже при использовании проверки на стороне клиента в серверном коде всегда также выполняется проверка.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-163">Even if you use client-side validation, validation is always also performed in server code.</span></span> <span data-ttu-id="ce5e7-164">Выполнение проверки в серверном коде является мерой безопасности, в случае, если пользователи пропуска проверки на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-164">Performing validation in server code is a security measure, in case users bypass client-based validation.</span></span>


1. <span data-ttu-id="ce5e7-165">Зарегистрируйте следующие библиотеки JavaScript на странице:</span><span class="sxs-lookup"><span data-stu-id="ce5e7-165">Register the following JavaScript libraries in the page:</span></span>  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   <span data-ttu-id="ce5e7-166">Двух библиотек могут загружаться из сети доставки содержимого (CDN), поэтому не нужно обязательно иметь их на компьютере или сервере.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-166">Two of the libraries are loadable from a content delivery network (CDN), so you don't necessarily have to have them on your computer or server.</span></span> <span data-ttu-id="ce5e7-167">Тем не менее, необходимо иметь локальную копию *jquery.validate.unobtrusive.js*.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-167">However, you must have a local copy of *jquery.validate.unobtrusive.js*.</span></span> <span data-ttu-id="ce5e7-168">Если вы уже работаете не с помощью WebMatrix шаблона (например **начального сайта** ), включает библиотеку, создайте сайт веб-страницы на основе **начального сайта**.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-168">If you are not already working with a WebMatrix template (like **Starter Site** ) that includes the library, create a Web Pages site that's based on **Starter Site**.</span></span> <span data-ttu-id="ce5e7-169">Затем скопируйте *.js* файла для текущего веб-узла.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-169">Then copy the *.js* file to your current site.</span></span>
2. <span data-ttu-id="ce5e7-170">В разметке для каждого элемента, который проверка, добавьте вызов `Validation.For(field)`.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-170">In markup, for each element that you're validating, add a call to `Validation.For(field)`.</span></span> <span data-ttu-id="ce5e7-171">Этот метод создает атрибуты, используемые для проверки на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-171">This method emits attributes that are used by client-side validation.</span></span> <span data-ttu-id="ce5e7-172">(Вместо раскрытием фактический код JavaScript, метод выдает такие атрибуты, как `data-val-...`.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-172">(Rather than emitting actual JavaScript code, the method emits attributes like `data-val-...`.</span></span> <span data-ttu-id="ce5e7-173">Эти атрибуты поддерживают ненавязчивой клиентской проверки используется jQuery для выполнения работы.)</span><span class="sxs-lookup"><span data-stu-id="ce5e7-173">These attributes support unobtrusive client validation that uses jQuery to do the work.)</span></span>

<span data-ttu-id="ce5e7-174">Следующая страница демонстрирует добавление функции проверки клиента в примере выше.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-174">The following page shows how to add client validation features to the example shown earlier.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

<span data-ttu-id="ce5e7-175">Не все проверки запуска на клиенте.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-175">Not all validation checks run on the client.</span></span> <span data-ttu-id="ce5e7-176">В частности проверка типа данных (целое число, даты и т. д) не выполняются на клиенте.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-176">In particular, data-type validation (integer, date, and so on) don't run on the client.</span></span> <span data-ttu-id="ce5e7-177">На клиенте и сервере работают следующие проверки:</span><span class="sxs-lookup"><span data-stu-id="ce5e7-177">The following checks work on both the client and server:</span></span>

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

<span data-ttu-id="ce5e7-178">В этом примере теста для допустимой датой не будет работать в клиентском коде.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-178">In this example, the test for a valid date won't work in client code.</span></span> <span data-ttu-id="ce5e7-179">Тем не менее тест будет выполняться в серверном коде.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-179">However, the test will be performed in server code.</span></span>

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a><span data-ttu-id="ce5e7-180">Форматирование ошибки проверки</span><span class="sxs-lookup"><span data-stu-id="ce5e7-180">Formatting Validation Errors</span></span>

<span data-ttu-id="ce5e7-181">Можно управлять отображением ошибки проверки, определив классы CSS, которые имеют следующие зарезервированные имена:</span><span class="sxs-lookup"><span data-stu-id="ce5e7-181">You can control how validation errors are displayed by defining CSS classes that have the following reserved names:</span></span>

- <span data-ttu-id="ce5e7-182">`field-validation-error`.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-182">`field-validation-error`.</span></span> <span data-ttu-id="ce5e7-183">Определяет выходные данные `Html.ValidationMessage` метод, если он отображает ошибку.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-183">Defines the output of the `Html.ValidationMessage` method when it's displaying an error.</span></span>
- <span data-ttu-id="ce5e7-184">`field-validation-valid`.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-184">`field-validation-valid`.</span></span> <span data-ttu-id="ce5e7-185">Определяет выходные данные `Html.ValidationMessage` метод, если нет ошибок.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-185">Defines the output of the `Html.ValidationMessage` method when there is no error.</span></span>
- <span data-ttu-id="ce5e7-186">`input-validation-error`.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-186">`input-validation-error`.</span></span> <span data-ttu-id="ce5e7-187">Определяет способ `<input>` элементы отображаются в том случае, когда возникает ошибка.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-187">Defines how `<input>` elements are rendered when there's an error.</span></span> <span data-ttu-id="ce5e7-188">(Например, этот класс можно использовать, чтобы задать цвет фона &lt;ввода&gt; элемент, цвет, если его значение недопустимо.) Этот класс CSS используется только во время проверки клиента (в ASP.NET Web Pages 2).</span><span class="sxs-lookup"><span data-stu-id="ce5e7-188">(For example, you can use this class to set the background color of an &lt;input&gt; element to a different color if its value is invalid.) This CSS class is used only during client validation (in ASP.NET Web Pages 2).</span></span>
- <span data-ttu-id="ce5e7-189">`input-validation-valid`.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-189">`input-validation-valid`.</span></span> <span data-ttu-id="ce5e7-190">Определяет внешний вид `<input>` элементы, когда он не содержит ошибок.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-190">Defines the appearance of `<input>` elements when there is no error.</span></span>
- <span data-ttu-id="ce5e7-191">`validation-summary-errors`.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-191">`validation-summary-errors`.</span></span> <span data-ttu-id="ce5e7-192">Определяет выходные данные `Html.ValidationSummary` метод, он отображает список ошибок.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-192">Defines the output of the `Html.ValidationSummary` method it's displaying a list of errors.</span></span>
- <span data-ttu-id="ce5e7-193">`validation-summary-valid`.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-193">`validation-summary-valid`.</span></span> <span data-ttu-id="ce5e7-194">Определяет выходные данные `Html.ValidationSummary` метод, если нет ошибок.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-194">Defines the output of the `Html.ValidationSummary` method when there is no error.</span></span>

<span data-ttu-id="ce5e7-195">Следующие `<style>` блок показаны правила для условия ошибки.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-195">The following `<style>` block shows rules for error conditions.</span></span>

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

<span data-ttu-id="ce5e7-196">Если включить этот блок стиля в примере страниц из ранее в этой статье, в случае отображения ошибок будет выглядеть как на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="ce5e7-196">If you include this style block in the example pages from earlier in the article, the error display will look like the following illustration:</span></span>

![Ошибки проверки, которые используют классы стилей CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="ce5e7-198">Если вы не используете клиентская проверка в ASP.NET Web Pages 2, классы CSS `<input>` элементы (`input-validation-error` и `input-validation-valid` не оказывает никакого воздействия.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-198">If you're not using client validation in ASP.NET Web Pages 2, the CSS classes for the `<input>` elements (`input-validation-error` and `input-validation-valid` don't have any effect.</span></span>


### <a name="static-and-dynamic-error-display"></a><span data-ttu-id="ce5e7-199">Отображение статических и динамических ошибок</span><span class="sxs-lookup"><span data-stu-id="ce5e7-199">Static and Dynamic Error Display</span></span>

<span data-ttu-id="ce5e7-200">Правила CSS возникают попарно, например `validation-summary-errors` и `validation-summary-valid`.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-200">The CSS rules come in pairs, such as `validation-summary-errors` and `validation-summary-valid`.</span></span> <span data-ttu-id="ce5e7-201">Эти пары позволяют определять правила выполняются оба условия: условие ошибки и условие «обычный» (без ошибок).</span><span class="sxs-lookup"><span data-stu-id="ce5e7-201">These pairs let you define rules for both conditions: an error condition and a "normal" (non-error) condition.</span></span> <span data-ttu-id="ce5e7-202">Важно понимать, что всегда отображается разметку для отображения ошибок, даже если ошибок нет.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-202">It's important to understand that the markup for the error display is always rendered, even if there are no errors.</span></span> <span data-ttu-id="ce5e7-203">Например, если страница содержит `Html.ValidationSummary` метод в разметке исходный код страницы будет содержать следующую разметку даже при первом запросе страницы:</span><span class="sxs-lookup"><span data-stu-id="ce5e7-203">For example, if a page has an `Html.ValidationSummary` method in the markup, the page source will contain the following markup even when the page is requested for the first time:</span></span>

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

<span data-ttu-id="ce5e7-204">Другими словами `Html.ValidationSummary` метод всегда отображает `<div>` элемент и списку, даже в том случае, если список ошибок является пустым.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-204">In other words, the `Html.ValidationSummary` method always renders a `<div>` element and a list, even if the error list is empty.</span></span> <span data-ttu-id="ce5e7-205">Аналогичным образом `Html.ValidationMessage` метод всегда отображает `<span>` элемент как заполнитель для ошибку отдельного поля, даже если нет ошибок.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-205">Similarly, the `Html.ValidationMessage` method always renders a `<span>` element as a placeholder for an individual field error, even if there is no error.</span></span>

<span data-ttu-id="ce5e7-206">В некоторых ситуациях с сообщением об ошибке может привести к странице, чтобы Перекомпоновка и может привести к элементов на странице для перемещения.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-206">In some situations, displaying an error message can cause the page to reflow and can cause elements on the page to move around.</span></span> <span data-ttu-id="ce5e7-207">Правила CSS, заканчивающиеся на `-valid` позволяют определить макет, который может помочь предотвратить возникновение этой проблемы.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-207">The CSS rules that end in `-valid` let you define a layout that can help prevent this problem.</span></span> <span data-ttu-id="ce5e7-208">Например, можно определить `field-validation-error` и `field-validation-valid` на оба имеют же фиксированный размер.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-208">For example, you can define `field-validation-error` and `field-validation-valid` to both have the same fixed size.</span></span> <span data-ttu-id="ce5e7-209">Таким образом, отображаемую область для поля является статическим и не изменит поток страниц, если отображается сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-209">That way, the display area for the field is static and won't change the page flow if an error message is displayed.</span></span>

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a><span data-ttu-id="ce5e7-210">Проверка данных, не поступать из пользователей</span><span class="sxs-lookup"><span data-stu-id="ce5e7-210">Validating Data That Doesn't Come Directly from Users</span></span>

<span data-ttu-id="ce5e7-211">Иногда необходимо проверить информацию непосредственно из формы HTML.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-211">Sometimes you have to validate information that doesn't come directly from an HTML form.</span></span> <span data-ttu-id="ce5e7-212">Типичным примером является страницей, когда значение передается в строке запроса, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="ce5e7-212">A typical example is a page where a value is passed in a query string, as in the following example:</span></span>

`http://server/myapp/EditClassInformation?classid=1022`

<span data-ttu-id="ce5e7-213">В этом случае необходимо убедиться, что значение, которое передается на страницу (здесь 1022 значение `classid`) является допустимым.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-213">In this case, you want to make sure that the value that's passed to the page (here, 1022 for the value of `classid`) is valid.</span></span> <span data-ttu-id="ce5e7-214">Нельзя напрямую использовать `Validation` вспомогательный метод для выполнения этой проверки.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-214">You can't directly use the `Validation` helper to perform this validation.</span></span> <span data-ttu-id="ce5e7-215">Тем не менее можно использовать другие функции проверки системы, такие как возможность отображать сообщения об ошибках проверки.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-215">However, you can use other features of the validation system, like the ability to display validation error messages.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="ce5e7-216">**Важные** всегда проверяйте все значения, получаемые из *любой* источника, включая значения поля формы, значения строки запроса и значений cookie.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-216">**Important** Always validate values that you get from *any* source, including form-field values, query-string values, and cookie values.</span></span> <span data-ttu-id="ce5e7-217">Он может легко изменять эти значения (возможно, для вредоносных целей).</span><span class="sxs-lookup"><span data-stu-id="ce5e7-217">It's easy for people to change these values (perhaps for malicious purposes).</span></span> <span data-ttu-id="ce5e7-218">Поэтому необходимо проверять эти значения для защиты приложения.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-218">So you must check these values in order to protect your application.</span></span>


<span data-ttu-id="ce5e7-219">В следующем примере показано, как может проверить значение, которое передается в строке запроса.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-219">The following example shows how you might validate a value that's passed in a query string.</span></span> <span data-ttu-id="ce5e7-220">В коде проверяется значение не указано, и что он является целым числом.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-220">The code tests that the value is not empty and that it's an integer.</span></span>

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

<span data-ttu-id="ce5e7-221">Обратите внимание, что тест выполняется при запросе отправки формы (`if(!IsPost)`).</span><span class="sxs-lookup"><span data-stu-id="ce5e7-221">Notice that the test is performed when the request is not a form submission (`if(!IsPost)`).</span></span> <span data-ttu-id="ce5e7-222">Этот тест будет передан при первом запросе страницы, но не в том случае, если запрос является отправки формы.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-222">This test would pass the first time that the page is requested, but not when the request is a form submission.</span></span>

<span data-ttu-id="ce5e7-223">Чтобы отобразить эту ошибку, можно добавить ошибки в список ошибок проверки путем вызова `Validation.AddFormError("message")`.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-223">To display this error, you can add the error to the list of validation errors by calling `Validation.AddFormError("message")`.</span></span> <span data-ttu-id="ce5e7-224">Если страница содержит вызов `Html.ValidationSummary` метод ошибку, отображается как ошибка проверки входных данных пользователя.</span><span class="sxs-lookup"><span data-stu-id="ce5e7-224">If the page contains a call to the `Html.ValidationSummary` method, the error is displayed there, just like a user-input validation error.</span></span>

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="ce5e7-225">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="ce5e7-225">Additional Resources</span></span>

[<span data-ttu-id="ce5e7-226">Работа с формами HTML веб-страниц сайтов ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ce5e7-226">Working with HTML Forms in ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkID=202892)
