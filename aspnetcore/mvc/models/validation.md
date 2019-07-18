---
title: Проверка модели в ASP.NET Core MVC
author: tdykstra
description: Сведения о проверке модели в ASP.NET Core MVC и Razor Pages.
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2019
monikerRange: '>= aspnetcore-2.1'
uid: mvc/models/validation
ms.openlocfilehash: 43b69e9b7588ad575f203200c5bc59a4272d0066
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814114"
---
# <a name="model-validation-in-aspnet-core-mvc-and-razor-pages"></a><span data-ttu-id="4129d-103">Проверка модели в ASP.NET Core MVC и Razor Pages</span><span class="sxs-lookup"><span data-stu-id="4129d-103">Model validation in ASP.NET Core MVC and Razor Pages</span></span>

<span data-ttu-id="4129d-104">В этой статье объясняется, как осуществлять проверки вводимых пользователем данных в приложении ASP.NET Core MVC или Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="4129d-104">This article explains how to validate user input in an ASP.NET Core MVC or Razor Pages app.</span></span>

<span data-ttu-id="4129d-105">[Просмотреть или скачать пример кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="4129d-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="model-state"></a><span data-ttu-id="4129d-106">Состояние модели</span><span class="sxs-lookup"><span data-stu-id="4129d-106">Model state</span></span>

<span data-ttu-id="4129d-107">Состояние модели представляет ошибки, создаваемые двумя подсистемами: привязкой модели и проверкой модели.</span><span class="sxs-lookup"><span data-stu-id="4129d-107">Model state represents errors that come from two subsystems: model binding and model validation.</span></span> <span data-ttu-id="4129d-108">Ошибки, поступающие из [привязки модели](model-binding.md), чаще всего представляют собой ошибки преобразования данных (например, x вводится в поле, которое ожидает целое число).</span><span class="sxs-lookup"><span data-stu-id="4129d-108">Errors that originate from [model binding](model-binding.md) are generally data conversion errors (for example, an "x" is entered in a field that expects an integer).</span></span> <span data-ttu-id="4129d-109">Проверка модели проводится после привязки модели и сообщает об ошибках, при которых данные не соответствуют бизнес-правилам (например, 0 вводится в поле, которое ожидает оценку от 1 до 5).</span><span class="sxs-lookup"><span data-stu-id="4129d-109">Model validation occurs after model binding and reports errors where the data doesn't conform to business rules (for example, a 0 is entered in a field that expects a rating between 1 and 5).</span></span>

<span data-ttu-id="4129d-110">Привязка модели и проверка модели происходят перед выполнением действия контроллера или метода обработчика Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="4129d-110">Both model binding and validation occur before the execution of a controller action or a Razor Pages handler method.</span></span> <span data-ttu-id="4129d-111">Веб-приложение отвечает за проверку `ModelState.IsValid` и реагирует соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="4129d-111">For web apps, it's the app's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="4129d-112">Веб-приложения обычно повторно отображают страницы с сообщением об ошибке.</span><span class="sxs-lookup"><span data-stu-id="4129d-112">Web apps typically redisplay the page with an error message:</span></span>

[!code-csharp[](validation/sample_snapshot/Create.cshtml.cs?name=snippet&highlight=3-6)]

<span data-ttu-id="4129d-113">Контроллерам веб-API не нужно проверять `ModelState.IsValid` при наличии атрибута `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="4129d-113">Web API controllers don't have to check `ModelState.IsValid` if they have the `[ApiController]` attribute.</span></span> <span data-ttu-id="4129d-114">В этом случае автоматически возвращается ответ HTTP 400, содержащий сведения о проблеме, если состояние модели недопустимо.</span><span class="sxs-lookup"><span data-stu-id="4129d-114">In that case, an automatic HTTP 400 response containing issue details is returned when model state is invalid.</span></span> <span data-ttu-id="4129d-115">Дополнительные сведения см. в разделе [Автоматические отклики HTTP 400](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="4129d-115">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="rerun-validation"></a><span data-ttu-id="4129d-116">Перезапуск проверки</span><span class="sxs-lookup"><span data-stu-id="4129d-116">Rerun validation</span></span>

<span data-ttu-id="4129d-117">Проверка выполняется автоматически, однако может потребоваться повторить ее вручную.</span><span class="sxs-lookup"><span data-stu-id="4129d-117">Validation is automatic, but you might want to repeat it manually.</span></span> <span data-ttu-id="4129d-118">Например, можно вычислить значение свойства и повторно выполнить проверку после установки свойства в вычисляемое значение.</span><span class="sxs-lookup"><span data-stu-id="4129d-118">For example, you might compute a value for a property and want to rerun validation after setting the property to the computed value.</span></span> <span data-ttu-id="4129d-119">Для повторной проверки вызовите метод `TryValidateModel`, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="4129d-119">To rerun validation, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[](validation/sample/Controllers/MoviesController.cs?name=snippet_TryValidateModel&highlight=11)]

## <a name="validation-attributes"></a><span data-ttu-id="4129d-120">Атрибуты проверки</span><span class="sxs-lookup"><span data-stu-id="4129d-120">Validation attributes</span></span>

<span data-ttu-id="4129d-121">Атрибуты проверки позволяют задать правила проверки для свойств модели.</span><span class="sxs-lookup"><span data-stu-id="4129d-121">Validation attributes let you specify validation rules for model properties.</span></span> <span data-ttu-id="4129d-122">В следующем примере из [примера приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) показан класс модели, который помечается с помощью атрибутов проверки.</span><span class="sxs-lookup"><span data-stu-id="4129d-122">The following example from [the sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) shows a model class that is annotated with validation attributes.</span></span> <span data-ttu-id="4129d-123">`[ClassicMovie]` является настраиваемым атрибутом проверки; остальные являются встроенными.</span><span class="sxs-lookup"><span data-stu-id="4129d-123">The `[ClassicMovie]` attribute is a custom validation attribute and the others are built-in.</span></span> <span data-ttu-id="4129d-124">(Не показан `[ClassicMovie2]`, который демонстрирует альтернативный способ реализации настраиваемого атрибута.)</span><span class="sxs-lookup"><span data-stu-id="4129d-124">(Not shown is `[ClassicMovie2]`, which shows an alternative way to implement a custom attribute.)</span></span>

[!code-csharp[](validation/sample/Models/Movie.cs?name=snippet_ModelClass)]

## <a name="built-in-attributes"></a><span data-ttu-id="4129d-125">Встроенные атрибуты</span><span class="sxs-lookup"><span data-stu-id="4129d-125">Built-in attributes</span></span>

<span data-ttu-id="4129d-126">Ниже приведены некоторые из встроенных атрибутов проверки.</span><span class="sxs-lookup"><span data-stu-id="4129d-126">Here are some of the built-in validation attributes:</span></span>

* <span data-ttu-id="4129d-127">`[CreditCard]`: проверяет, имеет ли свойство формат кредитной карты.</span><span class="sxs-lookup"><span data-stu-id="4129d-127">`[CreditCard]`: Validates that the property has a credit card format.</span></span>
* <span data-ttu-id="4129d-128">`[Compare]`: проверяет, совпадают ли два свойства модели.</span><span class="sxs-lookup"><span data-stu-id="4129d-128">`[Compare]`: Validates that two properties in a model match.</span></span>
* <span data-ttu-id="4129d-129">`[EmailAddress]`: проверяет, имеет ли свойство формат адреса электронной почты.</span><span class="sxs-lookup"><span data-stu-id="4129d-129">`[EmailAddress]`: Validates that the property has an email format.</span></span>
* <span data-ttu-id="4129d-130">`[Phone]`: проверяет, имеет ли свойство формат номера телефона.</span><span class="sxs-lookup"><span data-stu-id="4129d-130">`[Phone]`: Validates that the property has a telephone number format.</span></span>
* <span data-ttu-id="4129d-131">`[Range]`: проверяет, находится ли значение свойства в указанном диапазоне.</span><span class="sxs-lookup"><span data-stu-id="4129d-131">`[Range]`: Validates that the property value falls within a specified range.</span></span>
* <span data-ttu-id="4129d-132">`[RegularExpression]`: проверяет, соответствует ли значение свойства указанному регулярному выражению.</span><span class="sxs-lookup"><span data-stu-id="4129d-132">`[RegularExpression]`: Validates that the property value matches a specified regular expression.</span></span>
* <span data-ttu-id="4129d-133">`[Required]`: проверяет, что поле не равно NULL.</span><span class="sxs-lookup"><span data-stu-id="4129d-133">`[Required]`: Validates that the field is not null.</span></span> <span data-ttu-id="4129d-134">См. в разделе [Атрибут [Required]](#required-attribute) дополнительные сведения о поведении этого атрибута.</span><span class="sxs-lookup"><span data-stu-id="4129d-134">See [[Required] attribute](#required-attribute) for details about this attribute's behavior.</span></span>
* <span data-ttu-id="4129d-135">`[StringLength]`: проверяет, что значение свойства строки не превышает ограничение по указанной длине.</span><span class="sxs-lookup"><span data-stu-id="4129d-135">`[StringLength]`: Validates that a string property value doesn't exceed a specified length limit.</span></span>
* <span data-ttu-id="4129d-136">`[Url]`: проверяет, имеет ли свойство формат URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="4129d-136">`[Url]`: Validates that the property has a URL format.</span></span>
* <span data-ttu-id="4129d-137">`[Remote]`: проверяет входные данные на клиенте путем вызова метода действия на сервере.</span><span class="sxs-lookup"><span data-stu-id="4129d-137">`[Remote]`: Validates input on the client by calling an action method on the server.</span></span> <span data-ttu-id="4129d-138">См. в разделе [Атрибут [Remote]](#remote-attribute) дополнительные сведения о поведении этого атрибута.</span><span class="sxs-lookup"><span data-stu-id="4129d-138">See [[Remote] attribute](#remote-attribute) for details about this attribute's behavior.</span></span>

<span data-ttu-id="4129d-139">Полный перечень атрибутов проверки можно найти в пространстве имен [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations).</span><span class="sxs-lookup"><span data-stu-id="4129d-139">A complete list of validation attributes can be found in the [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) namespace.</span></span>

### <a name="error-messages"></a><span data-ttu-id="4129d-140">Сообщения об ошибках</span><span class="sxs-lookup"><span data-stu-id="4129d-140">Error messages</span></span>

<span data-ttu-id="4129d-141">Атрибуты проверки позволяют указать сообщение об ошибке, которое будет отображаться, если входные данные недопустимы.</span><span class="sxs-lookup"><span data-stu-id="4129d-141">Validation attributes let you specify the error message to be displayed for invalid input.</span></span> <span data-ttu-id="4129d-142">Например:</span><span class="sxs-lookup"><span data-stu-id="4129d-142">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

<span data-ttu-id="4129d-143">На внутреннем уровне атрибуты вызывают `String.Format` с заполнителем для имени поля и иногда дополнительным заполнителями.</span><span class="sxs-lookup"><span data-stu-id="4129d-143">Internally, the attributes call `String.Format` with a placeholder for the field name and sometimes additional placeholders.</span></span> <span data-ttu-id="4129d-144">Например:</span><span class="sxs-lookup"><span data-stu-id="4129d-144">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

<span data-ttu-id="4129d-145">При применении к свойству `Name` сообщение об ошибке, созданное в приведенном выше коде, имело бы вид Name length must be between 6 and 8 (Длина имени должна быть от 6 до 8).</span><span class="sxs-lookup"><span data-stu-id="4129d-145">When applied to a `Name` property, the error message created by the preceding code would be "Name length must be between 6 and 8.".</span></span>

<span data-ttu-id="4129d-146">Чтобы узнать, какие параметры передаются в `String.Format` для сообщения об ошибке определенного атрибута, см. раздел [Исходный код DataAnnotations](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span><span class="sxs-lookup"><span data-stu-id="4129d-146">To find out which parameters are passed to `String.Format` for a particular attribute's error message, see the [DataAnnotations source code](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span></span>

## <a name="required-attribute"></a><span data-ttu-id="4129d-147">Атрибут [Required]</span><span class="sxs-lookup"><span data-stu-id="4129d-147">[Required] attribute</span></span>

<span data-ttu-id="4129d-148">По умолчанию система проверки рассматривает не допускающие значение NULL параметры или свойства так, как если бы они имели атрибут `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="4129d-148">By default, the validation system treats non-nullable parameters or properties as if they had a `[Required]` attribute.</span></span> <span data-ttu-id="4129d-149">[Типы значений](/dotnet/csharp/language-reference/keywords/value-types), например `decimal` и `int`, не поддерживают значение NULL.</span><span class="sxs-lookup"><span data-stu-id="4129d-149">[Value types](/dotnet/csharp/language-reference/keywords/value-types) such as `decimal` and `int` are non-nullable.</span></span>

### <a name="required-validation-on-the-server"></a><span data-ttu-id="4129d-150">Проверка [Required] на сервере</span><span class="sxs-lookup"><span data-stu-id="4129d-150">[Required] validation on the server</span></span>

<span data-ttu-id="4129d-151">На сервере обязательное значение считается отсутствующим, если свойство имеет значение NULL.</span><span class="sxs-lookup"><span data-stu-id="4129d-151">On the server, a required value is considered missing if the property is null.</span></span> <span data-ttu-id="4129d-152">Поле, не допускающее значения NULL, всегда является допустимым, и сообщение об ошибке атрибута [Required] никогда не выводится.</span><span class="sxs-lookup"><span data-stu-id="4129d-152">A non-nullable field is always valid, and the [Required] attribute's error message is never displayed.</span></span>

<span data-ttu-id="4129d-153">Тем не менее привязка модели для ненулевого свойства может завершиться ошибкой, приводящей к сообщению об ошибке, например `The value '' is invalid`.</span><span class="sxs-lookup"><span data-stu-id="4129d-153">However, model binding for a non-nullable property may fail, resulting in an error message such as `The value '' is invalid`.</span></span> <span data-ttu-id="4129d-154">Чтобы задать настраиваемое сообщение об ошибке во время проверки не допускающих значения NULL типов на стороне сервера, у вас есть следующие варианты.</span><span class="sxs-lookup"><span data-stu-id="4129d-154">To specify a custom error message for server-side validation of non-nullable types, you have the following options:</span></span>

* <span data-ttu-id="4129d-155">Сделать поле допускающим значение NULL (например, `decimal?` вместо `decimal`).</span><span class="sxs-lookup"><span data-stu-id="4129d-155">Make the field nullable (for example, `decimal?` instead of `decimal`).</span></span> <span data-ttu-id="4129d-156">Типы значений [Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) обрабатываются как стандартные типы, допускающие значение NULL.</span><span class="sxs-lookup"><span data-stu-id="4129d-156">[Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) value types are treated like standard nullable types.</span></span>
* <span data-ttu-id="4129d-157">Указать сообщение об ошибке по умолчанию для использования в привязке модели, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="4129d-157">Specify the default error message to be used by model binding, as shown in the following example:</span></span>

  [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=4-5)]

  <span data-ttu-id="4129d-158">Дополнительные сведения об ошибках привязки модели, у которых можно задать сообщение по умолчанию, см. в разделе <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span><span class="sxs-lookup"><span data-stu-id="4129d-158">For more information about model binding errors that you can set default messages for, see <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span></span>

### <a name="required-validation-on-the-client"></a><span data-ttu-id="4129d-159">Проверка [Required] на клиенте</span><span class="sxs-lookup"><span data-stu-id="4129d-159">[Required] validation on the client</span></span>

<span data-ttu-id="4129d-160">Типы и строки, не допускающие значение NULL, обрабатываются на клиенте не так, как на сервере.</span><span class="sxs-lookup"><span data-stu-id="4129d-160">Non-nullable types and strings are handled differently on the client compared to the server.</span></span> <span data-ttu-id="4129d-161">На клиенте:</span><span class="sxs-lookup"><span data-stu-id="4129d-161">On the client:</span></span>

* <span data-ttu-id="4129d-162">Значение считается присутствующим только в том случае, если для него вводятся данные.</span><span class="sxs-lookup"><span data-stu-id="4129d-162">A value is considered present only if input is entered for it.</span></span> <span data-ttu-id="4129d-163">Таким образом, проверка на стороне клиента обрабатывает типы, не допускающие значение NULL, так же, как обнуляемые типы.</span><span class="sxs-lookup"><span data-stu-id="4129d-163">Therefore, client-side validation handles non-nullable types the same as nullable types.</span></span>
* <span data-ttu-id="4129d-164">Пробелы в поле строки считаются допустимыми входными данными при проверке методом jQuery [required](https://jqueryvalidation.org/required-method/).</span><span class="sxs-lookup"><span data-stu-id="4129d-164">Whitespace in a string field is considered valid input by the jQuery Validation [required](https://jqueryvalidation.org/required-method/) method.</span></span> <span data-ttu-id="4129d-165">Проверка на стороне сервера считает, что обязательное строковое поле недопустимо, если введены только пробелы.</span><span class="sxs-lookup"><span data-stu-id="4129d-165">Server-side validation considers a required string field invalid if only whitespace is entered.</span></span>

<span data-ttu-id="4129d-166">Как отмечалось ранее, не допускающие значение NULL типы рассматриваются как имеющие атрибут `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="4129d-166">As noted earlier, non-nullable types are treated as though they had a `[Required]` attribute.</span></span> <span data-ttu-id="4129d-167">Это означает, что вы получаете проверку на стороне клиента, даже если не применять атрибут `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="4129d-167">That means you get client-side validation even if you don't apply the `[Required]` attribute.</span></span> <span data-ttu-id="4129d-168">Но если вы не используете атрибут, вы получаете сообщение об ошибке по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="4129d-168">But if you don't use the attribute, you get a default error message.</span></span> <span data-ttu-id="4129d-169">Чтобы задать настраиваемое сообщение об ошибке, используйте атрибут.</span><span class="sxs-lookup"><span data-stu-id="4129d-169">To specify a custom error message, use the attribute.</span></span>

## <a name="remote-attribute"></a><span data-ttu-id="4129d-170">Атрибут [Remote]</span><span class="sxs-lookup"><span data-stu-id="4129d-170">[Remote] attribute</span></span>

<span data-ttu-id="4129d-171">Атрибут `[Remote]` реализует проверку на стороне клиента, в ходе которой требуется вызвать метод на сервере, чтобы определить, является ли допустимым поле ввода.</span><span class="sxs-lookup"><span data-stu-id="4129d-171">The `[Remote]` attribute implements client-side validation that requires calling a method on the server to determine whether field input is valid.</span></span> <span data-ttu-id="4129d-172">Например, приложению может потребоваться проверить, занято ли имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="4129d-172">For example, the app may need to verify whether a user name is already in use.</span></span>

<span data-ttu-id="4129d-173">Для реализации удаленной проверки сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="4129d-173">To implement remote validation:</span></span>

1. <span data-ttu-id="4129d-174">Создайте для вызова из JavaScript метод действия.</span><span class="sxs-lookup"><span data-stu-id="4129d-174">Create an action method for JavaScript to call.</span></span>  <span data-ttu-id="4129d-175">Метод проверки jQuery [remote](https://jqueryvalidation.org/remote-method/) ожидает ответ JSON.</span><span class="sxs-lookup"><span data-stu-id="4129d-175">The jQuery Validate [remote](https://jqueryvalidation.org/remote-method/) method expects a JSON response:</span></span>

   * <span data-ttu-id="4129d-176">`"true"` означает, что входные данные допустимы.</span><span class="sxs-lookup"><span data-stu-id="4129d-176">`"true"` means the input data is valid.</span></span>
   * <span data-ttu-id="4129d-177">`"false"`, `undefined` или `null` означают, что входные данные недопустимы.</span><span class="sxs-lookup"><span data-stu-id="4129d-177">`"false"`, `undefined`, or `null` means the input is invalid.</span></span>  <span data-ttu-id="4129d-178">Вывод стандартного сообщения об ошибке</span><span class="sxs-lookup"><span data-stu-id="4129d-178">Display the default error message.</span></span>
   * <span data-ttu-id="4129d-179">Все прочие значения означают, что входные данные недопустимы.</span><span class="sxs-lookup"><span data-stu-id="4129d-179">Any other string means the input is invalid.</span></span> <span data-ttu-id="4129d-180">Вывод строки как настраиваемого сообщения об ошибке.</span><span class="sxs-lookup"><span data-stu-id="4129d-180">Display the string as a custom error message.</span></span>

   <span data-ttu-id="4129d-181">Ниже приведен пример метода действия, который возвращает настраиваемое сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="4129d-181">Here's an example of an action method that returns a custom error message:</span></span>

   [!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. <span data-ttu-id="4129d-182">В классе модели пометьте свойство атрибутом `[Remote]`, указывающим метод действия проверки, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="4129d-182">In the model class, annotate the property with a `[Remote]` attribute that points to the validation action method, as shown in the following example:</span></span>

   [!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserEmailProperty)]
 
   <span data-ttu-id="4129d-183">Атрибут `[Remote]` находится в пространстве имен `Microsoft.AspNetCore.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="4129d-183">The `[Remote]` attribute is in the `Microsoft.AspNetCore.Mvc` namespace.</span></span> <span data-ttu-id="4129d-184">Установите пакет NuGet [Microsoft.AspNetCore.Mvc.ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures), если вы не используете метапакет `Microsoft.AspNetCore.App` или `Microsoft.AspNetCore.All`.</span><span class="sxs-lookup"><span data-stu-id="4129d-184">Install the [Microsoft.AspNetCore.Mvc.ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures) NuGet package if you're not using the `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All` metapackage.</span></span>
   
### <a name="additional-fields"></a><span data-ttu-id="4129d-185">Дополнительные поля</span><span class="sxs-lookup"><span data-stu-id="4129d-185">Additional fields</span></span>

<span data-ttu-id="4129d-186">Свойство `AdditionalFields` атрибута `[Remote]` позволяет проверять сочетания полей с данными на сервере.</span><span class="sxs-lookup"><span data-stu-id="4129d-186">The `AdditionalFields` property of the `[Remote]` attribute lets you validate combinations of fields against data on the server.</span></span> <span data-ttu-id="4129d-187">Например, если бы в модели `User` были свойства `FirstName` и `LastName`, могла бы возникнуть необходимость проверить, нет ли уже пользователя с такой парой имен.</span><span class="sxs-lookup"><span data-stu-id="4129d-187">For example, if the `User` model had `FirstName` and `LastName` properties, you might want to verify that no existing users already have that pair of names.</span></span> <span data-ttu-id="4129d-188">В следующем примере показано использование `AdditionalFields`.</span><span class="sxs-lookup"><span data-stu-id="4129d-188">The following example shows how to use `AdditionalFields`:</span></span>

[!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserNameProperties)]

<span data-ttu-id="4129d-189">Свойству `AdditionalFields` можно было бы явным образом присвоить строки `"FirstName"` и `"LastName"`, однако применение оператора [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) упрощает дальнейший рефакторинг.</span><span class="sxs-lookup"><span data-stu-id="4129d-189">`AdditionalFields` could be set explicitly to the strings `"FirstName"` and `"LastName"`, but using the [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) operator simplifies later refactoring.</span></span> <span data-ttu-id="4129d-190">Методу действия для этой проверки необходимо принимать аргументы для имени и фамилии.</span><span class="sxs-lookup"><span data-stu-id="4129d-190">The action method for this validation must accept both first name and last name arguments:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyName)]

<span data-ttu-id="4129d-191">Когда пользователь вводит имя или фамилию, JavaScript выполняет удаленный вызов, чтобы увидеть, будет ли эта пара принята.</span><span class="sxs-lookup"><span data-stu-id="4129d-191">When the user enters a first or last name, JavaScript makes a remote call to see if that pair of names has been taken.</span></span>

<span data-ttu-id="4129d-192">Чтобы проверить несколько дополнительных полей, их следует указывать в виде списка с разделителями-запятыми.</span><span class="sxs-lookup"><span data-stu-id="4129d-192">To validate two or more additional fields, provide them as a comma-delimited list.</span></span> <span data-ttu-id="4129d-193">Например, чтобы добавить в модель свойство `MiddleName`, задайте атрибут `[Remote]`, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="4129d-193">For example, to add a `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following example:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="4129d-194">`AdditionalFields`, как и все аргументы атрибутов, должен представлять собой константное выражение.</span><span class="sxs-lookup"><span data-stu-id="4129d-194">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span> <span data-ttu-id="4129d-195">Поэтому не следует использовать [интерполированную строку](/dotnet/csharp/language-reference/keywords/interpolated-strings) или вызов <xref:System.String.Join*>для инициализации `AdditionalFields`.</span><span class="sxs-lookup"><span data-stu-id="4129d-195">Therefore, don't use an [interpolated string](/dotnet/csharp/language-reference/keywords/interpolated-strings) or call <xref:System.String.Join*> to initialize `AdditionalFields`.</span></span>

## <a name="alternatives-to-built-in-attributes"></a><span data-ttu-id="4129d-196">Альтернативы для встроенных атрибутов</span><span class="sxs-lookup"><span data-stu-id="4129d-196">Alternatives to built-in attributes</span></span>

<span data-ttu-id="4129d-197">Если вам нужна проверка, которую не предоставляют встроенные атрибуты, вы можете следующее.</span><span class="sxs-lookup"><span data-stu-id="4129d-197">If you need validation not provided by built-in attributes, you can:</span></span>

* <span data-ttu-id="4129d-198">[Создать настраиваемые атрибуты](#custom-attributes).</span><span class="sxs-lookup"><span data-stu-id="4129d-198">[Create custom attributes](#custom-attributes).</span></span>
* <span data-ttu-id="4129d-199">[Реализовать IValidatableObject](#ivalidatableobject).</span><span class="sxs-lookup"><span data-stu-id="4129d-199">[Implement IValidatableObject](#ivalidatableobject).</span></span>

## <a name="custom-attributes"></a><span data-ttu-id="4129d-200">Настраиваемые атрибуты</span><span class="sxs-lookup"><span data-stu-id="4129d-200">Custom attributes</span></span>

<span data-ttu-id="4129d-201">Для сценариев, где не годятся встроенные атрибуты проверки, можно создать настраиваемые атрибуты.</span><span class="sxs-lookup"><span data-stu-id="4129d-201">For scenarios that the built-in validation attributes don't handle, you can create custom validation attributes.</span></span> <span data-ttu-id="4129d-202">Создайте класс, наследуемый от <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, и переопределите метод <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*>.</span><span class="sxs-lookup"><span data-stu-id="4129d-202">Create a class that inherits from <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, and override the <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> method.</span></span>

<span data-ttu-id="4129d-203">Метод `IsValid` принимает объект с именем *value*, который является входными данными для проверки.</span><span class="sxs-lookup"><span data-stu-id="4129d-203">The `IsValid` method accepts an object named *value*, which is the input to be validated.</span></span> <span data-ttu-id="4129d-204">Перегрузка также принимает объект `ValidationContext`, который предоставляет дополнительные сведения, такие как экземпляр модели, созданный с помощью привязки модели.</span><span class="sxs-lookup"><span data-stu-id="4129d-204">An overload also accepts a `ValidationContext` object, which provides additional information, such as the model instance created by model binding.</span></span>

<span data-ttu-id="4129d-205">В следующем примере проверяется, что дата выпуска фильмов в *классическом* жанре задана не позднее указанного года.</span><span class="sxs-lookup"><span data-stu-id="4129d-205">The following example validates that the release date for a movie in the *Classic* genre isn't later than a specified year.</span></span> <span data-ttu-id="4129d-206">Атрибут `[ClassicMovie2]` сначала проверяет жанр и продолжает проверку, только если он *классический*.</span><span class="sxs-lookup"><span data-stu-id="4129d-206">The `[ClassicMovie2]` attribute checks the genre first and continues only if it's *Classic*.</span></span> <span data-ttu-id="4129d-207">У фильмов, попавших в классику, система проверяет даты выпуска, чтобы убедиться в том, что она не является более поздней, чем ограничение, переданное в конструктор атрибута.</span><span class="sxs-lookup"><span data-stu-id="4129d-207">For movies identified as classics, it checks the release date to make sure it's not later than the limit passed to the attribute constructor.)</span></span>

[!code-csharp[](validation/sample/Attributes/ClassicMovieAttribute.cs?name=snippet_ClassicMovieAttribute)]

<span data-ttu-id="4129d-208">Приведенная выше переменная `movie` представляет объект `Movie`, который содержит данные из переданной формы.</span><span class="sxs-lookup"><span data-stu-id="4129d-208">The `movie` variable in the preceding example represents a `Movie` object that contains the data from the form submission.</span></span> <span data-ttu-id="4129d-209">Метод `IsValid` проверяет дату и жанр.</span><span class="sxs-lookup"><span data-stu-id="4129d-209">The `IsValid` method checks the date and genre.</span></span> <span data-ttu-id="4129d-210">После успешной проверки `IsValid` возвращает код `ValidationResult.Success`.</span><span class="sxs-lookup"><span data-stu-id="4129d-210">Upon successful validation, `IsValid` returns a `ValidationResult.Success` code.</span></span> <span data-ttu-id="4129d-211">Если проверка завершается неудачно, возвращается `ValidationResult` с сообщением об ошибке.</span><span class="sxs-lookup"><span data-stu-id="4129d-211">When validation fails, a `ValidationResult` with an error message is returned.</span></span>

## <a name="ivalidatableobject"></a><span data-ttu-id="4129d-212">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="4129d-212">IValidatableObject</span></span>

<span data-ttu-id="4129d-213">Предыдущий пример работает только с типами `Movie`.</span><span class="sxs-lookup"><span data-stu-id="4129d-213">The preceding example works only with `Movie` types.</span></span> <span data-ttu-id="4129d-214">Другой вариант для проверки на уровне класса — реализация `IValidatableObject` в классе модели, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="4129d-214">Another option for class-level validation is to implement `IValidatableObject` in the model class, as shown in the following example:</span></span>

[!code-csharp[](validation/sample/Models/MovieIValidatable.cs?name=snippet&highlight=1,26-34)]

## <a name="top-level-node-validation"></a><span data-ttu-id="4129d-215">Проверка узлов верхнего уровня</span><span class="sxs-lookup"><span data-stu-id="4129d-215">Top-level node validation</span></span>

<span data-ttu-id="4129d-216">Узлы верхнего уровня содержат:</span><span class="sxs-lookup"><span data-stu-id="4129d-216">Top-level nodes include:</span></span>

* <span data-ttu-id="4129d-217">параметры действия;</span><span class="sxs-lookup"><span data-stu-id="4129d-217">Action parameters</span></span>
* <span data-ttu-id="4129d-218">свойства контроллера;</span><span class="sxs-lookup"><span data-stu-id="4129d-218">Controller properties</span></span>
* <span data-ttu-id="4129d-219">параметры обработчика страниц;</span><span class="sxs-lookup"><span data-stu-id="4129d-219">Page handler parameters</span></span>
* <span data-ttu-id="4129d-220">свойства страничной модели.</span><span class="sxs-lookup"><span data-stu-id="4129d-220">Page model properties</span></span>

<span data-ttu-id="4129d-221">Проверка привязанных к модели узлов верхнего уровня осуществляется наряду с проверкой свойств модели.</span><span class="sxs-lookup"><span data-stu-id="4129d-221">Model-bound top-level nodes are validated in addition to validating model properties.</span></span> <span data-ttu-id="4129d-222">В следующем примере, взятом из примера приложения, метод `VerifyPhone` использует класс <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> для проверки параметра действия `phone`.</span><span class="sxs-lookup"><span data-stu-id="4129d-222">In the following example from the sample app, the `VerifyPhone` method uses the <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> to validate the `phone` action parameter:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

<span data-ttu-id="4129d-223">Узлы верхнего уровня могут применять класс <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> с атрибутами проверки.</span><span class="sxs-lookup"><span data-stu-id="4129d-223">Top-level nodes can use <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> with validation attributes.</span></span> <span data-ttu-id="4129d-224">В следующем примере, взятом из примера приложения, метод `CheckAge` указывает, что при отправке формы параметр `age` должен быть привязан из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="4129d-224">In the following example from the sample app, the `CheckAge` method specifies that the `age` parameter must be bound from the query string when the form is submitted:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_CheckAge)]

<span data-ttu-id="4129d-225">На странице "Check Age" (Проверка возраста) (*CheckAge.cshtml*) находятся две формы.</span><span class="sxs-lookup"><span data-stu-id="4129d-225">In the Check Age page (*CheckAge.cshtml*), there are two forms.</span></span> <span data-ttu-id="4129d-226">Первая форма отправляет значение `Age`, равное `99`, в виде строки запроса: `https://localhost:5001/Users/CheckAge?Age=99`.</span><span class="sxs-lookup"><span data-stu-id="4129d-226">The first form submits an `Age` value of `99` as a query string: `https://localhost:5001/Users/CheckAge?Age=99`.</span></span>

<span data-ttu-id="4129d-227">Если из строки запроса отправлен параметр `age` в правильном формате, форма проходит проверку.</span><span class="sxs-lookup"><span data-stu-id="4129d-227">When a properly formatted `age` parameter from the query string is submitted, the form validates.</span></span>

<span data-ttu-id="4129d-228">Вторая форма на странице "Check Age" (Проверка возраста) отправляет значение `Age` в теле запроса, и проверка завершается сбоем.</span><span class="sxs-lookup"><span data-stu-id="4129d-228">The second form on the Check Age page submits the `Age` value in the body of the request, and validation fails.</span></span> <span data-ttu-id="4129d-229">Ошибка привязки связана с тем, что параметр `age` должен поступать из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="4129d-229">Binding fails because the `age` parameter must come from a query string.</span></span>

<span data-ttu-id="4129d-230">При работе с `CompatibilityVersion.Version_2_1` или более поздней версией проверка узла верхнего уровня по умолчанию включена.</span><span class="sxs-lookup"><span data-stu-id="4129d-230">When running with `CompatibilityVersion.Version_2_1` or later, top-level node validation is enabled by default.</span></span> <span data-ttu-id="4129d-231">В противном случае проверка узла верхнего уровня отключена.</span><span class="sxs-lookup"><span data-stu-id="4129d-231">Otherwise, top-level node validation is disabled.</span></span> <span data-ttu-id="4129d-232">Параметр по умолчанию можно переопределить, задав свойство <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> в (`Startup.ConfigureServices`), как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="4129d-232">The default option can be overridden by setting the <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> property in (`Startup.ConfigureServices`), as shown here:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup.cs?name=snippet_AddMvc&highlight=4)]

## <a name="maximum-errors"></a><span data-ttu-id="4129d-233">Максимальное количество ошибок</span><span class="sxs-lookup"><span data-stu-id="4129d-233">Maximum errors</span></span>

<span data-ttu-id="4129d-234">При достижении максимального количества ошибок (по умолчанию 200) проверка прекращается.</span><span class="sxs-lookup"><span data-stu-id="4129d-234">Validation stops when the maximum number of errors is reached (200 by default).</span></span> <span data-ttu-id="4129d-235">Это число можно изменить с помощью следующего кода в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="4129d-235">You can configure this number with the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=3)]

## <a name="maximum-recursion"></a><span data-ttu-id="4129d-236">Максимальная рекурсия</span><span class="sxs-lookup"><span data-stu-id="4129d-236">Maximum recursion</span></span>

<span data-ttu-id="4129d-237"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> проходит через граф объектов в проверяемой модели.</span><span class="sxs-lookup"><span data-stu-id="4129d-237"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> traverses the object graph of the model being validated.</span></span> <span data-ttu-id="4129d-238">У моделей, которые очень глубоки или содержат бесконечную рекурсию, в ходе проверки может произойти переполнение стека.</span><span class="sxs-lookup"><span data-stu-id="4129d-238">For models that are very deep or are infinitely recursive, validation may result in stack overflow.</span></span> <span data-ttu-id="4129d-239">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) предоставляет способ остановить проверку до превышения настроенной глубины рекурсии обхода.</span><span class="sxs-lookup"><span data-stu-id="4129d-239">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) provides a way to stop validation early if the visitor recursion exceeds a configured depth.</span></span> <span data-ttu-id="4129d-240">Значение `MvcOptions.MaxValidationDepth` по умолчанию — 200 при работе в `CompatibilityVersion.Version_2_2` или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="4129d-240">The default value of `MvcOptions.MaxValidationDepth` is 200 when running with `CompatibilityVersion.Version_2_2` or later.</span></span> <span data-ttu-id="4129d-241">Для более ранних версий значение равно NULL; это означает отсутствие ограничения глубины.</span><span class="sxs-lookup"><span data-stu-id="4129d-241">For earlier versions, the value is null, which means no depth constraint.</span></span>

## <a name="automatic-short-circuit"></a><span data-ttu-id="4129d-242">Автоматическое сокращение</span><span class="sxs-lookup"><span data-stu-id="4129d-242">Automatic short-circuit</span></span>

<span data-ttu-id="4129d-243">Проверка автоматически сокращается (пропускается), если граф модели не требует проверки.</span><span class="sxs-lookup"><span data-stu-id="4129d-243">Validation is automatically short-circuited (skipped) if the model graph doesn't require validation.</span></span> <span data-ttu-id="4129d-244">К числу объектов, которые среда выполнения пропускает при проверке, относятся коллекции примитивов (такие как `byte[]`, `string[]`, `Dictionary<string, string>`) и сложные графы объектов, которые не имеют проверяющих элементов управления.</span><span class="sxs-lookup"><span data-stu-id="4129d-244">Objects that the runtime skips validation for include collections of primitives (such as `byte[]`, `string[]`, `Dictionary<string, string>`) and complex object graphs that don't have any validators.</span></span>

## <a name="disable-validation"></a><span data-ttu-id="4129d-245">Отключение проверки</span><span class="sxs-lookup"><span data-stu-id="4129d-245">Disable validation</span></span>

<span data-ttu-id="4129d-246">Чтобы отключить проверку, сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="4129d-246">To disable validation:</span></span>

1. <span data-ttu-id="4129d-247">Создайте реализацию интерфейса `IObjectModelValidator`, которая не помечает поля как недопустимые.</span><span class="sxs-lookup"><span data-stu-id="4129d-247">Create an implementation of `IObjectModelValidator` that doesn't mark any fields as invalid.</span></span>

   [!code-csharp[](validation/sample/Attributes/NullObjectModelValidator.cs?name=snippet_DisableValidation)]

1. <span data-ttu-id="4129d-248">Добавьте следующий код в `Startup.ConfigureServices` для замены реализации `IObjectModelValidator` по умолчанию в контейнере внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="4129d-248">Add the following code to `Startup.ConfigureServices` to replace the default `IObjectModelValidator` implementation in the dependency injection container.</span></span>

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_DisableValidation)]

<span data-ttu-id="4129d-249">По-прежнему могут отображаться ошибки состояния модели, которые идут из привязки модели.</span><span class="sxs-lookup"><span data-stu-id="4129d-249">You might still see model state errors that originate from model binding.</span></span>

## <a name="client-side-validation"></a><span data-ttu-id="4129d-250">Проверка на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="4129d-250">Client-side validation</span></span>

<span data-ttu-id="4129d-251">Проверка на стороне клиента не позволяет отправлять форму, пока ее данные не будут допустимыми.</span><span class="sxs-lookup"><span data-stu-id="4129d-251">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="4129d-252">При нажатии кнопки "Отправить" выполняется код JavaScript, который либо отправляет форму, либо выводит сообщения об ошибках.</span><span class="sxs-lookup"><span data-stu-id="4129d-252">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="4129d-253">Проверка на стороне клиента позволяет избежать ненужного кругового захода на сервер при наличии ошибки ввода в форме.</span><span class="sxs-lookup"><span data-stu-id="4129d-253">Client-side validation avoids an unnecessary round trip to the server when there are input errors on a form.</span></span> <span data-ttu-id="4129d-254">Ссылки на следующий скрипт в *_Layout.cshtml* и *_ValidationScriptsPartial.cshtml* поддерживают проверку на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="4129d-254">The following script references in *_Layout.cshtml* and *_ValidationScriptsPartial.cshtml* support client-side validation:</span></span>

[!code-cshtml[](validation/sample/Views/Shared/_Layout.cshtml?name=snippet_ScriptTag)]

[!code-cshtml[](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_ScriptTags)]

<span data-ttu-id="4129d-255">Скрипт [ненавязчивой проверки jQuery](https://github.com/aspnet/jquery-validation-unobtrusive) — это настраиваемая интерфейсная библиотека Майкрософт, которая основана на популярном подключаемом модуле [jQuery Validate](https://jqueryvalidation.org/).</span><span class="sxs-lookup"><span data-stu-id="4129d-255">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="4129d-256">Без скрипта ненавязчивой проверки jQuery одну и ту же логику проверки приходилось бы реализовывать в двух местах: в атрибутах проверки для свойств модели на стороне сервера, а затем еще раз в скриптах на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="4129d-256">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server-side validation attributes on model properties, and then again in client-side scripts.</span></span> <span data-ttu-id="4129d-257">Вместо этого [вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) и [вспомогательные функции HTML](xref:mvc/views/overview) могут использовать атрибуты проверки и метаданные типов из свойств модели для обработки атрибутов `data-` HTML 5 в элементах форм, требующих проверки.</span><span class="sxs-lookup"><span data-stu-id="4129d-257">Instead, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use the validation attributes and type metadata from model properties to render HTML 5 `data-` attributes for the form elements that need validation.</span></span> <span data-ttu-id="4129d-258">Скрипт ненавязчивой проверки jQuery анализирует эти атрибуты `data-` и передает логику в подключаемый модуль jQuery Validate, по сути копируя логику проверки на стороне сервера в клиент.</span><span class="sxs-lookup"><span data-stu-id="4129d-258">jQuery Unobtrusive Validation parses the `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server-side validation logic to the client.</span></span> <span data-ttu-id="4129d-259">Ошибки проверки могут выводиться в клиенте с помощью вспомогательных функций тегов, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="4129d-259">You can display validation errors on the client using tag helpers as shown here:</span></span>

[!code-cshtml[](validation/sample/Views/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=4-5)]

<span data-ttu-id="4129d-260">Приведенные выше вспомогательные функции тегов отрисовывают следующий код HTML.</span><span class="sxs-lookup"><span data-stu-id="4129d-260">The preceding tag helpers render the following HTML.</span></span>

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

<span data-ttu-id="4129d-261">Обратите внимание на то, что атрибуты `data-` в выходных данных HTML соответствуют атрибутам проверки для свойства `ReleaseDate`.</span><span class="sxs-lookup"><span data-stu-id="4129d-261">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `ReleaseDate` property.</span></span> <span data-ttu-id="4129d-262">Атрибут `data-val-required` содержит сообщение об ошибке, которое выводится, если пользователь не заполнил поле даты выхода.</span><span class="sxs-lookup"><span data-stu-id="4129d-262">The `data-val-required` attribute contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="4129d-263">Скрипт ненавязчивой проверки jQuery передает это значение в метод [`required()`](https://jqueryvalidation.org/required-method/) подключаемого модуля jQuery Validate, который затем выводит это сообщение в соответствующем элементе **\<span>** .</span><span class="sxs-lookup"><span data-stu-id="4129d-263">jQuery Unobtrusive Validation passes this value to the jQuery Validate [`required()`](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

<span data-ttu-id="4129d-264">Проверка типа данных основана на типе свойства в .NET, если его не переопределяет атрибут `[DataType]`.</span><span class="sxs-lookup"><span data-stu-id="4129d-264">Data type validation is based on the .NET type of a property, unless that is overridden by a `[DataType]` attribute.</span></span> <span data-ttu-id="4129d-265">Браузеры имеют свои сообщения об по умолчанию, но пакет ненавязчивой проверки jQuery может переопределять эти сообщения.</span><span class="sxs-lookup"><span data-stu-id="4129d-265">Browsers have their own default error messages, but the jQuery Validation Unobtrusive Validation package can override those messages.</span></span> <span data-ttu-id="4129d-266">Атрибуты и подклассы `[DataType]`, такие как `[EmailAddress]`, позволяют указать сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="4129d-266">`[DataType]` attributes and subclasses such as `[EmailAddress]` let you specify the error message.</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="4129d-267">Добавление проверки к динамическим формам</span><span class="sxs-lookup"><span data-stu-id="4129d-267">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="4129d-268">Скрипт ненавязчивой проверки jQuery передает логику и параметры проверки в подключаемый модуль jQuery Validate при первой загрузке страницы.</span><span class="sxs-lookup"><span data-stu-id="4129d-268">jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads.</span></span> <span data-ttu-id="4129d-269">Поэтому динамически создаваемые формы не подвергаются проверке автоматически.</span><span class="sxs-lookup"><span data-stu-id="4129d-269">Therefore, validation doesn't work automatically on dynamically generated forms.</span></span> <span data-ttu-id="4129d-270">Чтобы включить проверку, необходимо указать, что скрипт ненавязчивой проверки jQuery должен анализировать динамическую форму сразу после ее создания.</span><span class="sxs-lookup"><span data-stu-id="4129d-270">To enable validation, tell jQuery Unobtrusive Validation to parse the dynamic form immediately after you create it.</span></span> <span data-ttu-id="4129d-271">Например, приведенный ниже код показывает, как можно настроить проверку на стороне клиента для формы, добавленной посредством AJAX.</span><span class="sxs-lookup"><span data-stu-id="4129d-271">For example, the following code sets up client-side validation on a form added via AJAX.</span></span>

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

<span data-ttu-id="4129d-272">Метод `$.validator.unobtrusive.parse()` принимает селектор jQuery в качестве единственного аргумента.</span><span class="sxs-lookup"><span data-stu-id="4129d-272">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="4129d-273">Этот метод предписывает скрипту ненавязчивой проверки jQuery анализировать атрибуты `data-` форм в этом селекторе.</span><span class="sxs-lookup"><span data-stu-id="4129d-273">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="4129d-274">Значения этих атрибутов затем передаются в подключаемый модуль jQuery Validate.</span><span class="sxs-lookup"><span data-stu-id="4129d-274">The values of those attributes are then passed to the jQuery Validate plugin.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="4129d-275">Добавление проверки к динамическим элементам управления</span><span class="sxs-lookup"><span data-stu-id="4129d-275">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="4129d-276">Метод `$.validator.unobtrusive.parse()` обрабатывает всю форму, а не отдельные динамически создаваемые элементы управления, такие как `<input>` и `<select/>`.</span><span class="sxs-lookup"><span data-stu-id="4129d-276">The `$.validator.unobtrusive.parse()` method works on an entire form, not on individual dynamically generated controls, such as `<input>` and `<select/>`.</span></span> <span data-ttu-id="4129d-277">Для повторной обработки формы удалите данные проверки, которые были добавлены при анализе формы ранее, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="4129d-277">To reparse the form, remove the validation data that was added when the form was parsed earlier, as shown in the following example:</span></span>

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

## <a name="custom-client-side-validation"></a><span data-ttu-id="4129d-278">Настраиваемая проверка на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="4129d-278">Custom client-side validation</span></span>

<span data-ttu-id="4129d-279">Настраиваемая проверка на стороне клиента выполняется путем создания атрибутов HTML `data-`, которые работают с настраиваемым адаптером проверки jQuery.</span><span class="sxs-lookup"><span data-stu-id="4129d-279">Custom client-side validation is done by generating `data-` HTML attributes that work with a custom jQuery Validate adapter.</span></span> <span data-ttu-id="4129d-280">В следующем примере кода адаптера используются атрибуты `ClassicMovie` и `ClassicMovie2`, которые были введены ранее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="4129d-280">The following sample adapter code was written for the `ClassicMovie` and `ClassicMovie2` attributes that were introduced earlier in this article:</span></span>

[!code-javascript[](validation/sample/wwwroot/js/classicMovieValidator.js?name=snippet_UnobtrusiveValidation)]

<span data-ttu-id="4129d-281">Сведения о способах создания адаптеров см. в [документации по проверкам jQuery](https://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="4129d-281">For information about how to write adapters, see the [jQuery Validate documentation](https://jqueryvalidation.org/documentation/).</span></span>

<span data-ttu-id="4129d-282">Использование адаптера для заданного поля инициируется атрибутами `data-`, которые:</span><span class="sxs-lookup"><span data-stu-id="4129d-282">The use of an adapter for a given field is triggered by `data-` attributes that:</span></span>

* <span data-ttu-id="4129d-283">помечают поле как проходящее проверку (`data-val="true"`);</span><span class="sxs-lookup"><span data-stu-id="4129d-283">Flag the field as being subject to validation (`data-val="true"`).</span></span>
* <span data-ttu-id="4129d-284">указывают имя правила проверки и текст сообщения об ошибке (например, `data-val-rulename="Error message."`);</span><span class="sxs-lookup"><span data-stu-id="4129d-284">Identify a validation rule name and error message text (for example, `data-val-rulename="Error message."`).</span></span>
* <span data-ttu-id="4129d-285">указывают любые дополнительные параметры, в которых нуждается проверяющий элемент управления (например, `data-val-rulename-parm1="value"`).</span><span class="sxs-lookup"><span data-stu-id="4129d-285">Provide any additional parameters the validator needs (for example, `data-val-rulename-parm1="value"`).</span></span>

<span data-ttu-id="4129d-286">В следующем примере показаны атрибуты `data-` для нашего атрибута `ClassicMovie` из примера приложения.</span><span class="sxs-lookup"><span data-stu-id="4129d-286">The following example shows the `data-` attributes for the sample app's `ClassicMovie` attribute:</span></span>

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie1="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie1-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="">
```

<span data-ttu-id="4129d-287">Как отмечалось ранее, [вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) и [вспомогательные методы HTML](xref:mvc/views/overview) используют сведения из атрибутов проверки для подготовки к отрисовке атрибутов `data-`.</span><span class="sxs-lookup"><span data-stu-id="4129d-287">As noted earlier, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use information from validation attributes to render `data-` attributes.</span></span> <span data-ttu-id="4129d-288">Существует два варианта для написания кода, который приводит к созданию настраиваемых атрибутов HTML `data-`.</span><span class="sxs-lookup"><span data-stu-id="4129d-288">There are two options for writing code that results in the creation of custom `data-` HTML attributes:</span></span>

* <span data-ttu-id="4129d-289">Создайте класс, производный от `AttributeAdapterBase<TAttribute>`, и класс, реализующий `IValidationAttributeAdapterProvider`; зарегистрируйте атрибут и его адаптер в функции внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="4129d-289">Create a class that derives from `AttributeAdapterBase<TAttribute>` and a class that implements `IValidationAttributeAdapterProvider`, and register your attribute and its adapter in DI.</span></span> <span data-ttu-id="4129d-290">Этот метод следует за [участником с единственной обязанностью](https://wikipedia.org/wiki/Single_responsibility_principle) в том, что код проверки, связанный с сервером и клиентом, выделен в отдельные классы.</span><span class="sxs-lookup"><span data-stu-id="4129d-290">This method follows the [single responsibility principal](https://wikipedia.org/wiki/Single_responsibility_principle) in that server-related and client-related validation code is in separate classes.</span></span> <span data-ttu-id="4129d-291">Адаптер также имеет следующее преимущество: так как он зарегистрирован в функции внедрения зависимостей, для него при необходимости доступны другие службы в ней.</span><span class="sxs-lookup"><span data-stu-id="4129d-291">The adapter also has the advantage that since it is registered in DI, other services in DI are available to it if needed.</span></span>
* <span data-ttu-id="4129d-292">Реализуйте `IClientModelValidator` в вашем классе `ValidationAttribute`.</span><span class="sxs-lookup"><span data-stu-id="4129d-292">Implement `IClientModelValidator` in your `ValidationAttribute` class.</span></span> <span data-ttu-id="4129d-293">Этот метод стоит использовать, если атрибут не выполняет проверку на стороне сервера и не нуждается в службах из функции внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="4129d-293">This method might be appropriate if the attribute doesn't do any server-side validation and doesn't need any services from DI.</span></span>

### <a name="attributeadapter-for-client-side-validation"></a><span data-ttu-id="4129d-294">AttributeAdapter для проверки на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="4129d-294">AttributeAdapter for client-side validation</span></span>

<span data-ttu-id="4129d-295">Этот метод отрисовки атрибутов `data-` в формате HTML используется атрибутом `ClassicMovie` в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="4129d-295">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie` attribute in the sample app.</span></span> <span data-ttu-id="4129d-296">Чтобы добавить клиентскую проверку с помощью этого метода, сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="4129d-296">To add client validation by using this method:</span></span>

1. <span data-ttu-id="4129d-297">Создайте класс адаптера атрибута для настраиваемого атрибута проверки.</span><span class="sxs-lookup"><span data-stu-id="4129d-297">Create an attribute adapter class for the custom validation attribute.</span></span> <span data-ttu-id="4129d-298">Наследуйте класс от [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="4129d-298">Derive the class from [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span></span> <span data-ttu-id="4129d-299">Создайте метод `AddValidation`, который добавляет атрибуты `data-` для выводимых данных, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="4129d-299">Create an `AddValidation` method that adds `data-` attributes to the rendered output, as shown in this example:</span></span>

   [!code-csharp[](validation/sample/Attributes/ClassicMovieAttributeAdapter.cs?name=snippet_ClassicMovieAttributeAdapter)]

1. <span data-ttu-id="4129d-300">Создайте класс поставщика адаптера, который реализует <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span><span class="sxs-lookup"><span data-stu-id="4129d-300">Create an adapter provider class that implements <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span></span> <span data-ttu-id="4129d-301">В методе `GetAttributeAdapter` передайте настраиваемый атрибут в конструктор адаптера, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="4129d-301">In the `GetAttributeAdapter` method pass in the custom attribute to the adapter's constructor, as shown in this example:</span></span>

   [!code-csharp[](validation/sample/Attributes/CustomValidationAttributeAdapterProvider.cs?name=snippet_CustomValidationAttributeAdapterProvider)]

1. <span data-ttu-id="4129d-302">Зарегистрируйте поставщик адаптера для внедрения зависимостей в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="4129d-302">Register the adapter provider for DI in `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=8-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a><span data-ttu-id="4129d-303">IClientModelValidator для проверки на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="4129d-303">IClientModelValidator for client-side validation</span></span>

<span data-ttu-id="4129d-304">Этот метод отрисовки атрибутов `data-` в формате HTML используется атрибутом `ClassicMovie2` в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="4129d-304">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie2` attribute in the sample app.</span></span> <span data-ttu-id="4129d-305">Чтобы добавить клиентскую проверку с помощью этого метода, сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="4129d-305">To add client validation by using this method:</span></span>

* <span data-ttu-id="4129d-306">В настраиваемом атрибуте проверки реализуйте интерфейс `IClientModelValidator` и создайте метод `AddValidation`.</span><span class="sxs-lookup"><span data-stu-id="4129d-306">In the custom validation attribute, implement the `IClientModelValidator` interface and create an `AddValidation` method.</span></span> <span data-ttu-id="4129d-307">В метод `AddValidation` добавьте атрибуты `data-` для проверки, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="4129d-307">In the `AddValidation` method, add `data-` attributes for validation, as shown in the following example:</span></span>

  [!code-csharp[](validation/sample/Attributes/ClassicMovie2Attribute.cs?name=snippet_ClassicMovie2Attribute)]

## <a name="disable-client-side-validation"></a><span data-ttu-id="4129d-308">Отключение проверки на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="4129d-308">Disable client-side validation</span></span>

<span data-ttu-id="4129d-309">Следующий код отключает клиентскую проверку в представлениях MVC.</span><span class="sxs-lookup"><span data-stu-id="4129d-309">The following code disables client validation in MVC views:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup2.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="4129d-310">В Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="4129d-310">And in Razor Pages:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup3.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="4129d-311">Еще один вариант отключения клиентской проверки клиента — закомментировать ссылку на `_ValidationScriptsPartial` в вашем файле *.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="4129d-311">Another option for disabling client validation is to comment out the reference to `_ValidationScriptsPartial` in your *.cshtml* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4129d-312">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="4129d-312">Additional resources</span></span>

* [<span data-ttu-id="4129d-313">Пространство имен System.ComponentModel.DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="4129d-313">System.ComponentModel.DataAnnotations namespace</span></span>](xref:System.ComponentModel.DataAnnotations)
* [<span data-ttu-id="4129d-314">Привязка модели</span><span class="sxs-lookup"><span data-stu-id="4129d-314">Model Binding</span></span>](model-binding.md)
