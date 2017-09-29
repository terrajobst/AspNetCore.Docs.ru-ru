---
title: "Справочник по синтаксису Razor для ASP.NET Core"
author: rick-anderson
description: "Подробное описание синтаксиса Razor"
keywords: ASP.NET Core, Razor
ms.author: riande
manager: wpickett
ms.date: 07/04/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: 0e65f0e9f672f9f93256ebc039ea0db2e4ef5ae0
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/28/2017
---
# <a name="razor-syntax-for-aspnet-core"></a>Синтаксис Razor ASP.NET Core

По [Taylor Mullen](https://twitter.com/ntaylormullen) и [Рик Андерсон](https://twitter.com/RickAndMSFT)

## <a name="what-is-razor"></a>Что такое Razor

Razor является синтаксис разметки для внедрения серверного кода в веб-страницы. Синтаксис Razor состоит из разметки Razor, C# и HTML. Файлы, содержащие Razor, обычно имеют *.cshtml* расширение имени файла.

## <a name="rendering-html"></a>Подготовка к просмотру HTML

Язык по умолчанию Razor — HTML. Подготовка к просмотру HTML из Razor не отличается от HTML-файла. Файл Razor с следующую разметку:

```html
<p>Hello World</p>
```

Подготавливается к просмотру без изменений `<p>Hello World</p>` сервером.

## <a name="razor-syntax"></a>Синтаксис Razor

Razor поддерживает C# и использует `@` символ переход из HTML для C#. Razor вычисляет выражения C# и отображает их в выходных данных HTML. Razor может выполнять переход с HTML на C# или на разметку Razor. Когда `@` следуют символ [Razor зарезервированное ключевое слово](#razor-reserved-keywords) он переходит в разметку Razor, в противном случае он переходит в обычный C#.

<a name=escape-at-label></a>

Содержащий HTML `@` символы необходимо экранировать со вторым `@` символов. Пример:

```cshtml
<p>@@Username</p>
```

будет отображен следующий код HTML:

```cshtml
<p>@Username</p>
```

<a name=razor-email-ref></a>

HTML-атрибутов и содержимого, содержащий адреса электронной почты не обрабатывают `@` символ как символ перехода.

   `<a href="mailto:Support@contoso.com">Support@contoso.com</a>`

## <a name="implicit-razor-expressions"></a>Неявные выражения Razor

Неявные Razor выражения начинаются со `@` вместе с кодом C#. Пример:

```html
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

За исключением C# `await` неявные выражения ключевое слово не должно содержать пробелов. Например можно intermingle пробелы при условии, что оператор C# имеет очистить окончания:

```html
<p>@await DoSomething("hello", "world")</p>
```

## <a name="explicit-razor-expressions"></a>Прямые выражения Razor

Прямые Razor выражения состоит из символа с балансировкой скобки @. Например, для подготовки к просмотру время прошлой недели:

```html
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

Любое содержимое в @ круглая скобка () вычисляется и отображается в выходных данных.

Неявные выражения обычно не могут содержать пробелы. Например в следующем коде одну неделю не вычитается из текущего времени:

[!code-html[Main](razor/sample/Views/Home/Contact.cshtml?range=20)]

Который отображает следующий код HTML:

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

Явное выражение можно использовать для объединения текста с результатом выражения:

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

Без явного выражение `<p>Age@joe.Age</p>` будет рассматриваться как адрес электронной почты и `<p>Age@joe.Age</p>` будет проходить. Если они написаны как выражение явного `<p>Age33</p>` подготавливается к просмотру.

<a name=expression-encoding-label></a>

## <a name="expression-encoding"></a>Кодировка выражения

Выражения C#, которые были оценены как строка, кодируется в HTML. Выражения C#, которые были оценены как `IHtmlContent` подготавливаются к просмотру непосредственно через *IHtmlContent.WriteTo*. Выражения C#, которые не дают *IHtmlContent* преобразуются в строку (с *ToString*) и кодируются перед подготовкой к просмотру. Например, приведенная ниже разметка Razor:

```cshtml
@("<span>Hello World</span>")
```

Элементы HTML:

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

Который браузера отображается в виде:

`<span>Hello World</span>`

`HtmlHelper``Raw` выходные данные не закодированы, но к просмотру в виде HTML-разметка.

>[!WARNING]
> С помощью `HtmlHelper.Raw` на unsanitized пользователя ввод представляет угрозу безопасности. Ввод данных пользователем может содержать вредоносный JavaScript или использовать. Сложно очистки данных ввод данных пользователем, не используйте `HtmlHelper.Raw` на ввод данных пользователем.

Приведенная ниже разметка Razor:

```cshtml
@Html.Raw("<span>Hello World</span>")
```

Элементы HTML:

```html
<span>Hello World</span>
```

<a name=razor-code-blocks-label></a>

## <a name="razor-code-blocks"></a>Блоки кода Razor

Блоки кода Razor начинаться с `@` и заключены в `{}`. В отличие от выражения код внутри блоков кода C# не отображаются. Блоки кода и выражения в страницу Razor использующими ту же область и определены в порядке (то есть, объявления в блоке кода будет в области действия для блоков кода, более поздней версии, а также выражений).

```none
@{
    var output = "Hello World";
}

<p>The rendered result: @output</p>
```

Будет отображен:

```html
<p>The rendered result: Hello World</p>
```

<a name=implicit-transitions-label></a>

### <a name="implicit-transitions"></a>Неявные переходов

В блоке кода языка по умолчанию — C#, но вы можете переходить обратно в формат HTML. HTML в блоке кода перейдет обратно в визуализации HTML:

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

<a name=explicit-delimited-transition-label></a>

### <a name="explicit-delimited-transition"></a>Явные перехода с разделителями

Чтобы определить часть блока кода, должны обрабатывать HTML, заключите символов к просмотру с Razor `<text>` тег:

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

Такой подход обычно используется для отрисовки HTML-код, не заключенных в HTML-тега. Без тега HTML или Razor возникает ошибка времени выполнения Razor.

<a name=explicit-line-transition-with-label></a>

### <a name="explicit-line-transition-with-"></a>Явные строки перехода с`@:`

Для подготовки к просмотру остальная часть всю строку как HTML внутри блока кода, используйте `@:` синтаксис:

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

Без `@:` в приведенном выше коде получаемому Razor, ошибка во время выполнения.

<a name=control-structures-razor-label></a>

## <a name="control-structures"></a>Управляющие структуры

Управляющие структуры являются расширением блоков кода. Все аспекты блоков кода (плавный переход к разметки встроенный C#) также применяются к следующим структурам.

### <a name="conditionals-if-else-if-else-and-switch"></a>Условные выражения `@if`, `else if`, `else` и`@switch`

`@if` Семейство элементов управления при запуске кода:

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even</p>
}
```

`else`и `else if` не требуют `@` символа:

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even</p>
}
else if (value >= 1337)
{
    <p>The value is large.</p>
}
else
{
    <p>The value was not large and is odd.</p>
}
```

Можно использовать инструкцию switch следующим образом:

```cshtml
@switch (value)
{
    case 1:
        <p>The value is 1!</p>
        break;
    case 1337:
        <p>Your number is 1337!</p>
        break;
    default:
        <p>Your number was not 1 or 1337.</p>
        break;
}
```

### <a name="looping-for-foreach-while-and-do-while"></a>Циклы `@for`, `@foreach`, `@while`, и`@do while`

Может отображать шаблонного HTML с помощью элемента управления операторы цикла. Например, чтобы отобразить список пользователей:

```cshtml
@{
    var people = new Person[]
    {
          new Person("John", 33),
          new Person("Doe", 41),
    };
}
```

Можно использовать любой из следующих инструкций цикла:

`@for`

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@foreach`

```cshtml
@foreach (var person in people)
{
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@while`

```cshtml
@{ var i = 0; }
@while (i < people.Length)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
}
```

`@do while`

```cshtml
@{ var i = 0; }
@do
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
} while (i < people.Length);
```

### <a name="compound-using"></a>Составные`@using`

В C# using-оператор используется для обеспечения удаления объекта. В Razor такой же механизм может быть использован для создания вспомогательных методов HTML, которые содержат дополнительное содержимое. Например, можно использовать вспомогательные методы HTML для отрисовки тега формы с `@using` инструкции:

```cshtml
@using (Html.BeginForm())
{
    <div>
        email:
        <input type="email" id="Email" name="Email" value="" />
        <button type="submit"> Register </button>
    </div>
}
```

Можно также выполнять действия уровня области похожий на приведенный выше с [вспомогательных функций тегов](tag-helpers/index.md).

### <a name="try-catch-finally"></a>`@try`, `catch`, `finally`

Обработка исключений похожа на C#:

[!code-html[Main](razor/sample/Views/Home/Contact7.cshtml)]

### `@lock`

Razor имеет возможность защитить критические секции с помощью инструкций блокировки:

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a>Комментарии

Razor поддерживает комментарии C# и HTML. Следующую разметку:

```cshtml
@{
    /* C# comment. */
    // Another C# comment.
}
<!-- HTML comment -->
```

Отображается сервером как:

```cshtml
<!-- HTML comment -->
```

Перед отображением страницы комментариев Razor удаляются сервером. Использует Razor `@*  *@` в качестве разделителя комментариев. Следующий код закомментирована, поэтому сервер не будет содержать разметку:

```cshtml
@*
 @{
     /* C# comment. */
     // Another C# comment.
 }
 <!-- HTML comment -->
*@
```

<a name=razor-directives-label></a>

## <a name="directives"></a>Директивы

Неявные выражения с следующие зарезервированные ключевые слова представляются директивы Razor `@` символов. Директива обычно будет изменить способ разборе или включить различные функциональные возможности на странице Razor.

Основные сведения о том, как Razor создает код для представления будет упрощения понимания работы директивы. Страница Razor используется для создания файла C#. Например, эта страница Razor:

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

Создает класс следующего вида:

```csharp
public class _Views_Something_cshtml : RazorPage<dynamic>
{
    public override async Task ExecuteAsync()
    {
        var output = "Hello World";

        WriteLiteral("/r/n<div>Output: ");
        Write(output);
        WriteLiteral("</div>");
    }
}
```

[Просмотр класса Razor C#, созданного для представления](#razor-customcompilationservice-label) способы просмотра этим классом.

### `@using`

`@using` Директива добавит c# `using` на странице созданный razor директиву:

[!code-html[Main](razor/sample/Views/Home/Contact9.cshtml)]

### `@model`

`@model` Директива определяет тип модели, переданных в страницу Razor. Для этого используется следующий синтаксис:

```cshtml
@model TypeNameOfModel
```

Например, при создании приложения ASP.NET Core MVC для каждой учетной записи *Views/Account/Login.cshtml* представления Razor содержит следующее объявление модели:

```cshtml
@model LoginViewModel
```

В приведенном выше примере класс наследует класс, созданный `RazorPage<dynamic>`. Добавив `@model` контролировать то, что наследуется. Пример

```cshtml
@model LoginViewModel
```

Создает следующий класс

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

Предоставлять страниц Razor `Model` свойство для доступа к модели, переданных в страницу.

```cshtml
<div>The Login Email: @Model.Email</div>
```

`@model` Директива указана тип этого свойства (путем указания `T` в `RazorPage<T>` наследника созданного класса страницы). Если не указать `@model` директивы `Model` свойство будет иметь тип `dynamic`. Значение модели передается из контроллера в представление. В разделе [со строго типизированными моделей и @model ключевое слово](../../tutorials/first-mvc-app/adding-model.md#strongly-typed-models-keyword-label) для получения дополнительной информации.

### `@inherits`

`@inherits` Директива дает полный контроль над класс наследует Razor страницы:

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

Например предположим, мы должны были пользовательского страницы типа Razor:

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

Создает следующие Razor `<div>Custom text: Hello World</div>`.

[!code-html[Main](razor/sample/Views/Home/Contact10.cshtml)]

Нельзя использовать `@model` и `@inherits` на одной странице. Может иметь `@inherits` в *_ViewImports.cshtml* файл, который импортирует страниц Razor. Например, если представление Razor импортированы следующие *_ViewImports.cshtml* файла:

[!code-html[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

На следующей странице строго типизированные Razor

[!code-html[Main](razor/sample/Views/Home/Login1.cshtml)]

Создает этот HTML-разметку:

```cshtml
<div>The Login Email: Rick@contoso.com</div>
<div>Custom text: Hello World</div>
```

При передаче "[Rick@contoso.com](mailto:Rick@contoso.com)» в модели:

   Дополнительные сведения см. в статье о [макете](layout.md).

### `@inject`

`@inject` Директива позволяет внедрять службы из вашего [контейнер службы](../../fundamentals/dependency-injection.md) в Razor страницу для использования. В разделе [внедрение зависимостей в представления](dependency-injection.md).

<a name="functions"></a>

### `@functions`

`@functions` Директива позволяет добавлять функции уровня содержимого на страницу Razor. Синтаксис выглядит следующим образом.

```cshtml
@functions { // C# Code }
```

Пример:

[!code-html[Main](razor/sample/Views/Home/Contact6.cshtml)]

Создает следующую разметку HTML:

```cshtml
<div>From method: Hello</div>
```

Созданный Razor C# выглядит следующим образом.

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### `@section`

`@section` Директива используется в сочетании с [страница макета](layout.md) Включение представления для отображения содержимого в различных частях готовый для просмотра HTML-страницы. В разделе [разделы](layout.md#layout-sections-label) для получения дополнительной информации.

## <a name="tag-helpers"></a>Вспомогательных функций тегов

Следующие [вспомогательных функций тегов](tag-helpers/index.md) директивы подробно описаны в ссылкам.

* [@addTagHelper](tag-helpers/intro.md#add-helper-label)
* [@removeTagHelper](tag-helpers/intro.md#remove-razor-directives-label)
* [@tagHelperPrefix](tag-helpers/intro.md#prefix-razor-directives-label)

<a name=razor-reserved-keywords-label></a>

## <a name="razor-reserved-keywords"></a>Razor зарезервированные ключевые слова

### <a name="razor-keywords"></a>Ключевые слова Razor

* страница (требуется Core ASP.NET 2.0 и более поздние версии)
* функции
* наследует
* model
* section
* Вспомогательный (не поддерживается ASP.NET Core.)

Ключевые слова Razor предварять `@(Razor Keyword)`, например `@(functions)`. См. Полный пример ниже.

### <a name="c-razor-keywords"></a>Ключевые слова C# Razor

* case
* do
* default
* for
* foreach
* if
* else
* lock
* switch
* try
* catch
* finally
* использование
* while

Ключевые слова C# Razor должны быть двойные escape-последовательность с `@(@C# Razor Keyword)`, например `@(@case)`. Первый `@` экранирует анализатор Razor второй `@` экранирует анализатор C#. См. Полный пример ниже.

### <a name="reserved-keywords-not-used-by-razor"></a>Зарезервированные ключевые слова, не используется в Razor

* namespace
* класс

<a name=razor-customcompilationservice-label></a>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a>Просмотр класса Razor C#, созданного для представления

Добавьте следующий класс в проект ASP.NET MVC основных компонентов:

[!code-csharp[Main](razor/sample/Services/CustomCompilationService.cs)]

Переопределить `ICompilationService` добавленные MVC с выше класса;

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=29-33)]

Точка останова `Compile` метод `CustomCompilationService` и представление `compilationContent`.

![Представление визуализатор текста compilationContent](razor/_static/tvr.png)

<a name="case"></a>
## <a name="view-lookups-and-case-sensitivity"></a>Представление уточняющих запросов и чувствительности к регистру

Обработчик представлений Razor выполняет поиск с учетом регистра для представлений. Однако фактические уточняющего запроса определяется базового источника:

* На основе исходного файла: 

    * В операционных системах без учета регистра файловые системы (например, Windows) поиск поставщика физических файлов зависят от регистра. Например `return View("Test")` приведет к появлению `/Views/Home/Test.cshtml`, `/Views/home/test.cshtml` и всех других вариантов регистр обнаруживаются.
    * В системах файлов с учетом регистра, которая содержит Linux OSX и `EmbeddedFileProvider`, уточняющие запросы выполняются с учетом регистра. Например `return View("Test")` выглядит специально `/Views/Home/Test.cshtml`.
        
* Предварительно скомпилированный представления:

   * С помощью ASP.Net Core 2.0 и более поздних версиях поиска предкомпилированного представления без учета регистра для всех операционных систем. Поведение идентично поведение поставщика физического файла в Windows. 
   Примечание: Если два представления предкомпилированного отличаются только регистром, результатом поиска является недетерминированным.

Разработчики могут совпадает с регистром имена файлов и каталогов для имен области, контроллера и действия. Благодаря развертываний остаются независимыми от используемой файловой системы.
