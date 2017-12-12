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
ms.openlocfilehash: da36985206521798d3bfe71f6372dc5cc4fca09a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-using-tag-helpers-in-forms-in-aspnet-core"></a>Общие сведения об использовании вспомогательных функций тегов в форм в ASP.NET Core

По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), и [Jerrie Pelser](https://github.com/jerriep)

В этом документе демонстрируется работа с форм и элементов HTML, который обычно используется в форме. HTML [формы](https://www.w3.org/TR/html401/interact/forms.html) элемент предоставляет использования приложений Интернета основным механизмом для отправки данных на сервер. Большая часть этого документа описываются [вспомогательных функций тегов](tag-helpers/intro.md) и как они могут помочь в продуктивно создавать надежные HTML-формы. Рекомендуется прочитать [Общие сведения о вспомогательных функций тегов](tag-helpers/intro.md) перед прочтением этого документа.

Во многих случаях вспомогательные методы HTML предоставляют альтернативный подход для определенных вспомогательных тег, но это следует отметить, что вспомогательных функций тегов не заменяют вспомогательных методов HTML, а не существует тег вспомогательный класс для каждого вспомогательный метод HTML. Если существует альтернатива вспомогательный метод HTML, он упоминается.

<a name="my-asp-route-param-ref-label"></a>

## <a name="the-form-tag-helper"></a>Вспомогательный тег формы

[Формы](https://www.w3.org/TR/html401/interact/forms.html) тег вспомогательный:

* Создает HTML-код [ \<формы >](https://www.w3.org/TR/html401/interact/forms.html) `action` значение атрибута для действия контроллера MVC или именованный маршрут

* Создает скрытый [токена проверки запроса](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) для предотвращения подделки межсайтовых запросов (при использовании с `[ValidateAntiForgeryToken]` атрибута в методе действия HTTP Post)

* Предоставляет `asp-route-<Parameter Name>` атрибут, где `<Parameter Name>` добавляется значения маршрута. `routeValues` Параметры `Html.BeginForm` и `Html.BeginRouteForm` предоставляют аналогичные функциональные возможности.

* Вспомогательный метод HTML вариант `Html.BeginForm` и`Html.BeginRouteForm`

Пример:

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

Тег формы вспомогательного выше создает следующий код HTML:

```HTML
<form method="post" action="/Demo/Register">
     <!-- Input and Submit elements -->
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

Среда выполнения MVC создает `action` значение атрибута на основе атрибутов вспомогательный тег формы `asp-controller` и `asp-action`. Вспомогательный тег формы также создает скрытый [токена проверки запроса](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) для предотвращения подделки межсайтовых запросов (при использовании с `[ValidateAntiForgeryToken]` атрибута в методе действия HTTP Post). Трудно защиты от подделки межсайтовых запросов чистый HTML-формы, эта служба автоматически предоставляет вспомогательный тега формы.

### <a name="using-a-named-route"></a>С помощью именованный маршрут

`asp-route` Атрибут вспомогательной функции тегов также можно создавать разметку HTML `action` атрибут. Приложение с [маршрута](../../fundamentals/routing.md) с именем `register` использовать следующую разметку страницы регистрации:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

Множество представлений в *представления и учетная запись* папку (созданы при создании нового веб-приложения с *отдельных учетных записей пользователей*) содержать [asp маршрута returnurl](https://docs.microsoft.com/aspnet/core/mvc/views/working-with-forms) атрибут:

```cshtml
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
```

>[!NOTE]
>С помощью встроенных шаблонов `returnUrl` только заполняется автоматически при пытаются получить доступ к авторизованным ресурсов, но не с проверкой подлинности или авторизации. При попытке несанкционированного доступа, по промежуточного слоя безопасности вы будете перенаправлены на страницу входа с `returnUrl` значение.

## <a name="the-input-tag-helper"></a>Вспомогательный объект тега входных данных

Вспомогательный объект входного тег привязывает элемент HTML [ \<ввода >](https://www.w3.org/wiki/HTML/Elements/input) элемент модели выражения в представлении razor.

Синтаксис:

```HTML
<input asp-for="<Expression Name>" />
```

Вспомогательный объект тега входных данных:

* Приводит к возникновению ошибки `id` и `name` атрибуты HTML для имени выражения, указанного в `asp-for` атрибута. `asp-for="Property1.Property2"` равно `m => m.Property1.Property2`. Имя выражения используется для `asp-for` значение атрибута. В разделе [имена выражений](#expression-names) Дополнительные сведения.

* Задает HTML- `type` атрибута значение в зависимости от типа модели и [заметки к данным](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) атрибуты, примененные к свойству модели

* Не перезаписывает HTML `type` значение атрибута, если он указан

* Приводит к возникновению ошибки [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) атрибуты проверки из [заметки к данным](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) атрибуты, примененные к свойства модели

* Предусмотрена функция вспомогательный метод HTML перекрывать `Html.TextBoxFor` и `Html.EditorFor`. В разделе **вспомогательный метод HTML альтернативы вспомогательный тега входных данных** подробные сведения см.

* Предоставляет строгую типизацию. Если имя свойства изменяется и не обновлять вспомогательный тег вы получите ошибку следующего вида:

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

`Input` Вспомогательный тег задает HTML- `type` атрибут на основе .NET типа. В следующей таблице перечислены некоторые распространенные типы .NET и созданный тип HTML (не каждый тип .NET присутствует).

|Тип .NET|Тип входных данных|
|---|---|
|Bool|Тип = «checkbox»|
|Строка|Тип = «text»|
|DateTime|тип = «datetime»|
|Byte|Тип = «number»|
|Int|Тип = «number»|
|Single, Double|Тип = «number»|


В следующей таблице приведены некоторые наиболее распространенные [заметок к данным](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) атрибуты, сопоставленные вспомогательный тега входных данных для определенного типа входных данных (не каждый атрибут проверки присутствует):


|Атрибут|Тип входных данных|
|---|---|
|[EmailAddress]|Тип = «email»|
|[URL-адрес]|Тип = «url»|
|[HiddenInput]|Тип = «hidden»|
|[Phone]|Тип = «телефон»|
|[DataType(DataType.Password)]| Тип = «password»|
|[DataType(DataType.Date)]| тип = «date»|
|[DataType(DataType.Time)]| Тип = «время»|


Пример:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

Приведенный выше код создает следующий код HTML:

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

Заметки данных применяется к `Email` и `Password` свойства создавать метаданные для модели. Вспомогательный тега входных данных считывает метаданные модели и создает [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` атрибутов (см. [проверка модели](../models/validation.md)). Эти атрибуты описывают проверяющие элементы управления для присоединения к поля ввода. Это обеспечивает ненавязчивого HTML5 и [jQuery](https://jquery.com/) проверки. Атрибуты ненавязчивой имеют формат `data-val-rule="Error Message"`, где правила — это имя правила проверки (например, `data-val-required`, `data-val-email`, `data-val-maxlength`и т. д.) Если сообщение об ошибке в атрибуте, то он отображается как значение для `data-val-rule` атрибута. Также существуют атрибуты формы `data-val-ruleName-argumentName="argumentValue"` , содержат дополнительные сведения о правиле, например, `data-val-maxlength-max="1024"` .

### <a name="html-helper-alternatives-to-input-tag-helper"></a>Вспомогательный метод HTML альтернативы вспомогательный тега входных данных

`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` и `Html.EditorFor` пересекаться функции со вспомогательным методом тега входных данных. Вспомогательный тега входных данных будет автоматически присваивать параметру `type` по умолчанию. `Html.TextBox` и `Html.TextBoxFor` не будет. `Html.Editor`и `Html.EditorFor` обработки коллекций, сложные объекты и шаблоны; не поддерживает вспомогательные тега входных данных. Вспомогательный тега входных данных, `Html.EditorFor` и `Html.TextBoxFor` являются строго типизированными (они используют лямбда-выражений); `Html.TextBox` и `Html.Editor` не являются (они используют имена выражений).

### <a name="htmlattributes"></a>HtmlAttributes

`@Html.Editor()`и `@Html.EditorFor()` использовать специальное `ViewDataDictionary` записи с именем `htmlAttributes` при выполнении их шаблонов по умолчанию. Это поведение также дополнен с помощью `additionalViewData` параметров. Раздел «htmlAttributes» не учитывается регистр. Ключ «htmlAttributes» обрабатывается так же, как `htmlAttributes` объект, переданный в ввода вспомогательные методы, такие как `@Html.TextBox()`.

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a>Имена выражений

`asp-for` Значение атрибута `ModelExpression` и правую часть лямбда-выражения. Таким образом `asp-for="Property1"` становится `m => m.Property1` в созданный код, поэтому нет необходимости префикс `Model`. Можно использовать «@» символ для запуска встроенного выражения и переместить перед `m.`:

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe" />
```

Дает следующий результат:

```HTML
<input type="text" id="joe" name="joe" value="Joe" />
```

С помощью свойства коллекции `asp-for="CollectionProperty[23].Member"` приводит к возникновению ошибки совпадает с именем `asp-for="CollectionProperty[i].Member"` при `i` имеет значение `23`.

### <a name="navigating-child-properties"></a>Навигация по дочерние свойства

Также можно перейти к свойства дочерних элементов, используя путь к свойству модели представления. Рассмотрите возможность более сложные модели класс, который содержит дочерний элемент `Address` свойство.

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

В представлении привязки к `Address.AddressLine1`:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

Следующий код HTML, созданный для `Address.AddressLine1`:

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="" />
```

### <a name="expression-names-and-collections"></a>Имена выражений и коллекций

Пример модели, содержащий массив `Colors`:

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

Метод действия:

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
```

Следующие Razor показано, как получить доступ к определенному `Color` элемента:

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

*Views/Shared/EditorTemplates/String.cshtml* шаблона:

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

Пример с использованием `List<T>`:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

Следующие Razor показан способ итерации по коллекции.

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

*Views/Shared/EditorTemplates/ToDoItem.cshtml* шаблона:

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]


>[!NOTE]
>Всегда используйте `for` (и *не* `foreach`) для прохода по списку. Оценка индексатора в LINQ выражения могут потреблять много ресурсов и должно быть минимальным.

&nbsp;

>[!NOTE]
>Приведенный выше код комментариями образец показывает, как заменить лямбда-выражение с `@` оператор, чтобы иметь доступ к каждому `ToDoItem` в списке.

## <a name="the-textarea-tag-helper"></a>Вспомогательный объект Textarea тега

`Textarea Tag Helper` Вспомогательные тег аналогичен вспомогательный тега входных данных.

* Приводит к возникновению ошибки `id` и `name` атрибуты и атрибуты проверки данных из модели для [ \<textarea >](https://www.w3.org/wiki/HTML/Elements/textarea) элемента.

* Предоставляет строгую типизацию.

* Альтернативный способ вспомогательный метод HTML.`Html.TextAreaFor`

Пример:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

Создается следующий код HTML:

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

## <a name="the-label-tag-helper"></a>Вспомогательный тег метки

* Создает заголовок метки и `for` атрибут [ <label> ](https://www.w3.org/wiki/HTML/Elements/label) элемент для имени выражения

* Альтернативой вспомогательный метод HTML: `Html.LabelFor`.

`Label Tag Helper` Чистый HTML-элемент label обеспечивает следующие преимущества:

* Вы автоматически получаете значение описательную метку из `Display` атрибута. Предполагаемого отображаемое имя может измениться с течением времени и сочетание `Display` атрибута и вспомогательные тег метка будет применяться `Display` everywhere его использовать.

* Меньше разметки в исходном коде

* Строгая типизация с свойства модели.

Пример:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

Следующий код HTML, созданный для `<label>` элемента:

```HTML
<label for="Email">Email Address</label>
```

Вспомогательное приложение тег метки создан `for` значение атрибута «Email», идентификатор, связанной с `<input>` элемента. Создание согласованного вспомогательных функций тегов `id` и `for` элементы, чтобы они могли быть правильно связан. Заголовок в этом примере берется из `Display` атрибута. Если модель не содержит `Display` атрибут, заголовок будет имя свойства выражение.

## <a name="the-validation-tag-helpers"></a>Вспомогательных функций тегов проверки

Существует два вспомогательных функций тегов проверки. `Validation Message Tag Helper` (Который отображает сообщение проверки для одного свойства в модели) и `Validation Summary Tag Helper` (отображает сводку ошибок проверки). `Input Tag Helper` Добавляет HTML5 клиента стороны атрибуты проверки входных элементов на основе данных атрибуты заметок на классами модели. Проверка также выполняется на сервере. Вспомогательный объект проверки тег отображает эти сообщения об ошибках при возникновении ошибки проверки.

### <a name="the-validation-message-tag-helper"></a>Вспомогательный тег сообщение проверки

* Добавляет [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-valmsg-for="property"` атрибут [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) элемент, который присоединяет сообщений об ошибках проверки в поле ввода свойства указанной модели. При возникновении ошибки проверки со стороны клиента, [jQuery](https://jquery.com/) отображает сообщение об ошибке в `<span>` элемент.

* Проверка также выполняется на сервере. Клиенты могут быть JavaScript отключена, некоторые проверки может быть выполнено только на стороне сервера.

* Альтернативный способ вспомогательный метод HTML.`Html.ValidationMessageFor`

`Validation Message Tag Helper` Используется с `asp-validation-for` атрибута HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) элемента.

```HTML
<span asp-validation-for="Email"></span>
```

Вспомогательный тег сообщения проверки создает следующий код HTML:

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

Обычно используется `Validation Message Tag Helper` после `Input` вспомогательный тег для одного свойства. При этом отображаются сообщения об ошибках проверки рядом входных данных, который вызвал ошибку.

> [!NOTE]
> Необходимо иметь представление с использованием правильного JavaScript и [jQuery](https://jquery.com/) скрипт ссылки на месте для проверки на стороне клиента. В разделе [проверка модели](../models/validation.md) для получения дополнительной информации.

В случае ошибки проверки со стороны сервера (например если имеется проверки на стороне пользовательского сервера или проверки на стороне клиента отключена), MVC помещает сообщение об ошибке в тексте `<span>` элемента.

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a>Вспомогательный тег сводки проверки

* Целевые объекты `<div>` элементы с `asp-validation-summary` атрибута

* Альтернативный способ вспомогательный метод HTML.`@Html.ValidationSummary`

`Validation Summary Tag Helper` Используется для отображения сводку сообщений проверки. `asp-validation-summary` Значением атрибута может быть любой из следующих:

|ASP сводки проверки|Проверка сообщения.|
|--- |--- |
|ValidationSummary.All|Свойство и модель уровня|
|ValidationSummary.ModelOnly|Модель|
|ValidationSummary.None|Нет|

### <a name="sample"></a>Пример

В следующем примере модель данных снабжен `DataAnnotation` атрибутов, которые создает сообщения об ошибках проверки для `<input>` элемента.  При возникновении ошибки проверки, вспомогательный класс проверки тег отображает сообщение об ошибке:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

Созданный код HTML (если модель допустима):

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

## <a name="the-select-tag-helper"></a>Вспомогательный объект выберите тег.

* Приводит к возникновению ошибки [выберите](https://www.w3.org/wiki/HTML/Elements/select) и связанные [параметр](https://www.w3.org/wiki/HTML/Elements/option) элементы для свойств модели.

* Вспомогательный метод HTML вариант `Html.DropDownListFor` и`Html.ListBoxFor`

`Select Tag Helper` `asp-for` Указывает имя свойства модели для [выберите](https://www.w3.org/wiki/HTML/Elements/select) элемент и `asp-items` указывает [параметр](https://www.w3.org/wiki/HTML/Elements/option) элементов.  Пример:

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

Пример:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

`Index` Инициализирует метод `CountryViewModel`, задает выбранной страны и передает их в `Index` представления.

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

HTTP POST `Index` метод отображает выбор:

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

`Index` Представления:

[!code-cshtml[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

Который генерирует следующий код HTML (с «CA» выбран):

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
> Мы не рекомендуем использовать `ViewBag` или `ViewData` со вспомогательным методом выберите тег. Модель представления, более надежными, при предоставлении метаданных для MVC и обычно менее проблематичным.

`asp-for` Значение атрибута является особым случаем и не требует `Model` префикса, выполните атрибуты вспомогательного тег (например, `asp-items`)

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a>Перечисление привязки

Часто бывает удобно использовать `<select>` с `enum` свойства и создавать `SelectListItem` элементы из `enum` значения.

Пример:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

`GetEnumSelectList` Метод создает `SelectList` объектов для перечисления.

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

Вы можете декорировать список перечислителя с `Display` атрибут для получения более полных пользовательского интерфейса:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

Создается следующий код HTML:

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

### <a name="option-group"></a>Группа параметров

HTML [ \<optgroup >](https://www.w3.org/wiki/HTML/Elements/optgroup) элемент создается в том случае, если модель представления содержит один или несколько `SelectListGroup` объектов.

`CountryViewModelGroup` Группы `SelectListItem` элементы в группах «Северная Америка» и «Европа»:

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

Ниже приведены две группы:

![Пример группы параметр](working-with-forms/_static/grp.png)

Созданный HTML-код:

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

### <a name="multiple-select"></a>Выделить несколько

Вспомогательный объект выберите тег автоматически создаст [несколько = «несколько»](http://w3c.github.io/html-reference/select.html) атрибута, если свойства, указанного в `asp-for` атрибут `IEnumerable`. Например при наличии следующей модели:

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

Следующее представление:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

Создает следующий код HTML:

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

### <a name="no-selection"></a>Не выбрано

Если найти самостоятельно, используя параметр «не указано» на нескольких страницах, можно создать шаблон, чтобы исключить повторяющиеся HTML:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

*Views/Shared/EditorTemplates/CountryViewModel.cshtml* шаблона:

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

Добавление HTML [ \<параметр >](https://www.w3.org/wiki/HTML/Elements/option) элементы не ограничивается *выбора не* регистр. Например следующий метод представление и действие создаст аналогично приведенный выше код HTML:

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

Правильные `<option>` будет выбран элемент (содержат `selected="selected"` атрибута) в зависимости от текущего `Country` значение.

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

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Вспомогательные функции тегов](tag-helpers/intro.md)

* [Элемент HTML-формы](https://www.w3.org/TR/html401/interact/forms.html)

* [Маркер проверки запроса](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)

* [Привязка модели](../models/model-binding.md)

* [Проверка модели](../models/validation.md)

* [заметок к данным](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)

* [Фрагменты для этого документа кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample).
