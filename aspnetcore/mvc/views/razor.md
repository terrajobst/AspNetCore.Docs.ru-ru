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
# <a name="razor-syntax-for-aspnet-core"></a><span data-ttu-id="22bd6-103">Синтаксис Razor ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="22bd6-103">Razor syntax for ASP.NET Core</span></span>

<span data-ttu-id="22bd6-104">По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Latham Люк](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), и [Dan Vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="22bd6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="22bd6-105">Razor является синтаксис разметки для внедрения серверного кода в веб-страниц.</span><span class="sxs-lookup"><span data-stu-id="22bd6-105">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="22bd6-106">Синтаксис Razor содержит Razor разметки, C# и HTML.</span><span class="sxs-lookup"><span data-stu-id="22bd6-106">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="22bd6-107">Файлы, содержащие Razor, обычно имеют *.cshtml* расширение имени файла.</span><span class="sxs-lookup"><span data-stu-id="22bd6-107">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="22bd6-108">Подготовка к просмотру HTML</span><span class="sxs-lookup"><span data-stu-id="22bd6-108">Rendering HTML</span></span>

<span data-ttu-id="22bd6-109">Язык по умолчанию Razor — HTML.</span><span class="sxs-lookup"><span data-stu-id="22bd6-109">The default Razor language is HTML.</span></span> <span data-ttu-id="22bd6-110">Подготовки отчетов HTML из разметки Razor ничем не отличается от визуализации HTML из HTML-файл.</span><span class="sxs-lookup"><span data-stu-id="22bd6-110">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span> <span data-ttu-id="22bd6-111">HTML-разметку в *.cshtml* файлах Razor визуализируется с сервера без изменений.</span><span class="sxs-lookup"><span data-stu-id="22bd6-111">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="22bd6-112">Синтаксис Razor</span><span class="sxs-lookup"><span data-stu-id="22bd6-112">Razor syntax</span></span>

<span data-ttu-id="22bd6-113">Razor поддерживает C# и использует `@` символ переход из HTML для C#.</span><span class="sxs-lookup"><span data-stu-id="22bd6-113">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="22bd6-114">Razor вычисляет выражения C# и отображает их в выходных данных HTML.</span><span class="sxs-lookup"><span data-stu-id="22bd6-114">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="22bd6-115">Когда `@` следуют символ [Razor зарезервированное ключевое слово](#razor-reserved-keywords), он переходит в Razor разметку.</span><span class="sxs-lookup"><span data-stu-id="22bd6-115">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="22bd6-116">В противном случае он переходит в обычный C#.</span><span class="sxs-lookup"><span data-stu-id="22bd6-116">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="22bd6-117">Чтобы escape `@` символ в разметке Razor, использовать второй `@` символа:</span><span class="sxs-lookup"><span data-stu-id="22bd6-117">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="22bd6-118">Код отображается в формате HTML с помощью одного `@` символа:</span><span class="sxs-lookup"><span data-stu-id="22bd6-118">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="22bd6-119">HTML-атрибутов и содержимого, содержащий адреса электронной почты не обрабатывают `@` символ как символ перехода.</span><span class="sxs-lookup"><span data-stu-id="22bd6-119">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="22bd6-120">В следующем примере адреса электронной почты не изменятся путем синтаксического анализа Razor:</span><span class="sxs-lookup"><span data-stu-id="22bd6-120">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="22bd6-121">Неявные выражения Razor</span><span class="sxs-lookup"><span data-stu-id="22bd6-121">Implicit Razor expressions</span></span>

<span data-ttu-id="22bd6-122">Неявные Razor выражения начинаются со `@` вместе с кодом C#:</span><span class="sxs-lookup"><span data-stu-id="22bd6-122">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="22bd6-123">За исключением C# `await` ключевое слово, неявные выражения не должно содержать пробелов.</span><span class="sxs-lookup"><span data-stu-id="22bd6-123">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="22bd6-124">Если оператор C# есть очистить окончания, можно их объединения пространств:</span><span class="sxs-lookup"><span data-stu-id="22bd6-124">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="22bd6-125">Неявные выражения **не** содержать универсальные шаблоны C#, как символы в квадратных скобках (`<>`) интерпретируются как HTML-тега.</span><span class="sxs-lookup"><span data-stu-id="22bd6-125">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="22bd6-126">Следующий код является **не** допустимым:</span><span class="sxs-lookup"><span data-stu-id="22bd6-126">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="22bd6-127">Предыдущий код вызывает ошибку компилятора, примерно следующего вида:</span><span class="sxs-lookup"><span data-stu-id="22bd6-127">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="22bd6-128">Элемент «int» не была закрыта.</span><span class="sxs-lookup"><span data-stu-id="22bd6-128">The "int" element was not closed.</span></span> <span data-ttu-id="22bd6-129">Все элементы должны быть либо самостоятельно закрыть или имеет соответствующего закрывающего тега.</span><span class="sxs-lookup"><span data-stu-id="22bd6-129">All elements must be either self-closing or have a matching end tag.</span></span>
 * <span data-ttu-id="22bd6-130">Не удается преобразовать группу методов «GenericMethod», чтобы, не являющийся делегатом типа «объект».</span><span class="sxs-lookup"><span data-stu-id="22bd6-130">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="22bd6-131">Предполагается ли вызывать этот метод? "</span><span class="sxs-lookup"><span data-stu-id="22bd6-131">Did you intend to invoke the method?\`</span></span> 
 
<span data-ttu-id="22bd6-132">Универсальный метод вызывает должны быть заключены в [явное выражение Razor](#explicit-razor-expressions) или [блок кода Razor](#razor-code-blocks).</span><span class="sxs-lookup"><span data-stu-id="22bd6-132">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="22bd6-133">Прямые выражения Razor</span><span class="sxs-lookup"><span data-stu-id="22bd6-133">Explicit Razor expressions</span></span>

<span data-ttu-id="22bd6-134">Явные Razor выражения состоят из `@` символ со сбалансированной скобками.</span><span class="sxs-lookup"><span data-stu-id="22bd6-134">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="22bd6-135">Для подготовки к просмотру время прошлой неделе, используется следующая разметка Razor:</span><span class="sxs-lookup"><span data-stu-id="22bd6-135">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="22bd6-136">Любое содержимое в `@()` скобки вычисляется и отображается в выходных данных.</span><span class="sxs-lookup"><span data-stu-id="22bd6-136">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="22bd6-137">Неявные выражения, описанные в предыдущем разделе, обычно не может содержать пробелы.</span><span class="sxs-lookup"><span data-stu-id="22bd6-137">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="22bd6-138">В следующем коде одну неделю не вычитается из текущего времени:</span><span class="sxs-lookup"><span data-stu-id="22bd6-138">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="22bd6-139">Код отображает следующий код HTML:</span><span class="sxs-lookup"><span data-stu-id="22bd6-139">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="22bd6-140">Явные выражения могут использоваться для объединения текста с результатом выражения:</span><span class="sxs-lookup"><span data-stu-id="22bd6-140">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="22bd6-141">Без явного выражение `<p>Age@joe.Age</p>` рассматривается как адрес электронной почты и `<p>Age@joe.Age</p>` подготавливается к просмотру.</span><span class="sxs-lookup"><span data-stu-id="22bd6-141">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="22bd6-142">Если они написаны как выражение явного `<p>Age33</p>` подготавливается к просмотру.</span><span class="sxs-lookup"><span data-stu-id="22bd6-142">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>


<span data-ttu-id="22bd6-143">Прямые выражения можно использовать для отображения выходных данных из универсальных методов в *.cshtml* файлов.</span><span class="sxs-lookup"><span data-stu-id="22bd6-143">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="22bd6-144">В выражении неявное символов в квадратных скобках (`<>`) интерпретируются как HTML-тега.</span><span class="sxs-lookup"><span data-stu-id="22bd6-144">In an implicit expression, the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="22bd6-145">Следующая разметка является **не** допустимым Razor:</span><span class="sxs-lookup"><span data-stu-id="22bd6-145">The following markup is **not** valid Razor:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="22bd6-146">Предыдущий код вызывает ошибку компилятора, примерно следующего вида:</span><span class="sxs-lookup"><span data-stu-id="22bd6-146">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="22bd6-147">Элемент «int» не была закрыта.</span><span class="sxs-lookup"><span data-stu-id="22bd6-147">The "int" element was not closed.</span></span> <span data-ttu-id="22bd6-148">Все элементы должны быть либо самостоятельно закрыть или имеет соответствующего закрывающего тега.</span><span class="sxs-lookup"><span data-stu-id="22bd6-148">All elements must be either self-closing or have a matching end tag.</span></span>
 * <span data-ttu-id="22bd6-149">Не удается преобразовать группу методов «GenericMethod», чтобы, не являющийся делегатом типа «объект».</span><span class="sxs-lookup"><span data-stu-id="22bd6-149">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="22bd6-150">Предполагается ли вызывать этот метод? "</span><span class="sxs-lookup"><span data-stu-id="22bd6-150">Did you intend to invoke the method?\`</span></span> 
 
 <span data-ttu-id="22bd6-151">В следующем примере показана запись правильно этот код.</span><span class="sxs-lookup"><span data-stu-id="22bd6-151">The following markup shows the correct way write this code.</span></span> <span data-ttu-id="22bd6-152">Код записывается как явное выражение:</span><span class="sxs-lookup"><span data-stu-id="22bd6-152">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a><span data-ttu-id="22bd6-153">Кодировка выражения</span><span class="sxs-lookup"><span data-stu-id="22bd6-153">Expression encoding</span></span>

<span data-ttu-id="22bd6-154">Выражения C#, которые были оценены как строка, кодируется в HTML.</span><span class="sxs-lookup"><span data-stu-id="22bd6-154">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="22bd6-155">Выражения C#, которые были оценены как `IHtmlContent` подготавливаются к просмотру непосредственно через `IHtmlContent.WriteTo`.</span><span class="sxs-lookup"><span data-stu-id="22bd6-155">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="22bd6-156">Выражения C#, которые не дают `IHtmlContent` преобразуются в строки по `ToString` и закодированы, прежде чем они отображаются.</span><span class="sxs-lookup"><span data-stu-id="22bd6-156">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="22bd6-157">Код отображает следующий код HTML:</span><span class="sxs-lookup"><span data-stu-id="22bd6-157">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="22bd6-158">HTML-код отображается в обозревателе, как:</span><span class="sxs-lookup"><span data-stu-id="22bd6-158">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="22bd6-159">`HtmlHelper.Raw`выходные данные не закодированы, однако к просмотру в виде HTML-разметка.</span><span class="sxs-lookup"><span data-stu-id="22bd6-159">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="22bd6-160">С помощью `HtmlHelper.Raw` на unsanitized пользователя ввод представляет угрозу безопасности.</span><span class="sxs-lookup"><span data-stu-id="22bd6-160">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="22bd6-161">Ввод данных пользователем может содержать вредоносный JavaScript или использовать.</span><span class="sxs-lookup"><span data-stu-id="22bd6-161">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="22bd6-162">Ввод данных пользователем очистки данных сложно.</span><span class="sxs-lookup"><span data-stu-id="22bd6-162">Sanitizing user input is difficult.</span></span> <span data-ttu-id="22bd6-163">Избегайте использования `HtmlHelper.Raw` с вводимыми пользователем данными.</span><span class="sxs-lookup"><span data-stu-id="22bd6-163">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="22bd6-164">Код отображает следующий код HTML:</span><span class="sxs-lookup"><span data-stu-id="22bd6-164">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="22bd6-165">Блоки кода Razor</span><span class="sxs-lookup"><span data-stu-id="22bd6-165">Razor code blocks</span></span>

<span data-ttu-id="22bd6-166">Блоки кода Razor начинаться с `@` и заключены в `{}`.</span><span class="sxs-lookup"><span data-stu-id="22bd6-166">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="22bd6-167">В отличие от выражения код внутри блоков кода C# не отображаются.</span><span class="sxs-lookup"><span data-stu-id="22bd6-167">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="22bd6-168">Блоки кода и выражения в представлении использующими ту же область и определены в порядке:</span><span class="sxs-lookup"><span data-stu-id="22bd6-168">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

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

<span data-ttu-id="22bd6-169">Код отображает следующий код HTML:</span><span class="sxs-lookup"><span data-stu-id="22bd6-169">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a><span data-ttu-id="22bd6-170">Неявные переходов</span><span class="sxs-lookup"><span data-stu-id="22bd6-170">Implicit transitions</span></span>

<span data-ttu-id="22bd6-171">В блоке кода языка по умолчанию — C#, но страница Razor может перейти обратно в HTML:</span><span class="sxs-lookup"><span data-stu-id="22bd6-171">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="22bd6-172">Явные перехода с разделителями</span><span class="sxs-lookup"><span data-stu-id="22bd6-172">Explicit delimited transition</span></span>

<span data-ttu-id="22bd6-173">Чтобы определить подраздел блок кода, должны обрабатывать HTML, окружить символов для подготовки к просмотру Razor  **\<текст >** тег:</span><span class="sxs-lookup"><span data-stu-id="22bd6-173">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="22bd6-174">Этот способ используется для отрисовки HTML-код, не заключенных в HTML-тега.</span><span class="sxs-lookup"><span data-stu-id="22bd6-174">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="22bd6-175">Без тега HTML или Razor возникает ошибка времени выполнения Razor.</span><span class="sxs-lookup"><span data-stu-id="22bd6-175">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="22bd6-176">**\<Текст >** тег полезен для управления пробелов при подготовке к просмотру содержимого:</span><span class="sxs-lookup"><span data-stu-id="22bd6-176">The **\<text>** tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="22bd6-177">Содержимое между  **\<текст >** визуализации тега.</span><span class="sxs-lookup"><span data-stu-id="22bd6-177">Only the content between the **\<text>** tag is rendered.</span></span> 
* <span data-ttu-id="22bd6-178">Без пробелов до или после  **\<текст >** тег появляется в выходных данных HTML.</span><span class="sxs-lookup"><span data-stu-id="22bd6-178">No whitespace before or after the **\<text>** tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="22bd6-179">Явные строки перехода с @:</span><span class="sxs-lookup"><span data-stu-id="22bd6-179">Explicit Line Transition with @:</span></span>

<span data-ttu-id="22bd6-180">Для подготовки к просмотру остальная часть всю строку как HTML внутри блока кода, используйте `@:` синтаксис:</span><span class="sxs-lookup"><span data-stu-id="22bd6-180">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="22bd6-181">Без `@:` в коде, формируется ошибка выполнения Razor.</span><span class="sxs-lookup"><span data-stu-id="22bd6-181">Without the `@:` in the code, a Razor runtime error is generated.</span></span>

<span data-ttu-id="22bd6-182">Предупреждение: Дополнительный `@` символы в файле Razor может привести к причина ошибки компилятора на инструкции ниже в блоке.</span><span class="sxs-lookup"><span data-stu-id="22bd6-182">Warning: Extra `@` characters in a Razor file can cause cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="22bd6-183">Эти ошибки компилятора могут быть трудными для понимания, так как фактический ошибка возникает до указанной ошибки.</span><span class="sxs-lookup"><span data-stu-id="22bd6-183">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span> <span data-ttu-id="22bd6-184">Эта ошибка проявляется после объединения нескольких явного или неявного выражения в один блок кода.</span><span class="sxs-lookup"><span data-stu-id="22bd6-184">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="22bd6-185">Управляющие структуры</span><span class="sxs-lookup"><span data-stu-id="22bd6-185">Control Structures</span></span>

<span data-ttu-id="22bd6-186">Управляющие структуры являются расширением блоков кода.</span><span class="sxs-lookup"><span data-stu-id="22bd6-186">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="22bd6-187">Все аспекты блоков кода (плавный переход к разметки встроенный C#) также применяются к следующим структурам.</span><span class="sxs-lookup"><span data-stu-id="22bd6-187">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="22bd6-188">Условные выражения @if, else if, else, и@switch</span><span class="sxs-lookup"><span data-stu-id="22bd6-188">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="22bd6-189">`@if`элементы управления, при выполнении кода.</span><span class="sxs-lookup"><span data-stu-id="22bd6-189">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="22bd6-190">`else`и `else if` не требуют `@` символа:</span><span class="sxs-lookup"><span data-stu-id="22bd6-190">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="22bd6-191">Приведенная ниже разметка показано, как использовать инструкцию switch:</span><span class="sxs-lookup"><span data-stu-id="22bd6-191">The following markup shows how to use a switch statement:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="22bd6-192">Циклы @for, @foreach, @while, и @do во время</span><span class="sxs-lookup"><span data-stu-id="22bd6-192">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="22bd6-193">Шаблонные HTML может осуществляться с помощью элемента управления операторы цикла.</span><span class="sxs-lookup"><span data-stu-id="22bd6-193">Templated HTML can be rendered with looping control statements.</span></span> <span data-ttu-id="22bd6-194">Для подготовки к просмотру список лиц:</span><span class="sxs-lookup"><span data-stu-id="22bd6-194">To render a list of people:</span></span>

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

<span data-ttu-id="22bd6-195">Поддерживаются следующие операторы цикла:</span><span class="sxs-lookup"><span data-stu-id="22bd6-195">The following looping statements are supported:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="22bd6-196">Составные@using</span><span class="sxs-lookup"><span data-stu-id="22bd6-196">Compound @using</span></span>

<span data-ttu-id="22bd6-197">В C# `using` инструкция используется для обеспечения удаления объекта.</span><span class="sxs-lookup"><span data-stu-id="22bd6-197">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="22bd6-198">В Razor тот же механизм используется для создания вспомогательных методов HTML, который содержит дополнительное содержимое.</span><span class="sxs-lookup"><span data-stu-id="22bd6-198">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="22bd6-199">В следующем коде вспомогательных методов HTML отрисовки тега формы с `@using` инструкции:</span><span class="sxs-lookup"><span data-stu-id="22bd6-199">In the following code, HTML Helpers render a form tag with the `@using` statement:</span></span>


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

<span data-ttu-id="22bd6-200">Действия на уровне области, которые могут быть выполнены с [вспомогательных функций тегов](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="22bd6-200">Scope-level actions can be performed with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="22bd6-201">@try, catch, finally</span><span class="sxs-lookup"><span data-stu-id="22bd6-201">@try, catch, finally</span></span>

<span data-ttu-id="22bd6-202">Обработка исключений похожа на C#:</span><span class="sxs-lookup"><span data-stu-id="22bd6-202">Exception handling is similar to C#:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="22bd6-203">Razor имеет возможность защитить критические секции с помощью инструкций блокировки:</span><span class="sxs-lookup"><span data-stu-id="22bd6-203">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="22bd6-204">Комментарии</span><span class="sxs-lookup"><span data-stu-id="22bd6-204">Comments</span></span>

<span data-ttu-id="22bd6-205">Razor поддерживает комментарии C# и HTML.</span><span class="sxs-lookup"><span data-stu-id="22bd6-205">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="22bd6-206">Код отображает следующий код HTML:</span><span class="sxs-lookup"><span data-stu-id="22bd6-206">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="22bd6-207">Комментарии Razor удаляются сервером перед отображением веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="22bd6-207">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="22bd6-208">Использует Razor `@*  *@` в качестве разделителя комментариев.</span><span class="sxs-lookup"><span data-stu-id="22bd6-208">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="22bd6-209">Следующий код закомментирована, поэтому сервер не создают разметку:</span><span class="sxs-lookup"><span data-stu-id="22bd6-209">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="22bd6-210">Директивы</span><span class="sxs-lookup"><span data-stu-id="22bd6-210">Directives</span></span>

<span data-ttu-id="22bd6-211">Неявные выражения с следующие зарезервированные ключевые слова представляются директивы Razor `@` символов.</span><span class="sxs-lookup"><span data-stu-id="22bd6-211">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="22bd6-212">Директива обычно изменяет способ представления анализируется или включает различные функциональные возможности.</span><span class="sxs-lookup"><span data-stu-id="22bd6-212">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="22bd6-213">Основные сведения о том, как Razor создает код для представления помогает понять принципы работы директивы.</span><span class="sxs-lookup"><span data-stu-id="22bd6-213">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="22bd6-214">Код создает класс следующего вида:</span><span class="sxs-lookup"><span data-stu-id="22bd6-214">The code generates a class similar to the following:</span></span>

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

<span data-ttu-id="22bd6-215">Далее в этой статье разделе [Просмотр класс Razor C#, созданный для представления](#viewing-the-razor-c-class-generated-for-a-view) способы просмотра этим классом.</span><span class="sxs-lookup"><span data-stu-id="22bd6-215">Later in this article, the section [Viewing the Razor C# class generated for a view](#viewing-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

### <a name="using"></a>@using

<span data-ttu-id="22bd6-216">`@using` Добавляет директивы C# `using` директиву созданного представления:</span><span class="sxs-lookup"><span data-stu-id="22bd6-216">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="22bd6-217">`@model` Директива определяет тип модели, переданный в представлении:</span><span class="sxs-lookup"><span data-stu-id="22bd6-217">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="22bd6-218">В приложении ASP.NET Core MVC, созданных с помощью отдельных учетных записей пользователей *Views/Account/Login.cshtml* представление содержит следующее объявление модели:</span><span class="sxs-lookup"><span data-stu-id="22bd6-218">In an ASP.NET Core MVC app created with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="22bd6-219">Созданный класс наследует `RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="22bd6-219">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="22bd6-220">Предоставляет Razor `Model` свойство для доступа к модели передаются в представление:</span><span class="sxs-lookup"><span data-stu-id="22bd6-220">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="22bd6-221">`@model` Директива определяет тип этого свойства.</span><span class="sxs-lookup"><span data-stu-id="22bd6-221">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="22bd6-222">Указывает директиву `T` в `RazorPage<T>` , созданный класс, который представлении является производным от.</span><span class="sxs-lookup"><span data-stu-id="22bd6-222">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="22bd6-223">Если `@model` директив не будет указано, `Model` свойство относится к типу `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="22bd6-223">If the `@model` directive iisn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="22bd6-224">Значение модели передается из контроллера в представление.</span><span class="sxs-lookup"><span data-stu-id="22bd6-224">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="22bd6-225">Дополнительные сведения см. в разделе [со строго типизированными моделей и @model ключевое слово.</span><span class="sxs-lookup"><span data-stu-id="22bd6-225">For more information, see [Strongly typed models and the @model keyword.</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="22bd6-226">`@inherits` Директива предоставляет полный контроль над класс наследует представления:</span><span class="sxs-lookup"><span data-stu-id="22bd6-226">The `@inherits` directive provides full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="22bd6-227">Следующий код является пользовательским типом Razor страницы:</span><span class="sxs-lookup"><span data-stu-id="22bd6-227">The following code is a custom Razor page type:</span></span>

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="22bd6-228">`CustomText` Отображается в представлении:</span><span class="sxs-lookup"><span data-stu-id="22bd6-228">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="22bd6-229">Код отображает следующий код HTML:</span><span class="sxs-lookup"><span data-stu-id="22bd6-229">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 <span data-ttu-id="22bd6-230">`@model`и `@inherits` может использоваться в одном представлении.</span><span class="sxs-lookup"><span data-stu-id="22bd6-230">`@model` and `@inherits` can be used in the same view.</span></span> <span data-ttu-id="22bd6-231">`@inherits`может быть в *_ViewImports.cshtml* файл, который импортирует представления:</span><span class="sxs-lookup"><span data-stu-id="22bd6-231">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="22bd6-232">Следующий код является примером строго типизированное представление:</span><span class="sxs-lookup"><span data-stu-id="22bd6-232">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="22bd6-233">Если «rick@contoso.com» передается в модель, представление создает следующую разметку HTML:</span><span class="sxs-lookup"><span data-stu-id="22bd6-233">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


<span data-ttu-id="22bd6-234">`@inject` Директива включает страницы Razor, чтобы запустить службу из [контейнер службы](xref:fundamentals/dependency-injection) в представление.</span><span class="sxs-lookup"><span data-stu-id="22bd6-234">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="22bd6-235">Дополнительные сведения см. в разделе [внедрение зависимостей в представления](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="22bd6-235">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="22bd6-236">`@functions` Директива включает страницы Razor для добавления содержимого на уровне функций в представлении:</span><span class="sxs-lookup"><span data-stu-id="22bd6-236">The `@functions` directive enables a Razor Page to add function-level content to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="22bd6-237">Пример:</span><span class="sxs-lookup"><span data-stu-id="22bd6-237">For example:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="22bd6-238">Код формирует следующую разметку HTML:</span><span class="sxs-lookup"><span data-stu-id="22bd6-238">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="22bd6-239">Следующий код — это созданный класс Razor C#:</span><span class="sxs-lookup"><span data-stu-id="22bd6-239">The following code is the generated Razor C# class:</span></span>

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

<span data-ttu-id="22bd6-240">`@section` Директива используется в сочетании с [макета](xref:mvc/views/layout) Включение представления для отображения содержимого в различные части HTML-страницы.</span><span class="sxs-lookup"><span data-stu-id="22bd6-240">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="22bd6-241">Дополнительные сведения см. в разделе [разделы](xref:mvc/views/layout#layout-sections-label).</span><span class="sxs-lookup"><span data-stu-id="22bd6-241">For more information, see [Sections](xref:mvc/views/layout#layout-sections-label).</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="22bd6-242">Вспомогательных функций тегов</span><span class="sxs-lookup"><span data-stu-id="22bd6-242">Tag Helpers</span></span>

<span data-ttu-id="22bd6-243">Существует три директивы, которые относятся к [вспомогательных функций тегов](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="22bd6-243">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="22bd6-244">Директива</span><span class="sxs-lookup"><span data-stu-id="22bd6-244">Directive</span></span> | <span data-ttu-id="22bd6-245">Функция</span><span class="sxs-lookup"><span data-stu-id="22bd6-245">Function</span></span> |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="22bd6-246">Делает доступными для представления вспомогательных функций тегов.</span><span class="sxs-lookup"><span data-stu-id="22bd6-246">Makes Tag Helpers available to a view.</span></span> |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="22bd6-247">Удаляет ранее добавленный из представления в виде вспомогательных функций тегов.</span><span class="sxs-lookup"><span data-stu-id="22bd6-247">Removes Tag Helpers previously added from a view.</span></span> |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="22bd6-248">Указывает префикс тега для включения поддержка вспомогательных функций тегов и явно объявить использование вспомогательного тег.</span><span class="sxs-lookup"><span data-stu-id="22bd6-248">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="22bd6-249">Razor зарезервированные ключевые слова</span><span class="sxs-lookup"><span data-stu-id="22bd6-249">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="22bd6-250">Ключевые слова Razor</span><span class="sxs-lookup"><span data-stu-id="22bd6-250">Razor keywords</span></span>

* <span data-ttu-id="22bd6-251">страница (требуется Core ASP.NET 2.0 и более поздние версии)</span><span class="sxs-lookup"><span data-stu-id="22bd6-251">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="22bd6-252">функции</span><span class="sxs-lookup"><span data-stu-id="22bd6-252">functions</span></span>
* <span data-ttu-id="22bd6-253">наследует</span><span class="sxs-lookup"><span data-stu-id="22bd6-253">inherits</span></span>
* <span data-ttu-id="22bd6-254">model</span><span class="sxs-lookup"><span data-stu-id="22bd6-254">model</span></span>
* <span data-ttu-id="22bd6-255">section</span><span class="sxs-lookup"><span data-stu-id="22bd6-255">section</span></span>
* <span data-ttu-id="22bd6-256">Вспомогательный (не поддерживаемых ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="22bd6-256">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="22bd6-257">Ключевые слова Razor экранируются с `@(Razor Keyword)` (например, `@(functions)`).</span><span class="sxs-lookup"><span data-stu-id="22bd6-257">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="22bd6-258">Ключевые слова C# Razor</span><span class="sxs-lookup"><span data-stu-id="22bd6-258">C# Razor keywords</span></span>

* <span data-ttu-id="22bd6-259">регистр знаков</span><span class="sxs-lookup"><span data-stu-id="22bd6-259">case</span></span>
* <span data-ttu-id="22bd6-260">do</span><span class="sxs-lookup"><span data-stu-id="22bd6-260">do</span></span>
* <span data-ttu-id="22bd6-261">default</span><span class="sxs-lookup"><span data-stu-id="22bd6-261">default</span></span>
* <span data-ttu-id="22bd6-262">for</span><span class="sxs-lookup"><span data-stu-id="22bd6-262">for</span></span>
* <span data-ttu-id="22bd6-263">foreach</span><span class="sxs-lookup"><span data-stu-id="22bd6-263">foreach</span></span>
* <span data-ttu-id="22bd6-264">if</span><span class="sxs-lookup"><span data-stu-id="22bd6-264">if</span></span>
* <span data-ttu-id="22bd6-265">else</span><span class="sxs-lookup"><span data-stu-id="22bd6-265">else</span></span>
* <span data-ttu-id="22bd6-266">lock</span><span class="sxs-lookup"><span data-stu-id="22bd6-266">lock</span></span>
* <span data-ttu-id="22bd6-267">switch</span><span class="sxs-lookup"><span data-stu-id="22bd6-267">switch</span></span>
* <span data-ttu-id="22bd6-268">try</span><span class="sxs-lookup"><span data-stu-id="22bd6-268">try</span></span>
* <span data-ttu-id="22bd6-269">catch</span><span class="sxs-lookup"><span data-stu-id="22bd6-269">catch</span></span>
* <span data-ttu-id="22bd6-270">finally</span><span class="sxs-lookup"><span data-stu-id="22bd6-270">finally</span></span>
* <span data-ttu-id="22bd6-271">использование</span><span class="sxs-lookup"><span data-stu-id="22bd6-271">using</span></span>
* <span data-ttu-id="22bd6-272">while</span><span class="sxs-lookup"><span data-stu-id="22bd6-272">while</span></span>

<span data-ttu-id="22bd6-273">Ключевые слова C# Razor необходимо экранировать двойные с `@(@C# Razor Keyword)` (например, `@(@case)`).</span><span class="sxs-lookup"><span data-stu-id="22bd6-273">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="22bd6-274">Первый `@` экранирует анализатор Razor.</span><span class="sxs-lookup"><span data-stu-id="22bd6-274">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="22bd6-275">Второй `@` экранирует анализатор C#.</span><span class="sxs-lookup"><span data-stu-id="22bd6-275">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="22bd6-276">Зарезервированные ключевые слова, не используется в Razor</span><span class="sxs-lookup"><span data-stu-id="22bd6-276">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="22bd6-277">namespace</span><span class="sxs-lookup"><span data-stu-id="22bd6-277">namespace</span></span>
* <span data-ttu-id="22bd6-278">класс</span><span class="sxs-lookup"><span data-stu-id="22bd6-278">class</span></span>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="22bd6-279">Просмотр класса Razor C#, созданного для представления</span><span class="sxs-lookup"><span data-stu-id="22bd6-279">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="22bd6-280">Добавьте следующий класс в проект ASP.NET MVC основных компонентов:</span><span class="sxs-lookup"><span data-stu-id="22bd6-280">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="22bd6-281">Переопределить `RazorTemplateEngine` добавленные MVC с `CustomTemplateEngine` класса:</span><span class="sxs-lookup"><span data-stu-id="22bd6-281">Override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="22bd6-282">Точка останова `return csharpDocument` инструкция `CustomTemplateEngine`.</span><span class="sxs-lookup"><span data-stu-id="22bd6-282">Set a break point on the `return csharpDocument` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="22bd6-283">Когда выполнение программы остановится в точке останова, просматривать значения `generatedCode`.</span><span class="sxs-lookup"><span data-stu-id="22bd6-283">When program execution stops at the break point, view the value of `generatedCode`.</span></span>

![Представление визуализатор текста generatedCode](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="22bd6-285">Представление уточняющих запросов и чувствительности к регистру</span><span class="sxs-lookup"><span data-stu-id="22bd6-285">View lookups and case sensitivity</span></span>

<span data-ttu-id="22bd6-286">Обработчик представлений Razor выполняет поиск с учетом регистра для представлений.</span><span class="sxs-lookup"><span data-stu-id="22bd6-286">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="22bd6-287">Однако фактические уточняющего запроса определяется базовая файловая система:</span><span class="sxs-lookup"><span data-stu-id="22bd6-287">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="22bd6-288">На основе исходного файла:</span><span class="sxs-lookup"><span data-stu-id="22bd6-288">File based source:</span></span> 
  * <span data-ttu-id="22bd6-289">В операционных системах без учета регистра файловые системы (например, Windows) поиск поставщика физических файлов зависят от регистра.</span><span class="sxs-lookup"><span data-stu-id="22bd6-289">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="22bd6-290">Например `return View("Test")` приводит к соответствий для */Views/Home/Test.cshtml*, */Views/home/test.cshtml*и любой другой тип регистра.</span><span class="sxs-lookup"><span data-stu-id="22bd6-290">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="22bd6-291">В системах с учетом регистра файла (например, Linux, OSX и с `EmbeddedFileProvider`), уточняющие запросы выполняются с учетом регистра.</span><span class="sxs-lookup"><span data-stu-id="22bd6-291">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="22bd6-292">Например `return View("Test")` специально совпадений */Views/Home/Test.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="22bd6-292">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="22bd6-293">Предварительно скомпилированные представлений: С основными ASP.NET 2.0 и более поздних, поиске предкомпилированного представления без учета регистра для всех операционных систем.</span><span class="sxs-lookup"><span data-stu-id="22bd6-293">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="22bd6-294">Поведение идентично поведение поставщика физического файла в Windows.</span><span class="sxs-lookup"><span data-stu-id="22bd6-294">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="22bd6-295">Если два представления предкомпилированного отличаются только регистром, результатом поиска является недетерминированным.</span><span class="sxs-lookup"><span data-stu-id="22bd6-295">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="22bd6-296">Разработчики могут совпадает с регистром имена файлов и каталогов на регистр:</span><span class="sxs-lookup"><span data-stu-id="22bd6-296">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

    * <span data-ttu-id="22bd6-297">Имена областей, контроллера и действия.</span><span class="sxs-lookup"><span data-stu-id="22bd6-297">Area, controller, and action names.</span></span> 
    * <span data-ttu-id="22bd6-298">Страниц Razor.</span><span class="sxs-lookup"><span data-stu-id="22bd6-298">Razor Pages.</span></span>
    
<span data-ttu-id="22bd6-299">Регистра гарантирует, что развертываний найти их представления независимо от используемой файловой системы.</span><span class="sxs-lookup"><span data-stu-id="22bd6-299">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>
