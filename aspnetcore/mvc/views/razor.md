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
# <a name="razor-syntax-for-aspnet-core"></a><span data-ttu-id="c6308-103">Синтаксис Razor для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c6308-103">Razor syntax for ASP.NET Core</span></span>

<span data-ttu-id="c6308-104">Авторы: [Рик Андерсон (Rick Anderson)](https://twitter.com/RickAndMSFT), [Люк Лэтем (Luke Latham)](https://github.com/guardrex), [Тейлор Маллен (Taylor Mullen)](https://twitter.com/ntaylormullen) и [Дэн Викарел (Dan Vicarel)](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="c6308-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="c6308-105">Razor — это синтаксис разметки для внедрения в веб-страницы серверного кода.</span><span class="sxs-lookup"><span data-stu-id="c6308-105">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="c6308-106">Синтаксис Razor состоит из разметки Razor, C# и HTML.</span><span class="sxs-lookup"><span data-stu-id="c6308-106">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="c6308-107">Файлы, содержащие Razor, обычно имеют расширение *CSHTML*.</span><span class="sxs-lookup"><span data-stu-id="c6308-107">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="c6308-108">Отрисовка HTML</span><span class="sxs-lookup"><span data-stu-id="c6308-108">Rendering HTML</span></span>

<span data-ttu-id="c6308-109">Языком Razor по умолчанию является HTML.</span><span class="sxs-lookup"><span data-stu-id="c6308-109">The default Razor language is HTML.</span></span> <span data-ttu-id="c6308-110">Отрисовка HTML из разметки Razor ничем не отличается от отрисовки из HTML-файла.</span><span class="sxs-lookup"><span data-stu-id="c6308-110">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span> <span data-ttu-id="c6308-111">Сервер отображает разметку HTML в *CSHTML*-файлах Razor без изменений.</span><span class="sxs-lookup"><span data-stu-id="c6308-111">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="c6308-112">Синтаксис Razor</span><span class="sxs-lookup"><span data-stu-id="c6308-112">Razor syntax</span></span>

<span data-ttu-id="c6308-113">Razor поддерживает C# и использует символ `@` для перехода с HTML на C#.</span><span class="sxs-lookup"><span data-stu-id="c6308-113">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="c6308-114">Razor вычисляет выражения C# и отображает их в выходных данных HTML.</span><span class="sxs-lookup"><span data-stu-id="c6308-114">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="c6308-115">Если за символом `@` следует [зарезервированное ключевое слово Razor](#razor-reserved-keywords), он переходит на разметку Razor.</span><span class="sxs-lookup"><span data-stu-id="c6308-115">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="c6308-116">В противном случае он переходит на обычный C#.</span><span class="sxs-lookup"><span data-stu-id="c6308-116">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="c6308-117">В качестве escape-символа для `@` в разметке Razor используйте второй символ `@`:</span><span class="sxs-lookup"><span data-stu-id="c6308-117">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="c6308-118">Код будет отображен в HTML с одним символом `@`:</span><span class="sxs-lookup"><span data-stu-id="c6308-118">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="c6308-119">HTML-атрибуты и содержимое, включающие адреса электронной почты, не расценивают символ `@` как символ перехода.</span><span class="sxs-lookup"><span data-stu-id="c6308-119">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="c6308-120">В следующем примере синтаксический анализ Razor не затрагивает адреса электронной почты:</span><span class="sxs-lookup"><span data-stu-id="c6308-120">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="c6308-121">Неявные выражения Razor</span><span class="sxs-lookup"><span data-stu-id="c6308-121">Implicit Razor expressions</span></span>

<span data-ttu-id="c6308-122">Неявные выражения Razor начинаются с символа `@`, за которым следует код C#:</span><span class="sxs-lookup"><span data-stu-id="c6308-122">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="c6308-123">Неявные выражения не должны содержать пробелов. Исключением является ключевое слово C# `await`.</span><span class="sxs-lookup"><span data-stu-id="c6308-123">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="c6308-124">Если оператор C# имеет четкое окончание, пробелы вставлять можно:</span><span class="sxs-lookup"><span data-stu-id="c6308-124">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="c6308-125">Неявные выражения **не могут** содержать универсальные шаблоны C#, так как символы в угловых скобках (`<>`) интерпретируются как тег HTML.</span><span class="sxs-lookup"><span data-stu-id="c6308-125">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="c6308-126">Следующий код является **недопустимым**:</span><span class="sxs-lookup"><span data-stu-id="c6308-126">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="c6308-127">Приведенный выше код вызывает ошибку компилятора примерно следующего вида:</span><span class="sxs-lookup"><span data-stu-id="c6308-127">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="c6308-128">Элемент "int" не был закрыт.</span><span class="sxs-lookup"><span data-stu-id="c6308-128">The "int" element wasn't closed.</span></span> <span data-ttu-id="c6308-129">Все элементы должны быть самозакрывающимися или иметь соответствующий закрывающий тег.</span><span class="sxs-lookup"><span data-stu-id="c6308-129">All elements must be either self-closing or have a matching end tag.</span></span>
 *  <span data-ttu-id="c6308-130">Не удается преобразовать группу методов "GenericMethod" в не являющийся делегатом тип "object".</span><span class="sxs-lookup"><span data-stu-id="c6308-130">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="c6308-131">Предполагалось вызывать этот метод?</span><span class="sxs-lookup"><span data-stu-id="c6308-131">Did you intend to invoke the method?\`</span></span> 
 
<span data-ttu-id="c6308-132">Вызовы универсальных методов должны быть заключены в [явное выражение Razor](#explicit-razor-expressions) или [блок кода Razor](#razor-code-blocks).</span><span class="sxs-lookup"><span data-stu-id="c6308-132">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="c6308-133">Явные выражения Razor</span><span class="sxs-lookup"><span data-stu-id="c6308-133">Explicit Razor expressions</span></span>

<span data-ttu-id="c6308-134">Явные выражения Razor состоят из символа `@` с соответствующими открывающими и закрывающими скобками.</span><span class="sxs-lookup"><span data-stu-id="c6308-134">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="c6308-135">Для отображения времени прошлой недели используется следующая разметка Razor:</span><span class="sxs-lookup"><span data-stu-id="c6308-135">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="c6308-136">Любое содержимое в скобках `@()` вычисляется и отображается в выходных данных.</span><span class="sxs-lookup"><span data-stu-id="c6308-136">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="c6308-137">Неявные выражения, описанные в предыдущем разделе, обычно не содержат пробелов.</span><span class="sxs-lookup"><span data-stu-id="c6308-137">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="c6308-138">В следующем коде из значения текущего времени неделя не вычитается:</span><span class="sxs-lookup"><span data-stu-id="c6308-138">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="c6308-139">Код отображает следующий HTML:</span><span class="sxs-lookup"><span data-stu-id="c6308-139">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="c6308-140">Явные выражения позволяют объединять результат своего выполнения с дополнительным текстом:</span><span class="sxs-lookup"><span data-stu-id="c6308-140">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="c6308-141">Без явного выражения `<p>Age@joe.Age</p>` обрабатывается как адрес электронной почты, и на выходе отображается `<p>Age@joe.Age</p>`.</span><span class="sxs-lookup"><span data-stu-id="c6308-141">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="c6308-142">Если же текст написан как явное выражение, то вы получите `<p>Age33</p>`.</span><span class="sxs-lookup"><span data-stu-id="c6308-142">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>


<span data-ttu-id="c6308-143">Вы можете использовать явные выражения для отображения выходных данных из универсальных методов в файлах *CSHTML*.</span><span class="sxs-lookup"><span data-stu-id="c6308-143">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="c6308-144">В неявном выражении символы в угловых скобках (`<>`) интерпретируются как тег HTML.</span><span class="sxs-lookup"><span data-stu-id="c6308-144">In an implicit expression, the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="c6308-145">Следующая разметка является в Razor **недопустимой**:</span><span class="sxs-lookup"><span data-stu-id="c6308-145">The following markup is **not** valid Razor:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="c6308-146">Приведенный выше код вызывает ошибку компилятора примерно следующего вида:</span><span class="sxs-lookup"><span data-stu-id="c6308-146">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="c6308-147">Элемент "int" не был закрыт.</span><span class="sxs-lookup"><span data-stu-id="c6308-147">The "int" element wasn't closed.</span></span> <span data-ttu-id="c6308-148">Все элементы должны быть самозакрывающимися или иметь соответствующий закрывающий тег.</span><span class="sxs-lookup"><span data-stu-id="c6308-148">All elements must be either self-closing or have a matching end tag.</span></span>
 *  <span data-ttu-id="c6308-149">Не удается преобразовать группу методов "GenericMethod" в не являющийся делегатом тип "object".</span><span class="sxs-lookup"><span data-stu-id="c6308-149">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="c6308-150">Предполагалось вызывать этот метод?</span><span class="sxs-lookup"><span data-stu-id="c6308-150">Did you intend to invoke the method?\`</span></span> 
 
 <span data-ttu-id="c6308-151">Корректный способ написания этого кода показан в разметке ниже.</span><span class="sxs-lookup"><span data-stu-id="c6308-151">The following markup shows the correct way write this code.</span></span> <span data-ttu-id="c6308-152">Код записывается как явное выражение:</span><span class="sxs-lookup"><span data-stu-id="c6308-152">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a><span data-ttu-id="c6308-153">Кодирование выражений</span><span class="sxs-lookup"><span data-stu-id="c6308-153">Expression encoding</span></span>

<span data-ttu-id="c6308-154">Выражения C#, которые имеют строковое выходное значение, кодируются в формате HTML.</span><span class="sxs-lookup"><span data-stu-id="c6308-154">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="c6308-155">Выражения C#, которые имеют значение `IHtmlContent`, обрабатываются непосредственно с помощью `IHtmlContent.WriteTo`.</span><span class="sxs-lookup"><span data-stu-id="c6308-155">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="c6308-156">Выражения C#, которые не имеют выходное значение `IHtmlContent`, преобразуются в строку с помощью `ToString` и кодируются перед обработкой.</span><span class="sxs-lookup"><span data-stu-id="c6308-156">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="c6308-157">Код отображает следующий HTML:</span><span class="sxs-lookup"><span data-stu-id="c6308-157">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="c6308-158">HTML отображается в обозревателе следующим образом:</span><span class="sxs-lookup"><span data-stu-id="c6308-158">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="c6308-159">Выходные данные `HtmlHelper.Raw` не кодируются, но отображаются в виде разметки HTML.</span><span class="sxs-lookup"><span data-stu-id="c6308-159">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="c6308-160">Использование `HtmlHelper.Raw` с непроверенными входными данными пользователя представляет угрозу безопасности.</span><span class="sxs-lookup"><span data-stu-id="c6308-160">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="c6308-161">Эти входные данные могут содержать вредоносный код JavaScript или другие эксплойты.</span><span class="sxs-lookup"><span data-stu-id="c6308-161">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="c6308-162">Очистка вводимых пользователем данных является сложной задачей.</span><span class="sxs-lookup"><span data-stu-id="c6308-162">Sanitizing user input is difficult.</span></span> <span data-ttu-id="c6308-163">Старайтесь не использовать `HtmlHelper.Raw` с такими данными.</span><span class="sxs-lookup"><span data-stu-id="c6308-163">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="c6308-164">Код отображает следующий HTML:</span><span class="sxs-lookup"><span data-stu-id="c6308-164">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="c6308-165">Блоки кода Razor</span><span class="sxs-lookup"><span data-stu-id="c6308-165">Razor code blocks</span></span>

<span data-ttu-id="c6308-166">Блоки кода Razor начинаются с символа `@` и заключены в фигурные скобки `{}`.</span><span class="sxs-lookup"><span data-stu-id="c6308-166">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="c6308-167">В отличие от выражений код C# внутри блоков кода не обрабатывается.</span><span class="sxs-lookup"><span data-stu-id="c6308-167">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="c6308-168">Блоки кода и выражения в представлении используют общую область и определяются в следующем порядке:</span><span class="sxs-lookup"><span data-stu-id="c6308-168">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

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

<span data-ttu-id="c6308-169">Код отображает следующий HTML:</span><span class="sxs-lookup"><span data-stu-id="c6308-169">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a><span data-ttu-id="c6308-170">Неявные переходы</span><span class="sxs-lookup"><span data-stu-id="c6308-170">Implicit transitions</span></span>

<span data-ttu-id="c6308-171">В блоке кода языком по умолчанию является C#, но страница Razor Page может вернуться на HTML:</span><span class="sxs-lookup"><span data-stu-id="c6308-171">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="c6308-172">Явный переход с разделителями</span><span class="sxs-lookup"><span data-stu-id="c6308-172">Explicit delimited transition</span></span>

<span data-ttu-id="c6308-173">Чтобы определить подраздел блока кода, который должен отрисовывать HTML, окружите подлежащие отображению символы тегами Razor **\<text>**:</span><span class="sxs-lookup"><span data-stu-id="c6308-173">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="c6308-174">Используйте этот способ для отрисовки HTML, не заключенного в HTML-теги.</span><span class="sxs-lookup"><span data-stu-id="c6308-174">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="c6308-175">Без тега HTML или Razor возникает ошибка времени выполнения Razor.</span><span class="sxs-lookup"><span data-stu-id="c6308-175">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="c6308-176">Тег **\<text>** хорошо подходит для контроля пробелов при отрисовке содержимого:</span><span class="sxs-lookup"><span data-stu-id="c6308-176">The **\<text>** tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="c6308-177">Отрисовывается только содержимое между тегами **\<text>**.</span><span class="sxs-lookup"><span data-stu-id="c6308-177">Only the content between the **\<text>** tag is rendered.</span></span> 
* <span data-ttu-id="c6308-178">В выходных данных HTML пробелы до или после тега **\<text>** не отображаются.</span><span class="sxs-lookup"><span data-stu-id="c6308-178">No whitespace before or after the **\<text>** tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="c6308-179">Явные переходы по строкам с @:</span><span class="sxs-lookup"><span data-stu-id="c6308-179">Explicit Line Transition with @:</span></span>

<span data-ttu-id="c6308-180">Для отрисовки оставшейся части строки в виде HTML внутри блока кода используйте синтаксис "`@:`":</span><span class="sxs-lookup"><span data-stu-id="c6308-180">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="c6308-181">Если в коде отсутствует `@:`, возникает ошибка среды выполнения Razor.</span><span class="sxs-lookup"><span data-stu-id="c6308-181">Without the `@:` in the code, a Razor runtime error is generated.</span></span>

<span data-ttu-id="c6308-182">Предупреждение. Дополнительные символы `@` в файле Razor могут вызвать ошибки компилятора в последующих операторах блока.</span><span class="sxs-lookup"><span data-stu-id="c6308-182">Warning: Extra `@` characters in a Razor file can cause cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="c6308-183">Эти ошибки компилятора может быть трудно проанализировать, так как ошибка фактически возникает раньше, чем указано.</span><span class="sxs-lookup"><span data-stu-id="c6308-183">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span> <span data-ttu-id="c6308-184">Чаще всего эта ошибка появляется после объединения множества неявных или явных выражений в один блок кода.</span><span class="sxs-lookup"><span data-stu-id="c6308-184">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="c6308-185">Управляющие структуры</span><span class="sxs-lookup"><span data-stu-id="c6308-185">Control Structures</span></span>

<span data-ttu-id="c6308-186">Управляющие структуры являются расширением блоков кода.</span><span class="sxs-lookup"><span data-stu-id="c6308-186">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="c6308-187">Все аспекты блоков кода (переход на разметку, встроенный код C#) также относятся к следующим структурам.</span><span class="sxs-lookup"><span data-stu-id="c6308-187">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="c6308-188">Условные выражения @if, else if, else и @switch</span><span class="sxs-lookup"><span data-stu-id="c6308-188">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="c6308-189">`@if` контролирует, когда нужно запускать код:</span><span class="sxs-lookup"><span data-stu-id="c6308-189">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="c6308-190">Для `else` и `else if` символ `@` не требуется:</span><span class="sxs-lookup"><span data-stu-id="c6308-190">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="c6308-191">В следующей разметке показано использование оператора switch:</span><span class="sxs-lookup"><span data-stu-id="c6308-191">The following markup shows how to use a switch statement:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="c6308-192">Циклы @for, @foreach, @while и @do while</span><span class="sxs-lookup"><span data-stu-id="c6308-192">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="c6308-193">Операторы выполнения цикла позволяют выполнять отрисовку шаблонного HTML.</span><span class="sxs-lookup"><span data-stu-id="c6308-193">Templated HTML can be rendered with looping control statements.</span></span> <span data-ttu-id="c6308-194">Отрисовка списка людей:</span><span class="sxs-lookup"><span data-stu-id="c6308-194">To render a list of people:</span></span>

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

<span data-ttu-id="c6308-195">Поддерживаются следующие операторы выполнения цикла:</span><span class="sxs-lookup"><span data-stu-id="c6308-195">The following looping statements are supported:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="c6308-196">Составной оператор @using</span><span class="sxs-lookup"><span data-stu-id="c6308-196">Compound @using</span></span>

<span data-ttu-id="c6308-197">В C# оператор `using` позволяет обеспечить использование какого-то объекта.</span><span class="sxs-lookup"><span data-stu-id="c6308-197">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="c6308-198">В Razor этот механизм позволяет создавать вспомогательные функции HTML, включающие дополнительное содержимое.</span><span class="sxs-lookup"><span data-stu-id="c6308-198">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="c6308-199">В следующем коде вспомогательные функции HTML используют оператор `@using` для создания тега формы:</span><span class="sxs-lookup"><span data-stu-id="c6308-199">In the following code, HTML Helpers render a form tag with the `@using` statement:</span></span>


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

<span data-ttu-id="c6308-200">Для действий на уровне области можно использовать [вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="c6308-200">Scope-level actions can be performed with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="c6308-201">@try, catch, finally</span><span class="sxs-lookup"><span data-stu-id="c6308-201">@try, catch, finally</span></span>

<span data-ttu-id="c6308-202">Обработка исключений выполняется так же, как в C#:</span><span class="sxs-lookup"><span data-stu-id="c6308-202">Exception handling is similar to C#:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="c6308-203">Razor позволяет защищать важные разделы при помощи операторов блокировки:</span><span class="sxs-lookup"><span data-stu-id="c6308-203">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="c6308-204">Комментарии</span><span class="sxs-lookup"><span data-stu-id="c6308-204">Comments</span></span>

<span data-ttu-id="c6308-205">Razor поддерживает комментарии C# и HTML:</span><span class="sxs-lookup"><span data-stu-id="c6308-205">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="c6308-206">Код отображает следующий HTML:</span><span class="sxs-lookup"><span data-stu-id="c6308-206">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="c6308-207">Сервер удаляет комментарии Razor перед отображением веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="c6308-207">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="c6308-208">Для разделения комментариев Razor использует `@*  *@`.</span><span class="sxs-lookup"><span data-stu-id="c6308-208">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="c6308-209">Следующий код закомментирован, поэтому сервер не отрисовывает разметку:</span><span class="sxs-lookup"><span data-stu-id="c6308-209">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="c6308-210">Директивы</span><span class="sxs-lookup"><span data-stu-id="c6308-210">Directives</span></span>

<span data-ttu-id="c6308-211">Директивы Razor представлены неявными выражениями с зарезервированными ключевыми словами после символа `@`.</span><span class="sxs-lookup"><span data-stu-id="c6308-211">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="c6308-212">Как правило, директива изменяет способ анализа представления или открывает доступ к дополнительным функциям.</span><span class="sxs-lookup"><span data-stu-id="c6308-212">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="c6308-213">Узнав, каким образом Razor создает код для представления, вы сможете легко понять принципы работы директив.</span><span class="sxs-lookup"><span data-stu-id="c6308-213">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="c6308-214">Код создает класс, аналогичный следующему:</span><span class="sxs-lookup"><span data-stu-id="c6308-214">The code generates a class similar to the following:</span></span>

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

<span data-ttu-id="c6308-215">Сведения о просмотре этого класса приводятся в разделе [Просмотр Razor-класса C#, созданного для представления](#viewing-the-razor-c-class-generated-for-a-view) далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="c6308-215">Later in this article, the section [Viewing the Razor C# class generated for a view](#viewing-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

### <a name="using"></a>@using

<span data-ttu-id="c6308-216">Директива `@using` добавляет директиву C# `using` в созданное представление:</span><span class="sxs-lookup"><span data-stu-id="c6308-216">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="c6308-217">Директива `@model` определяет тип модели, передаваемой в представление:</span><span class="sxs-lookup"><span data-stu-id="c6308-217">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="c6308-218">В MVC-приложении ASP.NET Core, созданном с отдельными учетными записями пользователей, представление *Views/Account/Login.cshtml* содержит следующее объявление модели:</span><span class="sxs-lookup"><span data-stu-id="c6308-218">In an ASP.NET Core MVC app created with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="c6308-219">Созданный класс наследует от `RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="c6308-219">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="c6308-220">Для доступа к модели, переданной в представление, Razor предоставляет свойство `Model`:</span><span class="sxs-lookup"><span data-stu-id="c6308-220">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="c6308-221">Директива `@model` задает тип этого свойства.</span><span class="sxs-lookup"><span data-stu-id="c6308-221">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="c6308-222">Директива указывает `T` в `RazorPage<T>` — созданном классе, на основе которого создается производное представление.</span><span class="sxs-lookup"><span data-stu-id="c6308-222">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="c6308-223">Если директива `@model` не указана, свойство `Model` имеет тип `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="c6308-223">If the `@model` directive iisn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="c6308-224">Значение модели передается из контроллера в представление.</span><span class="sxs-lookup"><span data-stu-id="c6308-224">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="c6308-225">Дополнительные сведения см. в разделах, посвященных строго типизированным моделям и ключевому слову @model.</span><span class="sxs-lookup"><span data-stu-id="c6308-225">For more information, see [Strongly typed models and the @model keyword.</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="c6308-226">Директива `@inherits` позволяет полностью управлять классом, которому наследует представление:</span><span class="sxs-lookup"><span data-stu-id="c6308-226">The `@inherits` directive provides full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="c6308-227">Следующий код показывает настраиваемый тип страницы Razor:</span><span class="sxs-lookup"><span data-stu-id="c6308-227">The following code is a custom Razor page type:</span></span>

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="c6308-228">В представлении отображается `CustomText`:</span><span class="sxs-lookup"><span data-stu-id="c6308-228">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="c6308-229">Код отображает следующий HTML:</span><span class="sxs-lookup"><span data-stu-id="c6308-229">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 <span data-ttu-id="c6308-230">`@model` и `@inherits` могут использоваться в одном представлении.</span><span class="sxs-lookup"><span data-stu-id="c6308-230">`@model` and `@inherits` can be used in the same view.</span></span> <span data-ttu-id="c6308-231">`@inherits` может находиться в файле *_ViewImports.cshtml*, который импортируется представлением:</span><span class="sxs-lookup"><span data-stu-id="c6308-231">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="c6308-232">Следующий код показывает пример строго типизированного представления:</span><span class="sxs-lookup"><span data-stu-id="c6308-232">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="c6308-233">Если передать в модель "rick@contoso.com", представление создает следующую разметку HTML:</span><span class="sxs-lookup"><span data-stu-id="c6308-233">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


<span data-ttu-id="c6308-234">Директива `@inject` позволяет странице Razor внедрять в представление службу из [контейнера службы](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c6308-234">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="c6308-235">Дополнительные сведения: [Внедрение зависимостей в представления](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c6308-235">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="c6308-236">Директива `@functions` позволяет странице Razor добавлять в представление содержимое на уровне функций:</span><span class="sxs-lookup"><span data-stu-id="c6308-236">The `@functions` directive enables a Razor Page to add function-level content to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="c6308-237">Пример:</span><span class="sxs-lookup"><span data-stu-id="c6308-237">For example:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="c6308-238">Код создает следующую разметку HTML:</span><span class="sxs-lookup"><span data-stu-id="c6308-238">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="c6308-239">Следующий код показывает созданный Razor-класс C#:</span><span class="sxs-lookup"><span data-stu-id="c6308-239">The following code is the generated Razor C# class:</span></span>

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

<span data-ttu-id="c6308-240">Директива `@section` используется в сочетании с [макетом](xref:mvc/views/layout) и позволяет представлениям отображать содержимое в различных частях HTML-страницы.</span><span class="sxs-lookup"><span data-stu-id="c6308-240">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="c6308-241">Дополнительные сведения: [Разделы](xref:mvc/views/layout#layout-sections-label).</span><span class="sxs-lookup"><span data-stu-id="c6308-241">For more information, see [Sections](xref:mvc/views/layout#layout-sections-label).</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="c6308-242">Вспомогательные функции тегов</span><span class="sxs-lookup"><span data-stu-id="c6308-242">Tag Helpers</span></span>

<span data-ttu-id="c6308-243">Существует три директивы, которые относятся к [вспомогательным функциям тегов](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="c6308-243">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="c6308-244">Директива</span><span class="sxs-lookup"><span data-stu-id="c6308-244">Directive</span></span> | <span data-ttu-id="c6308-245">Функция</span><span class="sxs-lookup"><span data-stu-id="c6308-245">Function</span></span> |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="c6308-246">Делает вспомогательные функции тегов доступными в представлении.</span><span class="sxs-lookup"><span data-stu-id="c6308-246">Makes Tag Helpers available to a view.</span></span> |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="c6308-247">Удаляет из представления вспомогательные функции тегов, добавленные ранее.</span><span class="sxs-lookup"><span data-stu-id="c6308-247">Removes Tag Helpers previously added from a view.</span></span> |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="c6308-248">Задает префикс тега, который активирует поддержку вспомогательной функции тега и ее использования в явном виде.</span><span class="sxs-lookup"><span data-stu-id="c6308-248">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="c6308-249">Зарезервированные ключевые слова Razor</span><span class="sxs-lookup"><span data-stu-id="c6308-249">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="c6308-250">Ключевые слова Razor</span><span class="sxs-lookup"><span data-stu-id="c6308-250">Razor keywords</span></span>

* <span data-ttu-id="c6308-251">page (требуется ASP.NET Core 2.0 или более поздние версии)</span><span class="sxs-lookup"><span data-stu-id="c6308-251">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="c6308-252">функции</span><span class="sxs-lookup"><span data-stu-id="c6308-252">functions</span></span>
* <span data-ttu-id="c6308-253">наследует</span><span class="sxs-lookup"><span data-stu-id="c6308-253">inherits</span></span>
* <span data-ttu-id="c6308-254">model</span><span class="sxs-lookup"><span data-stu-id="c6308-254">model</span></span>
* <span data-ttu-id="c6308-255">section</span><span class="sxs-lookup"><span data-stu-id="c6308-255">section</span></span>
* <span data-ttu-id="c6308-256">helper (сейчас не поддерживается в ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="c6308-256">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="c6308-257">В качестве escape-символа для ключевых слов Razor используется `@(Razor Keyword)` (например, `@(functions)`).</span><span class="sxs-lookup"><span data-stu-id="c6308-257">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="c6308-258">Ключевые слова C# в Razor</span><span class="sxs-lookup"><span data-stu-id="c6308-258">C# Razor keywords</span></span>

* <span data-ttu-id="c6308-259">регистр знаков</span><span class="sxs-lookup"><span data-stu-id="c6308-259">case</span></span>
* <span data-ttu-id="c6308-260">do</span><span class="sxs-lookup"><span data-stu-id="c6308-260">do</span></span>
* <span data-ttu-id="c6308-261">default</span><span class="sxs-lookup"><span data-stu-id="c6308-261">default</span></span>
* <span data-ttu-id="c6308-262">for</span><span class="sxs-lookup"><span data-stu-id="c6308-262">for</span></span>
* <span data-ttu-id="c6308-263">foreach</span><span class="sxs-lookup"><span data-stu-id="c6308-263">foreach</span></span>
* <span data-ttu-id="c6308-264">if</span><span class="sxs-lookup"><span data-stu-id="c6308-264">if</span></span>
* <span data-ttu-id="c6308-265">else</span><span class="sxs-lookup"><span data-stu-id="c6308-265">else</span></span>
* <span data-ttu-id="c6308-266">lock</span><span class="sxs-lookup"><span data-stu-id="c6308-266">lock</span></span>
* <span data-ttu-id="c6308-267">switch</span><span class="sxs-lookup"><span data-stu-id="c6308-267">switch</span></span>
* <span data-ttu-id="c6308-268">try</span><span class="sxs-lookup"><span data-stu-id="c6308-268">try</span></span>
* <span data-ttu-id="c6308-269">catch</span><span class="sxs-lookup"><span data-stu-id="c6308-269">catch</span></span>
* <span data-ttu-id="c6308-270">finally</span><span class="sxs-lookup"><span data-stu-id="c6308-270">finally</span></span>
* <span data-ttu-id="c6308-271">использование</span><span class="sxs-lookup"><span data-stu-id="c6308-271">using</span></span>
* <span data-ttu-id="c6308-272">while</span><span class="sxs-lookup"><span data-stu-id="c6308-272">while</span></span>

<span data-ttu-id="c6308-273">Для ключевых слов C# в Razor требуется двойной escape-символ: `@(@C# Razor Keyword)` (например, `@(@case)`).</span><span class="sxs-lookup"><span data-stu-id="c6308-273">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="c6308-274">Первый `@` предназначен для обхода синтаксического анализа Razor,</span><span class="sxs-lookup"><span data-stu-id="c6308-274">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="c6308-275">а второй `@` — для обхода C#.</span><span class="sxs-lookup"><span data-stu-id="c6308-275">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="c6308-276">Зарезервированные ключевые слова, не используемые в Razor</span><span class="sxs-lookup"><span data-stu-id="c6308-276">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="c6308-277">namespace</span><span class="sxs-lookup"><span data-stu-id="c6308-277">namespace</span></span>
* <span data-ttu-id="c6308-278">класс</span><span class="sxs-lookup"><span data-stu-id="c6308-278">class</span></span>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="c6308-279">Просмотр Razor-класса C#, созданного для представления</span><span class="sxs-lookup"><span data-stu-id="c6308-279">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="c6308-280">Добавьте в MVC-проект ASP.NET следующий класс:</span><span class="sxs-lookup"><span data-stu-id="c6308-280">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="c6308-281">Переопределите класс `RazorTemplateEngine`, добавленный MVC, классом `CustomTemplateEngine`:</span><span class="sxs-lookup"><span data-stu-id="c6308-281">Override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="c6308-282">Установите точку останова в операторе `return csharpDocument` класса `CustomTemplateEngine`.</span><span class="sxs-lookup"><span data-stu-id="c6308-282">Set a break point on the `return csharpDocument` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="c6308-283">Когда выполнение программы остановится в этой точке, просмотрите значение `generatedCode`.</span><span class="sxs-lookup"><span data-stu-id="c6308-283">When program execution stops at the break point, view the value of `generatedCode`.</span></span>

![Представление generatedCode в визуализаторе текста](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="c6308-285">Поиск данных в представлениях и учет регистра</span><span class="sxs-lookup"><span data-stu-id="c6308-285">View lookups and case sensitivity</span></span>

<span data-ttu-id="c6308-286">Модуль представлений Razor позволяет искать в представлениях данные с учетом регистра.</span><span class="sxs-lookup"><span data-stu-id="c6308-286">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="c6308-287">Однако фактический поиск зависит от используемой файловой системы.</span><span class="sxs-lookup"><span data-stu-id="c6308-287">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="c6308-288">Источники в виде файлов:</span><span class="sxs-lookup"><span data-stu-id="c6308-288">File based source:</span></span> 
  * <span data-ttu-id="c6308-289">В операционных системах, файловые системы которых не учитывают регистр (например, Windows), поиск поставщика физических файлов не зависит от регистра.</span><span class="sxs-lookup"><span data-stu-id="c6308-289">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="c6308-290">Например, поиск по `return View("Test")` выводит совпадения */Views/Home/Test.cshtml*, */Views/home/test.cshtml* и другие варианты с различными сочетаниями регистра.</span><span class="sxs-lookup"><span data-stu-id="c6308-290">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="c6308-291">В файловых системах, учитывающих регистр (например, в Linux, OSX и где используется `EmbeddedFileProvider`), поиск выполняется с учетом регистра.</span><span class="sxs-lookup"><span data-stu-id="c6308-291">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="c6308-292">Например, поиск по `return View("Test")` дает точное совпадение */Views/Home/Test.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c6308-292">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="c6308-293">Предварительно скомпилированные представления: в ASP.NET Core 2.0 и более поздних версиях поиск в предварительно скомпилированных представлениях выполняется без учета регистра во всех операционных системах.</span><span class="sxs-lookup"><span data-stu-id="c6308-293">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="c6308-294">Это поведение аналогично поведению поставщика физических файлов в Windows.</span><span class="sxs-lookup"><span data-stu-id="c6308-294">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="c6308-295">Если два предварительно скомпилированных представления отличаются только регистром, результат поиска является недетерминированным.</span><span class="sxs-lookup"><span data-stu-id="c6308-295">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="c6308-296">Разработчикам рекомендуется использовать для файлов и каталогов тот же регистр, что и для:</span><span class="sxs-lookup"><span data-stu-id="c6308-296">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

    * <span data-ttu-id="c6308-297">имен областей, контроллеров и действий;</span><span class="sxs-lookup"><span data-stu-id="c6308-297">Area, controller, and action names.</span></span> 
    * <span data-ttu-id="c6308-298">страниц Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="c6308-298">Razor Pages.</span></span>
    
<span data-ttu-id="c6308-299">Совпадающий регистр гарантирует, что развертываемые службы смогут находить свои представления вне зависимости от используемой файловой системы.</span><span class="sxs-lookup"><span data-stu-id="c6308-299">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>
