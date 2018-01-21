---
title: "Справочник по синтаксису Razor для ASP.NET Core"
author: rick-anderson
description: "Дополнительные сведения о синтаксиса Razor разметки для внедрения серверного кода в веб-страниц."
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: d932e28246998c60e2b3f9c77a2521fe55991e85
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="razor-syntax-for-aspnet-core"></a>Синтаксис Razor ASP.NET Core

По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Latham Люк](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), и [Dan Vicarel](https://github.com/Rabadash8820)

Razor является синтаксис разметки для внедрения серверного кода в веб-страниц. Синтаксис Razor содержит Razor разметки, C# и HTML. Файлы, содержащие Razor, обычно имеют *.cshtml* расширение имени файла.

## <a name="rendering-html"></a>Подготовка к просмотру HTML

Язык по умолчанию Razor — HTML. Подготовки отчетов HTML из разметки Razor ничем не отличается от визуализации HTML из HTML-файл. HTML-разметку в *.cshtml* файлах Razor визуализируется с сервера без изменений.

## <a name="razor-syntax"></a>Синтаксис Razor

Razor поддерживает C# и использует `@` символ переход из HTML для C#. Razor вычисляет выражения C# и отображает их в выходных данных HTML.

Когда `@` следуют символ [Razor зарезервированное ключевое слово](#razor-reserved-keywords), он переходит в Razor разметку. В противном случае он переходит в обычный C#.

Чтобы escape `@` символ в разметке Razor, использовать второй `@` символа:

```cshtml
<p>@@Username</p>
```

Код отображается в формате HTML с помощью одного `@` символа:

```html
<p>@Username</p>
```

HTML-атрибутов и содержимого, содержащий адреса электронной почты не обрабатывают `@` символ как символ перехода. В следующем примере адреса электронной почты не изменятся путем синтаксического анализа Razor:

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a>Неявные выражения Razor

Неявные Razor выражения начинаются со `@` вместе с кодом C#:

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

За исключением C# `await` ключевое слово, неявные выражения не должно содержать пробелов. Если оператор C# есть очистить окончания, можно их объединения пространств:

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

Неявные выражения **не** содержать универсальные шаблоны C#, как символы в квадратных скобках (`<>`) интерпретируются как HTML-тега. Следующий код является **не** допустимым:

```cshtml
<p>@GenericMethod<int>()</p>
```

Предыдущий код вызывает ошибку компилятора, примерно следующего вида:

 * Элемент «int» не была закрыта. Все элементы должны быть либо самостоятельно закрыть или имеет соответствующего закрывающего тега.
 * Не удается преобразовать группу методов «GenericMethod», чтобы, не являющийся делегатом типа «объект». Предполагается ли вызывать этот метод? " 
 
Универсальный метод вызывает должны быть заключены в [явное выражение Razor](#explicit-razor-expressions) или [блок кода Razor](#razor-code-blocks).

## <a name="explicit-razor-expressions"></a>Прямые выражения Razor

Явные Razor выражения состоят из `@` символ со сбалансированной скобками. Для подготовки к просмотру время прошлой неделе, используется следующая разметка Razor:

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

Любое содержимое в `@()` скобки вычисляется и отображается в выходных данных.

Неявные выражения, описанные в предыдущем разделе, обычно не может содержать пробелы. В следующем коде одну неделю не вычитается из текущего времени:

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

Код отображает следующий код HTML:

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

Явные выражения могут использоваться для объединения текста с результатом выражения:

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

Без явного выражение `<p>Age@joe.Age</p>` рассматривается как адрес электронной почты и `<p>Age@joe.Age</p>` подготавливается к просмотру. Если они написаны как выражение явного `<p>Age33</p>` подготавливается к просмотру.


Прямые выражения можно использовать для отображения выходных данных из универсальных методов в *.cshtml* файлов. В выражении неявное символов в квадратных скобках (`<>`) интерпретируются как HTML-тега. Следующая разметка является **не** допустимым Razor:

```cshtml
<p>@GenericMethod<int>()</p>
```

Предыдущий код вызывает ошибку компилятора, примерно следующего вида:

 * Элемент «int» не была закрыта. Все элементы должны быть либо самостоятельно закрыть или имеет соответствующего закрывающего тега.
 * Не удается преобразовать группу методов «GenericMethod», чтобы, не являющийся делегатом типа «объект». Предполагается ли вызывать этот метод? " 
 
 В следующем примере показана запись правильно этот код. Код записывается как явное выражение:

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a>Кодировка выражения

Выражения C#, которые были оценены как строка, кодируется в HTML. Выражения C#, которые были оценены как `IHtmlContent` подготавливаются к просмотру непосредственно через `IHtmlContent.WriteTo`. Выражения C#, которые не дают `IHtmlContent` преобразуются в строки по `ToString` и закодированы, прежде чем они отображаются.

```cshtml
@("<span>Hello World</span>")
```

Код отображает следующий код HTML:

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

HTML-код отображается в обозревателе, как:

```
<span>Hello World</span>
```

`HtmlHelper.Raw`выходные данные не закодированы, однако к просмотру в виде HTML-разметка.

> [!WARNING]
> С помощью `HtmlHelper.Raw` на unsanitized пользователя ввод представляет угрозу безопасности. Ввод данных пользователем может содержать вредоносный JavaScript или использовать. Ввод данных пользователем очистки данных сложно. Избегайте использования `HtmlHelper.Raw` с вводимыми пользователем данными.

```cshtml
@Html.Raw("<span>Hello World</span>")
```

Код отображает следующий код HTML:

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a>Блоки кода Razor

Блоки кода Razor начинаться с `@` и заключены в `{}`. В отличие от выражения код внутри блоков кода C# не отображаются. Блоки кода и выражения в представлении использующими ту же область и определены в порядке:

```cshtml
@{
    var quote = "The future depends on what you do today. - Mahatma Gandhi";
}

<p>@quote</p>

@{
    quote = "Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.";
}

<p>@quote</p>
```

Код отображает следующий код HTML:

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a>Неявные переходов

В блоке кода языка по умолчанию — C#, но страница Razor может перейти обратно в HTML:

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a>Явные перехода с разделителями

Чтобы определить подраздел блок кода, должны обрабатывать HTML, окружить символов для подготовки к просмотру Razor  **\<текст >** тег:

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

Этот способ используется для отрисовки HTML-код, не заключенных в HTML-тега. Без тега HTML или Razor возникает ошибка времени выполнения Razor.

**\<Текст >** тег полезен для управления пробелов при подготовке к просмотру содержимого:

* Содержимое между  **\<текст >** визуализации тега. 
* Без пробелов до или после  **\<текст >** тег появляется в выходных данных HTML.

### <a name="explicit-line-transition-with-"></a>Явные строки перехода с @:

Для подготовки к просмотру остальная часть всю строку как HTML внутри блока кода, используйте `@:` синтаксис:

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

Без `@:` в коде, формируется ошибка выполнения Razor.

Предупреждение: Дополнительный `@` символы в файле Razor может привести к причина ошибки компилятора на инструкции ниже в блоке. Эти ошибки компилятора могут быть трудными для понимания, так как фактический ошибка возникает до указанной ошибки. Эта ошибка проявляется после объединения нескольких явного или неявного выражения в один блок кода.

## <a name="control-structures"></a>Управляющие структуры

Управляющие структуры являются расширением блоков кода. Все аспекты блоков кода (плавный переход к разметки встроенный C#) также применяются к следующим структурам.

### <a name="conditionals-if-else-if-else-and-switch"></a>Условные выражения @if, else if, else, и@switch

`@if`элементы управления, при выполнении кода.

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

`else`и `else if` не требуют `@` символа:

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
else if (value >= 1337)
{
    <p>The value is large.</p>
}
else
{
    <p>The value is odd and small.</p>
}
```

Приведенная ниже разметка показано, как использовать инструкцию switch:

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
        <p>Your number wasn't 1 or 1337.</p>
        break;
}
```

### <a name="looping-for-foreach-while-and-do-while"></a>Циклы @for, @foreach, @while, и @do во время

Шаблонные HTML может осуществляться с помощью элемента управления операторы цикла. Для подготовки к просмотру список лиц:

```cshtml
@{
    var people = new Person[]
    {
          new Person("Weston", 33),
          new Person("Johnathon", 41),
          ...
    };
}
```

Поддерживаются следующие операторы цикла:

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

### <a name="compound-using"></a>Составные@using

В C# `using` инструкция используется для обеспечения удаления объекта. В Razor тот же механизм используется для создания вспомогательных методов HTML, который содержит дополнительное содержимое. В следующем коде вспомогательных методов HTML отрисовки тега формы с `@using` инструкции:


```cshtml
@using (Html.BeginForm())
{
    <div>
        email:
        <input type="email" id="Email" value="">
        <button>Register</button>
    </div>
}
```

Действия на уровне области, которые могут быть выполнены с [вспомогательных функций тегов](xref:mvc/views/tag-helpers/intro).

### <a name="try-catch-finally"></a>@try, catch, finally

Обработка исключений похожа на C#:

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

Razor имеет возможность защитить критические секции с помощью инструкций блокировки:

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a>Комментарии

Razor поддерживает комментарии C# и HTML.

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

Код отображает следующий код HTML:

```html
<!-- HTML comment -->
```

Комментарии Razor удаляются сервером перед отображением веб-страницы. Использует Razor `@*  *@` в качестве разделителя комментариев. Следующий код закомментирована, поэтому сервер не создают разметку:

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a>Директивы

Неявные выражения с следующие зарезервированные ключевые слова представляются директивы Razor `@` символов. Директива обычно изменяет способ представления анализируется или включает различные функциональные возможности.

Основные сведения о том, как Razor создает код для представления помогает понять принципы работы директивы.

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

Код создает класс следующего вида:

```csharp
public class _Views_Something_cshtml : RazorPage<dynamic>
{
    public override async Task ExecuteAsync()
    {
        var output = "Getting old ain't for wimps! - Anonymous";

        WriteLiteral("/r/n<div>Quote of the Day: ");
        Write(output);
        WriteLiteral("</div>");
    }
}
```

Далее в этой статье разделе [Просмотр класс Razor C#, созданный для представления](#viewing-the-razor-c-class-generated-for-a-view) способы просмотра этим классом.

### <a name="using"></a>@using

`@using` Добавляет директивы C# `using` директиву созданного представления:

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

`@model` Директива определяет тип модели, переданный в представлении:

```cshtml
@model TypeNameOfModel
```

В приложении ASP.NET Core MVC, созданных с помощью отдельных учетных записей пользователей *Views/Account/Login.cshtml* представление содержит следующее объявление модели:

```cshtml
@model LoginViewModel
```

Созданный класс наследует `RazorPage<dynamic>`:

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

Предоставляет Razor `Model` свойство для доступа к модели передаются в представление:

```cshtml
<div>The Login Email: @Model.Email</div>
```

`@model` Директива определяет тип этого свойства. Указывает директиву `T` в `RazorPage<T>` , созданный класс, который представлении является производным от. Если `@model` директив не будет указано, `Model` свойство относится к типу `dynamic`. Значение модели передается из контроллера в представление. Дополнительные сведения см. в разделе [со строго типизированными моделей и @model ключевое слово.

### <a name="inherits"></a>@inherits

`@inherits` Директива предоставляет полный контроль над класс наследует представления:

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

Следующий код является пользовательским типом Razor страницы:

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

`CustomText` Отображается в представлении:

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

Код отображает следующий код HTML:

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 `@model`и `@inherits` может использоваться в одном представлении. `@inherits`может быть в *_ViewImports.cshtml* файл, который импортирует представления:

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

Следующий код является примером строго типизированное представление:

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

Если «rick@contoso.com» передается в модель, представление создает следующую разметку HTML:

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


`@inject` Директива включает страницы Razor, чтобы запустить службу из [контейнер службы](xref:fundamentals/dependency-injection) в представление. Дополнительные сведения см. в разделе [внедрение зависимостей в представления](xref:mvc/views/dependency-injection).

### <a name="functions"></a>@functions

`@functions` Директива включает страницы Razor для добавления содержимого на уровне функций в представлении:

```cshtml
@functions { // C# Code }
```

Пример:

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

Код формирует следующую разметку HTML:

```html
<div>From method: Hello</div>
```

Следующий код — это созданный класс Razor C#:

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

`@section` Директива используется в сочетании с [макета](xref:mvc/views/layout) Включение представления для отображения содержимого в различные части HTML-страницы. Дополнительные сведения см. в разделе [разделы](xref:mvc/views/layout#layout-sections-label).

## <a name="tag-helpers"></a>Вспомогательных функций тегов

Существует три директивы, которые относятся к [вспомогательных функций тегов](xref:mvc/views/tag-helpers/intro).

| Директива | Функция |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | Делает доступными для представления вспомогательных функций тегов. |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | Удаляет ранее добавленный из представления в виде вспомогательных функций тегов. |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | Указывает префикс тега для включения поддержка вспомогательных функций тегов и явно объявить использование вспомогательного тег. |

## <a name="razor-reserved-keywords"></a>Razor зарезервированные ключевые слова

### <a name="razor-keywords"></a>Ключевые слова Razor

* страница (требуется Core ASP.NET 2.0 и более поздние версии)
* функции
* наследует
* model
* section
* Вспомогательный (не поддерживаемых ASP.NET Core)

Ключевые слова Razor экранируются с `@(Razor Keyword)` (например, `@(functions)`).

### <a name="c-razor-keywords"></a>Ключевые слова C# Razor

* регистр знаков
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

Ключевые слова C# Razor необходимо экранировать двойные с `@(@C# Razor Keyword)` (например, `@(@case)`). Первый `@` экранирует анализатор Razor. Второй `@` экранирует анализатор C#.

### <a name="reserved-keywords-not-used-by-razor"></a>Зарезервированные ключевые слова, не используется в Razor

* namespace
* класс

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a>Просмотр класса Razor C#, созданного для представления

Добавьте следующий класс в проект ASP.NET MVC основных компонентов:

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

Переопределить `RazorTemplateEngine` добавленные MVC с `CustomTemplateEngine` класса:

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

Точка останова `return csharpDocument` инструкция `CustomTemplateEngine`. Когда выполнение программы остановится в точке останова, просматривать значения `generatedCode`.

![Представление визуализатор текста generatedCode](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a>Представление уточняющих запросов и чувствительности к регистру

Обработчик представлений Razor выполняет поиск с учетом регистра для представлений. Однако фактические уточняющего запроса определяется базовая файловая система:

* На основе исходного файла: 
  * В операционных системах без учета регистра файловые системы (например, Windows) поиск поставщика физических файлов зависят от регистра. Например `return View("Test")` приводит к соответствий для */Views/Home/Test.cshtml*, */Views/home/test.cshtml*и любой другой тип регистра.
  * В системах с учетом регистра файла (например, Linux, OSX и с `EmbeddedFileProvider`), уточняющие запросы выполняются с учетом регистра. Например `return View("Test")` специально совпадений */Views/Home/Test.cshtml*.
* Предварительно скомпилированные представлений: С основными ASP.NET 2.0 и более поздних, поиске предкомпилированного представления без учета регистра для всех операционных систем. Поведение идентично поведение поставщика физического файла в Windows. Если два представления предкомпилированного отличаются только регистром, результатом поиска является недетерминированным.

Разработчики могут совпадает с регистром имена файлов и каталогов на регистр:

    * Имена областей, контроллера и действия. 
    * Страниц Razor.
    
Регистра гарантирует, что развертываний найти их представления независимо от используемой файловой системы.
