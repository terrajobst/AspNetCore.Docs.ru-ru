---
title: "Предотвращение межсайтовых сценариев"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 95790927-2bfe-445e-b1fd-429c2c7030ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/cross-site-scripting
ms.openlocfilehash: 1816977837efd82f374a03d9f776db21358e2850
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="preventing-cross-site-scripting"></a><span data-ttu-id="f8290-103">Предотвращение межсайтовых сценариев</span><span class="sxs-lookup"><span data-stu-id="f8290-103">Preventing Cross-Site Scripting</span></span>

<a name=security-cross-site-scripting></a>

<span data-ttu-id="f8290-104">Межсайтовых сценариев (XSS) является уязвимость системы безопасности, позволяющая злоумышленнику разместить клиентские скрипты (обычно JavaScript) в веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="f8290-104">Cross-Site Scripting (XSS) is a security vulnerability which enables an attacker to place client side scripts (usually JavaScript) into web pages.</span></span> <span data-ttu-id="f8290-105">При других пользователей загрузить измененные страницы будут выполняться скрипты злоумышленники, что позволит ему похищать файлы cookie и токены сеанса изменение содержимого веб-страницы по обработке модели DOM или перенаправить браузер на другую страницу.</span><span class="sxs-lookup"><span data-stu-id="f8290-105">When other users load affected pages the attackers scripts will run, enabling the attacker to steal cookies and session tokens, change the contents of the web page through DOM manipulation or redirect the browser to another page.</span></span> <span data-ttu-id="f8290-106">Уязвимости XSS обычно возникают в том случае, когда приложение входные данные пользователя и выводит его на странице без проверки, кодирования или его преобразование.</span><span class="sxs-lookup"><span data-stu-id="f8290-106">XSS vulnerabilities generally occur when an application takes user input and outputs it in a page without validating, encoding or escaping it.</span></span>

## <a name="protecting-your-application-against-xss"></a><span data-ttu-id="f8290-107">Защита приложения от XSS</span><span class="sxs-lookup"><span data-stu-id="f8290-107">Protecting your application against XSS</span></span>

<span data-ttu-id="f8290-108">AT базовый уровень XSS работает путем tricking приложением для вставки `<script>` тег в готовом для просмотра страницы или путем вставки `On*` события в элемент.</span><span class="sxs-lookup"><span data-stu-id="f8290-108">At a basic level XSS works by tricking your application into inserting a `<script>` tag into your rendered page, or by inserting an `On*` event into an element.</span></span> <span data-ttu-id="f8290-109">Разработчикам следует использовать следующие шаги для предотвращения во избежание внесения XSS в свои приложения.</span><span class="sxs-lookup"><span data-stu-id="f8290-109">Developers should use the following prevention steps to avoid introducing XSS into their application.</span></span>

1. <span data-ttu-id="f8290-110">Никогда не попадают непроверенных данных входные данные HTML, пока не будут выполнены остальные шаги.</span><span class="sxs-lookup"><span data-stu-id="f8290-110">Never put untrusted data into your HTML input, unless you follow the rest of the steps below.</span></span> <span data-ttu-id="f8290-111">Непроверенных данных является любые данные, которые управляются злоумышленник, входных HTML, строк запросов, заголовки HTTP, даже данные из базы данных источника, как злоумышленник может иметь возможность нарушить базы данных, даже если они не может нарушить приложения.</span><span class="sxs-lookup"><span data-stu-id="f8290-111">Untrusted data is any data that may be controlled by an attacker, HTML form inputs, query strings, HTTP headers, even data sourced from a database as an attacker may be able to breach your database even if they cannot breach your application.</span></span>

2. <span data-ttu-id="f8290-112">Перед добавлением непроверенных данных внутри элемента HTML убедитесь, что он имеет кодировку HTML.</span><span class="sxs-lookup"><span data-stu-id="f8290-112">Before putting untrusted data inside an HTML element ensure it is HTML encoded.</span></span> <span data-ttu-id="f8290-113">HTML-кодирования принимает символы, такие как &lt; и изменяет их в безопасном форму как &amp;lt;</span><span class="sxs-lookup"><span data-stu-id="f8290-113">HTML encoding takes characters such as &lt; and changes them into a safe form like &amp;lt;</span></span>

3. <span data-ttu-id="f8290-114">Перед переводом в HTML-атрибут непроверенных данных убедитесь, что он является атрибутом HTML в кодировке.</span><span class="sxs-lookup"><span data-stu-id="f8290-114">Before putting untrusted data into an HTML attribute ensure it is HTML attribute encoded.</span></span> <span data-ttu-id="f8290-115">Кодировка атрибута HTML является надмножеством HTML-кодирования и кодирует дополнительные символы, такие как "и".</span><span class="sxs-lookup"><span data-stu-id="f8290-115">HTML attribute encoding is a superset of HTML encoding and encodes additional characters such as " and '.</span></span>

4. <span data-ttu-id="f8290-116">Перед добавлением в код JavaScript непроверенных данных помещает данные в HTML-элемент, содержимое которых получить во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="f8290-116">Before putting untrusted data into JavaScript place the data in an HTML element whose contents you retrieve at runtime.</span></span> <span data-ttu-id="f8290-117">Если это невозможно, то убедитесь, что данные кодируются JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f8290-117">If this is not possible then ensure the data is JavaScript encoded.</span></span> <span data-ttu-id="f8290-118">Кодирование JavaScript принимает небезопасные символы для JavaScript и заменяет их символом их hex, например &lt; должен кодироваться как `\u003C`.</span><span class="sxs-lookup"><span data-stu-id="f8290-118">JavaScript encoding takes dangerous characters for JavaScript and replaces them with their hex, for example &lt; would be encoded as `\u003C`.</span></span>

5. <span data-ttu-id="f8290-119">Перед переводом непроверенных данных в строку запроса URL-адреса убедитесь, что он является URL-кодированием.</span><span class="sxs-lookup"><span data-stu-id="f8290-119">Before putting untrusted data into a URL query string ensure it is URL encoded.</span></span>

## <a name="html-encoding-using-razor"></a><span data-ttu-id="f8290-120">Кодировка HTML с помощью Razor</span><span class="sxs-lookup"><span data-stu-id="f8290-120">HTML Encoding using Razor</span></span>

<span data-ttu-id="f8290-121">Подсистема Razor, автоматически используется в MVC кодирует все выходные данные источником переменные, если только вы много работать, чтобы он таким образом.</span><span class="sxs-lookup"><span data-stu-id="f8290-121">The Razor engine used in MVC automatically encodes all output sourced from variables, unless you work really hard to prevent it doing so.</span></span> <span data-ttu-id="f8290-122">Он использует правила кодирования при использовании атрибута HTML  *@*  директивы.</span><span class="sxs-lookup"><span data-stu-id="f8290-122">It uses HTML Attribute encoding rules whenever you use the *@* directive.</span></span> <span data-ttu-id="f8290-123">Как HTML кодировка атрибута является надмножеством HTML-кодирования, это значит, что не нужно заниматься, следует ли использовать HTML-кодирования или кодировка атрибута HTML.</span><span class="sxs-lookup"><span data-stu-id="f8290-123">As HTML attribute encoding is a superset of HTML encoding this means you don't have to concern yourself with whether you should use HTML encoding or HTML attribute encoding.</span></span> <span data-ttu-id="f8290-124">Необходимо убедиться, что используются только в контексте HTML не при попытке вставить недоверенные входные данные непосредственно в JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f8290-124">You must ensure that you only use @ in an HTML context, not when attempting to insert untrusted input directly into JavaScript.</span></span> <span data-ttu-id="f8290-125">Вспомогательных функций тегов также будут закодировать входные данные, используемые в параметрах тегов.</span><span class="sxs-lookup"><span data-stu-id="f8290-125">Tag helpers will also encode input you use in tag parameters.</span></span>

<span data-ttu-id="f8290-126">Использовать следующее представление Razor;</span><span class="sxs-lookup"><span data-stu-id="f8290-126">Take the following Razor view;</span></span>

```none
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

<span data-ttu-id="f8290-127">Это представление выводит содержимое *untrustedInput* переменной.</span><span class="sxs-lookup"><span data-stu-id="f8290-127">This view outputs the contents of the *untrustedInput* variable.</span></span> <span data-ttu-id="f8290-128">Эта переменная содержит некоторые символы, которые используются в атак с XSS, а именно &lt;,» и &gt;.</span><span class="sxs-lookup"><span data-stu-id="f8290-128">This variable includes some characters which are used in XSS attacks, namely &lt;, " and &gt;.</span></span> <span data-ttu-id="f8290-129">Проверка источника показаны выводимые данные кодируются как:</span><span class="sxs-lookup"><span data-stu-id="f8290-129">Examining the source shows the rendered output encoded as:</span></span>

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> <span data-ttu-id="f8290-130">Основные ASP.NET MVC предоставляет `HtmlString` класс, который не будет закодировано автоматически после выходных данных.</span><span class="sxs-lookup"><span data-stu-id="f8290-130">ASP.NET Core MVC provides an `HtmlString` class which is not automatically encoded upon output.</span></span> <span data-ttu-id="f8290-131">Это никогда не можно использовать в сочетании с ненадежных входных данных, при этом будут представлены уязвимость XSS.</span><span class="sxs-lookup"><span data-stu-id="f8290-131">This should never be used in combination with untrusted input as this will expose an XSS vulnerability.</span></span>

## <a name="javascript-encoding-using-razor"></a><span data-ttu-id="f8290-132">Кодирование JavaScript с помощью Razor</span><span class="sxs-lookup"><span data-stu-id="f8290-132">Javascript Encoding using Razor</span></span>

<span data-ttu-id="f8290-133">Могут возникнуть ситуации, нужно вставить значение в JavaScript для обработки в представлении.</span><span class="sxs-lookup"><span data-stu-id="f8290-133">There may be times you want to insert a value into JavaScript to process in your view.</span></span> <span data-ttu-id="f8290-134">Это можно сделать двумя способами.</span><span class="sxs-lookup"><span data-stu-id="f8290-134">There are two ways to do this.</span></span> <span data-ttu-id="f8290-135">Это самый безопасный способ вставки простых значений — поместить значение в атрибуте данных тега и поиск его в JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f8290-135">The safest way to insert simple values is to place the value in a data attribute of a tag and retrieve it in your JavaScript.</span></span> <span data-ttu-id="f8290-136">Пример:</span><span class="sxs-lookup"><span data-stu-id="f8290-136">For example:</span></span>

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

<span data-ttu-id="f8290-137">В результате получится следующий код HTML</span><span class="sxs-lookup"><span data-stu-id="f8290-137">This will produce the following HTML</span></span>

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

<span data-ttu-id="f8290-138">Если он выполняется, отображающего следующую процедуру.</span><span class="sxs-lookup"><span data-stu-id="f8290-138">Which, when it runs, will render the following;</span></span>

```none
<"123">
   <"123">
   ```

<span data-ttu-id="f8290-139">Можно также вызвать кодировщик JavaScript напрямую,</span><span class="sxs-lookup"><span data-stu-id="f8290-139">You can also call the JavaScript encoder directly,</span></span>

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

<span data-ttu-id="f8290-140">Это сделает в браузере, следующим образом:</span><span class="sxs-lookup"><span data-stu-id="f8290-140">This will render in the browser as follows;</span></span>

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> <span data-ttu-id="f8290-141">Не следует объединять недоверенные входные данные на языке JavaScript для создания элементов DOM.</span><span class="sxs-lookup"><span data-stu-id="f8290-141">Do not concatenate untrusted input in JavaScript to create DOM elements.</span></span> <span data-ttu-id="f8290-142">Следует использовать `createElement()` и назначить значения свойств, например соответствующим образом `node.TextContent=`, или используйте `element.SetAttribute()` / `element[attribute]=` в противном случае самостоятельно предоставлять на основе DOM XSS.</span><span class="sxs-lookup"><span data-stu-id="f8290-142">You should use `createElement()` and assign property values appropriately such as `node.TextContent=`, or use `element.SetAttribute()`/`element[attribute]=` otherwise you expose yourself to DOM-based XSS.</span></span>

## <a name="accessing-encoders-in-code"></a><span data-ttu-id="f8290-143">Доступ к кодировщики в коде</span><span class="sxs-lookup"><span data-stu-id="f8290-143">Accessing encoders in code</span></span>

<span data-ttu-id="f8290-144">Кодировщики HTML, JavaScript и URL-адреса доступны для кода двумя способами, можно ввести их через [внедрения зависимостей](../fundamentals/dependency-injection.md#fundamentals-dependency-injection) или использовании кодировщики по умолчанию, содержащиеся в `System.Text.Encodings.Web` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="f8290-144">The HTML, JavaScript and URL encoders are available to your code in two ways, you can inject them via [dependency injection](../fundamentals/dependency-injection.md#fundamentals-dependency-injection) or you can use the default encoders contained in the `System.Text.Encodings.Web` namespace.</span></span> <span data-ttu-id="f8290-145">При использовании кодировщики по умолчанию любой применяются для диапазонов символов следует рассматривать как безопасные не вступят в силу — кодировщики по умолчанию используйте безопасный из возможных правила кодирования.</span><span class="sxs-lookup"><span data-stu-id="f8290-145">If you use the default encoders then any  you applied to character ranges to be treated as safe will not take effect - the default encoders use the safest encoding rules possible.</span></span>

<span data-ttu-id="f8290-146">Для использования настраиваемых кодировщики через DI конструкторах займет *HtmlEncoder*, *JavaScriptEncoder* и *UrlEncoder* параметр соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="f8290-146">To use the configurable encoders via DI your constructors should take an *HtmlEncoder*, *JavaScriptEncoder* and *UrlEncoder* parameter as appropriate.</span></span> <span data-ttu-id="f8290-147">Например,</span><span class="sxs-lookup"><span data-stu-id="f8290-147">For example;</span></span>

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

## <a name="encoding-url-parameters"></a><span data-ttu-id="f8290-148">Параметры кодировки URL-адреса</span><span class="sxs-lookup"><span data-stu-id="f8290-148">Encoding URL Parameters</span></span>

<span data-ttu-id="f8290-149">Если вы хотите создать строку запроса URL-адрес с ненадежных входных данных, как использование значение `UrlEncoder` для кодирования значения.</span><span class="sxs-lookup"><span data-stu-id="f8290-149">If you want to build a URL query string with untrusted input as a value use the `UrlEncoder` to encode the value.</span></span> <span data-ttu-id="f8290-150">Например:</span><span class="sxs-lookup"><span data-stu-id="f8290-150">For example,</span></span>

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

<span data-ttu-id="f8290-151">После кодирования encodedValue переменная будет содержать `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span><span class="sxs-lookup"><span data-stu-id="f8290-151">After encoding the encodedValue variable will contain `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span></span> <span data-ttu-id="f8290-152">Пробелы, кавычки, знаки пунктуации и другие небезопасных символов кодироваться процента шестнадцатеричные значения, например символ пробела становится % 20.</span><span class="sxs-lookup"><span data-stu-id="f8290-152">Spaces, quotes, punctuation and other unsafe characters will be percent encoded to their hexadecimal value, for example a space character will become %20.</span></span>

>[!WARNING]
> <span data-ttu-id="f8290-153">Не используйте недоверенные входные данные как часть URL-пути.</span><span class="sxs-lookup"><span data-stu-id="f8290-153">Do not use untrusted input as part of a URL path.</span></span> <span data-ttu-id="f8290-154">Всегда передает недоверенные входные данные как значение строки запроса.</span><span class="sxs-lookup"><span data-stu-id="f8290-154">Always pass untrusted input as a query string value.</span></span>

<a name=security-cross-site-scripting-customization></a>

## <a name="customizing-the-encoders"></a><span data-ttu-id="f8290-155">Настройка кодировщики</span><span class="sxs-lookup"><span data-stu-id="f8290-155">Customizing the Encoders</span></span>

<span data-ttu-id="f8290-156">По умолчанию кодировщики используйте безопасный список, ограниченный диапазон Юникода Basic Latin и кодирования всех символов вне этого диапазона как эквиваленты код символа.</span><span class="sxs-lookup"><span data-stu-id="f8290-156">By default encoders use a safe list limited to the Basic Latin Unicode range and encode all characters outside of that range as their character code equivalents.</span></span> <span data-ttu-id="f8290-157">Это поведение также влияет на отрисовку вспомогательной функции тегов Razor и HtmlHelper как кодировщики будет использовать для сравнения строк в выходных данных.</span><span class="sxs-lookup"><span data-stu-id="f8290-157">This behavior also affects Razor TagHelper and HtmlHelper rendering as it will use the encoders to output your strings.</span></span>

<span data-ttu-id="f8290-158">Причины, обуславливающие это сделано для защиты от неизвестных или будущих браузера ошибок (предыдущих ошибок браузера задействуемый копирование основанные на обработку символов не на английском языке).</span><span class="sxs-lookup"><span data-stu-id="f8290-158">The reasoning behind this is to protect against unknown or future browser bugs (previous browser bugs have tripped up parsing based on the processing of non-English characters).</span></span> <span data-ttu-id="f8290-159">Если веб-сайте усложняет использование Нелатинские символы, такие как китайский, кириллица или другим это, скорее всего, не желаемое поведение.</span><span class="sxs-lookup"><span data-stu-id="f8290-159">If your web site makes heavy use of non-Latin characters, such as Chinese, Cyrillic or others this is probably not the behavior you want.</span></span>

<span data-ttu-id="f8290-160">Вы можете настроить списки безопасном кодировщика для включения Юникода, допустимые диапазоны для приложения во время запуска, в `ConfigureServices()`.</span><span class="sxs-lookup"><span data-stu-id="f8290-160">You can customize the encoder safe lists to include Unicode ranges appropriate to your application during startup, in `ConfigureServices()`.</span></span>

<span data-ttu-id="f8290-161">Например, с помощью конфигурации по умолчанию, можно использовать Razor HtmlHelper следующим образом;</span><span class="sxs-lookup"><span data-stu-id="f8290-161">For example, using the default configuration you might use a Razor HtmlHelper like so;</span></span>

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

<span data-ttu-id="f8290-162">При просмотре исходного кода веб-страницы, то будет выведено его обработки следующим образом с кодированием; текст на китайском языке</span><span class="sxs-lookup"><span data-stu-id="f8290-162">When you view the source of the web page you will see it has been rendered as follows, with the Chinese text encoded;</span></span>

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

<span data-ttu-id="f8290-163">Чтобы увеличить ширину символов, считаются безопасном кодировщиком необходимо вставить следующую строку в `ConfigureServices()` метод в `startup.cs`;</span><span class="sxs-lookup"><span data-stu-id="f8290-163">To widen the characters treated as safe by the encoder you would insert the following line into the `ConfigureServices()` method in `startup.cs`;</span></span>

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

<span data-ttu-id="f8290-164">В этом примере расширяется список безопасных для включения CjkUnifiedIdeographs диапазон Юникода.</span><span class="sxs-lookup"><span data-stu-id="f8290-164">This example widens the safe list to include the Unicode Range CjkUnifiedIdeographs.</span></span> <span data-ttu-id="f8290-165">Теперь стал бы выводимые данные</span><span class="sxs-lookup"><span data-stu-id="f8290-165">The rendered output would now become</span></span>

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

<span data-ttu-id="f8290-166">Список надежных диапазоны задаются в виде кодировок Юникода, не языков.</span><span class="sxs-lookup"><span data-stu-id="f8290-166">Safe list ranges are specified as Unicode code charts, not languages.</span></span> <span data-ttu-id="f8290-167">[Стандарт Юникод](http://unicode.org/) имеет список [кода диаграммы](http://www.unicode.org/charts/index.html) можно использовать для поиска диаграммы, содержащей вашей символов.</span><span class="sxs-lookup"><span data-stu-id="f8290-167">The [Unicode standard](http://unicode.org/) has a list of [code charts](http://www.unicode.org/charts/index.html) you can use to find the chart containing your characters.</span></span> <span data-ttu-id="f8290-168">Каждого кодировщика, Html, JavaScript и URL-адрес должен быть настроен отдельно.</span><span class="sxs-lookup"><span data-stu-id="f8290-168">Each encoder, Html, JavaScript and Url, must be configured separately.</span></span>

> [!NOTE]
> <span data-ttu-id="f8290-169">Настройка надежных влияет только на источником через DI кодировщиков.</span><span class="sxs-lookup"><span data-stu-id="f8290-169">Customization of the safe list only affects encoders sourced via DI.</span></span> <span data-ttu-id="f8290-170">Если прямой доступ к кодировщик через `System.Text.Encodings.Web.*Encoder.Default` затем по умолчанию, Basic Latin будет использоваться только списка надежных отправителей.</span><span class="sxs-lookup"><span data-stu-id="f8290-170">If you directly access an encoder via `System.Text.Encodings.Web.*Encoder.Default` then the default, Basic Latin only safelist will be used.</span></span>

## <a name="where-should-encoding-take-place"></a><span data-ttu-id="f8290-171">Где следует поместить кодирования take</span><span class="sxs-lookup"><span data-stu-id="f8290-171">Where should encoding take place?</span></span>

<span data-ttu-id="f8290-172">Применяемый общий подход заключается в том кодирование выполняется во время вывода закодированные значения никогда не должны храниться в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f8290-172">The general accepted practice is that encoding takes place at the point of output and encoded values should never be stored in a database.</span></span> <span data-ttu-id="f8290-173">Кодирование точке выходные данные можно изменить использование данных, например, из HTML-код, значение строки запроса.</span><span class="sxs-lookup"><span data-stu-id="f8290-173">Encoding at the point of output allows you to change the use of data, for example, from HTML to a query string value.</span></span> <span data-ttu-id="f8290-174">Также позволяет легко найти данных без необходимости кодирование значений перед поиском и позволяет воспользоваться преимуществами любые изменения или исправления, внесенные в кодировщиков.</span><span class="sxs-lookup"><span data-stu-id="f8290-174">It also enables you to easily search your data without having to encode values before searching and allows you to take advantage of any changes or bug fixes made to encoders.</span></span>

## <a name="validation-as-an-xss-prevention-technique"></a><span data-ttu-id="f8290-175">Проверка как способ защиты от XSS</span><span class="sxs-lookup"><span data-stu-id="f8290-175">Validation as an XSS prevention technique</span></span>

<span data-ttu-id="f8290-176">Проверка может быть полезным инструментом ограничение атак с XSS.</span><span class="sxs-lookup"><span data-stu-id="f8290-176">Validation can be a useful tool in limiting XSS attacks.</span></span> <span data-ttu-id="f8290-177">Например простой числовой строка, содержащая только символы 0 – 9 не будет вызывать XSS атаки.</span><span class="sxs-lookup"><span data-stu-id="f8290-177">For example, a simple numeric string containing only the characters 0-9 will not trigger an XSS attack.</span></span> <span data-ttu-id="f8290-178">Проверка становится более сложным, следует нужно принимать HTML во вводимых - синтаксического анализа HTML-ввода является сложным, невозможно.</span><span class="sxs-lookup"><span data-stu-id="f8290-178">Validation becomes more complicated should you wish to accept HTML in user input - parsing HTML input is difficult, if not impossible.</span></span> <span data-ttu-id="f8290-179">Другие форматы текста и разметки будет более безопасный параметр для форматированного ввода.</span><span class="sxs-lookup"><span data-stu-id="f8290-179">MarkDown and other text formats would be a safer option for rich input.</span></span> <span data-ttu-id="f8290-180">Никогда не следует полагаться на проверку сама по себе.</span><span class="sxs-lookup"><span data-stu-id="f8290-180">You should never rely on validation alone.</span></span> <span data-ttu-id="f8290-181">Всегда кодировать недоверенные входные данные перед выводом, независимо от того, какая проверка выполнена.</span><span class="sxs-lookup"><span data-stu-id="f8290-181">Always encode untrusted input before output, no matter what validation you have performed.</span></span>
