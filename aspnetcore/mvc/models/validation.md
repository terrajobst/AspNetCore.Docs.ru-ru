---
title: Проверка модели в ASP.NET Core MVC
author: tdykstra
description: Сведения о проверке модели в ASP.NET Core MVC и Razor Pages.
ms.author: riande
ms.custom: mvc
ms.date: 04/01/2019
uid: mvc/models/validation
ms.openlocfilehash: b766d47f296745ba4be6ea8cb6335db9c3e2d975
ms.sourcegitcommit: 5995f44e9e13d7e7aa8d193e2825381c42184e47
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/02/2019
ms.locfileid: "58809319"
---
# <a name="model-validation-in-aspnet-core-mvc-and-razor-pages"></a><span data-ttu-id="993a6-103">Проверка модели в ASP.NET Core MVC и Razor Pages</span><span class="sxs-lookup"><span data-stu-id="993a6-103">Model validation in ASP.NET Core MVC and Razor Pages</span></span>

<span data-ttu-id="993a6-104">В этой статье объясняется, как осуществлять проверки вводимых пользователем данных в приложении ASP.NET Core MVC или Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="993a6-104">This article explains how to validate user input in an ASP.NET Core MVC or Razor Pages app.</span></span>

<span data-ttu-id="993a6-105">[Просмотреть или скачать пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="993a6-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="model-state"></a><span data-ttu-id="993a6-106">Состояние модели</span><span class="sxs-lookup"><span data-stu-id="993a6-106">Model state</span></span>

<span data-ttu-id="993a6-107">Состояние модели представляет ошибки, создаваемые двумя подсистемами: привязкой модели и проверкой модели.</span><span class="sxs-lookup"><span data-stu-id="993a6-107">Model state represents errors that come from two subsystems: model binding and model validation.</span></span> <span data-ttu-id="993a6-108">Ошибки, поступающие из [привязки модели](model-binding.md), чаще всего представляют собой ошибки преобразования данных (например, x вводится в поле, которое ожидает целое число).</span><span class="sxs-lookup"><span data-stu-id="993a6-108">Errors that originate from [model binding](model-binding.md) are generally data conversion errors (for example, an "x" is entered in a field that expects an integer).</span></span> <span data-ttu-id="993a6-109">Проверка модели проводится после привязки модели и сообщает об ошибках, при которых данные не соответствуют бизнес-правилам (например, 0 вводится в поле, которое ожидает оценку от 1 до 5).</span><span class="sxs-lookup"><span data-stu-id="993a6-109">Model validation occurs after model binding and reports errors where the data doesn't conform to business rules (for example, a 0 is entered in a field that expects a rating between 1 and 5).</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="993a6-110">Привязка модели и проверка модели происходят перед выполнением действия контроллера или метода обработчика Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="993a6-110">Both model binding and validation occur before the execution of a controller action or a Razor Pages handler method.</span></span> <span data-ttu-id="993a6-111">Приложение отвечает за проверку `ModelState.IsValid` и реагирует соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="993a6-111">It's the app's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="993a6-112">Веб-приложения обычно повторно отображают страницы с сообщением об ошибке.</span><span class="sxs-lookup"><span data-stu-id="993a6-112">Web apps typically redisplay the page with an error message:</span></span>

[!code-csharp[](validation/sample_snapshot/Create.cshtml.cs?name=snippet&highlight=3-6)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="993a6-113">Привязка модели и проверка модели происходят перед выполнением действия контроллера или метода обработчика Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="993a6-113">Both model binding and validation occur before the execution of a controller action or a Razor Pages handler method.</span></span> <span data-ttu-id="993a6-114">Веб-приложение отвечает за проверку `ModelState.IsValid` и реагирует соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="993a6-114">For web apps, it's the app's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="993a6-115">Веб-приложения обычно повторно отображают страницы с сообщением об ошибке.</span><span class="sxs-lookup"><span data-stu-id="993a6-115">Web apps typically redisplay the page with an error message:</span></span>

[!code-csharp[](validation/sample_snapshot/Create.cshtml.cs?name=snippet&highlight=3-6)]

<span data-ttu-id="993a6-116">Контроллерам веб-API не нужно проверять `ModelState.IsValid` при наличии атрибута `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="993a6-116">Web API controllers don't have to check `ModelState.IsValid` if they have the `[ApiController]` attribute.</span></span> <span data-ttu-id="993a6-117">В этом случае автоматически возвращается ответ HTTP 400, содержащий сведения о проблеме, если состояние модели недопустимо.</span><span class="sxs-lookup"><span data-stu-id="993a6-117">In that case, an automatic HTTP 400 response containing issue details is returned when model state is invalid.</span></span> <span data-ttu-id="993a6-118">Дополнительные сведения см. в разделе [Автоматические отклики HTTP 400](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="993a6-118">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

::: moniker-end

## <a name="rerun-validation"></a><span data-ttu-id="993a6-119">Перезапуск проверки</span><span class="sxs-lookup"><span data-stu-id="993a6-119">Rerun validation</span></span>

<span data-ttu-id="993a6-120">Проверка выполняется автоматически, однако может потребоваться повторить ее вручную.</span><span class="sxs-lookup"><span data-stu-id="993a6-120">Validation is automatic, but you might want to repeat it manually.</span></span> <span data-ttu-id="993a6-121">Например, можно вычислить значение свойства и повторно выполнить проверку после установки свойства в вычисляемое значение.</span><span class="sxs-lookup"><span data-stu-id="993a6-121">For example, you might compute a value for a property and want to rerun validation after setting the property to the computed value.</span></span> <span data-ttu-id="993a6-122">Для повторной проверки вызовите метод `TryValidateModel`, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="993a6-122">To rerun validation, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[](validation/sample/Controllers/MoviesController.cs?name=snippet_TryValidateModel&highlight=11)]

## <a name="validation-attributes"></a><span data-ttu-id="993a6-123">Атрибуты проверки</span><span class="sxs-lookup"><span data-stu-id="993a6-123">Validation attributes</span></span>

<span data-ttu-id="993a6-124">Атрибуты проверки позволяют задать правила проверки для свойств модели.</span><span class="sxs-lookup"><span data-stu-id="993a6-124">Validation attributes let you specify validation rules for model properties.</span></span> <span data-ttu-id="993a6-125">В следующем примере из [примера приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample) показан класс модели, который помечается с помощью атрибутов проверки.</span><span class="sxs-lookup"><span data-stu-id="993a6-125">The following example from [the sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample) shows a model class that is annotated with validation attributes.</span></span> <span data-ttu-id="993a6-126">`[ClassicMovie]` является настраиваемым атрибутом проверки; остальные являются встроенными.</span><span class="sxs-lookup"><span data-stu-id="993a6-126">The `[ClassicMovie]` attribute is a custom validation attribute and the others are built-in.</span></span> <span data-ttu-id="993a6-127">(Не показан `[ClassicMovie2]`, который демонстрирует альтернативный способ реализации настраиваемого атрибута.)</span><span class="sxs-lookup"><span data-stu-id="993a6-127">(Not shown is `[ClassicMovie2]`, which shows an alternative way to implement a custom attribute.)</span></span>

[!code-csharp[](validation/sample/Models/Movie.cs?name=snippet_ModelClass)]

## <a name="built-in-attributes"></a><span data-ttu-id="993a6-128">Встроенные атрибуты</span><span class="sxs-lookup"><span data-stu-id="993a6-128">Built-in attributes</span></span>

<span data-ttu-id="993a6-129">Ниже приведены некоторые из встроенных атрибутов проверки.</span><span class="sxs-lookup"><span data-stu-id="993a6-129">Here are some of the built-in validation attributes:</span></span>

* <span data-ttu-id="993a6-130">`[CreditCard]`: проверяет, имеет ли свойство формат кредитной карты.</span><span class="sxs-lookup"><span data-stu-id="993a6-130">`[CreditCard]`: Validates that the property has a credit card format.</span></span>
* <span data-ttu-id="993a6-131">`[Compare]`: проверяет, совпадают ли два свойства модели.</span><span class="sxs-lookup"><span data-stu-id="993a6-131">`[Compare]`: Validates that two properties in a model match.</span></span>
* <span data-ttu-id="993a6-132">`[EmailAddress]`: проверяет, имеет ли свойство формат адреса электронной почты.</span><span class="sxs-lookup"><span data-stu-id="993a6-132">`[EmailAddress]`: Validates that the property has an email format.</span></span>
* <span data-ttu-id="993a6-133">`[Phone]`: проверяет, имеет ли свойство формат номера телефона.</span><span class="sxs-lookup"><span data-stu-id="993a6-133">`[Phone]`: Validates that the property has a telephone number format.</span></span>
* <span data-ttu-id="993a6-134">`[Range]`: проверяет, находится ли значение свойства в указанном диапазоне.</span><span class="sxs-lookup"><span data-stu-id="993a6-134">`[Range]`: Validates that the property value falls within a specified range.</span></span>
* <span data-ttu-id="993a6-135">`[RegularExpression]`: проверяет, соответствует ли значение свойства указанному регулярному выражению.</span><span class="sxs-lookup"><span data-stu-id="993a6-135">`[RegularExpression]`: Validates that the property value matches a specified regular expression.</span></span>
* <span data-ttu-id="993a6-136">`[Required]`: проверяет, что поле не равно NULL.</span><span class="sxs-lookup"><span data-stu-id="993a6-136">`[Required]`: Validates that the field is not null.</span></span> <span data-ttu-id="993a6-137">См. в разделе [Атрибут [Required]](#required-attribute) дополнительные сведения о поведении этого атрибута.</span><span class="sxs-lookup"><span data-stu-id="993a6-137">See [[Required] attribute](#required-attribute) for details about this attribute's behavior.</span></span>
* <span data-ttu-id="993a6-138">`[StringLength]`: проверяет, что значение свойства строки не превышает ограничение по указанной длине.</span><span class="sxs-lookup"><span data-stu-id="993a6-138">`[StringLength]`: Validates that a string property value doesn't exceed a specified length limit.</span></span>
* <span data-ttu-id="993a6-139">`[Url]`: проверяет, имеет ли свойство формат URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="993a6-139">`[Url]`: Validates that the property has a URL format.</span></span>
* <span data-ttu-id="993a6-140">`[Remote]`: проверяет входные данные на клиенте путем вызова метода действия на сервере.</span><span class="sxs-lookup"><span data-stu-id="993a6-140">`[Remote]`: Validates input on the client by calling an action method on the server.</span></span> <span data-ttu-id="993a6-141">См. в разделе [Атрибут [Remote]](#remote-attribute) дополнительные сведения о поведении этого атрибута.</span><span class="sxs-lookup"><span data-stu-id="993a6-141">See [[Remote] attribute](#remote-attribute) for details about this attribute's behavior.</span></span>

<span data-ttu-id="993a6-142">Полный перечень атрибутов проверки можно найти в пространстве имен [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations).</span><span class="sxs-lookup"><span data-stu-id="993a6-142">A complete list of validation attributes can be found in the [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) namespace.</span></span>

### <a name="error-messages"></a><span data-ttu-id="993a6-143">Сообщения об ошибках</span><span class="sxs-lookup"><span data-stu-id="993a6-143">Error messages</span></span>

<span data-ttu-id="993a6-144">Атрибуты проверки позволяют указать сообщение об ошибке, которое будет отображаться, если входные данные недопустимы.</span><span class="sxs-lookup"><span data-stu-id="993a6-144">Validation attributes let you specify the error message to be displayed for invalid input.</span></span> <span data-ttu-id="993a6-145">Например:</span><span class="sxs-lookup"><span data-stu-id="993a6-145">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

<span data-ttu-id="993a6-146">На внутреннем уровне атрибуты вызывают `String.Format` с заполнителем для имени поля и иногда дополнительным заполнителями.</span><span class="sxs-lookup"><span data-stu-id="993a6-146">Internally, the attributes call `String.Format` with a placeholder for the field name and sometimes additional placeholders.</span></span> <span data-ttu-id="993a6-147">Например:</span><span class="sxs-lookup"><span data-stu-id="993a6-147">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

<span data-ttu-id="993a6-148">При применении к свойству `Name` сообщение об ошибке, созданное в приведенном выше коде, имело бы вид Name length must be between 6 and 8 (Длина имени должна быть от 6 до 8).</span><span class="sxs-lookup"><span data-stu-id="993a6-148">When applied to a `Name` property, the error message created by the preceding code would be "Name length must be between 6 and 8.".</span></span>

<span data-ttu-id="993a6-149">Чтобы узнать, какие параметры передаются в `String.Format` для сообщения об ошибке определенного атрибута, см. раздел [Исходный код DataAnnotations](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span><span class="sxs-lookup"><span data-stu-id="993a6-149">To find out which parameters are passed to `String.Format` for a particular attribute's error message, see the [DataAnnotations source code](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span></span>

## <a name="required-attribute"></a><span data-ttu-id="993a6-150">Атрибут [Required]</span><span class="sxs-lookup"><span data-stu-id="993a6-150">[Required] attribute</span></span>

<span data-ttu-id="993a6-151">По умолчанию система проверки рассматривает не допускающие значение NULL параметры или свойства так, как если бы они имели атрибут `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="993a6-151">By default, the validation system treats non-nullable parameters or properties as if they had a `[Required]` attribute.</span></span> <span data-ttu-id="993a6-152">[Типы значений](/dotnet/csharp/language-reference/keywords/value-types), например `decimal` и `int`, не поддерживают значение NULL.</span><span class="sxs-lookup"><span data-stu-id="993a6-152">[Value types](/dotnet/csharp/language-reference/keywords/value-types) such as `decimal` and `int` are non-nullable.</span></span>

### <a name="required-validation-on-the-server"></a><span data-ttu-id="993a6-153">Проверка [Required] на сервере</span><span class="sxs-lookup"><span data-stu-id="993a6-153">[Required] validation on the server</span></span>

<span data-ttu-id="993a6-154">На сервере обязательное значение считается отсутствующим, если свойство имеет значение NULL.</span><span class="sxs-lookup"><span data-stu-id="993a6-154">On the server, a required value is considered missing if the property is null.</span></span> <span data-ttu-id="993a6-155">Поле, не допускающее значения NULL, всегда является допустимым, и сообщение об ошибке атрибута [Required] никогда не выводится.</span><span class="sxs-lookup"><span data-stu-id="993a6-155">A non-nullable field is always valid, and the [Required] attribute's error message is never displayed.</span></span>

<span data-ttu-id="993a6-156">Тем не менее привязка модели для ненулевого свойства может завершиться ошибкой, приводящей к сообщению об ошибке, например `The value '' is invalid`.</span><span class="sxs-lookup"><span data-stu-id="993a6-156">However, model binding for a non-nullable property may fail, resulting in an error message such as `The value '' is invalid`.</span></span> <span data-ttu-id="993a6-157">Чтобы задать настраиваемое сообщение об ошибке во время проверки не допускающих значения NULL типов на стороне сервера, у вас есть следующие варианты.</span><span class="sxs-lookup"><span data-stu-id="993a6-157">To specify a custom error message for server-side validation of non-nullable types, you have the following options:</span></span>

* <span data-ttu-id="993a6-158">Сделать поле допускающим значение NULL (например, `decimal?` вместо `decimal`).</span><span class="sxs-lookup"><span data-stu-id="993a6-158">Make the field nullable (for example, `decimal?` instead of `decimal`).</span></span> <span data-ttu-id="993a6-159">Типы значений [Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) обрабатываются как стандартные типы, допускающие значение NULL.</span><span class="sxs-lookup"><span data-stu-id="993a6-159">[Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) value types are treated like standard nullable types.</span></span>
* <span data-ttu-id="993a6-160">Указать сообщение об ошибке по умолчанию для использования в привязке модели, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="993a6-160">Specify the default error message to be used by model binding, as shown in the following example:</span></span>

  [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=4-5)]

  <span data-ttu-id="993a6-161">Дополнительные сведения об ошибках привязки модели, у которых можно задать сообщение по умолчанию, см. в разделе <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span><span class="sxs-lookup"><span data-stu-id="993a6-161">For more information about model binding errors that you can set default messages for, see <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span></span>

### <a name="required-validation-on-the-client"></a><span data-ttu-id="993a6-162">Проверка [Required] на клиенте</span><span class="sxs-lookup"><span data-stu-id="993a6-162">[Required] validation on the client</span></span>

<span data-ttu-id="993a6-163">Типы и строки, не допускающие значение NULL, обрабатываются на клиенте не так, как на сервере.</span><span class="sxs-lookup"><span data-stu-id="993a6-163">Non-nullable types and strings are handled differently on the client compared to the server.</span></span> <span data-ttu-id="993a6-164">На клиенте:</span><span class="sxs-lookup"><span data-stu-id="993a6-164">On the client:</span></span>

* <span data-ttu-id="993a6-165">Значение считается присутствующим только в том случае, если для него вводятся данные.</span><span class="sxs-lookup"><span data-stu-id="993a6-165">A value is considered present only if input is entered for it.</span></span> <span data-ttu-id="993a6-166">Таким образом, проверка на стороне клиента обрабатывает типы, не допускающие значение NULL, так же, как обнуляемые типы.</span><span class="sxs-lookup"><span data-stu-id="993a6-166">Therefore, client-side validation handles non-nullable types the same as nullable types.</span></span>
* <span data-ttu-id="993a6-167">Пробелы в поле строки считаются допустимыми входными данными при проверке методом jQuery [required](https://jqueryvalidation.org/required-method/).</span><span class="sxs-lookup"><span data-stu-id="993a6-167">Whitespace in a string field is considered valid input by the jQuery Validation [required](https://jqueryvalidation.org/required-method/) method.</span></span> <span data-ttu-id="993a6-168">Проверка на стороне сервера считает, что обязательное строковое поле недопустимо, если введены только пробелы.</span><span class="sxs-lookup"><span data-stu-id="993a6-168">Server-side validation considers a required string field invalid if only whitespace is entered.</span></span>

<span data-ttu-id="993a6-169">Как отмечалось ранее, не допускающие значение NULL типы рассматриваются как имеющие атрибут `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="993a6-169">As noted earlier, non-nullable types are treated as though they had a `[Required]` attribute.</span></span> <span data-ttu-id="993a6-170">Это означает, что вы получаете проверку на стороне клиента, даже если не применять атрибут `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="993a6-170">That means you get client-side validation even if you don't apply the `[Required]` attribute.</span></span> <span data-ttu-id="993a6-171">Но если вы не используете атрибут, вы получаете сообщение об ошибке по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="993a6-171">But if you don't use the attribute, you get a default error message.</span></span> <span data-ttu-id="993a6-172">Чтобы задать настраиваемое сообщение об ошибке, используйте атрибут.</span><span class="sxs-lookup"><span data-stu-id="993a6-172">To specify a custom error message, use the attribute.</span></span>

## <a name="remote-attribute"></a><span data-ttu-id="993a6-173">Атрибут [Remote]</span><span class="sxs-lookup"><span data-stu-id="993a6-173">[Remote] attribute</span></span>

<span data-ttu-id="993a6-174">Атрибут `[Remote]` реализует проверку на стороне клиента, в ходе которой требуется вызвать метод на сервере, чтобы определить, является ли допустимым поле ввода.</span><span class="sxs-lookup"><span data-stu-id="993a6-174">The `[Remote]` attribute implements client-side validation that requires calling a method on the server to determine whether field input is valid.</span></span> <span data-ttu-id="993a6-175">Например, приложению может потребоваться проверить, занято ли имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="993a6-175">For example, the app may need to verify whether a user name is already in use.</span></span>

<span data-ttu-id="993a6-176">Для реализации удаленной проверки сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="993a6-176">To implement remote validation:</span></span>

1. <span data-ttu-id="993a6-177">Создайте для вызова из JavaScript метод действия.</span><span class="sxs-lookup"><span data-stu-id="993a6-177">Create an action method for JavaScript to call.</span></span>  <span data-ttu-id="993a6-178">Метод проверки jQuery [remote](https://jqueryvalidation.org/remote-method/) ожидает ответ JSON.</span><span class="sxs-lookup"><span data-stu-id="993a6-178">The jQuery Validate [remote](https://jqueryvalidation.org/remote-method/) method expects a JSON response:</span></span>

   * <span data-ttu-id="993a6-179">`"true"` означает, что входные данные допустимы.</span><span class="sxs-lookup"><span data-stu-id="993a6-179">`"true"` means the input data is valid.</span></span>
   * <span data-ttu-id="993a6-180">`"false"`, `undefined` или `null` означают, что входные данные недопустимы.</span><span class="sxs-lookup"><span data-stu-id="993a6-180">`"false"`, `undefined`, or `null` means the input is invalid.</span></span>  <span data-ttu-id="993a6-181">Вывод стандартного сообщения об ошибке</span><span class="sxs-lookup"><span data-stu-id="993a6-181">Display the default error message.</span></span>
   * <span data-ttu-id="993a6-182">Все прочие значения означают, что входные данные недопустимы.</span><span class="sxs-lookup"><span data-stu-id="993a6-182">Any other string means the input is invalid.</span></span> <span data-ttu-id="993a6-183">Вывод строки как настраиваемого сообщения об ошибке.</span><span class="sxs-lookup"><span data-stu-id="993a6-183">Display the string as a custom error message.</span></span>

   <span data-ttu-id="993a6-184">Ниже приведен пример метода действия, который возвращает настраиваемое сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="993a6-184">Here's an example of an action method that returns a custom error message:</span></span>

   [!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. <span data-ttu-id="993a6-185">В классе модели пометьте свойство атрибутом `[Remote]`, указывающим метод действия проверки, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="993a6-185">In the model class, annotate the property with a `[Remote]` attribute that points to the validation action method, as shown in the following example:</span></span>

   [!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserEmailProperty)]

### <a name="additional-fields"></a><span data-ttu-id="993a6-186">Дополнительные поля</span><span class="sxs-lookup"><span data-stu-id="993a6-186">Additional fields</span></span>

<span data-ttu-id="993a6-187">Свойство `AdditionalFields` атрибута `[Remote]` позволяет проверять сочетания полей с данными на сервере.</span><span class="sxs-lookup"><span data-stu-id="993a6-187">The `AdditionalFields` property of the `[Remote]` attribute lets you validate combinations of fields against data on the server.</span></span> <span data-ttu-id="993a6-188">Например, если бы в модели `User` были свойства `FirstName` и `LastName`, могла бы возникнуть необходимость проверить, нет ли уже пользователя с такой парой имен.</span><span class="sxs-lookup"><span data-stu-id="993a6-188">For example, if the `User` model had `FirstName` and `LastName` properties, you might want to verify that no existing users already have that pair of names.</span></span> <span data-ttu-id="993a6-189">В следующем примере показано использование `AdditionalFields`.</span><span class="sxs-lookup"><span data-stu-id="993a6-189">The following example shows how to use `AdditionalFields`:</span></span>

[!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserNameProperties)]

<span data-ttu-id="993a6-190">Свойству `AdditionalFields` можно было бы явным образом присвоить строки `"FirstName"` и `"LastName"`, однако применение оператора [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) упрощает дальнейший рефакторинг.</span><span class="sxs-lookup"><span data-stu-id="993a6-190">`AdditionalFields` could be set explicitly to the strings `"FirstName"` and `"LastName"`, but using the [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) operator simplifies later refactoring.</span></span> <span data-ttu-id="993a6-191">Методу действия для этой проверки необходимо принимать аргументы для имени и фамилии.</span><span class="sxs-lookup"><span data-stu-id="993a6-191">The action method for this validation must accept both first name and last name arguments:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyName)]

<span data-ttu-id="993a6-192">Когда пользователь вводит имя или фамилию, JavaScript выполняет удаленный вызов, чтобы увидеть, будет ли эта пара принята.</span><span class="sxs-lookup"><span data-stu-id="993a6-192">When the user enters a first or last name, JavaScript makes a remote call to see if that pair of names has been taken.</span></span>

<span data-ttu-id="993a6-193">Чтобы проверить несколько дополнительных полей, их следует указывать в виде списка с разделителями-запятыми.</span><span class="sxs-lookup"><span data-stu-id="993a6-193">To validate two or more additional fields, provide them as a comma-delimited list.</span></span> <span data-ttu-id="993a6-194">Например, чтобы добавить в модель свойство `MiddleName`, задайте атрибут `[Remote]`, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="993a6-194">For example, to add a `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following example:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="993a6-195">`AdditionalFields`, как и все аргументы атрибутов, должен представлять собой константное выражение.</span><span class="sxs-lookup"><span data-stu-id="993a6-195">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span> <span data-ttu-id="993a6-196">Поэтому не следует использовать [интерполированную строку](/dotnet/csharp/language-reference/keywords/interpolated-strings) или вызов <xref:System.String.Join*>для инициализации `AdditionalFields`.</span><span class="sxs-lookup"><span data-stu-id="993a6-196">Therefore, don't use an [interpolated string](/dotnet/csharp/language-reference/keywords/interpolated-strings) or call <xref:System.String.Join*> to initialize `AdditionalFields`.</span></span>

## <a name="alternatives-to-built-in-attributes"></a><span data-ttu-id="993a6-197">Альтернативы для встроенных атрибутов</span><span class="sxs-lookup"><span data-stu-id="993a6-197">Alternatives to built-in attributes</span></span>

<span data-ttu-id="993a6-198">Если вам нужна проверка, которую не предоставляют встроенные атрибуты, вы можете следующее.</span><span class="sxs-lookup"><span data-stu-id="993a6-198">If you need validation not provided by built-in attributes, you can:</span></span>

* <span data-ttu-id="993a6-199">[Создать настраиваемые атрибуты](#custom-attributes).</span><span class="sxs-lookup"><span data-stu-id="993a6-199">[Create custom attributes](#custom-attributes).</span></span>
* <span data-ttu-id="993a6-200">[Реализовать IValidatableObject](#ivalidatableobject).</span><span class="sxs-lookup"><span data-stu-id="993a6-200">[Implement IValidatableObject](#ivalidatableobject).</span></span>


## <a name="custom-attributes"></a><span data-ttu-id="993a6-201">Настраиваемые атрибуты</span><span class="sxs-lookup"><span data-stu-id="993a6-201">Custom attributes</span></span>

<span data-ttu-id="993a6-202">Для сценариев, где не годятся встроенные атрибуты проверки, можно создать настраиваемые атрибуты.</span><span class="sxs-lookup"><span data-stu-id="993a6-202">For scenarios that the built-in validation attributes don't handle, you can create custom validation attributes.</span></span> <span data-ttu-id="993a6-203">Создайте класс, наследуемый от <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, и переопределите метод <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*>.</span><span class="sxs-lookup"><span data-stu-id="993a6-203">Create a class that inherits from <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, and override the <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> method.</span></span>

<span data-ttu-id="993a6-204">Метод `IsValid` принимает объект с именем *value*, который является входными данными для проверки.</span><span class="sxs-lookup"><span data-stu-id="993a6-204">The `IsValid` method accepts an object named *value*, which is the input to be validated.</span></span> <span data-ttu-id="993a6-205">Перегрузка также принимает объект `ValidationContext`, который предоставляет дополнительные сведения, такие как экземпляр модели, созданный с помощью привязки модели.</span><span class="sxs-lookup"><span data-stu-id="993a6-205">An overload also accepts a `ValidationContext` object, which provides additional information, such as the model instance created by model binding.</span></span>

<span data-ttu-id="993a6-206">В следующем примере проверяется, что дата выпуска фильмов в *классическом* жанре задана не позднее указанного года.</span><span class="sxs-lookup"><span data-stu-id="993a6-206">The following example validates that the release date for a movie in the *Classic* genre isn't later than a specified year.</span></span> <span data-ttu-id="993a6-207">Атрибут `[ClassicMovie2]` сначала проверяет жанр и продолжает проверку, только если он *классический*.</span><span class="sxs-lookup"><span data-stu-id="993a6-207">The `[ClassicMovie2]` attribute checks the genre first and continues only if it's *Classic*.</span></span> <span data-ttu-id="993a6-208">У фильмов, попавших в классику, система проверяет даты выпуска, чтобы убедиться в том, что она не является более поздней, чем ограничение, переданное в конструктор атрибута.</span><span class="sxs-lookup"><span data-stu-id="993a6-208">For movies identified as classics, it checks the release date to make sure it's not later than the limit passed to the attribute constructor.)</span></span>

[!code-csharp[](validation/sample/Attributes/ClassicMovieAttribute.cs?name=snippet_ClassicMovieAttribute)]

<span data-ttu-id="993a6-209">Приведенная выше переменная `movie` представляет объект `Movie`, который содержит данные из переданной формы.</span><span class="sxs-lookup"><span data-stu-id="993a6-209">The `movie` variable in the preceding example represents a `Movie` object that contains the data from the form submission.</span></span> <span data-ttu-id="993a6-210">Метод `IsValid` проверяет дату и жанр.</span><span class="sxs-lookup"><span data-stu-id="993a6-210">The `IsValid` method checks the date and genre.</span></span> <span data-ttu-id="993a6-211">После успешной проверки `IsValid` возвращает код `ValidationResult.Success`.</span><span class="sxs-lookup"><span data-stu-id="993a6-211">Upon successful validation, `IsValid` returns a `ValidationResult.Success` code.</span></span> <span data-ttu-id="993a6-212">Если проверка завершается неудачно, возвращается `ValidationResult` с сообщением об ошибке.</span><span class="sxs-lookup"><span data-stu-id="993a6-212">When validation fails, a `ValidationResult` with an error message is returned.</span></span>

## <a name="ivalidatableobject"></a><span data-ttu-id="993a6-213">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="993a6-213">IValidatableObject</span></span>

<span data-ttu-id="993a6-214">Предыдущий пример работает только с типами `Movie`.</span><span class="sxs-lookup"><span data-stu-id="993a6-214">The preceding example works only with `Movie` types.</span></span> <span data-ttu-id="993a6-215">Другой вариант для проверки на уровне класса — реализация `IValidatableObject` в классе модели, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="993a6-215">Another option for class-level validation is to implement `IValidatableObject` in the model class, as shown in the following example:</span></span>

[!code-csharp[](validation/sample/Models/MovieIValidatable.cs?name=snippet&highlight=1,26-34)]

::: moniker range=">= aspnetcore-2.1"

## <a name="top-level-node-validation"></a><span data-ttu-id="993a6-216">Проверка узлов верхнего уровня</span><span class="sxs-lookup"><span data-stu-id="993a6-216">Top-level node validation</span></span>

<span data-ttu-id="993a6-217">Узлы верхнего уровня содержат:</span><span class="sxs-lookup"><span data-stu-id="993a6-217">Top-level nodes include:</span></span>

* <span data-ttu-id="993a6-218">параметры действия;</span><span class="sxs-lookup"><span data-stu-id="993a6-218">Action parameters</span></span>
* <span data-ttu-id="993a6-219">свойства контроллера;</span><span class="sxs-lookup"><span data-stu-id="993a6-219">Controller properties</span></span>
* <span data-ttu-id="993a6-220">параметры обработчика страниц;</span><span class="sxs-lookup"><span data-stu-id="993a6-220">Page handler parameters</span></span>
* <span data-ttu-id="993a6-221">свойства страничной модели.</span><span class="sxs-lookup"><span data-stu-id="993a6-221">Page model properties</span></span>

<span data-ttu-id="993a6-222">Проверка привязанных к модели узлов верхнего уровня осуществляется наряду с проверкой свойств модели.</span><span class="sxs-lookup"><span data-stu-id="993a6-222">Model-bound top-level nodes are validated in addition to validating model properties.</span></span> <span data-ttu-id="993a6-223">В следующем примере, взятом из примера приложения, метод `VerifyPhone` использует класс <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> для проверки параметра действия `phone`.</span><span class="sxs-lookup"><span data-stu-id="993a6-223">In the following example from the sample app, the `VerifyPhone` method uses the <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> to validate the `phone` action parameter:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

<span data-ttu-id="993a6-224">Узлы верхнего уровня могут применять класс <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> с атрибутами проверки.</span><span class="sxs-lookup"><span data-stu-id="993a6-224">Top-level nodes can use <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> with validation attributes.</span></span> <span data-ttu-id="993a6-225">В следующем примере, взятом из примера приложения, метод `CheckAge` указывает, что при отправке формы параметр `age` должен быть привязан из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="993a6-225">In the following example from the sample app, the `CheckAge` method specifies that the `age` parameter must be bound from the query string when the form is submitted:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_CheckAge)]

<span data-ttu-id="993a6-226">На странице "Check Age" (Проверка возраста) (*CheckAge.cshtml*) находятся две формы.</span><span class="sxs-lookup"><span data-stu-id="993a6-226">In the Check Age page (*CheckAge.cshtml*), there are two forms.</span></span> <span data-ttu-id="993a6-227">Первая форма отправляет значение `Age`, равное `99`, в виде строки запроса: `https://localhost:5001/Users/CheckAge?Age=99`.</span><span class="sxs-lookup"><span data-stu-id="993a6-227">The first form submits an `Age` value of `99` as a query string: `https://localhost:5001/Users/CheckAge?Age=99`.</span></span>

<span data-ttu-id="993a6-228">Если из строки запроса отправлен параметр `age` в правильном формате, форма проходит проверку.</span><span class="sxs-lookup"><span data-stu-id="993a6-228">When a properly formatted `age` parameter from the query string is submitted, the form validates.</span></span>

<span data-ttu-id="993a6-229">Вторая форма на странице "Check Age" (Проверка возраста) отправляет значение `Age` в теле запроса, и проверка завершается сбоем.</span><span class="sxs-lookup"><span data-stu-id="993a6-229">The second form on the Check Age page submits the `Age` value in the body of the request, and validation fails.</span></span> <span data-ttu-id="993a6-230">Ошибка привязки связана с тем, что параметр `age` должен поступать из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="993a6-230">Binding fails because the `age` parameter must come from a query string.</span></span>

<span data-ttu-id="993a6-231">При работе с `CompatibilityVersion.Version_2_1` или более поздней версией проверка узла верхнего уровня по умолчанию включена.</span><span class="sxs-lookup"><span data-stu-id="993a6-231">When running with `CompatibilityVersion.Version_2_1` or later, top-level node validation is enabled by default.</span></span> <span data-ttu-id="993a6-232">В противном случае проверка узла верхнего уровня отключена.</span><span class="sxs-lookup"><span data-stu-id="993a6-232">Otherwise, top-level node validation is disabled.</span></span> <span data-ttu-id="993a6-233">Параметр по умолчанию можно переопределить, задав свойство <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> в (`Startup.ConfigureServices`), как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="993a6-233">The default option can be overridden by setting the <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> property in (`Startup.ConfigureServices`), as shown here:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup.cs?name=snippet_AddMvc&highlight=4)]

::: moniker-end

## <a name="maximum-errors"></a><span data-ttu-id="993a6-234">Максимальное количество ошибок</span><span class="sxs-lookup"><span data-stu-id="993a6-234">Maximum errors</span></span>

<span data-ttu-id="993a6-235">При достижении максимального количества ошибок (по умолчанию 200) проверка прекращается.</span><span class="sxs-lookup"><span data-stu-id="993a6-235">Validation stops when the maximum number of errors is reached (200 by default).</span></span> <span data-ttu-id="993a6-236">Это число можно изменить с помощью следующего кода в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="993a6-236">You can configure this number with the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=3)]

::: moniker range=">= aspnetcore-2.1"

## <a name="maximum-recursion"></a><span data-ttu-id="993a6-237">Максимальная рекурсия</span><span class="sxs-lookup"><span data-stu-id="993a6-237">Maximum recursion</span></span>

<span data-ttu-id="993a6-238"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> проходит через граф объектов в проверяемой модели.</span><span class="sxs-lookup"><span data-stu-id="993a6-238"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> traverses the object graph of the model being validated.</span></span> <span data-ttu-id="993a6-239">У моделей, которые очень глубоки или содержат бесконечную рекурсию, в ходе проверки может произойти переполнение стека.</span><span class="sxs-lookup"><span data-stu-id="993a6-239">For models that are very deep or are infinitely recursive, validation may result in stack overflow.</span></span> <span data-ttu-id="993a6-240">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) предоставляет способ остановить проверку до превышения настроенной глубины рекурсии обхода.</span><span class="sxs-lookup"><span data-stu-id="993a6-240">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) provides a way to stop validation early if the visitor recursion exceeds a configured depth.</span></span> <span data-ttu-id="993a6-241">Значение `MvcOptions.MaxValidationDepth` по умолчанию — 200 при работе в `CompatibilityVersion.Version_2_2` или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="993a6-241">The default value of `MvcOptions.MaxValidationDepth` is 200 when running with `CompatibilityVersion.Version_2_2` or later.</span></span> <span data-ttu-id="993a6-242">Для более ранних версий значение равно NULL; это означает отсутствие ограничения глубины.</span><span class="sxs-lookup"><span data-stu-id="993a6-242">For earlier versions, the value is null, which means no depth constraint.</span></span>

## <a name="automatic-short-circuit"></a><span data-ttu-id="993a6-243">Автоматическое сокращение</span><span class="sxs-lookup"><span data-stu-id="993a6-243">Automatic short-circuit</span></span>

<span data-ttu-id="993a6-244">Проверка автоматически сокращается (пропускается), если граф модели не требует проверки.</span><span class="sxs-lookup"><span data-stu-id="993a6-244">Validation is automatically short-circuited (skipped) if the model graph doesn't require validation.</span></span> <span data-ttu-id="993a6-245">К числу объектов, которые среда выполнения пропускает при проверке, относятся коллекции примитивов (такие как `byte[]`, `string[]`, `Dictionary<string, string>`) и сложные графы объектов, которые не имеют проверяющих элементов управления.</span><span class="sxs-lookup"><span data-stu-id="993a6-245">Objects that the runtime skips validation for include collections of primitives (such as `byte[]`, `string[]`, `Dictionary<string, string>`) and complex object graphs that don't have any validators.</span></span>

::: moniker-end

## <a name="disable-validation"></a><span data-ttu-id="993a6-246">Отключение проверки</span><span class="sxs-lookup"><span data-stu-id="993a6-246">Disable validation</span></span>

<span data-ttu-id="993a6-247">Чтобы отключить проверку, сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="993a6-247">To disable validation:</span></span>

1. <span data-ttu-id="993a6-248">Создайте реализацию интерфейса `IObjectModelValidator`, которая не помечает поля как недопустимые.</span><span class="sxs-lookup"><span data-stu-id="993a6-248">Create an implementation of `IObjectModelValidator` that doesn't mark any fields as invalid.</span></span>

   [!code-csharp[](validation/sample/Attributes/NullObjectModelValidator.cs?name=snippet_DisableValidation)]

1. <span data-ttu-id="993a6-249">Добавьте следующий код в `Startup.ConfigureServices` для замены реализации `IObjectModelValidator` по умолчанию в контейнере внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="993a6-249">Add the following code to `Startup.ConfigureServices` to replace the default `IObjectModelValidator` implementation in the dependency injection container.</span></span>

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_DisableValidation)]

<span data-ttu-id="993a6-250">По-прежнему могут отображаться ошибки состояния модели, которые идут из привязки модели.</span><span class="sxs-lookup"><span data-stu-id="993a6-250">You might still see model state errors that originate from model binding.</span></span>

## <a name="client-side-validation"></a><span data-ttu-id="993a6-251">Проверка на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="993a6-251">Client-side validation</span></span>

<span data-ttu-id="993a6-252">Проверка на стороне клиента не позволяет отправлять форму, пока ее данные не будут допустимыми.</span><span class="sxs-lookup"><span data-stu-id="993a6-252">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="993a6-253">При нажатии кнопки "Отправить" выполняется код JavaScript, который либо отправляет форму, либо выводит сообщения об ошибках.</span><span class="sxs-lookup"><span data-stu-id="993a6-253">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="993a6-254">Проверка на стороне клиента позволяет избежать ненужного кругового захода на сервер при наличии ошибки ввода в форме.</span><span class="sxs-lookup"><span data-stu-id="993a6-254">Client-side validation avoids an unnecessary round trip to the server when there are input errors on a form.</span></span> <span data-ttu-id="993a6-255">Ссылки на следующий скрипт в *_Layout.cshtml* и *_ValidationScriptsPartial.cshtml* поддерживают проверку на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="993a6-255">The following script references in *_Layout.cshtml* and *_ValidationScriptsPartial.cshtml* support client-side validation:</span></span>

[!code-cshtml[](validation/sample/Views/Shared/_Layout.cshtml?name=snippet_ScriptTag)]

[!code-cshtml[](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_ScriptTags)]

<span data-ttu-id="993a6-256">Скрипт [ненавязчивой проверки jQuery](https://github.com/aspnet/jquery-validation-unobtrusive) — это настраиваемая интерфейсная библиотека Майкрософт, которая основана на популярном подключаемом модуле [jQuery Validate](https://jqueryvalidation.org/).</span><span class="sxs-lookup"><span data-stu-id="993a6-256">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="993a6-257">Без скрипта ненавязчивой проверки jQuery одну и ту же логику проверки приходилось бы реализовывать в двух местах: в атрибутах проверки для свойств модели на стороне сервера, а затем еще раз в скриптах на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="993a6-257">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server-side validation attributes on model properties, and then again in client-side scripts.</span></span> <span data-ttu-id="993a6-258">Вместо этого [вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) и [вспомогательные функции HTML](xref:mvc/views/overview) могут использовать атрибуты проверки и метаданные типов из свойств модели для обработки атрибутов `data-` HTML 5 в элементах форм, требующих проверки.</span><span class="sxs-lookup"><span data-stu-id="993a6-258">Instead, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use the validation attributes and type metadata from model properties to render HTML 5 `data-` attributes for the form elements that need validation.</span></span> <span data-ttu-id="993a6-259">Скрипт ненавязчивой проверки jQuery анализирует эти атрибуты `data-` и передает логику в подключаемый модуль jQuery Validate, по сути копируя логику проверки на стороне сервера в клиент.</span><span class="sxs-lookup"><span data-stu-id="993a6-259">jQuery Unobtrusive Validation parses the `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server-side validation logic to the client.</span></span> <span data-ttu-id="993a6-260">Ошибки проверки могут выводиться в клиенте с помощью вспомогательных функций тегов, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="993a6-260">You can display validation errors on the client using tag helpers as shown here:</span></span>

[!code-cshtml[](validation/sample/Views/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=4-5)]

<span data-ttu-id="993a6-261">Приведенные выше вспомогательные функции тегов отрисовывают следующий код HTML.</span><span class="sxs-lookup"><span data-stu-id="993a6-261">The preceding tag helpers render the following HTML.</span></span>

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
                id="ReleaseDate" name="ReleaseDate" value="" />
                <span class="text-danger field-validation-valid"
                data-valmsg-for="ReleaseDate" data-valmsg-replace="true"></span>
            </div>
        </div>
    </div>
</form>
```

<span data-ttu-id="993a6-262">Обратите внимание на то, что атрибуты `data-` в выходных данных HTML соответствуют атрибутам проверки для свойства `ReleaseDate`.</span><span class="sxs-lookup"><span data-stu-id="993a6-262">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `ReleaseDate` property.</span></span> <span data-ttu-id="993a6-263">Атрибут `data-val-required` содержит сообщение об ошибке, которое выводится, если пользователь не заполнил поле даты выхода.</span><span class="sxs-lookup"><span data-stu-id="993a6-263">The `data-val-required` attribute contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="993a6-264">Скрипт ненавязчивой проверки jQuery передает это значение в метод [`required()`](https://jqueryvalidation.org/required-method/) подключаемого модуля jQuery Validate, который затем выводит это сообщение в соответствующем элементе **\<span>**.</span><span class="sxs-lookup"><span data-stu-id="993a6-264">jQuery Unobtrusive Validation passes this value to the jQuery Validate [`required()`](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

<span data-ttu-id="993a6-265">Проверка типа данных основана на типе свойства в .NET, если его не переопределяет атрибут `[DataType]`.</span><span class="sxs-lookup"><span data-stu-id="993a6-265">Data type validation is based on the .NET type of a property, unless that is overridden by a `[DataType]` attribute.</span></span> <span data-ttu-id="993a6-266">Браузеры имеют свои сообщения об по умолчанию, но пакет ненавязчивой проверки jQuery может переопределять эти сообщения.</span><span class="sxs-lookup"><span data-stu-id="993a6-266">Browsers have their own default error messages, but the jQuery Validation Unobtrusive Validation package can override those messages.</span></span> <span data-ttu-id="993a6-267">Атрибуты и подклассы `[DataType]`, такие как `[EmailAddress]`, позволяют указать сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="993a6-267">`[DataType]` attributes and subclasses such as `[EmailAddress]` let you specify the error message.</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="993a6-268">Добавление проверки к динамическим формам</span><span class="sxs-lookup"><span data-stu-id="993a6-268">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="993a6-269">Скрипт ненавязчивой проверки jQuery передает логику и параметры проверки в подключаемый модуль jQuery Validate при первой загрузке страницы.</span><span class="sxs-lookup"><span data-stu-id="993a6-269">jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads.</span></span> <span data-ttu-id="993a6-270">Поэтому динамически создаваемые формы не подвергаются проверке автоматически.</span><span class="sxs-lookup"><span data-stu-id="993a6-270">Therefore, validation doesn't work automatically on dynamically generated forms.</span></span> <span data-ttu-id="993a6-271">Чтобы включить проверку, необходимо указать, что скрипт ненавязчивой проверки jQuery должен анализировать динамическую форму сразу после ее создания.</span><span class="sxs-lookup"><span data-stu-id="993a6-271">To enable validation, tell jQuery Unobtrusive Validation to parse the dynamic form immediately after you create it.</span></span> <span data-ttu-id="993a6-272">Например, приведенный ниже код показывает, как можно настроить проверку на стороне клиента для формы, добавленной посредством AJAX.</span><span class="sxs-lookup"><span data-stu-id="993a6-272">For example, the following code sets up client-side validation on a form added via AJAX.</span></span>

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

<span data-ttu-id="993a6-273">Метод `$.validator.unobtrusive.parse()` принимает селектор jQuery в качестве единственного аргумента.</span><span class="sxs-lookup"><span data-stu-id="993a6-273">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="993a6-274">Этот метод предписывает скрипту ненавязчивой проверки jQuery анализировать атрибуты `data-` форм в этом селекторе.</span><span class="sxs-lookup"><span data-stu-id="993a6-274">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="993a6-275">Значения этих атрибутов затем передаются в подключаемый модуль jQuery Validate.</span><span class="sxs-lookup"><span data-stu-id="993a6-275">The values of those attributes are then passed to the jQuery Validate plugin.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="993a6-276">Добавление проверки к динамическим элементам управления</span><span class="sxs-lookup"><span data-stu-id="993a6-276">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="993a6-277">Метод `$.validator.unobtrusive.parse()` обрабатывает всю форму, а не отдельные динамически создаваемые элементы управления, такие как `<input/>` и `<select/>`.</span><span class="sxs-lookup"><span data-stu-id="993a6-277">The `$.validator.unobtrusive.parse()` method works on an entire form, not on individual dynamically generated controls, such as `<input/>` and `<select/>`.</span></span> <span data-ttu-id="993a6-278">Для повторной обработки формы удалите данные проверки, которые были добавлены при анализе формы ранее, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="993a6-278">To reparse the form, remove the validation data that was added when the form was parsed earlier, as shown in the following example:</span></span>

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

## <a name="custom-client-side-validation"></a><span data-ttu-id="993a6-279">Настраиваемая проверка на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="993a6-279">Custom client-side validation</span></span>

<span data-ttu-id="993a6-280">Настраиваемая проверка на стороне клиента выполняется путем создания атрибутов HTML `data-`, которые работают с настраиваемым адаптером проверки jQuery.</span><span class="sxs-lookup"><span data-stu-id="993a6-280">Custom client-side validation is done by generating `data-` HTML attributes that work with a custom jQuery Validate adapter.</span></span> <span data-ttu-id="993a6-281">В следующем примере кода адаптера используются атрибуты `ClassicMovie` и `ClassicMovie2`, которые были введены ранее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="993a6-281">The following sample adapter code was written for the `ClassicMovie` and `ClassicMovie2` attributes that were introduced earlier in this article:</span></span>

[!code-javascript[](validation/sample/wwwroot/js/classicMovieValidator.js?name=snippet_UnobtrusiveValidation)]

<span data-ttu-id="993a6-282">Сведения о способах создания адаптеров см. в [документации по проверкам jQuery](http://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="993a6-282">For information about how to write adapters, see the [jQuery Validate documentation](http://jqueryvalidation.org/documentation/).</span></span>

<span data-ttu-id="993a6-283">Использование адаптера для заданного поля инициируется атрибутами `data-`, которые:</span><span class="sxs-lookup"><span data-stu-id="993a6-283">The use of an adapter for a given field is triggered by `data-` attributes that:</span></span>

* <span data-ttu-id="993a6-284">помечают поле как проходящее проверку (`data-val="true"`);</span><span class="sxs-lookup"><span data-stu-id="993a6-284">Flag the field as being subject to validation (`data-val="true"`).</span></span>
* <span data-ttu-id="993a6-285">указывают имя правила проверки и текст сообщения об ошибке (например, `data-val-rulename="Error message."`);</span><span class="sxs-lookup"><span data-stu-id="993a6-285">Identify a validation rule name and error message text (for example, `data-val-rulename="Error message."`).</span></span>
* <span data-ttu-id="993a6-286">указывают любые дополнительные параметры, в которых нуждается проверяющий элемент управления (например, `data-val-rulename-parm1="value"`).</span><span class="sxs-lookup"><span data-stu-id="993a6-286">Provide any additional parameters the validator needs (for example, `data-val-rulename-parm1="value"`).</span></span>

<span data-ttu-id="993a6-287">В следующем примере показаны атрибуты `data-` для нашего атрибута `ClassicMovie` из примера приложения.</span><span class="sxs-lookup"><span data-stu-id="993a6-287">The following example shows the `data-` attributes for the sample app's `ClassicMovie` attribute:</span></span>

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie1="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie1-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="" />
```

<span data-ttu-id="993a6-288">Как отмечалось ранее, [вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) и [вспомогательные методы HTML](xref:mvc/views/overview) используют сведения из атрибутов проверки для подготовки к отрисовке атрибутов `data-`.</span><span class="sxs-lookup"><span data-stu-id="993a6-288">As noted earlier, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use information from validation attributes to render `data-` attributes.</span></span> <span data-ttu-id="993a6-289">Существует два варианта для написания кода, который приводит к созданию настраиваемых атрибутов HTML `data-`.</span><span class="sxs-lookup"><span data-stu-id="993a6-289">There are two options for writing code that results in the creation of custom `data-` HTML attributes:</span></span>

* <span data-ttu-id="993a6-290">Создайте класс, производный от `AttributeAdapterBase<TAttribute>`, и класс, реализующий `IValidationAttributeAdapterProvider`; зарегистрируйте атрибут и его адаптер в функции внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="993a6-290">Create a class that derives from `AttributeAdapterBase<TAttribute>` and a class that implements `IValidationAttributeAdapterProvider`, and register your attribute and its adapter in DI.</span></span> <span data-ttu-id="993a6-291">Этот метод следует за [участником с единственной обязанностью](https://wikipedia.org/wiki/Single_responsibility_principle) в том, что код проверки, связанный с сервером и клиентом, выделен в отдельные классы.</span><span class="sxs-lookup"><span data-stu-id="993a6-291">This method follows the [single responsibility principal](https://wikipedia.org/wiki/Single_responsibility_principle) in that server-related and client-related validation code is in separate classes.</span></span> <span data-ttu-id="993a6-292">Адаптер также имеет следующее преимущество: так как он зарегистрирован в функции внедрения зависимостей, для него при необходимости доступны другие службы в ней.</span><span class="sxs-lookup"><span data-stu-id="993a6-292">The adapter also has the advantage that since it is registered in DI, other services in DI are available to it if needed.</span></span>
* <span data-ttu-id="993a6-293">Реализуйте `IClientModelValidator` в вашем классе `ValidationAttribute`.</span><span class="sxs-lookup"><span data-stu-id="993a6-293">Implement `IClientModelValidator` in your `ValidationAttribute` class.</span></span> <span data-ttu-id="993a6-294">Этот метод стоит использовать, если атрибут не выполняет проверку на стороне сервера и не нуждается в службах из функции внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="993a6-294">This method might be appropriate if the attribute doesn't do any server-side validation and doesn't need any services from DI.</span></span>

### <a name="attributeadapter-for-client-side-validation"></a><span data-ttu-id="993a6-295">AttributeAdapter для проверки на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="993a6-295">AttributeAdapter for client-side validation</span></span>

<span data-ttu-id="993a6-296">Этот метод отрисовки атрибутов `data-` в формате HTML используется атрибутом `ClassicMovie` в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="993a6-296">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie` attribute in the sample app.</span></span> <span data-ttu-id="993a6-297">Чтобы добавить клиентскую проверку с помощью этого метода, сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="993a6-297">To add client validation by using this method:</span></span>

1. <span data-ttu-id="993a6-298">Создайте класс адаптера атрибута для настраиваемого атрибута проверки.</span><span class="sxs-lookup"><span data-stu-id="993a6-298">Create an attribute adapter class for the custom validation attribute.</span></span> <span data-ttu-id="993a6-299">Наследуйте класс от [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="993a6-299">Derive the class from [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span></span> <span data-ttu-id="993a6-300">Создайте метод `AddValidation`, который добавляет атрибуты `data-` для выводимых данных, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="993a6-300">Create an `AddValidation` method that adds `data-` attributes to the rendered output, as shown in this example:</span></span>

   [!code-csharp[](validation/sample/Attributes/ClassicMovieAttributeAdapter.cs?name=snippet_ClassicMovieAttributeAdapter)]

1. <span data-ttu-id="993a6-301">Создайте класс поставщика адаптера, который реализует <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span><span class="sxs-lookup"><span data-stu-id="993a6-301">Create an adapter provider class that implements <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span></span> <span data-ttu-id="993a6-302">В методе `GetAttributeAdapter` передайте настраиваемый атрибут в конструктор адаптера, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="993a6-302">In the `GetAttributeAdapter` method pass in the custom attribute to the adapter's constructor, as shown in this example:</span></span>

   [!code-csharp[](validation/sample/Attributes/CustomValidationAttributeAdapterProvider.cs?name=snippet_CustomValidationAttributeAdapterProvider)]

1. <span data-ttu-id="993a6-303">Зарегистрируйте поставщик адаптера для внедрения зависимостей в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="993a6-303">Register the adapter provider for DI in `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=8-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a><span data-ttu-id="993a6-304">IClientModelValidator для проверки на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="993a6-304">IClientModelValidator for client-side validation</span></span>

<span data-ttu-id="993a6-305">Этот метод отрисовки атрибутов `data-` в формате HTML используется атрибутом `ClassicMovie2` в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="993a6-305">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie2` attribute in the sample app.</span></span> <span data-ttu-id="993a6-306">Чтобы добавить клиентскую проверку с помощью этого метода, сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="993a6-306">To add client validation by using this method:</span></span>

* <span data-ttu-id="993a6-307">В настраиваемом атрибуте проверки реализуйте интерфейс `IClientModelValidator` и создайте метод `AddValidation`.</span><span class="sxs-lookup"><span data-stu-id="993a6-307">In the custom validation attribute, implement the `IClientModelValidator` interface and create an `AddValidation` method.</span></span> <span data-ttu-id="993a6-308">В метод `AddValidation` добавьте атрибуты `data-` для проверки, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="993a6-308">In the `AddValidation` method, add `data-` attributes for validation, as shown in the following example:</span></span>

  [!code-csharp[](validation/sample/Attributes/ClassicMovie2Attribute.cs?name=snippet_ClassicMovie2Attribute)]

## <a name="disable-client-side-validation"></a><span data-ttu-id="993a6-309">Отключение проверки на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="993a6-309">Disable client-side validation</span></span>

<span data-ttu-id="993a6-310">Следующий код отключает клиентскую проверку в представлениях MVC.</span><span class="sxs-lookup"><span data-stu-id="993a6-310">The following code disables client validation in MVC views:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup2.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="993a6-311">Это работает только в представлениях MVC, но не в Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="993a6-311">This works only in MVC views, not in Razor Pages.</span></span> <span data-ttu-id="993a6-312">Еще один вариант отключения клиентской проверки клиента — закомментировать ссылку на `_ValidationScriptsPartial` в вашем файле *.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="993a6-312">Another option for disabling client validation is to comment out the reference to `_ValidationScriptsPartial` in your *.cshtml* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="993a6-313">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="993a6-313">Additional resources</span></span>

* [<span data-ttu-id="993a6-314">Пространство имен System.ComponentModel.DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="993a6-314">System.ComponentModel.DataAnnotations namespace</span></span>](xref:System.ComponentModel.DataAnnotations)
* [<span data-ttu-id="993a6-315">Привязка модели</span><span class="sxs-lookup"><span data-stu-id="993a6-315">Model Binding</span></span>](model-binding.md)