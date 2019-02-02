---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: Чего не следует делать в ASP.NET и что нужно делать вместо этого | Документация Майкрософт
author: Rick-Anderson
description: В этом разделе описывается несколько распространенных ошибок, которые люди делают в веб-проектов ASP.NET. Представлены рекомендации по что делать, чтобы избежать этих commo...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 512d2e2b39467635390fa175546f79d8c9f89f4a
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667717"
---
# <a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a><span data-ttu-id="01f32-104">Запрещенные и разрешенные действия в ASP.NET</span><span class="sxs-lookup"><span data-stu-id="01f32-104">What not to do in ASP.NET, and what to do instead</span></span>

> <span data-ttu-id="01f32-105">В этом разделе описывается несколько распространенных ошибок, которые люди делают в веб-проектов ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="01f32-105">This topic describes several common mistakes people make within ASP.NET web projects.</span></span> <span data-ttu-id="01f32-106">Представлены рекомендации по что делать, чтобы избежать этих распространенных ошибок.</span><span class="sxs-lookup"><span data-stu-id="01f32-106">It provides recommendations for what you should do to avoid these common mistakes.</span></span> <span data-ttu-id="01f32-107">Он основан на [презентации](http://vimeo.com/68390507) по **Damian Edwards** на конференции Norwegian Developers Conference.</span><span class="sxs-lookup"><span data-stu-id="01f32-107">It is based on a [presentation](http://vimeo.com/68390507) by **Damian Edwards** at Norwegian Developers Conference.</span></span>


## <a name="disclaimer"></a><span data-ttu-id="01f32-108">Отказ от ответственности</span><span class="sxs-lookup"><span data-stu-id="01f32-108">Disclaimer</span></span>

<span data-ttu-id="01f32-109">В этом разделе не предназначен как полное руководство по, чтобы убедиться, что приложение является безопасной и эффективной.</span><span class="sxs-lookup"><span data-stu-id="01f32-109">This topic is not intended as a complete guide to ensure your application is secure and efficient.</span></span> <span data-ttu-id="01f32-110">Необходимо по-прежнему следуйте рекомендациям по безопасности и производительности, не описанные в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="01f32-110">You still need to follow best practices for security and performance that are not outlined in this topic.</span></span> <span data-ttu-id="01f32-111">Только способы избежать типичных ошибок, связанных с классами .NET и процессы.</span><span class="sxs-lookup"><span data-stu-id="01f32-111">It only suggests how to avoid common mistakes related to .NET classes and processes.</span></span>

## <a name="overview"></a><span data-ttu-id="01f32-112">Обзор</span><span class="sxs-lookup"><span data-stu-id="01f32-112">Overview</span></span>

<span data-ttu-id="01f32-113">В этом разделе содержатся следующие подразделы.</span><span class="sxs-lookup"><span data-stu-id="01f32-113">This topic contains the following sections:</span></span>

- [<span data-ttu-id="01f32-114">Соответствие стандартам</span><span class="sxs-lookup"><span data-stu-id="01f32-114">Standards Compliance</span></span>](#standards)

    - [<span data-ttu-id="01f32-115">Адаптеры элементов управления</span><span class="sxs-lookup"><span data-stu-id="01f32-115">Control Adapters</span></span>](#adapters)
    - [<span data-ttu-id="01f32-116">Свойства стиля для элементов управления</span><span class="sxs-lookup"><span data-stu-id="01f32-116">Style Properties on Controls</span></span>](#styleprop)
    - [<span data-ttu-id="01f32-117">Страницы и обратные вызовы элемент управления</span><span class="sxs-lookup"><span data-stu-id="01f32-117">Page and Control Callbacks</span></span>](#callback)
    - [<span data-ttu-id="01f32-118">Определение возможностей браузера</span><span class="sxs-lookup"><span data-stu-id="01f32-118">Browser Capability Detection</span></span>](#browsercap)
- [<span data-ttu-id="01f32-119">Безопасность</span><span class="sxs-lookup"><span data-stu-id="01f32-119">Security</span></span>](#security)

    - [<span data-ttu-id="01f32-120">Проверка запросов</span><span class="sxs-lookup"><span data-stu-id="01f32-120">Request Validation</span></span>](#validation)
    - [<span data-ttu-id="01f32-121">Аутентификация с помощью форм без поддержки файлов cookie и сеансов</span><span class="sxs-lookup"><span data-stu-id="01f32-121">Cookieless Forms Authentication and Session</span></span>](#cookieless)
    - [<span data-ttu-id="01f32-122">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="01f32-122">EnableViewStateMac</span></span>](#viewstatemac)
    - [<span data-ttu-id="01f32-123">Средний уровень доверия</span><span class="sxs-lookup"><span data-stu-id="01f32-123">Medium Trust</span></span>](#medium)
    - [<span data-ttu-id="01f32-124">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="01f32-124">&lt;appSettings&gt;</span></span>](#appsettings)
    - [<span data-ttu-id="01f32-125">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="01f32-125">UrlPathEncode</span></span>](#urlpathencode)
- [<span data-ttu-id="01f32-126">Надежность и производительность</span><span class="sxs-lookup"><span data-stu-id="01f32-126">Reliability and Performance</span></span>](#performance)

    - [<span data-ttu-id="01f32-127">PreSendRequestHeaders и PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="01f32-127">PreSendRequestHeaders and PreSendRequestContent</span></span>](#presend)
    - [<span data-ttu-id="01f32-128">События асинхронной страницы с веб-форм</span><span class="sxs-lookup"><span data-stu-id="01f32-128">Asynchronous Page Events with Web Forms</span></span>](#asyncevents)
    - [<span data-ttu-id="01f32-129">Выстрелил и забыл работы</span><span class="sxs-lookup"><span data-stu-id="01f32-129">Fire-and-Forget Work</span></span>](#fire)
    - [<span data-ttu-id="01f32-130">Тело сущности запроса</span><span class="sxs-lookup"><span data-stu-id="01f32-130">Request Entity Body</span></span>](#requestentity)
    - [<span data-ttu-id="01f32-131">Response.Redirect и Response.End</span><span class="sxs-lookup"><span data-stu-id="01f32-131">Response.Redirect and Response.End</span></span>](#redirect)
    - [<span data-ttu-id="01f32-132">EnableViewState и ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="01f32-132">EnableViewState and ViewStateMode</span></span>](#viewstatemode)
    - [<span data-ttu-id="01f32-133">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="01f32-133">SqlMembershipProvider</span></span>](#sqlprovider)
    - [<span data-ttu-id="01f32-134">Длительные запросы, под управлением (> 110 секунд)</span><span class="sxs-lookup"><span data-stu-id="01f32-134">Long Running Requests (>110 seconds)</span></span>](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a><span data-ttu-id="01f32-135">Соответствие стандартам</span><span class="sxs-lookup"><span data-stu-id="01f32-135">Standards Compliance</span></span>

<a id="adapters"></a>

### <a name="control-adapters"></a><span data-ttu-id="01f32-136">Адаптеры элементов управления</span><span class="sxs-lookup"><span data-stu-id="01f32-136">Control adapters</span></span>

<span data-ttu-id="01f32-137">Рекомендация: Отключить использование адаптеров элементов управления для адаптивной отрисовки, а вместо этого использовать CSS медиа-запросами и соответствующих стандартам HTML.</span><span class="sxs-lookup"><span data-stu-id="01f32-137">Recommendation: Stop using control adapters for adaptive rendering, and instead use CSS media queries and standards-compliant HTML.</span></span>

<span data-ttu-id="01f32-138">Адаптеры элементов управления появились в .NET 2.0 для визуализации представления код, который был настроен для различных устройств и сред.</span><span class="sxs-lookup"><span data-stu-id="01f32-138">Controls Adapters were introduced in .NET 2.0 to render presentation code that was customized for different devices and environments.</span></span> <span data-ttu-id="01f32-139">Теперь это адаптивный отрисовку можно выполнить с помощью CSS и HTML.</span><span class="sxs-lookup"><span data-stu-id="01f32-139">Now, this adaptive rendering can be accomplished with CSS and HTML.</span></span> <span data-ttu-id="01f32-140">Следует прекратить использование адаптеров элементов управления и преобразовать любые существующие адаптеры CSS и HTML.</span><span class="sxs-lookup"><span data-stu-id="01f32-140">You should stop using Control Adapters and convert any existing adapters to CSS and HTML.</span></span>

<span data-ttu-id="01f32-141">Дополнительные сведения см. в разделе [медиа-запросами](http://www.w3.org/TR/css3-mediaqueries/) и [How To: Добавление мобильных страниц в ASP.NET Web Forms и MVC-приложении](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="01f32-141">For more information, see [Media Queries](http://www.w3.org/TR/css3-mediaqueries/) and [How To: Add Mobile Pages to Your ASP.NET Web Forms / MVC Application](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span></span>

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a><span data-ttu-id="01f32-142">Свойства стиля для элементов управления</span><span class="sxs-lookup"><span data-stu-id="01f32-142">Style properties on controls</span></span>

<span data-ttu-id="01f32-143">Рекомендация: Остановить задание значений стиля в разметку элемента управления, а вместо этого задать форматирование значений в таблицах стилей CSS.</span><span class="sxs-lookup"><span data-stu-id="01f32-143">Recommendation: Stop setting style values in the control markup, and instead set formatting values in CSS stylesheets.</span></span>

<span data-ttu-id="01f32-144">Веб-сервера управления содержат множество свойств, которые могут использоваться для задания свойства встроенного стиля.</span><span class="sxs-lookup"><span data-stu-id="01f32-144">Web server controls contain dozens of properties which can be used to set in-line style properties.</span></span> <span data-ttu-id="01f32-145">Например свойства ForeColor задает цвет текста для элемента управления.</span><span class="sxs-lookup"><span data-stu-id="01f32-145">For example, the ForeColor property sets the color of the text for a control.</span></span> <span data-ttu-id="01f32-146">Это можно сделать этот же эффект, более эффективно через таблицы стилей CSS.</span><span class="sxs-lookup"><span data-stu-id="01f32-146">You can accomplish this same effect more efficiently through CSS stylesheets.</span></span> <span data-ttu-id="01f32-147">Таблицы стилей позволяют централизовать значений стиля и не рекомендуется устанавливать эти значения во всем приложении.</span><span class="sxs-lookup"><span data-stu-id="01f32-147">Stylesheets enable you to centralize style values and avoid setting these values throughout your application.</span></span>

<span data-ttu-id="01f32-148">Приведенный ниже показан класс CSS на красный текст наборов.</span><span class="sxs-lookup"><span data-stu-id="01f32-148">The following example shows a CSS class the sets text to red.</span></span>

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

<span data-ttu-id="01f32-149">В следующем примере показано, как динамически применить класс CSS.</span><span class="sxs-lookup"><span data-stu-id="01f32-149">The next example shows how to dynamically apply the CSS class.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a><span data-ttu-id="01f32-150">Обратные вызовы страниц и элементов управления</span><span class="sxs-lookup"><span data-stu-id="01f32-150">Page and control callbacks</span></span>

<span data-ttu-id="01f32-151">Рекомендация: Прекратить использование обратных вызовов страниц и элементов управления и вместо этого использовать любой из следующих: AJAX UpdatePanel, MVC методы действий, веб-API и SignalR.</span><span class="sxs-lookup"><span data-stu-id="01f32-151">Recommendation: Stop using page and control callbacks, and instead use any of the following: AJAX, UpdatePanel, MVC action methods, Web API, or SignalR.</span></span>

<span data-ttu-id="01f32-152">В более ранних версиях ASP.NET добавлены методы обратного вызова страницы и элемента управления позволяет обновлять части веб-страницы без обновления всей страницы.</span><span class="sxs-lookup"><span data-stu-id="01f32-152">In earlier versions of ASP.NET, Page and Control callback methods enabled you to update part of the web page without refreshing an entire page.</span></span> <span data-ttu-id="01f32-153">Теперь можно выполнить частичного обновления страницы через [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [веб-API](../../../web-api/index.md) или [SignalR](../../../signalr/index.md).</span><span class="sxs-lookup"><span data-stu-id="01f32-153">You can now accomplish partial-page updates through [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web API](../../../web-api/index.md) or [SignalR](../../../signalr/index.md).</span></span> <span data-ttu-id="01f32-154">Следует остановить с помощью методов обратного вызова, так как они могут вызвать проблемы с помощью удобных URL-адресов и маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="01f32-154">You should stop using callback methods because they can cause issues with friendly URLs and routing.</span></span> <span data-ttu-id="01f32-155">По умолчанию элементы управления не включайте методы обратного вызова, но если вы включили эту функцию в элементе управления, его следует отключать.</span><span class="sxs-lookup"><span data-stu-id="01f32-155">By default, controls do not enable callback methods, but if you enabled this feature in a control, you should disable it.</span></span>

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a><span data-ttu-id="01f32-156">Определение возможностей браузера</span><span class="sxs-lookup"><span data-stu-id="01f32-156">Browser capability detection</span></span>

<span data-ttu-id="01f32-157">Рекомендация: Прекратить использование статических обозревателя возможности обнаружения и вместо этого используйте функцию динамического обнаружения.</span><span class="sxs-lookup"><span data-stu-id="01f32-157">Recommendation: Stop using static browser capability detection, and instead use dynamic feature detection.</span></span>

<span data-ttu-id="01f32-158">В более ранних версиях ASP.NET поддерживаемых функций для каждого браузера, хранятся в XML-файл.</span><span class="sxs-lookup"><span data-stu-id="01f32-158">In earlier versions of ASP.NET, the supported features for each browser were stored in an XML file.</span></span> <span data-ttu-id="01f32-159">Обнаружение событий поддержка функций через статический уточняющего запроса не лучшим подходом.</span><span class="sxs-lookup"><span data-stu-id="01f32-159">Detecting feature support through a static lookup is not the best approach.</span></span> <span data-ttu-id="01f32-160">Теперь вы можете динамически обнаружить браузер поддерживаемых возможностей с помощью платформы обнаружение компонентов, таких как [Modernizr](http://modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="01f32-160">Now, you can dynamically detect a browser's supported features by using a feature detection framework, such as [Modernizr](http://modernizr.com/).</span></span> <span data-ttu-id="01f32-161">Функция обнаружения определяет поддержки, попытка использовать метод или свойство, а затем щелкните для просмотра, если браузер создаваться нужного результата.</span><span class="sxs-lookup"><span data-stu-id="01f32-161">Feature detection determines support by attempting to use a method or property and then checking to see if the browser produced the desired result.</span></span> <span data-ttu-id="01f32-162">По умолчанию Modernizr включается в шаблоны веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="01f32-162">By default, Modernizr is included in the Web application templates.</span></span>

<a id="security"></a>

## <a name="security"></a><span data-ttu-id="01f32-163">Безопасность</span><span class="sxs-lookup"><span data-stu-id="01f32-163">Security</span></span>

<a id="validation"></a>

### <a name="request-validation"></a><span data-ttu-id="01f32-164">Проверка запросов</span><span class="sxs-lookup"><span data-stu-id="01f32-164">Request validation</span></span>

<span data-ttu-id="01f32-165">Рекомендация: Проверки пользовательского ввода и кодирования выходных данных от пользователей.</span><span class="sxs-lookup"><span data-stu-id="01f32-165">Recommendation: Validate user input, and encode output from users.</span></span>

<span data-ttu-id="01f32-166">Проверка запросов — это функция ASP.NET, которое проверяет каждый запрос и останавливает запрос, если найден предполагаемых угроз.</span><span class="sxs-lookup"><span data-stu-id="01f32-166">Request validation is a feature of ASP.NET that inspects each request and stops the request if a perceived threat is found.</span></span> <span data-ttu-id="01f32-167">Не зависят от проверки запросов для защиты приложения от атак с использованием межузловых сценариев.</span><span class="sxs-lookup"><span data-stu-id="01f32-167">Do not depend on request validation for securing your application against cross-site scripting attacks.</span></span> <span data-ttu-id="01f32-168">Вместо этого проверяйте весь ввод от пользователей и кодирования выходных данных.</span><span class="sxs-lookup"><span data-stu-id="01f32-168">Instead, validate all input from users and encode the output.</span></span> <span data-ttu-id="01f32-169">В некоторых случаях можно использовать регулярные выражения для проверки входных данных, но в более сложных случаях следует проверять входные данные пользователя с помощью классов .NET, которые определяют, если значение соответствует допустимые значения.</span><span class="sxs-lookup"><span data-stu-id="01f32-169">In some limited cases, you can use regular expressions to validate the input, but in more complicated cases you should validate user input by using .NET classes that determine if the value matches allowed values.</span></span>

<span data-ttu-id="01f32-170">В следующем примере показано, как использовать статический метод в класс Uri, чтобы определить, допустим ли Uri, предоставленное пользователем.</span><span class="sxs-lookup"><span data-stu-id="01f32-170">The following example shows how to use a static method in the Uri class to determine whether the Uri provided by a user is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

<span data-ttu-id="01f32-171">Тем не менее, чтобы проверить, достаточно Uri, следует также проверить чтобы убедиться в том, он указывает `http` или `https`.</span><span class="sxs-lookup"><span data-stu-id="01f32-171">However, to sufficiently verify the Uri, you should also check to make sure it specifies `http` or `https`.</span></span> <span data-ttu-id="01f32-172">В следующем примере методы экземпляра убедитесь, что Uri является допустимым.</span><span class="sxs-lookup"><span data-stu-id="01f32-172">The following example uses instance methods to verify that the Uri is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

<span data-ttu-id="01f32-173">Перед отображением введенных пользователем данных как HTML или включая пользовательский ввод в SQL-запросе, закодируйте значения, чтобы убедиться, что вредоносный код не включается.</span><span class="sxs-lookup"><span data-stu-id="01f32-173">Before rendering user input as HTML or including user input in a SQL query, encode the values to ensure malicious code is not included.</span></span>

<span data-ttu-id="01f32-174">Вы можете HTML кодирования значения в разметке, с &lt;%: %&gt; синтаксис, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="01f32-174">You can HTML encode the value in markup with the &lt;%: %&gt; syntax, as shown below.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

<span data-ttu-id="01f32-175">В синтаксисе Razor, вы также можете HTML кодирования с @, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="01f32-175">Or, in Razor syntax, you can HTML encode with @, as shown below.</span></span>

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="01f32-176">В следующем примере показано как в виде HTML кодировать значение, которое в коде.</span><span class="sxs-lookup"><span data-stu-id="01f32-176">The next example shows how to HTML encode a value in code-behind.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

<span data-ttu-id="01f32-177">Чтобы безопасно кодировать значение для команды SQL, используйте параметры команды, такие как [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span><span class="sxs-lookup"><span data-stu-id="01f32-177">To safely encode a value for SQL commands, use command parameters such as the [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span></span> <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a><span data-ttu-id="01f32-178">Аутентификация с помощью форм без поддержки файлов cookie и сеансов</span><span class="sxs-lookup"><span data-stu-id="01f32-178">Cookieless forms authentication and session</span></span>

<span data-ttu-id="01f32-179">Рекомендация: Требуются файлы cookie.</span><span class="sxs-lookup"><span data-stu-id="01f32-179">Recommendation: Require cookies.</span></span>

<span data-ttu-id="01f32-180">Передавая сведения о проверке подлинности в строке запроса не является безопасным.</span><span class="sxs-lookup"><span data-stu-id="01f32-180">Passing authentication information in the query string is not secure.</span></span> <span data-ttu-id="01f32-181">Таким образом требуется файлы cookie, если приложение включает проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="01f32-181">Therefore, require cookies when your application includes authentication.</span></span> <span data-ttu-id="01f32-182">Если файлы cookie с компьютера хранятся конфиденциальные сведения, рассмотрите возможность обязательное использование SSL для файла cookie.</span><span class="sxs-lookup"><span data-stu-id="01f32-182">If your cookie stores sensitive information, consider requiring SSL for the cookie.</span></span>

<span data-ttu-id="01f32-183">В следующем примере показано, как укажите в файле Web.config, что аутентификация с помощью форм требуется файл cookie, который передается по протоколу SSL.</span><span class="sxs-lookup"><span data-stu-id="01f32-183">The following example shows how to specify in the Web.config file that Forms Authentication requires a cookie that is transmitted over SSL.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a><span data-ttu-id="01f32-184">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="01f32-184">EnableViewStateMac</span></span>

<span data-ttu-id="01f32-185">Рекомендация: Никогда не задано значение false.</span><span class="sxs-lookup"><span data-stu-id="01f32-185">Recommendation: Never set to false.</span></span>

<span data-ttu-id="01f32-186">По умолчанию имеет значение EnbableViewStateMac значение true.</span><span class="sxs-lookup"><span data-stu-id="01f32-186">By default, EnbableViewStateMac is set to true.</span></span> <span data-ttu-id="01f32-187">Даже если приложение не использует состояние просмотра, не устанавливайте свойство EnableViewStateMac значение false.</span><span class="sxs-lookup"><span data-stu-id="01f32-187">Even if your application is not using view state, do not set EnableViewStateMac to false.</span></span> <span data-ttu-id="01f32-188">Значение false делает приложение уязвимым для межузловых сценариев.</span><span class="sxs-lookup"><span data-stu-id="01f32-188">Setting this value to false will make your application vulnerable to cross-site scripting.</span></span>

<span data-ttu-id="01f32-189">Начиная с ASP.NET 4.5.2, среда выполнения обеспечивает **EnableViewStateMac = true**.</span><span class="sxs-lookup"><span data-stu-id="01f32-189">Starting with ASP.NET 4.5.2, the runtime enforces **EnableViewStateMac=true**.</span></span> <span data-ttu-id="01f32-190">Даже если ему присвоено значение false, среда выполнения игнорирует это значение и выполняет заданное значение в значение true.</span><span class="sxs-lookup"><span data-stu-id="01f32-190">Even if you set it to false, the runtime ignores this value and proceeds with the value set to true.</span></span> <span data-ttu-id="01f32-191">Дополнительные сведения см. в разделе [ASP.NET 4.5.2 и EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span><span class="sxs-lookup"><span data-stu-id="01f32-191">For more information, see [ASP.NET 4.5.2 and EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span></span>

<span data-ttu-id="01f32-192">В следующем примере показано, как присвоено значение true, свойство EnableViewStateMac.</span><span class="sxs-lookup"><span data-stu-id="01f32-192">The following example shows how to set EnableViewStateMac to true.</span></span> <span data-ttu-id="01f32-193">Необходимо непосредственно задается значение true, поскольку он имеет значение true, по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="01f32-193">You do not need to actually set this value to true because it is true by default.</span></span> <span data-ttu-id="01f32-194">Тем не менее если вы задали ее значение false на любой странице в приложении, необходимо немедленно исправить это значение.</span><span class="sxs-lookup"><span data-stu-id="01f32-194">However, if you have set it to false on any page in your application, you must immediately correct this value.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a><span data-ttu-id="01f32-195">Средний уровень доверия</span><span class="sxs-lookup"><span data-stu-id="01f32-195">Medium trust</span></span>

<span data-ttu-id="01f32-196">Рекомендация: Не зависят от средний уровень доверия (или любой другой уровень доверия) как границу безопасности.</span><span class="sxs-lookup"><span data-stu-id="01f32-196">Recommendation: Do not depend on Medium Trust (or any other trust level) as a security boundary.</span></span>

<span data-ttu-id="01f32-197">Частичное доверие не обеспечить адекватную защиту приложения и не должны использоваться.</span><span class="sxs-lookup"><span data-stu-id="01f32-197">Partial trust does not adequately protect your application and should not be used.</span></span> <span data-ttu-id="01f32-198">Вместо этого используйте полное доверие и изолировать ненадежных приложений в отдельных пулах приложений.</span><span class="sxs-lookup"><span data-stu-id="01f32-198">Instead, use Full Trust, and isolate untrusted applications in separate application pools.</span></span> <span data-ttu-id="01f32-199">Кроме того запустите каждый пул приложений с уникальным удостоверением.</span><span class="sxs-lookup"><span data-stu-id="01f32-199">Also, run each application pool under a unique identity.</span></span> <span data-ttu-id="01f32-200">Дополнительные сведения см. в разделе [частичного доверия в ASP.NET не обеспечивает изоляцию приложения](https://support.microsoft.com/kb/2698981).</span><span class="sxs-lookup"><span data-stu-id="01f32-200">For more information, see [ASP.NET Partial Trust does not guarantee application isolation](https://support.microsoft.com/kb/2698981).</span></span>

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a><span data-ttu-id="01f32-201">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="01f32-201">&lt;appSettings&gt;</span></span>

<span data-ttu-id="01f32-202">Рекомендация: Не отключайте параметры безопасности в &lt;appSettings&gt; элемент.</span><span class="sxs-lookup"><span data-stu-id="01f32-202">Recommendation: Do not disable security settings in &lt;appSettings&gt; element.</span></span>

<span data-ttu-id="01f32-203">Элемент appSettings содержит много значений, которые необходимы для обновления для системы безопасности.</span><span class="sxs-lookup"><span data-stu-id="01f32-203">The appSettings element contains many values which are required for security updates.</span></span> <span data-ttu-id="01f32-204">Нельзя изменить или отключить эти значения.</span><span class="sxs-lookup"><span data-stu-id="01f32-204">You should not change or disable these values.</span></span> <span data-ttu-id="01f32-205">Если необходимо отключить эти значения при развертывании обновлений, сразу же повторно включите после завершения развертывания.</span><span class="sxs-lookup"><span data-stu-id="01f32-205">If you must disable these values when deploying an update, immediately re-enable after completing deployment.</span></span>

<span data-ttu-id="01f32-206">Дополнительные сведения см. в разделе [ASP.NET: элемент appSettings](https://msdn.microsoft.com/library/hh975440.aspx).</span><span class="sxs-lookup"><span data-stu-id="01f32-206">For details, see [ASP.NET appSettings Element](https://msdn.microsoft.com/library/hh975440.aspx).</span></span>

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a><span data-ttu-id="01f32-207">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="01f32-207">UrlPathEncode</span></span>

<span data-ttu-id="01f32-208">Рекомендация: Используйте [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) вместо этого.</span><span class="sxs-lookup"><span data-stu-id="01f32-208">Recommendation: Use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) instead.</span></span>

<span data-ttu-id="01f32-209">Метод UrlPathEncode была добавлена в .NET Framework для разрешения проблем совместимости очень конкретного браузера.</span><span class="sxs-lookup"><span data-stu-id="01f32-209">The UrlPathEncode method was added to the .NET Framework to resolve a very specific browser compatibility problem.</span></span> <span data-ttu-id="01f32-210">Не адекватно кодирования URL-адрес и не обеспечивает дополнительную защиту от межузловых сценариев.</span><span class="sxs-lookup"><span data-stu-id="01f32-210">It does not adequately encode a URL, and does not protect your application from cross-site scripting.</span></span> <span data-ttu-id="01f32-211">Никогда не следует использовать его в приложении.</span><span class="sxs-lookup"><span data-stu-id="01f32-211">You should never use it in your application.</span></span> <span data-ttu-id="01f32-212">Вместо этого используйте [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span><span class="sxs-lookup"><span data-stu-id="01f32-212">Instead, use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span></span>

<span data-ttu-id="01f32-213">В следующем примере показано, как для передачи закодированный URL-адрес в качестве параметра строки запроса для элемента управления hyperlink.</span><span class="sxs-lookup"><span data-stu-id="01f32-213">The following example shows how to pass an encoded URL as a query string parameter for a hyperlink control.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a><span data-ttu-id="01f32-214">Надежность и производительность</span><span class="sxs-lookup"><span data-stu-id="01f32-214">Reliability and performance</span></span>

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a><span data-ttu-id="01f32-215">PreSendRequestHeaders и PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="01f32-215">PreSendRequestHeaders and PreSendRequestContent</span></span>

<span data-ttu-id="01f32-216">Рекомендация: Не используйте эти события с помощью управляемых модулей.</span><span class="sxs-lookup"><span data-stu-id="01f32-216">Recommendation: Do not use these events with managed modules.</span></span> <span data-ttu-id="01f32-217">Вместо этого можно напишите собственный модуль IIS для выполнения требуемой задачи.</span><span class="sxs-lookup"><span data-stu-id="01f32-217">Instead, write a native IIS module to perform the required task.</span></span> <span data-ttu-id="01f32-218">См. в разделе [создания машинного кода HTTP-модулей](https://msdn.microsoft.com/library/ms693629.aspx).</span><span class="sxs-lookup"><span data-stu-id="01f32-218">See [Creating Native-Code HTTP Modules](https://msdn.microsoft.com/library/ms693629.aspx).</span></span>

<span data-ttu-id="01f32-219">Можно использовать [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) и [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) событий с помощью собственных модулей IIS.</span><span class="sxs-lookup"><span data-stu-id="01f32-219">You can use the [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) and [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) events with native IIS modules.</span></span>
> [!WARNING]
> <span data-ttu-id="01f32-220">Не используйте `PreSendRequestHeaders` и `PreSendRequestContent` с управляемые модули, которые реализуют `IHttpModule`.</span><span class="sxs-lookup"><span data-stu-id="01f32-220">Do not use `PreSendRequestHeaders` and `PreSendRequestContent` with managed modules that implement `IHttpModule`.</span></span> <span data-ttu-id="01f32-221">Настройка этих свойств может вызвать проблемы с асинхронных запросов.</span><span class="sxs-lookup"><span data-stu-id="01f32-221">Setting these properties can cause issues with asynchronous requests.</span></span> <span data-ttu-id="01f32-222">Сочетание маршрутизации запрошенных приложений (ARR) и websockets может привести к исключения нарушения прав доступа, которые могут вызвать w3wp к сбою в работе.</span><span class="sxs-lookup"><span data-stu-id="01f32-222">The combination of Application Requested Routing (ARR) and websockets might lead to access violation exceptions that can cause w3wp to crash.</span></span> <span data-ttu-id="01f32-223">Например, iiscore! W3_CONTEXT_BASE::GetIsLastNotification + 68 в iiscore.dll вызвала нарушение прав доступа (0xC0000005).</span><span class="sxs-lookup"><span data-stu-id="01f32-223">For example, iiscore!W3_CONTEXT_BASE::GetIsLastNotification+68 in iiscore.dll has caused an access violation exception (0xC0000005).</span></span>

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a><span data-ttu-id="01f32-224">События асинхронной страницы с веб-форм</span><span class="sxs-lookup"><span data-stu-id="01f32-224">Asynchronous page events with web forms</span></span>

<span data-ttu-id="01f32-225">Рекомендация: В веб-формы, избежать написания асинхронные методы, возвращающие void, событий жизненного цикла страницы и вместо этого использовать [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) для асинхронного кода.</span><span class="sxs-lookup"><span data-stu-id="01f32-225">Recommendation: In Web Forms, avoid writing async void methods for Page lifecycle events, and instead use [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) for asynchronous code.</span></span>

<span data-ttu-id="01f32-226">Если пометить событие страницы с **async** и **void**, не может определить, когда асинхронный код будет завершено.</span><span class="sxs-lookup"><span data-stu-id="01f32-226">When you mark a page event with **async** and **void**, you cannot determine when the asynchronous code has finished.</span></span> <span data-ttu-id="01f32-227">Вместо этого используйте Page.RegisterAsyncTask для выполнения асинхронного кода способом, который позволяет отслеживать ее завершения.</span><span class="sxs-lookup"><span data-stu-id="01f32-227">Instead, use Page.RegisterAsyncTask to run the asynchronous code in a way that enables you to track its completion.</span></span>

<span data-ttu-id="01f32-228">В следующем примере показан кнопки щелкните обработчика, который содержит асинхронного кода.</span><span class="sxs-lookup"><span data-stu-id="01f32-228">The following example shows a button click handler that contains asynchronous code.</span></span> <span data-ttu-id="01f32-229">Этот пример включает чтения строковое значение, предоставляемое только как упрощенный пример асинхронной задачи, а не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="01f32-229">This example includes reading a string value asynchronously, which is provided only as a simplified example of an asynchronous task and not as a recommended practice.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

<span data-ttu-id="01f32-230">При использовании асинхронных задач, задать целевую платформу среды выполнения Http для 4.5 (или более поздней версии) в файле Web.config.</span><span class="sxs-lookup"><span data-stu-id="01f32-230">If you are using asynchronous Tasks, set the Http runtime target framework to 4.5 (or later) in the Web.config file.</span></span> <span data-ttu-id="01f32-231">Настройке требуемой версии .NET framework 4.5 включает на новый контекст синхронизации, была добавлена в .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="01f32-231">Setting the target framework to 4.5 turns on the new synchronization context that was added in .NET 4.5.</span></span> <span data-ttu-id="01f32-232">Это значение по умолчанию в новых проектах в Visual Studio, но не была настроена, если вы работаете с существующим проектом.</span><span class="sxs-lookup"><span data-stu-id="01f32-232">This value is set by default in new projects in Visual Studio, but is not be set if you are working with an existing project.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a><span data-ttu-id="01f32-233">Выстрелил и забыл работы</span><span class="sxs-lookup"><span data-stu-id="01f32-233">Fire-and-forget work</span></span>

<span data-ttu-id="01f32-234">Рекомендация: При обработке запроса в ASP.NET, избегайте запуска выстрелил и забыл рабочих (такой вызов метода ThreadPool.QueueUserWorkItem или создания таймера, который многократно вызывает делегат).</span><span class="sxs-lookup"><span data-stu-id="01f32-234">Recommendation: When handling a request within ASP.NET, avoid launching fire-and-forget work (such calling the ThreadPool.QueueUserWorkItem method or creating a timer that repeatedly calls a delegate).</span></span>

<span data-ttu-id="01f32-235">Если приложение имеет выстрелил и забыл работы, которая выполняется в ASP.NET, приложения могут стать асинхронными. В любое время домен приложения можно уничтожить это означает, что ваш непрерывный процесс больше не соответствует текущее состояние приложения.</span><span class="sxs-lookup"><span data-stu-id="01f32-235">If your application has fire-and-forget work that runs within ASP.NET, your application can get out of sync. At any time, the app domain can be destroyed which means your ongoing process may no longer match the current state of the application.</span></span>

<span data-ttu-id="01f32-236">Перенос такого рода работу за пределами ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="01f32-236">You should move this type of work outside of ASP.NET.</span></span> <span data-ttu-id="01f32-237">Можно использовать веб-задания, службы Windows или рабочей роли в Azure для выполнения текущей работы и выполните этот код от другого процесса.</span><span class="sxs-lookup"><span data-stu-id="01f32-237">You can use a Web Jobs, Windows Service or a Worker role in Azure to perform ongoing work, and run that code from another process.</span></span>

<span data-ttu-id="01f32-238">Если необходимо выполнить эту работу в ASP.NET, можно добавить пакет Nuget [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) для выполнения кода.</span><span class="sxs-lookup"><span data-stu-id="01f32-238">If you must perform this work within ASP.NET, you can add the Nuget package called [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) to run the code.</span></span>

<a id="requestentity"></a>

### <a name="request-entity-body"></a><span data-ttu-id="01f32-239">Тело сущности запроса</span><span class="sxs-lookup"><span data-stu-id="01f32-239">Request entity body</span></span>

<span data-ttu-id="01f32-240">Рекомендация: Избегайте чтения Request.Form или Request.InputStream перед выполнением обработчика событий.</span><span class="sxs-lookup"><span data-stu-id="01f32-240">Recommendation: Avoid reading Request.Form or Request.InputStream before the handler's execute event.</span></span>

<span data-ttu-id="01f32-241">Это самые ранние, которые следует прочитать из Request.Form или Request.InputStream во время данного обработчика, выполняют событий.</span><span class="sxs-lookup"><span data-stu-id="01f32-241">The earliest you should read from Request.Form or Request.InputStream is during the handler's execute event.</span></span> <span data-ttu-id="01f32-242">В MVC контроллер является обработчиком, и событие выполнения при выполнении метода.</span><span class="sxs-lookup"><span data-stu-id="01f32-242">In MVC, the Controller is the handler and the execute event is when the action method runs.</span></span> <span data-ttu-id="01f32-243">В веб-формы страницы является обработчиком, а событие выполнения — при возникновении события Page.Init.</span><span class="sxs-lookup"><span data-stu-id="01f32-243">In Web Forms, the Page is the handler and the execute event is when the Page.Init event fires.</span></span> <span data-ttu-id="01f32-244">Если более ранних, чем событие выполнения чтения тела сущности запроса, вам мешать обработки запроса.</span><span class="sxs-lookup"><span data-stu-id="01f32-244">If you read the request entity body earlier than the execute event, you interfere with the processing of the request.</span></span>

<span data-ttu-id="01f32-245">Если вам нужно прочитать тело сущности запроса, прежде чем событие выполнения, используйте либо [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) или [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span><span class="sxs-lookup"><span data-stu-id="01f32-245">If you need to read the request entity body before the execute event, use either [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) or [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span></span> <span data-ttu-id="01f32-246">При использовании GetBufferlessInputStream, вы получаете исходный поток из запроса и принимают на себя ответственность за обработку всего запроса.</span><span class="sxs-lookup"><span data-stu-id="01f32-246">When you use GetBufferlessInputStream, you get the raw stream from the request, and assume responsibility for processing the entire request.</span></span> <span data-ttu-id="01f32-247">После вызова GetBufferlessInputStream, Request.Form и Request.InputStream недоступны, так как они не заполнены в ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="01f32-247">After calling GetBufferlessInputStream, Request.Form and Request.InputStream are not available because they have not been populated by ASP.NET.</span></span> <span data-ttu-id="01f32-248">При использовании GetBufferedInputStream, вы получите копию потока из запроса.</span><span class="sxs-lookup"><span data-stu-id="01f32-248">When you use GetBufferedInputStream, you get a copy of the stream from the request.</span></span> <span data-ttu-id="01f32-249">Request.Form и Request.InputStream по-прежнему доступны позже в запросе, так как ASP.NET заполняет другие копии.</span><span class="sxs-lookup"><span data-stu-id="01f32-249">Request.Form and Request.InputStream are still available later in the request because ASP.NET populates the other copy.</span></span>

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a><span data-ttu-id="01f32-250">Response.Redirect и Response.End</span><span class="sxs-lookup"><span data-stu-id="01f32-250">Response.Redirect and Response.End</span></span>

<span data-ttu-id="01f32-251">Рекомендация: Следует учитывать различия в обработке поток после вызова метода [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span><span class="sxs-lookup"><span data-stu-id="01f32-251">Recommendation: Be aware of differences in how thread is handled after calling [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span></span>

<span data-ttu-id="01f32-252">[Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) метод вызывает метод Response.End.</span><span class="sxs-lookup"><span data-stu-id="01f32-252">The [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) method calls the Response.End method.</span></span> <span data-ttu-id="01f32-253">В синхронном процессе вызов Request.Redirect вызывает немедленно прерывание текущего потока.</span><span class="sxs-lookup"><span data-stu-id="01f32-253">In a synchronous process, calling Request.Redirect causes the current thread to immediately abort.</span></span> <span data-ttu-id="01f32-254">Тем не менее в асинхронном процессе, вызов Response.Redirect не будет прерываться текущего потока, поэтому выполнение кода продолжается для запроса.</span><span class="sxs-lookup"><span data-stu-id="01f32-254">However, in an asynchronous process, calling Response.Redirect does not abort the current thread, so code execution continues for the request.</span></span> <span data-ttu-id="01f32-255">В асинхронный процесс задача должна прекращать метод для остановки выполнения кода.</span><span class="sxs-lookup"><span data-stu-id="01f32-255">In an asynchronous process, you must return the Task from the method to stop the code execution.</span></span>

<span data-ttu-id="01f32-256">В проекте MVC не следует вызывать Response.Redirect.</span><span class="sxs-lookup"><span data-stu-id="01f32-256">In an MVC project, you should not call Response.Redirect.</span></span> <span data-ttu-id="01f32-257">Вместо этого возвращает RedirectResult.</span><span class="sxs-lookup"><span data-stu-id="01f32-257">Instead, return a RedirectResult.</span></span>

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a><span data-ttu-id="01f32-258">EnableViewState и ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="01f32-258">EnableViewState and ViewStateMode</span></span>

<span data-ttu-id="01f32-259">Рекомендация: Используйте ViewStateMode, вместо EnableViewState позволил выяснить, для обеспечения детального управления, по которому элементы управления используют состояние представления.</span><span class="sxs-lookup"><span data-stu-id="01f32-259">Recommendation: Use ViewStateMode, instead of EnableViewState, to provide granular control over which controls use view state.</span></span>

<span data-ttu-id="01f32-260">Если EnableViewState значение false в директиве Page, состояние представления отключено для всех элементов управления на странице и не может быть включен.</span><span class="sxs-lookup"><span data-stu-id="01f32-260">When you set EnableViewState to false in the Page directive, view state is disabled for all controls within the page and cannot be enabled.</span></span> <span data-ttu-id="01f32-261">Если вы хотите включить состояние просмотра только для определенных элементов управления на странице, установлен ViewStateMode отключено на странице.</span><span class="sxs-lookup"><span data-stu-id="01f32-261">If you want to enable view state for only certain controls in your page, set ViewStateMode to Disabled for the Page.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

<span data-ttu-id="01f32-262">Затем задайте ViewStateMode включена только элементов управления, которые фактически должны состояние представления.</span><span class="sxs-lookup"><span data-stu-id="01f32-262">Then, set ViewStateMode to Enabled on only the controls that actually need view state.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

<span data-ttu-id="01f32-263">Включение состояния представления только для элементов управления, которым она необходима, вы можете уменьшить размер состояния представления для веб-страниц.</span><span class="sxs-lookup"><span data-stu-id="01f32-263">By enabling view state for only the controls that need it, you can shrink the size of the view state for your web pages.</span></span>

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a><span data-ttu-id="01f32-264">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="01f32-264">SqlMembershipProvider</span></span>

<span data-ttu-id="01f32-265">Рекомендация: Используйте универсальные поставщики.</span><span class="sxs-lookup"><span data-stu-id="01f32-265">Recommendation: Use Universal Providers.</span></span>

<span data-ttu-id="01f32-266">В текущем шаблоны проектов, SqlMembershipProvider был заменен классом [универсальные поставщики ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.Providers), которая доступна как пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="01f32-266">In the current project templates, SqlMembershipProvider has been replaced by [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), which is available as a NuGet package.</span></span> <span data-ttu-id="01f32-267">Если вы используете SqlMembershipProvider в проект, который был построен с более ранней версии шаблонов, необходимо переключиться на универсальные поставщики.</span><span class="sxs-lookup"><span data-stu-id="01f32-267">If you are using SqlMembershipProvider in a project that was built with an earlier version of the templates, you should switch to Universal Providers.</span></span> <span data-ttu-id="01f32-268">Универсальные поставщики работать со всеми базами данных, которые поддерживаются платформой Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="01f32-268">The Universal Providers work with all databases that are supported by Entity Framework.</span></span>

<span data-ttu-id="01f32-269">Дополнительные сведения см. в разделе [введение в универсальные поставщики ASP.NET](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span><span class="sxs-lookup"><span data-stu-id="01f32-269">For more information, see [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span></span>

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a><span data-ttu-id="01f32-270">Длительные запросы (> 110 секунд)</span><span class="sxs-lookup"><span data-stu-id="01f32-270">Long-running requests (>110 seconds)</span></span>

<span data-ttu-id="01f32-271">Рекомендация: Используйте [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) или [SignalR](../../../signalr/index.md) для подключенных клиентов и используйте асинхронные операции ввода-вывода.</span><span class="sxs-lookup"><span data-stu-id="01f32-271">Recommendation: Use [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) or [SignalR](../../../signalr/index.md) for connected clients, and use asynchronous I/O operations.</span></span>

<span data-ttu-id="01f32-272">Длительных запросов может привести к непредсказуемым результатам и снижению производительности веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="01f32-272">Long-running requests can cause unpredictable results and poor performance in your web application.</span></span> <span data-ttu-id="01f32-273">По умолчанию время ожидания для запроса установлено значение 110 секунд.</span><span class="sxs-lookup"><span data-stu-id="01f32-273">The default timeout setting for a request is 110 seconds.</span></span> <span data-ttu-id="01f32-274">При использовании состояния сеанса с помощью выполняющейся длительное время запроса, ASP.NET будет снимать блокировку для объекта сеанса через 110 секунд.</span><span class="sxs-lookup"><span data-stu-id="01f32-274">If you are using session state with a long-running request, ASP.NET will release the lock on the Session object after 110 seconds.</span></span> <span data-ttu-id="01f32-275">Тем не менее приложение может выполнять другие операции в объекте сеанса, когда блокировка снимается, и не может выполнить операцию.</span><span class="sxs-lookup"><span data-stu-id="01f32-275">However, your application might be in the middle of an operation on the Session object when the lock is released, and the operation might not complete successfully.</span></span> <span data-ttu-id="01f32-276">Если второй запрос от пользователя блокируется, пока первый запрос выполняется, второй запрос может получить доступ к объект сеанса в несогласованном состоянии.</span><span class="sxs-lookup"><span data-stu-id="01f32-276">If a second request from the user is blocked while the first request is running, the second request might access the Session object in an inconsistent state.</span></span>

<span data-ttu-id="01f32-277">Если приложение включает блокировки (или синхронный) операций ввода-вывода, приложение будет отвечать на запросы.</span><span class="sxs-lookup"><span data-stu-id="01f32-277">If your application includes blocking (or synchronous) I/O operations, the application will be unresponsive.</span></span>

<span data-ttu-id="01f32-278">Чтобы повысить производительность, используйте асинхронные операции ввода-вывода в .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="01f32-278">To improve performance, use the asynchronous I/O operations in the .NET Framework.</span></span> <span data-ttu-id="01f32-279">Кроме того при подключении клиентов к серверу используете доступа WebSocket или SignalR.</span><span class="sxs-lookup"><span data-stu-id="01f32-279">Also, use WebSockets or SignalR for connecting clients to the server.</span></span> <span data-ttu-id="01f32-280">Эти функции предназначены для эффективной обработки длительных запросов.</span><span class="sxs-lookup"><span data-stu-id="01f32-280">These features are designed to efficiently handle long-running requests.</span></span>
