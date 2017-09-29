---
title: "Вспомогательных функций тегов в форм в ASP.NET Core"
author: rick-anderson
description: "Описывает встроенные вспомогательных функций тегов используется с формами."
keywords: "Формирует ASP.NET Core, вспомогательные тег вспомогательной функции тегов, HTML-формы"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 25595059-4fac-4785-8152-f88590e3169b
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/working-with-forms
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ff6fee6eee539fc77b6c6180a816daa760202848
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/28/2017
---
# <a name="introduction-to-using-tag-helpers-in-forms-in-aspnet-core"></a><span data-ttu-id="9ffe1-104">Общие сведения об использовании вспомогательных функций тегов в форм в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9ffe1-104">Introduction to using tag helpers in forms in ASP.NET Core</span></span>

<span data-ttu-id="9ffe1-105">По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), и [Jerrie Pelser](https://github.com/jerriep)</span><span class="sxs-lookup"><span data-stu-id="9ffe1-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), and [Jerrie Pelser](https://github.com/jerriep)</span></span>

<span data-ttu-id="9ffe1-106">В этом документе демонстрируется работа с форм и элементов HTML, который обычно используется в форме.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-106">This document demonstrates working with Forms and the HTML elements commonly used on a Form.</span></span> <span data-ttu-id="9ffe1-107">HTML [формы](https://www.w3.org/TR/html401/interact/forms.html) элемент предоставляет использования приложений Интернета основным механизмом для отправки данных на сервер.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-107">The HTML [Form](https://www.w3.org/TR/html401/interact/forms.html) element provides the primary mechanism web apps use to post back data to the server.</span></span> <span data-ttu-id="9ffe1-108">Большая часть этого документа описываются [вспомогательных функций тегов](tag-helpers/intro.md) и как они могут помочь в продуктивно создавать надежные HTML-формы.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-108">Most of this document describes [Tag Helpers](tag-helpers/intro.md) and how they can help you productively create robust HTML forms.</span></span> <span data-ttu-id="9ffe1-109">Рекомендуется прочитать [Общие сведения о вспомогательных функций тегов](tag-helpers/intro.md) перед прочтением этого документа.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-109">We recommend you read [Introduction to Tag Helpers](tag-helpers/intro.md) before you read this document.</span></span>

<span data-ttu-id="9ffe1-110">Во многих случаях вспомогательные методы HTML предоставляют альтернативный подход для определенных вспомогательных тег, но это следует отметить, что вспомогательных функций тегов не заменяют вспомогательных методов HTML, а не существует тег вспомогательный класс для каждого вспомогательный метод HTML.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-110">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers do not replace HTML Helpers and there is not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="9ffe1-111">Если существует альтернатива вспомогательный метод HTML, он упоминается.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-111">When an HTML Helper alternative exists, it is mentioned.</span></span>

<a name=my-asp-route-param-ref-label></a>

## <a name="the-form-tag-helper"></a><span data-ttu-id="9ffe1-112">Вспомогательный тег формы</span><span class="sxs-lookup"><span data-stu-id="9ffe1-112">The Form Tag Helper</span></span>

<span data-ttu-id="9ffe1-113">[Формы](https://www.w3.org/TR/html401/interact/forms.html) тег вспомогательный:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-113">The [Form](https://www.w3.org/TR/html401/interact/forms.html) Tag Helper:</span></span>

* <span data-ttu-id="9ffe1-114">Создает HTML-код [ \<формы >](https://www.w3.org/TR/html401/interact/forms.html) `action` значение атрибута для действия контроллера MVC или именованный маршрут</span><span class="sxs-lookup"><span data-stu-id="9ffe1-114">Generates the HTML [\<FORM>](https://www.w3.org/TR/html401/interact/forms.html) `action` attribute value for a MVC controller action or named route</span></span>

* <span data-ttu-id="9ffe1-115">Создает скрытый [токена проверки запроса](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) для предотвращения подделки межсайтовых запросов (при использовании с `[ValidateAntiForgeryToken]` атрибута в методе действия HTTP Post)</span><span class="sxs-lookup"><span data-stu-id="9ffe1-115">Generates a hidden [Request Verification Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method)</span></span>

* <span data-ttu-id="9ffe1-116">Предоставляет `asp-route-<Parameter Name>` атрибут, где `<Parameter Name>` добавляется значения маршрута.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-116">Provides the `asp-route-<Parameter Name>` attribute, where `<Parameter Name>` is added to the route values.</span></span> <span data-ttu-id="9ffe1-117">`routeValues` Параметры `Html.BeginForm` и `Html.BeginRouteForm` предоставляют аналогичные функциональные возможности.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-117">The  `routeValues` parameters to `Html.BeginForm` and `Html.BeginRouteForm` provide similar functionality.</span></span>

* <span data-ttu-id="9ffe1-118">Вспомогательный метод HTML вариант `Html.BeginForm` и`Html.BeginRouteForm`</span><span class="sxs-lookup"><span data-stu-id="9ffe1-118">Has an HTML Helper alternative `Html.BeginForm` and `Html.BeginRouteForm`</span></span>

<span data-ttu-id="9ffe1-119">Пример:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-119">Sample:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

<span data-ttu-id="9ffe1-120">Тег формы вспомогательного выше создает следующий код HTML:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-120">The Form Tag Helper above generates the following HTML:</span></span>

```HTML
<form method="post" action="/Demo/Register">
     <!-- Input and Submit elements -->
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

<span data-ttu-id="9ffe1-121">Среда выполнения MVC создает `action` значение атрибута на основе атрибутов вспомогательный тег формы `asp-controller` и `asp-action`.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-121">The MVC runtime generates the `action` attribute value from the Form Tag Helper attributes `asp-controller` and `asp-action`.</span></span> <span data-ttu-id="9ffe1-122">Вспомогательный тег формы также создает скрытый [токена проверки запроса](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) для предотвращения подделки межсайтовых запросов (при использовании с `[ValidateAntiForgeryToken]` атрибута в методе действия HTTP Post).</span><span class="sxs-lookup"><span data-stu-id="9ffe1-122">The Form Tag Helper also generates a hidden [Request Verification Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method).</span></span> <span data-ttu-id="9ffe1-123">Трудно защиты от подделки межсайтовых запросов чистый HTML-формы, эта служба автоматически предоставляет вспомогательный тега формы.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-123">Protecting a pure HTML Form from cross-site request forgery is difficult, the Form Tag Helper provides this service for you.</span></span>

### <a name="using-a-named-route"></a><span data-ttu-id="9ffe1-124">С помощью именованный маршрут</span><span class="sxs-lookup"><span data-stu-id="9ffe1-124">Using a named route</span></span>

<span data-ttu-id="9ffe1-125">`asp-route` Атрибут вспомогательной функции тегов также можно создавать разметку HTML `action` атрибут.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-125">The `asp-route` Tag Helper attribute can also generate markup for the HTML `action` attribute.</span></span> <span data-ttu-id="9ffe1-126">Приложение с [маршрута](../../fundamentals/routing.md) с именем `register` использовать следующую разметку страницы регистрации:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-126">An app with a [route](../../fundamentals/routing.md)  named `register` could use the following markup for the registration page:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

<span data-ttu-id="9ffe1-127">Множество представлений в *представления и учетная запись* папку (созданы при создании нового веб-приложения с *отдельных учетных записей пользователей*) содержать [asp маршрута returnurl](https://docs.microsoft.com/aspnet/core/mvc/views/working-with-forms) атрибут:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-127">Many of the views in the *Views/Account* folder (generated when you create a new web app with *Individual User Accounts*) contain the [asp-route-returnurl](https://docs.microsoft.com/aspnet/core/mvc/views/working-with-forms) attribute:</span></span>

```cshtml
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
```

>[!NOTE]
><span data-ttu-id="9ffe1-128">С помощью встроенных шаблонов `returnUrl` только заполняется автоматически при пытаются получить доступ к авторизованным ресурсов, но не с проверкой подлинности или авторизации.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-128">With the built in templates, `returnUrl` is only populated automatically when you try to access an authorized resource but are not authenticated or authorized.</span></span> <span data-ttu-id="9ffe1-129">При попытке несанкционированного доступа, по промежуточного слоя безопасности вы будете перенаправлены на страницу входа с `returnUrl` значение.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-129">When you attempt an unauthorized access, the security middleware redirects you to the login page with the `returnUrl` set.</span></span>

## <a name="the-input-tag-helper"></a><span data-ttu-id="9ffe1-130">Вспомогательный объект тега входных данных</span><span class="sxs-lookup"><span data-stu-id="9ffe1-130">The Input Tag Helper</span></span>

<span data-ttu-id="9ffe1-131">Вспомогательный объект входного тег привязывает элемент HTML [ \<ввода >](https://www.w3.org/wiki/HTML/Elements/input) элемент модели выражения в представлении razor.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-131">The Input Tag Helper binds an HTML [\<input>](https://www.w3.org/wiki/HTML/Elements/input) element to a model expression in your razor view.</span></span>

<span data-ttu-id="9ffe1-132">Синтаксис:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-132">Syntax:</span></span>

```HTML
<input asp-for="<Expression Name>" />
```

<span data-ttu-id="9ffe1-133">Вспомогательный объект тега входных данных:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-133">The Input Tag Helper:</span></span>

* <span data-ttu-id="9ffe1-134">Приводит к возникновению ошибки `id` и `name` атрибуты HTML для имени выражения, указанного в `asp-for` атрибута.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-134">Generates the `id` and `name` HTML attributes for the expression name specified in the `asp-for` attribute.</span></span> <span data-ttu-id="9ffe1-135">`asp-for="Property1.Property2"` равно `m => m.Property1.Property2`.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-135">`asp-for="Property1.Property2"` is equivalent to `m => m.Property1.Property2`.</span></span> <span data-ttu-id="9ffe1-136">Имя выражения используется для `asp-for` значение атрибута.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-136">The name of the expression is what is used for the `asp-for` attribute value.</span></span> <span data-ttu-id="9ffe1-137">В разделе [имена выражений](#expression-names) Дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-137">See the [Expression names](#expression-names) section for additional information.</span></span>

* <span data-ttu-id="9ffe1-138">Задает HTML- `type` атрибута значение в зависимости от типа модели и [заметки к данным](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) атрибуты, примененные к свойству модели</span><span class="sxs-lookup"><span data-stu-id="9ffe1-138">Sets the HTML `type` attribute value based on the model type and  [data annotation](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to the model property</span></span>

* <span data-ttu-id="9ffe1-139">Не перезаписывает HTML `type` значение атрибута, если он указан</span><span class="sxs-lookup"><span data-stu-id="9ffe1-139">Will not overwrite the HTML `type` attribute value when one is specified</span></span>

* <span data-ttu-id="9ffe1-140">Приводит к возникновению ошибки [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) атрибуты проверки из [заметки к данным](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) атрибуты, примененные к свойства модели</span><span class="sxs-lookup"><span data-stu-id="9ffe1-140">Generates [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  validation attributes from [data annotation](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to model properties</span></span>

* <span data-ttu-id="9ffe1-141">Предусмотрена функция вспомогательный метод HTML перекрывать `Html.TextBoxFor` и `Html.EditorFor`.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-141">Has an HTML Helper feature overlap with `Html.TextBoxFor` and `Html.EditorFor`.</span></span> <span data-ttu-id="9ffe1-142">В разделе **вспомогательный метод HTML альтернативы вспомогательный тега входных данных** подробные сведения см.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-142">See the **HTML Helper alternatives to Input Tag Helper** section for details.</span></span>

* <span data-ttu-id="9ffe1-143">Предоставляет строгую типизацию.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-143">Provides strong typing.</span></span> <span data-ttu-id="9ffe1-144">Если имя свойства изменяется и не обновлять вспомогательный тег вы получите ошибку следующего вида:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-144">If the name of the property changes and you don't update the Tag Helper you'll get an error similar to the following:</span></span>

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

<span data-ttu-id="9ffe1-145">`Input` Вспомогательный тег задает HTML- `type` атрибут на основе .NET типа.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-145">The `Input` Tag Helper sets the HTML `type` attribute based on the .NET type.</span></span> <span data-ttu-id="9ffe1-146">В следующей таблице перечислены некоторые распространенные типы .NET и созданный тип HTML (не каждый тип .NET присутствует).</span><span class="sxs-lookup"><span data-stu-id="9ffe1-146">The following table lists some common .NET types and generated HTML type (not every .NET type is listed).</span></span>

|<span data-ttu-id="9ffe1-147">Тип .NET</span><span class="sxs-lookup"><span data-stu-id="9ffe1-147">.NET type</span></span>|<span data-ttu-id="9ffe1-148">Тип входных данных</span><span class="sxs-lookup"><span data-stu-id="9ffe1-148">Input Type</span></span>|
|---|---|
|<span data-ttu-id="9ffe1-149">Bool</span><span class="sxs-lookup"><span data-stu-id="9ffe1-149">Bool</span></span>|<span data-ttu-id="9ffe1-150">Тип = «checkbox»</span><span class="sxs-lookup"><span data-stu-id="9ffe1-150">type=”checkbox”</span></span>|
|<span data-ttu-id="9ffe1-151">Строка</span><span class="sxs-lookup"><span data-stu-id="9ffe1-151">String</span></span>|<span data-ttu-id="9ffe1-152">Тип = «text»</span><span class="sxs-lookup"><span data-stu-id="9ffe1-152">type=”text”</span></span>|
|<span data-ttu-id="9ffe1-153">DateTime</span><span class="sxs-lookup"><span data-stu-id="9ffe1-153">DateTime</span></span>|<span data-ttu-id="9ffe1-154">тип = «datetime»</span><span class="sxs-lookup"><span data-stu-id="9ffe1-154">type=”datetime”</span></span>|
|<span data-ttu-id="9ffe1-155">Byte</span><span class="sxs-lookup"><span data-stu-id="9ffe1-155">Byte</span></span>|<span data-ttu-id="9ffe1-156">Тип = «number»</span><span class="sxs-lookup"><span data-stu-id="9ffe1-156">type=”number”</span></span>|
|<span data-ttu-id="9ffe1-157">Int</span><span class="sxs-lookup"><span data-stu-id="9ffe1-157">Int</span></span>|<span data-ttu-id="9ffe1-158">Тип = «number»</span><span class="sxs-lookup"><span data-stu-id="9ffe1-158">type=”number”</span></span>|
|<span data-ttu-id="9ffe1-159">Single, Double</span><span class="sxs-lookup"><span data-stu-id="9ffe1-159">Single, Double</span></span>|<span data-ttu-id="9ffe1-160">Тип = «number»</span><span class="sxs-lookup"><span data-stu-id="9ffe1-160">type=”number”</span></span>|


<span data-ttu-id="9ffe1-161">В следующей таблице приведены некоторые наиболее распространенные [заметок к данным](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) атрибуты, сопоставленные вспомогательный тега входных данных для определенного типа входных данных (не каждый атрибут проверки присутствует):</span><span class="sxs-lookup"><span data-stu-id="9ffe1-161">The following table shows some common [data annotations](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes that the input tag helper will map to specific input types (not every validation attribute is listed):</span></span>


|<span data-ttu-id="9ffe1-162">Атрибут</span><span class="sxs-lookup"><span data-stu-id="9ffe1-162">Attribute</span></span>|<span data-ttu-id="9ffe1-163">Тип входных данных</span><span class="sxs-lookup"><span data-stu-id="9ffe1-163">Input Type</span></span>|
|---|---|
|<span data-ttu-id="9ffe1-164">[EmailAddress]</span><span class="sxs-lookup"><span data-stu-id="9ffe1-164">[EmailAddress]</span></span>|<span data-ttu-id="9ffe1-165">Тип = «email»</span><span class="sxs-lookup"><span data-stu-id="9ffe1-165">type=”email”</span></span>|
|<span data-ttu-id="9ffe1-166">[URL-адрес]</span><span class="sxs-lookup"><span data-stu-id="9ffe1-166">[Url]</span></span>|<span data-ttu-id="9ffe1-167">Тип = «url»</span><span class="sxs-lookup"><span data-stu-id="9ffe1-167">type=”url”</span></span>|
|<span data-ttu-id="9ffe1-168">[HiddenInput]</span><span class="sxs-lookup"><span data-stu-id="9ffe1-168">[HiddenInput]</span></span>|<span data-ttu-id="9ffe1-169">Тип = «hidden»</span><span class="sxs-lookup"><span data-stu-id="9ffe1-169">type=”hidden”</span></span>|
|<span data-ttu-id="9ffe1-170">[Phone]</span><span class="sxs-lookup"><span data-stu-id="9ffe1-170">[Phone]</span></span>|<span data-ttu-id="9ffe1-171">Тип = «телефон»</span><span class="sxs-lookup"><span data-stu-id="9ffe1-171">type=”tel”</span></span>|
|<span data-ttu-id="9ffe1-172">[DataType(DataType.Password)]</span><span class="sxs-lookup"><span data-stu-id="9ffe1-172">[DataType(DataType.Password)]</span></span>| <span data-ttu-id="9ffe1-173">Тип = «password»</span><span class="sxs-lookup"><span data-stu-id="9ffe1-173">type=”password”</span></span>|
|<span data-ttu-id="9ffe1-174">[DataType(DataType.Date)]</span><span class="sxs-lookup"><span data-stu-id="9ffe1-174">[DataType(DataType.Date)]</span></span>| <span data-ttu-id="9ffe1-175">тип = «date»</span><span class="sxs-lookup"><span data-stu-id="9ffe1-175">type=”date”</span></span>|
|<span data-ttu-id="9ffe1-176">[DataType(DataType.Time)]</span><span class="sxs-lookup"><span data-stu-id="9ffe1-176">[DataType(DataType.Time)]</span></span>| <span data-ttu-id="9ffe1-177">Тип = «время»</span><span class="sxs-lookup"><span data-stu-id="9ffe1-177">type=”time”</span></span>|


<span data-ttu-id="9ffe1-178">Пример:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-178">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

<span data-ttu-id="9ffe1-179">Приведенный выше код создает следующий код HTML:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-179">The code above generates the following HTML:</span></span>

```HTML
  <form method="post" action="/Demo/RegisterInput">
       Email:
       <input type="email" data-val="true"
              data-val-email="The Email Address field is not a valid e-mail address."
              data-val-required="The Email Address field is required."
              id="Email" name="Email" value="" /> <br>
       Password:
       <input type="password" data-val="true"
              data-val-required="The Password field is required."
              id="Password" name="Password" /><br>
       <button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

<span data-ttu-id="9ffe1-180">Заметки данных применяется к `Email` и `Password` свойства создавать метаданные для модели.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-180">The data annotations applied to the `Email` and `Password` properties generate metadata on the model.</span></span> <span data-ttu-id="9ffe1-181">Вспомогательный тега входных данных считывает метаданные модели и создает [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` атрибутов (см. [проверка модели](../models/validation.md)).</span><span class="sxs-lookup"><span data-stu-id="9ffe1-181">The Input Tag Helper consumes the model metadata and produces [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` attributes (see [Model Validation](../models/validation.md)).</span></span> <span data-ttu-id="9ffe1-182">Эти атрибуты описывают проверяющие элементы управления для присоединения к поля ввода.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-182">These attributes describe the validators to attach to the input fields.</span></span> <span data-ttu-id="9ffe1-183">Это обеспечивает ненавязчивого HTML5 и [jQuery](https://jquery.com/) проверки.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-183">This provides unobtrusive HTML5 and [jQuery](https://jquery.com/) validation.</span></span> <span data-ttu-id="9ffe1-184">Атрибуты ненавязчивой имеют формат `data-val-rule="Error Message"`, где правила — это имя правила проверки (например, `data-val-required`, `data-val-email`, `data-val-maxlength`и т. д.) Если сообщение об ошибке в атрибуте, то он отображается как значение для `data-val-rule` атрибута.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-184">The unobtrusive attributes have the format `data-val-rule="Error Message"`, where rule is the name of the validation rule (such as `data-val-required`, `data-val-email`, `data-val-maxlength`, etc.) If an error message is provided in the attribute, it is displayed as the value for the `data-val-rule` attribute.</span></span> <span data-ttu-id="9ffe1-185">Также существуют атрибуты формы `data-val-ruleName-argumentName="argumentValue"` , содержат дополнительные сведения о правиле, например, `data-val-maxlength-max="1024"` .</span><span class="sxs-lookup"><span data-stu-id="9ffe1-185">There are also attributes of the form `data-val-ruleName-argumentName="argumentValue"` that provide additional details about the rule, for example, `data-val-maxlength-max="1024"` .</span></span>

### <a name="html-helper-alternatives-to-input-tag-helper"></a><span data-ttu-id="9ffe1-186">Вспомогательный метод HTML альтернативы вспомогательный тега входных данных</span><span class="sxs-lookup"><span data-stu-id="9ffe1-186">HTML Helper alternatives to Input Tag Helper</span></span>

<span data-ttu-id="9ffe1-187">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` и `Html.EditorFor` пересекаться функции со вспомогательным методом тега входных данных.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-187">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` and `Html.EditorFor` have overlapping features with the Input Tag Helper.</span></span> <span data-ttu-id="9ffe1-188">Вспомогательный тега входных данных будет автоматически присваивать параметру `type` по умолчанию. `Html.TextBox` и `Html.TextBoxFor` не будет.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-188">The Input Tag Helper will automatically set the `type` attribute; `Html.TextBox` and `Html.TextBoxFor` will not.</span></span> <span data-ttu-id="9ffe1-189">`Html.Editor`и `Html.EditorFor` обработки коллекций, сложные объекты и шаблоны; не поддерживает вспомогательные тега входных данных.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-189">`Html.Editor` and `Html.EditorFor` handle collections, complex objects and templates; the Input Tag Helper does not.</span></span> <span data-ttu-id="9ffe1-190">Вспомогательный тега входных данных, `Html.EditorFor` и `Html.TextBoxFor` являются строго типизированными (они используют лямбда-выражений); `Html.TextBox` и `Html.Editor` не являются (они используют имена выражений).</span><span class="sxs-lookup"><span data-stu-id="9ffe1-190">The Input Tag Helper, `Html.EditorFor`  and  `Html.TextBoxFor` are strongly typed (they use lambda expressions); `Html.TextBox` and `Html.Editor` are not (they use expression names).</span></span>

### <a name="htmlattributes"></a><span data-ttu-id="9ffe1-191">HtmlAttributes</span><span class="sxs-lookup"><span data-stu-id="9ffe1-191">HtmlAttributes</span></span>

<span data-ttu-id="9ffe1-192">`@Html.Editor()`и `@Html.EditorFor()` использовать специальное `ViewDataDictionary` записи с именем `htmlAttributes` при выполнении их шаблонов по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-192">`@Html.Editor()` and `@Html.EditorFor()` use a special `ViewDataDictionary` entry named `htmlAttributes` when executing their default templates.</span></span> <span data-ttu-id="9ffe1-193">Это поведение также дополнен с помощью `additionalViewData` параметров.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-193">This behavior is optionally augmented using `additionalViewData` parameters.</span></span> <span data-ttu-id="9ffe1-194">Раздел «htmlAttributes» не учитывается регистр.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-194">The key "htmlAttributes" is case-insensitive.</span></span> <span data-ttu-id="9ffe1-195">Ключ «htmlAttributes» обрабатывается так же, как `htmlAttributes` объект, переданный в ввода вспомогательные методы, такие как `@Html.TextBox()`.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-195">The key "htmlAttributes" is handled similarly to the `htmlAttributes` object passed to input helpers like `@Html.TextBox()`.</span></span>

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a><span data-ttu-id="9ffe1-196">Имена выражений</span><span class="sxs-lookup"><span data-stu-id="9ffe1-196">Expression names</span></span>

<span data-ttu-id="9ffe1-197">`asp-for` Значение атрибута `ModelExpression` и правую часть лямбда-выражения.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-197">The `asp-for` attribute value is a `ModelExpression` and the right hand side of a lambda expression.</span></span> <span data-ttu-id="9ffe1-198">Таким образом `asp-for="Property1"` становится `m => m.Property1` в созданный код, поэтому нет необходимости префикс `Model`.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-198">Therefore, `asp-for="Property1"` becomes `m => m.Property1` in the generated code which is why you don't need to prefix with `Model`.</span></span> <span data-ttu-id="9ffe1-199">Можно использовать «@» символ для запуска встроенного выражения и переместить перед `m.`:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-199">You can use the "@" character to start an inline expression and move before the `m.`:</span></span>

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe" />
```

<span data-ttu-id="9ffe1-200">Дает следующий результат:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-200">Generates the following:</span></span>

```HTML
<input type="text" id="joe" name="joe" value="Joe" />
```

<span data-ttu-id="9ffe1-201">С помощью свойства коллекции `asp-for="CollectionProperty[23].Member"` приводит к возникновению ошибки совпадает с именем `asp-for="CollectionProperty[i].Member"` при `i` имеет значение `23`.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-201">With collection properties, `asp-for="CollectionProperty[23].Member"` generates the same name as `asp-for="CollectionProperty[i].Member"` when `i` has the value `23`.</span></span>

### <a name="navigating-child-properties"></a><span data-ttu-id="9ffe1-202">Навигация по дочерние свойства</span><span class="sxs-lookup"><span data-stu-id="9ffe1-202">Navigating child properties</span></span>

<span data-ttu-id="9ffe1-203">Также можно перейти к свойства дочерних элементов, используя путь к свойству модели представления.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-203">You can also navigate to child properties using the property path of the view model.</span></span> <span data-ttu-id="9ffe1-204">Рассмотрите возможность более сложные модели класс, который содержит дочерний элемент `Address` свойство.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-204">Consider a more complex model class that contains a child `Address` property.</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

<span data-ttu-id="9ffe1-205">В представлении привязки к `Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-205">In the view, we bind to `Address.AddressLine1`:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

<span data-ttu-id="9ffe1-206">Следующий код HTML, созданный для `Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-206">The following HTML is generated for `Address.AddressLine1`:</span></span>

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="" />
```

### <a name="expression-names-and-collections"></a><span data-ttu-id="9ffe1-207">Имена выражений и коллекций</span><span class="sxs-lookup"><span data-stu-id="9ffe1-207">Expression names and Collections</span></span>

<span data-ttu-id="9ffe1-208">Пример модели, содержащий массив `Colors`:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-208">Sample, a model containing an array of `Colors`:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

<span data-ttu-id="9ffe1-209">Метод действия:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-209">The action method:</span></span>

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
```

<span data-ttu-id="9ffe1-210">Следующие Razor показано, как получить доступ к определенному `Color` элемента:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-210">The following Razor shows how you access a specific `Color` element:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

<span data-ttu-id="9ffe1-211">*Views/Shared/EditorTemplates/String.cshtml* шаблона:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-211">The *Views/Shared/EditorTemplates/String.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

<span data-ttu-id="9ffe1-212">Пример с использованием `List<T>`:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-212">Sample using `List<T>`:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

<span data-ttu-id="9ffe1-213">Следующие Razor показан способ итерации по коллекции.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-213">The following Razor shows how to iterate over a collection:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

<span data-ttu-id="9ffe1-214">*Views/Shared/EditorTemplates/ToDoItem.cshtml* шаблона:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-214">The *Views/Shared/EditorTemplates/ToDoItem.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]


>[!NOTE]
><span data-ttu-id="9ffe1-215">Всегда используйте `for` (и *не* `foreach`) для прохода по списку.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-215">Always use `for` (and *not* `foreach`) to iterate over a list.</span></span> <span data-ttu-id="9ffe1-216">Оценка индексатора в LINQ выражения могут потреблять много ресурсов и должно быть минимальным.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-216">Evaluating an indexer in a LINQ expression can be expensive and should be minimized.</span></span>

&nbsp;

>[!NOTE]
><span data-ttu-id="9ffe1-217">Приведенный выше код комментариями образец показывает, как заменить лямбда-выражение с `@` оператор, чтобы иметь доступ к каждому `ToDoItem` в списке.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-217">The commented sample code above shows how you would replace the lambda expression with the `@` operator to access each `ToDoItem` in the list.</span></span>

## <a name="the-textarea-tag-helper"></a><span data-ttu-id="9ffe1-218">Вспомогательный объект Textarea тега</span><span class="sxs-lookup"><span data-stu-id="9ffe1-218">The Textarea Tag Helper</span></span>

<span data-ttu-id="9ffe1-219">`Textarea Tag Helper` Вспомогательные тег аналогичен вспомогательный тега входных данных.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-219">The `Textarea Tag Helper` tag helper is  similar to the Input Tag Helper.</span></span>

* <span data-ttu-id="9ffe1-220">Приводит к возникновению ошибки `id` и `name` атрибуты и атрибуты проверки данных из модели для [ \<textarea >](https://www.w3.org/wiki/HTML/Elements/textarea) элемента.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-220">Generates the `id` and `name` attributes, and the data validation attributes from the model for a [\<textarea>](https://www.w3.org/wiki/HTML/Elements/textarea) element.</span></span>

* <span data-ttu-id="9ffe1-221">Предоставляет строгую типизацию.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-221">Provides strong typing.</span></span>

* <span data-ttu-id="9ffe1-222">Альтернативный способ вспомогательный метод HTML.`Html.TextAreaFor`</span><span class="sxs-lookup"><span data-stu-id="9ffe1-222">HTML Helper alternative: `Html.TextAreaFor`</span></span>

<span data-ttu-id="9ffe1-223">Пример:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-223">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

<span data-ttu-id="9ffe1-224">Создается следующий код HTML:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-224">The following HTML is generated:</span></span>

```HTML
<form method="post" action="/Demo/RegisterTextArea">
  <textarea data-val="true"
   data-val-maxlength="The field Description must be a string or array type with a maximum length of &#x27;1024&#x27;."
   data-val-maxlength-max="1024"
   data-val-minlength="The field Description must be a string or array type with a minimum length of &#x27;5&#x27;."
   data-val-minlength-min="5"
   id="Description" name="Description">
  </textarea>
  <button type="submit">Test</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-label-tag-helper"></a><span data-ttu-id="9ffe1-225">Вспомогательный тег метки</span><span class="sxs-lookup"><span data-stu-id="9ffe1-225">The Label Tag Helper</span></span>

* <span data-ttu-id="9ffe1-226">Создает заголовок метки и `for` атрибут [ <label> ](https://www.w3.org/wiki/HTML/Elements/label) элемент для имени выражения</span><span class="sxs-lookup"><span data-stu-id="9ffe1-226">Generates the label caption and `for` attribute on a [<label>](https://www.w3.org/wiki/HTML/Elements/label) element for an expression name</span></span>

* <span data-ttu-id="9ffe1-227">Альтернативой вспомогательный метод HTML: `Html.LabelFor`.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-227">HTML Helper alternative: `Html.LabelFor`.</span></span>

<span data-ttu-id="9ffe1-228">`Label Tag Helper` Чистый HTML-элемент label обеспечивает следующие преимущества:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-228">The `Label Tag Helper`  provides the following benefits over a pure HTML label element:</span></span>

* <span data-ttu-id="9ffe1-229">Вы автоматически получаете значение описательную метку из `Display` атрибута.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-229">You automatically get the descriptive label value from the `Display` attribute.</span></span> <span data-ttu-id="9ffe1-230">Предполагаемого отображаемое имя может измениться с течением времени и сочетание `Display` атрибута и вспомогательные тег метка будет применяться `Display` everywhere его использовать.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-230">The intended display name might change over time, and the combination of `Display` attribute and Label Tag Helper will apply the `Display` everywhere it's used.</span></span>

* <span data-ttu-id="9ffe1-231">Меньше разметки в исходном коде</span><span class="sxs-lookup"><span data-stu-id="9ffe1-231">Less markup in source code</span></span>

* <span data-ttu-id="9ffe1-232">Строгая типизация с свойства модели.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-232">Strong typing with the model property.</span></span>

<span data-ttu-id="9ffe1-233">Пример:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-233">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

<span data-ttu-id="9ffe1-234">Следующий код HTML, созданный для `<label>` элемента:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-234">The following HTML is generated for the `<label>` element:</span></span>

```HTML
<label for="Email">Email Address</label>
```

<span data-ttu-id="9ffe1-235">Вспомогательное приложение тег метки создан `for` значение атрибута «Email», идентификатор, связанной с `<input>` элемента.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-235">The Label Tag Helper generated the `for` attribute value of "Email", which is the ID associated with the `<input>` element.</span></span> <span data-ttu-id="9ffe1-236">Создание согласованного вспомогательных функций тегов `id` и `for` элементы, чтобы они могли быть правильно связан.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-236">The Tag Helpers generate consistent `id` and `for` elements so they can be correctly associated.</span></span> <span data-ttu-id="9ffe1-237">Заголовок в этом примере берется из `Display` атрибута.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-237">The caption in this sample comes from the `Display` attribute.</span></span> <span data-ttu-id="9ffe1-238">Если модель не содержит `Display` атрибут, заголовок будет имя свойства выражение.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-238">If the model didn't contain a `Display` attribute, the caption would be the property name of the expression.</span></span>

## <a name="the-validation-tag-helpers"></a><span data-ttu-id="9ffe1-239">Вспомогательных функций тегов проверки</span><span class="sxs-lookup"><span data-stu-id="9ffe1-239">The Validation Tag Helpers</span></span>

<span data-ttu-id="9ffe1-240">Существует два вспомогательных функций тегов проверки.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-240">There are two Validation Tag Helpers.</span></span> <span data-ttu-id="9ffe1-241">`Validation Message Tag Helper` (Который отображает сообщение проверки для одного свойства в модели) и `Validation Summary Tag Helper` (отображает сводку ошибок проверки).</span><span class="sxs-lookup"><span data-stu-id="9ffe1-241">The `Validation Message Tag Helper` (which displays a validation message for a single property on your model), and the `Validation Summary Tag Helper` (which displays a summary of validation errors).</span></span> <span data-ttu-id="9ffe1-242">`Input Tag Helper` Добавляет HTML5 клиента стороны атрибуты проверки входных элементов на основе данных атрибуты заметок на классами модели.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-242">The `Input Tag Helper` adds HTML5 client side validation attributes to input elements based on data annotation attributes on your model classes.</span></span> <span data-ttu-id="9ffe1-243">Проверка также выполняется на сервере.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-243">Validation is also performed on the server.</span></span> <span data-ttu-id="9ffe1-244">Вспомогательный объект проверки тег отображает эти сообщения об ошибках при возникновении ошибки проверки.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-244">The Validation Tag Helper displays these error messages when a validation error occurs.</span></span>

### <a name="the-validation-message-tag-helper"></a><span data-ttu-id="9ffe1-245">Вспомогательный тег сообщение проверки</span><span class="sxs-lookup"><span data-stu-id="9ffe1-245">The Validation Message Tag Helper</span></span>

* <span data-ttu-id="9ffe1-246">Добавляет [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-valmsg-for="property"` атрибут [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) элемент, который присоединяет сообщений об ошибках проверки в поле ввода свойства указанной модели.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-246">Adds the [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  `data-valmsg-for="property"` attribute to the [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element, which attaches the validation error messages on the input field of the specified model property.</span></span> <span data-ttu-id="9ffe1-247">При возникновении ошибки проверки со стороны клиента, [jQuery](https://jquery.com/) отображает сообщение об ошибке в `<span>` элемент.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-247">When a client side validation error occurs, [jQuery](https://jquery.com/) displays the error message in the `<span>` element.</span></span>

* <span data-ttu-id="9ffe1-248">Проверка также выполняется на сервере.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-248">Validation also takes place on the server.</span></span> <span data-ttu-id="9ffe1-249">Клиенты могут быть JavaScript отключена, некоторые проверки может быть выполнено только на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-249">Clients may have JavaScript disabled and some validation can only be done on the server side.</span></span>

* <span data-ttu-id="9ffe1-250">Альтернативный способ вспомогательный метод HTML.`Html.ValidationMessageFor`</span><span class="sxs-lookup"><span data-stu-id="9ffe1-250">HTML Helper alternative: `Html.ValidationMessageFor`</span></span>

<span data-ttu-id="9ffe1-251">`Validation Message Tag Helper` Используется с `asp-validation-for` атрибута HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) элемента.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-251">The `Validation Message Tag Helper`  is used with the `asp-validation-for` attribute on a HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element.</span></span>

```HTML
<span asp-validation-for="Email"></span>
```

<span data-ttu-id="9ffe1-252">Вспомогательный тег сообщения проверки создает следующий код HTML:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-252">The Validation Message Tag Helper will generate the following HTML:</span></span>

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

<span data-ttu-id="9ffe1-253">Обычно используется `Validation Message Tag Helper` после `Input` вспомогательный тег для одного свойства.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-253">You generally use the `Validation Message Tag Helper`  after an `Input` Tag Helper for the same property.</span></span> <span data-ttu-id="9ffe1-254">При этом отображаются сообщения об ошибках проверки рядом входных данных, который вызвал ошибку.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-254">Doing so displays any validation error messages near the input that caused the error.</span></span>

> [!NOTE]
> <span data-ttu-id="9ffe1-255">Необходимо иметь представление с использованием правильного JavaScript и [jQuery](https://jquery.com/) скрипт ссылки на месте для проверки на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-255">You must have a view with the correct JavaScript and [jQuery](https://jquery.com/) script references in place for client side validation.</span></span> <span data-ttu-id="9ffe1-256">В разделе [проверка модели](../models/validation.md) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-256">See [Model Validation](../models/validation.md) for more information.</span></span>

<span data-ttu-id="9ffe1-257">В случае ошибки проверки со стороны сервера (например если имеется проверки на стороне пользовательского сервера или проверки на стороне клиента отключена), MVC помещает сообщение об ошибке в тексте `<span>` элемента.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-257">When a server side validation error occurs (for example when you have custom server side validation or client-side validation is disabled), MVC places that error message as the body of the `<span>` element.</span></span>

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a><span data-ttu-id="9ffe1-258">Вспомогательный тег сводки проверки</span><span class="sxs-lookup"><span data-stu-id="9ffe1-258">The Validation Summary Tag Helper</span></span>

* <span data-ttu-id="9ffe1-259">Целевые объекты `<div>` элементы с `asp-validation-summary` атрибута</span><span class="sxs-lookup"><span data-stu-id="9ffe1-259">Targets `<div>` elements with the `asp-validation-summary` attribute</span></span>

* <span data-ttu-id="9ffe1-260">Альтернативный способ вспомогательный метод HTML.`@Html.ValidationSummary`</span><span class="sxs-lookup"><span data-stu-id="9ffe1-260">HTML Helper alternative: `@Html.ValidationSummary`</span></span>

<span data-ttu-id="9ffe1-261">`Validation Summary Tag Helper` Используется для отображения сводку сообщений проверки.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-261">The `Validation Summary Tag Helper`  is used to display a summary of validation messages.</span></span> <span data-ttu-id="9ffe1-262">`asp-validation-summary` Значением атрибута может быть любой из следующих:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-262">The `asp-validation-summary` attribute value can be any of the following:</span></span>

|<span data-ttu-id="9ffe1-263">ASP сводки проверки</span><span class="sxs-lookup"><span data-stu-id="9ffe1-263">asp-validation-summary</span></span>|<span data-ttu-id="9ffe1-264">Проверка сообщения.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-264">Validation messages displayed</span></span>|
|--- |--- |
|<span data-ttu-id="9ffe1-265">ValidationSummary.All</span><span class="sxs-lookup"><span data-stu-id="9ffe1-265">ValidationSummary.All</span></span>|<span data-ttu-id="9ffe1-266">Свойство и модель уровня</span><span class="sxs-lookup"><span data-stu-id="9ffe1-266">Property and model level</span></span>|
|<span data-ttu-id="9ffe1-267">ValidationSummary.ModelOnly</span><span class="sxs-lookup"><span data-stu-id="9ffe1-267">ValidationSummary.ModelOnly</span></span>|<span data-ttu-id="9ffe1-268">Модель</span><span class="sxs-lookup"><span data-stu-id="9ffe1-268">Model</span></span>|
|<span data-ttu-id="9ffe1-269">ValidationSummary.None</span><span class="sxs-lookup"><span data-stu-id="9ffe1-269">ValidationSummary.None</span></span>|<span data-ttu-id="9ffe1-270">Нет</span><span class="sxs-lookup"><span data-stu-id="9ffe1-270">None</span></span>|

### <a name="sample"></a><span data-ttu-id="9ffe1-271">Пример</span><span class="sxs-lookup"><span data-stu-id="9ffe1-271">Sample</span></span>

<span data-ttu-id="9ffe1-272">В следующем примере модель данных снабжен `DataAnnotation` атрибутов, которые создает сообщения об ошибках проверки для `<input>` элемента.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-272">In the following example, the data model is decorated with `DataAnnotation` attributes, which generates validation error messages on the `<input>` element.</span></span>  <span data-ttu-id="9ffe1-273">При возникновении ошибки проверки, вспомогательный класс проверки тег отображает сообщение об ошибке:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-273">When a validation error occurs, the Validation Tag Helper displays the error message:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

<span data-ttu-id="9ffe1-274">Созданный код HTML (если модель допустима):</span><span class="sxs-lookup"><span data-stu-id="9ffe1-274">The generated HTML (when the model is valid):</span></span>

```HTML
<form action="/DemoReg/Register" method="post">
  <div class="validation-summary-valid" data-valmsg-summary="true">
  <ul><li style="display:none"></li></ul></div>
  Email:  <input name="Email" id="Email" type="email" value=""
   data-val-required="The Email field is required."
   data-val-email="The Email field is not a valid e-mail address."
   data-val="true"> <br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Email"></span><br>
  Password: <input name="Password" id="Password" type="password"
   data-val-required="The Password field is required." data-val="true"><br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Password"></span><br>
  <button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-select-tag-helper"></a><span data-ttu-id="9ffe1-275">Вспомогательный объект выберите тег.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-275">The Select Tag Helper</span></span>

* <span data-ttu-id="9ffe1-276">Приводит к возникновению ошибки [выберите](https://www.w3.org/wiki/HTML/Elements/select) и связанные [параметр](https://www.w3.org/wiki/HTML/Elements/option) элементы для свойств модели.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-276">Generates [select](https://www.w3.org/wiki/HTML/Elements/select) and associated [option](https://www.w3.org/wiki/HTML/Elements/option) elements for properties of your model.</span></span>

* <span data-ttu-id="9ffe1-277">Вспомогательный метод HTML вариант `Html.DropDownListFor` и`Html.ListBoxFor`</span><span class="sxs-lookup"><span data-stu-id="9ffe1-277">Has an HTML Helper alternative `Html.DropDownListFor` and `Html.ListBoxFor`</span></span>

<span data-ttu-id="9ffe1-278">`Select Tag Helper` `asp-for` Указывает имя свойства модели для [выберите](https://www.w3.org/wiki/HTML/Elements/select) элемент и `asp-items` указывает [параметр](https://www.w3.org/wiki/HTML/Elements/option) элементов.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-278">The `Select Tag Helper` `asp-for` specifies the model property  name for the [select](https://www.w3.org/wiki/HTML/Elements/select) element  and `asp-items` specifies the [option](https://www.w3.org/wiki/HTML/Elements/option) elements.</span></span>  <span data-ttu-id="9ffe1-279">Пример:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-279">For example:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

<span data-ttu-id="9ffe1-280">Пример:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-280">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

<span data-ttu-id="9ffe1-281">`Index` Инициализирует метод `CountryViewModel`, задает выбранной страны и передает их в `Index` представления.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-281">The `Index` method initializes the `CountryViewModel`, sets the selected country and passes it to the `Index` view.</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

<span data-ttu-id="9ffe1-282">HTTP POST `Index` метод отображает выбор:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-282">The HTTP POST `Index` method displays the selection:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

<span data-ttu-id="9ffe1-283">`Index` Представления:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-283">The `Index` view:</span></span>

[!code-cshtml[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

<span data-ttu-id="9ffe1-284">Который генерирует следующий код HTML (с «CA» выбран):</span><span class="sxs-lookup"><span data-stu-id="9ffe1-284">Which generates the following HTML (with "CA" selected):</span></span>

```html
<form method="post" action="/">
     <select id="Country" name="Country">
       <option value="MX">Mexico</option>
       <option selected="selected" value="CA">Canada</option>
       <option value="US">USA</option>
     </select>
       <br /><button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

> [!NOTE]
> <span data-ttu-id="9ffe1-285">Мы не рекомендуем использовать `ViewBag` или `ViewData` со вспомогательным методом выберите тег.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-285">We do not recommend using `ViewBag` or `ViewData` with the Select Tag Helper.</span></span> <span data-ttu-id="9ffe1-286">Модель представления, более надежными, при предоставлении метаданных для MVC и обычно менее проблематичным.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-286">A view model is more robust at providing MVC metadata and generally less problematic.</span></span>

<span data-ttu-id="9ffe1-287">`asp-for` Значение атрибута является особым случаем и не требует `Model` префикса, выполните атрибуты вспомогательного тег (например, `asp-items`)</span><span class="sxs-lookup"><span data-stu-id="9ffe1-287">The `asp-for` attribute value is a special case and doesn't require a `Model` prefix, the other Tag Helper attributes do (such as `asp-items`)</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a><span data-ttu-id="9ffe1-288">Перечисление привязки</span><span class="sxs-lookup"><span data-stu-id="9ffe1-288">Enum binding</span></span>

<span data-ttu-id="9ffe1-289">Часто бывает удобно использовать `<select>` с `enum` свойства и создавать `SelectListItem` элементы из `enum` значения.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-289">It's often convenient to use `<select>` with an `enum` property and generate the `SelectListItem` elements from the `enum` values.</span></span>

<span data-ttu-id="9ffe1-290">Пример:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-290">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

<span data-ttu-id="9ffe1-291">`GetEnumSelectList` Метод создает `SelectList` объектов для перечисления.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-291">The `GetEnumSelectList` method generates a `SelectList` object for an enum.</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

<span data-ttu-id="9ffe1-292">Вы можете декорировать список перечислителя с `Display` атрибут для получения более полных пользовательского интерфейса:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-292">You can decorate your enumerator list with the `Display` attribute to get a richer UI:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

<span data-ttu-id="9ffe1-293">Создается следующий код HTML:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-293">The following HTML is generated:</span></span>

```HTML
  <form method="post" action="/Home/IndexEnum">
         <select data-val="true" data-val-required="The EnumCountry field is required."
                 id="EnumCountry" name="EnumCountry">
             <option value="0">United Mexican States</option>
             <option value="1">United States of America</option>
             <option value="2">Canada</option>
             <option value="3">France</option>
             <option value="4">Germany</option>
             <option selected="selected" value="5">Spain</option>
         </select>
         <br /><button type="submit">Register</button>
         <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

### <a name="option-group"></a><span data-ttu-id="9ffe1-294">Группа параметров</span><span class="sxs-lookup"><span data-stu-id="9ffe1-294">Option Group</span></span>

<span data-ttu-id="9ffe1-295">HTML [ \<optgroup >](https://www.w3.org/wiki/HTML/Elements/optgroup) элемент создается в том случае, если модель представления содержит один или несколько `SelectListGroup` объектов.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-295">The HTML  [\<optgroup>](https://www.w3.org/wiki/HTML/Elements/optgroup) element is generated when the view model contains one or more `SelectListGroup` objects.</span></span>

<span data-ttu-id="9ffe1-296">`CountryViewModelGroup` Группы `SelectListItem` элементы в группах «Северная Америка» и «Европа»:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-296">The `CountryViewModelGroup` groups the `SelectListItem` elements into the "North America" and "Europe" groups:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

<span data-ttu-id="9ffe1-297">Ниже приведены две группы:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-297">The two groups are shown below:</span></span>

![Пример группы параметр](working-with-forms/_static/grp.png)

<span data-ttu-id="9ffe1-299">Созданный HTML-код:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-299">The generated HTML:</span></span>

```HTML
 <form method="post" action="/Home/IndexGroup">
      <select id="Country" name="Country">
          <optgroup label="North America">
              <option value="MEX">Mexico</option>
              <option value="CAN">Canada</option>
              <option value="US">USA</option>
          </optgroup>
          <optgroup label="Europe">
              <option value="FR">France</option>
              <option value="ES">Spain</option>
              <option value="DE">Germany</option>
          </optgroup>
      </select>
      <br /><button type="submit">Register</button>
      <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
```

### <a name="multiple-select"></a><span data-ttu-id="9ffe1-300">Выделить несколько</span><span class="sxs-lookup"><span data-stu-id="9ffe1-300">Multiple select</span></span>

<span data-ttu-id="9ffe1-301">Вспомогательный объект выберите тег автоматически создаст [несколько = «несколько»](http://w3c.github.io/html-reference/select.html) атрибута, если свойства, указанного в `asp-for` атрибут `IEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-301">The Select Tag Helper  will automatically generate the [multiple = "multiple"](http://w3c.github.io/html-reference/select.html)  attribute if the property specified in the `asp-for` attribute is an `IEnumerable`.</span></span> <span data-ttu-id="9ffe1-302">Например при наличии следующей модели:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-302">For example, given the following model:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

<span data-ttu-id="9ffe1-303">Следующее представление:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-303">With the following view:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

<span data-ttu-id="9ffe1-304">Создает следующий код HTML:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-304">Generates the following HTML:</span></span>

```HTML
<form method="post" action="/Home/IndexMultiSelect">
    <select id="CountryCodes"
    multiple="multiple"
    name="CountryCodes"><option value="MX">Mexico</option>
<option value="CA">Canada</option>
<option value="US">USA</option>
<option value="FR">France</option>
<option value="ES">Spain</option>
<option value="DE">Germany</option>
</select>
    <br /><button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

### <a name="no-selection"></a><span data-ttu-id="9ffe1-305">Не выбрано</span><span class="sxs-lookup"><span data-stu-id="9ffe1-305">No selection</span></span>

<span data-ttu-id="9ffe1-306">Если найти самостоятельно, используя параметр «не указано» на нескольких страницах, можно создать шаблон, чтобы исключить повторяющиеся HTML:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-306">If you find yourself using the "not specified" option in multiple pages, you can create a template to eliminate repeating the HTML:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

<span data-ttu-id="9ffe1-307">*Views/Shared/EditorTemplates/CountryViewModel.cshtml* шаблона:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-307">The *Views/Shared/EditorTemplates/CountryViewModel.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

<span data-ttu-id="9ffe1-308">Добавление HTML [ \<параметр >](https://www.w3.org/wiki/HTML/Elements/option) элементы не ограничивается *выбора не* регистр.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-308">Adding HTML [\<option>](https://www.w3.org/wiki/HTML/Elements/option) elements is not limited to the *No selection* case.</span></span> <span data-ttu-id="9ffe1-309">Например следующий метод представление и действие создаст аналогично приведенный выше код HTML:</span><span class="sxs-lookup"><span data-stu-id="9ffe1-309">For example, the following view and action method will generate HTML similar to the code above:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

<span data-ttu-id="9ffe1-310">Правильные `<option>` будет выбран элемент (содержат `selected="selected"` атрибута) в зависимости от текущего `Country` значение.</span><span class="sxs-lookup"><span data-stu-id="9ffe1-310">The correct `<option>` element will be selected ( contain the `selected="selected"` attribute) depending on the current `Country` value.</span></span>

```HTML
 <form method="post" action="/Home/IndexEmpty">
      <select id="Country" name="Country">
          <option value="">&lt;none&gt;</option>
          <option value="MX">Mexico</option>
          <option value="CA" selected="selected">Canada</option>
          <option value="US">USA</option>
      </select>
      <br /><button type="submit">Register</button>
   <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
 ```

## <a name="additional-resources"></a><span data-ttu-id="9ffe1-311">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="9ffe1-311">Additional Resources</span></span>

* [<span data-ttu-id="9ffe1-312">Вспомогательные функции тегов</span><span class="sxs-lookup"><span data-stu-id="9ffe1-312">Tag Helpers</span></span>](tag-helpers/intro.md)

* [<span data-ttu-id="9ffe1-313">Элемент HTML-формы</span><span class="sxs-lookup"><span data-stu-id="9ffe1-313">HTML Form element</span></span>](https://www.w3.org/TR/html401/interact/forms.html)

* [<span data-ttu-id="9ffe1-314">Маркер проверки запроса</span><span class="sxs-lookup"><span data-stu-id="9ffe1-314">Request Verification Token</span></span>](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)

* [<span data-ttu-id="9ffe1-315">Привязка модели</span><span class="sxs-lookup"><span data-stu-id="9ffe1-315">Model Binding</span></span>](../models/model-binding.md)

* [<span data-ttu-id="9ffe1-316">Проверка модели</span><span class="sxs-lookup"><span data-stu-id="9ffe1-316">Model Validation</span></span>](../models/validation.md)

* [<span data-ttu-id="9ffe1-317">заметок к данным</span><span class="sxs-lookup"><span data-stu-id="9ffe1-317">data annotations</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)

* <span data-ttu-id="9ffe1-318">[Фрагменты для этого документа кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample).</span><span class="sxs-lookup"><span data-stu-id="9ffe1-318">[Code snippets for this document](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample).</span></span>
