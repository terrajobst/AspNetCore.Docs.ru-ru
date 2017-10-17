---
title: "Справочник по синтаксису Razor для ASP.NET Core"
author: rick-anderson
description: "Дополнительные сведения о синтаксиса Razor разметки для внедрения серверного кода в веб-страниц."
keywords: "Директивы Razor ASP.NET Core, Razor,"
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: 743c42b26c62d0e24b5d5b487b3154bc249fcff4
ms.sourcegitcommit: a873f862c8e68b2cf2998aaed3dddd93eeba9e0f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/17/2017
---
# <a name="razor-syntax-for-aspnet-core"></a><span data-ttu-id="330ff-104">Синтаксис Razor ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="330ff-104">Razor syntax for ASP.NET Core</span></span>

<span data-ttu-id="330ff-105">По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Latham Люк](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), и [Dan Vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="330ff-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex),  [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="330ff-106">Razor является синтаксис разметки для внедрения серверного кода в веб-страниц.</span><span class="sxs-lookup"><span data-stu-id="330ff-106">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="330ff-107">Синтаксис Razor содержит Razor разметки, C# и HTML.</span><span class="sxs-lookup"><span data-stu-id="330ff-107">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="330ff-108">Файлы, содержащие Razor, обычно имеют *.cshtml* расширение имени файла.</span><span class="sxs-lookup"><span data-stu-id="330ff-108">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="330ff-109">Подготовка к просмотру HTML</span><span class="sxs-lookup"><span data-stu-id="330ff-109">Rendering HTML</span></span>

<span data-ttu-id="330ff-110">Язык по умолчанию Razor — HTML.</span><span class="sxs-lookup"><span data-stu-id="330ff-110">The default Razor language is HTML.</span></span> <span data-ttu-id="330ff-111">Подготовки отчетов HTML из разметки Razor ничем не отличается от визуализации HTML из HTML-файл.</span><span class="sxs-lookup"><span data-stu-id="330ff-111">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span>  <span data-ttu-id="330ff-112">HTML-разметку в *.cshtml* файлах Razor визуализируется с сервера без изменений.</span><span class="sxs-lookup"><span data-stu-id="330ff-112">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="330ff-113">Синтаксис Razor</span><span class="sxs-lookup"><span data-stu-id="330ff-113">Razor syntax</span></span>

<span data-ttu-id="330ff-114">Razor поддерживает C# и использует `@` символ переход из HTML для C#.</span><span class="sxs-lookup"><span data-stu-id="330ff-114">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="330ff-115">Razor вычисляет выражения C# и отображает их в выходных данных HTML.</span><span class="sxs-lookup"><span data-stu-id="330ff-115">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="330ff-116">Когда `@` следуют символ [Razor зарезервированное ключевое слово](#razor-reserved-keywords), он переходит в Razor разметку.</span><span class="sxs-lookup"><span data-stu-id="330ff-116">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="330ff-117">В противном случае он переходит в обычный C#.</span><span class="sxs-lookup"><span data-stu-id="330ff-117">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="330ff-118">Чтобы escape `@` символ в разметке Razor, использовать второй `@` символа:</span><span class="sxs-lookup"><span data-stu-id="330ff-118">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="330ff-119">Код отображается в формате HTML с помощью одного `@` символа:</span><span class="sxs-lookup"><span data-stu-id="330ff-119">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="330ff-120">HTML-атрибутов и содержимого, содержащий адреса электронной почты не обрабатывают `@` символ как символ перехода.</span><span class="sxs-lookup"><span data-stu-id="330ff-120">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="330ff-121">В следующем примере адреса электронной почты не изменятся путем синтаксического анализа Razor:</span><span class="sxs-lookup"><span data-stu-id="330ff-121">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="330ff-122">Неявные выражения Razor</span><span class="sxs-lookup"><span data-stu-id="330ff-122">Implicit Razor expressions</span></span>

<span data-ttu-id="330ff-123">Неявные Razor выражения начинаются со `@` вместе с кодом C#:</span><span class="sxs-lookup"><span data-stu-id="330ff-123">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="330ff-124">За исключением C# `await` ключевое слово, неявные выражения не должно содержать пробелов.</span><span class="sxs-lookup"><span data-stu-id="330ff-124">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="330ff-125">Если оператор C# есть очистить окончания, можно их объединения пространств:</span><span class="sxs-lookup"><span data-stu-id="330ff-125">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="330ff-126">Неявные выражения **не** содержать универсальные шаблоны C#, как символы в квадратных скобках (`<>`) интерпретируются как HTML-тега.</span><span class="sxs-lookup"><span data-stu-id="330ff-126">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="330ff-127">Следующий код является **не** допустимым:</span><span class="sxs-lookup"><span data-stu-id="330ff-127">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="330ff-128">Предыдущий код вызывает ошибку компилятора, примерно следующего вида:</span><span class="sxs-lookup"><span data-stu-id="330ff-128">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="330ff-129">Элемент «int» не была закрыта.</span><span class="sxs-lookup"><span data-stu-id="330ff-129">The "int" element was not closed.</span></span>  <span data-ttu-id="330ff-130">Все элементы должны быть либо самостоятельно закрыть или имеет соответствующего закрывающего тега.</span><span class="sxs-lookup"><span data-stu-id="330ff-130">All elements must be either self-closing or have a matching end tag.</span></span>
 *  <span data-ttu-id="330ff-131">Не удается преобразовать группу методов «GenericMethod», чтобы, не являющийся делегатом типа «объект».</span><span class="sxs-lookup"><span data-stu-id="330ff-131">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="330ff-132">Предполагается ли вызывать этот метод? "</span><span class="sxs-lookup"><span data-stu-id="330ff-132">Did you intend to invoke the method?\`</span></span> 
 
<span data-ttu-id="330ff-133">Универсальный метод вызывает должны быть заключены в [явное выражение Razor](#explicit-razor-expressions) или [блок кода Razor](#razor-code-blocks).</span><span class="sxs-lookup"><span data-stu-id="330ff-133">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span> <span data-ttu-id="330ff-134">Это ограничение не применяется к *.vbhtml* файлы Razor, поскольку синтаксис Visual Basic помещает параметры универсального типа, вместо квадратных в круглые скобки.</span><span class="sxs-lookup"><span data-stu-id="330ff-134">This restriction doesn't apply to *.vbhtml* Razor files because Visual Basic syntax places parentheses around generic type parameters instead of brackets.</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="330ff-135">Прямые выражения Razor</span><span class="sxs-lookup"><span data-stu-id="330ff-135">Explicit Razor expressions</span></span>

<span data-ttu-id="330ff-136">Явные Razor выражения состоят из `@` символ со сбалансированной скобками.</span><span class="sxs-lookup"><span data-stu-id="330ff-136">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="330ff-137">Для подготовки к просмотру время прошлой неделе, используется следующая разметка Razor:</span><span class="sxs-lookup"><span data-stu-id="330ff-137">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="330ff-138">Любое содержимое в `@()` скобки вычисляется и отображается в выходных данных.</span><span class="sxs-lookup"><span data-stu-id="330ff-138">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="330ff-139">Неявные выражения, описанные в предыдущем разделе, обычно не может содержать пробелы.</span><span class="sxs-lookup"><span data-stu-id="330ff-139">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="330ff-140">В следующем коде одну неделю не вычитается из текущего времени:</span><span class="sxs-lookup"><span data-stu-id="330ff-140">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="330ff-141">Код отображает следующий код HTML:</span><span class="sxs-lookup"><span data-stu-id="330ff-141">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="330ff-142">Явные выражения могут использоваться для объединения текста с результатом выражения:</span><span class="sxs-lookup"><span data-stu-id="330ff-142">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="330ff-143">Без явного выражение `<p>Age@joe.Age</p>` рассматривается как адрес электронной почты и `<p>Age@joe.Age</p>` подготавливается к просмотру.</span><span class="sxs-lookup"><span data-stu-id="330ff-143">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="330ff-144">Если они написаны как выражение явного `<p>Age33</p>` подготавливается к просмотру.</span><span class="sxs-lookup"><span data-stu-id="330ff-144">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>


<span data-ttu-id="330ff-145">Прямые выражения можно использовать для отображения выходных данных из универсальных методов в *.cshtml* файлов.</span><span class="sxs-lookup"><span data-stu-id="330ff-145">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="330ff-146">В выражении неявное символов в квадратных скобках (`<>`) интерпретируются как HTML-тега.</span><span class="sxs-lookup"><span data-stu-id="330ff-146">In an implicit expression, the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="330ff-147">Следующая разметка является **не** допустимым Razor:</span><span class="sxs-lookup"><span data-stu-id="330ff-147">The following markup is **not** valid Razor:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="330ff-148">Предыдущий код вызывает ошибку компилятора, примерно следующего вида:</span><span class="sxs-lookup"><span data-stu-id="330ff-148">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="330ff-149">Элемент «int» не была закрыта.</span><span class="sxs-lookup"><span data-stu-id="330ff-149">The "int" element was not closed.</span></span>  <span data-ttu-id="330ff-150">Все элементы должны быть либо самостоятельно закрыть или имеет соответствующего закрывающего тега.</span><span class="sxs-lookup"><span data-stu-id="330ff-150">All elements must be either self-closing or have a matching end tag.</span></span>
 *  <span data-ttu-id="330ff-151">Не удается преобразовать группу методов «GenericMethod», чтобы, не являющийся делегатом типа «объект».</span><span class="sxs-lookup"><span data-stu-id="330ff-151">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="330ff-152">Предполагается ли вызывать этот метод? "</span><span class="sxs-lookup"><span data-stu-id="330ff-152">Did you intend to invoke the method?\`</span></span> 
 
 <span data-ttu-id="330ff-153">В следующем примере показана запись правильно этот код.</span><span class="sxs-lookup"><span data-stu-id="330ff-153">The following markup shows the correct way write this code.</span></span>  <span data-ttu-id="330ff-154">Код записывается как явное выражение:</span><span class="sxs-lookup"><span data-stu-id="330ff-154">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

<span data-ttu-id="330ff-155">Примечание: это ограничение не применяется к *.vbhtml* файлах Razor.</span><span class="sxs-lookup"><span data-stu-id="330ff-155">Note: this restriction doesn't apply to *.vbhtml* Razor files.</span></span>  <span data-ttu-id="330ff-156">С *.vbhtml* круглые скобки вокруг параметров универсального типа, вместо квадратных помещает файлы Razor, синтаксис Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="330ff-156">With *.vbhtml* Razor files, Visual Basic syntax places parentheses around generic type parameters instead of brackets.</span></span>

## <a name="expression-encoding"></a><span data-ttu-id="330ff-157">Кодировка выражения</span><span class="sxs-lookup"><span data-stu-id="330ff-157">Expression encoding</span></span>

<span data-ttu-id="330ff-158">Выражения C#, которые были оценены как строка, кодируется в HTML.</span><span class="sxs-lookup"><span data-stu-id="330ff-158">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="330ff-159">Выражения C#, которые были оценены как `IHtmlContent` подготавливаются к просмотру непосредственно через `IHtmlContent.WriteTo`.</span><span class="sxs-lookup"><span data-stu-id="330ff-159">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="330ff-160">Выражения C#, которые не дают `IHtmlContent` преобразуются в строки по `ToString` и закодированы, прежде чем они отображаются.</span><span class="sxs-lookup"><span data-stu-id="330ff-160">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="330ff-161">Код отображает следующий код HTML:</span><span class="sxs-lookup"><span data-stu-id="330ff-161">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="330ff-162">HTML-код отображается в обозревателе, как:</span><span class="sxs-lookup"><span data-stu-id="330ff-162">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="330ff-163">`HtmlHelper.Raw`выходные данные не закодированы, однако к просмотру в виде HTML-разметка.</span><span class="sxs-lookup"><span data-stu-id="330ff-163">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="330ff-164">С помощью `HtmlHelper.Raw` на unsanitized пользователя ввод представляет угрозу безопасности.</span><span class="sxs-lookup"><span data-stu-id="330ff-164">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="330ff-165">Ввод данных пользователем может содержать вредоносный JavaScript или использовать.</span><span class="sxs-lookup"><span data-stu-id="330ff-165">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="330ff-166">Ввод данных пользователем очистки данных сложно.</span><span class="sxs-lookup"><span data-stu-id="330ff-166">Sanitizing user input is difficult.</span></span> <span data-ttu-id="330ff-167">Избегайте использования `HtmlHelper.Raw` с вводимыми пользователем данными.</span><span class="sxs-lookup"><span data-stu-id="330ff-167">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="330ff-168">Код отображает следующий код HTML:</span><span class="sxs-lookup"><span data-stu-id="330ff-168">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="330ff-169">Блоки кода Razor</span><span class="sxs-lookup"><span data-stu-id="330ff-169">Razor code blocks</span></span>

<span data-ttu-id="330ff-170">Блоки кода Razor начинаться с `@` и заключены в `{}`.</span><span class="sxs-lookup"><span data-stu-id="330ff-170">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="330ff-171">В отличие от выражения код внутри блоков кода C# не отображаются.</span><span class="sxs-lookup"><span data-stu-id="330ff-171">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="330ff-172">Блоки кода и выражения в представлении использующими ту же область и определены в порядке:</span><span class="sxs-lookup"><span data-stu-id="330ff-172">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

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

<span data-ttu-id="330ff-173">Код отображает следующий код HTML:</span><span class="sxs-lookup"><span data-stu-id="330ff-173">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a><span data-ttu-id="330ff-174">Неявные переходов</span><span class="sxs-lookup"><span data-stu-id="330ff-174">Implicit transitions</span></span>

<span data-ttu-id="330ff-175">В блоке кода языка по умолчанию — C#, но страница Razor может перейти обратно в HTML:</span><span class="sxs-lookup"><span data-stu-id="330ff-175">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="330ff-176">Явные перехода с разделителями</span><span class="sxs-lookup"><span data-stu-id="330ff-176">Explicit delimited transition</span></span>

<span data-ttu-id="330ff-177">Чтобы определить подраздел блок кода, должны обрабатывать HTML, окружить символов для подготовки к просмотру Razor  **\<текст >** тег:</span><span class="sxs-lookup"><span data-stu-id="330ff-177">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="330ff-178">Этот способ используется для отрисовки HTML-код, не заключенных в HTML-тега.</span><span class="sxs-lookup"><span data-stu-id="330ff-178">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="330ff-179">Без тега HTML или Razor возникает ошибка времени выполнения Razor.</span><span class="sxs-lookup"><span data-stu-id="330ff-179">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="330ff-180"> **\<Текст >** тег полезен для управления пробелов при подготовке к просмотру содержимого:</span><span class="sxs-lookup"><span data-stu-id="330ff-180">The **\<text>** tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="330ff-181">Содержимое между  **\<текст >** визуализации тега.</span><span class="sxs-lookup"><span data-stu-id="330ff-181">Only the content between the **\<text>** tag is rendered.</span></span> 
* <span data-ttu-id="330ff-182">Без пробелов до или после  **\<текст >** тег появляется в выходных данных HTML.</span><span class="sxs-lookup"><span data-stu-id="330ff-182">No whitespace before or after the **\<text>** tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="330ff-183">Явные строки перехода с @:</span><span class="sxs-lookup"><span data-stu-id="330ff-183">Explicit Line Transition with @:</span></span>

<span data-ttu-id="330ff-184">Для подготовки к просмотру остальная часть всю строку как HTML внутри блока кода, используйте `@:` синтаксис:</span><span class="sxs-lookup"><span data-stu-id="330ff-184">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="330ff-185">Без `@:` в коде, формируется ошибка выполнения Razor.</span><span class="sxs-lookup"><span data-stu-id="330ff-185">Without the `@:` in the code,  a Razor runtime error is generated.</span></span>

<span data-ttu-id="330ff-186">Предупреждение: Дополнительный `@` символы в файле Razor может привести к причина ошибки компилятора на инструкции ниже в блоке.</span><span class="sxs-lookup"><span data-stu-id="330ff-186">Warning: Extra `@` characters in a Razor file can cause  cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="330ff-187">Эти ошибки компилятора могут быть трудными для понимания, так как фактический ошибка возникает до указанной ошибки.</span><span class="sxs-lookup"><span data-stu-id="330ff-187">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span>  <span data-ttu-id="330ff-188">Эта ошибка проявляется после объединения нескольких явного или неявного выражения в один блок кода.</span><span class="sxs-lookup"><span data-stu-id="330ff-188">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="330ff-189">Управляющие структуры</span><span class="sxs-lookup"><span data-stu-id="330ff-189">Control Structures</span></span>

<span data-ttu-id="330ff-190">Управляющие структуры являются расширением блоков кода.</span><span class="sxs-lookup"><span data-stu-id="330ff-190">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="330ff-191">Все аспекты блоков кода (плавный переход к разметки встроенный C#) также применяются к следующим структурам.</span><span class="sxs-lookup"><span data-stu-id="330ff-191">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="330ff-192">Условные выражения @if, else if, else, и@switch</span><span class="sxs-lookup"><span data-stu-id="330ff-192">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="330ff-193">`@if`элементы управления, при выполнении кода.</span><span class="sxs-lookup"><span data-stu-id="330ff-193">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="330ff-194">`else`и `else if` не требуют `@` символа:</span><span class="sxs-lookup"><span data-stu-id="330ff-194">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="330ff-195">Приведенная ниже разметка показано, как использовать инструкцию switch:</span><span class="sxs-lookup"><span data-stu-id="330ff-195">The following markup shows how to use a switch statement:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="330ff-196">Циклы @for, @foreach, @while, и @do во время</span><span class="sxs-lookup"><span data-stu-id="330ff-196">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="330ff-197">Шаблонные HTML может осуществляться с помощью элемента управления операторы цикла.</span><span class="sxs-lookup"><span data-stu-id="330ff-197">Templated HTML can be rendered with looping control statements.</span></span>  <span data-ttu-id="330ff-198">Для подготовки к просмотру список лиц:</span><span class="sxs-lookup"><span data-stu-id="330ff-198">To render a list of people:</span></span>

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

<span data-ttu-id="330ff-199">Поддерживаются следующие операторы цикла:</span><span class="sxs-lookup"><span data-stu-id="330ff-199">The following looping statements are supported:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="330ff-200">Составные@using</span><span class="sxs-lookup"><span data-stu-id="330ff-200">Compound @using</span></span>

<span data-ttu-id="330ff-201">В C# `using` инструкция используется для обеспечения удаления объекта.</span><span class="sxs-lookup"><span data-stu-id="330ff-201">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="330ff-202">В Razor тот же механизм используется для создания вспомогательных методов HTML, который содержит дополнительное содержимое.</span><span class="sxs-lookup"><span data-stu-id="330ff-202">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="330ff-203">В следующем коде вспомогательных методов HTML отрисовки тега формы с `@using` инструкции:</span><span class="sxs-lookup"><span data-stu-id="330ff-203">In the following code, HTML Helpers render a form tag with the `@using` statement:</span></span>


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

<span data-ttu-id="330ff-204">Действия на уровне области, которые могут быть выполнены с [вспомогательных функций тегов](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="330ff-204">Scope-level actions can be performed with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="330ff-205">@try, catch, finally</span><span class="sxs-lookup"><span data-stu-id="330ff-205">@try, catch, finally</span></span>

<span data-ttu-id="330ff-206">Обработка исключений похожа на C#:</span><span class="sxs-lookup"><span data-stu-id="330ff-206">Exception handling is similar to C#:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="330ff-207">Razor имеет возможность защитить критические секции с помощью инструкций блокировки:</span><span class="sxs-lookup"><span data-stu-id="330ff-207">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="330ff-208">Комментарии</span><span class="sxs-lookup"><span data-stu-id="330ff-208">Comments</span></span>

<span data-ttu-id="330ff-209">Razor поддерживает комментарии C# и HTML.</span><span class="sxs-lookup"><span data-stu-id="330ff-209">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="330ff-210">Код отображает следующий код HTML:</span><span class="sxs-lookup"><span data-stu-id="330ff-210">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="330ff-211">Комментарии Razor удаляются сервером перед отображением веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="330ff-211">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="330ff-212">Использует Razor `@*  *@` в качестве разделителя комментариев.</span><span class="sxs-lookup"><span data-stu-id="330ff-212">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="330ff-213">Следующий код закомментирована, поэтому сервер не создают разметку:</span><span class="sxs-lookup"><span data-stu-id="330ff-213">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="330ff-214">Директивы</span><span class="sxs-lookup"><span data-stu-id="330ff-214">Directives</span></span>

<span data-ttu-id="330ff-215">Неявные выражения с следующие зарезервированные ключевые слова представляются директивы Razor `@` символов.</span><span class="sxs-lookup"><span data-stu-id="330ff-215">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="330ff-216">Директива обычно изменяет способ представления анализируется или включает различные функциональные возможности.</span><span class="sxs-lookup"><span data-stu-id="330ff-216">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="330ff-217">Основные сведения о том, как Razor создает код для представления помогает понять принципы работы директивы.</span><span class="sxs-lookup"><span data-stu-id="330ff-217">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="330ff-218">Код создает класс следующего вида:</span><span class="sxs-lookup"><span data-stu-id="330ff-218">The code generates a class similar to the following:</span></span>

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

<span data-ttu-id="330ff-219">Далее в этой статье разделе [Просмотр класс Razor C#, созданный для представления](#viewing-the-razor-c-class-generated-for-a-view) способы просмотра этим классом.</span><span class="sxs-lookup"><span data-stu-id="330ff-219">Later in this article, the section [Viewing the Razor C# class generated for a view](#viewing-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

### <a name="using"></a>@using

<span data-ttu-id="330ff-220">`@using` Добавляет директивы C# `using` директиву созданного представления:</span><span class="sxs-lookup"><span data-stu-id="330ff-220">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="330ff-221">`@model` Директива определяет тип модели, переданный в представлении:</span><span class="sxs-lookup"><span data-stu-id="330ff-221">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="330ff-222">В приложении ASP.NET Core MVC, созданных с помощью отдельных учетных записей пользователей *Views/Account/Login.cshtml* представление содержит следующее объявление модели:</span><span class="sxs-lookup"><span data-stu-id="330ff-222">In an ASP.NET Core MVC app created with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="330ff-223">Созданный класс наследует `RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="330ff-223">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="330ff-224">Предоставляет Razor `Model` свойство для доступа к модели передаются в представление:</span><span class="sxs-lookup"><span data-stu-id="330ff-224">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="330ff-225">`@model` Директива определяет тип этого свойства.</span><span class="sxs-lookup"><span data-stu-id="330ff-225">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="330ff-226">Указывает директиву `T` в `RazorPage<T>` , созданный класс, который представлении является производным от.</span><span class="sxs-lookup"><span data-stu-id="330ff-226">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="330ff-227">Если `@model` директив не будет указано, `Model` свойство относится к типу `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="330ff-227">If  the `@model` directive iisn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="330ff-228">Значение модели передается из контроллера в представление.</span><span class="sxs-lookup"><span data-stu-id="330ff-228">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="330ff-229">Дополнительные сведения см. в разделе [со строго типизированными моделей и @model ключевое слово.</span><span class="sxs-lookup"><span data-stu-id="330ff-229">For more information, see [Strongly typed models and the @model keyword.</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="330ff-230">`@inherits` Директива предоставляет полный контроль над класс наследует представления:</span><span class="sxs-lookup"><span data-stu-id="330ff-230">The `@inherits` directive provides  full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="330ff-231">Следующий код является пользовательским типом Razor страницы:</span><span class="sxs-lookup"><span data-stu-id="330ff-231">The following code is a custom Razor page type:</span></span>

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="330ff-232">`CustomText` Отображается в представлении:</span><span class="sxs-lookup"><span data-stu-id="330ff-232">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="330ff-233">Код отображает следующий код HTML:</span><span class="sxs-lookup"><span data-stu-id="330ff-233">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 <span data-ttu-id="330ff-234">`@model`и `@inherits` может использоваться в одном представлении.</span><span class="sxs-lookup"><span data-stu-id="330ff-234">`@model` and `@inherits` can be used in the same view.</span></span>  <span data-ttu-id="330ff-235">`@inherits`может быть в *_ViewImports.cshtml* файл, который импортирует представления:</span><span class="sxs-lookup"><span data-stu-id="330ff-235">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="330ff-236">Следующий код является примером строго типизированное представление:</span><span class="sxs-lookup"><span data-stu-id="330ff-236">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="330ff-237">Если «rick@contoso.com» передается в модель, представление создает следующую разметку HTML:</span><span class="sxs-lookup"><span data-stu-id="330ff-237">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


<span data-ttu-id="330ff-238">`@inject` Директива включает страницы Razor, чтобы запустить службу из [контейнер службы](xref:fundamentals/dependency-injection) в представление.</span><span class="sxs-lookup"><span data-stu-id="330ff-238">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="330ff-239">Дополнительные сведения см. в разделе [внедрение зависимостей в представления](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="330ff-239">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="330ff-240">`@functions` Директива включает страницы Razor для добавления содержимого на уровне функций в представлении:</span><span class="sxs-lookup"><span data-stu-id="330ff-240">The `@functions` directive enables a Razor Page to add function-level content to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="330ff-241">Пример:</span><span class="sxs-lookup"><span data-stu-id="330ff-241">For example:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="330ff-242">Код формирует следующую разметку HTML:</span><span class="sxs-lookup"><span data-stu-id="330ff-242">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="330ff-243">Следующий код — это созданный класс Razor C#:</span><span class="sxs-lookup"><span data-stu-id="330ff-243">The following code is the generated Razor C# class:</span></span>

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

<span data-ttu-id="330ff-244">`@section` Директива используется в сочетании с [макета](xref:mvc/views/layout) Включение представления для отображения содержимого в различные части HTML-страницы.</span><span class="sxs-lookup"><span data-stu-id="330ff-244">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="330ff-245">Дополнительные сведения см. в разделе [разделы](xref:mvc/views/layout#layout-sections-label).</span><span class="sxs-lookup"><span data-stu-id="330ff-245">For more information, see [Sections](xref:mvc/views/layout#layout-sections-label).</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="330ff-246">Вспомогательных функций тегов</span><span class="sxs-lookup"><span data-stu-id="330ff-246">Tag Helpers</span></span>

<span data-ttu-id="330ff-247">Существует три директивы, которые относятся к [вспомогательных функций тегов](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="330ff-247">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="330ff-248">Директива</span><span class="sxs-lookup"><span data-stu-id="330ff-248">Directive</span></span> | <span data-ttu-id="330ff-249">Функция</span><span class="sxs-lookup"><span data-stu-id="330ff-249">Function</span></span> |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="330ff-250">Делает доступными для представления вспомогательных функций тегов.</span><span class="sxs-lookup"><span data-stu-id="330ff-250">Makes Tag Helpers available to a view.</span></span> |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="330ff-251">Удаляет ранее добавленный из представления в виде вспомогательных функций тегов.</span><span class="sxs-lookup"><span data-stu-id="330ff-251">Removes Tag Helpers previously added from a view.</span></span> |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="330ff-252">Указывает префикс тега для включения поддержка вспомогательных функций тегов и явно объявить использование вспомогательного тег.</span><span class="sxs-lookup"><span data-stu-id="330ff-252">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="330ff-253">Razor зарезервированные ключевые слова</span><span class="sxs-lookup"><span data-stu-id="330ff-253">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="330ff-254">Ключевые слова Razor</span><span class="sxs-lookup"><span data-stu-id="330ff-254">Razor keywords</span></span>

* <span data-ttu-id="330ff-255">страница (требуется Core ASP.NET 2.0 и более поздние версии)</span><span class="sxs-lookup"><span data-stu-id="330ff-255">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="330ff-256">функции</span><span class="sxs-lookup"><span data-stu-id="330ff-256">functions</span></span>
* <span data-ttu-id="330ff-257">наследует</span><span class="sxs-lookup"><span data-stu-id="330ff-257">inherits</span></span>
* <span data-ttu-id="330ff-258">model</span><span class="sxs-lookup"><span data-stu-id="330ff-258">model</span></span>
* <span data-ttu-id="330ff-259">section</span><span class="sxs-lookup"><span data-stu-id="330ff-259">section</span></span>
* <span data-ttu-id="330ff-260">Вспомогательный (не поддерживаемых ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="330ff-260">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="330ff-261">Ключевые слова Razor экранируются с `@(Razor Keyword)` (например, `@(functions)`).</span><span class="sxs-lookup"><span data-stu-id="330ff-261">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="330ff-262">Ключевые слова C# Razor</span><span class="sxs-lookup"><span data-stu-id="330ff-262">C# Razor keywords</span></span>

* <span data-ttu-id="330ff-263">case</span><span class="sxs-lookup"><span data-stu-id="330ff-263">case</span></span>
* <span data-ttu-id="330ff-264">do</span><span class="sxs-lookup"><span data-stu-id="330ff-264">do</span></span>
* <span data-ttu-id="330ff-265">default</span><span class="sxs-lookup"><span data-stu-id="330ff-265">default</span></span>
* <span data-ttu-id="330ff-266">for</span><span class="sxs-lookup"><span data-stu-id="330ff-266">for</span></span>
* <span data-ttu-id="330ff-267">foreach</span><span class="sxs-lookup"><span data-stu-id="330ff-267">foreach</span></span>
* <span data-ttu-id="330ff-268">if</span><span class="sxs-lookup"><span data-stu-id="330ff-268">if</span></span>
* <span data-ttu-id="330ff-269">else</span><span class="sxs-lookup"><span data-stu-id="330ff-269">else</span></span>
* <span data-ttu-id="330ff-270">lock</span><span class="sxs-lookup"><span data-stu-id="330ff-270">lock</span></span>
* <span data-ttu-id="330ff-271">switch</span><span class="sxs-lookup"><span data-stu-id="330ff-271">switch</span></span>
* <span data-ttu-id="330ff-272">try</span><span class="sxs-lookup"><span data-stu-id="330ff-272">try</span></span>
* <span data-ttu-id="330ff-273">catch</span><span class="sxs-lookup"><span data-stu-id="330ff-273">catch</span></span>
* <span data-ttu-id="330ff-274">finally</span><span class="sxs-lookup"><span data-stu-id="330ff-274">finally</span></span>
* <span data-ttu-id="330ff-275">использование</span><span class="sxs-lookup"><span data-stu-id="330ff-275">using</span></span>
* <span data-ttu-id="330ff-276">while</span><span class="sxs-lookup"><span data-stu-id="330ff-276">while</span></span>

<span data-ttu-id="330ff-277">Ключевые слова C# Razor необходимо экранировать двойные с `@(@C# Razor Keyword)` (например, `@(@case)`).</span><span class="sxs-lookup"><span data-stu-id="330ff-277">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="330ff-278">Первый `@` экранирует анализатор Razor.</span><span class="sxs-lookup"><span data-stu-id="330ff-278">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="330ff-279">Второй `@` экранирует анализатор C#.</span><span class="sxs-lookup"><span data-stu-id="330ff-279">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="330ff-280">Зарезервированные ключевые слова, не используется в Razor</span><span class="sxs-lookup"><span data-stu-id="330ff-280">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="330ff-281">namespace</span><span class="sxs-lookup"><span data-stu-id="330ff-281">namespace</span></span>
* <span data-ttu-id="330ff-282">класс</span><span class="sxs-lookup"><span data-stu-id="330ff-282">class</span></span>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="330ff-283">Просмотр класса Razor C#, созданного для представления</span><span class="sxs-lookup"><span data-stu-id="330ff-283">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="330ff-284">Добавьте следующий класс в проект ASP.NET MVC основных компонентов:</span><span class="sxs-lookup"><span data-stu-id="330ff-284">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="330ff-285">Переопределить `RazorTemplateEngine` добавленные MVC с `CustomTemplateEngine` класса:</span><span class="sxs-lookup"><span data-stu-id="330ff-285">Override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="330ff-286">Точка останова `return csharpDocument` инструкция `CustomTemplateEngine`.</span><span class="sxs-lookup"><span data-stu-id="330ff-286">Set a break point on the `return csharpDocument` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="330ff-287">Когда выполнение программы остановится в точке останова, просматривать значения `generatedCode`.</span><span class="sxs-lookup"><span data-stu-id="330ff-287">When program execution stops at the break point, view the value of `generatedCode`.</span></span>

![Представление визуализатор текста generatedCode](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="330ff-289">Представление уточняющих запросов и чувствительности к регистру</span><span class="sxs-lookup"><span data-stu-id="330ff-289">View lookups and case sensitivity</span></span>

<span data-ttu-id="330ff-290">Обработчик представлений Razor выполняет поиск с учетом регистра для представлений.</span><span class="sxs-lookup"><span data-stu-id="330ff-290">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="330ff-291">Однако фактические уточняющего запроса определяется базовая файловая система:</span><span class="sxs-lookup"><span data-stu-id="330ff-291">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="330ff-292">На основе исходного файла:</span><span class="sxs-lookup"><span data-stu-id="330ff-292">File based source:</span></span> 
  * <span data-ttu-id="330ff-293">В операционных системах без учета регистра файловые системы (например, Windows) поиск поставщика физических файлов зависят от регистра.</span><span class="sxs-lookup"><span data-stu-id="330ff-293">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="330ff-294">Например `return View("Test")` приводит к соответствий для */Views/Home/Test.cshtml*, */Views/home/test.cshtml*и любой другой тип регистра.</span><span class="sxs-lookup"><span data-stu-id="330ff-294">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="330ff-295">В системах с учетом регистра файла (например, Linux, OSX и с `EmbeddedFileProvider`), уточняющие запросы выполняются с учетом регистра.</span><span class="sxs-lookup"><span data-stu-id="330ff-295">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="330ff-296">Например `return View("Test")` специально совпадений */Views/Home/Test.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="330ff-296">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="330ff-297">Предварительно скомпилированные представлений: С основными ASP.Net 2.0 и более поздних, поиске предкомпилированного представления без учета регистра для всех операционных систем.</span><span class="sxs-lookup"><span data-stu-id="330ff-297">Precompiled views: With ASP.Net Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="330ff-298">Поведение идентично поведение поставщика физического файла в Windows.</span><span class="sxs-lookup"><span data-stu-id="330ff-298">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="330ff-299">Если два представления предкомпилированного отличаются только регистром, результатом поиска является недетерминированным.</span><span class="sxs-lookup"><span data-stu-id="330ff-299">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="330ff-300">Разработчики могут совпадает с регистром имена файлов и каталогов на регистр:</span><span class="sxs-lookup"><span data-stu-id="330ff-300">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

    * <span data-ttu-id="330ff-301">Имена областей, контроллера и действия.</span><span class="sxs-lookup"><span data-stu-id="330ff-301">Area, controller, and action names.</span></span> 
    * <span data-ttu-id="330ff-302">Страниц Razor.</span><span class="sxs-lookup"><span data-stu-id="330ff-302">Razor Pages.</span></span>
    
<span data-ttu-id="330ff-303">Регистра гарантирует, что развертываний найти их представления независимо от используемой файловой системы.</span><span class="sxs-lookup"><span data-stu-id="330ff-303">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>
