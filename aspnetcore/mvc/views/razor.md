---
title: Справочник по синтаксису Razor для ASP.NET Core
author: rick-anderson
description: Сведения о синтаксисе разметки Razor для внедрения в веб-страницы серверного кода.
ms.author: riande
ms.date: 06/12/2019
uid: mvc/views/razor
ms.openlocfilehash: 634623aa6df860a2ff728da4e65d277326db37e2
ms.sourcegitcommit: 051f068c78931432e030b60094c38376d64d013e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2019
ms.locfileid: "68440357"
---
# <a name="razor-syntax-reference-for-aspnet-core"></a><span data-ttu-id="3df7f-103">Справочник по синтаксису Razor для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3df7f-103">Razor syntax reference for ASP.NET Core</span></span>

<span data-ttu-id="3df7f-104">Авторы: [Рик Андерсон (Rick Anderson)](https://twitter.com/RickAndMSFT), [Люк Лэтем (Luke Latham)](https://github.com/guardrex), [Тейлор Маллен (Taylor Mullen)](https://twitter.com/ntaylormullen) и [Дэн Викарел (Dan Vicarel)](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="3df7f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="3df7f-105">Razor — это синтаксис разметки для внедрения в веб-страницы серверного кода.</span><span class="sxs-lookup"><span data-stu-id="3df7f-105">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="3df7f-106">Синтаксис Razor состоит из разметки Razor, C# и HTML.</span><span class="sxs-lookup"><span data-stu-id="3df7f-106">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="3df7f-107">Файлы, содержащие Razor, обычно имеют расширение *CSHTML*.</span><span class="sxs-lookup"><span data-stu-id="3df7f-107">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="3df7f-108">Отрисовка HTML</span><span class="sxs-lookup"><span data-stu-id="3df7f-108">Rendering HTML</span></span>

<span data-ttu-id="3df7f-109">Языком Razor по умолчанию является HTML.</span><span class="sxs-lookup"><span data-stu-id="3df7f-109">The default Razor language is HTML.</span></span> <span data-ttu-id="3df7f-110">Отрисовка HTML из разметки Razor ничем не отличается от отрисовки из HTML-файла.</span><span class="sxs-lookup"><span data-stu-id="3df7f-110">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span> <span data-ttu-id="3df7f-111">Сервер отображает разметку HTML в *CSHTML*-файлах Razor без изменений.</span><span class="sxs-lookup"><span data-stu-id="3df7f-111">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="3df7f-112">Синтаксис Razor</span><span class="sxs-lookup"><span data-stu-id="3df7f-112">Razor syntax</span></span>

<span data-ttu-id="3df7f-113">Razor поддерживает C# и использует символ `@` для перехода с HTML на C#.</span><span class="sxs-lookup"><span data-stu-id="3df7f-113">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="3df7f-114">Razor вычисляет выражения C# и отображает их в выходных данных HTML.</span><span class="sxs-lookup"><span data-stu-id="3df7f-114">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="3df7f-115">Если за символом `@` следует [зарезервированное ключевое слово Razor](#razor-reserved-keywords), он переходит на разметку Razor.</span><span class="sxs-lookup"><span data-stu-id="3df7f-115">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="3df7f-116">В противном случае он переходит на обычный C#.</span><span class="sxs-lookup"><span data-stu-id="3df7f-116">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="3df7f-117">В качестве escape-символа для `@` в разметке Razor используйте второй символ `@`:</span><span class="sxs-lookup"><span data-stu-id="3df7f-117">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="3df7f-118">Код будет отображен в HTML с одним символом `@`:</span><span class="sxs-lookup"><span data-stu-id="3df7f-118">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="3df7f-119">HTML-атрибуты и содержимое, включающие адреса электронной почты, не расценивают символ `@` как символ перехода.</span><span class="sxs-lookup"><span data-stu-id="3df7f-119">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="3df7f-120">В следующем примере синтаксический анализ Razor не затрагивает адреса электронной почты:</span><span class="sxs-lookup"><span data-stu-id="3df7f-120">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="3df7f-121">Неявные выражения Razor</span><span class="sxs-lookup"><span data-stu-id="3df7f-121">Implicit Razor expressions</span></span>

<span data-ttu-id="3df7f-122">Неявные выражения Razor начинаются с символа `@`, за которым следует код C#:</span><span class="sxs-lookup"><span data-stu-id="3df7f-122">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="3df7f-123">Неявные выражения не должны содержать пробелов. Исключением является ключевое слово C# `await`.</span><span class="sxs-lookup"><span data-stu-id="3df7f-123">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="3df7f-124">Если оператор C# имеет четкое окончание, пробелы вставлять можно:</span><span class="sxs-lookup"><span data-stu-id="3df7f-124">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="3df7f-125">Неявные выражения **не могут** содержать универсальные шаблоны C#, так как символы в угловых скобках (`<>`) интерпретируются как тег HTML.</span><span class="sxs-lookup"><span data-stu-id="3df7f-125">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="3df7f-126">Следующий код является **недопустимым**:</span><span class="sxs-lookup"><span data-stu-id="3df7f-126">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="3df7f-127">Приведенный выше код вызывает ошибку компилятора примерно следующего вида:</span><span class="sxs-lookup"><span data-stu-id="3df7f-127">The preceding code generates a compiler error similar to one of the following:</span></span>

* <span data-ttu-id="3df7f-128">Элемент "int" не был закрыт.</span><span class="sxs-lookup"><span data-stu-id="3df7f-128">The "int" element wasn't closed.</span></span> <span data-ttu-id="3df7f-129">Все элементы должны быть самозакрывающимися или иметь соответствующий закрывающий тег.</span><span class="sxs-lookup"><span data-stu-id="3df7f-129">All elements must be either self-closing or have a matching end tag.</span></span>
* <span data-ttu-id="3df7f-130">Не удается преобразовать группу методов "GenericMethod" в не являющийся делегатом тип "object".</span><span class="sxs-lookup"><span data-stu-id="3df7f-130">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="3df7f-131">Предполагалось вызывать этот метод?</span><span class="sxs-lookup"><span data-stu-id="3df7f-131">Did you intend to invoke the method?\`</span></span>

<span data-ttu-id="3df7f-132">Вызовы универсальных методов должны быть заключены в [явное выражение Razor](#explicit-razor-expressions) или [блок кода Razor](#razor-code-blocks).</span><span class="sxs-lookup"><span data-stu-id="3df7f-132">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="3df7f-133">Явные выражения Razor</span><span class="sxs-lookup"><span data-stu-id="3df7f-133">Explicit Razor expressions</span></span>

<span data-ttu-id="3df7f-134">Явные выражения Razor состоят из символа `@` с соответствующими открывающими и закрывающими скобками.</span><span class="sxs-lookup"><span data-stu-id="3df7f-134">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="3df7f-135">Для отображения времени прошлой недели используется следующая разметка Razor:</span><span class="sxs-lookup"><span data-stu-id="3df7f-135">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="3df7f-136">Любое содержимое в скобках `@()` вычисляется и отображается в выходных данных.</span><span class="sxs-lookup"><span data-stu-id="3df7f-136">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="3df7f-137">Неявные выражения, описанные в предыдущем разделе, обычно не содержат пробелов.</span><span class="sxs-lookup"><span data-stu-id="3df7f-137">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="3df7f-138">В следующем коде из значения текущего времени неделя не вычитается:</span><span class="sxs-lookup"><span data-stu-id="3df7f-138">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="3df7f-139">Код отображает следующий HTML:</span><span class="sxs-lookup"><span data-stu-id="3df7f-139">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="3df7f-140">Явные выражения позволяют объединять результат своего выполнения с дополнительным текстом:</span><span class="sxs-lookup"><span data-stu-id="3df7f-140">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="3df7f-141">Без явного выражения `<p>Age@joe.Age</p>` обрабатывается как адрес электронной почты, и на выходе отображается `<p>Age@joe.Age</p>`.</span><span class="sxs-lookup"><span data-stu-id="3df7f-141">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="3df7f-142">Если же текст написан как явное выражение, то вы получите `<p>Age33</p>`.</span><span class="sxs-lookup"><span data-stu-id="3df7f-142">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>

<span data-ttu-id="3df7f-143">Вы можете использовать явные выражения для отображения выходных данных из универсальных методов в файлах *CSHTML*.</span><span class="sxs-lookup"><span data-stu-id="3df7f-143">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="3df7f-144">В следующем примере показано, как исправить ошибку, показанную ранее и вызванную скобками в универсальном шаблоне C#.</span><span class="sxs-lookup"><span data-stu-id="3df7f-144">The following markup shows how to correct the error shown earlier caused by the brackets of a C# generic.</span></span> <span data-ttu-id="3df7f-145">Код записывается как явное выражение:</span><span class="sxs-lookup"><span data-stu-id="3df7f-145">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a><span data-ttu-id="3df7f-146">Кодирование выражений</span><span class="sxs-lookup"><span data-stu-id="3df7f-146">Expression encoding</span></span>

<span data-ttu-id="3df7f-147">Выражения C#, которые имеют строковое выходное значение, кодируются в формате HTML.</span><span class="sxs-lookup"><span data-stu-id="3df7f-147">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="3df7f-148">Выражения C#, которые имеют значение `IHtmlContent`, обрабатываются непосредственно с помощью `IHtmlContent.WriteTo`.</span><span class="sxs-lookup"><span data-stu-id="3df7f-148">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="3df7f-149">Выражения C#, которые не имеют выходное значение `IHtmlContent`, преобразуются в строку с помощью `ToString` и кодируются перед обработкой.</span><span class="sxs-lookup"><span data-stu-id="3df7f-149">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="3df7f-150">Код отображает следующий HTML:</span><span class="sxs-lookup"><span data-stu-id="3df7f-150">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="3df7f-151">HTML отображается в обозревателе следующим образом:</span><span class="sxs-lookup"><span data-stu-id="3df7f-151">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="3df7f-152">Выходные данные `HtmlHelper.Raw` не кодируются, но отображаются в виде разметки HTML.</span><span class="sxs-lookup"><span data-stu-id="3df7f-152">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="3df7f-153">Использование `HtmlHelper.Raw` с непроверенными входными данными пользователя представляет угрозу безопасности.</span><span class="sxs-lookup"><span data-stu-id="3df7f-153">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="3df7f-154">Эти входные данные могут содержать вредоносный код JavaScript или другие эксплойты.</span><span class="sxs-lookup"><span data-stu-id="3df7f-154">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="3df7f-155">Очистка вводимых пользователем данных является сложной задачей.</span><span class="sxs-lookup"><span data-stu-id="3df7f-155">Sanitizing user input is difficult.</span></span> <span data-ttu-id="3df7f-156">Старайтесь не использовать `HtmlHelper.Raw` с такими данными.</span><span class="sxs-lookup"><span data-stu-id="3df7f-156">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="3df7f-157">Код отображает следующий HTML:</span><span class="sxs-lookup"><span data-stu-id="3df7f-157">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="3df7f-158">Блоки кода Razor</span><span class="sxs-lookup"><span data-stu-id="3df7f-158">Razor code blocks</span></span>

<span data-ttu-id="3df7f-159">Блоки кода Razor начинаются с символа `@` и заключены в фигурные скобки `{}`.</span><span class="sxs-lookup"><span data-stu-id="3df7f-159">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="3df7f-160">В отличие от выражений код C# внутри блоков кода не обрабатывается.</span><span class="sxs-lookup"><span data-stu-id="3df7f-160">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="3df7f-161">Блоки кода и выражения в представлении используют общую область и определяются в следующем порядке:</span><span class="sxs-lookup"><span data-stu-id="3df7f-161">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

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

<span data-ttu-id="3df7f-162">Код отображает следующий HTML:</span><span class="sxs-lookup"><span data-stu-id="3df7f-162">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="3df7f-163">В блоках кода объявите [локальные функции](/dotnet/csharp/programming-guide/classes-and-structs/local-functions) с помощью разметки для использования в качестве методов создания шаблонов:</span><span class="sxs-lookup"><span data-stu-id="3df7f-163">In code blocks, declare [local functions](/dotnet/csharp/programming-guide/classes-and-structs/local-functions) with markup to serve as templating methods:</span></span>

```cshtml
@{
    void RenderName(string name)
    {
        <p>Name: <strong>@name</strong></p>
    }

    RenderName("Mahatma Gandhi");
    RenderName("Martin Luther King, Jr.");
}
```

<span data-ttu-id="3df7f-164">Код отображает следующий HTML:</span><span class="sxs-lookup"><span data-stu-id="3df7f-164">The code renders the following HTML:</span></span>

```html
<p>Name: <strong>Mahatma Gandhi</strong></p>
<p>Name: <strong>Martin Luther King, Jr.</strong></p>
```

::: moniker-end

### <a name="implicit-transitions"></a><span data-ttu-id="3df7f-165">Неявные переходы</span><span class="sxs-lookup"><span data-stu-id="3df7f-165">Implicit transitions</span></span>

<span data-ttu-id="3df7f-166">В блоке кода языком по умолчанию является C#, но страница Razor Page может вернуться на HTML:</span><span class="sxs-lookup"><span data-stu-id="3df7f-166">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="3df7f-167">Явный переход с разделителями</span><span class="sxs-lookup"><span data-stu-id="3df7f-167">Explicit delimited transition</span></span>

<span data-ttu-id="3df7f-168">Чтобы определить подраздел блока кода, который должен отрисовывать HTML, окружите подлежащие отображению символы тегами Razor **\<text>** :</span><span class="sxs-lookup"><span data-stu-id="3df7f-168">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="3df7f-169">Используйте этот способ для отрисовки HTML, не заключенного в HTML-теги.</span><span class="sxs-lookup"><span data-stu-id="3df7f-169">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="3df7f-170">Без тега HTML или Razor возникает ошибка времени выполнения Razor.</span><span class="sxs-lookup"><span data-stu-id="3df7f-170">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="3df7f-171">Тег **\<text>** хорошо подходит для контроля пробелов при отрисовке содержимого:</span><span class="sxs-lookup"><span data-stu-id="3df7f-171">The **\<text>** tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="3df7f-172">Отрисовывается только содержимое между тегами **\<text>** .</span><span class="sxs-lookup"><span data-stu-id="3df7f-172">Only the content between the **\<text>** tag is rendered.</span></span>
* <span data-ttu-id="3df7f-173">В выходных данных HTML пробелы до или после тега **\<text>** не отображаются.</span><span class="sxs-lookup"><span data-stu-id="3df7f-173">No whitespace before or after the **\<text>** tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="3df7f-174">Явные переходы по строкам с @:</span><span class="sxs-lookup"><span data-stu-id="3df7f-174">Explicit Line Transition with @:</span></span>

<span data-ttu-id="3df7f-175">Для отрисовки оставшейся части строки в виде HTML внутри блока кода используйте синтаксис "`@:`":</span><span class="sxs-lookup"><span data-stu-id="3df7f-175">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="3df7f-176">Если в коде отсутствует `@:`, возникает ошибка среды выполнения Razor.</span><span class="sxs-lookup"><span data-stu-id="3df7f-176">Without the `@:` in the code, a Razor runtime error is generated.</span></span>

<span data-ttu-id="3df7f-177">Предупреждение: Дополнительные символы `@` в файле Razor могут вызвать ошибки компилятора в последующих операторах блока.</span><span class="sxs-lookup"><span data-stu-id="3df7f-177">Warning: Extra `@` characters in a Razor file can cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="3df7f-178">Эти ошибки компилятора может быть трудно проанализировать, так как ошибка фактически возникает раньше, чем указано.</span><span class="sxs-lookup"><span data-stu-id="3df7f-178">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span> <span data-ttu-id="3df7f-179">Чаще всего эта ошибка появляется после объединения множества неявных или явных выражений в один блок кода.</span><span class="sxs-lookup"><span data-stu-id="3df7f-179">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="3df7f-180">Управляющие структуры</span><span class="sxs-lookup"><span data-stu-id="3df7f-180">Control structures</span></span>

<span data-ttu-id="3df7f-181">Управляющие структуры являются расширением блоков кода.</span><span class="sxs-lookup"><span data-stu-id="3df7f-181">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="3df7f-182">Все аспекты блоков кода (переход на разметку, встроенный код C#) также относятся к следующим структурам.</span><span class="sxs-lookup"><span data-stu-id="3df7f-182">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="3df7f-183">Условные выражения @if, else if, else и @switch</span><span class="sxs-lookup"><span data-stu-id="3df7f-183">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="3df7f-184">`@if` контролирует, когда нужно запускать код:</span><span class="sxs-lookup"><span data-stu-id="3df7f-184">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="3df7f-185">Для `else` и `else if` символ `@` не требуется:</span><span class="sxs-lookup"><span data-stu-id="3df7f-185">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="3df7f-186">В следующей разметке показано использование оператора switch:</span><span class="sxs-lookup"><span data-stu-id="3df7f-186">The following markup shows how to use a switch statement:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="3df7f-187">Циклы @for, @foreach, @while и @do while</span><span class="sxs-lookup"><span data-stu-id="3df7f-187">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="3df7f-188">Операторы выполнения цикла позволяют выполнять отрисовку шаблонного HTML.</span><span class="sxs-lookup"><span data-stu-id="3df7f-188">Templated HTML can be rendered with looping control statements.</span></span> <span data-ttu-id="3df7f-189">Отрисовка списка людей:</span><span class="sxs-lookup"><span data-stu-id="3df7f-189">To render a list of people:</span></span>

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

<span data-ttu-id="3df7f-190">Поддерживаются следующие операторы выполнения цикла:</span><span class="sxs-lookup"><span data-stu-id="3df7f-190">The following looping statements are supported:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="3df7f-191">Составной оператор @using</span><span class="sxs-lookup"><span data-stu-id="3df7f-191">Compound @using</span></span>

<span data-ttu-id="3df7f-192">В C# оператор `using` позволяет обеспечить использование какого-то объекта.</span><span class="sxs-lookup"><span data-stu-id="3df7f-192">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="3df7f-193">В Razor этот механизм позволяет создавать вспомогательные функции HTML, включающие дополнительное содержимое.</span><span class="sxs-lookup"><span data-stu-id="3df7f-193">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="3df7f-194">В следующем коде вспомогательные функции HTML используют оператор `@using` для создания тега формы:</span><span class="sxs-lookup"><span data-stu-id="3df7f-194">In the following code, HTML Helpers render a form tag with the `@using` statement:</span></span>

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

<span data-ttu-id="3df7f-195">Для действий на уровне области можно использовать [вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="3df7f-195">Scope-level actions can be performed with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="3df7f-196">@try, catch, finally</span><span class="sxs-lookup"><span data-stu-id="3df7f-196">@try, catch, finally</span></span>

<span data-ttu-id="3df7f-197">Обработка исключений выполняется так же, как в C#:</span><span class="sxs-lookup"><span data-stu-id="3df7f-197">Exception handling is similar to C#:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="3df7f-198">Razor позволяет защищать важные разделы при помощи операторов блокировки:</span><span class="sxs-lookup"><span data-stu-id="3df7f-198">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="3df7f-199">Комментарии</span><span class="sxs-lookup"><span data-stu-id="3df7f-199">Comments</span></span>

<span data-ttu-id="3df7f-200">Razor поддерживает комментарии C# и HTML:</span><span class="sxs-lookup"><span data-stu-id="3df7f-200">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="3df7f-201">Код отображает следующий HTML:</span><span class="sxs-lookup"><span data-stu-id="3df7f-201">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="3df7f-202">Сервер удаляет комментарии Razor перед отображением веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="3df7f-202">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="3df7f-203">Для разделения комментариев Razor использует `@*  *@`.</span><span class="sxs-lookup"><span data-stu-id="3df7f-203">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="3df7f-204">Следующий код закомментирован, поэтому сервер не отрисовывает разметку:</span><span class="sxs-lookup"><span data-stu-id="3df7f-204">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="3df7f-205">Директивы</span><span class="sxs-lookup"><span data-stu-id="3df7f-205">Directives</span></span>

<span data-ttu-id="3df7f-206">Директивы Razor представлены неявными выражениями с зарезервированными ключевыми словами после символа `@`.</span><span class="sxs-lookup"><span data-stu-id="3df7f-206">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="3df7f-207">Как правило, директива изменяет способ анализа представления или открывает доступ к дополнительным функциям.</span><span class="sxs-lookup"><span data-stu-id="3df7f-207">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="3df7f-208">Узнав, каким образом Razor создает код для представления, вы сможете легко понять принципы работы директив.</span><span class="sxs-lookup"><span data-stu-id="3df7f-208">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="3df7f-209">Код создает класс, аналогичный следующему:</span><span class="sxs-lookup"><span data-stu-id="3df7f-209">The code generates a class similar to the following:</span></span>

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

<span data-ttu-id="3df7f-210">Сведения о просмотре этого класса приводятся в разделе [Просмотр Razor-класса C#, созданного для представления](#inspect-the-razor-c-class-generated-for-a-view) далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="3df7f-210">Later in this article, the section [Inspect the Razor C# class generated for a view](#inspect-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

<a name="using"></a>

### <a name="using"></a>@using

<span data-ttu-id="3df7f-211">Директива `@using` добавляет директиву C# `using` в созданное представление:</span><span class="sxs-lookup"><span data-stu-id="3df7f-211">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="3df7f-212">Директива `@model` определяет тип модели, передаваемой в представление:</span><span class="sxs-lookup"><span data-stu-id="3df7f-212">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="3df7f-213">В MVC-приложении ASP.NET Core, созданном с отдельными учетными записями пользователей, представление *Views/Account/Login.cshtml* содержит следующее объявление модели:</span><span class="sxs-lookup"><span data-stu-id="3df7f-213">In an ASP.NET Core MVC app created with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="3df7f-214">Созданный класс наследует от `RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="3df7f-214">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="3df7f-215">Для доступа к модели, переданной в представление, Razor предоставляет свойство `Model`:</span><span class="sxs-lookup"><span data-stu-id="3df7f-215">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="3df7f-216">Директива `@model` задает тип этого свойства.</span><span class="sxs-lookup"><span data-stu-id="3df7f-216">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="3df7f-217">Директива указывает `T` в `RazorPage<T>` — созданном классе, на основе которого создается производное представление.</span><span class="sxs-lookup"><span data-stu-id="3df7f-217">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="3df7f-218">Если директива `@model` не указана, свойство `Model` имеет тип `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="3df7f-218">If the `@model` directive isn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="3df7f-219">Значение модели передается из контроллера в представление.</span><span class="sxs-lookup"><span data-stu-id="3df7f-219">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="3df7f-220">Дополнительные сведения см. в разделе [Строго типизированные модели и &commat;ключевое слово модели](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).</span><span class="sxs-lookup"><span data-stu-id="3df7f-220">For more information, see [Strongly typed models and the &commat;model keyword](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="3df7f-221">Директива `@inherits` позволяет полностью управлять классом, которому наследует представление:</span><span class="sxs-lookup"><span data-stu-id="3df7f-221">The `@inherits` directive provides full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="3df7f-222">Следующий код показывает настраиваемый тип страницы Razor:</span><span class="sxs-lookup"><span data-stu-id="3df7f-222">The following code is a custom Razor page type:</span></span>

[!code-csharp[](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="3df7f-223">В представлении отображается `CustomText`:</span><span class="sxs-lookup"><span data-stu-id="3df7f-223">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="3df7f-224">Код отображает следующий HTML:</span><span class="sxs-lookup"><span data-stu-id="3df7f-224">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 <span data-ttu-id="3df7f-225">`@model` и `@inherits` могут использоваться в одном представлении.</span><span class="sxs-lookup"><span data-stu-id="3df7f-225">`@model` and `@inherits` can be used in the same view.</span></span> <span data-ttu-id="3df7f-226">`@inherits` может находиться в файле *_ViewImports.cshtml*, который импортируется представлением:</span><span class="sxs-lookup"><span data-stu-id="3df7f-226">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="3df7f-227">Следующий код показывает пример строго типизированного представления:</span><span class="sxs-lookup"><span data-stu-id="3df7f-227">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="3df7f-228">Если передать в модель "rick@contoso.com", представление создает следующую разметку HTML:</span><span class="sxs-lookup"><span data-stu-id="3df7f-228">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject

<span data-ttu-id="3df7f-229">Директива `@inject` позволяет странице Razor внедрять в представление службу из [контейнера службы](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="3df7f-229">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="3df7f-230">Дополнительные сведения: [Внедрение зависимостей в представления](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="3df7f-230">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="3df7f-231">Директива `@functions` позволяет странице Razor добавлять в представление блок кода C#:</span><span class="sxs-lookup"><span data-stu-id="3df7f-231">The `@functions` directive enables a Razor Page to add a C# code block to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="3df7f-232">Пример:</span><span class="sxs-lookup"><span data-stu-id="3df7f-232">For example:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="3df7f-233">Код создает следующую разметку HTML:</span><span class="sxs-lookup"><span data-stu-id="3df7f-233">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="3df7f-234">Следующий код показывает созданный Razor-класс C#:</span><span class="sxs-lookup"><span data-stu-id="3df7f-234">The following code is the generated Razor C# class:</span></span>

[!code-csharp[](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="3df7f-235">Методы `@functions` служат в качестве методов создания шаблонов при наличии разметки:</span><span class="sxs-lookup"><span data-stu-id="3df7f-235">`@functions` methods serve as templating methods when they have markup:</span></span>

```cshtml
@{
    RenderName("Mahatma Gandhi");
    RenderName("Martin Luther King, Jr.");
}

@functions {
    private void RenderName(string name)
    {
        <p>Name: <strong>@name</strong></p>
    }
}
```

<span data-ttu-id="3df7f-236">Код отображает следующий HTML:</span><span class="sxs-lookup"><span data-stu-id="3df7f-236">The code renders the following HTML:</span></span>

```html
<p>Name: <strong>Mahatma Gandhi</strong></p>
<p>Name: <strong>Martin Luther King, Jr.</strong></p>
```

::: moniker-end

### <a name="attribute"></a>@attribute

<span data-ttu-id="3df7f-237">Директива `@attribute` добавляет данный атрибут к классу созданной страницы или представления.</span><span class="sxs-lookup"><span data-stu-id="3df7f-237">The `@attribute` directive adds the given attribute to the class of the generated page or view.</span></span> <span data-ttu-id="3df7f-238">В следующем примере добавляется атрибут `[Authorize]`:</span><span class="sxs-lookup"><span data-stu-id="3df7f-238">The following example adds the `[Authorize]` attribute:</span></span>

```cshtml
@attribute [Authorize]
```

### <a name="namespace"></a>@namespace

<span data-ttu-id="3df7f-239">Директива `@namespace` задает пространство имен класса созданной страницы или представления:</span><span class="sxs-lookup"><span data-stu-id="3df7f-239">The `@namespace` directive sets the namespace of the class of the generated page or view:</span></span>

```cshtml
@namespace Your.Namespace.Here
```

<span data-ttu-id="3df7f-240">Если страница или представление импортирует API с помощью директивы `@namespace`, пространство имен исходного файла задается относительно этого пространства имен.</span><span class="sxs-lookup"><span data-stu-id="3df7f-240">If a page or view imports API with an `@namespace` directive, the original file's namespace is set relative to that namespace.</span></span> 

<span data-ttu-id="3df7f-241">Если *MyApp/Pages/\_ViewImports.cshtml* содержит `@namespace Hello.World`, пространство имен страниц или представлений, которые импортируют пространство имен `Hello.World`, задается, как показано в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="3df7f-241">If *MyApp/Pages/\_ViewImports.cshtml* contains `@namespace Hello.World`, the namespace of pages or views that import the `Hello.World` namespace is set as shown in the following table.</span></span>

| <span data-ttu-id="3df7f-242">Страница (или представление)</span><span class="sxs-lookup"><span data-stu-id="3df7f-242">Page (or view)</span></span>                     | <span data-ttu-id="3df7f-243">Пространство имен</span><span class="sxs-lookup"><span data-stu-id="3df7f-243">Namespace</span></span>               |
| ---------------------------------- | ----------------------- |
| <span data-ttu-id="3df7f-244">*MyApp/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3df7f-244">*MyApp/Pages/Index.cshtml*</span></span>         | `Hello.World`           |
| <span data-ttu-id="3df7f-245">*MyApp/Pages/MorePages/Bar.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3df7f-245">*MyApp/Pages/MorePages/Bar.cshtml*</span></span> | `Hello.World.MorePages` |

<span data-ttu-id="3df7f-246">Если у нескольких файлов импорта есть директива `@namespace`, используется файл, ближайший к странице или представлению в цепочке каталога.</span><span class="sxs-lookup"><span data-stu-id="3df7f-246">If multiple import files have the `@namespace` directive, the file closest to the page or view in the directory chain is used.</span></span>

### <a name="section"></a>@section

<span data-ttu-id="3df7f-247">Директива `@section` используется в сочетании с [макетом](xref:mvc/views/layout) и позволяет страницам или представлениям отображать содержимое в различных частях HTML-страницы.</span><span class="sxs-lookup"><span data-stu-id="3df7f-247">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable pages or views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="3df7f-248">Дополнительные сведения: [Разделы](xref:mvc/views/layout#layout-sections-label).</span><span class="sxs-lookup"><span data-stu-id="3df7f-248">For more information, see [Sections](xref:mvc/views/layout#layout-sections-label).</span></span>

## <a name="templated-razor-delegates"></a><span data-ttu-id="3df7f-249">Шаблонные делегаты Razor</span><span class="sxs-lookup"><span data-stu-id="3df7f-249">Templated Razor delegates</span></span>

<span data-ttu-id="3df7f-250">Шаблоны Razor позволяют определить фрагмент кода пользовательского интерфейса в следующем формате:</span><span class="sxs-lookup"><span data-stu-id="3df7f-250">Razor templates allow you to define a UI snippet with the following format:</span></span>

```cshtml
@<tag>...</tag>
```

<span data-ttu-id="3df7f-251">Следующий пример показывает, как указать шаблонный делегат Razor в виде <xref:System.Func%602>.</span><span class="sxs-lookup"><span data-stu-id="3df7f-251">The following example illustrates how to specify a templated Razor delegate as a <xref:System.Func%602>.</span></span> <span data-ttu-id="3df7f-252">[Динамический тип](/dotnet/csharp/programming-guide/types/using-type-dynamic) указывается для параметра метода, инкапсулируемого делегатом.</span><span class="sxs-lookup"><span data-stu-id="3df7f-252">The [dynamic type](/dotnet/csharp/programming-guide/types/using-type-dynamic) is specified for the parameter of the method that the delegate encapsulates.</span></span> <span data-ttu-id="3df7f-253">[Тип объекта](/dotnet/csharp/language-reference/keywords/object) указывается в качестве возвращаемого значения делегата.</span><span class="sxs-lookup"><span data-stu-id="3df7f-253">An [object type](/dotnet/csharp/language-reference/keywords/object) is specified as the return value of the delegate.</span></span> <span data-ttu-id="3df7f-254">Этот шаблон используется с <xref:System.Collections.Generic.List%601>объекта `Pet`, имеющим свойство `Name`.</span><span class="sxs-lookup"><span data-stu-id="3df7f-254">The template is used with a <xref:System.Collections.Generic.List%601> of `Pet` that has a `Name` property.</span></span>

```csharp
public class Pet
{
    public string Name { get; set; }
}
```

```cshtml
@{
    Func<dynamic, object> petTemplate = @<p>You have a pet named <strong>@item.Name</strong>.</p>;

    var pets = new List<Pet>
    {
        new Pet { Name = "Rin Tin Tin" },
        new Pet { Name = "Mr. Bigglesworth" },
        new Pet { Name = "K-9" }
    };
}
```

<span data-ttu-id="3df7f-255">Шаблон отрисовывается с использованием `pets`, предоставляемого оператором `foreach`:</span><span class="sxs-lookup"><span data-stu-id="3df7f-255">The template is rendered with `pets` supplied by a `foreach` statement:</span></span>

```cshtml
@foreach (var pet in pets)
{
    @petTemplate(pet)
}
```

<span data-ttu-id="3df7f-256">Отображенные выходные данные:</span><span class="sxs-lookup"><span data-stu-id="3df7f-256">Rendered output:</span></span>

```html
<p>You have a pet named <strong>Rin Tin Tin</strong>.</p>
<p>You have a pet named <strong>Mr. Bigglesworth</strong>.</p>
<p>You have a pet named <strong>K-9</strong>.</p>
```

<span data-ttu-id="3df7f-257">Вы также можете предоставить встроенный шаблон Razor в качестве аргумента для метода.</span><span class="sxs-lookup"><span data-stu-id="3df7f-257">You can also supply an inline Razor template as an argument to a method.</span></span> <span data-ttu-id="3df7f-258">В следующем примере метод `Repeat` получает шаблон Razor.</span><span class="sxs-lookup"><span data-stu-id="3df7f-258">In the following example, the `Repeat` method receives a Razor template.</span></span> <span data-ttu-id="3df7f-259">Метод использует этот шаблон для создания HTML-содержимого с повторениями элементов из списка:</span><span class="sxs-lookup"><span data-stu-id="3df7f-259">The method uses the template to produce HTML content with repeats of items supplied from a list:</span></span>

```cshtml
@using Microsoft.AspNetCore.Html

@functions {
    public static IHtmlContent Repeat(IEnumerable<dynamic> items, int times,
        Func<dynamic, IHtmlContent> template)
    {
        var html = new HtmlContentBuilder();

        foreach (var item in items)
        {
            for (var i = 0; i < times; i++)
            {
                html.AppendHtml(template(item));
            }
        }

        return html;
    }
}
```

<span data-ttu-id="3df7f-260">С использованием списка домашних животных из предыдущего примера метод `Repeat` вызывается следующим образом:</span><span class="sxs-lookup"><span data-stu-id="3df7f-260">Using the list of pets from the prior example, the `Repeat` method is called with:</span></span>

* <span data-ttu-id="3df7f-261"><xref:System.Collections.Generic.List%601> объекта `Pet`.</span><span class="sxs-lookup"><span data-stu-id="3df7f-261"><xref:System.Collections.Generic.List%601> of `Pet`.</span></span>
* <span data-ttu-id="3df7f-262">Количество повторений для каждого домашнего животного.</span><span class="sxs-lookup"><span data-stu-id="3df7f-262">Number of times to repeat each pet.</span></span>
* <span data-ttu-id="3df7f-263">Встроенный шаблон, используемый для перечисления элементов неупорядоченного списка.</span><span class="sxs-lookup"><span data-stu-id="3df7f-263">Inline template to use for the list items of an unordered list.</span></span>

```cshtml
<ul>
    @Repeat(pets, 3, @<li>@item.Name</li>)
</ul>
```

<span data-ttu-id="3df7f-264">Отображенные выходные данные:</span><span class="sxs-lookup"><span data-stu-id="3df7f-264">Rendered output:</span></span>

```html
<ul>
    <li>Rin Tin Tin</li>
    <li>Rin Tin Tin</li>
    <li>Rin Tin Tin</li>
    <li>Mr. Bigglesworth</li>
    <li>Mr. Bigglesworth</li>
    <li>Mr. Bigglesworth</li>
    <li>K-9</li>
    <li>K-9</li>
    <li>K-9</li>
</ul>
```

## <a name="tag-helpers"></a><span data-ttu-id="3df7f-265">Вспомогательные функции тегов</span><span class="sxs-lookup"><span data-stu-id="3df7f-265">Tag Helpers</span></span>

<span data-ttu-id="3df7f-266">Существует три директивы, которые относятся к [вспомогательным функциям тегов](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="3df7f-266">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="3df7f-267">Директива</span><span class="sxs-lookup"><span data-stu-id="3df7f-267">Directive</span></span> | <span data-ttu-id="3df7f-268">Функция</span><span class="sxs-lookup"><span data-stu-id="3df7f-268">Function</span></span> |
| --------- | -------- |
| [<span data-ttu-id="3df7f-269">&commat;addTagHelper</span><span class="sxs-lookup"><span data-stu-id="3df7f-269">&commat;addTagHelper</span></span>](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="3df7f-270">Делает вспомогательные функции тегов доступными в представлении.</span><span class="sxs-lookup"><span data-stu-id="3df7f-270">Makes Tag Helpers available to a view.</span></span> |
| [<span data-ttu-id="3df7f-271">&commat;removeTagHelper</span><span class="sxs-lookup"><span data-stu-id="3df7f-271">&commat;removeTagHelper</span></span>](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="3df7f-272">Удаляет из представления вспомогательные функции тегов, добавленные ранее.</span><span class="sxs-lookup"><span data-stu-id="3df7f-272">Removes Tag Helpers previously added from a view.</span></span> |
| [<span data-ttu-id="3df7f-273">&commat;tagHelperPrefix</span><span class="sxs-lookup"><span data-stu-id="3df7f-273">&commat;tagHelperPrefix</span></span>](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="3df7f-274">Задает префикс тега, который активирует поддержку вспомогательной функции тега и ее использования в явном виде.</span><span class="sxs-lookup"><span data-stu-id="3df7f-274">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="3df7f-275">Зарезервированные ключевые слова Razor</span><span class="sxs-lookup"><span data-stu-id="3df7f-275">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="3df7f-276">Ключевые слова Razor</span><span class="sxs-lookup"><span data-stu-id="3df7f-276">Razor keywords</span></span>

* <span data-ttu-id="3df7f-277">page (требуется ASP.NET Core 2.0 или более поздние версии)</span><span class="sxs-lookup"><span data-stu-id="3df7f-277">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="3df7f-278">namespace</span><span class="sxs-lookup"><span data-stu-id="3df7f-278">namespace</span></span>
* <span data-ttu-id="3df7f-279">functions</span><span class="sxs-lookup"><span data-stu-id="3df7f-279">functions</span></span>
* <span data-ttu-id="3df7f-280">inherits</span><span class="sxs-lookup"><span data-stu-id="3df7f-280">inherits</span></span>
* <span data-ttu-id="3df7f-281">model</span><span class="sxs-lookup"><span data-stu-id="3df7f-281">model</span></span>
* <span data-ttu-id="3df7f-282">section</span><span class="sxs-lookup"><span data-stu-id="3df7f-282">section</span></span>
* <span data-ttu-id="3df7f-283">helper (сейчас не поддерживается в ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="3df7f-283">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="3df7f-284">В качестве escape-символа для ключевых слов Razor используется `@(Razor Keyword)` (например, `@(functions)`).</span><span class="sxs-lookup"><span data-stu-id="3df7f-284">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="3df7f-285">Ключевые слова C# в Razor</span><span class="sxs-lookup"><span data-stu-id="3df7f-285">C# Razor keywords</span></span>

* <span data-ttu-id="3df7f-286">case</span><span class="sxs-lookup"><span data-stu-id="3df7f-286">case</span></span>
* <span data-ttu-id="3df7f-287">do</span><span class="sxs-lookup"><span data-stu-id="3df7f-287">do</span></span>
* <span data-ttu-id="3df7f-288">default</span><span class="sxs-lookup"><span data-stu-id="3df7f-288">default</span></span>
* <span data-ttu-id="3df7f-289">for</span><span class="sxs-lookup"><span data-stu-id="3df7f-289">for</span></span>
* <span data-ttu-id="3df7f-290">foreach</span><span class="sxs-lookup"><span data-stu-id="3df7f-290">foreach</span></span>
* <span data-ttu-id="3df7f-291">if</span><span class="sxs-lookup"><span data-stu-id="3df7f-291">if</span></span>
* <span data-ttu-id="3df7f-292">else</span><span class="sxs-lookup"><span data-stu-id="3df7f-292">else</span></span>
* <span data-ttu-id="3df7f-293">lock</span><span class="sxs-lookup"><span data-stu-id="3df7f-293">lock</span></span>
* <span data-ttu-id="3df7f-294">switch</span><span class="sxs-lookup"><span data-stu-id="3df7f-294">switch</span></span>
* <span data-ttu-id="3df7f-295">try</span><span class="sxs-lookup"><span data-stu-id="3df7f-295">try</span></span>
* <span data-ttu-id="3df7f-296">catch</span><span class="sxs-lookup"><span data-stu-id="3df7f-296">catch</span></span>
* <span data-ttu-id="3df7f-297">finally</span><span class="sxs-lookup"><span data-stu-id="3df7f-297">finally</span></span>
* <span data-ttu-id="3df7f-298">using</span><span class="sxs-lookup"><span data-stu-id="3df7f-298">using</span></span>
* <span data-ttu-id="3df7f-299">while</span><span class="sxs-lookup"><span data-stu-id="3df7f-299">while</span></span>

<span data-ttu-id="3df7f-300">Для ключевых слов C# в Razor требуется двойной escape-символ: `@(@C# Razor Keyword)` (например, `@(@case)`).</span><span class="sxs-lookup"><span data-stu-id="3df7f-300">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="3df7f-301">Первый `@` предназначен для обхода синтаксического анализа Razor,</span><span class="sxs-lookup"><span data-stu-id="3df7f-301">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="3df7f-302">а второй `@` — для обхода C#.</span><span class="sxs-lookup"><span data-stu-id="3df7f-302">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="3df7f-303">Зарезервированные ключевые слова, не используемые в Razor</span><span class="sxs-lookup"><span data-stu-id="3df7f-303">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="3df7f-304">класс</span><span class="sxs-lookup"><span data-stu-id="3df7f-304">class</span></span>

## <a name="inspect-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="3df7f-305">Просмотр Razor-класса C#, созданного для представления</span><span class="sxs-lookup"><span data-stu-id="3df7f-305">Inspect the Razor C# class generated for a view</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="3df7f-306">При использовании пакета SDK для .NET Core 2.1 или более поздней версии [пакет SDK для Razor](xref:razor-pages/sdk) обрабатывает компиляцию файлов Razor.</span><span class="sxs-lookup"><span data-stu-id="3df7f-306">With .NET Core SDK 2.1 or later, the [Razor SDK](xref:razor-pages/sdk) handles compilation of Razor files.</span></span> <span data-ttu-id="3df7f-307">При сборке проекта пакет SDK для Razor создает каталог *obj/<конфигурация_сборки>/<моникер_целевой_платформы>/Razor* в корневом каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="3df7f-307">When building a project, the Razor SDK generates an *obj/<build_configuration>/<target_framework_moniker>/Razor* directory in the project root.</span></span> <span data-ttu-id="3df7f-308">Структура каталогов в каталоге *Razor* отражает структуру каталогов проекта.</span><span class="sxs-lookup"><span data-stu-id="3df7f-308">The directory structure within the *Razor* directory mirrors the project's directory structure.</span></span>

<span data-ttu-id="3df7f-309">Рассмотрим следующую структуру каталогов в проекте ASP.NET Core 2.1 Razor Pages, предназначенном для .NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="3df7f-309">Consider the following directory structure in an ASP.NET Core 2.1 Razor Pages project targeting .NET Core 2.1:</span></span>

* <span data-ttu-id="3df7f-310">**Areas/**</span><span class="sxs-lookup"><span data-stu-id="3df7f-310">**Areas/**</span></span>
  * <span data-ttu-id="3df7f-311">**Admin/**</span><span class="sxs-lookup"><span data-stu-id="3df7f-311">**Admin/**</span></span>
    * <span data-ttu-id="3df7f-312">**Pages/**</span><span class="sxs-lookup"><span data-stu-id="3df7f-312">**Pages/**</span></span>
      * <span data-ttu-id="3df7f-313">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3df7f-313">*Index.cshtml*</span></span>
      * <span data-ttu-id="3df7f-314">*Index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="3df7f-314">*Index.cshtml.cs*</span></span>
* <span data-ttu-id="3df7f-315">**Pages/**</span><span class="sxs-lookup"><span data-stu-id="3df7f-315">**Pages/**</span></span>
  * <span data-ttu-id="3df7f-316">**Shared/**</span><span class="sxs-lookup"><span data-stu-id="3df7f-316">**Shared/**</span></span>
    * <span data-ttu-id="3df7f-317">*_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3df7f-317">*_Layout.cshtml*</span></span>
  * <span data-ttu-id="3df7f-318">*_ViewImports.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3df7f-318">*_ViewImports.cshtml*</span></span>
  * <span data-ttu-id="3df7f-319">*_ViewStart.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3df7f-319">*_ViewStart.cshtml*</span></span>
  * <span data-ttu-id="3df7f-320">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3df7f-320">*Index.cshtml*</span></span>
  * <span data-ttu-id="3df7f-321">*Index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="3df7f-321">*Index.cshtml.cs*</span></span>

<span data-ttu-id="3df7f-322">При сборке проекта в конфигурации *Отладка* создается следующий каталог *obj*:</span><span class="sxs-lookup"><span data-stu-id="3df7f-322">Building the project in *Debug* configuration yields the following *obj* directory:</span></span>

* <span data-ttu-id="3df7f-323">**obj/**</span><span class="sxs-lookup"><span data-stu-id="3df7f-323">**obj/**</span></span>
  * <span data-ttu-id="3df7f-324">**Debug/**</span><span class="sxs-lookup"><span data-stu-id="3df7f-324">**Debug/**</span></span>
    * <span data-ttu-id="3df7f-325">**netcoreapp2.1/**</span><span class="sxs-lookup"><span data-stu-id="3df7f-325">**netcoreapp2.1/**</span></span>
      * <span data-ttu-id="3df7f-326">**Razor/**</span><span class="sxs-lookup"><span data-stu-id="3df7f-326">**Razor/**</span></span>
        * <span data-ttu-id="3df7f-327">**Areas/**</span><span class="sxs-lookup"><span data-stu-id="3df7f-327">**Areas/**</span></span>
          * <span data-ttu-id="3df7f-328">**Admin/**</span><span class="sxs-lookup"><span data-stu-id="3df7f-328">**Admin/**</span></span>
            * <span data-ttu-id="3df7f-329">**Pages/**</span><span class="sxs-lookup"><span data-stu-id="3df7f-329">**Pages/**</span></span>
              * <span data-ttu-id="3df7f-330">*Index.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="3df7f-330">*Index.g.cshtml.cs*</span></span>
        * <span data-ttu-id="3df7f-331">**Pages/**</span><span class="sxs-lookup"><span data-stu-id="3df7f-331">**Pages/**</span></span>
          * <span data-ttu-id="3df7f-332">**Shared/**</span><span class="sxs-lookup"><span data-stu-id="3df7f-332">**Shared/**</span></span>
            * <span data-ttu-id="3df7f-333">*_Layout.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="3df7f-333">*_Layout.g.cshtml.cs*</span></span>
          * <span data-ttu-id="3df7f-334">*_ViewImports.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="3df7f-334">*_ViewImports.g.cshtml.cs*</span></span>
          * <span data-ttu-id="3df7f-335">*_ViewStart.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="3df7f-335">*_ViewStart.g.cshtml.cs*</span></span>
          * <span data-ttu-id="3df7f-336">*Index.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="3df7f-336">*Index.g.cshtml.cs*</span></span>

<span data-ttu-id="3df7f-337">Чтобы просмотреть созданный класс для *Pages/Index.cshtml* откройте *obj/Debug/netcoreapp2.1/Razor/Pages/Index.g.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="3df7f-337">To view the generated class for *Pages/Index.cshtml*, open *obj/Debug/netcoreapp2.1/Razor/Pages/Index.g.cshtml.cs*.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="3df7f-338">Добавьте в MVC-проект ASP.NET следующий класс:</span><span class="sxs-lookup"><span data-stu-id="3df7f-338">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="3df7f-339">В `Startup.ConfigureServices` переопределите класс `RazorTemplateEngine`, добавленный MVC, классом `CustomTemplateEngine`:</span><span class="sxs-lookup"><span data-stu-id="3df7f-339">In `Startup.ConfigureServices`, override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="3df7f-340">Установите точку останова в операторе `return csharpDocument;` класса `CustomTemplateEngine`.</span><span class="sxs-lookup"><span data-stu-id="3df7f-340">Set a breakpoint on the `return csharpDocument;` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="3df7f-341">Когда выполнение программы остановится в этой точке, просмотрите значение `generatedCode`.</span><span class="sxs-lookup"><span data-stu-id="3df7f-341">When program execution stops at the breakpoint, view the value of `generatedCode`.</span></span>

![Представление generatedCode в визуализаторе текста](razor/_static/tvr.png)

::: moniker-end

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="3df7f-343">Поиск данных в представлениях и учет регистра</span><span class="sxs-lookup"><span data-stu-id="3df7f-343">View lookups and case sensitivity</span></span>

<span data-ttu-id="3df7f-344">Модуль представлений Razor позволяет искать в представлениях данные с учетом регистра.</span><span class="sxs-lookup"><span data-stu-id="3df7f-344">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="3df7f-345">Однако фактический поиск зависит от используемой файловой системы.</span><span class="sxs-lookup"><span data-stu-id="3df7f-345">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="3df7f-346">Источники в виде файлов:</span><span class="sxs-lookup"><span data-stu-id="3df7f-346">File based source:</span></span>
  * <span data-ttu-id="3df7f-347">В операционных системах, файловые системы которых не учитывают регистр (например, Windows), поиск поставщика физических файлов не зависит от регистра.</span><span class="sxs-lookup"><span data-stu-id="3df7f-347">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="3df7f-348">Например, поиск по `return View("Test")` выводит совпадения */Views/Home/Test.cshtml*, */Views/home/test.cshtml* и другие варианты с различными сочетаниями регистра.</span><span class="sxs-lookup"><span data-stu-id="3df7f-348">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="3df7f-349">В файловых системах, учитывающих регистр (например, в Linux, OSX и где используется `EmbeddedFileProvider`), поиск выполняется с учетом регистра.</span><span class="sxs-lookup"><span data-stu-id="3df7f-349">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="3df7f-350">Например, поиск по `return View("Test")` дает точное совпадение */Views/Home/Test.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3df7f-350">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="3df7f-351">Предварительно скомпилированные представления: в ASP.NET Core 2.0 и более поздних версиях поиск в предварительно скомпилированных представлениях выполняется без учета регистра во всех операционных системах.</span><span class="sxs-lookup"><span data-stu-id="3df7f-351">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="3df7f-352">Это поведение аналогично поведению поставщика физических файлов в Windows.</span><span class="sxs-lookup"><span data-stu-id="3df7f-352">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="3df7f-353">Если два предварительно скомпилированных представления отличаются только регистром, результат поиска является недетерминированным.</span><span class="sxs-lookup"><span data-stu-id="3df7f-353">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="3df7f-354">Разработчикам рекомендуется использовать для файлов и каталогов тот же регистр, что и для:</span><span class="sxs-lookup"><span data-stu-id="3df7f-354">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

* <span data-ttu-id="3df7f-355">имен областей, контроллеров и действий;</span><span class="sxs-lookup"><span data-stu-id="3df7f-355">Area, controller, and action names.</span></span>
* <span data-ttu-id="3df7f-356">страниц Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="3df7f-356">Razor Pages.</span></span>

<span data-ttu-id="3df7f-357">Совпадающий регистр гарантирует, что развертываемые службы смогут находить свои представления вне зависимости от используемой файловой системы.</span><span class="sxs-lookup"><span data-stu-id="3df7f-357">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>
