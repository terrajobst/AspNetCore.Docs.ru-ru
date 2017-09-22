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
ms.openlocfilehash: 066fe3b2486c63bd4de2ccb865ad432a67846d77
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/22/2017
---
# <a name="razor-syntax-for-aspnet-core"></a><span data-ttu-id="fe2da-104">Синтаксис Razor ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fe2da-104">Razor syntax for ASP.NET Core</span></span>

<span data-ttu-id="fe2da-105">По [Taylor Mullen](https://twitter.com/ntaylormullen) и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fe2da-105">By [Taylor Mullen](https://twitter.com/ntaylormullen) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-is-razor"></a><span data-ttu-id="fe2da-106">Что такое Razor</span><span class="sxs-lookup"><span data-stu-id="fe2da-106">What is Razor?</span></span>

<span data-ttu-id="fe2da-107">Razor является синтаксис разметки для внедрения серверного кода в веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="fe2da-107">Razor is a markup syntax for embedding server based code into web pages.</span></span> <span data-ttu-id="fe2da-108">Синтаксис Razor состоит из разметки Razor, C# и HTML.</span><span class="sxs-lookup"><span data-stu-id="fe2da-108">The Razor syntax consists of Razor markup, C# and HTML.</span></span> <span data-ttu-id="fe2da-109">Файлы, содержащие Razor, обычно имеют *.cshtml* расширение имени файла.</span><span class="sxs-lookup"><span data-stu-id="fe2da-109">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="fe2da-110">Подготовка к просмотру HTML</span><span class="sxs-lookup"><span data-stu-id="fe2da-110">Rendering HTML</span></span>

<span data-ttu-id="fe2da-111">Язык по умолчанию Razor — HTML.</span><span class="sxs-lookup"><span data-stu-id="fe2da-111">The default Razor language is HTML.</span></span> <span data-ttu-id="fe2da-112">Подготовка к просмотру HTML из Razor не отличается от HTML-файла.</span><span class="sxs-lookup"><span data-stu-id="fe2da-112">Rendering HTML from Razor is no different than in an HTML file.</span></span> <span data-ttu-id="fe2da-113">Файл Razor с следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="fe2da-113">A Razor file with the following markup:</span></span>

```html
<p>Hello World</p>
   ```

<span data-ttu-id="fe2da-114">Подготавливается к просмотру без изменений `<p>Hello World</p>` сервером.</span><span class="sxs-lookup"><span data-stu-id="fe2da-114">Is rendered unchanged as `<p>Hello World</p>` by the server.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="fe2da-115">Синтаксис Razor</span><span class="sxs-lookup"><span data-stu-id="fe2da-115">Razor syntax</span></span>

<span data-ttu-id="fe2da-116">Razor поддерживает C# и использует `@` символ переход из HTML для C#.</span><span class="sxs-lookup"><span data-stu-id="fe2da-116">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="fe2da-117">Razor вычисляет выражения C# и отображает их в выходных данных HTML.</span><span class="sxs-lookup"><span data-stu-id="fe2da-117">Razor evaluates C# expressions and renders them in the HTML output.</span></span> <span data-ttu-id="fe2da-118">Razor может выполнять переход с HTML на C# или на разметку Razor.</span><span class="sxs-lookup"><span data-stu-id="fe2da-118">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="fe2da-119">Когда `@` следуют символ [Razor зарезервированное ключевое слово](#razor-reserved-keywords) он переходит в разметку Razor, в противном случае он переходит в обычный C#.</span><span class="sxs-lookup"><span data-stu-id="fe2da-119">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords) it transitions into Razor-specific markup, otherwise it transitions into plain C#.</span></span>

<a name=escape-at-label></a>

<span data-ttu-id="fe2da-120">Содержащий HTML `@` символы необходимо экранировать со вторым `@` символов.</span><span class="sxs-lookup"><span data-stu-id="fe2da-120">HTML containing `@` symbols may need to be escaped with a second `@` symbol.</span></span> <span data-ttu-id="fe2da-121">Пример:</span><span class="sxs-lookup"><span data-stu-id="fe2da-121">For example:</span></span>

```html
<p>@@Username</p>
   ```

<span data-ttu-id="fe2da-122">будет отображен следующий код HTML:</span><span class="sxs-lookup"><span data-stu-id="fe2da-122">would render the following HTML:</span></span>

```html
<p>@Username</p>
   ```

<a name=razor-email-ref></a>

<span data-ttu-id="fe2da-123">HTML-атрибутов и содержимого, содержащий адреса электронной почты не обрабатывают `@` символ как символ перехода.</span><span class="sxs-lookup"><span data-stu-id="fe2da-123">HTML attributes and content containing email addresses don’t treat the `@` symbol as a transition character.</span></span>

   `<a href="mailto:Support@contoso.com">Support@contoso.com</a>`

## <a name="implicit-razor-expressions"></a><span data-ttu-id="fe2da-124">Неявные выражения Razor</span><span class="sxs-lookup"><span data-stu-id="fe2da-124">Implicit Razor expressions</span></span>

<span data-ttu-id="fe2da-125">Неявные Razor выражения начинаются со `@` вместе с кодом C#.</span><span class="sxs-lookup"><span data-stu-id="fe2da-125">Implicit Razor expressions start with `@` followed by C# code.</span></span> <span data-ttu-id="fe2da-126">Пример:</span><span class="sxs-lookup"><span data-stu-id="fe2da-126">For example:</span></span>

```html
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="fe2da-127">За исключением C# `await` неявные выражения ключевое слово не должно содержать пробелов.</span><span class="sxs-lookup"><span data-stu-id="fe2da-127">With the exception of the C# `await` keyword implicit expressions must not contain spaces.</span></span> <span data-ttu-id="fe2da-128">Например можно intermingle пробелы при условии, что оператор C# имеет очистить окончания:</span><span class="sxs-lookup"><span data-stu-id="fe2da-128">For example, you can intermingle spaces as long as the C# statement has a clear ending:</span></span>

```html
<p>@await DoSomething("hello", "world")</p>
```

## <a name="explicit-razor-expressions"></a><span data-ttu-id="fe2da-129">Прямые выражения Razor</span><span class="sxs-lookup"><span data-stu-id="fe2da-129">Explicit Razor expressions</span></span>

<span data-ttu-id="fe2da-130">Прямые Razor выражения состоит из символа с балансировкой скобки @.</span><span class="sxs-lookup"><span data-stu-id="fe2da-130">Explicit Razor expressions consists of an @ symbol with balanced parenthesis.</span></span> <span data-ttu-id="fe2da-131">Например, для подготовки к просмотру время прошлой недели:</span><span class="sxs-lookup"><span data-stu-id="fe2da-131">For example, to render last week's time:</span></span>

```html
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="fe2da-132">Любое содержимое в @ круглая скобка () вычисляется и отображается в выходных данных.</span><span class="sxs-lookup"><span data-stu-id="fe2da-132">Any content within the @() parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="fe2da-133">Неявные выражения обычно не могут содержать пробелы.</span><span class="sxs-lookup"><span data-stu-id="fe2da-133">Implicit expressions generally cannot contain spaces.</span></span> <span data-ttu-id="fe2da-134">Например в следующем коде одну неделю не вычитается из текущего времени:</span><span class="sxs-lookup"><span data-stu-id="fe2da-134">For example, in the code below, one week is not subtracted from the current time:</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact.cshtml?range=20)]

<span data-ttu-id="fe2da-135">Который отображает следующий код HTML:</span><span class="sxs-lookup"><span data-stu-id="fe2da-135">Which renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
   ```

<span data-ttu-id="fe2da-136">Явное выражение можно использовать для объединения текста с результатом выражения:</span><span class="sxs-lookup"><span data-stu-id="fe2da-136">You can use an explicit expression to concatenate text with an expression result:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [5]}} -->

```none
@{
    var joe = new Person("Joe", 33);
 }

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="fe2da-137">Без явного выражение `<p>Age@joe.Age</p>` будет рассматриваться как адрес электронной почты и `<p>Age@joe.Age</p>` будет проходить.</span><span class="sxs-lookup"><span data-stu-id="fe2da-137">Without the explicit expression, `<p>Age@joe.Age</p>` would be treated as an email address and `<p>Age@joe.Age</p>` would be rendered.</span></span> <span data-ttu-id="fe2da-138">Если они написаны как выражение явного `<p>Age33</p>` подготавливается к просмотру.</span><span class="sxs-lookup"><span data-stu-id="fe2da-138">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>

<a name=expression-encoding-label></a>

## <a name="expression-encoding"></a><span data-ttu-id="fe2da-139">Кодировка выражения</span><span class="sxs-lookup"><span data-stu-id="fe2da-139">Expression encoding</span></span>

<span data-ttu-id="fe2da-140">Выражения C#, которые были оценены как строка, кодируется в HTML.</span><span class="sxs-lookup"><span data-stu-id="fe2da-140">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="fe2da-141">Выражения C#, которые были оценены как `IHtmlContent` подготавливаются к просмотру непосредственно через *IHtmlContent.WriteTo*.</span><span class="sxs-lookup"><span data-stu-id="fe2da-141">C# expressions that evaluate to `IHtmlContent` are rendered directly through *IHtmlContent.WriteTo*.</span></span> <span data-ttu-id="fe2da-142">Выражения C#, которые не дают *IHtmlContent* преобразуются в строку (с *ToString*) и кодируются перед подготовкой к просмотру.</span><span class="sxs-lookup"><span data-stu-id="fe2da-142">C# expressions that don't evaluate to *IHtmlContent* are converted to a string (by *ToString*) and encoded before they are rendered.</span></span> <span data-ttu-id="fe2da-143">Например, приведенная ниже разметка Razor:</span><span class="sxs-lookup"><span data-stu-id="fe2da-143">For example, the following Razor markup:</span></span>

```html
@("<span>Hello World</span>")
   ```

<span data-ttu-id="fe2da-144">Элементы HTML:</span><span class="sxs-lookup"><span data-stu-id="fe2da-144">Renders this HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
   ```

<span data-ttu-id="fe2da-145">Который браузера отображается в виде:</span><span class="sxs-lookup"><span data-stu-id="fe2da-145">Which the browser renders as:</span></span>

`<span>Hello World</span>`

<span data-ttu-id="fe2da-146">`HtmlHelper``Raw` выходные данные не закодированы, но к просмотру в виде HTML-разметка.</span><span class="sxs-lookup"><span data-stu-id="fe2da-146">`HtmlHelper` `Raw` output is not encoded but rendered as HTML markup.</span></span>

>[!WARNING]
> <span data-ttu-id="fe2da-147">С помощью `HtmlHelper.Raw` на unsanitized пользователя ввод представляет угрозу безопасности.</span><span class="sxs-lookup"><span data-stu-id="fe2da-147">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="fe2da-148">Ввод данных пользователем может содержать вредоносный JavaScript или использовать.</span><span class="sxs-lookup"><span data-stu-id="fe2da-148">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="fe2da-149">Сложно очистки данных ввод данных пользователем, не используйте `HtmlHelper.Raw` на ввод данных пользователем.</span><span class="sxs-lookup"><span data-stu-id="fe2da-149">Sanitizing user input is difficult, avoid using `HtmlHelper.Raw` on user input.</span></span>

<span data-ttu-id="fe2da-150">Приведенная ниже разметка Razor:</span><span class="sxs-lookup"><span data-stu-id="fe2da-150">The following Razor markup:</span></span>

```html
@Html.Raw("<span>Hello World</span>")
   ```

<span data-ttu-id="fe2da-151">Элементы HTML:</span><span class="sxs-lookup"><span data-stu-id="fe2da-151">Renders this HTML:</span></span>

```html
<span>Hello World</span>
   ```

<a name=razor-code-blocks-label></a>

## <a name="razor-code-blocks"></a><span data-ttu-id="fe2da-152">Блоки кода Razor</span><span class="sxs-lookup"><span data-stu-id="fe2da-152">Razor code blocks</span></span>

<span data-ttu-id="fe2da-153">Блоки кода Razor начинаться с `@` и заключены в `{}`.</span><span class="sxs-lookup"><span data-stu-id="fe2da-153">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="fe2da-154">В отличие от выражения код внутри блоков кода C# не отображаются.</span><span class="sxs-lookup"><span data-stu-id="fe2da-154">Unlike expressions, C# code inside code blocks is not rendered.</span></span> <span data-ttu-id="fe2da-155">Блоки кода и выражения в страницу Razor использующими ту же область и определены в порядке (то есть, объявления в блоке кода будет в области действия для блоков кода, более поздней версии, а также выражений).</span><span class="sxs-lookup"><span data-stu-id="fe2da-155">Code blocks and expressions in a Razor page share the same scope and are defined in order (that is, declarations in a code block will be in scope for later code blocks and expressions).</span></span>

```none
@{
    var output = "Hello World";
}

<p>The rendered result: @output</p>
```

<span data-ttu-id="fe2da-156">Будет отображен:</span><span class="sxs-lookup"><span data-stu-id="fe2da-156">Would render:</span></span>

```html
<p>The rendered result: Hello World</p>
   ```

<a name=implicit-transitions-label></a>

### <a name="implicit-transitions"></a><span data-ttu-id="fe2da-157">Неявные переходов</span><span class="sxs-lookup"><span data-stu-id="fe2da-157">Implicit transitions</span></span>

<span data-ttu-id="fe2da-158">В блоке кода языка по умолчанию — C#, но вы можете переходить обратно в формат HTML.</span><span class="sxs-lookup"><span data-stu-id="fe2da-158">The default language in a code block is C#, but you can transition back to HTML.</span></span> <span data-ttu-id="fe2da-159">HTML в блоке кода перейдет обратно в визуализации HTML:</span><span class="sxs-lookup"><span data-stu-id="fe2da-159">HTML within a code block will transition back into rendering HTML:</span></span>

```none
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

<a name=explicit-delimited-transition-label></a>

### <a name="explicit-delimited-transition"></a><span data-ttu-id="fe2da-160">Явные перехода с разделителями</span><span class="sxs-lookup"><span data-stu-id="fe2da-160">Explicit delimited transition</span></span>

<span data-ttu-id="fe2da-161">Чтобы определить часть блока кода, должны обрабатывать HTML, заключите символов к просмотру с Razor `<text>` тег:</span><span class="sxs-lookup"><span data-stu-id="fe2da-161">To define a sub-section of a code block that should render HTML, surround the characters to be rendered with the Razor `<text>` tag:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [4]}} -->

```none
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="fe2da-162">Такой подход обычно используется для отрисовки HTML-код, не заключенных в HTML-тега.</span><span class="sxs-lookup"><span data-stu-id="fe2da-162">You generally use this approach when you want to render HTML that is not surrounded by an HTML tag.</span></span> <span data-ttu-id="fe2da-163">Без тега HTML или Razor возникает ошибка времени выполнения Razor.</span><span class="sxs-lookup"><span data-stu-id="fe2da-163">Without an HTML or Razor tag, you get a Razor runtime error.</span></span>

<a name=explicit-line-transition-with-label></a>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="fe2da-164">Явные строки перехода с`@:`</span><span class="sxs-lookup"><span data-stu-id="fe2da-164">Explicit Line Transition with `@:`</span></span>

<span data-ttu-id="fe2da-165">Для подготовки к просмотру остальная часть всю строку как HTML внутри блока кода, используйте `@:` синтаксис:</span><span class="sxs-lookup"><span data-stu-id="fe2da-165">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [4]}} -->

```none
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="fe2da-166">Без `@:` в приведенном выше коде получаемому Razor, ошибка во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="fe2da-166">Without the `@:` in the code above, you'd get a Razor run time error.</span></span>

<a name=control-structures-razor-label></a>

## <a name="control-structures"></a><span data-ttu-id="fe2da-167">Управляющие структуры</span><span class="sxs-lookup"><span data-stu-id="fe2da-167">Control Structures</span></span>

<span data-ttu-id="fe2da-168">Управляющие структуры являются расширением блоков кода.</span><span class="sxs-lookup"><span data-stu-id="fe2da-168">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="fe2da-169">Все аспекты блоков кода (плавный переход к разметки встроенный C#) также применяются к следующим структурам.</span><span class="sxs-lookup"><span data-stu-id="fe2da-169">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures.</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="fe2da-170">Условные выражения `@if`, `else if`, `else` и`@switch`</span><span class="sxs-lookup"><span data-stu-id="fe2da-170">Conditionals `@if`, `else if`, `else` and `@switch`</span></span>

<span data-ttu-id="fe2da-171">`@if` Семейство элементов управления при запуске кода:</span><span class="sxs-lookup"><span data-stu-id="fe2da-171">The `@if` family controls when code runs:</span></span>

```none
@if (value % 2 == 0)
{
    <p>The value was even</p>
}
```

<span data-ttu-id="fe2da-172">`else`и `else if` не требуют `@` символа:</span><span class="sxs-lookup"><span data-stu-id="fe2da-172">`else` and `else if` don't require the `@` symbol:</span></span>

```none
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

<span data-ttu-id="fe2da-173">Можно использовать инструкцию switch следующим образом:</span><span class="sxs-lookup"><span data-stu-id="fe2da-173">You can use a switch statement like this:</span></span>

```none
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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="fe2da-174">Циклы `@for`, `@foreach`, `@while`, и`@do while`</span><span class="sxs-lookup"><span data-stu-id="fe2da-174">Looping `@for`, `@foreach`, `@while`, and `@do while`</span></span>

<span data-ttu-id="fe2da-175">Может отображать шаблонного HTML с помощью элемента управления операторы цикла.</span><span class="sxs-lookup"><span data-stu-id="fe2da-175">You can render templated HTML with looping control statements.</span></span> <span data-ttu-id="fe2da-176">Например, чтобы отобразить список пользователей:</span><span class="sxs-lookup"><span data-stu-id="fe2da-176">For example, to render a list of people:</span></span>

```none
@{
    var people = new Person[]
    {
          new Person("John", 33),
          new Person("Doe", 41),
    };
}
```

<span data-ttu-id="fe2da-177">Можно использовать любой из следующих инструкций цикла:</span><span class="sxs-lookup"><span data-stu-id="fe2da-177">You can use any of the following looping statements:</span></span>

`@for`

```none
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@foreach`

```none
@foreach (var person in people)
{
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@while`

```none
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

```none
@{ var i = 0; }
@do
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
} while (i < people.Length);
```

### <a name="compound-using"></a><span data-ttu-id="fe2da-178">Составные`@using`</span><span class="sxs-lookup"><span data-stu-id="fe2da-178">Compound `@using`</span></span>

<span data-ttu-id="fe2da-179">В C# using-оператор используется для обеспечения удаления объекта.</span><span class="sxs-lookup"><span data-stu-id="fe2da-179">In C# a using statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="fe2da-180">В Razor такой же механизм может быть использован для создания вспомогательных методов HTML, которые содержат дополнительное содержимое.</span><span class="sxs-lookup"><span data-stu-id="fe2da-180">In Razor this same mechanism can be used to create HTML helpers that contain additional content.</span></span> <span data-ttu-id="fe2da-181">Например, можно использовать вспомогательные методы HTML для отрисовки тега формы с `@using` инструкции:</span><span class="sxs-lookup"><span data-stu-id="fe2da-181">For instance, we can utilize HTML Helpers to render a form tag with the `@using` statement:</span></span>

```none
@using (Html.BeginForm())
{
    <div>
        email:
        <input type="email" id="Email" name="Email" value="" />
        <button type="submit"> Register </button>
    </div>
}
```

<span data-ttu-id="fe2da-182">Можно также выполнять действия уровня области похожий на приведенный выше с [вспомогательных функций тегов](tag-helpers/index.md).</span><span class="sxs-lookup"><span data-stu-id="fe2da-182">You can also perform scope level actions like the above with [Tag Helpers](tag-helpers/index.md).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="fe2da-183">`@try`, `catch`, `finally`</span><span class="sxs-lookup"><span data-stu-id="fe2da-183">`@try`, `catch`, `finally`</span></span>

<span data-ttu-id="fe2da-184">Обработка исключений похожа на C#:</span><span class="sxs-lookup"><span data-stu-id="fe2da-184">Exception handling is similar to  C#:</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact7.cshtml)]

### `@lock`

<span data-ttu-id="fe2da-185">Razor имеет возможность защитить критические секции с помощью инструкций блокировки:</span><span class="sxs-lookup"><span data-stu-id="fe2da-185">Razor has the capability to protect critical sections with lock statements:</span></span>

```none
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="fe2da-186">Комментарии</span><span class="sxs-lookup"><span data-stu-id="fe2da-186">Comments</span></span>

<span data-ttu-id="fe2da-187">Razor поддерживает комментарии C# и HTML.</span><span class="sxs-lookup"><span data-stu-id="fe2da-187">Razor supports C# and HTML comments.</span></span> <span data-ttu-id="fe2da-188">Следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="fe2da-188">The following markup:</span></span>

```none
@{
    /* C# comment. */
    // Another C# comment.
}
<!-- HTML comment -->
```

<span data-ttu-id="fe2da-189">Отображается сервером как:</span><span class="sxs-lookup"><span data-stu-id="fe2da-189">Is rendered by the server as:</span></span>

```none
<!-- HTML comment -->
```

<span data-ttu-id="fe2da-190">Перед отображением страницы комментариев Razor удаляются сервером.</span><span class="sxs-lookup"><span data-stu-id="fe2da-190">Razor comments are removed by the server before the page is rendered.</span></span> <span data-ttu-id="fe2da-191">Использует Razor `@*  *@` в качестве разделителя комментариев.</span><span class="sxs-lookup"><span data-stu-id="fe2da-191">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="fe2da-192">Следующий код закомментирована, поэтому сервер не будет содержать разметку:</span><span class="sxs-lookup"><span data-stu-id="fe2da-192">The following code is commented out, so the server will not render any markup:</span></span>

```none
 @*
 @{
     /* C# comment. */
     // Another C# comment.
 }
 <!-- HTML comment -->
*@
```

<a name=razor-directives-label></a>

## <a name="directives"></a><span data-ttu-id="fe2da-193">Директивы</span><span class="sxs-lookup"><span data-stu-id="fe2da-193">Directives</span></span>

<span data-ttu-id="fe2da-194">Неявные выражения с следующие зарезервированные ключевые слова представляются директивы Razor `@` символов.</span><span class="sxs-lookup"><span data-stu-id="fe2da-194">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="fe2da-195">Директива обычно будет изменить способ разборе или включить различные функциональные возможности на странице Razor.</span><span class="sxs-lookup"><span data-stu-id="fe2da-195">A directive will typically change the way a page is parsed or enable different functionality within your Razor page.</span></span>

<span data-ttu-id="fe2da-196">Основные сведения о том, как Razor создает код для представления будет упрощения понимания работы директивы.</span><span class="sxs-lookup"><span data-stu-id="fe2da-196">Understanding how Razor generates code for a view will make it easier to understand how directives work.</span></span> <span data-ttu-id="fe2da-197">Страница Razor используется для создания файла C#.</span><span class="sxs-lookup"><span data-stu-id="fe2da-197">A Razor page is used to generate a C# file.</span></span> <span data-ttu-id="fe2da-198">Например, эта страница Razor:</span><span class="sxs-lookup"><span data-stu-id="fe2da-198">For example, this Razor page:</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="fe2da-199">Создает класс следующего вида:</span><span class="sxs-lookup"><span data-stu-id="fe2da-199">Generates a class similar to the following:</span></span>

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

<span data-ttu-id="fe2da-200">[Просмотр класса Razor C#, созданного для представления](#razor-customcompilationservice-label) способы просмотра этим классом.</span><span class="sxs-lookup"><span data-stu-id="fe2da-200">[Viewing the Razor C# class generated for a view](#razor-customcompilationservice-label) explains how to view this generated class.</span></span>

### `@using`

<span data-ttu-id="fe2da-201">`@using` Директива добавит c# `using` на странице созданный razor директиву:</span><span class="sxs-lookup"><span data-stu-id="fe2da-201">The `@using` directive will add the c# `using` directive to the generated razor page:</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact9.cshtml)]

### `@model`

<span data-ttu-id="fe2da-202">`@model` Директива определяет тип модели, переданных в страницу Razor.</span><span class="sxs-lookup"><span data-stu-id="fe2da-202">The `@model` directive specifies the type of the model passed to the Razor page.</span></span> <span data-ttu-id="fe2da-203">Для этого используется следующий синтаксис:</span><span class="sxs-lookup"><span data-stu-id="fe2da-203">It uses the following syntax:</span></span>

```none
@model TypeNameOfModel
   ```

<span data-ttu-id="fe2da-204">Например, при создании приложения ASP.NET Core MVC для каждой учетной записи *Views/Account/Login.cshtml* представления Razor содержит следующее объявление модели:</span><span class="sxs-lookup"><span data-stu-id="fe2da-204">For example, if you create an ASP.NET Core MVC app with individual user accounts, the *Views/Account/Login.cshtml* Razor view contains the following model declaration:</span></span>

```csharp
@model LoginViewModel
   ```

<span data-ttu-id="fe2da-205">В приведенном выше примере класс наследует класс, созданный `RazorPage<dynamic>`.</span><span class="sxs-lookup"><span data-stu-id="fe2da-205">In the preceding class example, the class generated inherits from `RazorPage<dynamic>`.</span></span> <span data-ttu-id="fe2da-206">Добавив `@model` контролировать то, что наследуется.</span><span class="sxs-lookup"><span data-stu-id="fe2da-206">By adding an `@model` you control what’s inherited.</span></span> <span data-ttu-id="fe2da-207">Пример</span><span class="sxs-lookup"><span data-stu-id="fe2da-207">For example</span></span>

```csharp
@model LoginViewModel
   ```

<span data-ttu-id="fe2da-208">Создает следующий класс</span><span class="sxs-lookup"><span data-stu-id="fe2da-208">Generates the following class</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
   ```

<span data-ttu-id="fe2da-209">Предоставлять страниц Razor `Model` свойство для доступа к модели, переданных в страницу.</span><span class="sxs-lookup"><span data-stu-id="fe2da-209">Razor pages expose a `Model` property for accessing the model passed to the page.</span></span>

```html
<div>The Login Email: @Model.Email</div>
   ```

<span data-ttu-id="fe2da-210">`@model` Директива указана тип этого свойства (путем указания `T` в `RazorPage<T>` наследника созданного класса страницы).</span><span class="sxs-lookup"><span data-stu-id="fe2da-210">The `@model` directive specified the type of this property (by specifying the `T` in `RazorPage<T>` that the generated class for your page derives from).</span></span> <span data-ttu-id="fe2da-211">Если не указать `@model` директивы `Model` свойство будет иметь тип `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="fe2da-211">If you don't specify the `@model` directive the `Model` property will be of type `dynamic`.</span></span> <span data-ttu-id="fe2da-212">Значение модели передается из контроллера в представление.</span><span class="sxs-lookup"><span data-stu-id="fe2da-212">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="fe2da-213">В разделе [со строго типизированными моделей и @model ключевое слово](../../tutorials/first-mvc-app/adding-model.md#strongly-typed-models-keyword-label) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="fe2da-213">See [Strongly typed models and the @model keyword](../../tutorials/first-mvc-app/adding-model.md#strongly-typed-models-keyword-label) for more information.</span></span>

### `@inherits`

<span data-ttu-id="fe2da-214">`@inherits` Директива дает полный контроль над класс наследует Razor страницы:</span><span class="sxs-lookup"><span data-stu-id="fe2da-214">The `@inherits` directive gives you full control of the class your Razor page inherits:</span></span>

```none
@inherits TypeNameOfClassToInheritFrom
   ```

<span data-ttu-id="fe2da-215">Например предположим, мы должны были пользовательского страницы типа Razor:</span><span class="sxs-lookup"><span data-stu-id="fe2da-215">For instance, let’s say we had the following custom Razor page type:</span></span>

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="fe2da-216">Создает следующие Razor `<div>Custom text: Hello World</div>`.</span><span class="sxs-lookup"><span data-stu-id="fe2da-216">The following Razor would generate `<div>Custom text: Hello World</div>`.</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="fe2da-217">Нельзя использовать `@model` и `@inherits` на одной странице.</span><span class="sxs-lookup"><span data-stu-id="fe2da-217">You can't use `@model` and `@inherits` on the same page.</span></span> <span data-ttu-id="fe2da-218">Может иметь `@inherits` в *_ViewImports.cshtml* файл, который импортирует страниц Razor.</span><span class="sxs-lookup"><span data-stu-id="fe2da-218">You can have `@inherits` in a *_ViewImports.cshtml* file that the Razor page imports.</span></span> <span data-ttu-id="fe2da-219">Например, если представление Razor импортированы следующие *_ViewImports.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="fe2da-219">For example, if your Razor view imported the following *_ViewImports.cshtml* file:</span></span>

[!code-html[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="fe2da-220">На следующей странице строго типизированные Razor</span><span class="sxs-lookup"><span data-stu-id="fe2da-220">The following strongly typed Razor page</span></span>

[!code-html[Main](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="fe2da-221">Создает этот HTML-разметку:</span><span class="sxs-lookup"><span data-stu-id="fe2da-221">Generates this HTML markup:</span></span>

```none
<div>The Login Email: Rick@contoso.com</div>
<div>Custom text: Hello World</div>
```

<span data-ttu-id="fe2da-222">При передаче "[Rick@contoso.com](mailto:Rick@contoso.com)» в модели:</span><span class="sxs-lookup"><span data-stu-id="fe2da-222">When passed "[Rick@contoso.com](mailto:Rick@contoso.com)" in the model:</span></span>

   <span data-ttu-id="fe2da-223">Дополнительные сведения см. в статье о [макете](layout.md).</span><span class="sxs-lookup"><span data-stu-id="fe2da-223">See [Layout](layout.md) for more information.</span></span>

### `@inject`

<span data-ttu-id="fe2da-224">`@inject` Директива позволяет внедрять службы из вашего [контейнер службы](../../fundamentals/dependency-injection.md) в Razor страницу для использования.</span><span class="sxs-lookup"><span data-stu-id="fe2da-224">The `@inject` directive enables you to inject a service from your [service container](../../fundamentals/dependency-injection.md)  into your Razor page for use.</span></span> <span data-ttu-id="fe2da-225">В разделе [внедрение зависимостей в представления](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="fe2da-225">See [Dependency injection into views](dependency-injection.md).</span></span>

<a name="functions"></a>

### `@functions`

<span data-ttu-id="fe2da-226">`@functions` Директива позволяет добавлять функции уровня содержимого на страницу Razor.</span><span class="sxs-lookup"><span data-stu-id="fe2da-226">The `@functions` directive enables you to add function level content to your Razor page.</span></span> <span data-ttu-id="fe2da-227">Синтаксис выглядит следующим образом.</span><span class="sxs-lookup"><span data-stu-id="fe2da-227">The syntax is:</span></span>

```none
@functions { // C# Code }
   ```

<span data-ttu-id="fe2da-228">Пример:</span><span class="sxs-lookup"><span data-stu-id="fe2da-228">For example:</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="fe2da-229">Создает следующую разметку HTML:</span><span class="sxs-lookup"><span data-stu-id="fe2da-229">Generates the following HTML markup:</span></span>

```none
<div>From method: Hello</div>
   ```

<span data-ttu-id="fe2da-230">Созданный Razor C# выглядит следующим образом.</span><span class="sxs-lookup"><span data-stu-id="fe2da-230">The generated Razor C# looks like:</span></span>

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### `@section`

<span data-ttu-id="fe2da-231">`@section` Директива используется в сочетании с [страница макета](layout.md) Включение представления для отображения содержимого в различных частях готовый для просмотра HTML-страницы.</span><span class="sxs-lookup"><span data-stu-id="fe2da-231">The `@section` directive is used in conjunction with the [layout page](layout.md) to enable views to render content in different parts of the rendered HTML page.</span></span> <span data-ttu-id="fe2da-232">В разделе [разделы](layout.md#layout-sections-label) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="fe2da-232">See [Sections](layout.md#layout-sections-label) for more information.</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="fe2da-233">Вспомогательных функций тегов</span><span class="sxs-lookup"><span data-stu-id="fe2da-233">Tag Helpers</span></span>

<span data-ttu-id="fe2da-234">Следующие [вспомогательных функций тегов](tag-helpers/index.md) директивы подробно описаны в ссылкам.</span><span class="sxs-lookup"><span data-stu-id="fe2da-234">The following [Tag Helpers](tag-helpers/index.md) directives are detailed in the links provided.</span></span>

* [@addTagHelper](tag-helpers/intro.md#add-helper-label)
* [@removeTagHelper](tag-helpers/intro.md#remove-razor-directives-label)
* [@tagHelperPrefix](tag-helpers/intro.md#prefix-razor-directives-label)

<a name=razor-reserved-keywords-label></a>

## <a name="razor-reserved-keywords"></a><span data-ttu-id="fe2da-235">Razor зарезервированные ключевые слова</span><span class="sxs-lookup"><span data-stu-id="fe2da-235">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="fe2da-236">Ключевые слова Razor</span><span class="sxs-lookup"><span data-stu-id="fe2da-236">Razor keywords</span></span>

* <span data-ttu-id="fe2da-237">страница (требуется Core ASP.NET 2.0 и более поздние версии)</span><span class="sxs-lookup"><span data-stu-id="fe2da-237">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="fe2da-238">функции</span><span class="sxs-lookup"><span data-stu-id="fe2da-238">functions</span></span>
* <span data-ttu-id="fe2da-239">наследует</span><span class="sxs-lookup"><span data-stu-id="fe2da-239">inherits</span></span>
* <span data-ttu-id="fe2da-240">model</span><span class="sxs-lookup"><span data-stu-id="fe2da-240">model</span></span>
* <span data-ttu-id="fe2da-241">section</span><span class="sxs-lookup"><span data-stu-id="fe2da-241">section</span></span>
* <span data-ttu-id="fe2da-242">Вспомогательный (не поддерживается ASP.NET Core.)</span><span class="sxs-lookup"><span data-stu-id="fe2da-242">helper   (Not supported by ASP.NET Core.)</span></span>

<span data-ttu-id="fe2da-243">Ключевые слова Razor предварять `@(Razor Keyword)`, например `@(functions)`.</span><span class="sxs-lookup"><span data-stu-id="fe2da-243">Razor keywords can be escaped with `@(Razor Keyword)`, for example `@(functions)`.</span></span> <span data-ttu-id="fe2da-244">См. Полный пример ниже.</span><span class="sxs-lookup"><span data-stu-id="fe2da-244">See the complete sample below.</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="fe2da-245">Ключевые слова C# Razor</span><span class="sxs-lookup"><span data-stu-id="fe2da-245">C# Razor keywords</span></span>

* <span data-ttu-id="fe2da-246">case</span><span class="sxs-lookup"><span data-stu-id="fe2da-246">case</span></span>
* <span data-ttu-id="fe2da-247">do</span><span class="sxs-lookup"><span data-stu-id="fe2da-247">do</span></span>
* <span data-ttu-id="fe2da-248">default</span><span class="sxs-lookup"><span data-stu-id="fe2da-248">default</span></span>
* <span data-ttu-id="fe2da-249">for</span><span class="sxs-lookup"><span data-stu-id="fe2da-249">for</span></span>
* <span data-ttu-id="fe2da-250">foreach</span><span class="sxs-lookup"><span data-stu-id="fe2da-250">foreach</span></span>
* <span data-ttu-id="fe2da-251">if</span><span class="sxs-lookup"><span data-stu-id="fe2da-251">if</span></span>
* <span data-ttu-id="fe2da-252">else</span><span class="sxs-lookup"><span data-stu-id="fe2da-252">else</span></span>
* <span data-ttu-id="fe2da-253">lock</span><span class="sxs-lookup"><span data-stu-id="fe2da-253">lock</span></span>
* <span data-ttu-id="fe2da-254">switch</span><span class="sxs-lookup"><span data-stu-id="fe2da-254">switch</span></span>
* <span data-ttu-id="fe2da-255">try</span><span class="sxs-lookup"><span data-stu-id="fe2da-255">try</span></span>
* <span data-ttu-id="fe2da-256">catch</span><span class="sxs-lookup"><span data-stu-id="fe2da-256">catch</span></span>
* <span data-ttu-id="fe2da-257">finally</span><span class="sxs-lookup"><span data-stu-id="fe2da-257">finally</span></span>
* <span data-ttu-id="fe2da-258">использование</span><span class="sxs-lookup"><span data-stu-id="fe2da-258">using</span></span>
* <span data-ttu-id="fe2da-259">while</span><span class="sxs-lookup"><span data-stu-id="fe2da-259">while</span></span>

<span data-ttu-id="fe2da-260">Ключевые слова C# Razor должны быть двойные escape-последовательность с `@(@C# Razor Keyword)`, например `@(@case)`.</span><span class="sxs-lookup"><span data-stu-id="fe2da-260">C# Razor keywords need to be double escaped with `@(@C# Razor Keyword)`, for example `@(@case)`.</span></span> <span data-ttu-id="fe2da-261">Первый `@` экранирует анализатор Razor второй `@` экранирует анализатор C#.</span><span class="sxs-lookup"><span data-stu-id="fe2da-261">The first `@` escapes the Razor parser, the second `@` escapes the C# parser.</span></span> <span data-ttu-id="fe2da-262">См. Полный пример ниже.</span><span class="sxs-lookup"><span data-stu-id="fe2da-262">See the complete sample below.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="fe2da-263">Зарезервированные ключевые слова, не используется в Razor</span><span class="sxs-lookup"><span data-stu-id="fe2da-263">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="fe2da-264">namespace</span><span class="sxs-lookup"><span data-stu-id="fe2da-264">namespace</span></span>
* <span data-ttu-id="fe2da-265">класс</span><span class="sxs-lookup"><span data-stu-id="fe2da-265">class</span></span>

<a name=razor-customcompilationservice-label></a>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="fe2da-266">Просмотр класса Razor C#, созданного для представления</span><span class="sxs-lookup"><span data-stu-id="fe2da-266">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="fe2da-267">Добавьте следующий класс в проект ASP.NET MVC основных компонентов:</span><span class="sxs-lookup"><span data-stu-id="fe2da-267">Add the following class to your ASP.NET Core MVC project:</span></span>

[!code-csharp[Main](razor/sample/Services/CustomCompilationService.cs)]

<span data-ttu-id="fe2da-268">Переопределить `ICompilationService` добавленные MVC с выше класса;</span><span class="sxs-lookup"><span data-stu-id="fe2da-268">Override the `ICompilationService` added by MVC with the above class;</span></span>

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=29-33)]

<span data-ttu-id="fe2da-269">Точка останова `Compile` метод `CustomCompilationService` и представление `compilationContent`.</span><span class="sxs-lookup"><span data-stu-id="fe2da-269">Set a break point on the `Compile` method of `CustomCompilationService` and view `compilationContent`.</span></span>

![Представление визуализатор текста compilationContent](razor/_static/tvr.png)

<a name="case"></a>
## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="fe2da-271">Представление уточняющих запросов и чувствительности к регистру</span><span class="sxs-lookup"><span data-stu-id="fe2da-271">View lookups and case sensitivity</span></span>

<span data-ttu-id="fe2da-272">Обработчик представлений Razor выполняет поиск с учетом регистра для представлений.</span><span class="sxs-lookup"><span data-stu-id="fe2da-272">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="fe2da-273">Однако фактические уточняющего запроса определяется базового источника:</span><span class="sxs-lookup"><span data-stu-id="fe2da-273">However, the actual lookup is determined by the underlying source:</span></span>

* <span data-ttu-id="fe2da-274">На основе исходного файла:</span><span class="sxs-lookup"><span data-stu-id="fe2da-274">File based source:</span></span> 

    * <span data-ttu-id="fe2da-275">В операционных системах без учета регистра файловые системы (например, Windows) поиск поставщика физических файлов зависят от регистра.</span><span class="sxs-lookup"><span data-stu-id="fe2da-275">On operating systems with case insensitive file systems (like Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="fe2da-276">Например `return View("Test")` приведет к появлению `/Views/Home/Test.cshtml`, `/Views/home/test.cshtml` и всех других вариантов регистр обнаруживаются.</span><span class="sxs-lookup"><span data-stu-id="fe2da-276">For example `return View("Test")` would result in `/Views/Home/Test.cshtml`, `/Views/home/test.cshtml` and all other casing variants would be discovered.</span></span>
    * <span data-ttu-id="fe2da-277">В системах файлов с учетом регистра, которая содержит Linux OSX и `EmbeddedFileProvider`, уточняющие запросы выполняются с учетом регистра.</span><span class="sxs-lookup"><span data-stu-id="fe2da-277">On case sensitive file systems, which includes Linux, OSX and `EmbeddedFileProvider`, lookups are case sensitive.</span></span> <span data-ttu-id="fe2da-278">Например `return View("Test")` выглядит специально `/Views/Home/Test.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="fe2da-278">For example, `return View("Test")` would specifically look for `/Views/Home/Test.cshtml`.</span></span>
        
* <span data-ttu-id="fe2da-279">Предварительно скомпилированный представления:</span><span class="sxs-lookup"><span data-stu-id="fe2da-279">Precompiled views:</span></span>

   * <span data-ttu-id="fe2da-280">С помощью ASP.Net Core 2.0 и более поздних версиях поиска предкомпилированного представления без учета регистра для всех операционных систем.</span><span class="sxs-lookup"><span data-stu-id="fe2da-280">With ASP.Net Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="fe2da-281">Поведение идентично поведение поставщика физического файла в Windows.</span><span class="sxs-lookup"><span data-stu-id="fe2da-281">The behavior is identical to physical file provider's behavior on Windows.</span></span> 
   <span data-ttu-id="fe2da-282">Примечание: Если два представления предкомпилированного отличаются только регистром, результатом поиска является недетерминированным.</span><span class="sxs-lookup"><span data-stu-id="fe2da-282">Note: If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="fe2da-283">Разработчики могут совпадает с регистром имена файлов и каталогов для имен области, контроллера и действия.</span><span class="sxs-lookup"><span data-stu-id="fe2da-283">Developers are encouraged to match the casing of file and directory names to the casing of area, controller and action names.</span></span> <span data-ttu-id="fe2da-284">Благодаря развертываний остаются независимыми от используемой файловой системы.</span><span class="sxs-lookup"><span data-stu-id="fe2da-284">This would ensure your deployments remain agnostic of the underlying file system.</span></span>
