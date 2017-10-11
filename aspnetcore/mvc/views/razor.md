---
title: "Справочник по синтаксису Razor для ASP.NET Core"
author: guardrex
description: "Дополнительные сведения о синтаксиса Razor разметки для внедрения серверного кода в веб-страниц."
keywords: "Директивы Razor ASP.NET Core, Razor,"
ms.author: riande
manager: wpickett
ms.date: 09/29/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: 532e278597a0029b5bae93068af5b7b147c35688
ms.sourcegitcommit: e45f8912ce32b4071bf7e83b8f8315cd8bba3520
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/04/2017
---
# <a name="razor-syntax-for-aspnet-core"></a><span data-ttu-id="8589f-104">Синтаксис Razor ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8589f-104">Razor syntax for ASP.NET Core</span></span>

<span data-ttu-id="8589f-105">По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Latham Люк](https://github.com/guardrex), и [Taylor Mullen](https://twitter.com/ntaylormullen)</span><span class="sxs-lookup"><span data-stu-id="8589f-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), and [Taylor Mullen](https://twitter.com/ntaylormullen)</span></span>

<span data-ttu-id="8589f-106">Razor является синтаксис разметки для внедрения серверного кода в веб-страниц.</span><span class="sxs-lookup"><span data-stu-id="8589f-106">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="8589f-107">Синтаксис Razor содержит Razor разметки, C# и HTML.</span><span class="sxs-lookup"><span data-stu-id="8589f-107">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="8589f-108">Файлы, содержащие Razor, обычно имеют *.cshtml* расширение имени файла.</span><span class="sxs-lookup"><span data-stu-id="8589f-108">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="8589f-109">Подготовка к просмотру HTML</span><span class="sxs-lookup"><span data-stu-id="8589f-109">Rendering HTML</span></span>

<span data-ttu-id="8589f-110">Язык по умолчанию Razor — HTML.</span><span class="sxs-lookup"><span data-stu-id="8589f-110">The default Razor language is HTML.</span></span> <span data-ttu-id="8589f-111">Подготовки отчетов HTML из разметки Razor ничем не отличается от визуализации HTML из HTML-файл.</span><span class="sxs-lookup"><span data-stu-id="8589f-111">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span> <span data-ttu-id="8589f-112">Если поместить HTML-разметку в *.cshtml* Razor-файл, он отображается на сервере без изменений.</span><span class="sxs-lookup"><span data-stu-id="8589f-112">If you place HTML markup into a *.cshtml* Razor file, it's rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="8589f-113">Синтаксис Razor</span><span class="sxs-lookup"><span data-stu-id="8589f-113">Razor syntax</span></span>

<span data-ttu-id="8589f-114">Razor поддерживает C# и использует `@` символ переход из HTML для C#.</span><span class="sxs-lookup"><span data-stu-id="8589f-114">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="8589f-115">Razor вычисляет выражения C# и отображает их в выходных данных HTML.</span><span class="sxs-lookup"><span data-stu-id="8589f-115">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="8589f-116">Когда `@` следуют символ [Razor зарезервированное ключевое слово](#razor-reserved-keywords), он переходит в Razor разметку.</span><span class="sxs-lookup"><span data-stu-id="8589f-116">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="8589f-117">В противном случае он переходит в обычный C#.</span><span class="sxs-lookup"><span data-stu-id="8589f-117">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="8589f-118">Чтобы escape `@` символ в разметке Razor, использовать второй `@` символа:</span><span class="sxs-lookup"><span data-stu-id="8589f-118">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="8589f-119">Код отображается в формате HTML с помощью одного `@` символа:</span><span class="sxs-lookup"><span data-stu-id="8589f-119">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="8589f-120">HTML-атрибутов и содержимого, содержащий адреса электронной почты не обрабатывают `@` символ как символ перехода.</span><span class="sxs-lookup"><span data-stu-id="8589f-120">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="8589f-121">В следующем примере адреса электронной почты не изменятся путем синтаксического анализа Razor:</span><span class="sxs-lookup"><span data-stu-id="8589f-121">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="8589f-122">Неявные выражения Razor</span><span class="sxs-lookup"><span data-stu-id="8589f-122">Implicit Razor expressions</span></span>

<span data-ttu-id="8589f-123">Неявные Razor выражения начинаются со `@` вместе с кодом C#:</span><span class="sxs-lookup"><span data-stu-id="8589f-123">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="8589f-124">За исключением C# `await` ключевое слово, неявные выражения не должно содержать пробелов.</span><span class="sxs-lookup"><span data-stu-id="8589f-124">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="8589f-125">Можно intermingle пробелы, если очистить конца оператора C#:</span><span class="sxs-lookup"><span data-stu-id="8589f-125">You can intermingle spaces if the C# statement has a clear ending:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

## <a name="explicit-razor-expressions"></a><span data-ttu-id="8589f-126">Прямые выражения Razor</span><span class="sxs-lookup"><span data-stu-id="8589f-126">Explicit Razor expressions</span></span>

<span data-ttu-id="8589f-127">Явные Razor выражения состоят из `@` символ со сбалансированной скобками.</span><span class="sxs-lookup"><span data-stu-id="8589f-127">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="8589f-128">Для подготовки к просмотру время прошлой неделе, используется следующая разметка Razor:</span><span class="sxs-lookup"><span data-stu-id="8589f-128">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="8589f-129">Любое содержимое в `@()` скобки вычисляется и отображается в выходных данных.</span><span class="sxs-lookup"><span data-stu-id="8589f-129">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="8589f-130">Неявные выражения, описанные в предыдущем разделе, обычно не может содержать пробелы.</span><span class="sxs-lookup"><span data-stu-id="8589f-130">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="8589f-131">В следующем коде одну неделю не вычитается из текущего времени:</span><span class="sxs-lookup"><span data-stu-id="8589f-131">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="8589f-132">Код отображает следующий код HTML:</span><span class="sxs-lookup"><span data-stu-id="8589f-132">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="8589f-133">Явное выражение можно использовать для объединения текста с результатом выражения:</span><span class="sxs-lookup"><span data-stu-id="8589f-133">You can use an explicit expression to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="8589f-134">Без явного выражение `<p>Age@joe.Age</p>` рассматривается как адрес электронной почты и `<p>Age@joe.Age</p>` подготавливается к просмотру.</span><span class="sxs-lookup"><span data-stu-id="8589f-134">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="8589f-135">Если они написаны как выражение явного `<p>Age33</p>` подготавливается к просмотру.</span><span class="sxs-lookup"><span data-stu-id="8589f-135">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>

## <a name="expression-encoding"></a><span data-ttu-id="8589f-136">Кодировка выражения</span><span class="sxs-lookup"><span data-stu-id="8589f-136">Expression encoding</span></span>

<span data-ttu-id="8589f-137">Выражения C#, которые были оценены как строка, кодируется в HTML.</span><span class="sxs-lookup"><span data-stu-id="8589f-137">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="8589f-138">Выражения C#, которые были оценены как `IHtmlContent` подготавливаются к просмотру непосредственно через `IHtmlContent.WriteTo`.</span><span class="sxs-lookup"><span data-stu-id="8589f-138">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="8589f-139">Выражения C#, которые не дают `IHtmlContent` преобразуются в строки по `ToString` и закодированы, прежде чем они отображаются.</span><span class="sxs-lookup"><span data-stu-id="8589f-139">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="8589f-140">Код отображает следующий код HTML:</span><span class="sxs-lookup"><span data-stu-id="8589f-140">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="8589f-141">HTML-код отображается в обозревателе, как:</span><span class="sxs-lookup"><span data-stu-id="8589f-141">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="8589f-142">`HtmlHelper.Raw`выходные данные не закодированы, однако к просмотру в виде HTML-разметка.</span><span class="sxs-lookup"><span data-stu-id="8589f-142">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="8589f-143">С помощью `HtmlHelper.Raw` на unsanitized пользователя ввод представляет угрозу безопасности.</span><span class="sxs-lookup"><span data-stu-id="8589f-143">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="8589f-144">Ввод данных пользователем может содержать вредоносный JavaScript или использовать.</span><span class="sxs-lookup"><span data-stu-id="8589f-144">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="8589f-145">Ввод данных пользователем очистки данных сложно.</span><span class="sxs-lookup"><span data-stu-id="8589f-145">Sanitizing user input is difficult.</span></span> <span data-ttu-id="8589f-146">Избегайте использования `HtmlHelper.Raw` с вводимыми пользователем данными.</span><span class="sxs-lookup"><span data-stu-id="8589f-146">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="8589f-147">Код отображает следующий код HTML:</span><span class="sxs-lookup"><span data-stu-id="8589f-147">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="8589f-148">Блоки кода Razor</span><span class="sxs-lookup"><span data-stu-id="8589f-148">Razor code blocks</span></span>

<span data-ttu-id="8589f-149">Блоки кода Razor начинаться с `@` и заключены в `{}`.</span><span class="sxs-lookup"><span data-stu-id="8589f-149">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="8589f-150">В отличие от выражения код внутри блоков кода C# не отображаются.</span><span class="sxs-lookup"><span data-stu-id="8589f-150">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="8589f-151">Блоки кода и выражения в представлении использующими ту же область и определены в порядке:</span><span class="sxs-lookup"><span data-stu-id="8589f-151">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

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

<span data-ttu-id="8589f-152">Код отображает следующий код HTML:</span><span class="sxs-lookup"><span data-stu-id="8589f-152">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a><span data-ttu-id="8589f-153">Неявные переходов</span><span class="sxs-lookup"><span data-stu-id="8589f-153">Implicit transitions</span></span>

<span data-ttu-id="8589f-154">В блоке кода языка по умолчанию — C#, но вы можете переходить обратно в формат HTML:</span><span class="sxs-lookup"><span data-stu-id="8589f-154">The default language in a code block is C#, but you can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="8589f-155">Явные перехода с разделителями</span><span class="sxs-lookup"><span data-stu-id="8589f-155">Explicit delimited transition</span></span>

<span data-ttu-id="8589f-156">Чтобы определить часть блока кода, должны обрабатывать HTML, окружить символов для подготовки к просмотру Razor  **\<текст >** тег:</span><span class="sxs-lookup"><span data-stu-id="8589f-156">To define a sub-section of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="8589f-157">Используйте этот подход, когда нужно отобразить HTML-код, не заключенных в HTML-тега.</span><span class="sxs-lookup"><span data-stu-id="8589f-157">Use this approach when you want to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="8589f-158">Без тега HTML или Razor возникнет ошибка среды выполнения Razor.</span><span class="sxs-lookup"><span data-stu-id="8589f-158">Without an HTML or Razor tag, you receive a Razor runtime error.</span></span>

<span data-ttu-id="8589f-159"> **\<Текст >** тег также полезен для управления пробелов при подготовке к просмотру содержимого.</span><span class="sxs-lookup"><span data-stu-id="8589f-159">The **\<text>** tag is also useful to control whitespace when rendering content.</span></span> <span data-ttu-id="8589f-160">Содержимое между  **\<текст >** визуализации тегов и пробелов до или после  **\<текст >** тегов отображается в выходных данных HTML.</span><span class="sxs-lookup"><span data-stu-id="8589f-160">Only the content between the **\<text>** tags is rendered, and no whitespace before or after the **\<text>** tags appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="8589f-161">Явные строки перехода с @:</span><span class="sxs-lookup"><span data-stu-id="8589f-161">Explicit Line Transition with @:</span></span>

<span data-ttu-id="8589f-162">Для подготовки к просмотру остальная часть всю строку как HTML внутри блока кода, используйте `@:` синтаксис:</span><span class="sxs-lookup"><span data-stu-id="8589f-162">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="8589f-163">Без `@:` в коде, возникнет ошибка среды выполнения Razor.</span><span class="sxs-lookup"><span data-stu-id="8589f-163">Without the `@:` in the code, you recieve a Razor runtime error.</span></span>

## <a name="control-structures"></a><span data-ttu-id="8589f-164">Управляющие структуры</span><span class="sxs-lookup"><span data-stu-id="8589f-164">Control Structures</span></span>

<span data-ttu-id="8589f-165">Управляющие структуры являются расширением блоков кода.</span><span class="sxs-lookup"><span data-stu-id="8589f-165">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="8589f-166">Все аспекты блоков кода (плавный переход к разметки встроенный C#) также применяются к следующим структурам.</span><span class="sxs-lookup"><span data-stu-id="8589f-166">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures.</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="8589f-167">Условные выражения @if, else if, else, и@switch</span><span class="sxs-lookup"><span data-stu-id="8589f-167">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="8589f-168">`@if`элементы управления, при выполнении кода.</span><span class="sxs-lookup"><span data-stu-id="8589f-168">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="8589f-169">`else`и `else if` не требуют `@` символа:</span><span class="sxs-lookup"><span data-stu-id="8589f-169">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="8589f-170">Можно использовать инструкцию switch следующим образом:</span><span class="sxs-lookup"><span data-stu-id="8589f-170">You can use a switch statement like this:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="8589f-171">Циклы @for, @foreach, @while, и @do во время</span><span class="sxs-lookup"><span data-stu-id="8589f-171">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="8589f-172">Может отображать шаблонного HTML с помощью элемента управления операторы цикла.</span><span class="sxs-lookup"><span data-stu-id="8589f-172">You can render templated HTML with looping control statements.</span></span> <span data-ttu-id="8589f-173">Для подготовки к просмотру список лиц:</span><span class="sxs-lookup"><span data-stu-id="8589f-173">To render a list of people:</span></span>

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

<span data-ttu-id="8589f-174">Можно использовать любой из следующих инструкций цикла:</span><span class="sxs-lookup"><span data-stu-id="8589f-174">You can use any of the following looping statements:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="8589f-175">Составные@using</span><span class="sxs-lookup"><span data-stu-id="8589f-175">Compound @using</span></span>

<span data-ttu-id="8589f-176">В C# `using` инструкция используется для обеспечения удаления объекта.</span><span class="sxs-lookup"><span data-stu-id="8589f-176">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="8589f-177">В Razor тот же механизм используется для создания вспомогательных методов HTML, который содержит дополнительное содержимое.</span><span class="sxs-lookup"><span data-stu-id="8589f-177">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="8589f-178">Например, можно с помощью вспомогательных методов HTML для отрисовки тега формы с `@using` инструкции:</span><span class="sxs-lookup"><span data-stu-id="8589f-178">For instance, you can utilize HTML Helpers to render a form tag with the `@using` statement:</span></span>

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

<span data-ttu-id="8589f-179">Также можно выполнять действия на уровне области с [вспомогательных функций тегов](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="8589f-179">You can also perform scope-level actions with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="8589f-180">@try, catch, finally</span><span class="sxs-lookup"><span data-stu-id="8589f-180">@try, catch, finally</span></span>

<span data-ttu-id="8589f-181">Обработка исключений похожа на C#:</span><span class="sxs-lookup"><span data-stu-id="8589f-181">Exception handling is similar to C#:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="8589f-182">Razor имеет возможность защитить критические секции с помощью инструкций блокировки:</span><span class="sxs-lookup"><span data-stu-id="8589f-182">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="8589f-183">Комментарии</span><span class="sxs-lookup"><span data-stu-id="8589f-183">Comments</span></span>

<span data-ttu-id="8589f-184">Razor поддерживает комментарии C# и HTML.</span><span class="sxs-lookup"><span data-stu-id="8589f-184">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="8589f-185">Код отображает следующий код HTML:</span><span class="sxs-lookup"><span data-stu-id="8589f-185">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="8589f-186">Комментарии Razor удаляются сервером перед отображением веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="8589f-186">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="8589f-187">Использует Razor `@*  *@` в качестве разделителя комментариев.</span><span class="sxs-lookup"><span data-stu-id="8589f-187">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="8589f-188">Следующий код закомментирована, поэтому сервер не создают разметку:</span><span class="sxs-lookup"><span data-stu-id="8589f-188">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="8589f-189">Директивы</span><span class="sxs-lookup"><span data-stu-id="8589f-189">Directives</span></span>

<span data-ttu-id="8589f-190">Неявные выражения с следующие зарезервированные ключевые слова представляются директивы Razor `@` символов.</span><span class="sxs-lookup"><span data-stu-id="8589f-190">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="8589f-191">Директива обычно изменяет способ представления анализируется или включает различные функциональные возможности.</span><span class="sxs-lookup"><span data-stu-id="8589f-191">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="8589f-192">Основные сведения о том, как Razor создает код для представления помогает понять принципы работы директивы.</span><span class="sxs-lookup"><span data-stu-id="8589f-192">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="8589f-193">Код создает класс следующего вида:</span><span class="sxs-lookup"><span data-stu-id="8589f-193">The code generates a class similar to the following:</span></span>

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

<span data-ttu-id="8589f-194">Далее в этой статье разделе [Просмотр класс Razor C#, созданный для представления](#viewing-the-razor-c-class-generated-for-a-view) способы просмотра этим классом.</span><span class="sxs-lookup"><span data-stu-id="8589f-194">Later in this article, the section [Viewing the Razor C# class generated for a view](#viewing-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

### <a name="using"></a>@using

<span data-ttu-id="8589f-195">`@using` Добавляет директивы C# `using` директиву созданного представления:</span><span class="sxs-lookup"><span data-stu-id="8589f-195">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="8589f-196">`@model` Директива определяет тип модели, переданный в представлении:</span><span class="sxs-lookup"><span data-stu-id="8589f-196">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="8589f-197">При создании приложения ASP.NET Core MVC для каждой учетной записи *Views/Account/Login.cshtml* представление содержит следующее объявление модели:</span><span class="sxs-lookup"><span data-stu-id="8589f-197">If you create an ASP.NET Core MVC app with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="8589f-198">Созданный класс наследует `RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="8589f-198">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="8589f-199">Предоставляет Razor `Model` свойство для доступа к модели передаются в представление:</span><span class="sxs-lookup"><span data-stu-id="8589f-199">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="8589f-200">`@model` Директива определяет тип этого свойства.</span><span class="sxs-lookup"><span data-stu-id="8589f-200">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="8589f-201">Указывает директиву `T` в `RazorPage<T>` , созданный класс, который представлении является производным от.</span><span class="sxs-lookup"><span data-stu-id="8589f-201">The directive specifies the `T` in `RazorPage<T>` that the generated class that your view derives from.</span></span> <span data-ttu-id="8589f-202">Если не указать `@model` директивы, `Model` свойство относится к типу `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="8589f-202">If you don't specify the `@model` directive, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="8589f-203">Значение модели передается из контроллера в представление.</span><span class="sxs-lookup"><span data-stu-id="8589f-203">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="8589f-204">В разделе [со строго типизированными моделей и @model ключевое слово](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-keyword-label) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="8589f-204">See [Strongly typed models and the @model keyword](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-keyword-label) for more information.</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="8589f-205">`@inherits` Директива дает полный контроль над класс наследует представления:</span><span class="sxs-lookup"><span data-stu-id="8589f-205">The `@inherits` directive gives you full control of the class your view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="8589f-206">Ниже приведен пользовательский тип страницы Razor.</span><span class="sxs-lookup"><span data-stu-id="8589f-206">The following is a custom Razor page type:</span></span>

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="8589f-207">`CustomText` Отображается в представлении:</span><span class="sxs-lookup"><span data-stu-id="8589f-207">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="8589f-208">Код отображает следующий код HTML:</span><span class="sxs-lookup"><span data-stu-id="8589f-208">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

<span data-ttu-id="8589f-209">Нельзя использовать `@model` и `@inherits` в одном представлении.</span><span class="sxs-lookup"><span data-stu-id="8589f-209">You can't use `@model` and `@inherits` in the same view.</span></span> <span data-ttu-id="8589f-210">Может иметь `@inherits` в *_ViewImports.cshtml* файл, который импортирует представления:</span><span class="sxs-lookup"><span data-stu-id="8589f-210">You can have `@inherits` in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="8589f-211">Ниже приведен пример строго типизированное представление:</span><span class="sxs-lookup"><span data-stu-id="8589f-211">The following is an example of a strongly-typed view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="8589f-212">Если «rick@contoso.com» передается в модель, представление создает следующую разметку HTML:</span><span class="sxs-lookup"><span data-stu-id="8589f-212">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject

<span data-ttu-id="8589f-213">`@inject` Директива позволяет внедрять службы из вашего [контейнер службы](xref:fundamentals/dependency-injection) в представлении.</span><span class="sxs-lookup"><span data-stu-id="8589f-213">The `@inject` directive enables you to inject a service from your [service container](xref:fundamentals/dependency-injection) into your view.</span></span> <span data-ttu-id="8589f-214">В разделе [внедрение зависимостей в представления](xref:mvc/views/dependency-injection) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="8589f-214">See [Dependency injection into views](xref:mvc/views/dependency-injection) for more information.</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="8589f-215">`@functions` Директива позволяет добавить в представление содержимого на уровне функций:</span><span class="sxs-lookup"><span data-stu-id="8589f-215">The `@functions` directive enables you to add function-level content to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="8589f-216">Пример:</span><span class="sxs-lookup"><span data-stu-id="8589f-216">For example:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="8589f-217">Код формирует следующую разметку HTML:</span><span class="sxs-lookup"><span data-stu-id="8589f-217">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="8589f-218">Следующий код — это созданный класс Razor C#:</span><span class="sxs-lookup"><span data-stu-id="8589f-218">The following code is the generated Razor C# class:</span></span>

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

<span data-ttu-id="8589f-219">`@section` Директива используется в сочетании с [макета](xref:mvc/views/layout) Включение представления для отображения содержимого в различные части HTML-страницы.</span><span class="sxs-lookup"><span data-stu-id="8589f-219">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="8589f-220">В разделе [разделы](xref:mvc/views/layout#layout-sections-label) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="8589f-220">See [Sections](xref:mvc/views/layout#layout-sections-label) for more information.</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="8589f-221">Вспомогательных функций тегов</span><span class="sxs-lookup"><span data-stu-id="8589f-221">Tag Helpers</span></span>

<span data-ttu-id="8589f-222">Существует три директивы, которые относятся к [вспомогательных функций тегов](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="8589f-222">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="8589f-223">Директива</span><span class="sxs-lookup"><span data-stu-id="8589f-223">Directive</span></span> | <span data-ttu-id="8589f-224">Функция</span><span class="sxs-lookup"><span data-stu-id="8589f-224">Function</span></span> |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="8589f-225">Делает доступными для представления вспомогательных функций тегов.</span><span class="sxs-lookup"><span data-stu-id="8589f-225">Makes Tag Helpers available to a view.</span></span> |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="8589f-226">Удаляет ранее добавленный из представления в виде вспомогательных функций тегов.</span><span class="sxs-lookup"><span data-stu-id="8589f-226">Removes Tag Helpers previously added from a view.</span></span> |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="8589f-227">Указывает префикс тега для включения поддержка вспомогательных функций тегов и явно объявить использование вспомогательного тег.</span><span class="sxs-lookup"><span data-stu-id="8589f-227">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="8589f-228">Razor зарезервированные ключевые слова</span><span class="sxs-lookup"><span data-stu-id="8589f-228">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="8589f-229">Ключевые слова Razor</span><span class="sxs-lookup"><span data-stu-id="8589f-229">Razor keywords</span></span>

* <span data-ttu-id="8589f-230">страница (требуется Core ASP.NET 2.0 и более поздние версии)</span><span class="sxs-lookup"><span data-stu-id="8589f-230">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="8589f-231">функции</span><span class="sxs-lookup"><span data-stu-id="8589f-231">functions</span></span>
* <span data-ttu-id="8589f-232">наследует</span><span class="sxs-lookup"><span data-stu-id="8589f-232">inherits</span></span>
* <span data-ttu-id="8589f-233">model</span><span class="sxs-lookup"><span data-stu-id="8589f-233">model</span></span>
* <span data-ttu-id="8589f-234">section</span><span class="sxs-lookup"><span data-stu-id="8589f-234">section</span></span>
* <span data-ttu-id="8589f-235">Вспомогательный (не поддерживаемых ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="8589f-235">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="8589f-236">Ключевые слова Razor экранируются с `@(Razor Keyword)` (например, `@(functions)`).</span><span class="sxs-lookup"><span data-stu-id="8589f-236">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="8589f-237">Ключевые слова C# Razor</span><span class="sxs-lookup"><span data-stu-id="8589f-237">C# Razor keywords</span></span>

* <span data-ttu-id="8589f-238">case</span><span class="sxs-lookup"><span data-stu-id="8589f-238">case</span></span>
* <span data-ttu-id="8589f-239">do</span><span class="sxs-lookup"><span data-stu-id="8589f-239">do</span></span>
* <span data-ttu-id="8589f-240">default</span><span class="sxs-lookup"><span data-stu-id="8589f-240">default</span></span>
* <span data-ttu-id="8589f-241">for</span><span class="sxs-lookup"><span data-stu-id="8589f-241">for</span></span>
* <span data-ttu-id="8589f-242">foreach</span><span class="sxs-lookup"><span data-stu-id="8589f-242">foreach</span></span>
* <span data-ttu-id="8589f-243">if</span><span class="sxs-lookup"><span data-stu-id="8589f-243">if</span></span>
* <span data-ttu-id="8589f-244">else</span><span class="sxs-lookup"><span data-stu-id="8589f-244">else</span></span>
* <span data-ttu-id="8589f-245">lock</span><span class="sxs-lookup"><span data-stu-id="8589f-245">lock</span></span>
* <span data-ttu-id="8589f-246">switch</span><span class="sxs-lookup"><span data-stu-id="8589f-246">switch</span></span>
* <span data-ttu-id="8589f-247">try</span><span class="sxs-lookup"><span data-stu-id="8589f-247">try</span></span>
* <span data-ttu-id="8589f-248">catch</span><span class="sxs-lookup"><span data-stu-id="8589f-248">catch</span></span>
* <span data-ttu-id="8589f-249">finally</span><span class="sxs-lookup"><span data-stu-id="8589f-249">finally</span></span>
* <span data-ttu-id="8589f-250">использование</span><span class="sxs-lookup"><span data-stu-id="8589f-250">using</span></span>
* <span data-ttu-id="8589f-251">while</span><span class="sxs-lookup"><span data-stu-id="8589f-251">while</span></span>

<span data-ttu-id="8589f-252">Ключевые слова C# Razor необходимо экранировать двойные с `@(@C# Razor Keyword)` (например, `@(@case)`).</span><span class="sxs-lookup"><span data-stu-id="8589f-252">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="8589f-253">Первый `@` экранирует анализатор Razor.</span><span class="sxs-lookup"><span data-stu-id="8589f-253">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="8589f-254">Второй `@` экранирует анализатор C#.</span><span class="sxs-lookup"><span data-stu-id="8589f-254">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="8589f-255">Зарезервированные ключевые слова, не используется в Razor</span><span class="sxs-lookup"><span data-stu-id="8589f-255">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="8589f-256">namespace</span><span class="sxs-lookup"><span data-stu-id="8589f-256">namespace</span></span>
* <span data-ttu-id="8589f-257">класс</span><span class="sxs-lookup"><span data-stu-id="8589f-257">class</span></span>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="8589f-258">Просмотр класса Razor C#, созданного для представления</span><span class="sxs-lookup"><span data-stu-id="8589f-258">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="8589f-259">Добавьте следующий класс в проект ASP.NET MVC основных компонентов:</span><span class="sxs-lookup"><span data-stu-id="8589f-259">Add the following class to your ASP.NET Core MVC project:</span></span>

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="8589f-260">Переопределить `RazorTemplateEngine` добавленные MVC с `CustomTemplateEngine` класса:</span><span class="sxs-lookup"><span data-stu-id="8589f-260">Override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="8589f-261">Точка останова `return csharpDocument` инструкция `CustomTemplateEngine`.</span><span class="sxs-lookup"><span data-stu-id="8589f-261">Set a break point on the `return csharpDocument` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="8589f-262">Когда выполнение программы остановится в точке останова, просматривать значения `generatedCode`.</span><span class="sxs-lookup"><span data-stu-id="8589f-262">When program execution stops at the break point, view the value of `generatedCode`.</span></span>

![Представление визуализатор текста generatedCode](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="8589f-264">Представление уточняющих запросов и чувствительности к регистру</span><span class="sxs-lookup"><span data-stu-id="8589f-264">View lookups and case sensitivity</span></span>

<span data-ttu-id="8589f-265">Обработчик представлений Razor выполняет поиск с учетом регистра для представлений.</span><span class="sxs-lookup"><span data-stu-id="8589f-265">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="8589f-266">Однако фактические уточняющего запроса определяется базовая файловая система:</span><span class="sxs-lookup"><span data-stu-id="8589f-266">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="8589f-267">На основе исходного файла:</span><span class="sxs-lookup"><span data-stu-id="8589f-267">File based source:</span></span> 
  * <span data-ttu-id="8589f-268">В операционных системах без учета регистра файловые системы (например, Windows) поиск поставщика физических файлов зависят от регистра.</span><span class="sxs-lookup"><span data-stu-id="8589f-268">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="8589f-269">Например `return View("Test")` приводит к соответствий для */Views/Home/Test.cshtml*, */Views/home/test.cshtml*и любой другой тип регистра.</span><span class="sxs-lookup"><span data-stu-id="8589f-269">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="8589f-270">В системах файлов с учетом регистра (например, Linux, OSX и с `EmbeddedFileProvider`), уточняющие запросы выполняются с учетом регистра.</span><span class="sxs-lookup"><span data-stu-id="8589f-270">On case sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case sensitive.</span></span> <span data-ttu-id="8589f-271">Например `return View("Test")` специально совпадений */Views/Home/Test.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8589f-271">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="8589f-272">Предварительно скомпилированные представлений: С основными ASP.Net 2.0 и более поздних, поиске предкомпилированного представления без учета регистра для всех операционных систем.</span><span class="sxs-lookup"><span data-stu-id="8589f-272">Precompiled views: With ASP.Net Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="8589f-273">Поведение идентично поведение поставщика физического файла в Windows.</span><span class="sxs-lookup"><span data-stu-id="8589f-273">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="8589f-274">Если два представления предкомпилированного отличаются только регистром, результатом поиска является недетерминированным.</span><span class="sxs-lookup"><span data-stu-id="8589f-274">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="8589f-275">Разработчики могут совпадает с регистром имена файлов и каталогов для имен области, контроллера и действия.</span><span class="sxs-lookup"><span data-stu-id="8589f-275">Developers are encouraged to match the casing of file and directory names to the casing of area, controller, and action names.</span></span> <span data-ttu-id="8589f-276">Это гарантирует, что поиск развертываний их представлений независимо от используемой файловой системы.</span><span class="sxs-lookup"><span data-stu-id="8589f-276">This ensures your deployments will find their views regardless of the underlying file system.</span></span>
