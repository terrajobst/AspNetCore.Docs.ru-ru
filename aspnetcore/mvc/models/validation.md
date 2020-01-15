---
title: Проверка модели в ASP.NET Core MVC
author: rick-anderson
description: Сведения о проверке модели в ASP.NET Core MVC и Razor Pages.
ms.author: riande
ms.custom: mvc
ms.date: 12/15/2019
uid: mvc/models/validation
ms.openlocfilehash: 042a9933e561de4957f6332bdff3c4f09d2e119b
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75355265"
---
# <a name="model-validation-in-aspnet-core-mvc-and-razor-pages"></a><span data-ttu-id="7268e-103">Проверка модели в ASP.NET Core MVC и Razor Pages</span><span class="sxs-lookup"><span data-stu-id="7268e-103">Model validation in ASP.NET Core MVC and Razor Pages</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7268e-104">Автор: [Кирк Ларкин (Kirk Larkin)](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="7268e-104">By [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="7268e-105">В этой статье объясняется, как осуществлять проверки вводимых пользователем данных в приложении ASP.NET Core MVC или Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="7268e-105">This article explains how to validate user input in an ASP.NET Core MVC or Razor Pages app.</span></span>

<span data-ttu-id="7268e-106">[Просмотреть или скачать пример кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/samples) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="7268e-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="model-state"></a><span data-ttu-id="7268e-107">Состояние модели</span><span class="sxs-lookup"><span data-stu-id="7268e-107">Model state</span></span>

<span data-ttu-id="7268e-108">Состояние модели представляет ошибки, создаваемые двумя подсистемами: привязкой модели и проверкой модели.</span><span class="sxs-lookup"><span data-stu-id="7268e-108">Model state represents errors that come from two subsystems: model binding and model validation.</span></span> <span data-ttu-id="7268e-109">Ошибки [привязки модели](model-binding.md) обычно являются ошибками преобразования данных.</span><span class="sxs-lookup"><span data-stu-id="7268e-109">Errors that originate from [model binding](model-binding.md) are generally data conversion errors.</span></span> <span data-ttu-id="7268e-110">Например, в целочисленном поле указывается "x".</span><span class="sxs-lookup"><span data-stu-id="7268e-110">For example, an "x" is entered in an integer field.</span></span> <span data-ttu-id="7268e-111">Проверка модели происходит после ее привязки. В процессе сообщается об ошибках несоответствия данных бизнес-правилам.</span><span class="sxs-lookup"><span data-stu-id="7268e-111">Model validation occurs after model binding and reports errors where data doesn't conform to business rules.</span></span> <span data-ttu-id="7268e-112">Например, в поле, которое ожидает оценку от 1 до 5, указывается 0.</span><span class="sxs-lookup"><span data-stu-id="7268e-112">For example, a 0 is entered in a field that expects a rating between 1 and 5.</span></span>

<span data-ttu-id="7268e-113">Привязка и проверка модели происходят перед выполнением действия контроллера или метода обработчика Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="7268e-113">Both model binding and model validation occur before the execution of a controller action or a Razor Pages handler method.</span></span> <span data-ttu-id="7268e-114">Веб-приложение отвечает за проверку `ModelState.IsValid` и реагирует соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="7268e-114">For web apps, it's the app's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="7268e-115">Веб-приложения обычно повторно отображают страницы с сообщением об ошибке.</span><span class="sxs-lookup"><span data-stu-id="7268e-115">Web apps typically redisplay the page with an error message:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Pages/Movies/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=3-6)]

<span data-ttu-id="7268e-116">Контроллерам веб-API не нужно проверять `ModelState.IsValid` при наличии атрибута `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="7268e-116">Web API controllers don't have to check `ModelState.IsValid` if they have the `[ApiController]` attribute.</span></span> <span data-ttu-id="7268e-117">В этом случае автоматически возвращается ответ HTTP 400, содержащий сведения об ошибке, если состояние модели недопустимо.</span><span class="sxs-lookup"><span data-stu-id="7268e-117">In that case, an automatic HTTP 400 response containing error details is returned when model state is invalid.</span></span> <span data-ttu-id="7268e-118">Дополнительные сведения см. в разделе [Автоматические отклики HTTP 400](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="7268e-118">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="rerun-validation"></a><span data-ttu-id="7268e-119">Перезапуск проверки</span><span class="sxs-lookup"><span data-stu-id="7268e-119">Rerun validation</span></span>

<span data-ttu-id="7268e-120">Проверка выполняется автоматически, однако может потребоваться повторить ее вручную.</span><span class="sxs-lookup"><span data-stu-id="7268e-120">Validation is automatic, but you might want to repeat it manually.</span></span> <span data-ttu-id="7268e-121">Например, можно вычислить значение свойства и повторно выполнить проверку после установки свойства в вычисляемое значение.</span><span class="sxs-lookup"><span data-stu-id="7268e-121">For example, you might compute a value for a property and want to rerun validation after setting the property to the computed value.</span></span> <span data-ttu-id="7268e-122">Для повторной проверки вызовите метод `TryValidateModel`, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="7268e-122">To rerun validation, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Pages/Movies/Create.cshtml.cs?name=snippet_TryValidate&highlight=3-6)]

## <a name="validation-attributes"></a><span data-ttu-id="7268e-123">Атрибуты проверки</span><span class="sxs-lookup"><span data-stu-id="7268e-123">Validation attributes</span></span>

<span data-ttu-id="7268e-124">Атрибуты проверки позволяют задать правила проверки для свойств модели.</span><span class="sxs-lookup"><span data-stu-id="7268e-124">Validation attributes let you specify validation rules for model properties.</span></span> <span data-ttu-id="7268e-125">В следующем примере из примера приложения показан класс модели, который помечается с помощью атрибутов проверки.</span><span class="sxs-lookup"><span data-stu-id="7268e-125">The following example from the sample app shows a model class that is annotated with validation attributes.</span></span> <span data-ttu-id="7268e-126">`[ClassicMovie]` является настраиваемым атрибутом проверки; остальные являются встроенными.</span><span class="sxs-lookup"><span data-stu-id="7268e-126">The `[ClassicMovie]` attribute is a custom validation attribute and the others are built-in.</span></span> <span data-ttu-id="7268e-127">Не показан `[ClassicMovieWithClientValidator]`.</span><span class="sxs-lookup"><span data-stu-id="7268e-127">Not shown is `[ClassicMovieWithClientValidator]`.</span></span> <span data-ttu-id="7268e-128">`[ClassicMovieWithClientValidator]` демонстрирует альтернативный способ реализации настраиваемого атрибута.</span><span class="sxs-lookup"><span data-stu-id="7268e-128">`[ClassicMovieWithClientValidator]` shows an alternative way to implement a custom attribute.</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Models/Movie.cs?name=snippet_Class)]

## <a name="built-in-attributes"></a><span data-ttu-id="7268e-129">Встроенные атрибуты</span><span class="sxs-lookup"><span data-stu-id="7268e-129">Built-in attributes</span></span>

<span data-ttu-id="7268e-130">Ниже приведены некоторые из встроенных атрибутов проверки.</span><span class="sxs-lookup"><span data-stu-id="7268e-130">Here are some of the built-in validation attributes:</span></span>

* <span data-ttu-id="7268e-131">`[CreditCard]`. проверяет, имеет ли свойство формат кредитной карты.</span><span class="sxs-lookup"><span data-stu-id="7268e-131">`[CreditCard]`: Validates that the property has a credit card format.</span></span>
* <span data-ttu-id="7268e-132">`[Compare]`. проверяет, совпадают ли два свойства модели.</span><span class="sxs-lookup"><span data-stu-id="7268e-132">`[Compare]`: Validates that two properties in a model match.</span></span>
* <span data-ttu-id="7268e-133">`[EmailAddress]`. проверяет, имеет ли свойство формат адреса электронной почты.</span><span class="sxs-lookup"><span data-stu-id="7268e-133">`[EmailAddress]`: Validates that the property has an email format.</span></span>
* <span data-ttu-id="7268e-134">`[Phone]`. проверяет, имеет ли свойство формат номера телефона.</span><span class="sxs-lookup"><span data-stu-id="7268e-134">`[Phone]`: Validates that the property has a telephone number format.</span></span>
* <span data-ttu-id="7268e-135">`[Range]`. проверяет, находится ли значение свойства в указанном диапазоне.</span><span class="sxs-lookup"><span data-stu-id="7268e-135">`[Range]`: Validates that the property value falls within a specified range.</span></span>
* <span data-ttu-id="7268e-136">`[RegularExpression]`. проверяет, соответствует ли значение свойства указанному регулярному выражению.</span><span class="sxs-lookup"><span data-stu-id="7268e-136">`[RegularExpression]`: Validates that the property value matches a specified regular expression.</span></span>
* <span data-ttu-id="7268e-137">`[Required]`. проверяет, что поле не равно NULL.</span><span class="sxs-lookup"><span data-stu-id="7268e-137">`[Required]`: Validates that the field is not null.</span></span> <span data-ttu-id="7268e-138">Дополнительные сведения о поведении этого атрибута см. в разделе [Атрибут `[Required]`](#required-attribute).</span><span class="sxs-lookup"><span data-stu-id="7268e-138">See [`[Required]` attribute](#required-attribute) for details about this attribute's behavior.</span></span>
* <span data-ttu-id="7268e-139">`[StringLength]`. проверяет, что значение свойства строки не превышает ограничение по указанной длине.</span><span class="sxs-lookup"><span data-stu-id="7268e-139">`[StringLength]`: Validates that a string property value doesn't exceed a specified length limit.</span></span>
* <span data-ttu-id="7268e-140">`[Url]`. проверяет, имеет ли свойство формат URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="7268e-140">`[Url]`: Validates that the property has a URL format.</span></span>
* <span data-ttu-id="7268e-141">`[Remote]`. проверяет входные данные на клиенте путем вызова метода действия на сервере.</span><span class="sxs-lookup"><span data-stu-id="7268e-141">`[Remote]`: Validates input on the client by calling an action method on the server.</span></span> <span data-ttu-id="7268e-142">Дополнительные сведения о поведении этого атрибута см. в разделе [Атрибут `[Remote]`](#remote-attribute).</span><span class="sxs-lookup"><span data-stu-id="7268e-142">See [`[Remote]` attribute](#remote-attribute) for details about this attribute's behavior.</span></span>

<span data-ttu-id="7268e-143">Полный перечень атрибутов проверки можно найти в пространстве имен [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations).</span><span class="sxs-lookup"><span data-stu-id="7268e-143">A complete list of validation attributes can be found in the [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) namespace.</span></span>

### <a name="error-messages"></a><span data-ttu-id="7268e-144">Сообщения об ошибках</span><span class="sxs-lookup"><span data-stu-id="7268e-144">Error messages</span></span>

<span data-ttu-id="7268e-145">Атрибуты проверки позволяют указать сообщение об ошибке, которое будет отображаться, если входные данные недопустимы.</span><span class="sxs-lookup"><span data-stu-id="7268e-145">Validation attributes let you specify the error message to be displayed for invalid input.</span></span> <span data-ttu-id="7268e-146">Пример:</span><span class="sxs-lookup"><span data-stu-id="7268e-146">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

<span data-ttu-id="7268e-147">На внутреннем уровне атрибуты вызывают `String.Format` с заполнителем для имени поля и иногда дополнительным заполнителями.</span><span class="sxs-lookup"><span data-stu-id="7268e-147">Internally, the attributes call `String.Format` with a placeholder for the field name and sometimes additional placeholders.</span></span> <span data-ttu-id="7268e-148">Пример:</span><span class="sxs-lookup"><span data-stu-id="7268e-148">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

<span data-ttu-id="7268e-149">При применении к свойству `Name` сообщение об ошибке, созданное в приведенном выше коде, имело бы вид Name length must be between 6 and 8 (Длина имени должна быть от 6 до 8).</span><span class="sxs-lookup"><span data-stu-id="7268e-149">When applied to a `Name` property, the error message created by the preceding code would be "Name length must be between 6 and 8.".</span></span>

<span data-ttu-id="7268e-150">Чтобы узнать, какие параметры передаются в `String.Format` для сообщения об ошибке определенного атрибута, см. раздел [Исходный код DataAnnotations](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span><span class="sxs-lookup"><span data-stu-id="7268e-150">To find out which parameters are passed to `String.Format` for a particular attribute's error message, see the [DataAnnotations source code](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span></span>

## <a name="required-attribute"></a><span data-ttu-id="7268e-151">Атрибут [Required]</span><span class="sxs-lookup"><span data-stu-id="7268e-151">[Required] attribute</span></span>

<span data-ttu-id="7268e-152">По умолчанию система проверки рассматривает не допускающие значение NULL параметры или свойства так, как если бы они имели атрибут `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="7268e-152">By default, the validation system treats non-nullable parameters or properties as if they had a `[Required]` attribute.</span></span> <span data-ttu-id="7268e-153">[Типы значений](/dotnet/csharp/language-reference/keywords/value-types), например `decimal` и `int`, не поддерживают значение NULL.</span><span class="sxs-lookup"><span data-stu-id="7268e-153">[Value types](/dotnet/csharp/language-reference/keywords/value-types) such as `decimal` and `int` are non-nullable.</span></span>

### <a name="required-validation-on-the-server"></a><span data-ttu-id="7268e-154">Проверка [Required] на сервере</span><span class="sxs-lookup"><span data-stu-id="7268e-154">[Required] validation on the server</span></span>

<span data-ttu-id="7268e-155">На сервере обязательное значение считается отсутствующим, если свойство имеет значение NULL.</span><span class="sxs-lookup"><span data-stu-id="7268e-155">On the server, a required value is considered missing if the property is null.</span></span> <span data-ttu-id="7268e-156">Поле, не допускающее значения NULL, всегда является допустимым, и сообщение об ошибке атрибута `[Required]` никогда не выводится.</span><span class="sxs-lookup"><span data-stu-id="7268e-156">A non-nullable field is always valid, and the `[Required]` attribute's error message is never displayed.</span></span>

<span data-ttu-id="7268e-157">Тем не менее привязка модели для ненулевого свойства может завершиться ошибкой, приводящей к сообщению об ошибке, например `The value '' is invalid`.</span><span class="sxs-lookup"><span data-stu-id="7268e-157">However, model binding for a non-nullable property may fail, resulting in an error message such as `The value '' is invalid`.</span></span> <span data-ttu-id="7268e-158">Чтобы задать настраиваемое сообщение об ошибке во время проверки не допускающих значения NULL типов на стороне сервера, у вас есть следующие варианты.</span><span class="sxs-lookup"><span data-stu-id="7268e-158">To specify a custom error message for server-side validation of non-nullable types, you have the following options:</span></span>

* <span data-ttu-id="7268e-159">Сделать поле допускающим значение NULL (например, `decimal?` вместо `decimal`).</span><span class="sxs-lookup"><span data-stu-id="7268e-159">Make the field nullable (for example, `decimal?` instead of `decimal`).</span></span> <span data-ttu-id="7268e-160">Типы значений [Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) обрабатываются как стандартные типы, допускающие значение NULL.</span><span class="sxs-lookup"><span data-stu-id="7268e-160">[Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) value types are treated like standard nullable types.</span></span>
* <span data-ttu-id="7268e-161">Указать сообщение об ошибке по умолчанию для использования в привязке модели, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="7268e-161">Specify the default error message to be used by model binding, as shown in the following example:</span></span>

  [!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_Configuration&highlight=5-6)]

  <span data-ttu-id="7268e-162">Дополнительные сведения об ошибках привязки модели, у которых можно задать сообщение по умолчанию, см. в разделе <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span><span class="sxs-lookup"><span data-stu-id="7268e-162">For more information about model binding errors that you can set default messages for, see <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span></span>

### <a name="required-validation-on-the-client"></a><span data-ttu-id="7268e-163">Проверка [Required] на клиенте</span><span class="sxs-lookup"><span data-stu-id="7268e-163">[Required] validation on the client</span></span>

<span data-ttu-id="7268e-164">Типы и строки, не допускающие значение NULL, обрабатываются на клиенте не так, как на сервере.</span><span class="sxs-lookup"><span data-stu-id="7268e-164">Non-nullable types and strings are handled differently on the client compared to the server.</span></span> <span data-ttu-id="7268e-165">На клиенте:</span><span class="sxs-lookup"><span data-stu-id="7268e-165">On the client:</span></span>

* <span data-ttu-id="7268e-166">Значение считается присутствующим только в том случае, если для него вводятся данные.</span><span class="sxs-lookup"><span data-stu-id="7268e-166">A value is considered present only if input is entered for it.</span></span> <span data-ttu-id="7268e-167">Таким образом, проверка на стороне клиента обрабатывает типы, не допускающие значение NULL, так же, как обнуляемые типы.</span><span class="sxs-lookup"><span data-stu-id="7268e-167">Therefore, client-side validation handles non-nullable types the same as nullable types.</span></span>
* <span data-ttu-id="7268e-168">Пробелы в поле строки считаются допустимыми входными данными при проверке методом jQuery [required](https://jqueryvalidation.org/required-method/).</span><span class="sxs-lookup"><span data-stu-id="7268e-168">Whitespace in a string field is considered valid input by the jQuery Validation [required](https://jqueryvalidation.org/required-method/) method.</span></span> <span data-ttu-id="7268e-169">Проверка на стороне сервера считает, что обязательное строковое поле недопустимо, если введены только пробелы.</span><span class="sxs-lookup"><span data-stu-id="7268e-169">Server-side validation considers a required string field invalid if only whitespace is entered.</span></span>

<span data-ttu-id="7268e-170">Как отмечалось ранее, не допускающие значение NULL типы рассматриваются как имеющие атрибут `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="7268e-170">As noted earlier, non-nullable types are treated as though they had a `[Required]` attribute.</span></span> <span data-ttu-id="7268e-171">Это означает, что вы получаете проверку на стороне клиента, даже если не применять атрибут `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="7268e-171">That means you get client-side validation even if you don't apply the `[Required]` attribute.</span></span> <span data-ttu-id="7268e-172">Но если вы не используете атрибут, вы получаете сообщение об ошибке по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="7268e-172">But if you don't use the attribute, you get a default error message.</span></span> <span data-ttu-id="7268e-173">Чтобы задать настраиваемое сообщение об ошибке, используйте атрибут.</span><span class="sxs-lookup"><span data-stu-id="7268e-173">To specify a custom error message, use the attribute.</span></span>

## <a name="remote-attribute"></a><span data-ttu-id="7268e-174">Атрибут [Remote]</span><span class="sxs-lookup"><span data-stu-id="7268e-174">[Remote] attribute</span></span>

<span data-ttu-id="7268e-175">Атрибут `[Remote]` реализует проверку на стороне клиента, в ходе которой требуется вызвать метод на сервере, чтобы определить, является ли допустимым поле ввода.</span><span class="sxs-lookup"><span data-stu-id="7268e-175">The `[Remote]` attribute implements client-side validation that requires calling a method on the server to determine whether field input is valid.</span></span> <span data-ttu-id="7268e-176">Например, приложению может потребоваться проверить, занято ли имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="7268e-176">For example, the app may need to verify whether a user name is already in use.</span></span>

<span data-ttu-id="7268e-177">Для реализации удаленной проверки сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="7268e-177">To implement remote validation:</span></span>

1. <span data-ttu-id="7268e-178">Создайте для вызова из JavaScript метод действия.</span><span class="sxs-lookup"><span data-stu-id="7268e-178">Create an action method for JavaScript to call.</span></span>  <span data-ttu-id="7268e-179">Метод проверки jQuery [remote](https://jqueryvalidation.org/remote-method/) ожидает ответ JSON.</span><span class="sxs-lookup"><span data-stu-id="7268e-179">The jQuery Validate [remote](https://jqueryvalidation.org/remote-method/) method expects a JSON response:</span></span>

   * <span data-ttu-id="7268e-180">`true` означает, что входные данные допустимы.</span><span class="sxs-lookup"><span data-stu-id="7268e-180">`true` means the input data is valid.</span></span>
   * <span data-ttu-id="7268e-181">`false`, `undefined` или `null` означают, что входные данные недопустимы.</span><span class="sxs-lookup"><span data-stu-id="7268e-181">`false`, `undefined`, or `null` means the input is invalid.</span></span> <span data-ttu-id="7268e-182">Вывод стандартного сообщения об ошибке</span><span class="sxs-lookup"><span data-stu-id="7268e-182">Display the default error message.</span></span>
   * <span data-ttu-id="7268e-183">Все прочие значения означают, что входные данные недопустимы.</span><span class="sxs-lookup"><span data-stu-id="7268e-183">Any other string means the input is invalid.</span></span> <span data-ttu-id="7268e-184">Вывод строки как настраиваемого сообщения об ошибке.</span><span class="sxs-lookup"><span data-stu-id="7268e-184">Display the string as a custom error message.</span></span>

   <span data-ttu-id="7268e-185">Ниже приведен пример метода действия, который возвращает настраиваемое сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="7268e-185">Here's an example of an action method that returns a custom error message:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. <span data-ttu-id="7268e-186">В классе модели пометьте свойство атрибутом `[Remote]`, указывающим метод действия проверки, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="7268e-186">In the model class, annotate the property with a `[Remote]` attribute that points to the validation action method, as shown in the following example:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Models/User.cs?name=snippet_Email)]
 
   <span data-ttu-id="7268e-187">Атрибут `[Remote]` находится в пространстве имен `Microsoft.AspNetCore.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="7268e-187">The `[Remote]` attribute is in the `Microsoft.AspNetCore.Mvc` namespace.</span></span>
   
### <a name="additional-fields"></a><span data-ttu-id="7268e-188">Дополнительные поля</span><span class="sxs-lookup"><span data-stu-id="7268e-188">Additional fields</span></span>

<span data-ttu-id="7268e-189">Свойство `AdditionalFields` атрибута `[Remote]` позволяет проверять сочетания полей с данными на сервере.</span><span class="sxs-lookup"><span data-stu-id="7268e-189">The `AdditionalFields` property of the `[Remote]` attribute lets you validate combinations of fields against data on the server.</span></span> <span data-ttu-id="7268e-190">Например, если бы в модели `User` были свойства `FirstName` и `LastName`, могла бы возникнуть необходимость проверить, нет ли уже пользователя с такой парой имен.</span><span class="sxs-lookup"><span data-stu-id="7268e-190">For example, if the `User` model had `FirstName` and `LastName` properties, you might want to verify that no existing users already have that pair of names.</span></span> <span data-ttu-id="7268e-191">В следующем примере показано использование `AdditionalFields`.</span><span class="sxs-lookup"><span data-stu-id="7268e-191">The following example shows how to use `AdditionalFields`:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Models/User.cs?name=snippet_Name&highlight=1,5)]

<span data-ttu-id="7268e-192">`AdditionalFields` можно явно присвоить строкам "FirstName" и "LastName", но использование оператора [nameof](/dotnet/csharp/language-reference/keywords/nameof) упрощает дальнейший рефакторинг.</span><span class="sxs-lookup"><span data-stu-id="7268e-192">`AdditionalFields` could be set explicitly to the strings "FirstName" and "LastName", but using the [nameof](/dotnet/csharp/language-reference/keywords/nameof) operator simplifies later refactoring.</span></span> <span data-ttu-id="7268e-193">Методу действия для этой проверки необходимо принимать аргументы `firstName` и `lastName`</span><span class="sxs-lookup"><span data-stu-id="7268e-193">The action method for this validation must accept both `firstName` and `lastName` arguments:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyName)]

<span data-ttu-id="7268e-194">Когда пользователь вводит имя или фамилию, JavaScript выполняет удаленный вызов, чтобы увидеть, будет ли эта пара принята.</span><span class="sxs-lookup"><span data-stu-id="7268e-194">When the user enters a first or last name, JavaScript makes a remote call to see if that pair of names has been taken.</span></span>

<span data-ttu-id="7268e-195">Чтобы проверить несколько дополнительных полей, их следует указывать в виде списка с разделителями-запятыми.</span><span class="sxs-lookup"><span data-stu-id="7268e-195">To validate two or more additional fields, provide them as a comma-delimited list.</span></span> <span data-ttu-id="7268e-196">Например, чтобы добавить в модель свойство `MiddleName`, задайте атрибут `[Remote]`, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="7268e-196">For example, to add a `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following example:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="7268e-197">`AdditionalFields`, как и все аргументы атрибутов, должен представлять собой константное выражение.</span><span class="sxs-lookup"><span data-stu-id="7268e-197">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span> <span data-ttu-id="7268e-198">Поэтому не следует использовать [интерполированную строку](/dotnet/csharp/language-reference/keywords/interpolated-strings) или вызов <xref:System.String.Join*>для инициализации `AdditionalFields`.</span><span class="sxs-lookup"><span data-stu-id="7268e-198">Therefore, don't use an [interpolated string](/dotnet/csharp/language-reference/keywords/interpolated-strings) or call <xref:System.String.Join*> to initialize `AdditionalFields`.</span></span>

## <a name="alternatives-to-built-in-attributes"></a><span data-ttu-id="7268e-199">Альтернативы для встроенных атрибутов</span><span class="sxs-lookup"><span data-stu-id="7268e-199">Alternatives to built-in attributes</span></span>

<span data-ttu-id="7268e-200">Если вам нужна проверка, которую не предоставляют встроенные атрибуты, вы можете следующее.</span><span class="sxs-lookup"><span data-stu-id="7268e-200">If you need validation not provided by built-in attributes, you can:</span></span>

* <span data-ttu-id="7268e-201">[Создать настраиваемые атрибуты](#custom-attributes).</span><span class="sxs-lookup"><span data-stu-id="7268e-201">[Create custom attributes](#custom-attributes).</span></span>
* <span data-ttu-id="7268e-202">[Реализовать IValidatableObject](#ivalidatableobject).</span><span class="sxs-lookup"><span data-stu-id="7268e-202">[Implement IValidatableObject](#ivalidatableobject).</span></span>

## <a name="custom-attributes"></a><span data-ttu-id="7268e-203">Настраиваемые атрибуты</span><span class="sxs-lookup"><span data-stu-id="7268e-203">Custom attributes</span></span>

<span data-ttu-id="7268e-204">Для сценариев, где не годятся встроенные атрибуты проверки, можно создать настраиваемые атрибуты.</span><span class="sxs-lookup"><span data-stu-id="7268e-204">For scenarios that the built-in validation attributes don't handle, you can create custom validation attributes.</span></span> <span data-ttu-id="7268e-205">Создайте класс, наследуемый от <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, и переопределите метод <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*>.</span><span class="sxs-lookup"><span data-stu-id="7268e-205">Create a class that inherits from <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, and override the <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> method.</span></span>

<span data-ttu-id="7268e-206">Метод `IsValid` принимает объект с именем *value*, который является входными данными для проверки.</span><span class="sxs-lookup"><span data-stu-id="7268e-206">The `IsValid` method accepts an object named *value*, which is the input to be validated.</span></span> <span data-ttu-id="7268e-207">Перегрузка также принимает объект `ValidationContext`, который предоставляет дополнительные сведения, такие как экземпляр модели, созданный с помощью привязки модели.</span><span class="sxs-lookup"><span data-stu-id="7268e-207">An overload also accepts a `ValidationContext` object, which provides additional information, such as the model instance created by model binding.</span></span>

<span data-ttu-id="7268e-208">В следующем примере проверяется, что дата выпуска фильмов в *классическом* жанре задана не позднее указанного года.</span><span class="sxs-lookup"><span data-stu-id="7268e-208">The following example validates that the release date for a movie in the *Classic* genre isn't later than a specified year.</span></span> <span data-ttu-id="7268e-209">Атрибут `[ClassicMovie]`:</span><span class="sxs-lookup"><span data-stu-id="7268e-209">The `[ClassicMovie]` attribute:</span></span>

* <span data-ttu-id="7268e-210">выполняется только на сервере.</span><span class="sxs-lookup"><span data-stu-id="7268e-210">Is only run on the server.</span></span>
* <span data-ttu-id="7268e-211">Для классических фильмов проверяет дату выпуска:</span><span class="sxs-lookup"><span data-stu-id="7268e-211">For Classic movies, validates the release date:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Validation/ClassicMovieAttribute.cs?name=snippet_Class)]

<span data-ttu-id="7268e-212">Приведенная выше переменная `movie` представляет объект `Movie`, который содержит данные из переданной формы.</span><span class="sxs-lookup"><span data-stu-id="7268e-212">The `movie` variable in the preceding example represents a `Movie` object that contains the data from the form submission.</span></span> <span data-ttu-id="7268e-213">Если проверка завершается неудачно, возвращается `ValidationResult` с сообщением об ошибке.</span><span class="sxs-lookup"><span data-stu-id="7268e-213">When validation fails, a `ValidationResult` with an error message is returned.</span></span>

## <a name="ivalidatableobject"></a><span data-ttu-id="7268e-214">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="7268e-214">IValidatableObject</span></span>

<span data-ttu-id="7268e-215">Предыдущий пример работает только с типами `Movie`.</span><span class="sxs-lookup"><span data-stu-id="7268e-215">The preceding example works only with `Movie` types.</span></span> <span data-ttu-id="7268e-216">Другой вариант для проверки на уровне класса — реализация `IValidatableObject` в классе модели, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="7268e-216">Another option for class-level validation is to implement `IValidatableObject` in the model class, as shown in the following example:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Models/ValidatableMovie.cs?name=snippet_Class&highlight=1,26-34)]

## <a name="top-level-node-validation"></a><span data-ttu-id="7268e-217">Проверка узлов верхнего уровня</span><span class="sxs-lookup"><span data-stu-id="7268e-217">Top-level node validation</span></span>

<span data-ttu-id="7268e-218">Узлы верхнего уровня содержат:</span><span class="sxs-lookup"><span data-stu-id="7268e-218">Top-level nodes include:</span></span>

* <span data-ttu-id="7268e-219">параметры действия;</span><span class="sxs-lookup"><span data-stu-id="7268e-219">Action parameters</span></span>
* <span data-ttu-id="7268e-220">свойства контроллера;</span><span class="sxs-lookup"><span data-stu-id="7268e-220">Controller properties</span></span>
* <span data-ttu-id="7268e-221">параметры обработчика страниц;</span><span class="sxs-lookup"><span data-stu-id="7268e-221">Page handler parameters</span></span>
* <span data-ttu-id="7268e-222">свойства страничной модели.</span><span class="sxs-lookup"><span data-stu-id="7268e-222">Page model properties</span></span>

<span data-ttu-id="7268e-223">Проверка привязанных к модели узлов верхнего уровня осуществляется наряду с проверкой свойств модели.</span><span class="sxs-lookup"><span data-stu-id="7268e-223">Model-bound top-level nodes are validated in addition to validating model properties.</span></span> <span data-ttu-id="7268e-224">В следующем примере, взятом из примера приложения, метод `VerifyPhone` использует класс <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> для проверки параметра действия `phone`.</span><span class="sxs-lookup"><span data-stu-id="7268e-224">In the following example from the sample app, the `VerifyPhone` method uses the <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> to validate the `phone` action parameter:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

<span data-ttu-id="7268e-225">Узлы верхнего уровня могут применять класс <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> с атрибутами проверки.</span><span class="sxs-lookup"><span data-stu-id="7268e-225">Top-level nodes can use <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> with validation attributes.</span></span> <span data-ttu-id="7268e-226">В следующем примере, взятом из примера приложения, метод `CheckAge` указывает, что при отправке формы параметр `age` должен быть привязан из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="7268e-226">In the following example from the sample app, the `CheckAge` method specifies that the `age` parameter must be bound from the query string when the form is submitted:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_CheckAgeSignature)]

<span data-ttu-id="7268e-227">На странице "Check Age" (Проверка возраста) (*CheckAge.cshtml*) находятся две формы.</span><span class="sxs-lookup"><span data-stu-id="7268e-227">In the Check Age page (*CheckAge.cshtml*), there are two forms.</span></span> <span data-ttu-id="7268e-228">Первая форма отправляет значение `Age`, равное `99`, в виде параметра строки запроса: `https://localhost:5001/Users/CheckAge?Age=99`.</span><span class="sxs-lookup"><span data-stu-id="7268e-228">The first form submits an `Age` value of `99` as a query string parameter: `https://localhost:5001/Users/CheckAge?Age=99`.</span></span>

<span data-ttu-id="7268e-229">Если из строки запроса отправлен параметр `age` в правильном формате, форма проходит проверку.</span><span class="sxs-lookup"><span data-stu-id="7268e-229">When a properly formatted `age` parameter from the query string is submitted, the form validates.</span></span>

<span data-ttu-id="7268e-230">Вторая форма на странице "Check Age" (Проверка возраста) отправляет значение `Age` в теле запроса, и проверка завершается сбоем.</span><span class="sxs-lookup"><span data-stu-id="7268e-230">The second form on the Check Age page submits the `Age` value in the body of the request, and validation fails.</span></span> <span data-ttu-id="7268e-231">Ошибка привязки связана с тем, что параметр `age` должен поступать из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="7268e-231">Binding fails because the `age` parameter must come from a query string.</span></span>

## <a name="maximum-errors"></a><span data-ttu-id="7268e-232">Максимальное количество ошибок</span><span class="sxs-lookup"><span data-stu-id="7268e-232">Maximum errors</span></span>

<span data-ttu-id="7268e-233">При достижении максимального количества ошибок (по умолчанию 200) проверка прекращается.</span><span class="sxs-lookup"><span data-stu-id="7268e-233">Validation stops when the maximum number of errors is reached (200 by default).</span></span> <span data-ttu-id="7268e-234">Это число можно изменить с помощью следующего кода в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7268e-234">You can configure this number with the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_Configuration&highlight=4)]

## <a name="maximum-recursion"></a><span data-ttu-id="7268e-235">Максимальная рекурсия</span><span class="sxs-lookup"><span data-stu-id="7268e-235">Maximum recursion</span></span>

<span data-ttu-id="7268e-236"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> проходит через граф объектов в проверяемой модели.</span><span class="sxs-lookup"><span data-stu-id="7268e-236"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> traverses the object graph of the model being validated.</span></span> <span data-ttu-id="7268e-237">У глубоких моделей, содержащих бесконечную рекурсию, в ходе проверки может произойти переполнение стека.</span><span class="sxs-lookup"><span data-stu-id="7268e-237">For models that are deep or are infinitely recursive, validation may result in stack overflow.</span></span> <span data-ttu-id="7268e-238">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) предоставляет способ остановить проверку до превышения настроенной глубины рекурсии обхода.</span><span class="sxs-lookup"><span data-stu-id="7268e-238">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) provides a way to stop validation early if the visitor recursion exceeds a configured depth.</span></span> <span data-ttu-id="7268e-239">Значение `MvcOptions.MaxValidationDepth` по умолчанию —32.</span><span class="sxs-lookup"><span data-stu-id="7268e-239">The default value of `MvcOptions.MaxValidationDepth` is 32.</span></span>

## <a name="automatic-short-circuit"></a><span data-ttu-id="7268e-240">Автоматическое сокращение</span><span class="sxs-lookup"><span data-stu-id="7268e-240">Automatic short-circuit</span></span>

<span data-ttu-id="7268e-241">Проверка автоматически сокращается (пропускается), если граф модели не требует проверки.</span><span class="sxs-lookup"><span data-stu-id="7268e-241">Validation is automatically short-circuited (skipped) if the model graph doesn't require validation.</span></span> <span data-ttu-id="7268e-242">К числу объектов, которые среда выполнения пропускает при проверке, относятся коллекции примитивов (такие как `byte[]`, `string[]`, `Dictionary<string, string>`) и сложные графы объектов, которые не имеют проверяющих элементов управления.</span><span class="sxs-lookup"><span data-stu-id="7268e-242">Objects that the runtime skips validation for include collections of primitives (such as `byte[]`, `string[]`, `Dictionary<string, string>`) and complex object graphs that don't have any validators.</span></span>

## <a name="disable-validation"></a><span data-ttu-id="7268e-243">Отключение проверки</span><span class="sxs-lookup"><span data-stu-id="7268e-243">Disable validation</span></span>

<span data-ttu-id="7268e-244">Чтобы отключить проверку, сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="7268e-244">To disable validation:</span></span>

1. <span data-ttu-id="7268e-245">Создайте реализацию интерфейса `IObjectModelValidator`, которая не помечает поля как недопустимые.</span><span class="sxs-lookup"><span data-stu-id="7268e-245">Create an implementation of `IObjectModelValidator` that doesn't mark any fields as invalid.</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/NullObjectModelValidator.cs?name=snippet_Class)]

1. <span data-ttu-id="7268e-246">Добавьте следующий код в `Startup.ConfigureServices` для замены реализации `IObjectModelValidator` по умолчанию в контейнере внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="7268e-246">Add the following code to `Startup.ConfigureServices` to replace the default `IObjectModelValidator` implementation in the dependency injection container.</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_DisableValidation)]

<span data-ttu-id="7268e-247">По-прежнему могут отображаться ошибки состояния модели, которые идут из привязки модели.</span><span class="sxs-lookup"><span data-stu-id="7268e-247">You might still see model state errors that originate from model binding.</span></span>

## <a name="client-side-validation"></a><span data-ttu-id="7268e-248">Проверка на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="7268e-248">Client-side validation</span></span>

<span data-ttu-id="7268e-249">Проверка на стороне клиента не позволяет отправлять форму, пока ее данные не будут допустимыми.</span><span class="sxs-lookup"><span data-stu-id="7268e-249">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="7268e-250">При нажатии кнопки "Отправить" выполняется код JavaScript, который либо отправляет форму, либо выводит сообщения об ошибках.</span><span class="sxs-lookup"><span data-stu-id="7268e-250">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="7268e-251">Проверка на стороне клиента позволяет избежать ненужного кругового захода на сервер при наличии ошибки ввода в форме.</span><span class="sxs-lookup"><span data-stu-id="7268e-251">Client-side validation avoids an unnecessary round trip to the server when there are input errors on a form.</span></span> <span data-ttu-id="7268e-252">Ссылки на следующий скрипт в *_Layout.cshtml* и *_ValidationScriptsPartial.cshtml* поддерживают проверку на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="7268e-252">The following script references in *_Layout.cshtml* and *_ValidationScriptsPartial.cshtml* support client-side validation:</span></span>

[!code-cshtml[](validation/samples/3.x/ValidationSample/Views/Shared/_Layout.cshtml?name=snippet_Scripts)]

[!code-cshtml[](validation/samples/3.x/ValidationSample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_Scripts)]

<span data-ttu-id="7268e-253">Скрипт [ненавязчивой проверки jQuery](https://github.com/aspnet/jquery-validation-unobtrusive) — это настраиваемая интерфейсная библиотека Майкрософт, которая основана на популярном подключаемом модуле [jQuery Validate](https://jqueryvalidation.org/).</span><span class="sxs-lookup"><span data-stu-id="7268e-253">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="7268e-254">Без скрипта ненавязчивой проверки jQuery одну и ту же логику проверки приходилось бы реализовывать в двух местах: в атрибутах проверки для свойств модели на стороне сервера, а затем еще раз в скриптах на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="7268e-254">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server-side validation attributes on model properties, and then again in client-side scripts.</span></span> <span data-ttu-id="7268e-255">Вместо этого [вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) и [вспомогательные функции HTML](xref:mvc/views/overview) могут использовать атрибуты проверки и метаданные типов из свойств модели для обработки атрибутов `data-` HTML 5 в элементах форм, требующих проверки.</span><span class="sxs-lookup"><span data-stu-id="7268e-255">Instead, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use the validation attributes and type metadata from model properties to render HTML 5 `data-` attributes for the form elements that need validation.</span></span> <span data-ttu-id="7268e-256">Скрипт ненавязчивой проверки jQuery анализирует эти атрибуты `data-` и передает логику в подключаемый модуль jQuery Validate, по сути копируя логику проверки на стороне сервера в клиент.</span><span class="sxs-lookup"><span data-stu-id="7268e-256">jQuery Unobtrusive Validation parses the `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server-side validation logic to the client.</span></span> <span data-ttu-id="7268e-257">Ошибки проверки могут выводиться в клиенте с помощью вспомогательных функций тегов, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="7268e-257">You can display validation errors on the client using tag helpers as shown here:</span></span>

[!code-cshtml[](validation/samples/3.x/ValidationSample/Pages/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=3-4)]

<span data-ttu-id="7268e-258">Приведенные выше вспомогательные функции тегов отрисовывают следующий код HTML:</span><span class="sxs-lookup"><span data-stu-id="7268e-258">The preceding tag helpers render the following HTML:</span></span>

```html
<div class="form-group">
    <label class="control-label" for="Movie_ReleaseDate">Release Date</label>
    <input class="form-control" type="date" data-val="true"
        data-val-required="The Release Date field is required."
        id="Movie_ReleaseDate" name="Movie.ReleaseDate" value="">
    <span class="text-danger field-validation-valid"
        data-valmsg-for="Movie.ReleaseDate" data-valmsg-replace="true"></span>
</div>
```

<span data-ttu-id="7268e-259">Обратите внимание на то, что атрибуты `data-` в выходных данных HTML соответствуют атрибутам проверки для свойства `Movie.ReleaseDate`.</span><span class="sxs-lookup"><span data-stu-id="7268e-259">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `Movie.ReleaseDate` property.</span></span> <span data-ttu-id="7268e-260">Атрибут `data-val-required` содержит сообщение об ошибке, которое выводится, если пользователь не заполнил поле даты выхода.</span><span class="sxs-lookup"><span data-stu-id="7268e-260">The `data-val-required` attribute contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="7268e-261">Скрипт ненавязчивой проверки jQuery передает это значение в метод [required()](https://jqueryvalidation.org/required-method/) подключаемого модуля jQuery Validate, который затем выводит это сообщение в соответствующем элементе **\<span>** .</span><span class="sxs-lookup"><span data-stu-id="7268e-261">jQuery Unobtrusive Validation passes this value to the jQuery Validate [required()](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

<span data-ttu-id="7268e-262">Проверка типа данных основана на типе свойства в .NET, если его не переопределяет атрибут `[DataType]`.</span><span class="sxs-lookup"><span data-stu-id="7268e-262">Data type validation is based on the .NET type of a property, unless that is overridden by a `[DataType]` attribute.</span></span> <span data-ttu-id="7268e-263">Браузеры имеют свои сообщения об по умолчанию, но пакет ненавязчивой проверки jQuery может переопределять эти сообщения.</span><span class="sxs-lookup"><span data-stu-id="7268e-263">Browsers have their own default error messages, but the jQuery Validation Unobtrusive Validation package can override those messages.</span></span> <span data-ttu-id="7268e-264">Атрибуты и подклассы `[DataType]`, такие как `[EmailAddress]`, позволяют указать сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="7268e-264">`[DataType]` attributes and subclasses such as `[EmailAddress]` let you specify the error message.</span></span>

## <a name="unobtrusive-validation"></a><span data-ttu-id="7268e-265">Ненавязчивая проверка</span><span class="sxs-lookup"><span data-stu-id="7268e-265">Unobtrusive validation</span></span>

<span data-ttu-id="7268e-266">См. сведения о ненавязчивой проверке в этой [проблеме GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/1111).</span><span class="sxs-lookup"><span data-stu-id="7268e-266">For information on unobtrusive validation, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/1111).</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="7268e-267">Добавление проверки к динамическим формам</span><span class="sxs-lookup"><span data-stu-id="7268e-267">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="7268e-268">Скрипт ненавязчивой проверки jQuery передает логику и параметры проверки в подключаемый модуль jQuery Validate при первой загрузке страницы.</span><span class="sxs-lookup"><span data-stu-id="7268e-268">jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads.</span></span> <span data-ttu-id="7268e-269">Поэтому динамически создаваемые формы не подвергаются проверке автоматически.</span><span class="sxs-lookup"><span data-stu-id="7268e-269">Therefore, validation doesn't work automatically on dynamically generated forms.</span></span> <span data-ttu-id="7268e-270">Чтобы включить проверку, необходимо указать, что скрипт ненавязчивой проверки jQuery должен анализировать динамическую форму сразу после ее создания.</span><span class="sxs-lookup"><span data-stu-id="7268e-270">To enable validation, tell jQuery Unobtrusive Validation to parse the dynamic form immediately after you create it.</span></span> <span data-ttu-id="7268e-271">Например, приведенный ниже код показывает, как можно настроить проверку на стороне клиента для формы, добавленной посредством AJAX.</span><span class="sxs-lookup"><span data-stu-id="7268e-271">For example, the following code sets up client-side validation on a form added via AJAX.</span></span>

```js
$.get({
    url: "https://url/that/returns/a/form",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add form. " + errorThrown);
    },
    success: function(newFormHTML) {
        var container = document.getElementById("form-container");
        container.insertAdjacentHTML("beforeend", newFormHTML);
        var forms = container.getElementsByTagName("form");
        var newForm = forms[forms.length - 1];
        $.validator.unobtrusive.parse(newForm);
    }
})
```

<span data-ttu-id="7268e-272">Метод `$.validator.unobtrusive.parse()` принимает селектор jQuery в качестве единственного аргумента.</span><span class="sxs-lookup"><span data-stu-id="7268e-272">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="7268e-273">Этот метод предписывает скрипту ненавязчивой проверки jQuery анализировать атрибуты `data-` форм в этом селекторе.</span><span class="sxs-lookup"><span data-stu-id="7268e-273">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="7268e-274">Значения этих атрибутов затем передаются в подключаемый модуль jQuery Validate.</span><span class="sxs-lookup"><span data-stu-id="7268e-274">The values of those attributes are then passed to the jQuery Validate plugin.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="7268e-275">Добавление проверки к динамическим элементам управления</span><span class="sxs-lookup"><span data-stu-id="7268e-275">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="7268e-276">Метод `$.validator.unobtrusive.parse()` обрабатывает всю форму, а не отдельные динамически создаваемые элементы управления, такие как `<input>` и `<select/>`.</span><span class="sxs-lookup"><span data-stu-id="7268e-276">The `$.validator.unobtrusive.parse()` method works on an entire form, not on individual dynamically generated controls, such as `<input>` and `<select/>`.</span></span> <span data-ttu-id="7268e-277">Для повторной обработки формы удалите данные проверки, которые были добавлены при анализе формы ранее, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="7268e-277">To reparse the form, remove the validation data that was added when the form was parsed earlier, as shown in the following example:</span></span>

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add control. " + errorThrown);
    },
    success: function(newInputHTML) {
        var form = document.getElementById("my-form");
        form.insertAdjacentHTML("beforeend", newInputHTML);
        $(form).removeData("validator")    // Added by jQuery Validate
               .removeData("unobtrusiveValidation");   // Added by jQuery Unobtrusive Validation
        $.validator.unobtrusive.parse(form);
    }
})
```

## <a name="custom-client-side-validation"></a><span data-ttu-id="7268e-278">Настраиваемая проверка на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="7268e-278">Custom client-side validation</span></span>

<span data-ttu-id="7268e-279">Настраиваемая проверка на стороне клиента выполняется путем создания атрибутов HTML `data-`, которые работают с настраиваемым адаптером проверки jQuery.</span><span class="sxs-lookup"><span data-stu-id="7268e-279">Custom client-side validation is done by generating `data-` HTML attributes that work with a custom jQuery Validate adapter.</span></span> <span data-ttu-id="7268e-280">В следующем примере кода адаптера используются атрибуты `[ClassicMovie]` и `[ClassicMovieWithClientValidator]`, которые были введены ранее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="7268e-280">The following sample adapter code was written for the `[ClassicMovie]` and `[ClassicMovieWithClientValidator]` attributes that were introduced earlier in this article:</span></span>

[!code-javascript[](validation/samples/3.x/ValidationSample/wwwroot/js/classicMovieValidator.js)]

<span data-ttu-id="7268e-281">Сведения о способах создания адаптеров см. в [документации по проверкам jQuery](https://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="7268e-281">For information about how to write adapters, see the [jQuery Validate documentation](https://jqueryvalidation.org/documentation/).</span></span>

<span data-ttu-id="7268e-282">Использование адаптера для заданного поля инициируется атрибутами `data-`, которые:</span><span class="sxs-lookup"><span data-stu-id="7268e-282">The use of an adapter for a given field is triggered by `data-` attributes that:</span></span>

* <span data-ttu-id="7268e-283">помечают поле как проходящее проверку (`data-val="true"`);</span><span class="sxs-lookup"><span data-stu-id="7268e-283">Flag the field as being subject to validation (`data-val="true"`).</span></span>
* <span data-ttu-id="7268e-284">указывают имя правила проверки и текст сообщения об ошибке (например, `data-val-rulename="Error message."`);</span><span class="sxs-lookup"><span data-stu-id="7268e-284">Identify a validation rule name and error message text (for example, `data-val-rulename="Error message."`).</span></span>
* <span data-ttu-id="7268e-285">указывают любые дополнительные параметры, в которых нуждается проверяющий элемент управления (например, `data-val-rulename-param1="value"`).</span><span class="sxs-lookup"><span data-stu-id="7268e-285">Provide any additional parameters the validator needs (for example, `data-val-rulename-param1="value"`).</span></span>

<span data-ttu-id="7268e-286">В следующем примере показаны атрибуты `data-` для нашего атрибута `ClassicMovie` из примера приложения.</span><span class="sxs-lookup"><span data-stu-id="7268e-286">The following example shows the `data-` attributes for the sample app's `ClassicMovie` attribute:</span></span>

```html
<input class="form-control" type="date"
    data-val="true"
    data-val-classicmovie="Classic movies must have a release year no later than 1960."
    data-val-classicmovie-year="1960"
    data-val-required="The Release Date field is required."
    id="Movie_ReleaseDate" name="Movie.ReleaseDate" value="">
```

<span data-ttu-id="7268e-287">Как отмечалось ранее, [вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) и [вспомогательные методы HTML](xref:mvc/views/overview) используют сведения из атрибутов проверки для подготовки к отрисовке атрибутов `data-`.</span><span class="sxs-lookup"><span data-stu-id="7268e-287">As noted earlier, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use information from validation attributes to render `data-` attributes.</span></span> <span data-ttu-id="7268e-288">Существует два варианта для написания кода, который приводит к созданию настраиваемых атрибутов HTML `data-`.</span><span class="sxs-lookup"><span data-stu-id="7268e-288">There are two options for writing code that results in the creation of custom `data-` HTML attributes:</span></span>

* <span data-ttu-id="7268e-289">Создайте класс, производный от `AttributeAdapterBase<TAttribute>`, и класс, реализующий `IValidationAttributeAdapterProvider`; зарегистрируйте атрибут и его адаптер в функции внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="7268e-289">Create a class that derives from `AttributeAdapterBase<TAttribute>` and a class that implements `IValidationAttributeAdapterProvider`, and register your attribute and its adapter in DI.</span></span> <span data-ttu-id="7268e-290">Этот метод следует за [участником с единственной обязанностью](https://wikipedia.org/wiki/Single_responsibility_principle) в том, что код проверки, связанный с сервером и клиентом, выделен в отдельные классы.</span><span class="sxs-lookup"><span data-stu-id="7268e-290">This method follows the [single responsibility principal](https://wikipedia.org/wiki/Single_responsibility_principle) in that server-related and client-related validation code is in separate classes.</span></span> <span data-ttu-id="7268e-291">Адаптер также имеет следующее преимущество: так как он зарегистрирован в функции внедрения зависимостей, для него при необходимости доступны другие службы в ней.</span><span class="sxs-lookup"><span data-stu-id="7268e-291">The adapter also has the advantage that since it is registered in DI, other services in DI are available to it if needed.</span></span>
* <span data-ttu-id="7268e-292">Реализуйте `IClientModelValidator` в вашем классе `ValidationAttribute`.</span><span class="sxs-lookup"><span data-stu-id="7268e-292">Implement `IClientModelValidator` in your `ValidationAttribute` class.</span></span> <span data-ttu-id="7268e-293">Этот метод стоит использовать, если атрибут не выполняет проверку на стороне сервера и не нуждается в службах из функции внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="7268e-293">This method might be appropriate if the attribute doesn't do any server-side validation and doesn't need any services from DI.</span></span>

### <a name="attributeadapter-for-client-side-validation"></a><span data-ttu-id="7268e-294">AttributeAdapter для проверки на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="7268e-294">AttributeAdapter for client-side validation</span></span>

<span data-ttu-id="7268e-295">Этот метод отрисовки атрибутов `data-` в формате HTML используется атрибутом `ClassicMovie` в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="7268e-295">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie` attribute in the sample app.</span></span> <span data-ttu-id="7268e-296">Чтобы добавить клиентскую проверку с помощью этого метода, сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="7268e-296">To add client validation by using this method:</span></span>

1. <span data-ttu-id="7268e-297">Создайте класс адаптера атрибута для настраиваемого атрибута проверки.</span><span class="sxs-lookup"><span data-stu-id="7268e-297">Create an attribute adapter class for the custom validation attribute.</span></span> <span data-ttu-id="7268e-298">Наследуйте класс от [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="7268e-298">Derive the class from [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span></span> <span data-ttu-id="7268e-299">Создайте метод `AddValidation`, который добавляет атрибуты `data-` для выводимых данных, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="7268e-299">Create an `AddValidation` method that adds `data-` attributes to the rendered output, as shown in this example:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/ClassicMovieAttributeAdapter.cs?name=snippet_Class)]

1. <span data-ttu-id="7268e-300">Создайте класс поставщика адаптера, который реализует <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span><span class="sxs-lookup"><span data-stu-id="7268e-300">Create an adapter provider class that implements <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span></span> <span data-ttu-id="7268e-301">В методе `GetAttributeAdapter` передайте настраиваемый атрибут в конструктор адаптера, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="7268e-301">In the `GetAttributeAdapter` method pass in the custom attribute to the adapter's constructor, as shown in this example:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/CustomValidationAttributeAdapterProvider.cs?name=snippet_Class)]

1. <span data-ttu-id="7268e-302">Зарегистрируйте поставщик адаптера для внедрения зависимостей в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7268e-302">Register the adapter provider for DI in `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_Configuration&highlight=9-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a><span data-ttu-id="7268e-303">IClientModelValidator для проверки на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="7268e-303">IClientModelValidator for client-side validation</span></span>

<span data-ttu-id="7268e-304">Этот метод отрисовки атрибутов `data-` в формате HTML используется атрибутом `ClassicMovieWithClientValidator` в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="7268e-304">This method of rendering `data-` attributes in HTML is used by the `ClassicMovieWithClientValidator` attribute in the sample app.</span></span> <span data-ttu-id="7268e-305">Чтобы добавить клиентскую проверку с помощью этого метода, сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="7268e-305">To add client validation by using this method:</span></span>

* <span data-ttu-id="7268e-306">В настраиваемом атрибуте проверки реализуйте интерфейс `IClientModelValidator` и создайте метод `AddValidation`.</span><span class="sxs-lookup"><span data-stu-id="7268e-306">In the custom validation attribute, implement the `IClientModelValidator` interface and create an `AddValidation` method.</span></span> <span data-ttu-id="7268e-307">В метод `AddValidation` добавьте атрибуты `data-` для проверки, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="7268e-307">In the `AddValidation` method, add `data-` attributes for validation, as shown in the following example:</span></span>

  [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/ClassicMovieWithClientValidatorAttribute.cs?name=snippet_Class)]

## <a name="disable-client-side-validation"></a><span data-ttu-id="7268e-308">Отключение проверки на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="7268e-308">Disable client-side validation</span></span>

<span data-ttu-id="7268e-309">Следующий код отключает клиентскую проверку в Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="7268e-309">The following code disables client validation in Razor Pages:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_DisableClientValidation&highlight=2-5)]

<span data-ttu-id="7268e-310">Другие параметры отключения проверки на стороне клиента:</span><span class="sxs-lookup"><span data-stu-id="7268e-310">Other options to disable client-side validation:</span></span>

* <span data-ttu-id="7268e-311">Закомментируйте ссылку на `_ValidationScriptsPartial` во всех файлах с расширением *CSHTML*.</span><span class="sxs-lookup"><span data-stu-id="7268e-311">Comment out the reference to `_ValidationScriptsPartial` in all the *.cshtml* files.</span></span>
* <span data-ttu-id="7268e-312">Удалите содержимое файла *Pages\Shared\_ValidationScriptsPartial.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7268e-312">Remove the contents of the *Pages\Shared\_ValidationScriptsPartial.cshtml* file.</span></span>

<span data-ttu-id="7268e-313">Предыдущий подход не помешает проверке на стороне клиента библиотеки классов Razor для ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="7268e-313">The preceding approach won't prevent client side validation of ASP.NET Core Identity Razor Class Library.</span></span> <span data-ttu-id="7268e-314">Для получения дополнительной информации см. <xref:security/authentication/scaffold-identity>.</span><span class="sxs-lookup"><span data-stu-id="7268e-314">For more information, see <xref:security/authentication/scaffold-identity>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7268e-315">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="7268e-315">Additional resources</span></span>

* [<span data-ttu-id="7268e-316">Пространство имен System.ComponentModel.DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="7268e-316">System.ComponentModel.DataAnnotations namespace</span></span>](xref:System.ComponentModel.DataAnnotations)
* [<span data-ttu-id="7268e-317">Привязка модели</span><span class="sxs-lookup"><span data-stu-id="7268e-317">Model Binding</span></span>](model-binding.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7268e-318">В этой статье объясняется, как осуществлять проверки вводимых пользователем данных в приложении ASP.NET Core MVC или Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="7268e-318">This article explains how to validate user input in an ASP.NET Core MVC or Razor Pages app.</span></span>

<span data-ttu-id="7268e-319">[Просмотреть или скачать пример кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="7268e-319">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="model-state"></a><span data-ttu-id="7268e-320">Состояние модели</span><span class="sxs-lookup"><span data-stu-id="7268e-320">Model state</span></span>

<span data-ttu-id="7268e-321">Состояние модели представляет ошибки, создаваемые двумя подсистемами: привязкой модели и проверкой модели.</span><span class="sxs-lookup"><span data-stu-id="7268e-321">Model state represents errors that come from two subsystems: model binding and model validation.</span></span> <span data-ttu-id="7268e-322">Ошибки, поступающие из [привязки модели](model-binding.md), чаще всего представляют собой ошибки преобразования данных (например, x вводится в поле, которое ожидает целое число).</span><span class="sxs-lookup"><span data-stu-id="7268e-322">Errors that originate from [model binding](model-binding.md) are generally data conversion errors (for example, an "x" is entered in a field that expects an integer).</span></span> <span data-ttu-id="7268e-323">Проверка модели проводится после привязки модели и сообщает об ошибках, при которых данные не соответствуют бизнес-правилам (например, 0 вводится в поле, которое ожидает оценку от 1 до 5).</span><span class="sxs-lookup"><span data-stu-id="7268e-323">Model validation occurs after model binding and reports errors where the data doesn't conform to business rules (for example, a 0 is entered in a field that expects a rating between 1 and 5).</span></span>

<span data-ttu-id="7268e-324">Привязка модели и проверка модели происходят перед выполнением действия контроллера или метода обработчика Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="7268e-324">Both model binding and validation occur before the execution of a controller action or a Razor Pages handler method.</span></span> <span data-ttu-id="7268e-325">Веб-приложение отвечает за проверку `ModelState.IsValid` и реагирует соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="7268e-325">For web apps, it's the app's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="7268e-326">Веб-приложения обычно повторно отображают страницы с сообщением об ошибке.</span><span class="sxs-lookup"><span data-stu-id="7268e-326">Web apps typically redisplay the page with an error message:</span></span>

[!code-csharp[](validation/samples_snapshot/2.x/Create.cshtml.cs?name=snippet&highlight=3-6)]

<span data-ttu-id="7268e-327">Контроллерам веб-API не нужно проверять `ModelState.IsValid` при наличии атрибута `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="7268e-327">Web API controllers don't have to check `ModelState.IsValid` if they have the `[ApiController]` attribute.</span></span> <span data-ttu-id="7268e-328">В этом случае автоматически возвращается ответ HTTP 400, содержащий сведения об ошибке, если состояние модели недопустимо.</span><span class="sxs-lookup"><span data-stu-id="7268e-328">In that case, an automatic HTTP 400 response containing error details is returned when model state is invalid.</span></span> <span data-ttu-id="7268e-329">Дополнительные сведения см. в разделе [Автоматические отклики HTTP 400](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="7268e-329">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="rerun-validation"></a><span data-ttu-id="7268e-330">Перезапуск проверки</span><span class="sxs-lookup"><span data-stu-id="7268e-330">Rerun validation</span></span>

<span data-ttu-id="7268e-331">Проверка выполняется автоматически, однако может потребоваться повторить ее вручную.</span><span class="sxs-lookup"><span data-stu-id="7268e-331">Validation is automatic, but you might want to repeat it manually.</span></span> <span data-ttu-id="7268e-332">Например, можно вычислить значение свойства и повторно выполнить проверку после установки свойства в вычисляемое значение.</span><span class="sxs-lookup"><span data-stu-id="7268e-332">For example, you might compute a value for a property and want to rerun validation after setting the property to the computed value.</span></span> <span data-ttu-id="7268e-333">Для повторной проверки вызовите метод `TryValidateModel`, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="7268e-333">To rerun validation, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/MoviesController.cs?name=snippet_TryValidateModel&highlight=11)]

## <a name="validation-attributes"></a><span data-ttu-id="7268e-334">Атрибуты проверки</span><span class="sxs-lookup"><span data-stu-id="7268e-334">Validation attributes</span></span>

<span data-ttu-id="7268e-335">Атрибуты проверки позволяют задать правила проверки для свойств модели.</span><span class="sxs-lookup"><span data-stu-id="7268e-335">Validation attributes let you specify validation rules for model properties.</span></span> <span data-ttu-id="7268e-336">В следующем примере из [примера приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) показан класс модели, который помечается с помощью атрибутов проверки.</span><span class="sxs-lookup"><span data-stu-id="7268e-336">The following example from [the sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) shows a model class that is annotated with validation attributes.</span></span> <span data-ttu-id="7268e-337">`[ClassicMovie]` является настраиваемым атрибутом проверки; остальные являются встроенными.</span><span class="sxs-lookup"><span data-stu-id="7268e-337">The `[ClassicMovie]` attribute is a custom validation attribute and the others are built-in.</span></span> <span data-ttu-id="7268e-338">Не показан `[ClassicMovie2]`, который демонстрирует альтернативный способ реализации настраиваемого атрибута.</span><span class="sxs-lookup"><span data-stu-id="7268e-338">Not shown is `[ClassicMovie2]`, which shows an alternative way to implement a custom attribute.</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Models/Movie.cs?name=snippet_ModelClass)]

## <a name="built-in-attributes"></a><span data-ttu-id="7268e-339">Встроенные атрибуты</span><span class="sxs-lookup"><span data-stu-id="7268e-339">Built-in attributes</span></span>

<span data-ttu-id="7268e-340">К встроенным атрибутам проверки относятся:</span><span class="sxs-lookup"><span data-stu-id="7268e-340">Built-in validation attributes include:</span></span>

* <span data-ttu-id="7268e-341">`[CreditCard]`. проверяет, имеет ли свойство формат кредитной карты.</span><span class="sxs-lookup"><span data-stu-id="7268e-341">`[CreditCard]`: Validates that the property has a credit card format.</span></span>
* <span data-ttu-id="7268e-342">`[Compare]`. проверяет, совпадают ли два свойства модели.</span><span class="sxs-lookup"><span data-stu-id="7268e-342">`[Compare]`: Validates that two properties in a model match.</span></span> <span data-ttu-id="7268e-343">Например, файл *Register.cshtml.cs* использует `[Compare]` для проверки совпадений двух введенных паролей.</span><span class="sxs-lookup"><span data-stu-id="7268e-343">For example, the *Register.cshtml.cs* file uses `[Compare]` to validate the two entered passwords match.</span></span> <span data-ttu-id="7268e-344">[Удостоверение шаблона](xref:security/authentication/scaffold-identity) для просмотра кода регистрации.</span><span class="sxs-lookup"><span data-stu-id="7268e-344">[Scaffold Identity](xref:security/authentication/scaffold-identity) to see the Register code.</span></span>
* <span data-ttu-id="7268e-345">`[EmailAddress]`. проверяет, имеет ли свойство формат адреса электронной почты.</span><span class="sxs-lookup"><span data-stu-id="7268e-345">`[EmailAddress]`: Validates that the property has an email format.</span></span>
* <span data-ttu-id="7268e-346">`[Phone]`. проверяет, имеет ли свойство формат номера телефона.</span><span class="sxs-lookup"><span data-stu-id="7268e-346">`[Phone]`: Validates that the property has a telephone number format.</span></span>
* <span data-ttu-id="7268e-347">`[Range]`. проверяет, находится ли значение свойства в указанном диапазоне.</span><span class="sxs-lookup"><span data-stu-id="7268e-347">`[Range]`: Validates that the property value falls within a specified range.</span></span>
* <span data-ttu-id="7268e-348">`[RegularExpression]`. проверяет, соответствует ли значение свойства указанному регулярному выражению.</span><span class="sxs-lookup"><span data-stu-id="7268e-348">`[RegularExpression]`: Validates that the property value matches a specified regular expression.</span></span>
* <span data-ttu-id="7268e-349">`[Required]`. проверяет, что поле не равно NULL.</span><span class="sxs-lookup"><span data-stu-id="7268e-349">`[Required]`: Validates that the field is not null.</span></span> <span data-ttu-id="7268e-350">Дополнительные сведения о поведении этого атрибута см. в разделе [Атрибут `[Required]`](#required-attribute).</span><span class="sxs-lookup"><span data-stu-id="7268e-350">See [`[Required]` attribute](#required-attribute) for details about this attribute's behavior.</span></span>
* <span data-ttu-id="7268e-351">`[StringLength]`. проверяет, что значение свойства строки не превышает ограничение по указанной длине.</span><span class="sxs-lookup"><span data-stu-id="7268e-351">`[StringLength]`: Validates that a string property value doesn't exceed a specified length limit.</span></span>
* <span data-ttu-id="7268e-352">`[Url]`. проверяет, имеет ли свойство формат URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="7268e-352">`[Url]`: Validates that the property has a URL format.</span></span>
* <span data-ttu-id="7268e-353">`[Remote]`. проверяет входные данные на клиенте путем вызова метода действия на сервере.</span><span class="sxs-lookup"><span data-stu-id="7268e-353">`[Remote]`: Validates input on the client by calling an action method on the server.</span></span> <span data-ttu-id="7268e-354">Дополнительные сведения о поведении этого атрибута см. в разделе [Атрибут `[Remote]`](#remote-attribute).</span><span class="sxs-lookup"><span data-stu-id="7268e-354">See [`[Remote]` attribute](#remote-attribute) for details about this attribute's behavior.</span></span>

<span data-ttu-id="7268e-355">При использовании атрибута `[RegularExpression]` с проверкой на стороне клиента регулярное выражение выполняется в JavaScript в клиенте.</span><span class="sxs-lookup"><span data-stu-id="7268e-355">When using the `[RegularExpression]` attribute with client-side validation, the regex is executed in JavaScript on the client.</span></span> <span data-ttu-id="7268e-356">Это означает, что будет использоваться поведение сопоставления [ECMAScript](/dotnet/standard/base-types/regular-expression-options#ecmascript-matching-behavior).</span><span class="sxs-lookup"><span data-stu-id="7268e-356">This means [ECMAScript](/dotnet/standard/base-types/regular-expression-options#ecmascript-matching-behavior) matching behavior will be used.</span></span> <span data-ttu-id="7268e-357">Дополнительные сведения см. в [этой статье об ошибке на GitHub](https://github.com/dotnet/corefx/issues/42487).</span><span class="sxs-lookup"><span data-stu-id="7268e-357">For more information, see [this GitHub issue](https://github.com/dotnet/corefx/issues/42487).</span></span>

<span data-ttu-id="7268e-358">Полный перечень атрибутов проверки можно найти в пространстве имен [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations).</span><span class="sxs-lookup"><span data-stu-id="7268e-358">A complete list of validation attributes can be found in the [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) namespace.</span></span>

### <a name="error-messages"></a><span data-ttu-id="7268e-359">Сообщения об ошибках</span><span class="sxs-lookup"><span data-stu-id="7268e-359">Error messages</span></span>

<span data-ttu-id="7268e-360">Атрибуты проверки позволяют указать сообщение об ошибке, которое будет отображаться, если входные данные недопустимы.</span><span class="sxs-lookup"><span data-stu-id="7268e-360">Validation attributes let you specify the error message to be displayed for invalid input.</span></span> <span data-ttu-id="7268e-361">Пример:</span><span class="sxs-lookup"><span data-stu-id="7268e-361">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

<span data-ttu-id="7268e-362">На внутреннем уровне атрибуты вызывают `String.Format` с заполнителем для имени поля и иногда дополнительным заполнителями.</span><span class="sxs-lookup"><span data-stu-id="7268e-362">Internally, the attributes call `String.Format` with a placeholder for the field name and sometimes additional placeholders.</span></span> <span data-ttu-id="7268e-363">Пример:</span><span class="sxs-lookup"><span data-stu-id="7268e-363">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

<span data-ttu-id="7268e-364">При применении к свойству `Name` сообщение об ошибке, созданное в приведенном выше коде, имело бы вид Name length must be between 6 and 8 (Длина имени должна быть от 6 до 8).</span><span class="sxs-lookup"><span data-stu-id="7268e-364">When applied to a `Name` property, the error message created by the preceding code would be "Name length must be between 6 and 8.".</span></span>

<span data-ttu-id="7268e-365">Чтобы узнать, какие параметры передаются в `String.Format` для сообщения об ошибке определенного атрибута, см. раздел [Исходный код DataAnnotations](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span><span class="sxs-lookup"><span data-stu-id="7268e-365">To find out which parameters are passed to `String.Format` for a particular attribute's error message, see the [DataAnnotations source code](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span></span>

## <a name="required-attribute"></a><span data-ttu-id="7268e-366">Атрибут [Required]</span><span class="sxs-lookup"><span data-stu-id="7268e-366">[Required] attribute</span></span>

<span data-ttu-id="7268e-367">По умолчанию система проверки рассматривает не допускающие значение NULL параметры или свойства так, как если бы они имели атрибут `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="7268e-367">By default, the validation system treats non-nullable parameters or properties as if they had a `[Required]` attribute.</span></span> <span data-ttu-id="7268e-368">[Типы значений](/dotnet/csharp/language-reference/keywords/value-types), например `decimal` и `int`, не поддерживают значение NULL.</span><span class="sxs-lookup"><span data-stu-id="7268e-368">[Value types](/dotnet/csharp/language-reference/keywords/value-types) such as `decimal` and `int` are non-nullable.</span></span>

### <a name="required-validation-on-the-server"></a><span data-ttu-id="7268e-369">Проверка [Required] на сервере</span><span class="sxs-lookup"><span data-stu-id="7268e-369">[Required] validation on the server</span></span>

<span data-ttu-id="7268e-370">На сервере обязательное значение считается отсутствующим, если свойство имеет значение NULL.</span><span class="sxs-lookup"><span data-stu-id="7268e-370">On the server, a required value is considered missing if the property is null.</span></span> <span data-ttu-id="7268e-371">Поле, не допускающее значения NULL, всегда является допустимым, и сообщение об ошибке атрибута [Required] никогда не выводится.</span><span class="sxs-lookup"><span data-stu-id="7268e-371">A non-nullable field is always valid, and the [Required] attribute's error message is never displayed.</span></span>

<span data-ttu-id="7268e-372">Тем не менее привязка модели для ненулевого свойства может завершиться ошибкой, приводящей к сообщению об ошибке, например `The value '' is invalid`.</span><span class="sxs-lookup"><span data-stu-id="7268e-372">However, model binding for a non-nullable property may fail, resulting in an error message such as `The value '' is invalid`.</span></span> <span data-ttu-id="7268e-373">Чтобы задать настраиваемое сообщение об ошибке во время проверки не допускающих значения NULL типов на стороне сервера, у вас есть следующие варианты.</span><span class="sxs-lookup"><span data-stu-id="7268e-373">To specify a custom error message for server-side validation of non-nullable types, you have the following options:</span></span>

* <span data-ttu-id="7268e-374">Сделать поле допускающим значение NULL (например, `decimal?` вместо `decimal`).</span><span class="sxs-lookup"><span data-stu-id="7268e-374">Make the field nullable (for example, `decimal?` instead of `decimal`).</span></span> <span data-ttu-id="7268e-375">Типы значений [Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) обрабатываются как стандартные типы, допускающие значение NULL.</span><span class="sxs-lookup"><span data-stu-id="7268e-375">[Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) value types are treated like standard nullable types.</span></span>
* <span data-ttu-id="7268e-376">Указать сообщение об ошибке по умолчанию для использования в привязке модели, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="7268e-376">Specify the default error message to be used by model binding, as shown in the following example:</span></span>

  [!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=4-5)]

  <span data-ttu-id="7268e-377">Дополнительные сведения об ошибках привязки модели, у которых можно задать сообщение по умолчанию, см. в разделе <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span><span class="sxs-lookup"><span data-stu-id="7268e-377">For more information about model binding errors that you can set default messages for, see <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span></span>

### <a name="required-validation-on-the-client"></a><span data-ttu-id="7268e-378">Проверка [Required] на клиенте</span><span class="sxs-lookup"><span data-stu-id="7268e-378">[Required] validation on the client</span></span>

<span data-ttu-id="7268e-379">Типы и строки, не допускающие значение NULL, обрабатываются на клиенте не так, как на сервере.</span><span class="sxs-lookup"><span data-stu-id="7268e-379">Non-nullable types and strings are handled differently on the client compared to the server.</span></span> <span data-ttu-id="7268e-380">На клиенте:</span><span class="sxs-lookup"><span data-stu-id="7268e-380">On the client:</span></span>

* <span data-ttu-id="7268e-381">Значение считается присутствующим только в том случае, если для него вводятся данные.</span><span class="sxs-lookup"><span data-stu-id="7268e-381">A value is considered present only if input is entered for it.</span></span> <span data-ttu-id="7268e-382">Таким образом, проверка на стороне клиента обрабатывает типы, не допускающие значение NULL, так же, как обнуляемые типы.</span><span class="sxs-lookup"><span data-stu-id="7268e-382">Therefore, client-side validation handles non-nullable types the same as nullable types.</span></span>
* <span data-ttu-id="7268e-383">Пробелы в поле строки считаются допустимыми входными данными при проверке методом jQuery [required](https://jqueryvalidation.org/required-method/).</span><span class="sxs-lookup"><span data-stu-id="7268e-383">Whitespace in a string field is considered valid input by the jQuery Validation [required](https://jqueryvalidation.org/required-method/) method.</span></span> <span data-ttu-id="7268e-384">Проверка на стороне сервера считает, что обязательное строковое поле недопустимо, если введены только пробелы.</span><span class="sxs-lookup"><span data-stu-id="7268e-384">Server-side validation considers a required string field invalid if only whitespace is entered.</span></span>

<span data-ttu-id="7268e-385">Как отмечалось ранее, не допускающие значение NULL типы рассматриваются как имеющие атрибут `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="7268e-385">As noted earlier, non-nullable types are treated as though they had a `[Required]` attribute.</span></span> <span data-ttu-id="7268e-386">Это означает, что вы получаете проверку на стороне клиента, даже если не применять атрибут `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="7268e-386">That means you get client-side validation even if you don't apply the `[Required]` attribute.</span></span> <span data-ttu-id="7268e-387">Но если вы не используете атрибут, вы получаете сообщение об ошибке по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="7268e-387">But if you don't use the attribute, you get a default error message.</span></span> <span data-ttu-id="7268e-388">Чтобы задать настраиваемое сообщение об ошибке, используйте атрибут.</span><span class="sxs-lookup"><span data-stu-id="7268e-388">To specify a custom error message, use the attribute.</span></span>

## <a name="remote-attribute"></a><span data-ttu-id="7268e-389">Атрибут [Remote]</span><span class="sxs-lookup"><span data-stu-id="7268e-389">[Remote] attribute</span></span>

<span data-ttu-id="7268e-390">Атрибут `[Remote]` реализует проверку на стороне клиента, в ходе которой требуется вызвать метод на сервере, чтобы определить, является ли допустимым поле ввода.</span><span class="sxs-lookup"><span data-stu-id="7268e-390">The `[Remote]` attribute implements client-side validation that requires calling a method on the server to determine whether field input is valid.</span></span> <span data-ttu-id="7268e-391">Например, приложению может потребоваться проверить, занято ли имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="7268e-391">For example, the app may need to verify whether a user name is already in use.</span></span>

<span data-ttu-id="7268e-392">Для реализации удаленной проверки сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="7268e-392">To implement remote validation:</span></span>

1. <span data-ttu-id="7268e-393">Создайте для вызова из JavaScript метод действия.</span><span class="sxs-lookup"><span data-stu-id="7268e-393">Create an action method for JavaScript to call.</span></span>  <span data-ttu-id="7268e-394">Метод проверки jQuery [remote](https://jqueryvalidation.org/remote-method/) ожидает ответ JSON.</span><span class="sxs-lookup"><span data-stu-id="7268e-394">The jQuery Validate [remote](https://jqueryvalidation.org/remote-method/) method expects a JSON response:</span></span>

   * <span data-ttu-id="7268e-395">`"true"` означает, что входные данные допустимы.</span><span class="sxs-lookup"><span data-stu-id="7268e-395">`"true"` means the input data is valid.</span></span>
   * <span data-ttu-id="7268e-396">`"false"`, `undefined` или `null` означают, что входные данные недопустимы.</span><span class="sxs-lookup"><span data-stu-id="7268e-396">`"false"`, `undefined`, or `null` means the input is invalid.</span></span>  <span data-ttu-id="7268e-397">Вывод стандартного сообщения об ошибке</span><span class="sxs-lookup"><span data-stu-id="7268e-397">Display the default error message.</span></span>
   * <span data-ttu-id="7268e-398">Все прочие значения означают, что входные данные недопустимы.</span><span class="sxs-lookup"><span data-stu-id="7268e-398">Any other string means the input is invalid.</span></span> <span data-ttu-id="7268e-399">Вывод строки как настраиваемого сообщения об ошибке.</span><span class="sxs-lookup"><span data-stu-id="7268e-399">Display the string as a custom error message.</span></span>

   <span data-ttu-id="7268e-400">Ниже приведен пример метода действия, который возвращает настраиваемое сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="7268e-400">Here's an example of an action method that returns a custom error message:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. <span data-ttu-id="7268e-401">В классе модели пометьте свойство атрибутом `[Remote]`, указывающим метод действия проверки, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="7268e-401">In the model class, annotate the property with a `[Remote]` attribute that points to the validation action method, as shown in the following example:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Models/User.cs?name=snippet_UserEmailProperty)]
 
   <span data-ttu-id="7268e-402">Атрибут `[Remote]` находится в пространстве имен `Microsoft.AspNetCore.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="7268e-402">The `[Remote]` attribute is in the `Microsoft.AspNetCore.Mvc` namespace.</span></span> <span data-ttu-id="7268e-403">Установите пакет NuGet [Microsoft.AspNetCore.Mvc.ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures), если вы не используете метапакет `Microsoft.AspNetCore.App` или `Microsoft.AspNetCore.All`.</span><span class="sxs-lookup"><span data-stu-id="7268e-403">Install the [Microsoft.AspNetCore.Mvc.ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures) NuGet package if you're not using the `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All` metapackage.</span></span>
   
### <a name="additional-fields"></a><span data-ttu-id="7268e-404">Дополнительные поля</span><span class="sxs-lookup"><span data-stu-id="7268e-404">Additional fields</span></span>

<span data-ttu-id="7268e-405">Свойство `AdditionalFields` атрибута `[Remote]` позволяет проверять сочетания полей с данными на сервере.</span><span class="sxs-lookup"><span data-stu-id="7268e-405">The `AdditionalFields` property of the `[Remote]` attribute lets you validate combinations of fields against data on the server.</span></span> <span data-ttu-id="7268e-406">Например, если бы в модели `User` были свойства `FirstName` и `LastName`, могла бы возникнуть необходимость проверить, нет ли уже пользователя с такой парой имен.</span><span class="sxs-lookup"><span data-stu-id="7268e-406">For example, if the `User` model had `FirstName` and `LastName` properties, you might want to verify that no existing users already have that pair of names.</span></span> <span data-ttu-id="7268e-407">В следующем примере показано использование `AdditionalFields`.</span><span class="sxs-lookup"><span data-stu-id="7268e-407">The following example shows how to use `AdditionalFields`:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Models/User.cs?name=snippet_UserNameProperties)]

<span data-ttu-id="7268e-408">`AdditionalFields` можно явно присвоить строкам `"FirstName"` и `"LastName"`, но использование оператора [nameof](/dotnet/csharp/language-reference/keywords/nameof) упрощает дальнейший рефакторинг.</span><span class="sxs-lookup"><span data-stu-id="7268e-408">`AdditionalFields` could be set explicitly to the strings `"FirstName"` and `"LastName"`, but using the [nameof](/dotnet/csharp/language-reference/keywords/nameof) operator simplifies later refactoring.</span></span> <span data-ttu-id="7268e-409">Методу действия для этой проверки необходимо принимать аргументы для имени и фамилии.</span><span class="sxs-lookup"><span data-stu-id="7268e-409">The action method for this validation must accept both first name and last name arguments:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyName)]

<span data-ttu-id="7268e-410">Когда пользователь вводит имя или фамилию, JavaScript выполняет удаленный вызов, чтобы увидеть, будет ли эта пара принята.</span><span class="sxs-lookup"><span data-stu-id="7268e-410">When the user enters a first or last name, JavaScript makes a remote call to see if that pair of names has been taken.</span></span>

<span data-ttu-id="7268e-411">Чтобы проверить несколько дополнительных полей, их следует указывать в виде списка с разделителями-запятыми.</span><span class="sxs-lookup"><span data-stu-id="7268e-411">To validate two or more additional fields, provide them as a comma-delimited list.</span></span> <span data-ttu-id="7268e-412">Например, чтобы добавить в модель свойство `MiddleName`, задайте атрибут `[Remote]`, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="7268e-412">For example, to add a `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following example:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="7268e-413">`AdditionalFields`, как и все аргументы атрибутов, должен представлять собой константное выражение.</span><span class="sxs-lookup"><span data-stu-id="7268e-413">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span> <span data-ttu-id="7268e-414">Поэтому не следует использовать [интерполированную строку](/dotnet/csharp/language-reference/keywords/interpolated-strings) или вызов <xref:System.String.Join*>для инициализации `AdditionalFields`.</span><span class="sxs-lookup"><span data-stu-id="7268e-414">Therefore, don't use an [interpolated string](/dotnet/csharp/language-reference/keywords/interpolated-strings) or call <xref:System.String.Join*> to initialize `AdditionalFields`.</span></span>

## <a name="alternatives-to-built-in-attributes"></a><span data-ttu-id="7268e-415">Альтернативы для встроенных атрибутов</span><span class="sxs-lookup"><span data-stu-id="7268e-415">Alternatives to built-in attributes</span></span>

<span data-ttu-id="7268e-416">Если вам нужна проверка, которую не предоставляют встроенные атрибуты, вы можете следующее.</span><span class="sxs-lookup"><span data-stu-id="7268e-416">If you need validation not provided by built-in attributes, you can:</span></span>

* <span data-ttu-id="7268e-417">[Создать настраиваемые атрибуты](#custom-attributes).</span><span class="sxs-lookup"><span data-stu-id="7268e-417">[Create custom attributes](#custom-attributes).</span></span>
* <span data-ttu-id="7268e-418">[Реализовать IValidatableObject](#ivalidatableobject).</span><span class="sxs-lookup"><span data-stu-id="7268e-418">[Implement IValidatableObject](#ivalidatableobject).</span></span>

## <a name="custom-attributes"></a><span data-ttu-id="7268e-419">Настраиваемые атрибуты</span><span class="sxs-lookup"><span data-stu-id="7268e-419">Custom attributes</span></span>

<span data-ttu-id="7268e-420">Для сценариев, где не годятся встроенные атрибуты проверки, можно создать настраиваемые атрибуты.</span><span class="sxs-lookup"><span data-stu-id="7268e-420">For scenarios that the built-in validation attributes don't handle, you can create custom validation attributes.</span></span> <span data-ttu-id="7268e-421">Создайте класс, наследуемый от <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, и переопределите метод <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*>.</span><span class="sxs-lookup"><span data-stu-id="7268e-421">Create a class that inherits from <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, and override the <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> method.</span></span>

<span data-ttu-id="7268e-422">Метод `IsValid` принимает объект с именем *value*, который является входными данными для проверки.</span><span class="sxs-lookup"><span data-stu-id="7268e-422">The `IsValid` method accepts an object named *value*, which is the input to be validated.</span></span> <span data-ttu-id="7268e-423">Перегрузка также принимает объект `ValidationContext`, который предоставляет дополнительные сведения, такие как экземпляр модели, созданный с помощью привязки модели.</span><span class="sxs-lookup"><span data-stu-id="7268e-423">An overload also accepts a `ValidationContext` object, which provides additional information, such as the model instance created by model binding.</span></span>

<span data-ttu-id="7268e-424">В следующем примере проверяется, что дата выпуска фильмов в *классическом* жанре задана не позднее указанного года.</span><span class="sxs-lookup"><span data-stu-id="7268e-424">The following example validates that the release date for a movie in the *Classic* genre isn't later than a specified year.</span></span> <span data-ttu-id="7268e-425">Атрибут `[ClassicMovie2]` сначала проверяет жанр и продолжает проверку, только если он *классический*.</span><span class="sxs-lookup"><span data-stu-id="7268e-425">The `[ClassicMovie2]` attribute checks the genre first and continues only if it's *Classic*.</span></span> <span data-ttu-id="7268e-426">У фильмов, попавших в классику, система проверяет даты выпуска, чтобы убедиться в том, что она не является более поздней, чем ограничение, переданное в конструктор атрибута.</span><span class="sxs-lookup"><span data-stu-id="7268e-426">For movies identified as classics, it checks the release date to make sure it's not later than the limit passed to the attribute constructor.)</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/ClassicMovieAttribute.cs?name=snippet_ClassicMovieAttribute)]

<span data-ttu-id="7268e-427">Приведенная выше переменная `movie` представляет объект `Movie`, который содержит данные из переданной формы.</span><span class="sxs-lookup"><span data-stu-id="7268e-427">The `movie` variable in the preceding example represents a `Movie` object that contains the data from the form submission.</span></span> <span data-ttu-id="7268e-428">Метод `IsValid` проверяет дату и жанр.</span><span class="sxs-lookup"><span data-stu-id="7268e-428">The `IsValid` method checks the date and genre.</span></span> <span data-ttu-id="7268e-429">После успешной проверки `IsValid` возвращает код `ValidationResult.Success`.</span><span class="sxs-lookup"><span data-stu-id="7268e-429">Upon successful validation, `IsValid` returns a `ValidationResult.Success` code.</span></span> <span data-ttu-id="7268e-430">Если проверка завершается неудачно, возвращается `ValidationResult` с сообщением об ошибке.</span><span class="sxs-lookup"><span data-stu-id="7268e-430">When validation fails, a `ValidationResult` with an error message is returned.</span></span>

## <a name="ivalidatableobject"></a><span data-ttu-id="7268e-431">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="7268e-431">IValidatableObject</span></span>

<span data-ttu-id="7268e-432">Предыдущий пример работает только с типами `Movie`.</span><span class="sxs-lookup"><span data-stu-id="7268e-432">The preceding example works only with `Movie` types.</span></span> <span data-ttu-id="7268e-433">Другой вариант для проверки на уровне класса — реализация `IValidatableObject` в классе модели, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="7268e-433">Another option for class-level validation is to implement `IValidatableObject` in the model class, as shown in the following example:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Models/MovieIValidatable.cs?name=snippet&highlight=1,26-34)]

## <a name="top-level-node-validation"></a><span data-ttu-id="7268e-434">Проверка узлов верхнего уровня</span><span class="sxs-lookup"><span data-stu-id="7268e-434">Top-level node validation</span></span>

<span data-ttu-id="7268e-435">Узлы верхнего уровня содержат:</span><span class="sxs-lookup"><span data-stu-id="7268e-435">Top-level nodes include:</span></span>

* <span data-ttu-id="7268e-436">параметры действия;</span><span class="sxs-lookup"><span data-stu-id="7268e-436">Action parameters</span></span>
* <span data-ttu-id="7268e-437">свойства контроллера;</span><span class="sxs-lookup"><span data-stu-id="7268e-437">Controller properties</span></span>
* <span data-ttu-id="7268e-438">параметры обработчика страниц;</span><span class="sxs-lookup"><span data-stu-id="7268e-438">Page handler parameters</span></span>
* <span data-ttu-id="7268e-439">свойства страничной модели.</span><span class="sxs-lookup"><span data-stu-id="7268e-439">Page model properties</span></span>

<span data-ttu-id="7268e-440">Проверка привязанных к модели узлов верхнего уровня осуществляется наряду с проверкой свойств модели.</span><span class="sxs-lookup"><span data-stu-id="7268e-440">Model-bound top-level nodes are validated in addition to validating model properties.</span></span> <span data-ttu-id="7268e-441">В следующем примере, взятом из примера приложения, метод `VerifyPhone` использует класс <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> для проверки параметра действия `phone`.</span><span class="sxs-lookup"><span data-stu-id="7268e-441">In the following example from the sample app, the `VerifyPhone` method uses the <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> to validate the `phone` action parameter:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

<span data-ttu-id="7268e-442">Узлы верхнего уровня могут применять класс <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> с атрибутами проверки.</span><span class="sxs-lookup"><span data-stu-id="7268e-442">Top-level nodes can use <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> with validation attributes.</span></span> <span data-ttu-id="7268e-443">В следующем примере, взятом из примера приложения, метод `CheckAge` указывает, что при отправке формы параметр `age` должен быть привязан из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="7268e-443">In the following example from the sample app, the `CheckAge` method specifies that the `age` parameter must be bound from the query string when the form is submitted:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_CheckAge)]

<span data-ttu-id="7268e-444">На странице "Check Age" (Проверка возраста) (*CheckAge.cshtml*) находятся две формы.</span><span class="sxs-lookup"><span data-stu-id="7268e-444">In the Check Age page (*CheckAge.cshtml*), there are two forms.</span></span> <span data-ttu-id="7268e-445">Первая форма отправляет значение `Age`, равное `99`, в виде строки запроса: `https://localhost:5001/Users/CheckAge?Age=99`.</span><span class="sxs-lookup"><span data-stu-id="7268e-445">The first form submits an `Age` value of `99` as a query string: `https://localhost:5001/Users/CheckAge?Age=99`.</span></span>

<span data-ttu-id="7268e-446">Если из строки запроса отправлен параметр `age` в правильном формате, форма проходит проверку.</span><span class="sxs-lookup"><span data-stu-id="7268e-446">When a properly formatted `age` parameter from the query string is submitted, the form validates.</span></span>

<span data-ttu-id="7268e-447">Вторая форма на странице "Check Age" (Проверка возраста) отправляет значение `Age` в теле запроса, и проверка завершается сбоем.</span><span class="sxs-lookup"><span data-stu-id="7268e-447">The second form on the Check Age page submits the `Age` value in the body of the request, and validation fails.</span></span> <span data-ttu-id="7268e-448">Ошибка привязки связана с тем, что параметр `age` должен поступать из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="7268e-448">Binding fails because the `age` parameter must come from a query string.</span></span>

<span data-ttu-id="7268e-449">При работе с `CompatibilityVersion.Version_2_1` или более поздней версией проверка узла верхнего уровня по умолчанию включена.</span><span class="sxs-lookup"><span data-stu-id="7268e-449">When running with `CompatibilityVersion.Version_2_1` or later, top-level node validation is enabled by default.</span></span> <span data-ttu-id="7268e-450">В противном случае проверка узла верхнего уровня отключена.</span><span class="sxs-lookup"><span data-stu-id="7268e-450">Otherwise, top-level node validation is disabled.</span></span> <span data-ttu-id="7268e-451">Параметр по умолчанию можно переопределить, задав свойство <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> в (`Startup.ConfigureServices`), как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="7268e-451">The default option can be overridden by setting the <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> property in (`Startup.ConfigureServices`), as shown here:</span></span>

[!code-csharp[](validation/samples_snapshot/2.x/Startup.cs?name=snippet_AddMvc&highlight=4)]

## <a name="maximum-errors"></a><span data-ttu-id="7268e-452">Максимальное количество ошибок</span><span class="sxs-lookup"><span data-stu-id="7268e-452">Maximum errors</span></span>

<span data-ttu-id="7268e-453">При достижении максимального количества ошибок (по умолчанию 200) проверка прекращается.</span><span class="sxs-lookup"><span data-stu-id="7268e-453">Validation stops when the maximum number of errors is reached (200 by default).</span></span> <span data-ttu-id="7268e-454">Это число можно изменить с помощью следующего кода в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7268e-454">You can configure this number with the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=3)]

## <a name="maximum-recursion"></a><span data-ttu-id="7268e-455">Максимальная рекурсия</span><span class="sxs-lookup"><span data-stu-id="7268e-455">Maximum recursion</span></span>

<span data-ttu-id="7268e-456"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> проходит через граф объектов в проверяемой модели.</span><span class="sxs-lookup"><span data-stu-id="7268e-456"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> traverses the object graph of the model being validated.</span></span> <span data-ttu-id="7268e-457">У моделей, которые очень глубоки или содержат бесконечную рекурсию, в ходе проверки может произойти переполнение стека.</span><span class="sxs-lookup"><span data-stu-id="7268e-457">For models that are very deep or are infinitely recursive, validation may result in stack overflow.</span></span> <span data-ttu-id="7268e-458">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) предоставляет способ остановить проверку до превышения настроенной глубины рекурсии обхода.</span><span class="sxs-lookup"><span data-stu-id="7268e-458">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) provides a way to stop validation early if the visitor recursion exceeds a configured depth.</span></span> <span data-ttu-id="7268e-459">Значение `MvcOptions.MaxValidationDepth` по умолчанию — 32 при работе в `CompatibilityVersion.Version_2_2` или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="7268e-459">The default value of `MvcOptions.MaxValidationDepth` is 32 when running with `CompatibilityVersion.Version_2_2` or later.</span></span> <span data-ttu-id="7268e-460">Для более ранних версий значение равно NULL; это означает отсутствие ограничения глубины.</span><span class="sxs-lookup"><span data-stu-id="7268e-460">For earlier versions, the value is null, which means no depth constraint.</span></span>

## <a name="automatic-short-circuit"></a><span data-ttu-id="7268e-461">Автоматическое сокращение</span><span class="sxs-lookup"><span data-stu-id="7268e-461">Automatic short-circuit</span></span>

<span data-ttu-id="7268e-462">Проверка автоматически сокращается (пропускается), если граф модели не требует проверки.</span><span class="sxs-lookup"><span data-stu-id="7268e-462">Validation is automatically short-circuited (skipped) if the model graph doesn't require validation.</span></span> <span data-ttu-id="7268e-463">К числу объектов, которые среда выполнения пропускает при проверке, относятся коллекции примитивов (такие как `byte[]`, `string[]`, `Dictionary<string, string>`) и сложные графы объектов, которые не имеют проверяющих элементов управления.</span><span class="sxs-lookup"><span data-stu-id="7268e-463">Objects that the runtime skips validation for include collections of primitives (such as `byte[]`, `string[]`, `Dictionary<string, string>`) and complex object graphs that don't have any validators.</span></span>

## <a name="disable-validation"></a><span data-ttu-id="7268e-464">Отключение проверки</span><span class="sxs-lookup"><span data-stu-id="7268e-464">Disable validation</span></span>

<span data-ttu-id="7268e-465">Чтобы отключить проверку, сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="7268e-465">To disable validation:</span></span>

1. <span data-ttu-id="7268e-466">Создайте реализацию интерфейса `IObjectModelValidator`, которая не помечает поля как недопустимые.</span><span class="sxs-lookup"><span data-stu-id="7268e-466">Create an implementation of `IObjectModelValidator` that doesn't mark any fields as invalid.</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/NullObjectModelValidator.cs?name=snippet_DisableValidation)]

1. <span data-ttu-id="7268e-467">Добавьте следующий код в `Startup.ConfigureServices` для замены реализации `IObjectModelValidator` по умолчанию в контейнере внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="7268e-467">Add the following code to `Startup.ConfigureServices` to replace the default `IObjectModelValidator` implementation in the dependency injection container.</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_DisableValidation)]

<span data-ttu-id="7268e-468">По-прежнему могут отображаться ошибки состояния модели, которые идут из привязки модели.</span><span class="sxs-lookup"><span data-stu-id="7268e-468">You might still see model state errors that originate from model binding.</span></span>

## <a name="client-side-validation"></a><span data-ttu-id="7268e-469">Проверка на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="7268e-469">Client-side validation</span></span>

<span data-ttu-id="7268e-470">Проверка на стороне клиента не позволяет отправлять форму, пока ее данные не будут допустимыми.</span><span class="sxs-lookup"><span data-stu-id="7268e-470">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="7268e-471">При нажатии кнопки "Отправить" выполняется код JavaScript, который либо отправляет форму, либо выводит сообщения об ошибках.</span><span class="sxs-lookup"><span data-stu-id="7268e-471">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="7268e-472">Проверка на стороне клиента позволяет избежать ненужного кругового захода на сервер при наличии ошибки ввода в форме.</span><span class="sxs-lookup"><span data-stu-id="7268e-472">Client-side validation avoids an unnecessary round trip to the server when there are input errors on a form.</span></span> <span data-ttu-id="7268e-473">Ссылки на следующий скрипт в *_Layout.cshtml* и *_ValidationScriptsPartial.cshtml* поддерживают проверку на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="7268e-473">The following script references in *_Layout.cshtml* and *_ValidationScriptsPartial.cshtml* support client-side validation:</span></span>

[!code-cshtml[](validation/samples/2.x/ValidationSample/Views/Shared/_Layout.cshtml?name=snippet_ScriptTag)]

[!code-cshtml[](validation/samples/2.x/ValidationSample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_ScriptTags)]

<span data-ttu-id="7268e-474">Скрипт [ненавязчивой проверки jQuery](https://github.com/aspnet/jquery-validation-unobtrusive) — это настраиваемая интерфейсная библиотека Майкрософт, которая основана на популярном подключаемом модуле [jQuery Validate](https://jqueryvalidation.org/).</span><span class="sxs-lookup"><span data-stu-id="7268e-474">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="7268e-475">Без скрипта ненавязчивой проверки jQuery одну и ту же логику проверки приходилось бы реализовывать в двух местах: в атрибутах проверки для свойств модели на стороне сервера, а затем еще раз в скриптах на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="7268e-475">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server-side validation attributes on model properties, and then again in client-side scripts.</span></span> <span data-ttu-id="7268e-476">Вместо этого [вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) и [вспомогательные функции HTML](xref:mvc/views/overview) могут использовать атрибуты проверки и метаданные типов из свойств модели для обработки атрибутов `data-` HTML 5 в элементах форм, требующих проверки.</span><span class="sxs-lookup"><span data-stu-id="7268e-476">Instead, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use the validation attributes and type metadata from model properties to render HTML 5 `data-` attributes for the form elements that need validation.</span></span> <span data-ttu-id="7268e-477">Скрипт ненавязчивой проверки jQuery анализирует эти атрибуты `data-` и передает логику в подключаемый модуль jQuery Validate, по сути копируя логику проверки на стороне сервера в клиент.</span><span class="sxs-lookup"><span data-stu-id="7268e-477">jQuery Unobtrusive Validation parses the `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server-side validation logic to the client.</span></span> <span data-ttu-id="7268e-478">Ошибки проверки могут выводиться в клиенте с помощью вспомогательных функций тегов, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="7268e-478">You can display validation errors on the client using tag helpers as shown here:</span></span>

[!code-cshtml[](validation/samples/2.x/ValidationSample/Views/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=4-5)]

<span data-ttu-id="7268e-479">Приведенные выше вспомогательные функции тегов отрисовывают следующий код HTML.</span><span class="sxs-lookup"><span data-stu-id="7268e-479">The preceding tag helpers render the following HTML.</span></span>

```html
<form action="/Movies/Create" method="post">
    <div class="form-horizontal">
        <h4>Movie</h4>
        <div class="text-danger"></div>
        <div class="form-group">
            <label class="col-md-2 control-label" for="ReleaseDate">ReleaseDate</label>
            <div class="col-md-10">
                <input class="form-control" type="datetime"
                data-val="true" data-val-required="The ReleaseDate field is required."
                id="ReleaseDate" name="ReleaseDate" value="">
                <span class="text-danger field-validation-valid"
                data-valmsg-for="ReleaseDate" data-valmsg-replace="true"></span>
            </div>
        </div>
    </div>
</form>
```

<span data-ttu-id="7268e-480">Обратите внимание на то, что атрибуты `data-` в выходных данных HTML соответствуют атрибутам проверки для свойства `ReleaseDate`.</span><span class="sxs-lookup"><span data-stu-id="7268e-480">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `ReleaseDate` property.</span></span> <span data-ttu-id="7268e-481">Атрибут `data-val-required` содержит сообщение об ошибке, которое выводится, если пользователь не заполнил поле даты выхода.</span><span class="sxs-lookup"><span data-stu-id="7268e-481">The `data-val-required` attribute contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="7268e-482">Скрипт ненавязчивой проверки jQuery передает это значение в метод [required()](https://jqueryvalidation.org/required-method/) подключаемого модуля jQuery Validate, который затем выводит это сообщение в соответствующем элементе **\<span>** .</span><span class="sxs-lookup"><span data-stu-id="7268e-482">jQuery Unobtrusive Validation passes this value to the jQuery Validate [required()](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

<span data-ttu-id="7268e-483">Проверка типа данных основана на типе свойства в .NET, если его не переопределяет атрибут `[DataType]`.</span><span class="sxs-lookup"><span data-stu-id="7268e-483">Data type validation is based on the .NET type of a property, unless that is overridden by a `[DataType]` attribute.</span></span> <span data-ttu-id="7268e-484">Браузеры имеют свои сообщения об по умолчанию, но пакет ненавязчивой проверки jQuery может переопределять эти сообщения.</span><span class="sxs-lookup"><span data-stu-id="7268e-484">Browsers have their own default error messages, but the jQuery Validation Unobtrusive Validation package can override those messages.</span></span> <span data-ttu-id="7268e-485">Атрибуты и подклассы `[DataType]`, такие как `[EmailAddress]`, позволяют указать сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="7268e-485">`[DataType]` attributes and subclasses such as `[EmailAddress]` let you specify the error message.</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="7268e-486">Добавление проверки к динамическим формам</span><span class="sxs-lookup"><span data-stu-id="7268e-486">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="7268e-487">Скрипт ненавязчивой проверки jQuery передает логику и параметры проверки в подключаемый модуль jQuery Validate при первой загрузке страницы.</span><span class="sxs-lookup"><span data-stu-id="7268e-487">jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads.</span></span> <span data-ttu-id="7268e-488">Поэтому динамически создаваемые формы не подвергаются проверке автоматически.</span><span class="sxs-lookup"><span data-stu-id="7268e-488">Therefore, validation doesn't work automatically on dynamically generated forms.</span></span> <span data-ttu-id="7268e-489">Чтобы включить проверку, необходимо указать, что скрипт ненавязчивой проверки jQuery должен анализировать динамическую форму сразу после ее создания.</span><span class="sxs-lookup"><span data-stu-id="7268e-489">To enable validation, tell jQuery Unobtrusive Validation to parse the dynamic form immediately after you create it.</span></span> <span data-ttu-id="7268e-490">Например, приведенный ниже код показывает, как можно настроить проверку на стороне клиента для формы, добавленной посредством AJAX.</span><span class="sxs-lookup"><span data-stu-id="7268e-490">For example, the following code sets up client-side validation on a form added via AJAX.</span></span>

```js
$.get({
    url: "https://url/that/returns/a/form",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add form. " + errorThrown);
    },
    success: function(newFormHTML) {
        var container = document.getElementById("form-container");
        container.insertAdjacentHTML("beforeend", newFormHTML);
        var forms = container.getElementsByTagName("form");
        var newForm = forms[forms.length - 1];
        $.validator.unobtrusive.parse(newForm);
    }
})
```

<span data-ttu-id="7268e-491">Метод `$.validator.unobtrusive.parse()` принимает селектор jQuery в качестве единственного аргумента.</span><span class="sxs-lookup"><span data-stu-id="7268e-491">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="7268e-492">Этот метод предписывает скрипту ненавязчивой проверки jQuery анализировать атрибуты `data-` форм в этом селекторе.</span><span class="sxs-lookup"><span data-stu-id="7268e-492">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="7268e-493">Значения этих атрибутов затем передаются в подключаемый модуль jQuery Validate.</span><span class="sxs-lookup"><span data-stu-id="7268e-493">The values of those attributes are then passed to the jQuery Validate plugin.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="7268e-494">Добавление проверки к динамическим элементам управления</span><span class="sxs-lookup"><span data-stu-id="7268e-494">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="7268e-495">Метод `$.validator.unobtrusive.parse()` обрабатывает всю форму, а не отдельные динамически создаваемые элементы управления, такие как `<input>` и `<select/>`.</span><span class="sxs-lookup"><span data-stu-id="7268e-495">The `$.validator.unobtrusive.parse()` method works on an entire form, not on individual dynamically generated controls, such as `<input>` and `<select/>`.</span></span> <span data-ttu-id="7268e-496">Для повторной обработки формы удалите данные проверки, которые были добавлены при анализе формы ранее, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="7268e-496">To reparse the form, remove the validation data that was added when the form was parsed earlier, as shown in the following example:</span></span>

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add control. " + errorThrown);
    },
    success: function(newInputHTML) {
        var form = document.getElementById("my-form");
        form.insertAdjacentHTML("beforeend", newInputHTML);
        $(form).removeData("validator")    // Added by jQuery Validate
               .removeData("unobtrusiveValidation");   // Added by jQuery Unobtrusive Validation
        $.validator.unobtrusive.parse(form);
    }
})
```

## <a name="custom-client-side-validation"></a><span data-ttu-id="7268e-497">Настраиваемая проверка на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="7268e-497">Custom client-side validation</span></span>

<span data-ttu-id="7268e-498">Настраиваемая проверка на стороне клиента выполняется путем создания атрибутов HTML `data-`, которые работают с настраиваемым адаптером проверки jQuery.</span><span class="sxs-lookup"><span data-stu-id="7268e-498">Custom client-side validation is done by generating `data-` HTML attributes that work with a custom jQuery Validate adapter.</span></span> <span data-ttu-id="7268e-499">В следующем примере кода адаптера используются атрибуты `ClassicMovie` и `ClassicMovie2`, которые были введены ранее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="7268e-499">The following sample adapter code was written for the `ClassicMovie` and `ClassicMovie2` attributes that were introduced earlier in this article:</span></span>

[!code-javascript[](validation/samples/2.x/ValidationSample/wwwroot/js/classicMovieValidator.js?name=snippet_UnobtrusiveValidation)]

<span data-ttu-id="7268e-500">Сведения о способах создания адаптеров см. в [документации по проверкам jQuery](https://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="7268e-500">For information about how to write adapters, see the [jQuery Validate documentation](https://jqueryvalidation.org/documentation/).</span></span>

<span data-ttu-id="7268e-501">Использование адаптера для заданного поля инициируется атрибутами `data-`, которые:</span><span class="sxs-lookup"><span data-stu-id="7268e-501">The use of an adapter for a given field is triggered by `data-` attributes that:</span></span>

* <span data-ttu-id="7268e-502">помечают поле как проходящее проверку (`data-val="true"`);</span><span class="sxs-lookup"><span data-stu-id="7268e-502">Flag the field as being subject to validation (`data-val="true"`).</span></span>
* <span data-ttu-id="7268e-503">указывают имя правила проверки и текст сообщения об ошибке (например, `data-val-rulename="Error message."`);</span><span class="sxs-lookup"><span data-stu-id="7268e-503">Identify a validation rule name and error message text (for example, `data-val-rulename="Error message."`).</span></span>
* <span data-ttu-id="7268e-504">указывают любые дополнительные параметры, в которых нуждается проверяющий элемент управления (например, `data-val-rulename-parm1="value"`).</span><span class="sxs-lookup"><span data-stu-id="7268e-504">Provide any additional parameters the validator needs (for example, `data-val-rulename-parm1="value"`).</span></span>

<span data-ttu-id="7268e-505">В следующем примере показаны атрибуты `data-` для нашего атрибута `ClassicMovie` из примера приложения.</span><span class="sxs-lookup"><span data-stu-id="7268e-505">The following example shows the `data-` attributes for the sample app's `ClassicMovie` attribute:</span></span>

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie1="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie1-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="">
```

<span data-ttu-id="7268e-506">Как отмечалось ранее, [вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) и [вспомогательные методы HTML](xref:mvc/views/overview) используют сведения из атрибутов проверки для подготовки к отрисовке атрибутов `data-`.</span><span class="sxs-lookup"><span data-stu-id="7268e-506">As noted earlier, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use information from validation attributes to render `data-` attributes.</span></span> <span data-ttu-id="7268e-507">Существует два варианта для написания кода, который приводит к созданию настраиваемых атрибутов HTML `data-`.</span><span class="sxs-lookup"><span data-stu-id="7268e-507">There are two options for writing code that results in the creation of custom `data-` HTML attributes:</span></span>

* <span data-ttu-id="7268e-508">Создайте класс, производный от `AttributeAdapterBase<TAttribute>`, и класс, реализующий `IValidationAttributeAdapterProvider`; зарегистрируйте атрибут и его адаптер в функции внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="7268e-508">Create a class that derives from `AttributeAdapterBase<TAttribute>` and a class that implements `IValidationAttributeAdapterProvider`, and register your attribute and its adapter in DI.</span></span> <span data-ttu-id="7268e-509">Этот метод следует за [участником с единственной обязанностью](https://wikipedia.org/wiki/Single_responsibility_principle) в том, что код проверки, связанный с сервером и клиентом, выделен в отдельные классы.</span><span class="sxs-lookup"><span data-stu-id="7268e-509">This method follows the [single responsibility principal](https://wikipedia.org/wiki/Single_responsibility_principle) in that server-related and client-related validation code is in separate classes.</span></span> <span data-ttu-id="7268e-510">Адаптер также имеет следующее преимущество: так как он зарегистрирован в функции внедрения зависимостей, для него при необходимости доступны другие службы в ней.</span><span class="sxs-lookup"><span data-stu-id="7268e-510">The adapter also has the advantage that since it is registered in DI, other services in DI are available to it if needed.</span></span>
* <span data-ttu-id="7268e-511">Реализуйте `IClientModelValidator` в вашем классе `ValidationAttribute`.</span><span class="sxs-lookup"><span data-stu-id="7268e-511">Implement `IClientModelValidator` in your `ValidationAttribute` class.</span></span> <span data-ttu-id="7268e-512">Этот метод стоит использовать, если атрибут не выполняет проверку на стороне сервера и не нуждается в службах из функции внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="7268e-512">This method might be appropriate if the attribute doesn't do any server-side validation and doesn't need any services from DI.</span></span>

### <a name="attributeadapter-for-client-side-validation"></a><span data-ttu-id="7268e-513">AttributeAdapter для проверки на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="7268e-513">AttributeAdapter for client-side validation</span></span>

<span data-ttu-id="7268e-514">Этот метод отрисовки атрибутов `data-` в формате HTML используется атрибутом `ClassicMovie` в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="7268e-514">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie` attribute in the sample app.</span></span> <span data-ttu-id="7268e-515">Чтобы добавить клиентскую проверку с помощью этого метода, сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="7268e-515">To add client validation by using this method:</span></span>

1. <span data-ttu-id="7268e-516">Создайте класс адаптера атрибута для настраиваемого атрибута проверки.</span><span class="sxs-lookup"><span data-stu-id="7268e-516">Create an attribute adapter class for the custom validation attribute.</span></span> <span data-ttu-id="7268e-517">Наследуйте класс от [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="7268e-517">Derive the class from [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span></span> <span data-ttu-id="7268e-518">Создайте метод `AddValidation`, который добавляет атрибуты `data-` для выводимых данных, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="7268e-518">Create an `AddValidation` method that adds `data-` attributes to the rendered output, as shown in this example:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/ClassicMovieAttributeAdapter.cs?name=snippet_ClassicMovieAttributeAdapter)]

1. <span data-ttu-id="7268e-519">Создайте класс поставщика адаптера, который реализует <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span><span class="sxs-lookup"><span data-stu-id="7268e-519">Create an adapter provider class that implements <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span></span> <span data-ttu-id="7268e-520">В методе `GetAttributeAdapter` передайте настраиваемый атрибут в конструктор адаптера, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="7268e-520">In the `GetAttributeAdapter` method pass in the custom attribute to the adapter's constructor, as shown in this example:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/CustomValidationAttributeAdapterProvider.cs?name=snippet_CustomValidationAttributeAdapterProvider)]

1. <span data-ttu-id="7268e-521">Зарегистрируйте поставщик адаптера для внедрения зависимостей в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7268e-521">Register the adapter provider for DI in `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=8-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a><span data-ttu-id="7268e-522">IClientModelValidator для проверки на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="7268e-522">IClientModelValidator for client-side validation</span></span>

<span data-ttu-id="7268e-523">Этот метод отрисовки атрибутов `data-` в формате HTML используется атрибутом `ClassicMovie2` в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="7268e-523">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie2` attribute in the sample app.</span></span> <span data-ttu-id="7268e-524">Чтобы добавить клиентскую проверку с помощью этого метода, сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="7268e-524">To add client validation by using this method:</span></span>

* <span data-ttu-id="7268e-525">В настраиваемом атрибуте проверки реализуйте интерфейс `IClientModelValidator` и создайте метод `AddValidation`.</span><span class="sxs-lookup"><span data-stu-id="7268e-525">In the custom validation attribute, implement the `IClientModelValidator` interface and create an `AddValidation` method.</span></span> <span data-ttu-id="7268e-526">В метод `AddValidation` добавьте атрибуты `data-` для проверки, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="7268e-526">In the `AddValidation` method, add `data-` attributes for validation, as shown in the following example:</span></span>

  [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/ClassicMovie2Attribute.cs?name=snippet_ClassicMovie2Attribute)]

## <a name="disable-client-side-validation"></a><span data-ttu-id="7268e-527">Отключение проверки на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="7268e-527">Disable client-side validation</span></span>

<span data-ttu-id="7268e-528">Следующий код отключает клиентскую проверку в представлениях MVC.</span><span class="sxs-lookup"><span data-stu-id="7268e-528">The following code disables client validation in MVC views:</span></span>

[!code-csharp[](validation/samples_snapshot/2.x/Startup2.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="7268e-529">В Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="7268e-529">And in Razor Pages:</span></span>

[!code-csharp[](validation/samples_snapshot/2.x/Startup3.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="7268e-530">Еще один вариант отключения клиентской проверки клиента — закомментировать ссылку на `_ValidationScriptsPartial` в вашем файле *.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7268e-530">Another option for disabling client validation is to comment out the reference to `_ValidationScriptsPartial` in your *.cshtml* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7268e-531">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="7268e-531">Additional resources</span></span>

* [<span data-ttu-id="7268e-532">Пространство имен System.ComponentModel.DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="7268e-532">System.ComponentModel.DataAnnotations namespace</span></span>](xref:System.ComponentModel.DataAnnotations)
* [<span data-ttu-id="7268e-533">Привязка модели</span><span class="sxs-lookup"><span data-stu-id="7268e-533">Model Binding</span></span>](model-binding.md)

::: moniker-end
