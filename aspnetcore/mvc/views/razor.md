---
title: "Справочник по синтаксису Razor для ASP.NET Core"
author: rick-anderson
description: "Сведения о синтаксисе разметки Razor для внедрения в веб-страницы серверного кода."
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/razor
ms.openlocfilehash: 98021cc76555f0c1402764c845471a4730b01b20
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="razor-syntax-for-aspnet-core"></a>Синтаксис Razor для ASP.NET Core

Авторы: [Рик Андерсон (Rick Anderson)](https://twitter.com/RickAndMSFT), [Люк Лэтем (Luke Latham)](https://github.com/guardrex), [Тейлор Маллен (Taylor Mullen)](https://twitter.com/ntaylormullen) и [Дэн Викарел (Dan Vicarel)](https://github.com/Rabadash8820)

Razor — это синтаксис разметки для внедрения в веб-страницы серверного кода. Синтаксис Razor состоит из разметки Razor, C# и HTML. Файлы, содержащие Razor, обычно имеют расширение *CSHTML*.

## <a name="rendering-html"></a>Отрисовка HTML

Языком Razor по умолчанию является HTML. Отрисовка HTML из разметки Razor ничем не отличается от отрисовки из HTML-файла. Сервер отображает разметку HTML в *CSHTML*-файлах Razor без изменений.

## <a name="razor-syntax"></a>Синтаксис Razor

Razor поддерживает C# и использует символ `@` для перехода с HTML на C#. Razor вычисляет выражения C# и отображает их в выходных данных HTML.

Если за символом `@` следует [зарезервированное ключевое слово Razor](#razor-reserved-keywords), он переходит на разметку Razor. В противном случае он переходит на обычный C#.

В качестве escape-символа для `@` в разметке Razor используйте второй символ `@`:

```cshtml
<p>@@Username</p>
```

Код будет отображен в HTML с одним символом `@`:

```html
<p>@Username</p>
```

HTML-атрибуты и содержимое, включающие адреса электронной почты, не расценивают символ `@` как символ перехода. В следующем примере синтаксический анализ Razor не затрагивает адреса электронной почты:

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a>Неявные выражения Razor

Неявные выражения Razor начинаются с символа `@`, за которым следует код C#:

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

Неявные выражения не должны содержать пробелов. Исключением является ключевое слово C# `await`. Если оператор C# имеет четкое окончание, пробелы вставлять можно:

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

Неявные выражения **не могут** содержать универсальные шаблоны C#, так как символы в угловых скобках (`<>`) интерпретируются как тег HTML. Следующий код является **недопустимым**:

```cshtml
<p>@GenericMethod<int>()</p>
```

Приведенный выше код вызывает ошибку компилятора примерно следующего вида:

 * Элемент "int" не был закрыт. Все элементы должны быть самозакрывающимися или иметь соответствующий закрывающий тег.
 *  Не удается преобразовать группу методов "GenericMethod" в не являющийся делегатом тип "object". Предполагалось вызывать этот метод? 
 
Вызовы универсальных методов должны быть заключены в [явное выражение Razor](#explicit-razor-expressions) или [блок кода Razor](#razor-code-blocks).

## <a name="explicit-razor-expressions"></a>Явные выражения Razor

Явные выражения Razor состоят из символа `@` с соответствующими открывающими и закрывающими скобками. Для отображения времени прошлой недели используется следующая разметка Razor:

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

Любое содержимое в скобках `@()` вычисляется и отображается в выходных данных.

Неявные выражения, описанные в предыдущем разделе, обычно не содержат пробелов. В следующем коде из значения текущего времени неделя не вычитается:

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

Код отображает следующий HTML:

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

Явные выражения позволяют объединять результат своего выполнения с дополнительным текстом:

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

Без явного выражения `<p>Age@joe.Age</p>` обрабатывается как адрес электронной почты, и на выходе отображается `<p>Age@joe.Age</p>`. Если же текст написан как явное выражение, то вы получите `<p>Age33</p>`.


Вы можете использовать явные выражения для отображения выходных данных из универсальных методов в файлах *CSHTML*. В неявном выражении символы в угловых скобках (`<>`) интерпретируются как тег HTML. Следующая разметка является в Razor **недопустимой**:

```cshtml
<p>@GenericMethod<int>()</p>
```

Приведенный выше код вызывает ошибку компилятора примерно следующего вида:

 * Элемент "int" не был закрыт. Все элементы должны быть самозакрывающимися или иметь соответствующий закрывающий тег.
 *  Не удается преобразовать группу методов "GenericMethod" в не являющийся делегатом тип "object". Предполагалось вызывать этот метод? 
 
 Корректный способ написания этого кода показан в разметке ниже. Код записывается как явное выражение:

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a>Кодирование выражений

Выражения C#, которые имеют строковое выходное значение, кодируются в формате HTML. Выражения C#, которые имеют значение `IHtmlContent`, обрабатываются непосредственно с помощью `IHtmlContent.WriteTo`. Выражения C#, которые не имеют выходное значение `IHtmlContent`, преобразуются в строку с помощью `ToString` и кодируются перед обработкой.

```cshtml
@("<span>Hello World</span>")
```

Код отображает следующий HTML:

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

HTML отображается в обозревателе следующим образом:

```
<span>Hello World</span>
```

Выходные данные `HtmlHelper.Raw` не кодируются, но отображаются в виде разметки HTML.

> [!WARNING]
> Использование `HtmlHelper.Raw` с непроверенными входными данными пользователя представляет угрозу безопасности. Эти входные данные могут содержать вредоносный код JavaScript или другие эксплойты. Очистка вводимых пользователем данных является сложной задачей. Старайтесь не использовать `HtmlHelper.Raw` с такими данными.

```cshtml
@Html.Raw("<span>Hello World</span>")
```

Код отображает следующий HTML:

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a>Блоки кода Razor

Блоки кода Razor начинаются с символа `@` и заключены в фигурные скобки `{}`. В отличие от выражений код C# внутри блоков кода не обрабатывается. Блоки кода и выражения в представлении используют общую область и определяются в следующем порядке:

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

Код отображает следующий HTML:

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a>Неявные переходы

В блоке кода языком по умолчанию является C#, но страница Razor Page может вернуться на HTML:

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a>Явный переход с разделителями

Чтобы определить подраздел блока кода, который должен отрисовывать HTML, окружите подлежащие отображению символы тегами Razor **\<text>**:

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

Используйте этот способ для отрисовки HTML, не заключенного в HTML-теги. Без тега HTML или Razor возникает ошибка времени выполнения Razor.

Тег **\<text>** хорошо подходит для контроля пробелов при отрисовке содержимого:

* Отрисовывается только содержимое между тегами **\<text>**. 
* В выходных данных HTML пробелы до или после тега **\<text>** не отображаются.

### <a name="explicit-line-transition-with-"></a>Явные переходы по строкам с @:

Для отрисовки оставшейся части строки в виде HTML внутри блока кода используйте синтаксис "`@:`":

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

Если в коде отсутствует `@:`, возникает ошибка среды выполнения Razor.

Предупреждение. Дополнительные символы `@` в файле Razor могут вызвать ошибки компилятора в последующих операторах блока. Эти ошибки компилятора может быть трудно проанализировать, так как ошибка фактически возникает раньше, чем указано. Чаще всего эта ошибка появляется после объединения множества неявных или явных выражений в один блок кода.

## <a name="control-structures"></a>Управляющие структуры

Управляющие структуры являются расширением блоков кода. Все аспекты блоков кода (переход на разметку, встроенный код C#) также относятся к следующим структурам.

### <a name="conditionals-if-else-if-else-and-switch"></a>Условные выражения @if, else if, else и @switch

`@if` контролирует, когда нужно запускать код:

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

Для `else` и `else if` символ `@` не требуется:

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

В следующей разметке показано использование оператора switch:

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

### <a name="looping-for-foreach-while-and-do-while"></a>Циклы @for, @foreach, @while и @do while

Операторы выполнения цикла позволяют выполнять отрисовку шаблонного HTML. Отрисовка списка людей:

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

Поддерживаются следующие операторы выполнения цикла:

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

### <a name="compound-using"></a>Составной оператор @using

В C# оператор `using` позволяет обеспечить использование какого-то объекта. В Razor этот механизм позволяет создавать вспомогательные функции HTML, включающие дополнительное содержимое. В следующем коде вспомогательные функции HTML используют оператор `@using` для создания тега формы:


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

Для действий на уровне области можно использовать [вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro).

### <a name="try-catch-finally"></a>@try, catch, finally

Обработка исключений выполняется так же, как в C#:

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

Razor позволяет защищать важные разделы при помощи операторов блокировки:

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a>Комментарии

Razor поддерживает комментарии C# и HTML:

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

Код отображает следующий HTML:

```html
<!-- HTML comment -->
```

Сервер удаляет комментарии Razor перед отображением веб-страницы. Для разделения комментариев Razor использует `@*  *@`. Следующий код закомментирован, поэтому сервер не отрисовывает разметку:

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

Директивы Razor представлены неявными выражениями с зарезервированными ключевыми словами после символа `@`. Как правило, директива изменяет способ анализа представления или открывает доступ к дополнительным функциям.

Узнав, каким образом Razor создает код для представления, вы сможете легко понять принципы работы директив.

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

Код создает класс, аналогичный следующему:

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

Сведения о просмотре этого класса приводятся в разделе [Просмотр Razor-класса C#, созданного для представления](#viewing-the-razor-c-class-generated-for-a-view) далее в этой статье.

### <a name="using"></a>@using

Директива `@using` добавляет директиву C# `using` в созданное представление:

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

Директива `@model` определяет тип модели, передаваемой в представление:

```cshtml
@model TypeNameOfModel
```

В MVC-приложении ASP.NET Core, созданном с отдельными учетными записями пользователей, представление *Views/Account/Login.cshtml* содержит следующее объявление модели:

```cshtml
@model LoginViewModel
```

Созданный класс наследует от `RazorPage<dynamic>`:

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

Для доступа к модели, переданной в представление, Razor предоставляет свойство `Model`:

```cshtml
<div>The Login Email: @Model.Email</div>
```

Директива `@model` задает тип этого свойства. Директива указывает `T` в `RazorPage<T>` — созданном классе, на основе которого создается производное представление. Если директива `@model` не указана, свойство `Model` имеет тип `dynamic`. Значение модели передается из контроллера в представление. Дополнительные сведения см. в разделах, посвященных строго типизированным моделям и ключевому слову @model.

### <a name="inherits"></a>@inherits

Директива `@inherits` позволяет полностью управлять классом, которому наследует представление:

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

Следующий код показывает настраиваемый тип страницы Razor:

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

В представлении отображается `CustomText`:

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

Код отображает следующий HTML:

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 `@model` и `@inherits` могут использоваться в одном представлении. `@inherits` может находиться в файле *_ViewImports.cshtml*, который импортируется представлением:

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

Следующий код показывает пример строго типизированного представления:

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

Если передать в модель "rick@contoso.com", представление создает следующую разметку HTML:

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


Директива `@inject` позволяет странице Razor внедрять в представление службу из [контейнера службы](xref:fundamentals/dependency-injection). Дополнительные сведения: [Внедрение зависимостей в представления](xref:mvc/views/dependency-injection).

### <a name="functions"></a>@functions

Директива `@functions` позволяет странице Razor добавлять в представление содержимое на уровне функций:

```cshtml
@functions { // C# Code }
```

Пример:

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

Код создает следующую разметку HTML:

```html
<div>From method: Hello</div>
```

Следующий код показывает созданный Razor-класс C#:

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

Директива `@section` используется в сочетании с [макетом](xref:mvc/views/layout) и позволяет представлениям отображать содержимое в различных частях HTML-страницы. Дополнительные сведения: [Разделы](xref:mvc/views/layout#layout-sections-label).

## <a name="tag-helpers"></a>Вспомогательные функции тегов

Существует три директивы, которые относятся к [вспомогательным функциям тегов](xref:mvc/views/tag-helpers/intro).

| Директива | Функция |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | Делает вспомогательные функции тегов доступными в представлении. |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | Удаляет из представления вспомогательные функции тегов, добавленные ранее. |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | Задает префикс тега, который активирует поддержку вспомогательной функции тега и ее использования в явном виде. |

## <a name="razor-reserved-keywords"></a>Зарезервированные ключевые слова Razor

### <a name="razor-keywords"></a>Ключевые слова Razor

* page (требуется ASP.NET Core 2.0 или более поздние версии)
* функции
* наследует
* model
* section
* helper (сейчас не поддерживается в ASP.NET Core)

В качестве escape-символа для ключевых слов Razor используется `@(Razor Keyword)` (например, `@(functions)`).

### <a name="c-razor-keywords"></a>Ключевые слова C# в Razor

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

Для ключевых слов C# в Razor требуется двойной escape-символ: `@(@C# Razor Keyword)` (например, `@(@case)`). Первый `@` предназначен для обхода синтаксического анализа Razor, а второй `@` — для обхода C#.

### <a name="reserved-keywords-not-used-by-razor"></a>Зарезервированные ключевые слова, не используемые в Razor

* namespace
* класс

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a>Просмотр Razor-класса C#, созданного для представления

Добавьте в MVC-проект ASP.NET следующий класс:

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

Переопределите класс `RazorTemplateEngine`, добавленный MVC, классом `CustomTemplateEngine`:

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

Установите точку останова в операторе `return csharpDocument` класса `CustomTemplateEngine`. Когда выполнение программы остановится в этой точке, просмотрите значение `generatedCode`.

![Представление generatedCode в визуализаторе текста](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a>Поиск данных в представлениях и учет регистра

Модуль представлений Razor позволяет искать в представлениях данные с учетом регистра. Однако фактический поиск зависит от используемой файловой системы.

* Источники в виде файлов: 
  * В операционных системах, файловые системы которых не учитывают регистр (например, Windows), поиск поставщика физических файлов не зависит от регистра. Например, поиск по `return View("Test")` выводит совпадения */Views/Home/Test.cshtml*, */Views/home/test.cshtml* и другие варианты с различными сочетаниями регистра.
  * В файловых системах, учитывающих регистр (например, в Linux, OSX и где используется `EmbeddedFileProvider`), поиск выполняется с учетом регистра. Например, поиск по `return View("Test")` дает точное совпадение */Views/Home/Test.cshtml*.
* Предварительно скомпилированные представления: в ASP.NET Core 2.0 и более поздних версиях поиск в предварительно скомпилированных представлениях выполняется без учета регистра во всех операционных системах. Это поведение аналогично поведению поставщика физических файлов в Windows. Если два предварительно скомпилированных представления отличаются только регистром, результат поиска является недетерминированным.

Разработчикам рекомендуется использовать для файлов и каталогов тот же регистр, что и для:

    * имен областей, контроллеров и действий; 
    * страниц Razor Pages.
    
Совпадающий регистр гарантирует, что развертываемые службы смогут находить свои представления вне зависимости от используемой файловой системы.
