---
title: Предотвращения межсайтовых сценариев (XSS) в ASP.NET Core
author: rick-anderson
description: Дополнительные сведения о межсайтовых сценариев (XSS) и методы для устранения этой уязвимости в приложении ASP.NET Core.
ms.author: riande
ms.date: 10/02/2018
uid: security/cross-site-scripting
ms.openlocfilehash: e937ce47b7151155197cd607832eeb6bf62e3a19
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577448"
---
# <a name="prevent-cross-site-scripting-xss-in-aspnet-core"></a><span data-ttu-id="cc0c1-103">Предотвращения межсайтовых сценариев (XSS) в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cc0c1-103">Prevent Cross-Site Scripting (XSS) in ASP.NET Core</span></span>

<span data-ttu-id="cc0c1-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="cc0c1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="cc0c1-105">Межузловых сценариев (XSS) — это уязвимость системы безопасности, что позволяет злоумышленнику поместите скриптах на стороне клиента (обычно JavaScript) в веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-105">Cross-Site Scripting (XSS) is a security vulnerability which enables an attacker to place client side scripts (usually JavaScript) into web pages.</span></span> <span data-ttu-id="cc0c1-106">При других пользователей загрузить затронутые страницы, которые будут запущены сценарии злоумышленники, что позволит ему похищать файлы cookie и маркеры сеанса изменить содержание веб-страницы через обработке модели DOM или перенаправить браузер на другую страницу.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-106">When other users load affected pages the attackers scripts will run, enabling the attacker to steal cookies and session tokens, change the contents of the web page through DOM manipulation or redirect the browser to another page.</span></span> <span data-ttu-id="cc0c1-107">Уязвимости к XSS, как правило, происходят, когда приложение принимает предоставляемые пользователем данные и выводит его на странице без проверки, кодирования или экранирование его.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-107">XSS vulnerabilities generally occur when an application takes user input and outputs it in a page without validating, encoding or escaping it.</span></span>

## <a name="protecting-your-application-against-xss"></a><span data-ttu-id="cc0c1-108">Защита приложения от межсайтовых Сценариев</span><span class="sxs-lookup"><span data-stu-id="cc0c1-108">Protecting your application against XSS</span></span>

<span data-ttu-id="cc0c1-109">AT основных уровня XSS осуществляется честных и приложение в виде вставки `<script>` тег в готовом для просмотра страницы или путем вставки `On*` событий в элемент.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-109">At a basic level XSS works by tricking your application into inserting a `<script>` tag into your rendered page, or by inserting an `On*` event into an element.</span></span> <span data-ttu-id="cc0c1-110">Разработчикам следует использовать следующие действия для предотвращения во избежание внесения XSS в свое приложение.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-110">Developers should use the following prevention steps to avoid introducing XSS into their application.</span></span>

1. <span data-ttu-id="cc0c1-111">Никогда не помещайте непроверенных данных в HTML входные данные, если не следовать остальной части указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-111">Never put untrusted data into your HTML input, unless you follow the rest of the steps below.</span></span> <span data-ttu-id="cc0c1-112">Непроверенных данных — это любые данные, которые могут управляться злоумышленник, формы ввода данных HTML, строки запроса, заголовки HTTP, даже данные, исходный код из базы данных, как злоумышленник может иметь возможность нарушения безопасности базы данных, даже если они не может нарушить приложения.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-112">Untrusted data is any data that may be controlled by an attacker, HTML form inputs, query strings, HTTP headers, even data sourced from a database as an attacker may be able to breach your database even if they cannot breach your application.</span></span>

2. <span data-ttu-id="cc0c1-113">Перед добавлением непроверенных данных внутри HTML-элемент убедитесь, что он имеет кодировку HTML.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-113">Before putting untrusted data inside an HTML element ensure it's HTML encoded.</span></span> <span data-ttu-id="cc0c1-114">HTML-кодирования принимает символы, такие как &lt; и изменяет их в безопасном формат наподобие &amp;lt;</span><span class="sxs-lookup"><span data-stu-id="cc0c1-114">HTML encoding takes characters such as &lt; and changes them into a safe form like &amp;lt;</span></span>

3. <span data-ttu-id="cc0c1-115">Перед переводом непроверенных данных в HTML-атрибут убедитесь, что он является атрибутом HTML в кодировке.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-115">Before putting untrusted data into an HTML attribute ensure it's HTML attribute encoded.</span></span> <span data-ttu-id="cc0c1-116">Кодировка атрибута HTML является надмножеством HTML-кодирования и кодирует дополнительные символы, такие как "и".</span><span class="sxs-lookup"><span data-stu-id="cc0c1-116">HTML attribute encoding is a superset of HTML encoding and encodes additional characters such as " and '.</span></span>

4. <span data-ttu-id="cc0c1-117">Перед переводом непроверенных данных в JavaScript помещает данные в HTML-элемент, содержимое которых получить во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-117">Before putting untrusted data into JavaScript place the data in an HTML element whose contents you retrieve at runtime.</span></span> <span data-ttu-id="cc0c1-118">Если это невозможно, то убедитесь, данные кодируются JavaScript.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-118">If this isn't possible then ensure the data is JavaScript encoded.</span></span> <span data-ttu-id="cc0c1-119">Кодирование JavaScript принимает опасных символов для JavaScript и заменяет их символом их hex, например &lt; должен кодироваться как `\u003C`.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-119">JavaScript encoding takes dangerous characters for JavaScript and replaces them with their hex, for example &lt; would be encoded as `\u003C`.</span></span>

5. <span data-ttu-id="cc0c1-120">Перед добавлением непроверенных данных в строку запроса URL-адрес, убедитесь, что это URL-кодированием.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-120">Before putting untrusted data into a URL query string ensure it's URL encoded.</span></span>

## <a name="html-encoding-using-razor"></a><span data-ttu-id="cc0c1-121">Кодировка HTML, с помощью Razor</span><span class="sxs-lookup"><span data-stu-id="cc0c1-121">HTML Encoding using Razor</span></span>

<span data-ttu-id="cc0c1-122">Подсистема Razor, используемый в MVC автоматически кодирует все выходные данные источником переменные, если вы не работаете усердной блокирует его таким образом.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-122">The Razor engine used in MVC automatically encodes all output sourced from variables, unless you work really hard to prevent it doing so.</span></span> <span data-ttu-id="cc0c1-123">Он использует атрибут HTML, правила кодирования при использовании *@* директива.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-123">It uses HTML Attribute encoding rules whenever you use the *@* directive.</span></span> <span data-ttu-id="cc0c1-124">Как HTML кодировка атрибута является надмножеством HTML-кодирования, это означает, что вам не нужно заботиться, нужно ли использовать HTML-кодирования или кодировка атрибута HTML.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-124">As HTML attribute encoding is a superset of HTML encoding this means you don't have to concern yourself with whether you should use HTML encoding or HTML attribute encoding.</span></span> <span data-ttu-id="cc0c1-125">Необходимо убедиться, что используются только в контексте HTML, не в том случае, при попытке вставить ненадежных входных данных непосредственно в JavaScript.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-125">You must ensure that you only use @ in an HTML context, not when attempting to insert untrusted input directly into JavaScript.</span></span> <span data-ttu-id="cc0c1-126">Вспомогательные функции тегов также будет кодирование входных данных, используемых в параметров tag.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-126">Tag helpers will also encode input you use in tag parameters.</span></span>

<span data-ttu-id="cc0c1-127">Выполните следующее представление Razor.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-127">Take the following Razor view:</span></span>

```cshtml
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

<span data-ttu-id="cc0c1-128">Это представление выводит содержимое *untrustedInput* переменной.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-128">This view outputs the contents of the *untrustedInput* variable.</span></span> <span data-ttu-id="cc0c1-129">Эта переменная содержит некоторые символы, которые используются в атак XSS, а именно &lt;, "и &gt;.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-129">This variable includes some characters which are used in XSS attacks, namely &lt;, " and &gt;.</span></span> <span data-ttu-id="cc0c1-130">Анализ источника показаны выводимые данные кодируются как:</span><span class="sxs-lookup"><span data-stu-id="cc0c1-130">Examining the source shows the rendered output encoded as:</span></span>

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> <span data-ttu-id="cc0c1-131">ASP.NET Core MVC предоставляет `HtmlString` класса, которой не автоматически после вывода.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-131">ASP.NET Core MVC provides an `HtmlString` class which isn't automatically encoded upon output.</span></span> <span data-ttu-id="cc0c1-132">Это никогда не можно использовать в сочетании с ненадежных входных данных, так как это будет предоставлять уязвимость XSS.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-132">This should never be used in combination with untrusted input as this will expose an XSS vulnerability.</span></span>

## <a name="javascript-encoding-using-razor"></a><span data-ttu-id="cc0c1-133">JavaScript кодирование с помощью Razor</span><span class="sxs-lookup"><span data-stu-id="cc0c1-133">Javascript Encoding using Razor</span></span>

<span data-ttu-id="cc0c1-134">Могут возникнуть ситуации, нужно вставить значение в JavaScript для обработки в представлении.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-134">There may be times you want to insert a value into JavaScript to process in your view.</span></span> <span data-ttu-id="cc0c1-135">Это можно сделать двумя способами.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-135">There are two ways to do this.</span></span> <span data-ttu-id="cc0c1-136">Наиболее безопасным способом для вставки значений — разместить значение в атрибуте данных тега и его можно получить в JavaScript.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-136">The safest way to insert  values is to place the value in a data attribute of a tag and retrieve it in your JavaScript.</span></span> <span data-ttu-id="cc0c1-137">Пример:</span><span class="sxs-lookup"><span data-stu-id="cc0c1-137">For example:</span></span>

```cshtml
@{
       var untrustedInput = "<\"123\">";
   }

   <div
       id="injectedData"
       data-untrustedinput="@untrustedInput" />

   <script>
     var injectedData = document.getElementById("injectedData");

     // All clients
     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     // HTML 5 clients only
     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

<span data-ttu-id="cc0c1-138">Этот скрипт создаст следующий код HTML</span><span class="sxs-lookup"><span data-stu-id="cc0c1-138">This will produce the following HTML</span></span>

```html
<div
     id="injectedData"
     data-untrustedinput="&lt;&quot;123&quot;&gt;" />

   <script>
     var injectedData = document.getElementById("injectedData");

     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

<span data-ttu-id="cc0c1-139">Если она выполняется, отображающего следующую процедуру.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-139">Which, when it runs, will render the following;</span></span>

```none
<"123">
   <"123">
   ```

<span data-ttu-id="cc0c1-140">Можно также напрямую вызвать кодировщик JavaScript:</span><span class="sxs-lookup"><span data-stu-id="cc0c1-140">You can also call the JavaScript encoder directly:</span></span>

```cshtml
@using System.Text.Encodings.Web;
   @inject JavaScriptEncoder encoder;

   @{
       var untrustedInput = "<\"123\">";
   }

   <script>
       document.write("@encoder.Encode(untrustedInput)");
   </script>
   ```

<span data-ttu-id="cc0c1-141">Это сделает в браузере, следующим образом:</span><span class="sxs-lookup"><span data-stu-id="cc0c1-141">This will render in the browser as follows;</span></span>

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> <span data-ttu-id="cc0c1-142">Не объединять ненадежных входных данных в JavaScript для создания элементов DOM.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-142">Don't concatenate untrusted input in JavaScript to create DOM elements.</span></span> <span data-ttu-id="cc0c1-143">Следует использовать `createElement()` и соответствующим образом, такие как присвоение значений свойствам `node.TextContent=`, или использовать `element.SetAttribute()` / `element[attribute]=` в противном случае можно предоставить себе коду основе DOM XSS.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-143">You should use `createElement()` and assign property values appropriately such as `node.TextContent=`, or use `element.SetAttribute()`/`element[attribute]=` otherwise you expose yourself to DOM-based XSS.</span></span>

## <a name="accessing-encoders-in-code"></a><span data-ttu-id="cc0c1-144">Доступ к кодировщиков в коде</span><span class="sxs-lookup"><span data-stu-id="cc0c1-144">Accessing encoders in code</span></span>

<span data-ttu-id="cc0c1-145">Кодировщики HTML, JavaScript и URL-адрес, доступные для кода двумя способами, вы можете внедрить их через [внедрения зависимостей](xref:fundamentals/dependency-injection) или вы можете использовать кодировщики по умолчанию, содержащиеся в `System.Text.Encodings.Web` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-145">The HTML, JavaScript and URL encoders are available to your code in two ways, you can inject them via [dependency injection](xref:fundamentals/dependency-injection) or you can use the default encoders contained in the `System.Text.Encodings.Web` namespace.</span></span> <span data-ttu-id="cc0c1-146">При использовании кодировщики по умолчанию любой применяются к диапазонов символов следует рассматривать как безопасные вступят в силу — кодировщики по умолчанию используется безопасный из возможных правил кодирования.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-146">If you use the default encoders then any  you applied to character ranges to be treated as safe won't take effect - the default encoders use the safest encoding rules possible.</span></span>

<span data-ttu-id="cc0c1-147">Чтобы использовать настраиваемые кодировщики через внедрение Зависимостей в конструкторах займет *HtmlEncoder*, *JavaScriptEncoder* и *UrlEncoder* параметр соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-147">To use the configurable encoders via DI your constructors should take an *HtmlEncoder*, *JavaScriptEncoder* and *UrlEncoder* parameter as appropriate.</span></span> <span data-ttu-id="cc0c1-148">Например, если для</span><span class="sxs-lookup"><span data-stu-id="cc0c1-148">For example;</span></span>

```csharp
public class HomeController : Controller
   {
       HtmlEncoder _htmlEncoder;
       JavaScriptEncoder _javaScriptEncoder;
       UrlEncoder _urlEncoder;

       public HomeController(HtmlEncoder htmlEncoder,
                             JavaScriptEncoder javascriptEncoder,
                             UrlEncoder urlEncoder)
       {
           _htmlEncoder = htmlEncoder;
           _javaScriptEncoder = javascriptEncoder;
           _urlEncoder = urlEncoder;
       }
   }
   ```

## <a name="encoding-url-parameters"></a><span data-ttu-id="cc0c1-149">Параметры кодирования URL-адрес</span><span class="sxs-lookup"><span data-stu-id="cc0c1-149">Encoding URL Parameters</span></span>

<span data-ttu-id="cc0c1-150">Если вы хотите создать строку запроса URL-адрес с ненадежных входных данных в качестве значения используйте `UrlEncoder` для кодирования значения.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-150">If you want to build a URL query string with untrusted input as a value use the `UrlEncoder` to encode the value.</span></span> <span data-ttu-id="cc0c1-151">Например, примененная к объекту директива</span><span class="sxs-lookup"><span data-stu-id="cc0c1-151">For example,</span></span>

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

<span data-ttu-id="cc0c1-152">После кодирования encodedValue переменная будет содержать `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-152">After encoding the encodedValue variable will contain `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span></span> <span data-ttu-id="cc0c1-153">Пробелов, кавычки, знаки препинания и другие небезопасные символы процента кодируется в их шестнадцатеричное значение, например символ пробела станет % 20.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-153">Spaces, quotes, punctuation and other unsafe characters will be percent encoded to their hexadecimal value, for example a space character will become %20.</span></span>

>[!WARNING]
> <span data-ttu-id="cc0c1-154">Не используйте ненадежных входных данных как часть пути URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-154">Don't use untrusted input as part of a URL path.</span></span> <span data-ttu-id="cc0c1-155">Всегда выполнять передачу ненадежных входных данных как значения строки запроса.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-155">Always pass untrusted input as a query string value.</span></span>

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a><span data-ttu-id="cc0c1-156">Настройка кодировщиков</span><span class="sxs-lookup"><span data-stu-id="cc0c1-156">Customizing the Encoders</span></span>

<span data-ttu-id="cc0c1-157">По умолчанию кодировщиков используйте ограниченный диапазон Юникода для основной латиницы списка надежных отправителей и кодирования всех символов вне этого диапазона как эквиваленты код символа.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-157">By default encoders use a safe list limited to the Basic Latin Unicode range and encode all characters outside of that range as their character code equivalents.</span></span> <span data-ttu-id="cc0c1-158">Это поведение также влияет на Razor TagHelper и HtmlHelper отрисовки, так как он будет использовать кодировщики для вывода строк.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-158">This behavior also affects Razor TagHelper and HtmlHelper rendering as it will use the encoders to output your strings.</span></span>

<span data-ttu-id="cc0c1-159">Обуславливается для защиты от неизвестных или будущих браузера ошибок (предыдущих ошибок в обозревателе приводило к ошибкам вверх синтаксического анализа на основе во время обработки неанглийские символы).</span><span class="sxs-lookup"><span data-stu-id="cc0c1-159">The reasoning behind this is to protect against unknown or future browser bugs (previous browser bugs have tripped up parsing based on the processing of non-English characters).</span></span> <span data-ttu-id="cc0c1-160">Если веб-сайт усложняет использование Нелатинские символы, такие как китайский, кириллица, или другими это скорее всего, не необходимого поведения.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-160">If your web site makes heavy use of non-Latin characters, such as Chinese, Cyrillic or others this is probably not the behavior you want.</span></span>

<span data-ttu-id="cc0c1-161">Вы можете настроить списки безопасных кодировщика для включения Юникода диапазонами подходящими для вашего приложения, во время запуска, в `ConfigureServices()`.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-161">You can customize the encoder safe lists to include Unicode ranges appropriate to your application during startup, in `ConfigureServices()`.</span></span>

<span data-ttu-id="cc0c1-162">Например, с помощью конфигурации по умолчанию, можно использовать Razor HtmlHelper следующим образом;</span><span class="sxs-lookup"><span data-stu-id="cc0c1-162">For example, using the default configuration you might use a Razor HtmlHelper like so;</span></span>

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

<span data-ttu-id="cc0c1-163">При просмотре исходного кода веб-страницы, будет видно, что он визуализирован следующим образом, с помощью закодирован; текст на китайском языке</span><span class="sxs-lookup"><span data-stu-id="cc0c1-163">When you view the source of the web page you will see it has been rendered as follows, with the Chinese text encoded;</span></span>

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

<span data-ttu-id="cc0c1-164">Чтобы расширить знаки, обрабатываемые как безопасный кодировщиком, необходимо вставить следующую строку в `ConfigureServices()` метод в `startup.cs`;</span><span class="sxs-lookup"><span data-stu-id="cc0c1-164">To widen the characters treated as safe by the encoder you would insert the following line into the `ConfigureServices()` method in `startup.cs`;</span></span>

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

<span data-ttu-id="cc0c1-165">В этом примере можно расширить список безопасных элементов для включения CjkUnifiedIdeographs диапазон Юникода.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-165">This example widens the safe list to include the Unicode Range CjkUnifiedIdeographs.</span></span> <span data-ttu-id="cc0c1-166">Теперь стал бы выводимые данные</span><span class="sxs-lookup"><span data-stu-id="cc0c1-166">The rendered output would now become</span></span>

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

<span data-ttu-id="cc0c1-167">Список безопасных диапазоны задаются в виде таблиц кодировок Юникода, не языки.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-167">Safe list ranges are specified as Unicode code charts, not languages.</span></span> <span data-ttu-id="cc0c1-168">[Стандарта Юникод](http://unicode.org/) имеет список [кода диаграммы](http://www.unicode.org/charts/index.html) можно использовать для поиска таблица, содержащая символы.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-168">The [Unicode standard](http://unicode.org/) has a list of [code charts](http://www.unicode.org/charts/index.html) you can use to find the chart containing your characters.</span></span> <span data-ttu-id="cc0c1-169">Каждый из которых, Html, JavaScript и URL-адрес, необходимо настроить по отдельности.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-169">Each encoder, Html, JavaScript and Url, must be configured separately.</span></span>

> [!NOTE]
> <span data-ttu-id="cc0c1-170">Настройка надежных влияет только на кодировщиков, отправляемый с помощью внедрения Зависимостей.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-170">Customization of the safe list only affects encoders sourced via DI.</span></span> <span data-ttu-id="cc0c1-171">Если прямой доступ к кодировщик через `System.Text.Encodings.Web.*Encoder.Default` затем по умолчанию, Basic Latin только safelist будет использоваться.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-171">If you directly access an encoder via `System.Text.Encodings.Web.*Encoder.Default` then the default, Basic Latin only safelist will be used.</span></span>

## <a name="where-should-encoding-take-place"></a><span data-ttu-id="cc0c1-172">Куда следует поместить кодирования take?</span><span class="sxs-lookup"><span data-stu-id="cc0c1-172">Where should encoding take place?</span></span>

<span data-ttu-id="cc0c1-173">Общие применяемый практикой является кодирование выполняется точке выходные данные, что закодированные значения никогда не должны храниться в базе данных.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-173">The general accepted practice is that encoding takes place at the point of output and encoded values should never be stored in a database.</span></span> <span data-ttu-id="cc0c1-174">Кодирование точке выходные данные можно изменить использование данных, например, с HTML на значения строки запроса.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-174">Encoding at the point of output allows you to change the use of data, for example, from HTML to a query string value.</span></span> <span data-ttu-id="cc0c1-175">Также позволяет легко найти данные без необходимости кодирования значений перед поиском и позволяет воспользоваться преимуществами изменения или исправления, внесенные в кодировщиков.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-175">It also enables you to easily search your data without having to encode values before searching and allows you to take advantage of any changes or bug fixes made to encoders.</span></span>

## <a name="validation-as-an-xss-prevention-technique"></a><span data-ttu-id="cc0c1-176">Проверку, что метод защиты от XSS</span><span class="sxs-lookup"><span data-stu-id="cc0c1-176">Validation as an XSS prevention technique</span></span>

<span data-ttu-id="cc0c1-177">Проверка может быть полезным инструментом ограничение атак XSS.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-177">Validation can be a useful tool in limiting XSS attacks.</span></span> <span data-ttu-id="cc0c1-178">Например числовая строка, содержащая только символы 0 – 9, не вызовет срабатывания атаки XSS.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-178">For example, a numeric string containing only the characters 0-9 won't trigger an XSS attack.</span></span> <span data-ttu-id="cc0c1-179">Проверка усложняется при приеме HTML во входных данных пользователя.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-179">Validation becomes more complicated when accepting HTML in user input.</span></span> <span data-ttu-id="cc0c1-180">Синтаксический анализ вводимых данных HTML сложно, если вообще.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-180">Parsing HTML input is difficult, if not impossible.</span></span> <span data-ttu-id="cc0c1-181">Markdown, в сочетании с средство синтаксического анализа, которое удаляет внедренный HTML-код, более безопасный вариант для принятия широкие возможности ввода.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-181">Markdown, coupled with a parser that strips embedded HTML, is a safer option for accepting rich input.</span></span> <span data-ttu-id="cc0c1-182">Никогда не используют только проверку.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-182">Never rely on validation alone.</span></span> <span data-ttu-id="cc0c1-183">Всегда кодировать ненадежных входных данных перед выводом, независимо от того, какие проверки или очистки будет выполнена.</span><span class="sxs-lookup"><span data-stu-id="cc0c1-183">Always encode untrusted input before output, no matter what validation or sanitization has been performed.</span></span>
