---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: Введение в программирование веб-ASP.NET с использованием синтаксиса Razor (C#) | Документация Майкрософт
author: tfitzmac
description: В этой главе дает Общие сведения о программировании с помощью веб-страниц ASP.NET с использованием синтаксиса Razor. ASP.NET — технология Майкрософт для выполнения динамических веб-pa...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: b242bf52bbd63d726e6ce6ab7be01a1b81c5bf1b
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758262"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a><span data-ttu-id="1c451-104">Введение в программирование веб-ASP.NET с использованием синтаксиса Razor (C#)</span><span class="sxs-lookup"><span data-stu-id="1c451-104">Introduction to ASP.NET Web Programming Using the Razor Syntax (C#)</span></span>
====================
<span data-ttu-id="1c451-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="1c451-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="1c451-106">В этой статье дает Общие сведения о программировании с помощью веб-страниц ASP.NET с использованием синтаксиса Razor.</span><span class="sxs-lookup"><span data-stu-id="1c451-106">This article gives you an overview of programming with ASP.NET Web Pages using the Razor syntax.</span></span> <span data-ttu-id="1c451-107">ASP.NET — технология Майкрософт для выполнения динамических веб-страниц на веб-серверах.</span><span class="sxs-lookup"><span data-stu-id="1c451-107">ASP.NET is Microsoft's technology for running dynamic web pages on web servers.</span></span> <span data-ttu-id="1c451-108">Этой статьи, рассматриваются с использованием языка программирования C#.</span><span class="sxs-lookup"><span data-stu-id="1c451-108">This articles focuses on using the C# programming language.</span></span>
> 
> <span data-ttu-id="1c451-109">**Вы узнаете, как**:</span><span class="sxs-lookup"><span data-stu-id="1c451-109">**What you'll learn**:</span></span>
> 
> - <span data-ttu-id="1c451-110">Top 8, программирование советов по началу работы с программирования веб-страниц ASP.NET с использованием синтаксиса Razor.</span><span class="sxs-lookup"><span data-stu-id="1c451-110">The top 8 programming tips for getting started with programming ASP.NET Web Pages using Razor syntax.</span></span>
> - <span data-ttu-id="1c451-111">Основные концепции программирования вам.</span><span class="sxs-lookup"><span data-stu-id="1c451-111">Basic programming concepts you'll need.</span></span>
> - <span data-ttu-id="1c451-112">Какой код сервера ASP.NET и синтаксиса Razor посвящен.</span><span class="sxs-lookup"><span data-stu-id="1c451-112">What ASP.NET server code and the Razor syntax is all about.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="1c451-113">Версии программного обеспечения</span><span class="sxs-lookup"><span data-stu-id="1c451-113">Software versions</span></span>
> 
> 
> - <span data-ttu-id="1c451-114">Веб-страниц ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="1c451-114">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="1c451-115">Этот учебник также работает с ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="1c451-115">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="the-top-8-programming-tips"></a><span data-ttu-id="1c451-116">Top 8 советов по программированию</span><span class="sxs-lookup"><span data-stu-id="1c451-116">The Top 8 Programming Tips</span></span>

<span data-ttu-id="1c451-117">В этом разделе перечислены несколько советов, которые абсолютно необходимо знать, как приступить к созданию кода сервера ASP.NET с использованием синтаксиса Razor.</span><span class="sxs-lookup"><span data-stu-id="1c451-117">This section lists a few tips that you absolutely need to know as you start writing ASP.NET server code using the Razor syntax.</span></span>

> [!NOTE]
> <span data-ttu-id="1c451-118">Синтаксис Razor основан на языке C#, и это язык, который используется чаще всего с помощью веб-страниц ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1c451-118">The Razor syntax is based on the C# programming language, and that's the language that's used most often with ASP.NET Web Pages.</span></span> <span data-ttu-id="1c451-119">Однако синтаксис Razor поддерживает язык Visual Basic и все, что вы увидите, что также можно сделать в Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="1c451-119">However, the Razor syntax also supports the Visual Basic language, and everything you see you can also do in Visual Basic.</span></span> <span data-ttu-id="1c451-120">Дополнительные сведения см. в приложении [языка Visual Basic и синтаксиса](https://go.microsoft.com/fwlink/?LinkId=202908).</span><span class="sxs-lookup"><span data-stu-id="1c451-120">For details, see the appendix [Visual Basic Language and Syntax](https://go.microsoft.com/fwlink/?LinkId=202908).</span></span>


<span data-ttu-id="1c451-121">Дополнительные сведения о большей части эти приемы программирования можно найти далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="1c451-121">You can find more details about most of these programming techniques later in the article.</span></span>

### <a name="1-you-add-code-to-a-page-using-the--character"></a><span data-ttu-id="1c451-122">1. Добавьте код для страницы с помощью символа @</span><span class="sxs-lookup"><span data-stu-id="1c451-122">1. You add code to a page using the @ character</span></span>

<span data-ttu-id="1c451-123">`@` Символ запускает встроенные выражения, одной инструкции блоков и блоков с несколькими инструкциями:</span><span class="sxs-lookup"><span data-stu-id="1c451-123">The `@` character starts inline expressions, single statement blocks, and multi-statement blocks:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

<span data-ttu-id="1c451-124">Это как выглядеть эти инструкции, при запуске страницы в браузере:</span><span class="sxs-lookup"><span data-stu-id="1c451-124">This is what these statements look like when the page runs in a browser:</span></span>

![Razor Img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> <span data-ttu-id="1c451-126">**Кодировка HTML**</span><span class="sxs-lookup"><span data-stu-id="1c451-126">**HTML Encoding**</span></span>
> 
> <span data-ttu-id="1c451-127">При выводе содержимого на страницы с помощью `@` символов, как и в предыдущих примерах ASP.NET HTML кодирует выходные данные.</span><span class="sxs-lookup"><span data-stu-id="1c451-127">When you display content in a page using the `@` character, as in the preceding examples, ASP.NET HTML-encodes the output.</span></span> <span data-ttu-id="1c451-128">Этот параметр заменяет зарезервированные символы HTML (например, `<` и `>` и `&`) с кодами, которые позволяют символов для отображения в качестве символов на веб-странице, а как HTML-теги или сущности.</span><span class="sxs-lookup"><span data-stu-id="1c451-128">This replaces reserved HTML characters (such as `<` and `>` and `&`) with codes that enable the characters to be displayed as characters in a web page instead of being interpreted as HTML tags or entities.</span></span> <span data-ttu-id="1c451-129">Без HTML-кодирования, выходные данные из кода сервера могут отображаться неправильно и может предоставить доступ к странице угрозам безопасности.</span><span class="sxs-lookup"><span data-stu-id="1c451-129">Without HTML encoding, the output from your server code might not display correctly, and could expose a page to security risks.</span></span>
> 
> <span data-ttu-id="1c451-130">Если ваша цель — вывод HTML-разметка, отображает теги как разметка (например `<p></p>` для абзаца или `<em></em>` для выделения текста), см. в разделе [Объединение текста, разметка и код в блоках кода](#BM_CombiningTextMarkupAndCode) далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="1c451-130">If your goal is to output HTML markup that renders tags as markup (for example `<p></p>` for a paragraph or `<em></em>` to emphasize text), see the section [Combining Text, Markup, and Code in Code Blocks](#BM_CombiningTextMarkupAndCode) later in this article.</span></span>
> 
> <span data-ttu-id="1c451-131">Дополнительные сведения о HTML-кодирования в [работа с формами](https://go.microsoft.com/fwlink/?LinkId=202892).</span><span class="sxs-lookup"><span data-stu-id="1c451-131">You can read more about HTML encoding in [Working with Forms](https://go.microsoft.com/fwlink/?LinkId=202892).</span></span>


### <a name="2-you-enclose-code-blocks-in-braces"></a><span data-ttu-id="1c451-132">2. Блоки кода, заключите в скобки</span><span class="sxs-lookup"><span data-stu-id="1c451-132">2. You enclose code blocks in braces</span></span>

<span data-ttu-id="1c451-133">Объект *блок кода* включает один или несколько операторов кода и заключается в фигурные скобки.</span><span class="sxs-lookup"><span data-stu-id="1c451-133">A *code block* includes one or more code statements and is enclosed in braces.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

<span data-ttu-id="1c451-134">Результат отображается в браузере:</span><span class="sxs-lookup"><span data-stu-id="1c451-134">The result displayed in a browser:</span></span>

![Razor Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a><span data-ttu-id="1c451-136">3. Внутри блока завершении каждого оператора точкой с запятой</span><span class="sxs-lookup"><span data-stu-id="1c451-136">3. Inside a block, you end each code statement with a semicolon</span></span>

<span data-ttu-id="1c451-137">Внутри блока кода каждая инструкция полный код должен заканчиваться точкой с запятой.</span><span class="sxs-lookup"><span data-stu-id="1c451-137">Inside a code block, each complete code statement must end with a semicolon.</span></span> <span data-ttu-id="1c451-138">Встроенные выражения не заканчивается точкой с запятой.</span><span class="sxs-lookup"><span data-stu-id="1c451-138">Inline expressions don't end with a semicolon.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a><span data-ttu-id="1c451-139">4. Использовать переменные для хранения значений</span><span class="sxs-lookup"><span data-stu-id="1c451-139">4. You use variables to store values</span></span>

<span data-ttu-id="1c451-140">Можно хранить значения в *переменной*, включая строки, числа и даты, и т.д. Создание новой переменной с помощью `var` ключевое слово.</span><span class="sxs-lookup"><span data-stu-id="1c451-140">You can store values in a *variable*, including strings, numbers, and dates, etc. You create a new variable using the `var` keyword.</span></span> <span data-ttu-id="1c451-141">Значения переменных можно вставить непосредственно в страницы с помощью `@`.</span><span class="sxs-lookup"><span data-stu-id="1c451-141">You can insert variable values directly in a page using `@`.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

<span data-ttu-id="1c451-142">Результат отображается в браузере:</span><span class="sxs-lookup"><span data-stu-id="1c451-142">The result displayed in a browser:</span></span>

![Razor Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a><span data-ttu-id="1c451-144">5. Вы литеральные значения строк следует заключить в двойные кавычки</span><span class="sxs-lookup"><span data-stu-id="1c451-144">5. You enclose literal string values in double quotation marks</span></span>

<span data-ttu-id="1c451-145">Объект *строка* — это последовательность символов, которые обрабатываются как текст.</span><span class="sxs-lookup"><span data-stu-id="1c451-145">A *string* is a sequence of characters that are treated as text.</span></span> <span data-ttu-id="1c451-146">Чтобы указать строку, заключите его в двойные кавычки:</span><span class="sxs-lookup"><span data-stu-id="1c451-146">To specify a string, you enclose it in double quotation marks:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

<span data-ttu-id="1c451-147">Если строка, которая будет отображаться содержит символ обратной косой черты ( `\` ) или двойные кавычки ( `"` ), используйте *буквального строкового литерала* , начинающийся с `@` оператор.</span><span class="sxs-lookup"><span data-stu-id="1c451-147">If the string that you want to display contains a backslash character ( `\` ) or double quotation marks ( `"` ), use a *verbatim string literal* that's prefixed with the `@` operator.</span></span> <span data-ttu-id="1c451-148">(В C# \ символ имеет особое значение, если вы не используете буквального строкового литерала.)</span><span class="sxs-lookup"><span data-stu-id="1c451-148">(In C#, the \ character has special meaning unless you use a verbatim string literal.)</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

<span data-ttu-id="1c451-149">Чтобы внедрить двойные кавычки, используйте буквального строкового литерала и повторите кавычки:</span><span class="sxs-lookup"><span data-stu-id="1c451-149">To embed double quotation marks, use a verbatim string literal and repeat the quotation marks:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

<span data-ttu-id="1c451-150">Ниже приведен результат использования обоих этих примерах на странице.</span><span class="sxs-lookup"><span data-stu-id="1c451-150">Here's the result of using both of these examples in a page:</span></span>

![Razor Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> <span data-ttu-id="1c451-152">Обратите внимание, что `@` символ используется для пометки буквальные строковые литералы в C# и для пометки кода на страницах ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1c451-152">Notice that the `@` character is used both to mark verbatim string literals in C# and to mark code in ASP.NET pages.</span></span>


### <a name="6-code-is-case-sensitive"></a><span data-ttu-id="1c451-153">6. Код чувствительно к регистру</span><span class="sxs-lookup"><span data-stu-id="1c451-153">6. Code is case sensitive</span></span>

<span data-ttu-id="1c451-154">В C#, ключевые слова (такие как `var`, `true`, и `if`) и в именах переменных учитывается регистр.</span><span class="sxs-lookup"><span data-stu-id="1c451-154">In C#, keywords (like `var`, `true`, and `if`) and variable names are case sensitive.</span></span> <span data-ttu-id="1c451-155">Следующие строки кода создают две различные переменные `lastName` и `LastName.`</span><span class="sxs-lookup"><span data-stu-id="1c451-155">The following lines of code create two different variables, `lastName` and `LastName.`</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

<span data-ttu-id="1c451-156">Если объявить переменную как `var lastName = "Smith";` и при попытке сослаться на эту переменную, на странице как `@LastName`, ошибка результаты, так как `LastName` не распознается.</span><span class="sxs-lookup"><span data-stu-id="1c451-156">If you declare a variable as `var lastName = "Smith";` and if you try to reference that variable in your page as `@LastName`, an error results because `LastName` won't be recognized.</span></span>

> [!NOTE]
> <span data-ttu-id="1c451-157">В Visual Basic, ключевые слова и переменные были *не* с учетом регистра.</span><span class="sxs-lookup"><span data-stu-id="1c451-157">In Visual Basic, keywords and variables are *not* case sensitive.</span></span>


### <a name="7-much-of-your-coding-involves-objects"></a><span data-ttu-id="1c451-158">7. Большая часть программирования включает в себя объекты</span><span class="sxs-lookup"><span data-stu-id="1c451-158">7. Much of your coding involves objects</span></span>

<span data-ttu-id="1c451-159">*Объект* представляет нечто, что можно программировать &#8212; страницы текстовое поле, файл, изображения, веб-запроса, сообщение электронной почты, запись о заказчике (строки базы данных), и т.д. Объекты имеют свойства, описывающие их характеристик и чтение или изменение &#8212; имеет объект с текстовыми полями `Text` (среди прочего), объект запроса имеет `Url` , сообщение электронной почты имеет `From` свойство, и объект клиента имеет `FirstName` свойства.</span><span class="sxs-lookup"><span data-stu-id="1c451-159">An *object* represents a thing that you can program with &#8212; a page, a text box, a file, an image, a web request, an email message, a customer record (database row), etc. Objects have properties that describe their characteristics and that you can read or change &#8212; a text box object has a `Text` property (among others), a request object has a `Url` property, an email message has a `From` property, and a customer object has a `FirstName` property.</span></span> <span data-ttu-id="1c451-160">Объекты также имеют методы, которые являются &quot;команд&quot; они могут выполнять.</span><span class="sxs-lookup"><span data-stu-id="1c451-160">Objects also have methods that are the &quot;verbs&quot; they can perform.</span></span> <span data-ttu-id="1c451-161">Примеры включают объект файла `Save` метод, объект изображения `Rotate` метод и объект по электронной почте `Send` метод.</span><span class="sxs-lookup"><span data-stu-id="1c451-161">Examples include a file object's `Save` method, an image object's `Rotate` method, and an email object's `Send` method.</span></span>

<span data-ttu-id="1c451-162">Вы будете часто иметь дело с `Request` объекта, который предоставляет сведения, такие как значения текстовых полей (поля формы) на странице, тип браузера, выдавший запрос, URL-адрес страницы, удостоверение пользователя и т. д. В следующем примере показано, как доступ к свойствам `Request` объекта и как вызывать `MapPath` метод `Request` объекта, который дает абсолютный путь к странице на сервере:</span><span class="sxs-lookup"><span data-stu-id="1c451-162">You'll often work with the `Request` object, which gives you information like the values of text boxes (form fields) on the page, what type of browser made the request, the URL of the page, the user identity, etc. The following example shows how to access properties of the `Request` object and how to call the `MapPath` method of the `Request` object, which gives you the absolute path of the page on the server:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

<span data-ttu-id="1c451-163">Результат отображается в браузере:</span><span class="sxs-lookup"><span data-stu-id="1c451-163">The result displayed in a browser:</span></span>

![Razor Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a><span data-ttu-id="1c451-165">8. Можно написать код, который принимает решения</span><span class="sxs-lookup"><span data-stu-id="1c451-165">8. You can write code that makes decisions</span></span>

<span data-ttu-id="1c451-166">Ключевой особенностью динамических веб-страниц — это, вы можете определять порядок действий в зависимости от условия.</span><span class="sxs-lookup"><span data-stu-id="1c451-166">A key feature of dynamic web pages is that you can determine what to do based on conditions.</span></span> <span data-ttu-id="1c451-167">Наиболее распространенный способ сделать это — с `if` инструкции (и необязательно `else` инструкции).</span><span class="sxs-lookup"><span data-stu-id="1c451-167">The most common way to do this is with the `if` statement (and optional `else` statement).</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

<span data-ttu-id="1c451-168">Инструкция `if(IsPost)` является быстрым способом написания `if(IsPost == true)`.</span><span class="sxs-lookup"><span data-stu-id="1c451-168">The statement `if(IsPost)` is a shorthand way of writing `if(IsPost == true)`.</span></span> <span data-ttu-id="1c451-169">Вместе с `if` инструкции, существует множество способов для проверки условий, повторите блоки кода, и т. д., которые являются описано далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="1c451-169">Along with `if` statements, there are a variety of ways to test conditions, repeat blocks of code, and so on, which are described later in this article.</span></span>

<span data-ttu-id="1c451-170">Результат отображается в браузере (после нажатия кнопки **отправить**):</span><span class="sxs-lookup"><span data-stu-id="1c451-170">The result displayed in a browser (after clicking **Submit**):</span></span>

![Razor Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a><span data-ttu-id="1c451-172">HTTP GET и POST методы и свойства IsPost</span><span class="sxs-lookup"><span data-stu-id="1c451-172">HTTP GET and POST Methods and the IsPost Property</span></span>
> 
> <span data-ttu-id="1c451-173">Протокол, используемый для веб-страниц (HTTP) поддерживает очень ограниченное число методы (команды), которые используются для выполнения запросов к серверу.</span><span class="sxs-lookup"><span data-stu-id="1c451-173">The protocol used for web pages (HTTP) supports a very limited number of methods (verbs) that are used to make requests to the server.</span></span> <span data-ttu-id="1c451-174">Два самых распространенных рисков: GET, который используется для чтения страницы, и POST, который используется для отправки страницы.</span><span class="sxs-lookup"><span data-stu-id="1c451-174">The two most common ones are GET, which is used to read a page, and POST, which is used to submit a page.</span></span> <span data-ttu-id="1c451-175">Как правило первый раз пользователь запрашивает страницу, страница запрашивается с помощью запроса GET.</span><span class="sxs-lookup"><span data-stu-id="1c451-175">In general, the first time a user requests a page, the page is requested using GET.</span></span> <span data-ttu-id="1c451-176">Если пользователь вводит в форму и нажимает кнопку отправки, браузер отправляет запрос POST на сервер.</span><span class="sxs-lookup"><span data-stu-id="1c451-176">If the user fills in a form and then clicks a submit button, the browser makes a POST request to the server.</span></span>
> 
> <span data-ttu-id="1c451-177">В веб-программирования часто бывает полезно знать ли страница запрашивается как GET или POST, чтобы вы знаете, как для обработки страницы.</span><span class="sxs-lookup"><span data-stu-id="1c451-177">In web programming, it's often useful to know whether a page is being requested as a GET or as a POST so that you know how to process the page.</span></span> <span data-ttu-id="1c451-178">Веб-страницах ASP.NET, можно использовать `IsPost` свойство, чтобы увидеть, является ли запрос GET или POST.</span><span class="sxs-lookup"><span data-stu-id="1c451-178">In ASP.NET Web Pages, you can use the `IsPost` property to see whether a request is a GET or a POST.</span></span> <span data-ttu-id="1c451-179">Если запрос POST, `IsPost` свойство возвратит значение true, и вы можете выполнять действия, такие как чтение значения текстовых полей в форме.</span><span class="sxs-lookup"><span data-stu-id="1c451-179">If the request is a POST, the `IsPost` property will return true, and you can do things like read the values of text boxes on a form.</span></span> <span data-ttu-id="1c451-180">Вы увидите множество примеров продемонстрируем способ обработки страницы по-разному в зависимости от значения `IsPost`.</span><span class="sxs-lookup"><span data-stu-id="1c451-180">Many examples you'll see show you how to process the page differently depending on the value of `IsPost`.</span></span>


## <a name="a-simple-code-example"></a><span data-ttu-id="1c451-181">Простой пример кода</span><span class="sxs-lookup"><span data-stu-id="1c451-181">A Simple Code Example</span></span>

<span data-ttu-id="1c451-182">Эта процедура показано, как создать страницу, которая иллюстрирует основные приемы программирования.</span><span class="sxs-lookup"><span data-stu-id="1c451-182">This procedure shows you how to create a page that illustrates basic programming techniques.</span></span> <span data-ttu-id="1c451-183">В примере создайте страницу, которая позволяет пользователям вводить двух чисел, а затем он добавляет их и отображает результат.</span><span class="sxs-lookup"><span data-stu-id="1c451-183">In the example, you create a page that lets users enter two numbers, then it adds them and displays the result.</span></span>

1. <span data-ttu-id="1c451-184">В редакторе создайте файл и назовите его *AddNumbers.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1c451-184">In your editor, create a new file and name it *AddNumbers.cshtml*.</span></span>
2. <span data-ttu-id="1c451-185">Скопируйте следующий код и разметку в странице, заменив все уже на странице.</span><span class="sxs-lookup"><span data-stu-id="1c451-185">Copy the following code and markup into the page, replacing anything already in the page.</span></span>  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    <span data-ttu-id="1c451-186">Ниже приведены некоторые аспекты отметить:</span><span class="sxs-lookup"><span data-stu-id="1c451-186">Here are some things for you to note:</span></span>

    - <span data-ttu-id="1c451-187">`@` Символ запускается первым блоком кода в странице, и перед `totalMessage` переменной, которое внедрено в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="1c451-187">The `@` character starts the first block of code in the page, and it precedes the `totalMessage` variable that's embedded near the bottom of the page.</span></span>
    - <span data-ttu-id="1c451-188">В верхней части страницы будет заключен в фигурные скобки.</span><span class="sxs-lookup"><span data-stu-id="1c451-188">The block at the top of the page is enclosed in braces.</span></span>
    - <span data-ttu-id="1c451-189">В блок на вершине всех строк заканчиваются точкой с запятой.</span><span class="sxs-lookup"><span data-stu-id="1c451-189">In the block at the top, all lines end with a semicolon.</span></span>
    - <span data-ttu-id="1c451-190">Переменные `total`, `num1`, `num2`, и `totalMessage` хранить несколько чисел и строка.</span><span class="sxs-lookup"><span data-stu-id="1c451-190">The variables `total`, `num1`, `num2`, and `totalMessage` store several numbers and a string.</span></span>
    - <span data-ttu-id="1c451-191">Строковый литерал значение, присваиваемое `totalMessage` переменная находится в двойные кавычки.</span><span class="sxs-lookup"><span data-stu-id="1c451-191">The literal string value assigned to the `totalMessage` variable is in double quotation marks.</span></span>
    - <span data-ttu-id="1c451-192">Так как код, с учетом регистра, когда `totalMessage` переменная используется в нижней части страницы, его имя должно точно соответствовать переменной в верхней.</span><span class="sxs-lookup"><span data-stu-id="1c451-192">Because the code is case-sensitive, when the `totalMessage` variable is used near the bottom of the page, its name must match the variable at the top exactly.</span></span>
    - <span data-ttu-id="1c451-193">Выражение `num1.AsInt() + num2.AsInt()` показано, как работать с объектами и методами.</span><span class="sxs-lookup"><span data-stu-id="1c451-193">The expression `num1.AsInt() + num2.AsInt()` shows how to work with objects and methods.</span></span> <span data-ttu-id="1c451-194">`AsInt` Метод для каждой переменной преобразует строку, введенные пользователем, в число (целое число), чтобы на нем можно выполнять арифметические операции.</span><span class="sxs-lookup"><span data-stu-id="1c451-194">The `AsInt` method on each variable converts the string entered by a user to a number (an integer) so that you can perform arithmetic on it.</span></span>
    - <span data-ttu-id="1c451-195">`<form>` Тег включает `method="post"` атрибута.</span><span class="sxs-lookup"><span data-stu-id="1c451-195">The `<form>` tag includes a `method="post"` attribute.</span></span> <span data-ttu-id="1c451-196">Указывает, что когда пользователь щелкает **добавить**, страницы будут отправляться на сервер с помощью метода HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="1c451-196">This specifies that when the user clicks **Add**, the page will be sent to the server using the HTTP POST method.</span></span> <span data-ttu-id="1c451-197">При отправке страницы `if(IsPost)` результатом проверки является значение true, а также условного кода, результат сложения чисел.</span><span class="sxs-lookup"><span data-stu-id="1c451-197">When the page is submitted, the `if(IsPost)` test evaluates to true and the conditional code runs, displaying the result of adding the numbers.</span></span>
3. <span data-ttu-id="1c451-198">Сохраните страницу и запустите его в браузере.</span><span class="sxs-lookup"><span data-stu-id="1c451-198">Save the page and run it in a browser.</span></span> <span data-ttu-id="1c451-199">(Убедитесь, что выбран страницы **файлы** рабочей области, прежде чем запускать его.) Введите двумя целыми числами и нажмите кнопку **добавить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="1c451-199">(Make sure the page is selected in the **Files** workspace before you run it.) Enter two whole numbers and then click the **Add** button.</span></span> 

    ![Razor Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a><span data-ttu-id="1c451-201">Основные понятия программирования</span><span class="sxs-lookup"><span data-stu-id="1c451-201">Basic Programming Concepts</span></span>

<span data-ttu-id="1c451-202">В этой статье предоставляются общие сведения о веб-программирование ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1c451-202">This article provides you with an overview of ASP.NET web programming.</span></span> <span data-ttu-id="1c451-203">Это не исчерпывающий анализ, просто вкратце описал принципы программирования, которые будут использоваться чаще всего.</span><span class="sxs-lookup"><span data-stu-id="1c451-203">It isn't an exhaustive examination, just a quick tour through the programming concepts you'll use most often.</span></span> <span data-ttu-id="1c451-204">Тем не менее он охватывает почти все, что необходимо приступить к работе с веб-страниц ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1c451-204">Even so, it covers almost everything you'll need to get started with ASP.NET Web Pages.</span></span>

<span data-ttu-id="1c451-205">Но сначала необходимо немного техническими знаниями.</span><span class="sxs-lookup"><span data-stu-id="1c451-205">But first, a little technical background.</span></span>

### <a name="the-razor-syntax-server-code-and-aspnet"></a><span data-ttu-id="1c451-206">Синтаксис Razor, серверный код и ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1c451-206">The Razor Syntax, Server Code, and ASP.NET</span></span>

<span data-ttu-id="1c451-207">Синтаксис Razor — это простой синтаксис программирования для внедрения серверного кода на веб-странице.</span><span class="sxs-lookup"><span data-stu-id="1c451-207">Razor syntax is a simple programming syntax for embedding server-based code in a web page.</span></span> <span data-ttu-id="1c451-208">В веб-страницы с синтаксисом Razor, существует два типа содержимого: клиент содержимое и серверного кода.</span><span class="sxs-lookup"><span data-stu-id="1c451-208">In a web page that uses the Razor syntax, there are two kinds of content: client content and server code.</span></span> <span data-ttu-id="1c451-209">Содержимое клиента является материалы, вы привыкли в веб-страницах: HTML-разметку (элементы), сведения о стиле такие как CSS, может быть определенный клиентский скрипт, например JavaScript и обычного текста.</span><span class="sxs-lookup"><span data-stu-id="1c451-209">Client content is the stuff you're used to in web pages: HTML markup (elements), style information such as CSS, maybe some client script such as JavaScript, and plain text.</span></span>

<span data-ttu-id="1c451-210">Синтаксис Razor позволяет добавлять код сервера к содержимому этого клиента.</span><span class="sxs-lookup"><span data-stu-id="1c451-210">Razor syntax lets you add server code to this client content.</span></span> <span data-ttu-id="1c451-211">Если на странице кода сервера, сервер запускает этот код во-первых, перед отправкой страницы в браузер.</span><span class="sxs-lookup"><span data-stu-id="1c451-211">If there's server code in the page, the server runs that code first, before it sends the page to the browser.</span></span> <span data-ttu-id="1c451-212">Запустив на сервере, код можно выполнять задачи, может быть гораздо более сложные решения с использованием содержимого клиента отдельно, как и доступ к базам данных на основе сервера.</span><span class="sxs-lookup"><span data-stu-id="1c451-212">By running on the server, the code can perform tasks that can be a lot more complex to do using client content alone, like accessing server-based databases.</span></span> <span data-ttu-id="1c451-213">Самое главное, серверный код может динамически генерировать клиентский контент &#8212; он может создавать разметку HTML или другое содержимое в режиме реального времени и отправьте его в браузер вместе со статическим HTML-страница может содержать.</span><span class="sxs-lookup"><span data-stu-id="1c451-213">Most importantly, server code can dynamically create client content &#8212; it can generate HTML markup or other content on the fly and then send it to the browser along with any static HTML that the page might contain.</span></span> <span data-ttu-id="1c451-214">С точки зрения браузера клиента содержимое, которое генерируется код сервера ничем не отличается от любого другого содержимого клиента.</span><span class="sxs-lookup"><span data-stu-id="1c451-214">From the browser's perspective, client content that's generated by your server code is no different than any other client content.</span></span> <span data-ttu-id="1c451-215">Как вы уже видели, серверный код, который требуется довольно прост.</span><span class="sxs-lookup"><span data-stu-id="1c451-215">As you've already seen, the server code that's required is quite simple.</span></span>

<span data-ttu-id="1c451-216">Веб-страниц ASP.NET с синтаксисом Razor имеют специальное расширение (*.cshtml* или *.vbhtml*).</span><span class="sxs-lookup"><span data-stu-id="1c451-216">ASP.NET web pages that include the Razor syntax have a special file extension (*.cshtml* or *.vbhtml*).</span></span> <span data-ttu-id="1c451-217">Сервер распознает эти расширения, запускает код, помеченный синтаксисом Razor, а затем отправляет страницу в браузер.</span><span class="sxs-lookup"><span data-stu-id="1c451-217">The server recognizes these extensions, runs the code that's marked with Razor syntax, and then sends the page to the browser.</span></span>

### <a name="where-does-aspnet-fit-in"></a><span data-ttu-id="1c451-218">ASP.NET помещается?</span><span class="sxs-lookup"><span data-stu-id="1c451-218">Where does ASP.NET fit in?</span></span>

<span data-ttu-id="1c451-219">Синтаксис Razor основан на технологии корпорации Майкрософт, называемой ASP.NET, которая в свою очередь основана на Microsoft .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="1c451-219">Razor syntax is based on a technology from Microsoft called ASP.NET, which in turn is based on the Microsoft .NET Framework.</span></span> <span data-ttu-id="1c451-220">.NET Framework является большой и полную платформу программирования от корпорации Майкрософт по разработке компьютерных приложений практически любого типа.</span><span class="sxs-lookup"><span data-stu-id="1c451-220">The.NET Framework is a big, comprehensive programming framework from Microsoft for developing virtually any type of computer application.</span></span> <span data-ttu-id="1c451-221">ASP.NET является частью .NET Framework, предназначенный специально для создания веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="1c451-221">ASP.NET is the part of the .NET Framework that's specifically designed for creating web applications.</span></span> <span data-ttu-id="1c451-222">Разработчики использовали ASP.NET для создания многих крупнейших и самый высокий трафик веб-сайтов в мире.</span><span class="sxs-lookup"><span data-stu-id="1c451-222">Developers have used ASP.NET to create many of the largest and highest-traffic websites in the world.</span></span> <span data-ttu-id="1c451-223">(Любое время вы видите расширение имени файла *.aspx* как часть URL-адрес узла, вы будете знать, что сайт, написанный с использованием ASP.NET.)</span><span class="sxs-lookup"><span data-stu-id="1c451-223">(Any time you see the file-name extension *.aspx* as part of the URL in a site, you'll know that the site was written using ASP.NET.)</span></span>

<span data-ttu-id="1c451-224">Синтаксис Razor предоставляет все возможности ASP.NET, но с помощью упрощенного синтаксиса, которая проще для получения, если вы только начинаете знакомиться и который станет продуктивнее ли вы специалистом.</span><span class="sxs-lookup"><span data-stu-id="1c451-224">The Razor syntax gives you all the power of ASP.NET, but using a simplified syntax that's easier to learn if you're a beginner and that makes you more productive if you're an expert.</span></span> <span data-ttu-id="1c451-225">Несмотря на то, что этот синтаксис прост в использовании, ее семейства связи с ASP.NET и .NET Framework означает, что как веб-сайты становятся все более сложными, мощь больше платформ, доступные пользователю.</span><span class="sxs-lookup"><span data-stu-id="1c451-225">Even though this syntax is simple to use, its family relationship to ASP.NET and the .NET Framework means that as your websites become more sophisticated, you have the power of the larger frameworks available to you.</span></span>

![Razor Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> <span data-ttu-id="1c451-227">**Классы и экземпляры**</span><span class="sxs-lookup"><span data-stu-id="1c451-227">**Classes and Instances**</span></span>
> 
> <span data-ttu-id="1c451-228">Код сервера ASP.NET использует объекты, которые в свою очередь лежит идея классов.</span><span class="sxs-lookup"><span data-stu-id="1c451-228">ASP.NET server code uses objects, which are in turn built on the idea of classes.</span></span> <span data-ttu-id="1c451-229">Класс является определение или шаблон для объекта.</span><span class="sxs-lookup"><span data-stu-id="1c451-229">The class is the definition or template for an object.</span></span> <span data-ttu-id="1c451-230">Например, приложение может содержать `Customer` класс, который определяет свойства и методы, необходимые для любого объекта клиента.</span><span class="sxs-lookup"><span data-stu-id="1c451-230">For example, an application might contain a `Customer` class that defines the properties and methods that any customer object needs.</span></span>
> 
> <span data-ttu-id="1c451-231">Если приложению необходима для работы с данными клиента, он создает экземпляр (или *создает*) объект customer.</span><span class="sxs-lookup"><span data-stu-id="1c451-231">When the application needs to work with actual customer information, it creates an instance of (or *instantiates*) a customer object.</span></span> <span data-ttu-id="1c451-232">Каждого клиента — это отдельный экземпляр `Customer` класса.</span><span class="sxs-lookup"><span data-stu-id="1c451-232">Each individual customer is a separate instance of the `Customer` class.</span></span> <span data-ttu-id="1c451-233">Каждый экземпляр поддерживает те же свойства и методы, но значения свойств для каждого экземпляра обычно отличаются, так как каждый объект клиента является уникальным.</span><span class="sxs-lookup"><span data-stu-id="1c451-233">Every instance supports the same properties and methods, but the property values for each instance are typically different, because each customer object is unique.</span></span> <span data-ttu-id="1c451-234">В объекте одного клиента `LastName` свойство может быть «Smith»; в другом объекте клиента, `LastName` свойство может быть «Jones.»</span><span class="sxs-lookup"><span data-stu-id="1c451-234">In one customer object, the `LastName` property might be "Smith"; in another customer object, the `LastName` property might be "Jones."</span></span>
> 
> <span data-ttu-id="1c451-235">Аналогичным образом, любой отдельных веб-страницы в веб-узла будет `Page` объект, который является экземпляром класса `Page` класса.</span><span class="sxs-lookup"><span data-stu-id="1c451-235">Similarly, any individual web page in your site is a `Page` object that's an instance of the `Page` class.</span></span> <span data-ttu-id="1c451-236">Кнопка на странице `Button` объект, который является экземпляром класса `Button` класс и т. д.</span><span class="sxs-lookup"><span data-stu-id="1c451-236">A button on the page is a `Button` object that is an instance of the `Button` class, and so on.</span></span> <span data-ttu-id="1c451-237">Каждый экземпляр имеет свои собственные характеристики, но все они основаны на параметрах, указанных в определении класса объекта.</span><span class="sxs-lookup"><span data-stu-id="1c451-237">Each instance has its own characteristics, but they all are based on what's specified in the object's class definition.</span></span>


## <a name="basic-syntax"></a><span data-ttu-id="1c451-238">Базовый синтаксис</span><span class="sxs-lookup"><span data-stu-id="1c451-238">Basic Syntax</span></span>

<span data-ttu-id="1c451-239">Вы уже видели простой пример, как создать страницу ASP.NET Web Pages и как можно добавить код сервера для HTML-разметка.</span><span class="sxs-lookup"><span data-stu-id="1c451-239">Earlier you saw a basic example of how to create an ASP.NET Web Pages page, and how you can add server code to HTML markup.</span></span> <span data-ttu-id="1c451-240">Здесь вы узнаете основы написания кода сервера ASP.NET, с помощью синтаксиса Razor &#8212; то есть правила языка программирования.</span><span class="sxs-lookup"><span data-stu-id="1c451-240">Here you'll learn the basics of writing ASP.NET server code using the Razor syntax &#8212; that is, the programming language rules.</span></span>

<span data-ttu-id="1c451-241">Если вы являетесь опытным при программировании (особенно если вы использовали, C, C++, C#, Visual Basic или JavaScript), большая часть сведений здесь будут знакомы.</span><span class="sxs-lookup"><span data-stu-id="1c451-241">If you're experienced with programming (especially if you've used C, C++, C#, Visual Basic, or JavaScript), much of what you read here will be familiar.</span></span> <span data-ttu-id="1c451-242">Возможно, необходимо ознакомиться только с добавляется как серверный код разметки в *.cshtml* файлов.</span><span class="sxs-lookup"><span data-stu-id="1c451-242">You'll probably need to familiarize yourself only with how server code is added to markup in *.cshtml* files.</span></span>

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a><span data-ttu-id="1c451-243">Объединение текста, разметки и кода в блоках кода</span><span class="sxs-lookup"><span data-stu-id="1c451-243">Combining Text, Markup, and Code in Code Blocks</span></span>

<span data-ttu-id="1c451-244">В блоках кода сервера часто требуется вывод текста или разметки (или оба) на страницу.</span><span class="sxs-lookup"><span data-stu-id="1c451-244">In server code blocks, you often want to output text or markup (or both) to the page.</span></span> <span data-ttu-id="1c451-245">Если блок кода сервера содержит текст, не кода и, вместо этого должны отображаться как есть, ASP.NET необходимо иметь возможность различать этот текст из кода.</span><span class="sxs-lookup"><span data-stu-id="1c451-245">If a server code block contains text that's not code and that instead should be rendered as is, ASP.NET needs to be able to distinguish that text from code.</span></span> <span data-ttu-id="1c451-246">Для этого можно использовать следующие способы.</span><span class="sxs-lookup"><span data-stu-id="1c451-246">There are several ways to do this.</span></span>

- <span data-ttu-id="1c451-247">Заключите текст в HTML-элемента как `<p></p>` или `<em></em>`:</span><span class="sxs-lookup"><span data-stu-id="1c451-247">Enclose the text in an HTML element like `<p></p>` or `<em></em>`:</span></span>   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    <span data-ttu-id="1c451-248">HTML-элемента может включать текст, дополнительных элементов HTML и код сервера выражения.</span><span class="sxs-lookup"><span data-stu-id="1c451-248">The HTML element can include text, additional HTML elements, and server-code expressions.</span></span> <span data-ttu-id="1c451-249">Когда ASP.NET обнаруживает открывающий тег HTML (например, `<p>`), она отображает всего в том числе элемент и его содержимое, как является в браузер, в разрешении выражения кода сервера, он.</span><span class="sxs-lookup"><span data-stu-id="1c451-249">When ASP.NET sees the opening HTML tag (for example, `<p>`), it renders everything including the element and its content as is to the browser, resolving server-code expressions as it goes.</span></span>
- <span data-ttu-id="1c451-250">Используйте `@:` оператор или `<text>` элемент.</span><span class="sxs-lookup"><span data-stu-id="1c451-250">Use the `@:` operator or the `<text>` element.</span></span> <span data-ttu-id="1c451-251">`@:` Выводит одну строку содержимого, содержащего обычный текст или непарные теги HTML; `<text>` элемент заключает в себе несколько строк для вывода.</span><span class="sxs-lookup"><span data-stu-id="1c451-251">The `@:` outputs a single line of content containing plain text or unmatched HTML tags; the `<text>` element encloses multiple lines to output.</span></span> <span data-ttu-id="1c451-252">Эти параметры полезны в тех случаях, если не требуется для отрисовки HTML-элемента как часть выходных данных.</span><span class="sxs-lookup"><span data-stu-id="1c451-252">These options are useful when you don't want to render an HTML element as part of the output.</span></span>  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    <span data-ttu-id="1c451-253">Если нужно вывести несколько строк текста или непарные теги HTML, может предшествовать каждая строка начинается с `@:`, или можно заключить строку в `<text>` элемент.</span><span class="sxs-lookup"><span data-stu-id="1c451-253">If you want to output multiple lines of text or unmatched HTML tags, you can precede each line with `@:`, or you can enclose the line in a `<text>` element.</span></span> <span data-ttu-id="1c451-254">Как и `@:` оператор,`<text>` теги используются платформой ASP.NET для определения содержимого текста и никогда не отображаются в выходных данных страницы.</span><span class="sxs-lookup"><span data-stu-id="1c451-254">Like the `@:` operator,`<text>` tags are used by ASP.NET to identify text content and are never rendered in the page output.</span></span>

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    <span data-ttu-id="1c451-255">В первом примере повторяется предыдущий пример, но использует одну пару `<text>` тегов, чтобы заключить текст для отображения.</span><span class="sxs-lookup"><span data-stu-id="1c451-255">The first example repeats the previous example but uses a single pair of `<text>` tags to enclose the text to render.</span></span> <span data-ttu-id="1c451-256">Во втором примере `<text>` и `</text>` теги заключите три строки, каждая из которых имеет неавтономные текста и несовпадающие HTML-теги (`<br />`), а также серверный код и соответствующие теги HTML.</span><span class="sxs-lookup"><span data-stu-id="1c451-256">In the second example, the `<text>` and `</text>` tags enclose three lines, all of which have some uncontained text and unmatched HTML tags (`<br />`), along with server code and matched HTML tags.</span></span> <span data-ttu-id="1c451-257">Опять же, может предшествовать каждой строки по отдельности с помощью `@:` оператор; либо works способом.</span><span class="sxs-lookup"><span data-stu-id="1c451-257">Again, you could also precede each line individually with the `@:` operator; either way works.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1c451-258">При выводе текста как показано в этом разделе &#8212; с помощью HTML-элемент, `@:` оператора, или `<text>` элемент &#8212; ASP.NET не HTML-кодирование выходных данных.</span><span class="sxs-lookup"><span data-stu-id="1c451-258">When you output text as shown in this section &#8212; using an HTML element, the `@:` operator, or the `<text>` element &#8212; ASP.NET doesn't HTML-encode the output.</span></span> <span data-ttu-id="1c451-259">(Как уже отмечалось, ASP.NET кодировать выходные данные выражения кода сервера и блоков кода сервера, которым предшествует `@`, за исключением особых случаев, указанных в этом разделе.)</span><span class="sxs-lookup"><span data-stu-id="1c451-259">(As noted earlier, ASP.NET does encode the output of server code expressions and server code blocks that are preceded by `@`, except in the special cases noted in this section.)</span></span>

### <a name="whitespace"></a><span data-ttu-id="1c451-260">Whitespace</span><span class="sxs-lookup"><span data-stu-id="1c451-260">Whitespace</span></span>

<span data-ttu-id="1c451-261">Лишние пробелы в операторе (и за ее пределами строковым литералом) не влияют на инструкцию:</span><span class="sxs-lookup"><span data-stu-id="1c451-261">Extra spaces in a statement (and outside of a string literal) don't affect the statement:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

<span data-ttu-id="1c451-262">Разрыв строки в инструкции не оказывает влияния на инструкции, и можно включать инструкции для удобства чтения.</span><span class="sxs-lookup"><span data-stu-id="1c451-262">A line break in a statement has no effect on the statement, and you can wrap statements for readability.</span></span> <span data-ttu-id="1c451-263">Следующие инструкции равнозначны:</span><span class="sxs-lookup"><span data-stu-id="1c451-263">The following statements are the same:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

<span data-ttu-id="1c451-264">Тем не менее нельзя создать оболочку строки середине строкового литерала.</span><span class="sxs-lookup"><span data-stu-id="1c451-264">However, you can't wrap a line in the middle of a string literal.</span></span> <span data-ttu-id="1c451-265">Следующий пример не будет работать:</span><span class="sxs-lookup"><span data-stu-id="1c451-265">The following example doesn't work:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

<span data-ttu-id="1c451-266">Чтобы объединить длинную строку, которая переносится на несколько строк, такие как приведенный выше код, можно двумя способами.</span><span class="sxs-lookup"><span data-stu-id="1c451-266">To combine a long string that wraps to multiple lines like the above code, there are two options.</span></span> <span data-ttu-id="1c451-267">Можно использовать оператор объединения (`+`), который вы увидите далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="1c451-267">You can use the concatenation operator (`+`), which you'll see later in this article.</span></span> <span data-ttu-id="1c451-268">Можно также использовать `@` символ для создания буквального строкового литерала, как было показано ранее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="1c451-268">You can also use the `@` character to create a verbatim string literal, as you saw earlier in this article.</span></span> <span data-ttu-id="1c451-269">Буквальные строковые литералы могут располагаться в нескольких строках:</span><span class="sxs-lookup"><span data-stu-id="1c451-269">You can break verbatim string literals across lines:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a><span data-ttu-id="1c451-270">Код (и разметки) комментарии</span><span class="sxs-lookup"><span data-stu-id="1c451-270">Code (and Markup) Comments</span></span>

<span data-ttu-id="1c451-271">Комментарии можно оставлять заметки как для себя или другим пользователям.</span><span class="sxs-lookup"><span data-stu-id="1c451-271">Comments let you leave notes for yourself or others.</span></span> <span data-ttu-id="1c451-272">Они также позволяют отключать (*закомментируйте*) часть кода или разметку, вы не хотите запускать, но хотите сохранить на странице, на данный момент.</span><span class="sxs-lookup"><span data-stu-id="1c451-272">They also allow you to disable (*comment out*) a section of code or markup that you don't want to run but want to keep in your page for the time being.</span></span>

<span data-ttu-id="1c451-273">Существует отличается, комментирование синтаксиса Razor кода и разметки HTML.</span><span class="sxs-lookup"><span data-stu-id="1c451-273">There's different commenting syntax for Razor code and for HTML markup.</span></span> <span data-ttu-id="1c451-274">Как и в случае с любым кодом Razor, комментарии Razor обрабатываются (и затем удалить) на сервере перед отправкой страницы в браузер.</span><span class="sxs-lookup"><span data-stu-id="1c451-274">As with all Razor code, Razor comments are processed (and then removed) on the server before the page is sent to the browser.</span></span> <span data-ttu-id="1c451-275">Таким образом комментирования синтаксиса Razor позволяет поместить комментарии в код (или даже в разметку), которые можно использовать при редактировании файла, но пользователи не видят, даже в исходный код страницы.</span><span class="sxs-lookup"><span data-stu-id="1c451-275">Therefore, the Razor commenting syntax lets you put comments into the code (or even into the markup) that you can see when you edit the file, but that users don't see, even in the page source.</span></span>

<span data-ttu-id="1c451-276">Для комментариев ASP.NET Razor начать комментарий с `@*` и завершается оператором `*@`.</span><span class="sxs-lookup"><span data-stu-id="1c451-276">For ASP.NET Razor comments, you start the comment with `@*` and end it with `*@`.</span></span> <span data-ttu-id="1c451-277">Комментарий можно на одной или нескольких строках:</span><span class="sxs-lookup"><span data-stu-id="1c451-277">The comment can be on one line or multiple lines:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

<span data-ttu-id="1c451-278">Вот комментария в блок кода:</span><span class="sxs-lookup"><span data-stu-id="1c451-278">Here is a comment within a code block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

<span data-ttu-id="1c451-279">Здесь же блок кода, с помощью строки кода закомментирован, чтобы он не будет работать:</span><span class="sxs-lookup"><span data-stu-id="1c451-279">Here is the same block of code, with the line of code commented out so that it won't run:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

<span data-ttu-id="1c451-280">Внутри блока кода в качестве альтернативы с помощью синтаксиса комментариев Razor, можно использовать комментирования синтаксис языка программирования, которую вы используете, например C#:</span><span class="sxs-lookup"><span data-stu-id="1c451-280">Inside a code block, as an alternative to using Razor comment syntax, you can use the commenting syntax of the programming language you're using, such as C#:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

<span data-ttu-id="1c451-281">В C#, которым предшествует однострочные комментарии `//` символов, а многострочные комментарии начинаются с `/*` и заканчиваться `*/`.</span><span class="sxs-lookup"><span data-stu-id="1c451-281">In C#, single-line comments are preceded by the `//` characters, and multi-line comments begin with `/*` and end with `*/`.</span></span> <span data-ttu-id="1c451-282">(Как и в комментарии Razor, C# комментарии не подготавливаются к просмотру в браузере.)</span><span class="sxs-lookup"><span data-stu-id="1c451-282">(As with Razor comments, C# comments are not rendered to the browser.)</span></span>

<span data-ttu-id="1c451-283">Для разметки как вы, вероятно, знаете, можно создать комментарий HTML:</span><span class="sxs-lookup"><span data-stu-id="1c451-283">For markup, as you probably know, you can create an HTML comment:</span></span>

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

<span data-ttu-id="1c451-284">Комментарии HTML, начинаются с `<!--` символов и заканчиваются `-->`.</span><span class="sxs-lookup"><span data-stu-id="1c451-284">HTML comments start with `<!--` characters and end with `-->`.</span></span> <span data-ttu-id="1c451-285">Можно использовать комментарии HTML для окружения не только текст, но также разметку HTML, который вы хотите оставить на странице, но не хотите для подготовки к просмотру.</span><span class="sxs-lookup"><span data-stu-id="1c451-285">You can use HTML comments to surround not only text, but also any HTML markup that you may want to keep in the page but don't want to render.</span></span> <span data-ttu-id="1c451-286">Этот комментарий HTML будет скрыть все содержимое теги и текст, который они содержат:</span><span class="sxs-lookup"><span data-stu-id="1c451-286">This HTML comment will hide the entire content of the tags and the text they contain:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

<span data-ttu-id="1c451-287">В отличие от комментариев Razor, комментарии HTML *являются* отображен на странице и пользователь может видеть их путем просмотра исходного кода страницы.</span><span class="sxs-lookup"><span data-stu-id="1c451-287">Unlike Razor comments, HTML comments *are* rendered to the page and the user can see them by viewing the page source.</span></span>

<span data-ttu-id="1c451-288">Razor имеет ограничения на вложенные блоки C#.</span><span class="sxs-lookup"><span data-stu-id="1c451-288">Razor has limitations on nested blocks of C#.</span></span> <span data-ttu-id="1c451-289">Дополнительные сведения см. в разделе [переменных C# с именем и вложенные блоки создать неработающие код](http://aspnetwebstack.codeplex.com/workitem/1914)</span><span class="sxs-lookup"><span data-stu-id="1c451-289">For more information see [Named C# Variables and Nested Blocks Generate Broken Code](http://aspnetwebstack.codeplex.com/workitem/1914)</span></span>

## <a name="variables"></a><span data-ttu-id="1c451-290">Переменные</span><span class="sxs-lookup"><span data-stu-id="1c451-290">Variables</span></span>

<span data-ttu-id="1c451-291">Переменная является именованный объект, который используется для хранения данных.</span><span class="sxs-lookup"><span data-stu-id="1c451-291">A variable is a named object that you use to store data.</span></span> <span data-ttu-id="1c451-292">Вы можно назвать переменные, но имя должно начинаться с буквы и не может содержать пробелы или зарезервированные символы.</span><span class="sxs-lookup"><span data-stu-id="1c451-292">You can name variables anything, but the name must begin with an alphabetic character and it cannot contain whitespace or reserved characters.</span></span>

### <a name="variables-and-data-types"></a><span data-ttu-id="1c451-293">Переменные и типы данных</span><span class="sxs-lookup"><span data-stu-id="1c451-293">Variables and Data Types</span></span>

<span data-ttu-id="1c451-294">Переменная может иметь определенный тип данных, который указывает, какие данные хранятся в переменной.</span><span class="sxs-lookup"><span data-stu-id="1c451-294">A variable can have a specific data type, which indicates what kind of data is stored in the variable.</span></span> <span data-ttu-id="1c451-295">У вас есть строковых переменных, в которых хранятся строковые значения (например &quot;Здравствуй, мир&quot;), целочисленные переменные, которые сохраняют значения целого числа (например, 3 или 79) и переменные date, в которых хранятся значения даты в различных форматах (например, 4/12/2012 или марта 2009 г. ).</span><span class="sxs-lookup"><span data-stu-id="1c451-295">You can have string variables that store string values (like &quot;Hello world&quot;), integer variables that store whole-number values (like 3 or 79), and date variables that store date values in a variety of formats (like 4/12/2012 or March 2009).</span></span> <span data-ttu-id="1c451-296">И многие другие типы данных, которые можно использовать.</span><span class="sxs-lookup"><span data-stu-id="1c451-296">And there are many other data types you can use.</span></span>

<span data-ttu-id="1c451-297">Тем не менее обычно не нужно указывать тип переменной.</span><span class="sxs-lookup"><span data-stu-id="1c451-297">However, you generally don't have to specify a type for a variable.</span></span> <span data-ttu-id="1c451-298">В большинстве случаев, ASP.NET может определить тип, в зависимости от того, как используются данные в переменной.</span><span class="sxs-lookup"><span data-stu-id="1c451-298">Most of the time, ASP.NET can figure out the type based on how the data in the variable is being used.</span></span> <span data-ttu-id="1c451-299">(Иногда необходимо указать тип, вы увидите примеры, когда это значение равно true.)</span><span class="sxs-lookup"><span data-stu-id="1c451-299">(Occasionally you must specify a type; you'll see examples where this is true.)</span></span>

<span data-ttu-id="1c451-300">Объявите переменную с помощью `var` ключевое слово (Если вы не хотите указывать тип) или с помощью имени типа:</span><span class="sxs-lookup"><span data-stu-id="1c451-300">You declare a variable using the `var` keyword (if you don't want to specify a type) or by using the name of the type:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

<span data-ttu-id="1c451-301">В следующем примере показано некоторые типичные способы применения переменных на веб-странице:</span><span class="sxs-lookup"><span data-stu-id="1c451-301">The following example shows some typical uses of variables in a web page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

<span data-ttu-id="1c451-302">Если объединить предыдущих примерах на странице, вы увидите, это отображается в браузере:</span><span class="sxs-lookup"><span data-stu-id="1c451-302">If you combine the previous examples in a page, you see this displayed in a browser:</span></span>

![Razor Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a><span data-ttu-id="1c451-304">Преобразование и тестирование типы данных</span><span class="sxs-lookup"><span data-stu-id="1c451-304">Converting and Testing Data Types</span></span>

<span data-ttu-id="1c451-305">Несмотря на то, что ASP.NET обычно можно автоматически определить тип данных, иногда это невозможно.</span><span class="sxs-lookup"><span data-stu-id="1c451-305">Although ASP.NET can usually determine a data type automatically, sometimes it can't.</span></span> <span data-ttu-id="1c451-306">Таким образом может потребоваться помощь ASP.NET, выполнив явное преобразование.</span><span class="sxs-lookup"><span data-stu-id="1c451-306">Therefore, you might need to help ASP.NET out by performing an explicit conversion.</span></span> <span data-ttu-id="1c451-307">Даже если у вас нет для преобразования типов, иногда бывает полезно проверить, какие типы данных вы можете работать с.</span><span class="sxs-lookup"><span data-stu-id="1c451-307">Even if you don't have to convert types, sometimes it's helpful to test to see what type of data you might be working with.</span></span>

<span data-ttu-id="1c451-308">Наиболее распространенным случаем является необходимость преобразования строки в другой тип, такой как целое число или дату.</span><span class="sxs-lookup"><span data-stu-id="1c451-308">The most common case is that you have to convert a string to another type, such as to an integer or date.</span></span> <span data-ttu-id="1c451-309">В примере показан типичный случай, где необходимо преобразовать строку в число.</span><span class="sxs-lookup"><span data-stu-id="1c451-309">The following example shows a typical case where you must convert a string to a number.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

<span data-ttu-id="1c451-310">Как правило входные данные пользователя будут доставляться вам как строки.</span><span class="sxs-lookup"><span data-stu-id="1c451-310">As a rule, user input comes to you as strings.</span></span> <span data-ttu-id="1c451-311">Даже если предлагается пользователю ввести число, и даже если они вводят цифры, при отправке ввод данных пользователем и прочитать его в код, данные в строковом формате.</span><span class="sxs-lookup"><span data-stu-id="1c451-311">Even if you've prompted users to enter a number, and even if they've entered a digit, when user input is submitted and you read it in code, the data is in string format.</span></span> <span data-ttu-id="1c451-312">Таким образом необходимо преобразовать строку в число.</span><span class="sxs-lookup"><span data-stu-id="1c451-312">Therefore, you must convert the string to a number.</span></span> <span data-ttu-id="1c451-313">В примере при попытке выполнить арифметические операции над значениями без преобразования их, возникает следующая ошибка, так как ASP.NET не удается добавить две строки:</span><span class="sxs-lookup"><span data-stu-id="1c451-313">In the example, if you try to perform arithmetic on the values without converting them, the following error results, because ASP.NET cannot add two strings:</span></span>

<span data-ttu-id="1c451-314">*Не удается неявно преобразовать тип «string» для «int».*</span><span class="sxs-lookup"><span data-stu-id="1c451-314">*Cannot implicitly convert type 'string' to 'int'.*</span></span>

<span data-ttu-id="1c451-315">Для преобразования значений в целые числа, следует вызвать `AsInt` метод.</span><span class="sxs-lookup"><span data-stu-id="1c451-315">To convert the values to integers, you call the `AsInt` method.</span></span> <span data-ttu-id="1c451-316">Если преобразование прошло успешно, затем можно добавить номера.</span><span class="sxs-lookup"><span data-stu-id="1c451-316">If the conversion is successful, you can then add the numbers.</span></span>

<span data-ttu-id="1c451-317">Ниже перечислены некоторые распространенные методы преобразования и тестирования для переменных.</span><span class="sxs-lookup"><span data-stu-id="1c451-317">The following table lists some common conversion and test methods for variables.</span></span>

:::row:::
    :::column:::
    <span data-ttu-id="1c451-318"><strong>Метод</strong></span><span class="sxs-lookup"><span data-stu-id="1c451-318"><strong>Method</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="1c451-319"><strong>Описание</strong></span><span class="sxs-lookup"><span data-stu-id="1c451-319"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="1c451-320"><strong>Пример</strong></span><span class="sxs-lookup"><span data-stu-id="1c451-320"><strong>Example</strong></span></span>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="1c451-321">Преобразует строку, которая представляет целое число (например, «593)» в целое число.</span><span class="sxs-lookup"><span data-stu-id="1c451-321">Converts a string that represents a whole number (like "593") to an integer.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsBool(), IsBool()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="1c451-322">Преобразует строку как &quot;true&quot; или &quot;false&quot; к логическому типу.</span><span class="sxs-lookup"><span data-stu-id="1c451-322">Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsFloat(), IsFloat()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="1c451-323">Преобразует строку, которая имеет значение десятичного числа, например &quot;1.3&quot; или &quot;7.439&quot; число с плавающей запятой.</span><span class="sxs-lookup"><span data-stu-id="1c451-323">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="1c451-324">Преобразует строку, которая имеет значение десятичного числа, например &quot;1.3&quot; или &quot;7.439&quot; в десятичное число.</span><span class="sxs-lookup"><span data-stu-id="1c451-324">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number.</span></span> <span data-ttu-id="1c451-325">(В ASP.NET, десятичное число является более точным, чем число с плавающей запятой.)</span><span class="sxs-lookup"><span data-stu-id="1c451-325">(In ASP.NET, a decimal number is more precise than a floating-point number.)</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDateTime(), IsDateTime()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="1c451-326">Преобразует строку, представляющую значение даты и времени для ASP.NET `DateTime` типа.</span><span class="sxs-lookup"><span data-stu-id="1c451-326">Converts a string that represents a date and time value to the ASP.NET `DateTime` type.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `ToString()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="1c451-327">Любой другой тип данных преобразуется в строку.</span><span class="sxs-lookup"><span data-stu-id="1c451-327">Converts any other data type to a string.</span></span>
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a><span data-ttu-id="1c451-328">Операторы</span><span class="sxs-lookup"><span data-stu-id="1c451-328">Operators</span></span>

<span data-ttu-id="1c451-329">Оператор — это ключевое слово или символ, который сообщает ASP.NET, какого рода команду, выполняемую в выражении.</span><span class="sxs-lookup"><span data-stu-id="1c451-329">An operator is a keyword or character that tells ASP.NET what kind of command to perform in an expression.</span></span> <span data-ttu-id="1c451-330">В языке C# (и синтаксиса Razor на его основе) поддерживает многие операторы, но требуется только для распознавания немного, чтобы приступить к работе.</span><span class="sxs-lookup"><span data-stu-id="1c451-330">The C# language (and the Razor syntax that's based on it) supports many operators, but you only need to recognize a few to get started.</span></span> <span data-ttu-id="1c451-331">В следующей таблице перечислены наиболее распространенные операторы.</span><span class="sxs-lookup"><span data-stu-id="1c451-331">The following table summarizes the most common operators.</span></span>


:::row:::
    :::column:::
    <span data-ttu-id="1c451-332"><strong>Operator</strong></span><span class="sxs-lookup"><span data-stu-id="1c451-332"><strong>Operator</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="1c451-333"><strong>Описание</strong></span><span class="sxs-lookup"><span data-stu-id="1c451-333"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="1c451-334"><strong>Примеры</strong></span><span class="sxs-lookup"><span data-stu-id="1c451-334"><strong>Examples</strong></span></span>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+` `-` `*` `/`
    :::column-end:::
    :::column:::
    <span data-ttu-id="1c451-335">Математические операторы, используемые в числовых выражений.</span><span class="sxs-lookup"><span data-stu-id="1c451-335">Math operators used in numerical expressions.</span></span>
    :::column-end:::
    :::column:::
        [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
    <span data-ttu-id="1c451-336">Присваивание.</span><span class="sxs-lookup"><span data-stu-id="1c451-336">Assignment.</span></span> <span data-ttu-id="1c451-337">Присваивает значение правой стороны оператора объекту с левой стороны.</span><span class="sxs-lookup"><span data-stu-id="1c451-337">Assigns the value on the right side of a statement to the object on the left side.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `==`
    :::column-end:::
    :::column:::
    <span data-ttu-id="1c451-338">Равенство.</span><span class="sxs-lookup"><span data-stu-id="1c451-338">Equality.</span></span> <span data-ttu-id="1c451-339">Возвращает `true` Если значения равны.</span><span class="sxs-lookup"><span data-stu-id="1c451-339">Returns `true` if the values are equal.</span></span> <span data-ttu-id="1c451-340">(Обратите внимание, что разница между `=` оператор и `==` оператор.)</span><span class="sxs-lookup"><span data-stu-id="1c451-340">(Notice the distinction between the `=` operator and the `==` operator.)</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `!=`
    :::column-end:::
    :::column:::
    <span data-ttu-id="1c451-341">Неравенство.</span><span class="sxs-lookup"><span data-stu-id="1c451-341">Inequality.</span></span> <span data-ttu-id="1c451-342">Возвращает `true` Если значения не равны.</span><span class="sxs-lookup"><span data-stu-id="1c451-342">Returns `true` if the values are not equal.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
    <span data-ttu-id="1c451-343">Меньше-чем», «больше — чем меньше или равно и больше или равно.</span><span class="sxs-lookup"><span data-stu-id="1c451-343">Less-than, greater-than, less-than-or-equal, and greater-than-or-equal.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+`
    :::column-end:::
    :::column:::
    <span data-ttu-id="1c451-344">Объединение, который используется для объединения строк.</span><span class="sxs-lookup"><span data-stu-id="1c451-344">Concatenation, which is used to join strings.</span></span> <span data-ttu-id="1c451-345">ASP.NET знает разницу между этот оператор и оператор сложения, исходя из типа данных выражения.</span><span class="sxs-lookup"><span data-stu-id="1c451-345">ASP.NET knows the difference between this operator and the addition operator based on the data type of the expression.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+=` `-=`
    :::column-end:::
    :::column:::
    <span data-ttu-id="1c451-346">Операторы инкремента и декремента, которые сложения и вычитания 1 (соответственно) из переменной.</span><span class="sxs-lookup"><span data-stu-id="1c451-346">The increment and decrement operators, which add and subtract 1 (respectively) from a variable.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `.`
    :::column-end:::
    :::column:::
    <span data-ttu-id="1c451-347">Точка.</span><span class="sxs-lookup"><span data-stu-id="1c451-347">Dot.</span></span> <span data-ttu-id="1c451-348">Используются для различения объектов и их свойства и методы.</span><span class="sxs-lookup"><span data-stu-id="1c451-348">Used to distinguish objects and their properties and methods.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="1c451-349">Круглые скобки.</span><span class="sxs-lookup"><span data-stu-id="1c451-349">Parentheses.</span></span> <span data-ttu-id="1c451-350">Используется для группирования выражений и для передачи параметров в методы.</span><span class="sxs-lookup"><span data-stu-id="1c451-350">Used to group expressions and to pass parameters to methods.</span></span>
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `[]`
    :::column-end:::
    :::column:::
    <span data-ttu-id="1c451-351">Квадратные скобки.</span><span class="sxs-lookup"><span data-stu-id="1c451-351">Brackets.</span></span> <span data-ttu-id="1c451-352">Используется для доступа к значениям в массивы или коллекции.</span><span class="sxs-lookup"><span data-stu-id="1c451-352">Used for accessing values in arrays or collections.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `!`
    :::column-end:::
    :::column:::
    <span data-ttu-id="1c451-353">Нет.</span><span class="sxs-lookup"><span data-stu-id="1c451-353">Not.</span></span> <span data-ttu-id="1c451-354">Обращает `true` значение `false` и наоборот.</span><span class="sxs-lookup"><span data-stu-id="1c451-354">Reverses a `true` value to `false` and vice versa.</span></span> <span data-ttu-id="1c451-355">Обычно используется в качестве быстрым способом для проверки `false` (то есть для не `true`).</span><span class="sxs-lookup"><span data-stu-id="1c451-355">Typically used as a shorthand way to test for `false` (that is, for not `true`).</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `&&` <code>&#124;&#124;</code>
    :::column-end:::
    :::column:::
    <span data-ttu-id="1c451-356">Логическое и и или, которые используются для связывания условий.</span><span class="sxs-lookup"><span data-stu-id="1c451-356">Logical AND and OR, which are used to link conditions together.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]
    :::column-end:::
:::row-end:::

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a><span data-ttu-id="1c451-357">Работа с файлом и пути к папкам в коде</span><span class="sxs-lookup"><span data-stu-id="1c451-357">Working with File and Folder Paths in Code</span></span>

<span data-ttu-id="1c451-358">Вы будете часто работать пути файлов и папок в коде.</span><span class="sxs-lookup"><span data-stu-id="1c451-358">You'll often work with file and folder paths in your code.</span></span> <span data-ttu-id="1c451-359">Ниже приведен пример структуры физическую папку для веб-сайта, которое может отображаться на компьютере разработчика:</span><span class="sxs-lookup"><span data-stu-id="1c451-359">Here is an example of physical folder structure for a website as it might appear on your development computer:</span></span>

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

<span data-ttu-id="1c451-360">Ниже приведены некоторые важные сведения о URL-адреса и пути.</span><span class="sxs-lookup"><span data-stu-id="1c451-360">Here are some essential details about URLs and paths:</span></span>

- <span data-ttu-id="1c451-361">URL-адрес начинается с любым именем домена (`http://www.example.com`) или имя сервера (`http://localhost`, `http://mycomputer`).</span><span class="sxs-lookup"><span data-stu-id="1c451-361">A URL begins with either a domain name (`http://www.example.com`) or a server name (`http://localhost`, `http://mycomputer`).</span></span>
- <span data-ttu-id="1c451-362">URL-адрес соответствует физический путь на главном компьютере.</span><span class="sxs-lookup"><span data-stu-id="1c451-362">A URL corresponds to a physical path on a host computer.</span></span> <span data-ttu-id="1c451-363">Например `http://myserver` может соответствовать папке *C:\websites\mywebsite* на сервере.</span><span class="sxs-lookup"><span data-stu-id="1c451-363">For example, `http://myserver` might correspond to the folder *C:\websites\mywebsite* on the server.</span></span>
- <span data-ttu-id="1c451-364">Виртуальный путь является сокращением для представления путей в коде без указания полного пути.</span><span class="sxs-lookup"><span data-stu-id="1c451-364">A virtual path is shorthand to represent paths in code without having to specify the full path.</span></span> <span data-ttu-id="1c451-365">Он включает в себя часть URL-адрес, который следует за именем домена или сервера.</span><span class="sxs-lookup"><span data-stu-id="1c451-365">It includes the portion of a URL that follows the domain or server name.</span></span> <span data-ttu-id="1c451-366">При использовании виртуальных путей, можно перенести код в другой домен или сервер без необходимости обновите пути.</span><span class="sxs-lookup"><span data-stu-id="1c451-366">When you use virtual paths, you can move your code to a different domain or server without having to update the paths.</span></span>

<span data-ttu-id="1c451-367">Ниже приведен пример, чтобы помочь вам понять различия:</span><span class="sxs-lookup"><span data-stu-id="1c451-367">Here's an example to help you understand the differences:</span></span>

| <span data-ttu-id="1c451-368">Полный URL-адрес</span><span class="sxs-lookup"><span data-stu-id="1c451-368">Complete URL</span></span> | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| <span data-ttu-id="1c451-369">Имя сервера</span><span class="sxs-lookup"><span data-stu-id="1c451-369">Server name</span></span> | <span data-ttu-id="1c451-370">*mycompanyserver*</span><span class="sxs-lookup"><span data-stu-id="1c451-370">*mycompanyserver*</span></span> |
| <span data-ttu-id="1c451-371">Виртуальный путь</span><span class="sxs-lookup"><span data-stu-id="1c451-371">Virtual path</span></span> | <span data-ttu-id="1c451-372">*/humanresources/CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="1c451-372">*/humanresources/CompanyPolicy.htm*</span></span> |
| <span data-ttu-id="1c451-373">Физический путь</span><span class="sxs-lookup"><span data-stu-id="1c451-373">Physical path</span></span> | <span data-ttu-id="1c451-374">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="1c451-374">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span></span> |

<span data-ttu-id="1c451-375">Виртуальный корневой каталог — /, так же, как корень диска c диск — \.</span><span class="sxs-lookup"><span data-stu-id="1c451-375">The virtual root is /, just like the root of your C: drive is \.</span></span> <span data-ttu-id="1c451-376">(Виртуальная папка пути всегда использовать символы косой черты). Виртуальный путь к папке не нужно иметь то же имя как физический каталог; Это может быть псевдонимом.</span><span class="sxs-lookup"><span data-stu-id="1c451-376">(Virtual folder paths always use forward slashes.) The virtual path of a folder doesn't have to have the same name as the physical folder; it can be an alias.</span></span> <span data-ttu-id="1c451-377">(На рабочих серверах, виртуальный путь редко соответствует как полный физический путь.)</span><span class="sxs-lookup"><span data-stu-id="1c451-377">(On production servers, the virtual path rarely matches an exact physical path.)</span></span>

<span data-ttu-id="1c451-378">При работе с файлами и папками в коде, иногда требуется для ссылки на физический путь и иногда виртуального пути, в зависимости от того, какие объекты, которыми вы работаете.</span><span class="sxs-lookup"><span data-stu-id="1c451-378">When you work with files and folders in code, sometimes you need to reference the physical path and sometimes a virtual path, depending on what objects you're working with.</span></span> <span data-ttu-id="1c451-379">ASP.NET предоставляет эти средства для работы с пути файлов и папок в коде: `Server.MapPath` метод и `~` оператор и `Href` метод.</span><span class="sxs-lookup"><span data-stu-id="1c451-379">ASP.NET gives you these tools for working with file and folder paths in code: the `Server.MapPath` method, and the `~` operator and `Href` method.</span></span>

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a><span data-ttu-id="1c451-380">Преобразование виртуальных и физических путей: метод Server.MapPath</span><span class="sxs-lookup"><span data-stu-id="1c451-380">Converting virtual to physical paths: the Server.MapPath method</span></span>

<span data-ttu-id="1c451-381">`Server.MapPath` Метод преобразует виртуальный путь (например */default.cshtml*) в абсолютный физический путь (например *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="1c451-381">The `Server.MapPath` method converts a virtual path (like */default.cshtml*) to an absolute physical path (like *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span></span> <span data-ttu-id="1c451-382">Этот метод использовать когда требуется полный физический путь.</span><span class="sxs-lookup"><span data-stu-id="1c451-382">You use this method any time you need a complete physical path.</span></span> <span data-ttu-id="1c451-383">Типичный пример — при чтении или записи в текстовый файл или файл изображения на веб-сервере.</span><span class="sxs-lookup"><span data-stu-id="1c451-383">A typical example is when you're reading or writing a text file or image file on the web server.</span></span>

<span data-ttu-id="1c451-384">Абсолютный физический путь веб-сайта на сервере размещения сайта обычно не известен, поэтому этот метод можно преобразовать путь вы знаете, виртуальный путь, соответствующий путь на сервере для вас.</span><span class="sxs-lookup"><span data-stu-id="1c451-384">You typically don't know the absolute physical path of your site on a hosting site's server, so this method can convert the path you do know — the virtual path — to the corresponding path on the server for you.</span></span> <span data-ttu-id="1c451-385">Передайте виртуальный путь к файлу или папке в метод и возвращает физический путь:</span><span class="sxs-lookup"><span data-stu-id="1c451-385">You pass the virtual path to a file or folder to the method, and it returns the physical path:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a><span data-ttu-id="1c451-386">Ссылка на виртуальный корневой каталог: ~ оператор и метод Href</span><span class="sxs-lookup"><span data-stu-id="1c451-386">Referencing the virtual root: the ~ operator and Href method</span></span>

<span data-ttu-id="1c451-387">В *.cshtml* или *.vbhtml* файл, вы можете ссылаться на пути виртуального корневого каталога, используя `~` оператор.</span><span class="sxs-lookup"><span data-stu-id="1c451-387">In a *.cshtml* or *.vbhtml* file, you can reference the virtual root path using the `~` operator.</span></span> <span data-ttu-id="1c451-388">Это очень удобно, так как для перемещения страниц на узле и все ссылки, содержащиеся в них на другие страницы не будет нарушена.</span><span class="sxs-lookup"><span data-stu-id="1c451-388">This is very handy because you can move pages around in a site, and any links they contain to other pages won't be broken.</span></span> <span data-ttu-id="1c451-389">Это также удобно в случае, если когда-либо переместить веб-сайта в другое расположение.</span><span class="sxs-lookup"><span data-stu-id="1c451-389">It's also handy in case you ever move your website to a different location.</span></span> <span data-ttu-id="1c451-390">Далее приводятся некоторые примеры.</span><span class="sxs-lookup"><span data-stu-id="1c451-390">Here are some examples:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

<span data-ttu-id="1c451-391">Если веб-сайт `http://myserver/myapp`, вот как ASP.NET будет интерпретировать эти пути, при запуске страницы:</span><span class="sxs-lookup"><span data-stu-id="1c451-391">If the website is `http://myserver/myapp`, here's how ASP.NET will treat these paths when the page runs:</span></span>

- <span data-ttu-id="1c451-392">`myImagesFolder`: `http://myserver/myapp/images`</span><span class="sxs-lookup"><span data-stu-id="1c451-392">`myImagesFolder`: `http://myserver/myapp/images`</span></span>
- <span data-ttu-id="1c451-393">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span><span class="sxs-lookup"><span data-stu-id="1c451-393">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span></span>

<span data-ttu-id="1c451-394">(Вы не увидите эти пути в качестве значения переменной, но ASP.NET будет рассматривать пути, как в случае, они были).</span><span class="sxs-lookup"><span data-stu-id="1c451-394">(You won't actually see these paths as the values of the variable, but ASP.NET will treat the paths as if that's what they were.)</span></span>

<span data-ttu-id="1c451-395">Можно использовать `~` оператор, как в серверном коде (как описано выше), так и в разметке, следующим образом:</span><span class="sxs-lookup"><span data-stu-id="1c451-395">You can use the `~` operator both in server code (as above) and in markup, like this:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

<span data-ttu-id="1c451-396">В разметке, использовании `~` оператор для создания путей к ресурсам, например файлы изображений, другие веб-страниц и файлов CSS.</span><span class="sxs-lookup"><span data-stu-id="1c451-396">In markup, you use the `~` operator to create paths to resources like image files, other web pages, and CSS files.</span></span> <span data-ttu-id="1c451-397">При запуске страницы ASP.NET, просматривает страницы (код и разметку) и разрешает все `~` ссылки на соответствующий путь.</span><span class="sxs-lookup"><span data-stu-id="1c451-397">When the page runs, ASP.NET looks through the page (both code and markup) and resolves all the `~` references to the appropriate path.</span></span>

## <a name="conditional-logic-and-loops"></a><span data-ttu-id="1c451-398">Условной логики и циклов</span><span class="sxs-lookup"><span data-stu-id="1c451-398">Conditional Logic and Loops</span></span>

<span data-ttu-id="1c451-399">Код сервера ASP.NET, которая позволяет выполнять задачи на основе условий и написать код, который повторяется инструкций определенное число раз (то есть код, который запускает цикл).</span><span class="sxs-lookup"><span data-stu-id="1c451-399">ASP.NET server code lets you perform tasks based on conditions and write code that repeats statements a specific number of times (that is, code that runs a loop).</span></span>

### <a name="testing-conditions"></a><span data-ttu-id="1c451-400">Проверка условий</span><span class="sxs-lookup"><span data-stu-id="1c451-400">Testing Conditions</span></span>

<span data-ttu-id="1c451-401">Для тестирования на выполнение простого условия можно использовать `if` инструкцию, которая возвращает true или false, основываясь на тест, можно указать:</span><span class="sxs-lookup"><span data-stu-id="1c451-401">To test a simple condition you use the `if` statement, which returns true or false based on a test you specify:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

<span data-ttu-id="1c451-402">`if` Ключевое слово начинается блок.</span><span class="sxs-lookup"><span data-stu-id="1c451-402">The `if` keyword starts a block.</span></span> <span data-ttu-id="1c451-403">Тест этот сам (условие) указан в скобках и возвращает значение true или false.</span><span class="sxs-lookup"><span data-stu-id="1c451-403">The actual test (condition) is in parentheses and returns true or false.</span></span> <span data-ttu-id="1c451-404">Операторы под управлением, если тест имеет значение true, заключенных в фигурные скобки.</span><span class="sxs-lookup"><span data-stu-id="1c451-404">The statements that run if the test is true are enclosed in braces.</span></span> <span data-ttu-id="1c451-405">`if` Инструкция может содержать `else` блок, который задает операторы, которые выполняются, если условие имеет значение false:</span><span class="sxs-lookup"><span data-stu-id="1c451-405">An `if` statement can include an `else` block that specifies statements to run if the condition is false:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

<span data-ttu-id="1c451-406">Можно добавить несколько условий с помощью `else if` блок:</span><span class="sxs-lookup"><span data-stu-id="1c451-406">You can add multiple conditions using an `else if` block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

<span data-ttu-id="1c451-407">В этом примере, если первое условие в Если блок не имеет значение true, `else if` проверяется условие.</span><span class="sxs-lookup"><span data-stu-id="1c451-407">In this example, if the first condition in the if block is not true, the `else if` condition is checked.</span></span> <span data-ttu-id="1c451-408">Если это условие выполнено, операторы в `else if` блоке.</span><span class="sxs-lookup"><span data-stu-id="1c451-408">If that condition is met, the statements in the `else if` block are executed.</span></span> <span data-ttu-id="1c451-409">Если ни одно из условий не соблюдается, операторы в `else` блоке.</span><span class="sxs-lookup"><span data-stu-id="1c451-409">If none of the conditions are met, the statements in the `else` block are executed.</span></span> <span data-ttu-id="1c451-410">Можно добавить любое количество иначе Если блокируется, а затем закройте с `else` заблокировать, в виде &quot;все остальное&quot; условие.</span><span class="sxs-lookup"><span data-stu-id="1c451-410">You can add any number of else if blocks, and then close with an `else` block as the &quot;everything else&quot; condition.</span></span>

<span data-ttu-id="1c451-411">Чтобы протестировать большое количество условий, используйте `switch` блок:</span><span class="sxs-lookup"><span data-stu-id="1c451-411">To test a large number of conditions, use a `switch` block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

<span data-ttu-id="1c451-412">Значение для проверки указан в скобках (в примере `weekday` переменной).</span><span class="sxs-lookup"><span data-stu-id="1c451-412">The value to test is in parentheses (in the example, the `weekday` variable).</span></span> <span data-ttu-id="1c451-413">Каждого отдельного теста использует `case` инструкцию, которая заканчивается двоеточием (:).</span><span class="sxs-lookup"><span data-stu-id="1c451-413">Each individual test uses a `case` statement that ends with a colon (:).</span></span> <span data-ttu-id="1c451-414">Если значение `case` инструкции совпадает со значением теста, выполнении кода в соответствующем блоке case.</span><span class="sxs-lookup"><span data-stu-id="1c451-414">If the value of a `case` statement matches the test value, the code in that case block is executed.</span></span> <span data-ttu-id="1c451-415">Закройте каждая инструкция case с `break` инструкции.</span><span class="sxs-lookup"><span data-stu-id="1c451-415">You close each case statement with a `break` statement.</span></span> <span data-ttu-id="1c451-416">(Если вы забудете включить разрыв в каждом `case` block, код из следующего `case` инструкция выполнится также.) Объект `switch` блок часто имеет `default` инструкцию как в случае с последнего &quot;все остальное&quot; вариант, который выполняется, если ни одно из остальных случаях.</span><span class="sxs-lookup"><span data-stu-id="1c451-416">(If you forget to include break in each `case` block, the code from the next `case` statement will run also.) A `switch` block often has a `default` statement as the last case for an &quot;everything else&quot; option that runs if none of the other cases are true.</span></span>

<span data-ttu-id="1c451-417">Результат выполнения двух последних блоки условий, отображаемый в браузере:</span><span class="sxs-lookup"><span data-stu-id="1c451-417">The result of the last two conditional blocks displayed in a browser:</span></span>

![Razor Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a><span data-ttu-id="1c451-419">Циклическое кода</span><span class="sxs-lookup"><span data-stu-id="1c451-419">Looping Code</span></span>

<span data-ttu-id="1c451-420">Часто требуется многократно выполнять те же операторы.</span><span class="sxs-lookup"><span data-stu-id="1c451-420">You often need to run the same statements repeatedly.</span></span> <span data-ttu-id="1c451-421">Для этого цикла.</span><span class="sxs-lookup"><span data-stu-id="1c451-421">You do this by looping.</span></span> <span data-ttu-id="1c451-422">Например вы часто выполняются те же операторы для каждого элемента в коллекции данных.</span><span class="sxs-lookup"><span data-stu-id="1c451-422">For example, you often run the same statements for each item in a collection of data.</span></span> <span data-ttu-id="1c451-423">Если вы знаете, точно того, как часто необходимо применить цикл, можно использовать `for` цикла.</span><span class="sxs-lookup"><span data-stu-id="1c451-423">If you know exactly how many times you want to loop, you can use a `for` loop.</span></span> <span data-ttu-id="1c451-424">Этот тип цикла удобен для подсчета или отсчет:</span><span class="sxs-lookup"><span data-stu-id="1c451-424">This kind of loop is especially useful for counting up or counting down:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

<span data-ttu-id="1c451-425">Цикл начинается с `for` ключевое слово, и три оператора в круглые скобки, каждый заканчиваться точкой с запятой.</span><span class="sxs-lookup"><span data-stu-id="1c451-425">The loop begins with the `for` keyword, followed by three statements in parentheses, each terminated with a semicolon.</span></span>

- <span data-ttu-id="1c451-426">В скобках, первый оператор (`var i=10;`) создает счетчик и инициализирует его до 10.</span><span class="sxs-lookup"><span data-stu-id="1c451-426">Inside the parentheses, the first statement (`var i=10;`) creates a counter and initializes it to 10.</span></span> <span data-ttu-id="1c451-427">Вы не обязаны именовать счетчика `i` &#8212; можно использовать любую переменную.</span><span class="sxs-lookup"><span data-stu-id="1c451-427">You don't have to name the counter `i` &#8212; you can use any variable.</span></span> <span data-ttu-id="1c451-428">Когда `for` выполнении цикла, этот счетчик увеличивается автоматически.</span><span class="sxs-lookup"><span data-stu-id="1c451-428">When the `for` loop runs, the counter is automatically incremented.</span></span>
- <span data-ttu-id="1c451-429">Второй оператор (`i < 21;`) задает условие для насколько вам требуется подсчитать.</span><span class="sxs-lookup"><span data-stu-id="1c451-429">The second statement (`i < 21;`) sets the condition for how far you want to count.</span></span> <span data-ttu-id="1c451-430">В этом случае необходимо его, чтобы перейти к более 20 (то есть, продолжайте работу при счетчика меньше 21).</span><span class="sxs-lookup"><span data-stu-id="1c451-430">In this case, you want it to go to a maximum of 20 (that is, keep going while the counter is less than 21).</span></span>
- <span data-ttu-id="1c451-431">Третья инструкция (`i++` ) использует оператор приращения, который просто указывает, что счетчик должен иметь 1 добавляется при каждом выполнении цикла.</span><span class="sxs-lookup"><span data-stu-id="1c451-431">The third statement (`i++` ) uses an increment operator, which simply specifies that the counter should have 1 added to it each time the loop runs.</span></span>

<span data-ttu-id="1c451-432">В скобках приведен код, который будет выполняться для каждой итерации цикла.</span><span class="sxs-lookup"><span data-stu-id="1c451-432">Inside the braces is the code that will run for each iteration of the loop.</span></span> <span data-ttu-id="1c451-433">Разметка создает новый абзац (`<p>` элемент) каждый раз и добавляет строку в выходные данные, отображают значение `i` (счетчик).</span><span class="sxs-lookup"><span data-stu-id="1c451-433">The markup creates a new paragraph (`<p>` element) each time and adds a line to the output, displaying the value of `i` (the counter).</span></span> <span data-ttu-id="1c451-434">Когда вы запускаете эту страницу, в примере создается 11 строк, отображение выходных данных, с текстом в каждой строке, указывающее номер элемента.</span><span class="sxs-lookup"><span data-stu-id="1c451-434">When you run this page, the example creates 11 lines displaying the output, with the text in each line indicating the item number.</span></span>

![Razor Img11](introducing-razor-syntax-c/_static/image11.jpg)

<span data-ttu-id="1c451-436">Если вы работаете в коллекции или массива, часто используют `foreach` цикла.</span><span class="sxs-lookup"><span data-stu-id="1c451-436">If you're working with a collection or array, you often use a `foreach` loop.</span></span> <span data-ttu-id="1c451-437">Коллекция — это группа схожих объектов и `foreach` цикле позволяет выполнить задачу для каждого элемента в коллекции.</span><span class="sxs-lookup"><span data-stu-id="1c451-437">A collection is a group of similar objects, and the `foreach` loop lets you carry out a task on each item in the collection.</span></span> <span data-ttu-id="1c451-438">Этот тип цикла удобен для коллекций, так как в отличие от `for` цикл, у вас нет для увеличения значения счетчика или ограничить.</span><span class="sxs-lookup"><span data-stu-id="1c451-438">This type of loop is convenient for collections, because unlike a `for` loop, you don't have to increment the counter or set a limit.</span></span> <span data-ttu-id="1c451-439">Вместо этого `foreach` код цикла просто продолжается через коллекцию до его завершения.</span><span class="sxs-lookup"><span data-stu-id="1c451-439">Instead, the `foreach` loop code simply proceeds through the collection until it's finished.</span></span>

<span data-ttu-id="1c451-440">Например, следующий код возвращает элементы в `Request.ServerVariables` коллекции, которая представляет собой объект, содержащий сведения о веб-сервере.</span><span class="sxs-lookup"><span data-stu-id="1c451-440">For example, the following code returns the items in the `Request.ServerVariables` collection, which is an object that contains information about your web server.</span></span> <span data-ttu-id="1c451-441">Она использует `foreac` h цикл для отображения имени каждого элемента путем создания нового `<li>` элемента в HTML-маркированного списка.</span><span class="sxs-lookup"><span data-stu-id="1c451-441">It uses a `foreac` h loop to display the name of each item by creating a new `<li>` element in an HTML bulleted list.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

<span data-ttu-id="1c451-442">`foreach` Ключевого слова следуют скобки где объявления переменной, которая представляет один элемент в коллекции (в примере `var item`), за которым следует `in` ключевое слово, и вы хотите организовать цикл по коллекции.</span><span class="sxs-lookup"><span data-stu-id="1c451-442">The `foreach` keyword is followed by parentheses where you declare a variable that represents a single item in the collection (in the example, `var item`), followed by the `in` keyword, followed by the collection you want to loop through.</span></span> <span data-ttu-id="1c451-443">В теле `foreach` цикла, можно получить доступ к текущего элемента, с помощью переменной, объявленный ранее.</span><span class="sxs-lookup"><span data-stu-id="1c451-443">In the body of the `foreach` loop, you can access the current item using the variable that you declared earlier.</span></span>

![Razor Img12](introducing-razor-syntax-c/_static/image12.jpg)

<span data-ttu-id="1c451-445">Чтобы создать цикл программирования более общего назначения, используйте `while` инструкции:</span><span class="sxs-lookup"><span data-stu-id="1c451-445">To create a more general-purpose loop, use the `while` statement:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

<span data-ttu-id="1c451-446">Объект `while` цикл начинается с `while` ключевое слово, и круглые скобки вы зададите, как долго цикл продолжается (здесь, пока `countNum` меньше 50), затем блок для повторения.</span><span class="sxs-lookup"><span data-stu-id="1c451-446">A `while` loop begins with the `while` keyword, followed by parentheses where you specify how long the loop continues (here, for as long as `countNum` is less than 50), then the block to repeat.</span></span> <span data-ttu-id="1c451-447">Циклы обычно увеличивается (Добавление) или уменьшения (вычитаемое) переменной или объекта, используемые для подсчета.</span><span class="sxs-lookup"><span data-stu-id="1c451-447">Loops typically increment (add to) or decrement (subtract from) a variable or object used for counting.</span></span> <span data-ttu-id="1c451-448">В примере `+=` оператор добавляет 1 к `countNum` при каждом выполнении цикла.</span><span class="sxs-lookup"><span data-stu-id="1c451-448">In the example, the `+=` operator adds 1 to `countNum` each time the loop runs.</span></span> <span data-ttu-id="1c451-449">(Чтобы уменьшают значение переменной в цикле, который отсчитывает, используйте оператор декремента `-=`).</span><span class="sxs-lookup"><span data-stu-id="1c451-449">(To decrement a variable in a loop that counts down, you would use the decrement operator `-=`).</span></span>

## <a name="objects-and-collections"></a><span data-ttu-id="1c451-450">Объекты и коллекции</span><span class="sxs-lookup"><span data-stu-id="1c451-450">Objects and Collections</span></span>

<span data-ttu-id="1c451-451">Почти все в на веб-сайте ASP.NET — это объект, включая саму веб-страницу.</span><span class="sxs-lookup"><span data-stu-id="1c451-451">Nearly everything in an ASP.NET website is an object, including the web page itself.</span></span> <span data-ttu-id="1c451-452">В этом разделе рассматриваются некоторые важные объекты, которые вы будете работать с часто в коде.</span><span class="sxs-lookup"><span data-stu-id="1c451-452">This section discusses some important objects you'll work with frequently in your code.</span></span>

### <a name="page-objects"></a><span data-ttu-id="1c451-453">Объекты страниц</span><span class="sxs-lookup"><span data-stu-id="1c451-453">Page Objects</span></span>

<span data-ttu-id="1c451-454">Самый простой объект в ASP.NET является страница.</span><span class="sxs-lookup"><span data-stu-id="1c451-454">The most basic object in ASP.NET is the page.</span></span> <span data-ttu-id="1c451-455">Можно получить доступ к свойствам объекта страницу напрямую без любой подходящий объект.</span><span class="sxs-lookup"><span data-stu-id="1c451-455">You can access properties of the page object directly without any qualifying object.</span></span> <span data-ttu-id="1c451-456">Следующий код получает путь к файлу страницы, с помощью `Request` объект страницы:</span><span class="sxs-lookup"><span data-stu-id="1c451-456">The following code gets the page's file path, using the `Request` object of the page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

<span data-ttu-id="1c451-457">Чтобы сделать его очистки, которые вы ссылаетесь, свойства и методы для текущего объекта страницы, при необходимости можно использовать ключевое слово `this` для представления объекта страницы в коде.</span><span class="sxs-lookup"><span data-stu-id="1c451-457">To make it clear that you're referencing properties and methods on the current page object, you can optionally use the keyword `this` to represent the page object in your code.</span></span> <span data-ttu-id="1c451-458">Ниже приведен в предыдущем примере кода с `this` добавляется для представления страницы:</span><span class="sxs-lookup"><span data-stu-id="1c451-458">Here is the previous code example, with `this` added to represent the page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

<span data-ttu-id="1c451-459">Можно использовать свойства `Page` объекта, чтобы получить массу информации, такие как:</span><span class="sxs-lookup"><span data-stu-id="1c451-459">You can use properties of the `Page` object to get a lot of information, such as:</span></span>

- <span data-ttu-id="1c451-460">`Request`.</span><span class="sxs-lookup"><span data-stu-id="1c451-460">`Request`.</span></span> <span data-ttu-id="1c451-461">Как вы уже видели, это коллекция сведений о текущем запросе, включая тип браузера, выдавший запрос, URL-адрес страницы, удостоверение пользователя и т. д.</span><span class="sxs-lookup"><span data-stu-id="1c451-461">As you've already seen, this is a collection of information about the current request, including what type of browser made the request, the URL of the page, the user identity, etc.</span></span>
- <span data-ttu-id="1c451-462">`Response`.</span><span class="sxs-lookup"><span data-stu-id="1c451-462">`Response`.</span></span> <span data-ttu-id="1c451-463">Это коллекция сведений об ответе (страницы), отправляемого в браузер при серверный код завершения работы.</span><span class="sxs-lookup"><span data-stu-id="1c451-463">This is a collection of information about the response (page) that will be sent to the browser when the server code has finished running.</span></span> <span data-ttu-id="1c451-464">Например это свойство можно использовать для записи сведений в ответ.</span><span class="sxs-lookup"><span data-stu-id="1c451-464">For example, you can use this property to write information into the response.</span></span> 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a><span data-ttu-id="1c451-465">Коллекция объектов (массивов и словарей)</span><span class="sxs-lookup"><span data-stu-id="1c451-465">Collection Objects (Arrays and Dictionaries)</span></span>

<span data-ttu-id="1c451-466">Объект *коллекции* — это группа объектов одного типа, например, коллекцию `Customer` объекты из базы данных.</span><span class="sxs-lookup"><span data-stu-id="1c451-466">A *collection* is a group of objects of the same type, such as a collection of `Customer` objects from a database.</span></span> <span data-ttu-id="1c451-467">ASP.NET содержит множество встроенных коллекций, включая `Request.Files` коллекции.</span><span class="sxs-lookup"><span data-stu-id="1c451-467">ASP.NET contains many built-in collections, like the `Request.Files` collection.</span></span>

<span data-ttu-id="1c451-468">Вы будете часто работать с данными в коллекции.</span><span class="sxs-lookup"><span data-stu-id="1c451-468">You'll often work with data in collections.</span></span> <span data-ttu-id="1c451-469">Два распространенных типов коллекций являются *массива* и *словарь*.</span><span class="sxs-lookup"><span data-stu-id="1c451-469">Two common collection types are the *array* and the *dictionary*.</span></span> <span data-ttu-id="1c451-470">Массив полезно в том случае, если вы хотите хранить коллекции схожих элементов, но не нужно создавать отдельную переменную для хранения каждого элемента:</span><span class="sxs-lookup"><span data-stu-id="1c451-470">An array is useful when you want to store a collection of similar items but don't want to create a separate variable to hold each item:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

<span data-ttu-id="1c451-471">В массивах можно объявить определенный тип данных, например `string`, `int`, или `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="1c451-471">With arrays, you declare a specific data type, such as `string`, `int`, or `DateTime`.</span></span> <span data-ttu-id="1c451-472">Чтобы указать, что переменная может содержать массив, добавьте скобки к объявлению (такие как `string[]` или `int[]`).</span><span class="sxs-lookup"><span data-stu-id="1c451-472">To indicate that the variable can contain an array, you add brackets to the declaration (such as `string[]` or `int[]`).</span></span> <span data-ttu-id="1c451-473">Можно получить доступ к элементам в массиве с помощью их положение (индекс) или с помощью `foreach` инструкции.</span><span class="sxs-lookup"><span data-stu-id="1c451-473">You can access items in an array using their position (index) or by using the `foreach` statement.</span></span> <span data-ttu-id="1c451-474">Массив индексы отсчитываются от нуля &#8212; то есть первый элемент находится в позиции 0, второй элемент находится в позиции 1 и т. д.</span><span class="sxs-lookup"><span data-stu-id="1c451-474">Array indexes are zero-based &#8212; that is, the first item is at position 0, the second item is at position 1, and so on.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

<span data-ttu-id="1c451-475">Можно определить количество элементов в массиве, получив его `Length` свойство.</span><span class="sxs-lookup"><span data-stu-id="1c451-475">You can determine the number of items in an array by getting its `Length` property.</span></span> <span data-ttu-id="1c451-476">Чтобы получить позицию конкретного элемента в массиве (для поиска массив), используйте `Array.IndexOf` метод.</span><span class="sxs-lookup"><span data-stu-id="1c451-476">To get the position of a specific item in the array (to search the array), use the `Array.IndexOf` method.</span></span> <span data-ttu-id="1c451-477">Вы можете также выполнять действия как обратное содержимое массива ( `Array.Reverse` метод) или Сортировка содержимого ( `Array.Sort` метод).</span><span class="sxs-lookup"><span data-stu-id="1c451-477">You can also do things like reverse the contents of an array (the `Array.Reverse` method) or sort the contents (the `Array.Sort` method).</span></span>

<span data-ttu-id="1c451-478">В результате выполнения код массив строки, отображаемый в браузере.</span><span class="sxs-lookup"><span data-stu-id="1c451-478">The output of the string array code displayed in a browser:</span></span>

![Razor Img13](introducing-razor-syntax-c/_static/image13.jpg)

<span data-ttu-id="1c451-480">Словарь является коллекцией пар "ключ значение", где можно ввести ключ (или имя), чтобы задать или получить соответствующее значение:</span><span class="sxs-lookup"><span data-stu-id="1c451-480">A dictionary is a collection of key/value pairs, where you provide the key (or name) to set or retrieve the corresponding value:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

<span data-ttu-id="1c451-481">Чтобы создать словарь, используйте `new` ключевое слово, чтобы указать, что вы создаете новый объект словаря.</span><span class="sxs-lookup"><span data-stu-id="1c451-481">To create a dictionary, you use the `new` keyword to indicate that you're creating a new dictionary object.</span></span> <span data-ttu-id="1c451-482">Словарь можно назначить переменной с помощью `var` ключевое слово.</span><span class="sxs-lookup"><span data-stu-id="1c451-482">You can assign a dictionary to a variable using the `var` keyword.</span></span> <span data-ttu-id="1c451-483">Можно указать типы данных элементов в словарь, используя угловые скобки ( `< >` ).</span><span class="sxs-lookup"><span data-stu-id="1c451-483">You indicate the data types of the items in the dictionary using angle brackets ( `< >` ).</span></span> <span data-ttu-id="1c451-484">В конце объявления необходимо добавить пару скобок, так как это фактически является методом, который создает новый словарь.</span><span class="sxs-lookup"><span data-stu-id="1c451-484">At the end of the declaration, you must add a pair of parentheses, because this is actually a method that creates a new dictionary.</span></span>

<span data-ttu-id="1c451-485">Для добавления элементов в словарь, можно вызвать `Add` метод переменной словарь (`myScores` в данном случае), а затем укажите ключ и значение.</span><span class="sxs-lookup"><span data-stu-id="1c451-485">To add items to the dictionary, you can call the `Add` method of the dictionary variable (`myScores` in this case), and then specify a key and a value.</span></span> <span data-ttu-id="1c451-486">Кроме того можно использовать квадратные скобки для указания ключа и выполнить простое присваивание, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="1c451-486">Alternatively, you can use square brackets to indicate the key and do a simple assignment, as in the following example:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

<span data-ttu-id="1c451-487">Чтобы получить значение из словаря, необходимо указать ключ в квадратные скобки:</span><span class="sxs-lookup"><span data-stu-id="1c451-487">To get a value from the dictionary, you specify the key in brackets:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a><span data-ttu-id="1c451-488">Вызов методов с параметрами</span><span class="sxs-lookup"><span data-stu-id="1c451-488">Calling Methods with Parameters</span></span>

<span data-ttu-id="1c451-489">Во время прочтения этой статьи, объекты, которые программируется с могут иметь методы.</span><span class="sxs-lookup"><span data-stu-id="1c451-489">As you read earlier in this article, the objects that you program with can have methods.</span></span> <span data-ttu-id="1c451-490">Например `Database` объект может иметь `Database.Connect` метод.</span><span class="sxs-lookup"><span data-stu-id="1c451-490">For example, a `Database` object might have a `Database.Connect` method.</span></span> <span data-ttu-id="1c451-491">Многие методы также имеют один или несколько параметров.</span><span class="sxs-lookup"><span data-stu-id="1c451-491">Many methods also have one or more parameters.</span></span> <span data-ttu-id="1c451-492">Объект *параметр* является значение, которое можно передать в метод, чтобы включить этот метод для выполнения необходимых задач.</span><span class="sxs-lookup"><span data-stu-id="1c451-492">A *parameter* is a value that you pass to a method to enable the method to complete its task.</span></span> <span data-ttu-id="1c451-493">Например, взгляните на объявление для `Request.MapPath` метод, который принимает три параметра:</span><span class="sxs-lookup"><span data-stu-id="1c451-493">For example, look at a declaration for the `Request.MapPath` method, which takes three parameters:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

<span data-ttu-id="1c451-494">(Строка была разбита на сделать код более удобочитаемым.</span><span class="sxs-lookup"><span data-stu-id="1c451-494">(The line has been wrapped to make it more readable.</span></span> <span data-ttu-id="1c451-495">Помните, что можно поместить разрывы строк практически любом месте, за исключением того, внутри строк, заключенных в кавычки.)</span><span class="sxs-lookup"><span data-stu-id="1c451-495">Remember that you can put line breaks almost any place except inside strings that are enclosed in quotation marks.)</span></span>

<span data-ttu-id="1c451-496">Этот метод возвращает физический путь на сервере, который соответствует для заданного виртуального пути.</span><span class="sxs-lookup"><span data-stu-id="1c451-496">This method returns the physical path on the server that corresponds to a specified virtual path.</span></span> <span data-ttu-id="1c451-497">Три параметра для метода являются `virtualPath`, `baseVirtualDir`, и `allowCrossAppMapping`.</span><span class="sxs-lookup"><span data-stu-id="1c451-497">The three parameters for the method are `virtualPath`, `baseVirtualDir`, and `allowCrossAppMapping`.</span></span> <span data-ttu-id="1c451-498">(Обратите внимание на то, что в объявлении с типами данных, данных, которые они оставлю перечисляются параметры). При вызове этого метода необходимо указать значения для всех трех параметров.</span><span class="sxs-lookup"><span data-stu-id="1c451-498">(Notice that in the declaration, the parameters are listed with the data types of the data that they'll accept.) When you call this method, you must supply values for all three parameters.</span></span>

<span data-ttu-id="1c451-499">Синтаксис Razor предоставляет два варианта для передачи параметров в метод: *позиционные параметры* и *именованные параметры*.</span><span class="sxs-lookup"><span data-stu-id="1c451-499">The Razor syntax gives you two options for passing parameters to a method: *positional parameters* and *named parameters*.</span></span> <span data-ttu-id="1c451-500">Чтобы вызвать метод, с помощью позиционные параметры, параметры передаются в строгом порядке, который указан в объявлении метода.</span><span class="sxs-lookup"><span data-stu-id="1c451-500">To call a method using positional parameters, you pass the parameters in a strict order that's specified in the method declaration.</span></span> <span data-ttu-id="1c451-501">(Вы обычно будет этот порядок по чтению документации для метода.) Необходимо соблюдать порядок и какого-либо параметра невозможно пропустить &#8212; при необходимости можно передать пустую строку (`""`) или `null` для позиционного параметра, которое не имеет значения для.</span><span class="sxs-lookup"><span data-stu-id="1c451-501">(You would typically know this order by reading documentation for the method.) You must follow the order, and you can't skip any of the parameters &#8212; if necessary, you pass an empty string (`""`) or `null` for a positional parameter that you don't have a value for.</span></span>

<span data-ttu-id="1c451-502">В следующем примере предполагается, что у вас есть папка с именем *сценариев* на веб-сайте.</span><span class="sxs-lookup"><span data-stu-id="1c451-502">The following example assumes you have a folder named *scripts* on your website.</span></span> <span data-ttu-id="1c451-503">Этот код вызывает `Request.MapPath` метод и передает эти значения для трех параметров в правильном порядке.</span><span class="sxs-lookup"><span data-stu-id="1c451-503">The code calls the `Request.MapPath` method and passes values for the three parameters in the correct order.</span></span> <span data-ttu-id="1c451-504">Затем отображается сопоставленные результирующий контур.</span><span class="sxs-lookup"><span data-stu-id="1c451-504">It then displays the resulting mapped path.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

<span data-ttu-id="1c451-505">При метод имеет множество параметров, можно сохранить код более удобочитаемым, используя именованные параметры.</span><span class="sxs-lookup"><span data-stu-id="1c451-505">When a method has many parameters, you can keep your code more readable by using named parameters.</span></span> <span data-ttu-id="1c451-506">Для вызова метода с использованием именованных параметров, укажите имя параметра следует двоеточие (:), а затем значение.</span><span class="sxs-lookup"><span data-stu-id="1c451-506">To call a method using named parameters, you specify the parameter name followed by a colon (:), and then the value.</span></span> <span data-ttu-id="1c451-507">Именованных параметров удобен тем, что их можно передать в любом порядке, который вы хотите.</span><span class="sxs-lookup"><span data-stu-id="1c451-507">The advantage of named parameters is that you can pass them in any order you want.</span></span> <span data-ttu-id="1c451-508">(Недостаток заключается в вызове метода не является компактным).</span><span class="sxs-lookup"><span data-stu-id="1c451-508">(A disadvantage is that the method call is not as compact.)</span></span>

<span data-ttu-id="1c451-509">В следующем примере вызывается тот же метод, как описано выше, но использует именованные параметры для предоставления значений:</span><span class="sxs-lookup"><span data-stu-id="1c451-509">The following example calls the same method as above, but uses named parameters to supply the values:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

<span data-ttu-id="1c451-510">Как видите, эти параметры передаются в другом порядке.</span><span class="sxs-lookup"><span data-stu-id="1c451-510">As you can see, the parameters are passed in a different order.</span></span> <span data-ttu-id="1c451-511">Тем не менее при выполнении предыдущего примера и в этом примере, они будут возвращать то же значение.</span><span class="sxs-lookup"><span data-stu-id="1c451-511">However, if you run the previous example and this example, they'll return the same value.</span></span>

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a><span data-ttu-id="1c451-512">Обработка ошибок</span><span class="sxs-lookup"><span data-stu-id="1c451-512">Handling Errors</span></span>

### <a name="try-catch-statements"></a><span data-ttu-id="1c451-513">Операторы Try-Catch</span><span class="sxs-lookup"><span data-stu-id="1c451-513">Try-Catch Statements</span></span>

<span data-ttu-id="1c451-514">У вас будет часто инструкций в коде, может завершиться ошибкой по соображениям за пределами элемента управления.</span><span class="sxs-lookup"><span data-stu-id="1c451-514">You'll often have statements in your code that might fail for reasons outside your control.</span></span> <span data-ttu-id="1c451-515">Пример:</span><span class="sxs-lookup"><span data-stu-id="1c451-515">For example:</span></span>

- <span data-ttu-id="1c451-516">Если код пытается создать или получить доступ к файлу, может возникнуть все виды ошибок.</span><span class="sxs-lookup"><span data-stu-id="1c451-516">If your code tries to create or access a file, all sorts of errors might occur.</span></span> <span data-ttu-id="1c451-517">Файл может отсутствовать, может быть заблокирован, код может не иметь разрешения и т. д.</span><span class="sxs-lookup"><span data-stu-id="1c451-517">The file you want might not exist, it might be locked, the code might not have permissions, and so on.</span></span>
- <span data-ttu-id="1c451-518">Аналогичным образом Если код пытается выполнить обновления записей в базе данных, могут быть проблемы с разрешениями, подключение к базе данных могут быть удалены, сохраняемые данные может быть недопустимым и т. д.</span><span class="sxs-lookup"><span data-stu-id="1c451-518">Similarly, if your code tries to update records in a database, there can be permissions issues, the connection to the database might be dropped, the data to save might be invalid, and so on.</span></span>

<span data-ttu-id="1c451-519">С точки зрения программирования, называются таких ситуациях *исключения*.</span><span class="sxs-lookup"><span data-stu-id="1c451-519">In programming terms, these situations are called *exceptions*.</span></span> <span data-ttu-id="1c451-520">Если код обнаруживает исключение, он создает (вызывает) сообщение об ошибке каталог, в лучшем случае раздражающих пользователям:</span><span class="sxs-lookup"><span data-stu-id="1c451-520">If your code encounters an exception, it generates (throws) an error message that's, at best, annoying to users:</span></span>

![Razor Img14](introducing-razor-syntax-c/_static/image14.jpg)

<span data-ttu-id="1c451-522">В ситуациях, где ваш код может обнаружить исключения, а также чтобы избежать сообщения об ошибках этого типа, можно использовать `try/catch` инструкций.</span><span class="sxs-lookup"><span data-stu-id="1c451-522">In situations where your code might encounter exceptions, and in order to avoid error messages of this type, you can use `try/catch` statements.</span></span> <span data-ttu-id="1c451-523">В `try` инструкции, запустите код, который вы проверяете.</span><span class="sxs-lookup"><span data-stu-id="1c451-523">In the `try` statement, you run the code that you're checking.</span></span> <span data-ttu-id="1c451-524">В одной или нескольких `catch` инструкций, можно произвести поиск конкретных ошибок (определенные типы исключений), которые могли возникнуть.</span><span class="sxs-lookup"><span data-stu-id="1c451-524">In one or more `catch` statements, you can look for specific errors (specific types of exceptions) that might have occurred.</span></span> <span data-ttu-id="1c451-525">Можно включить столько `catch` инструкциях, как вам нужно искать ошибки, которые вы ожидаете.</span><span class="sxs-lookup"><span data-stu-id="1c451-525">You can include as many `catch` statements as you need to look for errors that you are anticipating.</span></span>

> [!NOTE]
> <span data-ttu-id="1c451-526">Рекомендуется избегать использования `Response.Redirect` метод в `try/catch` инструкции, так как он может вызвать исключение в страницу.</span><span class="sxs-lookup"><span data-stu-id="1c451-526">We recommend that you avoid using the `Response.Redirect` method in `try/catch` statements, because it can cause an exception in your page.</span></span>


<span data-ttu-id="1c451-527">Пример страницы, которая создает текстовый файл, при первом запросе и затем отображает кнопку, которая позволяет пользователю открыть файл.</span><span class="sxs-lookup"><span data-stu-id="1c451-527">The following example shows a page that creates a text file on the first request and then displays a button that lets the user open the file.</span></span> <span data-ttu-id="1c451-528">В примере намеренно используется недопустимое имя файла, чтобы он вызовет исключение.</span><span class="sxs-lookup"><span data-stu-id="1c451-528">The example deliberately uses a bad file name so that it will cause an exception.</span></span> <span data-ttu-id="1c451-529">Код включает `catch` инструкций для двух возможных исключений: `FileNotFoundException`, это происходит, если имя файла недопустимо, и `DirectoryNotFoundException`, это происходит, если ASP.NET даже не удается найти папку.</span><span class="sxs-lookup"><span data-stu-id="1c451-529">The code includes `catch` statements for two possible exceptions: `FileNotFoundException`, which occurs if the file name is bad, and `DirectoryNotFoundException`, which occurs if ASP.NET can't even find the folder.</span></span> <span data-ttu-id="1c451-530">(Чтобы увидеть, как оно работает, если все работает надлежащим образом можно Раскомментировать инструкция в примере.)</span><span class="sxs-lookup"><span data-stu-id="1c451-530">(You can uncomment a statement in the example in order to see how it runs when everything works properly.)</span></span>

<span data-ttu-id="1c451-531">Если ваш код не обрабатывает исключение, вы увидите страницу ошибки, как и на предыдущем снимке экрана.</span><span class="sxs-lookup"><span data-stu-id="1c451-531">If your code didn't handle the exception, you would see an error page like the previous screen shot.</span></span> <span data-ttu-id="1c451-532">Тем не менее `try/catch` раздел помогает предотвратить просмотр таких ошибок пользователя.</span><span class="sxs-lookup"><span data-stu-id="1c451-532">However, the `try/catch` section helps prevent the user from seeing these types of errors.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="1c451-533">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="1c451-533">Additional Resources</span></span>

<span data-ttu-id="1c451-534">**Программирование с использованием Visual Basic**</span><span class="sxs-lookup"><span data-stu-id="1c451-534">**Programming with Visual Basic**</span></span>


[<span data-ttu-id="1c451-535">Приложение: язык Visual Basic и синтаксис</span><span class="sxs-lookup"><span data-stu-id="1c451-535">Appendix: Visual Basic Language and Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkId=202908)


<span data-ttu-id="1c451-536">**Справочная документация**</span><span class="sxs-lookup"><span data-stu-id="1c451-536">**Reference Documentation**</span></span>


[<span data-ttu-id="1c451-537">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1c451-537">ASP.NET</span></span>](https://msdn.microsoft.com/library/ee532866.aspx)

[<span data-ttu-id="1c451-538">Язык C#</span><span class="sxs-lookup"><span data-stu-id="1c451-538">C# Language</span></span>](https://msdn.microsoft.com/library/kx37x362.aspx)
