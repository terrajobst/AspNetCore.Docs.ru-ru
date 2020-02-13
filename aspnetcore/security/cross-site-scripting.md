---
title: Предотвращение межсайтовых сценариев (XSS) в ASP.NET Core
author: rick-anderson
description: Сведения о межузловых сценариях (XSS) и методах решения этой уязвимости в ASP.NET Core приложении.
ms.author: riande
ms.date: 10/02/2018
uid: security/cross-site-scripting
ms.openlocfilehash: 1d6f605dc336d8768b8a47e4995f119d198a61af
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172638"
---
# <a name="prevent-cross-site-scripting-xss-in-aspnet-core"></a><span data-ttu-id="96c37-103">Предотвращение межсайтовых сценариев (XSS) в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="96c37-103">Prevent Cross-Site Scripting (XSS) in ASP.NET Core</span></span>

<span data-ttu-id="96c37-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="96c37-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="96c37-105">Межсайтовые сценарии (XSS) — это уязвимость системы безопасности, которая позволяет злоумышленнику размещать сценарии на стороне клиента (обычно JavaScript) на веб-страницах.</span><span class="sxs-lookup"><span data-stu-id="96c37-105">Cross-Site Scripting (XSS) is a security vulnerability which enables an attacker to place client side scripts (usually JavaScript) into web pages.</span></span> <span data-ttu-id="96c37-106">Когда другие пользователи загружают затронутые страницы, запускаются сценарии злоумышленника, позволяющие злоумышленнику украсть файлы cookie и токены сеансов, изменить содержимое веб-страницы с помощью манипуляций DOM или перенаправить браузер на другую страницу.</span><span class="sxs-lookup"><span data-stu-id="96c37-106">When other users load affected pages the attacker's scripts will run, enabling the attacker to steal cookies and session tokens, change the contents of the web page through DOM manipulation or redirect the browser to another page.</span></span> <span data-ttu-id="96c37-107">Уязвимости XSS обычно возникают, когда приложение принимает введенные пользователем данные и выводит его на страницу без проверки, кодирования или экранирования.</span><span class="sxs-lookup"><span data-stu-id="96c37-107">XSS vulnerabilities generally occur when an application takes user input and outputs it to a page without validating, encoding or escaping it.</span></span>

## <a name="protecting-your-application-against-xss"></a><span data-ttu-id="96c37-108">Защита приложения с помощью XSS</span><span class="sxs-lookup"><span data-stu-id="96c37-108">Protecting your application against XSS</span></span>

<span data-ttu-id="96c37-109">На уровне XSS уровня "базовый" работает путем того, как приложение вставляет тег `<script>` на отображаемую страницу или вставляет событие `On*` в элемент.</span><span class="sxs-lookup"><span data-stu-id="96c37-109">At a basic level XSS works by tricking your application into inserting a `<script>` tag into your rendered page, or by inserting an `On*` event into an element.</span></span> <span data-ttu-id="96c37-110">Разработчики должны использовать следующие шаги по предотвращению, чтобы избежать введения XSS в приложение.</span><span class="sxs-lookup"><span data-stu-id="96c37-110">Developers should use the following prevention steps to avoid introducing XSS into their application.</span></span>

1. <span data-ttu-id="96c37-111">Никогда не размещайте недоверенные данные в входных данных HTML, если не выполнить остальные действия, описанные ниже.</span><span class="sxs-lookup"><span data-stu-id="96c37-111">Never put untrusted data into your HTML input, unless you follow the rest of the steps below.</span></span> <span data-ttu-id="96c37-112">Недоверенные данные — это любые данные, которыми может управлять злоумышленник, входные данные HTML-форм, строки запросов, заголовки HTTP, даже данные, источником которых является база данных, которые могут стать злоумышленниками, даже если они не могут повредить ваше приложение.</span><span class="sxs-lookup"><span data-stu-id="96c37-112">Untrusted data is any data that may be controlled by an attacker, HTML form inputs, query strings, HTTP headers, even data sourced from a database as an attacker may be able to breach your database even if they cannot breach your application.</span></span>

2. <span data-ttu-id="96c37-113">Перед размещением недоверенных данных внутри элемента HTML убедитесь, что они закодированы в формате HTML.</span><span class="sxs-lookup"><span data-stu-id="96c37-113">Before putting untrusted data inside an HTML element ensure it's HTML encoded.</span></span> <span data-ttu-id="96c37-114">Кодировка HTML принимает такие символы, как &lt;, и изменяет их в сейфовую форму, например &amp;lt;</span><span class="sxs-lookup"><span data-stu-id="96c37-114">HTML encoding takes characters such as &lt; and changes them into a safe form like &amp;lt;</span></span>

3. <span data-ttu-id="96c37-115">Перед помещением непроверенных данных в атрибут HTML убедитесь, что он закодирован в формате HTML.</span><span class="sxs-lookup"><span data-stu-id="96c37-115">Before putting untrusted data into an HTML attribute ensure it's HTML encoded.</span></span> <span data-ttu-id="96c37-116">Кодирование атрибута HTML — это надмножество кодировки HTML и кодирует дополнительные символы, например "и".</span><span class="sxs-lookup"><span data-stu-id="96c37-116">HTML attribute encoding is a superset of HTML encoding and encodes additional characters such as " and '.</span></span>

4. <span data-ttu-id="96c37-117">Перед размещением недоверенных данных в JavaScript Поместите данные в элемент HTML, содержимое которого извлекается во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="96c37-117">Before putting untrusted data into JavaScript place the data in an HTML element whose contents you retrieve at runtime.</span></span> <span data-ttu-id="96c37-118">Если это невозможно, убедитесь, что данные кодируются в JavaScript.</span><span class="sxs-lookup"><span data-stu-id="96c37-118">If this isn't possible, then ensure the data is JavaScript encoded.</span></span> <span data-ttu-id="96c37-119">Кодировка JavaScript принимает небезопасные символы для JavaScript и заменяет их шестнадцатеричными, например &lt; будет кодироваться как `\u003C`.</span><span class="sxs-lookup"><span data-stu-id="96c37-119">JavaScript encoding takes dangerous characters for JavaScript and replaces them with their hex, for example &lt; would be encoded as `\u003C`.</span></span>

5. <span data-ttu-id="96c37-120">Перед размещением недоверенных данных в строке запроса URL-адреса убедитесь, что он кодируется в формате URL.</span><span class="sxs-lookup"><span data-stu-id="96c37-120">Before putting untrusted data into a URL query string ensure it's URL encoded.</span></span>

## <a name="html-encoding-using-razor"></a><span data-ttu-id="96c37-121">Кодирование HTML с помощью Razor</span><span class="sxs-lookup"><span data-stu-id="96c37-121">HTML Encoding using Razor</span></span>

<span data-ttu-id="96c37-122">Ядро Razor, используемое в MVC, автоматически кодирует все выходные данные, источником которых являются переменные, если только вы не уверены в том, чтобы предотвратить это.</span><span class="sxs-lookup"><span data-stu-id="96c37-122">The Razor engine used in MVC automatically encodes all output sourced from variables, unless you work really hard to prevent it doing so.</span></span> <span data-ttu-id="96c37-123">При использовании директивы *@* используются правила кодирования HTML-атрибутов.</span><span class="sxs-lookup"><span data-stu-id="96c37-123">It uses HTML attribute encoding rules whenever you use the *@* directive.</span></span> <span data-ttu-id="96c37-124">Поскольку кодировка атрибута HTML является надмножеством кодировки HTML, это означает, что вам не нужно беспокоиться о том, следует ли использовать кодировку HTML или кодировку атрибута HTML.</span><span class="sxs-lookup"><span data-stu-id="96c37-124">As HTML attribute encoding is a superset of HTML encoding this means you don't have to concern yourself with whether you should use HTML encoding or HTML attribute encoding.</span></span> <span data-ttu-id="96c37-125">Необходимо убедиться, что используется только @ в контексте HTML, а не при попытке вставить недоверенные входные данные непосредственно в JavaScript.</span><span class="sxs-lookup"><span data-stu-id="96c37-125">You must ensure that you only use @ in an HTML context, not when attempting to insert untrusted input directly into JavaScript.</span></span> <span data-ttu-id="96c37-126">Вспомогательные функции тегов также будут кодировать входные данные, используемые в параметрах тегов.</span><span class="sxs-lookup"><span data-stu-id="96c37-126">Tag helpers will also encode input you use in tag parameters.</span></span>

<span data-ttu-id="96c37-127">Возьмем следующее представление Razor:</span><span class="sxs-lookup"><span data-stu-id="96c37-127">Take the following Razor view:</span></span>

```cshtml
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

<span data-ttu-id="96c37-128">В этом представлении выводится содержимое переменной *унтрустединпут* .</span><span class="sxs-lookup"><span data-stu-id="96c37-128">This view outputs the contents of the *untrustedInput* variable.</span></span> <span data-ttu-id="96c37-129">Эта переменная содержит некоторые символы, которые используются в атаках XSS, а именно &lt;и &gt;.</span><span class="sxs-lookup"><span data-stu-id="96c37-129">This variable includes some characters which are used in XSS attacks, namely &lt;, " and &gt;.</span></span> <span data-ttu-id="96c37-130">При проверке исходного кода отображаются выходные данные, закодированные как:</span><span class="sxs-lookup"><span data-stu-id="96c37-130">Examining the source shows the rendered output encoded as:</span></span>

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> <span data-ttu-id="96c37-131">ASP.NET Core MVC предоставляет `HtmlString` класс, который не кодируется автоматически при выводе.</span><span class="sxs-lookup"><span data-stu-id="96c37-131">ASP.NET Core MVC provides an `HtmlString` class which isn't automatically encoded upon output.</span></span> <span data-ttu-id="96c37-132">Этот параметр никогда не должен использоваться в сочетании с ненадежными входными данными, так как в этом случае будет предоставлена уязвимость XSS.</span><span class="sxs-lookup"><span data-stu-id="96c37-132">This should never be used in combination with untrusted input as this will expose an XSS vulnerability.</span></span>

## <a name="javascript-encoding-using-razor"></a><span data-ttu-id="96c37-133">Кодирование JavaScript с помощью Razor</span><span class="sxs-lookup"><span data-stu-id="96c37-133">JavaScript Encoding using Razor</span></span>

<span data-ttu-id="96c37-134">Иногда требуется вставить значение в код JavaScript для обработки в представлении.</span><span class="sxs-lookup"><span data-stu-id="96c37-134">There may be times you want to insert a value into JavaScript to process in your view.</span></span> <span data-ttu-id="96c37-135">Это можно сделать двумя способами.</span><span class="sxs-lookup"><span data-stu-id="96c37-135">There are two ways to do this.</span></span> <span data-ttu-id="96c37-136">Самым надежным способом вставки значений является размещение значения в атрибуте данных тега и его получение в JavaScript.</span><span class="sxs-lookup"><span data-stu-id="96c37-136">The safest way to insert values is to place the value in a data attribute of a tag and retrieve it in your JavaScript.</span></span> <span data-ttu-id="96c37-137">Пример:</span><span class="sxs-lookup"><span data-stu-id="96c37-137">For example:</span></span>

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

<span data-ttu-id="96c37-138">Это приведет к созданию следующего HTML-кода</span><span class="sxs-lookup"><span data-stu-id="96c37-138">This will produce the following HTML</span></span>

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

<span data-ttu-id="96c37-139">При запуске будет отображаться следующее:</span><span class="sxs-lookup"><span data-stu-id="96c37-139">Which, when it runs, will render the following:</span></span>

```
<"123">
   <"123">
```

<span data-ttu-id="96c37-140">Вы также можете вызвать кодировщик JavaScript напрямую:</span><span class="sxs-lookup"><span data-stu-id="96c37-140">You can also call the JavaScript encoder directly:</span></span>

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

<span data-ttu-id="96c37-141">Это будет отображаться в браузере следующим образом:</span><span class="sxs-lookup"><span data-stu-id="96c37-141">This will render in the browser as follows:</span></span>

```html
<script>
    document.write("\u003C\u0022123\u0022\u003E");
</script>
```

>[!WARNING]
> <span data-ttu-id="96c37-142">Не следует объединять ненадежные входные данные в JavaScript для создания элементов DOM.</span><span class="sxs-lookup"><span data-stu-id="96c37-142">Don't concatenate untrusted input in JavaScript to create DOM elements.</span></span> <span data-ttu-id="96c37-143">Следует использовать `createElement()` и назначать значения свойств соответствующим образом, например `node.TextContent=`, или использовать `element.SetAttribute()`/`element[attribute]=` в противном случае вы предоставляете себе для XSS на основе DOM.</span><span class="sxs-lookup"><span data-stu-id="96c37-143">You should use `createElement()` and assign property values appropriately such as `node.TextContent=`, or use `element.SetAttribute()`/`element[attribute]=` otherwise you expose yourself to DOM-based XSS.</span></span>

## <a name="accessing-encoders-in-code"></a><span data-ttu-id="96c37-144">Доступ к кодировщикам в коде</span><span class="sxs-lookup"><span data-stu-id="96c37-144">Accessing encoders in code</span></span>

<span data-ttu-id="96c37-145">Кодировщики HTML, JavaScript и URL-адреса доступны в коде двумя способами: их можно внедрять с помощью [внедрения зависимостей](xref:fundamentals/dependency-injection) или использовать кодировщики по умолчанию, содержащиеся в пространстве имен `System.Text.Encodings.Web`.</span><span class="sxs-lookup"><span data-stu-id="96c37-145">The HTML, JavaScript and URL encoders are available to your code in two ways, you can inject them via [dependency injection](xref:fundamentals/dependency-injection) or you can use the default encoders contained in the `System.Text.Encodings.Web` namespace.</span></span> <span data-ttu-id="96c37-146">Если вы используете кодировщики по умолчанию, любые значения, которые вы применили к диапазонам символов, не вступают в силу. кодировщики по умолчанию используют наиболее надежные правила кодировки.</span><span class="sxs-lookup"><span data-stu-id="96c37-146">If you use the default encoders then any  you applied to character ranges to be treated as safe won't take effect - the default encoders use the safest encoding rules possible.</span></span>

<span data-ttu-id="96c37-147">Для использования настраиваемых кодировщиков с помощью ondi конструкторы должны использовать параметр *HtmlEncoder*, *JavaScriptEncoder* и *UrlEncoder* , если это уместно.</span><span class="sxs-lookup"><span data-stu-id="96c37-147">To use the configurable encoders via DI your constructors should take an *HtmlEncoder*, *JavaScriptEncoder* and *UrlEncoder* parameter as appropriate.</span></span> <span data-ttu-id="96c37-148">Например:</span><span class="sxs-lookup"><span data-stu-id="96c37-148">For example;</span></span>

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

## <a name="encoding-url-parameters"></a><span data-ttu-id="96c37-149">Параметры URL-адреса кодировки</span><span class="sxs-lookup"><span data-stu-id="96c37-149">Encoding URL Parameters</span></span>

<span data-ttu-id="96c37-150">Если требуется создать строку запроса URL-адреса с ненадежным входом в качестве значения, используйте `UrlEncoder` для кодирования значения.</span><span class="sxs-lookup"><span data-stu-id="96c37-150">If you want to build a URL query string with untrusted input as a value use the `UrlEncoder` to encode the value.</span></span> <span data-ttu-id="96c37-151">Например,</span><span class="sxs-lookup"><span data-stu-id="96c37-151">For example,</span></span>

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

<span data-ttu-id="96c37-152">После кодирования переменная Енкодедвалуе будет содержать `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span><span class="sxs-lookup"><span data-stu-id="96c37-152">After encoding the encodedValue variable will contain `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span></span> <span data-ttu-id="96c37-153">Пробелы, кавычки, знаки препинания и другие незащищенные символы будут закодироваться в шестнадцатеричное значение, например, символ пробела станет %20.</span><span class="sxs-lookup"><span data-stu-id="96c37-153">Spaces, quotes, punctuation and other unsafe characters will be percent encoded to their hexadecimal value, for example a space character will become %20.</span></span>

>[!WARNING]
> <span data-ttu-id="96c37-154">Не используйте ненадежные входные данные в качестве части URL-пути.</span><span class="sxs-lookup"><span data-stu-id="96c37-154">Don't use untrusted input as part of a URL path.</span></span> <span data-ttu-id="96c37-155">Всегда передавать недоверенные входные данные в качестве значения строки запроса.</span><span class="sxs-lookup"><span data-stu-id="96c37-155">Always pass untrusted input as a query string value.</span></span>

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a><span data-ttu-id="96c37-156">Настройка кодировщиков</span><span class="sxs-lookup"><span data-stu-id="96c37-156">Customizing the Encoders</span></span>

<span data-ttu-id="96c37-157">По умолчанию кодировщики используют список безопасности, ограниченный базовым диапазоном символов латиницы в Юникоде, и закодировать все символы за пределами этого диапазона в качестве эквивалентов кода символа.</span><span class="sxs-lookup"><span data-stu-id="96c37-157">By default encoders use a safe list limited to the Basic Latin Unicode range and encode all characters outside of that range as their character code equivalents.</span></span> <span data-ttu-id="96c37-158">Это поведение также влияет на визуализацию Razor TagHelper и HtmlHelper, так как она будет использовать кодировщики для вывода строк.</span><span class="sxs-lookup"><span data-stu-id="96c37-158">This behavior also affects Razor TagHelper and HtmlHelper rendering as it will use the encoders to output your strings.</span></span>

<span data-ttu-id="96c37-159">Причина этого заключается в защите от неизвестных или будущих ошибок браузера (предыдущие ошибки браузера приступили к анализу на основе обработки символов, отличных от английского).</span><span class="sxs-lookup"><span data-stu-id="96c37-159">The reasoning behind this is to protect against unknown or future browser bugs (previous browser bugs have tripped up parsing based on the processing of non-English characters).</span></span> <span data-ttu-id="96c37-160">Если ваш веб-узел сильно использует символы, отличные от латинских, например китайский, кириллица или другие, это, вероятно, не является нужным поведением.</span><span class="sxs-lookup"><span data-stu-id="96c37-160">If your web site makes heavy use of non-Latin characters, such as Chinese, Cyrillic or others this is probably not the behavior you want.</span></span>

<span data-ttu-id="96c37-161">Списки надежных кодировщиков можно настроить для включения диапазонов Юникода, соответствующих приложению во время запуска, в `ConfigureServices()`.</span><span class="sxs-lookup"><span data-stu-id="96c37-161">You can customize the encoder safe lists to include Unicode ranges appropriate to your application during startup, in `ConfigureServices()`.</span></span>

<span data-ttu-id="96c37-162">Например, используя конфигурацию по умолчанию, можно использовать HtmlHelper Razor, например так:</span><span class="sxs-lookup"><span data-stu-id="96c37-162">For example, using the default configuration you might use a Razor HtmlHelper like so;</span></span>

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

<span data-ttu-id="96c37-163">При просмотре исходного кода веб-страницы он отображается следующим образом с кодировкой китайского текста.</span><span class="sxs-lookup"><span data-stu-id="96c37-163">When you view the source of the web page you will see it has been rendered as follows, with the Chinese text encoded;</span></span>

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

<span data-ttu-id="96c37-164">Чтобы расширить символы, обрабатываемые кодировщиком, вставьте следующую строку в метод `ConfigureServices()` в `startup.cs`;</span><span class="sxs-lookup"><span data-stu-id="96c37-164">To widen the characters treated as safe by the encoder you would insert the following line into the `ConfigureServices()` method in `startup.cs`;</span></span>

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

<span data-ttu-id="96c37-165">В этом примере список безопасного списка расширяется для включения диапазона Юникода Кжкунифиедидеографс.</span><span class="sxs-lookup"><span data-stu-id="96c37-165">This example widens the safe list to include the Unicode Range CjkUnifiedIdeographs.</span></span> <span data-ttu-id="96c37-166">Отображаемые выходные данные теперь станут</span><span class="sxs-lookup"><span data-stu-id="96c37-166">The rendered output would now become</span></span>

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

<span data-ttu-id="96c37-167">Диапазоны безсписок надежных списков указываются как диаграммы с кодом Юникода, а не язык.</span><span class="sxs-lookup"><span data-stu-id="96c37-167">Safe list ranges are specified as Unicode code charts, not languages.</span></span> <span data-ttu-id="96c37-168">[Стандарт Unicode](https://unicode.org/) содержит список [диаграмм кода](https://www.unicode.org/charts/index.html) , которые можно использовать для поиска диаграммы, содержащей символы.</span><span class="sxs-lookup"><span data-stu-id="96c37-168">The [Unicode standard](https://unicode.org/) has a list of [code charts](https://www.unicode.org/charts/index.html) you can use to find the chart containing your characters.</span></span> <span data-ttu-id="96c37-169">Каждый кодировщик, HTML, JavaScript и URL-адрес необходимо настроить отдельно.</span><span class="sxs-lookup"><span data-stu-id="96c37-169">Each encoder, Html, JavaScript and Url, must be configured separately.</span></span>

> [!NOTE]
> <span data-ttu-id="96c37-170">Настройка безопасного списка влияет только на кодировщики, источником которых является ondi.</span><span class="sxs-lookup"><span data-stu-id="96c37-170">Customization of the safe list only affects encoders sourced via DI.</span></span> <span data-ttu-id="96c37-171">Если вы напрямую обращаетесь к кодировщику через `System.Text.Encodings.Web.*Encoder.Default` то будет использоваться основной Латинский доступ только по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="96c37-171">If you directly access an encoder via `System.Text.Encodings.Web.*Encoder.Default` then the default, Basic Latin only safelist will be used.</span></span>

## <a name="where-should-encoding-take-place"></a><span data-ttu-id="96c37-172">Где следует выполнять кодирование?</span><span class="sxs-lookup"><span data-stu-id="96c37-172">Where should encoding take place?</span></span>

<span data-ttu-id="96c37-173">Общепринятой практикой является то, что кодирование происходит в момент выходных данных, а закодированные значения никогда не должны храниться в базе данных.</span><span class="sxs-lookup"><span data-stu-id="96c37-173">The general accepted practice is that encoding takes place at the point of output and encoded values should never be stored in a database.</span></span> <span data-ttu-id="96c37-174">Кодирование в точке вывода позволяет изменять использование данных, например, из HTML в значение строки запроса.</span><span class="sxs-lookup"><span data-stu-id="96c37-174">Encoding at the point of output allows you to change the use of data, for example, from HTML to a query string value.</span></span> <span data-ttu-id="96c37-175">Он также позволяет легко искать данные без необходимости кодирования значений перед поиском и позволяет использовать любые изменения или исправления ошибок, внесенные в кодировщики.</span><span class="sxs-lookup"><span data-stu-id="96c37-175">It also enables you to easily search your data without having to encode values before searching and allows you to take advantage of any changes or bug fixes made to encoders.</span></span>

## <a name="validation-as-an-xss-prevention-technique"></a><span data-ttu-id="96c37-176">Проверка как методика предотвращения XSS</span><span class="sxs-lookup"><span data-stu-id="96c37-176">Validation as an XSS prevention technique</span></span>

<span data-ttu-id="96c37-177">Проверка может быть полезным инструментом при ограничении атак XSS.</span><span class="sxs-lookup"><span data-stu-id="96c37-177">Validation can be a useful tool in limiting XSS attacks.</span></span> <span data-ttu-id="96c37-178">Например, числовая строка, содержащая только символы 0-9, не будет вызывать атаку XSS.</span><span class="sxs-lookup"><span data-stu-id="96c37-178">For example, a numeric string containing only the characters 0-9 won't trigger an XSS attack.</span></span> <span data-ttu-id="96c37-179">Проверка выполняется более сложно при приеме HTML в пользовательском вводе.</span><span class="sxs-lookup"><span data-stu-id="96c37-179">Validation becomes more complicated when accepting HTML in user input.</span></span> <span data-ttu-id="96c37-180">Анализ входных данных HTML является сложным, если это невозможно.</span><span class="sxs-lookup"><span data-stu-id="96c37-180">Parsing HTML input is difficult, if not impossible.</span></span> <span data-ttu-id="96c37-181">Markdown, в сочетании с средством синтаксического анализа, которое является лентой внедренного HTML, является более безопасным вариантом для приема расширенных входных данных.</span><span class="sxs-lookup"><span data-stu-id="96c37-181">Markdown, coupled with a parser that strips embedded HTML, is a safer option for accepting rich input.</span></span> <span data-ttu-id="96c37-182">Никогда не полагайтесь только на проверку.</span><span class="sxs-lookup"><span data-stu-id="96c37-182">Never rely on validation alone.</span></span> <span data-ttu-id="96c37-183">Всегда кодировать недоверенные входные данные перед выводом независимо от того, какая проверка или очистка выполнена.</span><span class="sxs-lookup"><span data-stu-id="96c37-183">Always encode untrusted input before output, no matter what validation or sanitization has been performed.</span></span>
