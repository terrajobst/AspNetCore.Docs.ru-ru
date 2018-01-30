---
title: "Предотвращение межсайтовых сценариев"
author: rick-anderson
description: "В этом документе представлены межсайтовых сценариев (XSS) и методы устранения этой уязвимости в приложении ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/cross-site-scripting
ms.openlocfilehash: 679d9689fbc2679d9ba20bf9c6dba5c95d76dbce
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="preventing-cross-site-scripting"></a><span data-ttu-id="9dc70-103">Предотвращение межсайтовых сценариев</span><span class="sxs-lookup"><span data-stu-id="9dc70-103">Preventing Cross-Site Scripting</span></span>

<span data-ttu-id="9dc70-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="9dc70-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9dc70-105">Межсайтовых сценариев (XSS) является уязвимость системы безопасности, позволяющая злоумышленнику разместить клиентские скрипты (обычно JavaScript) в веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="9dc70-105">Cross-Site Scripting (XSS) is a security vulnerability which enables an attacker to place client side scripts (usually JavaScript) into web pages.</span></span> <span data-ttu-id="9dc70-106">При других пользователей загрузить измененные страницы будут выполняться скрипты злоумышленники, что позволит ему похищать файлы cookie и токены сеанса изменение содержимого веб-страницы по обработке модели DOM или перенаправить браузер на другую страницу.</span><span class="sxs-lookup"><span data-stu-id="9dc70-106">When other users load affected pages the attackers scripts will run, enabling the attacker to steal cookies and session tokens, change the contents of the web page through DOM manipulation or redirect the browser to another page.</span></span> <span data-ttu-id="9dc70-107">Уязвимости XSS обычно возникают в том случае, когда приложение входные данные пользователя и выводит его на странице без проверки, кодирования или его преобразование.</span><span class="sxs-lookup"><span data-stu-id="9dc70-107">XSS vulnerabilities generally occur when an application takes user input and outputs it in a page without validating, encoding or escaping it.</span></span>

## <a name="protecting-your-application-against-xss"></a><span data-ttu-id="9dc70-108">Защита приложения от XSS</span><span class="sxs-lookup"><span data-stu-id="9dc70-108">Protecting your application against XSS</span></span>

<span data-ttu-id="9dc70-109">AT базовый уровень XSS работает путем tricking приложением для вставки `<script>` тег в готовом для просмотра страницы или путем вставки `On*` события в элемент.</span><span class="sxs-lookup"><span data-stu-id="9dc70-109">At a basic level XSS works by tricking your application into inserting a `<script>` tag into your rendered page, or by inserting an `On*` event into an element.</span></span> <span data-ttu-id="9dc70-110">Разработчикам следует использовать следующие шаги для предотвращения во избежание внесения XSS в свои приложения.</span><span class="sxs-lookup"><span data-stu-id="9dc70-110">Developers should use the following prevention steps to avoid introducing XSS into their application.</span></span>

1. <span data-ttu-id="9dc70-111">Никогда не попадают непроверенных данных входные данные HTML, пока не будут выполнены остальные шаги.</span><span class="sxs-lookup"><span data-stu-id="9dc70-111">Never put untrusted data into your HTML input, unless you follow the rest of the steps below.</span></span> <span data-ttu-id="9dc70-112">Непроверенных данных является любые данные, которые управляются злоумышленник, входных HTML, строк запросов, заголовки HTTP, даже данные из базы данных источника, как злоумышленник может иметь возможность нарушить базы данных, даже если они не может нарушить приложения.</span><span class="sxs-lookup"><span data-stu-id="9dc70-112">Untrusted data is any data that may be controlled by an attacker, HTML form inputs, query strings, HTTP headers, even data sourced from a database as an attacker may be able to breach your database even if they cannot breach your application.</span></span>

2. <span data-ttu-id="9dc70-113">Перед добавлением непроверенных данных внутри элемента HTML убедитесь, что он имеет кодировку HTML.</span><span class="sxs-lookup"><span data-stu-id="9dc70-113">Before putting untrusted data inside an HTML element ensure it's HTML encoded.</span></span> <span data-ttu-id="9dc70-114">HTML-кодирования принимает символы, такие как &lt; и изменяет их в безопасном форму как &amp;lt;</span><span class="sxs-lookup"><span data-stu-id="9dc70-114">HTML encoding takes characters such as &lt; and changes them into a safe form like &amp;lt;</span></span>

3. <span data-ttu-id="9dc70-115">Перед переводом в HTML-атрибут непроверенных данных убедитесь, что он является атрибутом HTML в кодировке.</span><span class="sxs-lookup"><span data-stu-id="9dc70-115">Before putting untrusted data into an HTML attribute ensure it's HTML attribute encoded.</span></span> <span data-ttu-id="9dc70-116">Кодировка атрибута HTML является надмножеством HTML-кодирования и кодирует дополнительные символы, такие как "и".</span><span class="sxs-lookup"><span data-stu-id="9dc70-116">HTML attribute encoding is a superset of HTML encoding and encodes additional characters such as " and '.</span></span>

4. <span data-ttu-id="9dc70-117">Перед добавлением в код JavaScript непроверенных данных помещает данные в HTML-элемент, содержимое которых получить во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="9dc70-117">Before putting untrusted data into JavaScript place the data in an HTML element whose contents you retrieve at runtime.</span></span> <span data-ttu-id="9dc70-118">Если это невозможно, то убедитесь, что данные кодируются JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9dc70-118">If this isn't possible then ensure the data is JavaScript encoded.</span></span> <span data-ttu-id="9dc70-119">Кодирование JavaScript принимает небезопасные символы для JavaScript и заменяет их символом их hex, например &lt; должен кодироваться как `\u003C`.</span><span class="sxs-lookup"><span data-stu-id="9dc70-119">JavaScript encoding takes dangerous characters for JavaScript and replaces them with their hex, for example &lt; would be encoded as `\u003C`.</span></span>

5. <span data-ttu-id="9dc70-120">Перед переводом непроверенных данных в строку запроса URL-адреса убедитесь, что он является URL-кодированием.</span><span class="sxs-lookup"><span data-stu-id="9dc70-120">Before putting untrusted data into a URL query string ensure it's URL encoded.</span></span>

## <a name="html-encoding-using-razor"></a><span data-ttu-id="9dc70-121">Кодировка HTML с помощью Razor</span><span class="sxs-lookup"><span data-stu-id="9dc70-121">HTML Encoding using Razor</span></span>

<span data-ttu-id="9dc70-122">Подсистема Razor, автоматически используется в MVC кодирует все выходные данные источником переменные, если только вы много работать, чтобы он таким образом.</span><span class="sxs-lookup"><span data-stu-id="9dc70-122">The Razor engine used in MVC automatically encodes all output sourced from variables, unless you work really hard to prevent it doing so.</span></span> <span data-ttu-id="9dc70-123">Он использует правила кодирования при использовании атрибута HTML * @ * директивы.</span><span class="sxs-lookup"><span data-stu-id="9dc70-123">It uses HTML Attribute encoding rules whenever you use the *@* directive.</span></span> <span data-ttu-id="9dc70-124">Как HTML кодировка атрибута является надмножеством HTML-кодирования, это значит, что не нужно заниматься, следует ли использовать HTML-кодирования или кодировка атрибута HTML.</span><span class="sxs-lookup"><span data-stu-id="9dc70-124">As HTML attribute encoding is a superset of HTML encoding this means you don't have to concern yourself with whether you should use HTML encoding or HTML attribute encoding.</span></span> <span data-ttu-id="9dc70-125">Необходимо убедиться, что используются только в контексте HTML не при попытке вставить недоверенные входные данные непосредственно в JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9dc70-125">You must ensure that you only use @ in an HTML context, not when attempting to insert untrusted input directly into JavaScript.</span></span> <span data-ttu-id="9dc70-126">Вспомогательных функций тегов также будут закодировать входные данные, используемые в параметрах тегов.</span><span class="sxs-lookup"><span data-stu-id="9dc70-126">Tag helpers will also encode input you use in tag parameters.</span></span>

<span data-ttu-id="9dc70-127">Использовать следующее представление Razor;</span><span class="sxs-lookup"><span data-stu-id="9dc70-127">Take the following Razor view;</span></span>

```none
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

<span data-ttu-id="9dc70-128">Это представление выводит содержимое *untrustedInput* переменной.</span><span class="sxs-lookup"><span data-stu-id="9dc70-128">This view outputs the contents of the *untrustedInput* variable.</span></span> <span data-ttu-id="9dc70-129">Эта переменная содержит некоторые символы, которые используются в атак с XSS, а именно &lt;,» и &gt;.</span><span class="sxs-lookup"><span data-stu-id="9dc70-129">This variable includes some characters which are used in XSS attacks, namely &lt;, " and &gt;.</span></span> <span data-ttu-id="9dc70-130">Проверка источника показаны выводимые данные кодируются как:</span><span class="sxs-lookup"><span data-stu-id="9dc70-130">Examining the source shows the rendered output encoded as:</span></span>

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> <span data-ttu-id="9dc70-131">Основные ASP.NET MVC предоставляет `HtmlString` класс, которой не автоматически после выходных данных.</span><span class="sxs-lookup"><span data-stu-id="9dc70-131">ASP.NET Core MVC provides an `HtmlString` class which isn't automatically encoded upon output.</span></span> <span data-ttu-id="9dc70-132">Это никогда не можно использовать в сочетании с ненадежных входных данных, при этом будут представлены уязвимость XSS.</span><span class="sxs-lookup"><span data-stu-id="9dc70-132">This should never be used in combination with untrusted input as this will expose an XSS vulnerability.</span></span>

## <a name="javascript-encoding-using-razor"></a><span data-ttu-id="9dc70-133">Кодирование JavaScript с помощью Razor</span><span class="sxs-lookup"><span data-stu-id="9dc70-133">Javascript Encoding using Razor</span></span>

<span data-ttu-id="9dc70-134">Могут возникнуть ситуации, нужно вставить значение в JavaScript для обработки в представлении.</span><span class="sxs-lookup"><span data-stu-id="9dc70-134">There may be times you want to insert a value into JavaScript to process in your view.</span></span> <span data-ttu-id="9dc70-135">Это можно сделать двумя способами.</span><span class="sxs-lookup"><span data-stu-id="9dc70-135">There are two ways to do this.</span></span> <span data-ttu-id="9dc70-136">Это самый безопасный способ вставки простых значений — поместить значение в атрибуте данных тега и поиск его в JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9dc70-136">The safest way to insert simple values is to place the value in a data attribute of a tag and retrieve it in your JavaScript.</span></span> <span data-ttu-id="9dc70-137">Пример:</span><span class="sxs-lookup"><span data-stu-id="9dc70-137">For example:</span></span>

```none
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

<span data-ttu-id="9dc70-138">В результате получится следующий код HTML</span><span class="sxs-lookup"><span data-stu-id="9dc70-138">This will produce the following HTML</span></span>

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

<span data-ttu-id="9dc70-139">Если он выполняется, отображающего следующую процедуру.</span><span class="sxs-lookup"><span data-stu-id="9dc70-139">Which, when it runs, will render the following;</span></span>

```none
<"123">
   <"123">
   ```

<span data-ttu-id="9dc70-140">Можно также вызвать кодировщик JavaScript напрямую,</span><span class="sxs-lookup"><span data-stu-id="9dc70-140">You can also call the JavaScript encoder directly,</span></span>

```none
@using System.Text.Encodings.Web;
   @inject JavaScriptEncoder encoder;

   @{
       var untrustedInput = "<\"123\">";
   }

   <script>
       document.write("@encoder.Encode(untrustedInput)");
   </script>
   ```

<span data-ttu-id="9dc70-141">Это сделает в браузере, следующим образом:</span><span class="sxs-lookup"><span data-stu-id="9dc70-141">This will render in the browser as follows;</span></span>

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> <span data-ttu-id="9dc70-142">Не объединять недоверенные входные данные на языке JavaScript для создания элементов DOM.</span><span class="sxs-lookup"><span data-stu-id="9dc70-142">Don't concatenate untrusted input in JavaScript to create DOM elements.</span></span> <span data-ttu-id="9dc70-143">Следует использовать `createElement()` и назначить значения свойств, например соответствующим образом `node.TextContent=`, или используйте `element.SetAttribute()` / `element[attribute]=` в противном случае самостоятельно предоставлять на основе DOM XSS.</span><span class="sxs-lookup"><span data-stu-id="9dc70-143">You should use `createElement()` and assign property values appropriately such as `node.TextContent=`, or use `element.SetAttribute()`/`element[attribute]=` otherwise you expose yourself to DOM-based XSS.</span></span>

## <a name="accessing-encoders-in-code"></a><span data-ttu-id="9dc70-144">Доступ к кодировщики в коде</span><span class="sxs-lookup"><span data-stu-id="9dc70-144">Accessing encoders in code</span></span>

<span data-ttu-id="9dc70-145">Кодировщики HTML, JavaScript и URL-адреса доступны для кода двумя способами, можно ввести их через [внедрения зависимостей](../fundamentals/dependency-injection.md#fundamentals-dependency-injection) или использовании кодировщики по умолчанию, содержащиеся в `System.Text.Encodings.Web` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="9dc70-145">The HTML, JavaScript and URL encoders are available to your code in two ways, you can inject them via [dependency injection](../fundamentals/dependency-injection.md#fundamentals-dependency-injection) or you can use the default encoders contained in the `System.Text.Encodings.Web` namespace.</span></span> <span data-ttu-id="9dc70-146">При использовании кодировщики по умолчанию любой применяются к диапазоны знаков следует рассматривать как безопасные вступят в силу — кодировщики по умолчанию используйте безопасный из возможных правила кодирования.</span><span class="sxs-lookup"><span data-stu-id="9dc70-146">If you use the default encoders then any  you applied to character ranges to be treated as safe won't take effect - the default encoders use the safest encoding rules possible.</span></span>

<span data-ttu-id="9dc70-147">Для использования настраиваемых кодировщики через DI конструкторах займет *HtmlEncoder*, *JavaScriptEncoder* и *UrlEncoder* параметр соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="9dc70-147">To use the configurable encoders via DI your constructors should take an *HtmlEncoder*, *JavaScriptEncoder* and *UrlEncoder* parameter as appropriate.</span></span> <span data-ttu-id="9dc70-148">Например,</span><span class="sxs-lookup"><span data-stu-id="9dc70-148">For example;</span></span>

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

## <a name="encoding-url-parameters"></a><span data-ttu-id="9dc70-149">Параметры кодировки URL-адреса</span><span class="sxs-lookup"><span data-stu-id="9dc70-149">Encoding URL Parameters</span></span>

<span data-ttu-id="9dc70-150">Если вы хотите создать строку запроса URL-адрес с ненадежных входных данных, как использование значение `UrlEncoder` для кодирования значения.</span><span class="sxs-lookup"><span data-stu-id="9dc70-150">If you want to build a URL query string with untrusted input as a value use the `UrlEncoder` to encode the value.</span></span> <span data-ttu-id="9dc70-151">Например, примененная к объекту директива</span><span class="sxs-lookup"><span data-stu-id="9dc70-151">For example,</span></span>

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

<span data-ttu-id="9dc70-152">После кодирования encodedValue переменная будет содержать `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span><span class="sxs-lookup"><span data-stu-id="9dc70-152">After encoding the encodedValue variable will contain `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span></span> <span data-ttu-id="9dc70-153">Пробелы, кавычки, знаки пунктуации и другие небезопасных символов кодироваться процента шестнадцатеричные значения, например символ пробела становится % 20.</span><span class="sxs-lookup"><span data-stu-id="9dc70-153">Spaces, quotes, punctuation and other unsafe characters will be percent encoded to their hexadecimal value, for example a space character will become %20.</span></span>

>[!WARNING]
> <span data-ttu-id="9dc70-154">Не используйте недоверенные входные данные как часть URL-пути.</span><span class="sxs-lookup"><span data-stu-id="9dc70-154">Don't use untrusted input as part of a URL path.</span></span> <span data-ttu-id="9dc70-155">Всегда передает недоверенные входные данные как значение строки запроса.</span><span class="sxs-lookup"><span data-stu-id="9dc70-155">Always pass untrusted input as a query string value.</span></span>

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a><span data-ttu-id="9dc70-156">Настройка кодировщики</span><span class="sxs-lookup"><span data-stu-id="9dc70-156">Customizing the Encoders</span></span>

<span data-ttu-id="9dc70-157">По умолчанию кодировщики используйте безопасный список, ограниченный диапазон Юникода Basic Latin и кодирования всех символов вне этого диапазона как эквиваленты код символа.</span><span class="sxs-lookup"><span data-stu-id="9dc70-157">By default encoders use a safe list limited to the Basic Latin Unicode range and encode all characters outside of that range as their character code equivalents.</span></span> <span data-ttu-id="9dc70-158">Это поведение также влияет на отрисовку вспомогательной функции тегов Razor и HtmlHelper как кодировщики будет использовать для сравнения строк в выходных данных.</span><span class="sxs-lookup"><span data-stu-id="9dc70-158">This behavior also affects Razor TagHelper and HtmlHelper rendering as it will use the encoders to output your strings.</span></span>

<span data-ttu-id="9dc70-159">Причины, обуславливающие это сделано для защиты от неизвестных или будущих браузера ошибок (предыдущих ошибок браузера задействуемый копирование основанные на обработку символов не на английском языке).</span><span class="sxs-lookup"><span data-stu-id="9dc70-159">The reasoning behind this is to protect against unknown or future browser bugs (previous browser bugs have tripped up parsing based on the processing of non-English characters).</span></span> <span data-ttu-id="9dc70-160">Если веб-сайте усложняет использование Нелатинские символы, такие как китайский, кириллица или другим это, скорее всего, не желаемое поведение.</span><span class="sxs-lookup"><span data-stu-id="9dc70-160">If your web site makes heavy use of non-Latin characters, such as Chinese, Cyrillic or others this is probably not the behavior you want.</span></span>

<span data-ttu-id="9dc70-161">Вы можете настроить списки безопасном кодировщика для включения Юникода, допустимые диапазоны для приложения во время запуска, в `ConfigureServices()`.</span><span class="sxs-lookup"><span data-stu-id="9dc70-161">You can customize the encoder safe lists to include Unicode ranges appropriate to your application during startup, in `ConfigureServices()`.</span></span>

<span data-ttu-id="9dc70-162">Например, с помощью конфигурации по умолчанию, можно использовать Razor HtmlHelper следующим образом;</span><span class="sxs-lookup"><span data-stu-id="9dc70-162">For example, using the default configuration you might use a Razor HtmlHelper like so;</span></span>

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

<span data-ttu-id="9dc70-163">При просмотре исходного кода веб-страницы, то будет выведено его обработки следующим образом с кодированием; текст на китайском языке</span><span class="sxs-lookup"><span data-stu-id="9dc70-163">When you view the source of the web page you will see it has been rendered as follows, with the Chinese text encoded;</span></span>

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

<span data-ttu-id="9dc70-164">Чтобы увеличить ширину символов, считаются безопасном кодировщиком необходимо вставить следующую строку в `ConfigureServices()` метод в `startup.cs`;</span><span class="sxs-lookup"><span data-stu-id="9dc70-164">To widen the characters treated as safe by the encoder you would insert the following line into the `ConfigureServices()` method in `startup.cs`;</span></span>

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

<span data-ttu-id="9dc70-165">В этом примере расширяется список безопасных для включения CjkUnifiedIdeographs диапазон Юникода.</span><span class="sxs-lookup"><span data-stu-id="9dc70-165">This example widens the safe list to include the Unicode Range CjkUnifiedIdeographs.</span></span> <span data-ttu-id="9dc70-166">Теперь стал бы выводимые данные</span><span class="sxs-lookup"><span data-stu-id="9dc70-166">The rendered output would now become</span></span>

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

<span data-ttu-id="9dc70-167">Список надежных диапазоны задаются в виде кодировок Юникода, не языков.</span><span class="sxs-lookup"><span data-stu-id="9dc70-167">Safe list ranges are specified as Unicode code charts, not languages.</span></span> <span data-ttu-id="9dc70-168">[Стандарт Юникод](http://unicode.org/) имеет список [кода диаграммы](http://www.unicode.org/charts/index.html) можно использовать для поиска диаграммы, содержащей вашей символов.</span><span class="sxs-lookup"><span data-stu-id="9dc70-168">The [Unicode standard](http://unicode.org/) has a list of [code charts](http://www.unicode.org/charts/index.html) you can use to find the chart containing your characters.</span></span> <span data-ttu-id="9dc70-169">Каждого кодировщика, Html, JavaScript и URL-адрес должен быть настроен отдельно.</span><span class="sxs-lookup"><span data-stu-id="9dc70-169">Each encoder, Html, JavaScript and Url, must be configured separately.</span></span>

> [!NOTE]
> <span data-ttu-id="9dc70-170">Настройка надежных влияет только на источником через DI кодировщиков.</span><span class="sxs-lookup"><span data-stu-id="9dc70-170">Customization of the safe list only affects encoders sourced via DI.</span></span> <span data-ttu-id="9dc70-171">Если прямой доступ к кодировщик через `System.Text.Encodings.Web.*Encoder.Default` затем по умолчанию, Basic Latin будет использоваться только списка надежных отправителей.</span><span class="sxs-lookup"><span data-stu-id="9dc70-171">If you directly access an encoder via `System.Text.Encodings.Web.*Encoder.Default` then the default, Basic Latin only safelist will be used.</span></span>

## <a name="where-should-encoding-take-place"></a><span data-ttu-id="9dc70-172">Где следует поместить кодирования take</span><span class="sxs-lookup"><span data-stu-id="9dc70-172">Where should encoding take place?</span></span>

<span data-ttu-id="9dc70-173">Применяемый общий подход заключается в том кодирование выполняется во время вывода закодированные значения никогда не должны храниться в базе данных.</span><span class="sxs-lookup"><span data-stu-id="9dc70-173">The general accepted practice is that encoding takes place at the point of output and encoded values should never be stored in a database.</span></span> <span data-ttu-id="9dc70-174">Кодирование точке выходные данные можно изменить использование данных, например, из HTML-код, значение строки запроса.</span><span class="sxs-lookup"><span data-stu-id="9dc70-174">Encoding at the point of output allows you to change the use of data, for example, from HTML to a query string value.</span></span> <span data-ttu-id="9dc70-175">Также позволяет легко найти данных без необходимости кодирование значений перед поиском и позволяет воспользоваться преимуществами любые изменения или исправления, внесенные в кодировщиков.</span><span class="sxs-lookup"><span data-stu-id="9dc70-175">It also enables you to easily search your data without having to encode values before searching and allows you to take advantage of any changes or bug fixes made to encoders.</span></span>

## <a name="validation-as-an-xss-prevention-technique"></a><span data-ttu-id="9dc70-176">Проверка как способ защиты от XSS</span><span class="sxs-lookup"><span data-stu-id="9dc70-176">Validation as an XSS prevention technique</span></span>

<span data-ttu-id="9dc70-177">Проверка может быть полезным инструментом ограничение атак с XSS.</span><span class="sxs-lookup"><span data-stu-id="9dc70-177">Validation can be a useful tool in limiting XSS attacks.</span></span> <span data-ttu-id="9dc70-178">Например простой числовой строка, содержащая только символы 0 – 9 не активирует XSS атаки.</span><span class="sxs-lookup"><span data-stu-id="9dc70-178">For example, a simple numeric string containing only the characters 0-9 won't trigger an XSS attack.</span></span> <span data-ttu-id="9dc70-179">Проверка становится более сложным, следует нужно принимать HTML во вводимых - синтаксического анализа HTML-ввода является сложным, невозможно.</span><span class="sxs-lookup"><span data-stu-id="9dc70-179">Validation becomes more complicated should you wish to accept HTML in user input - parsing HTML input is difficult, if not impossible.</span></span> <span data-ttu-id="9dc70-180">Другие форматы текста и разметки будет более безопасный параметр для форматированного ввода.</span><span class="sxs-lookup"><span data-stu-id="9dc70-180">MarkDown and other text formats would be a safer option for rich input.</span></span> <span data-ttu-id="9dc70-181">Никогда не следует полагаться на проверку сама по себе.</span><span class="sxs-lookup"><span data-stu-id="9dc70-181">You should never rely on validation alone.</span></span> <span data-ttu-id="9dc70-182">Всегда кодировать недоверенные входные данные перед выводом, независимо от того, какая проверка выполнена.</span><span class="sxs-lookup"><span data-stu-id="9dc70-182">Always encode untrusted input before output, no matter what validation you have performed.</span></span>
