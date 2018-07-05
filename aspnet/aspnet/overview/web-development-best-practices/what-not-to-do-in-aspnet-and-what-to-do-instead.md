---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: Чего не следует делать в ASP.NET и что нужно делать вместо этого | Документация Майкрософт
author: tfitzmac
description: В этом разделе описывается несколько распространенных ошибок, которые люди делают в веб-проектов ASP.NET. Представлены рекомендации по что делать, чтобы избежать этих commo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/08/2014
ms.topic: article
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
ms.technology: ''
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: bf46d0b4997d9816071df20fb1884dd76dce8903
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371886"
---
<a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a><span data-ttu-id="21c88-104">Чего не следует делать в ASP.NET и разрешенные действия</span><span class="sxs-lookup"><span data-stu-id="21c88-104">What not to do in ASP.NET, and what to do instead</span></span>
====================
<span data-ttu-id="21c88-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="21c88-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="21c88-106">В этом разделе описывается несколько распространенных ошибок, которые люди делают в веб-проектов ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="21c88-106">This topic describes several common mistakes people make within ASP.NET web projects.</span></span> <span data-ttu-id="21c88-107">Представлены рекомендации по что делать, чтобы избежать этих распространенных ошибок.</span><span class="sxs-lookup"><span data-stu-id="21c88-107">It provides recommendations for what you should do to avoid these common mistakes.</span></span> <span data-ttu-id="21c88-108">Он основан на [презентации](http://vimeo.com/68390507) по **Damian Edwards** на конференции Norwegian Developers Conference.</span><span class="sxs-lookup"><span data-stu-id="21c88-108">It is based on a [presentation](http://vimeo.com/68390507) by **Damian Edwards** at Norwegian Developers Conference.</span></span>


## <a name="disclaimer"></a><span data-ttu-id="21c88-109">Отказ от ответственности</span><span class="sxs-lookup"><span data-stu-id="21c88-109">Disclaimer</span></span>

<span data-ttu-id="21c88-110">В этом разделе не предназначен как полное руководство по, чтобы убедиться, что приложение является безопасной и эффективной.</span><span class="sxs-lookup"><span data-stu-id="21c88-110">This topic is not intended as a complete guide to ensure your application is secure and efficient.</span></span> <span data-ttu-id="21c88-111">Необходимо по-прежнему следуйте рекомендациям по безопасности и производительности, не описанные в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="21c88-111">You still need to follow best practices for security and performance that are not outlined in this topic.</span></span> <span data-ttu-id="21c88-112">Только способы избежать типичных ошибок, связанных с классами .NET и процессы.</span><span class="sxs-lookup"><span data-stu-id="21c88-112">It only suggests how to avoid common mistakes related to .NET classes and processes.</span></span>

## <a name="overview"></a><span data-ttu-id="21c88-113">Обзор</span><span class="sxs-lookup"><span data-stu-id="21c88-113">Overview</span></span>

<span data-ttu-id="21c88-114">В этом разделе содержатся следующие подразделы.</span><span class="sxs-lookup"><span data-stu-id="21c88-114">This topic contains the following sections:</span></span>

- [<span data-ttu-id="21c88-115">Соответствие стандартам</span><span class="sxs-lookup"><span data-stu-id="21c88-115">Standards Compliance</span></span>](#standards)

    - [<span data-ttu-id="21c88-116">Адаптеры элементов управления</span><span class="sxs-lookup"><span data-stu-id="21c88-116">Control Adapters</span></span>](#adapters)
    - [<span data-ttu-id="21c88-117">Свойства стиля для элементов управления</span><span class="sxs-lookup"><span data-stu-id="21c88-117">Style Properties on Controls</span></span>](#styleprop)
    - [<span data-ttu-id="21c88-118">Страницы и обратные вызовы элемент управления</span><span class="sxs-lookup"><span data-stu-id="21c88-118">Page and Control Callbacks</span></span>](#callback)
    - [<span data-ttu-id="21c88-119">Определение возможностей браузера</span><span class="sxs-lookup"><span data-stu-id="21c88-119">Browser Capability Detection</span></span>](#browsercap)
- [<span data-ttu-id="21c88-120">Безопасность</span><span class="sxs-lookup"><span data-stu-id="21c88-120">Security</span></span>](#security)

    - [<span data-ttu-id="21c88-121">Проверка запросов</span><span class="sxs-lookup"><span data-stu-id="21c88-121">Request Validation</span></span>](#validation)
    - [<span data-ttu-id="21c88-122">Аутентификация с помощью форм без поддержки файлов cookie и сеансов</span><span class="sxs-lookup"><span data-stu-id="21c88-122">Cookieless Forms Authentication and Session</span></span>](#cookieless)
    - [<span data-ttu-id="21c88-123">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="21c88-123">EnableViewStateMac</span></span>](#viewstatemac)
    - [<span data-ttu-id="21c88-124">Средний уровень доверия</span><span class="sxs-lookup"><span data-stu-id="21c88-124">Medium Trust</span></span>](#medium)
    - [<span data-ttu-id="21c88-125">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="21c88-125">&lt;appSettings&gt;</span></span>](#appsettings)
    - [<span data-ttu-id="21c88-126">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="21c88-126">UrlPathEncode</span></span>](#urlpathencode)
- [<span data-ttu-id="21c88-127">Надежность и производительность</span><span class="sxs-lookup"><span data-stu-id="21c88-127">Reliability and Performance</span></span>](#performance)

    - [<span data-ttu-id="21c88-128">PreSendRequestHeaders и PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="21c88-128">PreSendRequestHeaders and PreSendRequestContent</span></span>](#presend)
    - [<span data-ttu-id="21c88-129">События асинхронной страницы с веб-форм</span><span class="sxs-lookup"><span data-stu-id="21c88-129">Asynchronous Page Events with Web Forms</span></span>](#asyncevents)
    - [<span data-ttu-id="21c88-130">Выстрелил и забыл работы</span><span class="sxs-lookup"><span data-stu-id="21c88-130">Fire-and-Forget Work</span></span>](#fire)
    - [<span data-ttu-id="21c88-131">Тело сущности запроса</span><span class="sxs-lookup"><span data-stu-id="21c88-131">Request Entity Body</span></span>](#requestentity)
    - [<span data-ttu-id="21c88-132">Response.Redirect и Response.End</span><span class="sxs-lookup"><span data-stu-id="21c88-132">Response.Redirect and Response.End</span></span>](#redirect)
    - [<span data-ttu-id="21c88-133">EnableViewState и ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="21c88-133">EnableViewState and ViewStateMode</span></span>](#viewstatemode)
    - [<span data-ttu-id="21c88-134">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="21c88-134">SqlMembershipProvider</span></span>](#sqlprovider)
    - [<span data-ttu-id="21c88-135">Длительные запросы, под управлением (> 110 секунд)</span><span class="sxs-lookup"><span data-stu-id="21c88-135">Long Running Requests (>110 seconds)</span></span>](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a><span data-ttu-id="21c88-136">Соответствие стандартам</span><span class="sxs-lookup"><span data-stu-id="21c88-136">Standards Compliance</span></span>

<a id="adapters"></a>

### <a name="control-adapters"></a><span data-ttu-id="21c88-137">Адаптеры элементов управления</span><span class="sxs-lookup"><span data-stu-id="21c88-137">Control Adapters</span></span>

<span data-ttu-id="21c88-138">Рекомендация: Отключить использование адаптеров элементов управления для адаптивной отрисовки, а вместо этого использовать CSS медиа-запросами и соответствующих стандартам HTML.</span><span class="sxs-lookup"><span data-stu-id="21c88-138">Recommendation: Stop using control adapters for adaptive rendering, and instead use CSS media queries and standards-compliant HTML.</span></span>

<span data-ttu-id="21c88-139">Адаптеры элементов управления появились в .NET 2.0 для визуализации представления код, который был настроен для различных устройств и сред.</span><span class="sxs-lookup"><span data-stu-id="21c88-139">Controls Adapters were introduced in .NET 2.0 to render presentation code that was customized for different devices and environments.</span></span> <span data-ttu-id="21c88-140">Теперь это адаптивный отрисовку можно выполнить с помощью CSS и HTML.</span><span class="sxs-lookup"><span data-stu-id="21c88-140">Now, this adaptive rendering can be accomplished with CSS and HTML.</span></span> <span data-ttu-id="21c88-141">Следует прекратить использование адаптеров элементов управления и преобразовать любые существующие адаптеры CSS и HTML.</span><span class="sxs-lookup"><span data-stu-id="21c88-141">You should stop using Control Adapters and convert any existing adapters to CSS and HTML.</span></span>

<span data-ttu-id="21c88-142">Дополнительные сведения см. в разделе [медиа-запросами](http://www.w3.org/TR/css3-mediaqueries/) и [How To: Добавление страниц для мобильных устройств, чтобы ваш веб-форм ASP.NET / приложения MVC](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="21c88-142">For more information, see [Media Queries](http://www.w3.org/TR/css3-mediaqueries/) and [How To: Add Mobile Pages to Your ASP.NET Web Forms / MVC Application](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span></span>

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a><span data-ttu-id="21c88-143">Свойства стиля для элементов управления</span><span class="sxs-lookup"><span data-stu-id="21c88-143">Style Properties on Controls</span></span>

<span data-ttu-id="21c88-144">Рекомендация: Остановить задание значений стиля в разметку элемента управления, а вместо этого задать форматирование значений в таблицах стилей CSS.</span><span class="sxs-lookup"><span data-stu-id="21c88-144">Recommendation: Stop setting style values in the control markup, and instead set formatting values in CSS stylesheets.</span></span>

<span data-ttu-id="21c88-145">Веб-сервера управления содержат множество свойств, которые могут использоваться для задания свойства встроенного стиля.</span><span class="sxs-lookup"><span data-stu-id="21c88-145">Web server controls contain dozens of properties which can be used to set in-line style properties.</span></span> <span data-ttu-id="21c88-146">Например свойства ForeColor задает цвет текста для элемента управления.</span><span class="sxs-lookup"><span data-stu-id="21c88-146">For example, the ForeColor property sets the color of the text for a control.</span></span> <span data-ttu-id="21c88-147">Это можно сделать этот же эффект, более эффективно через таблицы стилей CSS.</span><span class="sxs-lookup"><span data-stu-id="21c88-147">You can accomplish this same effect more efficiently through CSS stylesheets.</span></span> <span data-ttu-id="21c88-148">Таблицы стилей позволяют централизовать значений стиля и не рекомендуется устанавливать эти значения во всем приложении.</span><span class="sxs-lookup"><span data-stu-id="21c88-148">Stylesheets enable you to centralize style values and avoid setting these values throughout your application.</span></span>

<span data-ttu-id="21c88-149">Приведенный ниже показан класс CSS на красный текст наборов.</span><span class="sxs-lookup"><span data-stu-id="21c88-149">The following example shows a CSS class the sets text to red.</span></span>

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

<span data-ttu-id="21c88-150">В следующем примере показано, как динамически применить класс CSS.</span><span class="sxs-lookup"><span data-stu-id="21c88-150">The next example shows how to dynamically apply the CSS class.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a><span data-ttu-id="21c88-151">Страницы и обратные вызовы элемент управления</span><span class="sxs-lookup"><span data-stu-id="21c88-151">Page and Control Callbacks</span></span>

<span data-ttu-id="21c88-152">Рекомендация: Прекратить использование обратных вызовов страниц и элементов управления и вместо этого использовать любой из следующих: AJAX UpdatePanel, MVC методы действий, веб-API и SignalR.</span><span class="sxs-lookup"><span data-stu-id="21c88-152">Recommendation: Stop using page and control callbacks, and instead use any of the following: AJAX, UpdatePanel, MVC action methods, Web API, or SignalR.</span></span>

<span data-ttu-id="21c88-153">В более ранних версиях ASP.NET добавлены методы обратного вызова страницы и элемента управления позволяет обновлять части веб-страницы без обновления всей страницы.</span><span class="sxs-lookup"><span data-stu-id="21c88-153">In earlier versions of ASP.NET, Page and Control callback methods enabled you to update part of the web page without refreshing an entire page.</span></span> <span data-ttu-id="21c88-154">Теперь можно выполнить частичного обновления страницы через [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [веб-API](../../../web-api/index.md) или [SignalR](../../../signalr/index.md).</span><span class="sxs-lookup"><span data-stu-id="21c88-154">You can now accomplish partial-page updates through [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web API](../../../web-api/index.md) or [SignalR](../../../signalr/index.md).</span></span> <span data-ttu-id="21c88-155">Следует остановить с помощью методов обратного вызова, так как они могут вызвать проблемы с помощью удобных URL-адресов и маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="21c88-155">You should stop using callback methods because they can cause issues with friendly URLs and routing.</span></span> <span data-ttu-id="21c88-156">По умолчанию элементы управления не включайте методы обратного вызова, но если вы включили эту функцию в элементе управления, его следует отключать.</span><span class="sxs-lookup"><span data-stu-id="21c88-156">By default, controls do not enable callback methods, but if you enabled this feature in a control, you should disable it.</span></span>

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a><span data-ttu-id="21c88-157">Определение возможностей браузера</span><span class="sxs-lookup"><span data-stu-id="21c88-157">Browser Capability Detection</span></span>

<span data-ttu-id="21c88-158">Рекомендация: Прекратить использование статических обозревателя возможности обнаружения и вместо этого используйте функцию динамического обнаружения.</span><span class="sxs-lookup"><span data-stu-id="21c88-158">Recommendation: Stop using static browser capability detection, and instead use dynamic feature detection.</span></span>

<span data-ttu-id="21c88-159">В более ранних версиях ASP.NET поддерживаемых функций для каждого браузера, хранятся в XML-файл.</span><span class="sxs-lookup"><span data-stu-id="21c88-159">In earlier versions of ASP.NET, the supported features for each browser were stored in an XML file.</span></span> <span data-ttu-id="21c88-160">Обнаружение событий поддержка функций через статический уточняющего запроса не лучшим подходом.</span><span class="sxs-lookup"><span data-stu-id="21c88-160">Detecting feature support through a static lookup is not the best approach.</span></span> <span data-ttu-id="21c88-161">Теперь вы можете динамически обнаружить браузер поддерживаемых возможностей с помощью платформы обнаружение компонентов, таких как [Modernizr](http://modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="21c88-161">Now, you can dynamically detect a browser's supported features by using a feature detection framework, such as [Modernizr](http://modernizr.com/).</span></span> <span data-ttu-id="21c88-162">Функция обнаружения определяет поддержки, попытка использовать метод или свойство, а затем щелкните для просмотра, если браузер создаваться нужного результата.</span><span class="sxs-lookup"><span data-stu-id="21c88-162">Feature detection determines support by attempting to use a method or property and then checking to see if the browser produced the desired result.</span></span> <span data-ttu-id="21c88-163">По умолчанию Modernizr включается в шаблоны веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="21c88-163">By default, Modernizr is included in the Web application templates.</span></span>

<a id="security"></a>

## <a name="security"></a><span data-ttu-id="21c88-164">Безопасность</span><span class="sxs-lookup"><span data-stu-id="21c88-164">Security</span></span>

<a id="validation"></a>

### <a name="request-validation"></a><span data-ttu-id="21c88-165">Проверка запросов</span><span class="sxs-lookup"><span data-stu-id="21c88-165">Request Validation</span></span>

<span data-ttu-id="21c88-166">Рекомендация: Проверки пользовательского ввода и кодирования выходных данных от пользователей.</span><span class="sxs-lookup"><span data-stu-id="21c88-166">Recommendation: Validate user input, and encode output from users.</span></span>

<span data-ttu-id="21c88-167">Проверка запросов — это функция ASP.NET, которое проверяет каждый запрос и останавливает запрос, если найден предполагаемых угроз.</span><span class="sxs-lookup"><span data-stu-id="21c88-167">Request validation is a feature of ASP.NET that inspects each request and stops the request if a perceived threat is found.</span></span> <span data-ttu-id="21c88-168">Не зависят от проверки запросов для защиты приложения от атак с использованием межузловых сценариев.</span><span class="sxs-lookup"><span data-stu-id="21c88-168">Do not depend on request validation for securing your application against cross-site scripting attacks.</span></span> <span data-ttu-id="21c88-169">Вместо этого проверяйте весь ввод от пользователей и кодирования выходных данных.</span><span class="sxs-lookup"><span data-stu-id="21c88-169">Instead, validate all input from users and encode the output.</span></span> <span data-ttu-id="21c88-170">В некоторых случаях можно использовать регулярные выражения для проверки входных данных, но в более сложных случаях следует проверять входные данные пользователя с помощью классов .NET, которые определяют, если значение соответствует допустимые значения.</span><span class="sxs-lookup"><span data-stu-id="21c88-170">In some limited cases, you can use regular expressions to validate the input, but in more complicated cases you should validate user input by using .NET classes that determine if the value matches allowed values.</span></span>

<span data-ttu-id="21c88-171">В следующем примере показано, как использовать статический метод в класс Uri, чтобы определить, допустим ли Uri, предоставленное пользователем.</span><span class="sxs-lookup"><span data-stu-id="21c88-171">The following example shows how to use a static method in the Uri class to determine whether the Uri provided by a user is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

<span data-ttu-id="21c88-172">Тем не менее, чтобы проверить, достаточно Uri, следует также проверить чтобы убедиться в том, он указывает `http` или `https`.</span><span class="sxs-lookup"><span data-stu-id="21c88-172">However, to sufficiently verify the Uri, you should also check to make sure it specifies `http` or `https`.</span></span> <span data-ttu-id="21c88-173">В следующем примере методы экземпляра убедитесь, что Uri является допустимым.</span><span class="sxs-lookup"><span data-stu-id="21c88-173">The following example uses instance methods to verify that the Uri is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

<span data-ttu-id="21c88-174">Перед отображением введенных пользователем данных как HTML или включая пользовательский ввод в SQL-запросе, закодируйте значения, чтобы убедиться, что вредоносный код не включается.</span><span class="sxs-lookup"><span data-stu-id="21c88-174">Before rendering user input as HTML or including user input in a SQL query, encode the values to ensure malicious code is not included.</span></span>

<span data-ttu-id="21c88-175">Вы можете HTML кодирования значения в разметке, с &lt;%: %&gt; синтаксис, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="21c88-175">You can HTML encode the value in markup with the &lt;%: %&gt; syntax, as shown below.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

<span data-ttu-id="21c88-176">В синтаксисе Razor, вы также можете HTML кодирования с @, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="21c88-176">Or, in Razor syntax, you can HTML encode with @, as shown below.</span></span>

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="21c88-177">В следующем примере показано как в виде HTML кодировать значение, которое в коде.</span><span class="sxs-lookup"><span data-stu-id="21c88-177">The next example shows how to HTML encode a value in code-behind.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

<span data-ttu-id="21c88-178">Чтобы безопасно кодировать значение для команды SQL, используйте параметры команды, такие как [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span><span class="sxs-lookup"><span data-stu-id="21c88-178">To safely encode a value for SQL commands, use command parameters such as the [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span></span> <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a><span data-ttu-id="21c88-179">Аутентификация с помощью форм без поддержки файлов cookie и сеансов</span><span class="sxs-lookup"><span data-stu-id="21c88-179">Cookieless Forms Authentication and Session</span></span>

<span data-ttu-id="21c88-180">Рекомендация: Требуется файлы cookie.</span><span class="sxs-lookup"><span data-stu-id="21c88-180">Recommendation: Require cookies.</span></span>

<span data-ttu-id="21c88-181">Передавая сведения о проверке подлинности в строке запроса не является безопасным.</span><span class="sxs-lookup"><span data-stu-id="21c88-181">Passing authentication information in the query string is not secure.</span></span> <span data-ttu-id="21c88-182">Таким образом требуется файлы cookie, если приложение включает проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="21c88-182">Therefore, require cookies when your application includes authentication.</span></span> <span data-ttu-id="21c88-183">Если файлы cookie с компьютера хранятся конфиденциальные сведения, рассмотрите возможность обязательное использование SSL для файла cookie.</span><span class="sxs-lookup"><span data-stu-id="21c88-183">If your cookie stores sensitive information, consider requiring SSL for the cookie.</span></span>

<span data-ttu-id="21c88-184">В следующем примере показано, как укажите в файле Web.config, что аутентификация с помощью форм требуется файл cookie, который передается по протоколу SSL.</span><span class="sxs-lookup"><span data-stu-id="21c88-184">The following example shows how to specify in the Web.config file that Forms Authentication requires a cookie that is transmitted over SSL.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a><span data-ttu-id="21c88-185">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="21c88-185">EnableViewStateMac</span></span>

<span data-ttu-id="21c88-186">Рекомендация: Никогда не задано значение false.</span><span class="sxs-lookup"><span data-stu-id="21c88-186">Recommendation: Never set to false.</span></span>

<span data-ttu-id="21c88-187">По умолчанию имеет значение EnbableViewStateMac значение true.</span><span class="sxs-lookup"><span data-stu-id="21c88-187">By default, EnbableViewStateMac is set to true.</span></span> <span data-ttu-id="21c88-188">Даже если приложение не использует состояние просмотра, не устанавливайте свойство EnableViewStateMac значение false.</span><span class="sxs-lookup"><span data-stu-id="21c88-188">Even if your application is not using view state, do not set EnableViewStateMac to false.</span></span> <span data-ttu-id="21c88-189">Значение false делает приложение уязвимым для межузловых сценариев.</span><span class="sxs-lookup"><span data-stu-id="21c88-189">Setting this value to false will make your application vulnerable to cross-site scripting.</span></span>

<span data-ttu-id="21c88-190">Начиная с ASP.NET 4.5.2, среда выполнения обеспечивает **EnableViewStateMac = true**.</span><span class="sxs-lookup"><span data-stu-id="21c88-190">Starting with ASP.NET 4.5.2, the runtime enforces **EnableViewStateMac=true**.</span></span> <span data-ttu-id="21c88-191">Даже если ему присвоено значение false, среда выполнения игнорирует это значение и выполняет заданное значение в значение true.</span><span class="sxs-lookup"><span data-stu-id="21c88-191">Even if you set it to false, the runtime ignores this value and proceeds with the value set to true.</span></span> <span data-ttu-id="21c88-192">Дополнительные сведения см. в разделе [ASP.NET 4.5.2 и EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span><span class="sxs-lookup"><span data-stu-id="21c88-192">For more information, see [ASP.NET 4.5.2 and EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span></span>

<span data-ttu-id="21c88-193">В следующем примере показано, как присвоено значение true, свойство EnableViewStateMac.</span><span class="sxs-lookup"><span data-stu-id="21c88-193">The following example shows how to set EnableViewStateMac to true.</span></span> <span data-ttu-id="21c88-194">Необходимо непосредственно задается значение true, поскольку он имеет значение true, по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="21c88-194">You do not need to actually set this value to true because it is true by default.</span></span> <span data-ttu-id="21c88-195">Тем не менее если вы задали ее значение false на любой странице в приложении, необходимо немедленно исправить это значение.</span><span class="sxs-lookup"><span data-stu-id="21c88-195">However, if you have set it to false on any page in your application, you must immediately correct this value.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a><span data-ttu-id="21c88-196">Средний уровень доверия</span><span class="sxs-lookup"><span data-stu-id="21c88-196">Medium Trust</span></span>

<span data-ttu-id="21c88-197">Рекомендация: Не полагаться на средний уровень доверия (или любой другой уровень доверия) как границу безопасности.</span><span class="sxs-lookup"><span data-stu-id="21c88-197">Recommendation: Do not depend on Medium Trust (or any other trust level) as a security boundary.</span></span>

<span data-ttu-id="21c88-198">Частичное доверие не обеспечить адекватную защиту приложения и не должны использоваться.</span><span class="sxs-lookup"><span data-stu-id="21c88-198">Partial trust does not adequately protect your application and should not be used.</span></span> <span data-ttu-id="21c88-199">Вместо этого используйте полное доверие и изолировать ненадежных приложений в отдельных пулах приложений.</span><span class="sxs-lookup"><span data-stu-id="21c88-199">Instead, use Full Trust, and isolate untrusted applications in separate application pools.</span></span> <span data-ttu-id="21c88-200">Кроме того запустите каждый пул приложений с уникальным удостоверением.</span><span class="sxs-lookup"><span data-stu-id="21c88-200">Also, run each application pool under a unique identity.</span></span> <span data-ttu-id="21c88-201">Дополнительные сведения см. в разделе [частичного доверия в ASP.NET не обеспечивает изоляцию приложения](https://support.microsoft.com/kb/2698981).</span><span class="sxs-lookup"><span data-stu-id="21c88-201">For more information, see [ASP.NET Partial Trust does not guarantee application isolation](https://support.microsoft.com/kb/2698981).</span></span>

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a><span data-ttu-id="21c88-202">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="21c88-202">&lt;appSettings&gt;</span></span>

<span data-ttu-id="21c88-203">Рекомендация: Не отключайте параметры безопасности в &lt;appSettings&gt; элемент.</span><span class="sxs-lookup"><span data-stu-id="21c88-203">Recommendation: Do not disable security settings in &lt;appSettings&gt; element.</span></span>

<span data-ttu-id="21c88-204">Элемент appSettings содержит много значений, которые необходимы для обновления для системы безопасности.</span><span class="sxs-lookup"><span data-stu-id="21c88-204">The appSettings element contains many values which are required for security updates.</span></span> <span data-ttu-id="21c88-205">Нельзя изменить или отключить эти значения.</span><span class="sxs-lookup"><span data-stu-id="21c88-205">You should not change or disable these values.</span></span> <span data-ttu-id="21c88-206">Если необходимо отключить эти значения при развертывании обновлений, сразу же повторно включите после завершения развертывания.</span><span class="sxs-lookup"><span data-stu-id="21c88-206">If you must disable these values when deploying an update, immediately re-enable after completing deployment.</span></span>

<span data-ttu-id="21c88-207">Дополнительные сведения см. в разделе [ASP.NET: элемент appSettings](https://msdn.microsoft.com/library/hh975440.aspx).</span><span class="sxs-lookup"><span data-stu-id="21c88-207">For details, see [ASP.NET appSettings Element](https://msdn.microsoft.com/library/hh975440.aspx).</span></span>

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a><span data-ttu-id="21c88-208">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="21c88-208">UrlPathEncode</span></span>

<span data-ttu-id="21c88-209">Рекомендация: Используйте [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) вместо этого.</span><span class="sxs-lookup"><span data-stu-id="21c88-209">Recommendation: Use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) instead.</span></span>

<span data-ttu-id="21c88-210">Метод UrlPathEncode была добавлена в .NET Framework для разрешения проблем совместимости очень конкретного браузера.</span><span class="sxs-lookup"><span data-stu-id="21c88-210">The UrlPathEncode method was added to the .NET Framework to resolve a very specific browser compatibility problem.</span></span> <span data-ttu-id="21c88-211">Не адекватно кодирования URL-адрес и не обеспечивает дополнительную защиту от межузловых сценариев.</span><span class="sxs-lookup"><span data-stu-id="21c88-211">It does not adequately encode a URL, and does not protect your application from cross-site scripting.</span></span> <span data-ttu-id="21c88-212">Никогда не следует использовать его в приложении.</span><span class="sxs-lookup"><span data-stu-id="21c88-212">You should never use it in your application.</span></span> <span data-ttu-id="21c88-213">Вместо этого используйте [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span><span class="sxs-lookup"><span data-stu-id="21c88-213">Instead, use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span></span>

<span data-ttu-id="21c88-214">В следующем примере показано, как для передачи закодированный URL-адрес в качестве параметра строки запроса для элемента управления hyperlink.</span><span class="sxs-lookup"><span data-stu-id="21c88-214">The following example shows how to pass an encoded URL as a query string parameter for a hyperlink control.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a><span data-ttu-id="21c88-215">Надежность и производительность</span><span class="sxs-lookup"><span data-stu-id="21c88-215">Reliability and Performance</span></span>

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a><span data-ttu-id="21c88-216">PreSendRequestHeaders и PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="21c88-216">PreSendRequestHeaders and PreSendRequestContent</span></span>

<span data-ttu-id="21c88-217">Рекомендация: Не используйте эти события с помощью управляемых модулей.</span><span class="sxs-lookup"><span data-stu-id="21c88-217">Recommendation: Do not use these events with managed modules.</span></span> <span data-ttu-id="21c88-218">Вместо этого можно напишите собственный модуль IIS для выполнения требуемой задачи.</span><span class="sxs-lookup"><span data-stu-id="21c88-218">Instead, write a native IIS module to perform the required task.</span></span> <span data-ttu-id="21c88-219">См. в разделе [создания машинного кода HTTP-модулей](https://msdn.microsoft.com/library/ms693629.aspx).</span><span class="sxs-lookup"><span data-stu-id="21c88-219">See [Creating Native-Code HTTP Modules](https://msdn.microsoft.com/library/ms693629.aspx).</span></span>

<span data-ttu-id="21c88-220">Можно использовать [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) и [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) событий с помощью собственных модулей IIS.</span><span class="sxs-lookup"><span data-stu-id="21c88-220">You can use the [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) and [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) events with native IIS modules.</span></span>
> [!WARNING]
> <span data-ttu-id="21c88-221">Не используйте `PreSendRequestHeaders` и `PreSendRequestContent` с управляемые модули, которые реализуют `IHttpModule`.</span><span class="sxs-lookup"><span data-stu-id="21c88-221">Do not use `PreSendRequestHeaders` and `PreSendRequestContent` with managed modules that implement `IHttpModule`.</span></span> <span data-ttu-id="21c88-222">Настройка этих свойств может вызвать проблемы с асинхронных запросов.</span><span class="sxs-lookup"><span data-stu-id="21c88-222">Setting these properties can cause issues with asynchronous requests.</span></span> <span data-ttu-id="21c88-223">Сочетание маршрутизации запрошенных приложений (ARR) и websockets может привести к исключения нарушения прав доступа, которые могут вызвать w3wp к сбою в работе.</span><span class="sxs-lookup"><span data-stu-id="21c88-223">The combination of Application Requested Routing (ARR) and websockets might lead to access violation exceptions that can cause w3wp to crash.</span></span> <span data-ttu-id="21c88-224">Например, iiscore! W3_CONTEXT_BASE::GetIsLastNotification + 68 в iiscore.dll вызвала нарушение прав доступа (0xC0000005).</span><span class="sxs-lookup"><span data-stu-id="21c88-224">For example, iiscore!W3_CONTEXT_BASE::GetIsLastNotification+68 in iiscore.dll has caused an access violation exception (0xC0000005).</span></span>

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a><span data-ttu-id="21c88-225">События асинхронной страницы с веб-форм</span><span class="sxs-lookup"><span data-stu-id="21c88-225">Asynchronous Page Events with Web Forms</span></span>

<span data-ttu-id="21c88-226">Рекомендация: Веб-форм, избежать написания асинхронного void методы для событий жизненного цикла страницы и вместо этого использовать [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) для асинхронного кода.</span><span class="sxs-lookup"><span data-stu-id="21c88-226">Recommendation: In Web Forms, avoid writing async void methods for Page lifecycle events, and instead use [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) for asynchronous code.</span></span>

<span data-ttu-id="21c88-227">Если пометить событие страницы с **async** и **void**, не может определить, когда асинхронный код будет завершено.</span><span class="sxs-lookup"><span data-stu-id="21c88-227">When you mark a page event with **async** and **void**, you cannot determine when the asynchronous code has finished.</span></span> <span data-ttu-id="21c88-228">Вместо этого используйте Page.RegisterAsyncTask для выполнения асинхронного кода способом, который позволяет отслеживать ее завершения.</span><span class="sxs-lookup"><span data-stu-id="21c88-228">Instead, use Page.RegisterAsyncTask to run the asynchronous code in a way that enables you to track its completion.</span></span>

<span data-ttu-id="21c88-229">В следующем примере показан кнопки щелкните обработчика, который содержит асинхронного кода.</span><span class="sxs-lookup"><span data-stu-id="21c88-229">The following example shows a button click handler that contains asynchronous code.</span></span> <span data-ttu-id="21c88-230">Этот пример включает чтения строковое значение, предоставляемое только как упрощенный пример асинхронной задачи, а не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="21c88-230">This example includes reading a string value asynchronously, which is provided only as a simplified example of an asynchronous task and not as a recommended practice.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

<span data-ttu-id="21c88-231">При использовании асинхронных задач, задайте целевую платформу среды выполнения Http для 4.5 в файле Web.config.</span><span class="sxs-lookup"><span data-stu-id="21c88-231">If you are using asynchronous Tasks, set the Http runtime target framework to 4.5 in the Web.config file.</span></span> <span data-ttu-id="21c88-232">Настройке требуемой версии .NET framework 4.5 включает на новый контекст синхронизации, была добавлена в .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="21c88-232">Setting the target framework to 4.5 turns on the new synchronization context that was added in .NET 4.5.</span></span> <span data-ttu-id="21c88-233">Это значение по умолчанию в новых проектах в Visual Studio 2012, но не была настроена, если вы работаете с существующим проектом.</span><span class="sxs-lookup"><span data-stu-id="21c88-233">This value is set by default in new projects in Visual Studio 2012, but is not be set if you are working with an existing project.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a><span data-ttu-id="21c88-234">Выстрелил и забыл работы</span><span class="sxs-lookup"><span data-stu-id="21c88-234">Fire-and-Forget Work</span></span>

<span data-ttu-id="21c88-235">Рекомендация: При обработке запроса в ASP.NET, избегайте запуска выстрелил и забыл рабочих (такой вызов метода ThreadPool.QueueUserWorkItem или создания таймера, который многократно вызывает делегат).</span><span class="sxs-lookup"><span data-stu-id="21c88-235">Recommendation: When handling a request within ASP.NET, avoid launching fire-and-forget work (such calling the ThreadPool.QueueUserWorkItem method or creating a timer that repeatedly calls a delegate).</span></span>

<span data-ttu-id="21c88-236">Если приложение имеет выстрелил и забыл работы, которая выполняется в ASP.NET, приложения могут стать асинхронными. В любое время домен приложения можно уничтожить это означает, что ваш непрерывный процесс больше не соответствует текущее состояние приложения.</span><span class="sxs-lookup"><span data-stu-id="21c88-236">If your application has fire-and-forget work that runs within ASP.NET, your application can get out of sync. At any time, the app domain can be destroyed which means your ongoing process may no longer match the current state of the application.</span></span>

<span data-ttu-id="21c88-237">Перенос такого рода работу за пределами ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="21c88-237">You should move this type of work outside of ASP.NET.</span></span> <span data-ttu-id="21c88-238">Можно использовать веб-задания, службы Windows или рабочей роли в Azure для выполнения текущей работы и выполните этот код от другого процесса.</span><span class="sxs-lookup"><span data-stu-id="21c88-238">You can use a Web Jobs, Windows Service or a Worker role in Azure to perform ongoing work, and run that code from another process.</span></span>

<span data-ttu-id="21c88-239">Если необходимо выполнить эту работу в ASP.NET, можно добавить пакет Nuget [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) для выполнения кода.</span><span class="sxs-lookup"><span data-stu-id="21c88-239">If you must perform this work within ASP.NET, you can add the Nuget package called [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) to run the code.</span></span>

<a id="requestentity"></a>

### <a name="request-entity-body"></a><span data-ttu-id="21c88-240">Тело сущности запроса</span><span class="sxs-lookup"><span data-stu-id="21c88-240">Request Entity Body</span></span>

<span data-ttu-id="21c88-241">Рекомендация: Избегайте чтения Request.Form или Request.InputStream перед выполнением обработчика событий.</span><span class="sxs-lookup"><span data-stu-id="21c88-241">Recommendation: Avoid reading Request.Form or Request.InputStream before the handler's execute event.</span></span>

<span data-ttu-id="21c88-242">Это самые ранние, которые следует прочитать из Request.Form или Request.InputStream во время данного обработчика, выполняют событий.</span><span class="sxs-lookup"><span data-stu-id="21c88-242">The earliest you should read from Request.Form or Request.InputStream is during the handler's execute event.</span></span> <span data-ttu-id="21c88-243">В MVC контроллер является обработчиком, и событие выполнения при выполнении метода.</span><span class="sxs-lookup"><span data-stu-id="21c88-243">In MVC, the Controller is the handler and the execute event is when the action method runs.</span></span> <span data-ttu-id="21c88-244">В веб-формы страницы является обработчиком, а событие выполнения — при возникновении события Page.Init.</span><span class="sxs-lookup"><span data-stu-id="21c88-244">In Web Forms, the Page is the handler and the execute event is when the Page.Init event fires.</span></span> <span data-ttu-id="21c88-245">Если более ранних, чем событие выполнения чтения тела сущности запроса, вам мешать обработки запроса.</span><span class="sxs-lookup"><span data-stu-id="21c88-245">If you read the request entity body earlier than the execute event, you interfere with the processing of the request.</span></span>

<span data-ttu-id="21c88-246">Если вам нужно прочитать тело сущности запроса, прежде чем событие выполнения, используйте либо [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) или [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span><span class="sxs-lookup"><span data-stu-id="21c88-246">If you need to read the request entity body before the execute event, use either [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) or [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span></span> <span data-ttu-id="21c88-247">При использовании GetBufferlessInputStream, вы получаете исходный поток из запроса и принимают на себя ответственность за обработку всего запроса.</span><span class="sxs-lookup"><span data-stu-id="21c88-247">When you use GetBufferlessInputStream, you get the raw stream from the request, and assume responsibility for processing the entire request.</span></span> <span data-ttu-id="21c88-248">После вызова GetBufferlessInputStream, Request.Form и Request.InputStream недоступны, так как они не заполнены в ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="21c88-248">After calling GetBufferlessInputStream, Request.Form and Request.InputStream are not available because they have not been populated by ASP.NET.</span></span> <span data-ttu-id="21c88-249">При использовании GetBufferedInputStream, вы получите копию потока из запроса.</span><span class="sxs-lookup"><span data-stu-id="21c88-249">When you use GetBufferedInputStream, you get a copy of the stream from the request.</span></span> <span data-ttu-id="21c88-250">Request.Form и Request.InputStream по-прежнему доступны позже в запросе, так как ASP.NET заполняет другие копии.</span><span class="sxs-lookup"><span data-stu-id="21c88-250">Request.Form and Request.InputStream are still available later in the request because ASP.NET populates the other copy.</span></span>

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a><span data-ttu-id="21c88-251">Response.Redirect и Response.End</span><span class="sxs-lookup"><span data-stu-id="21c88-251">Response.Redirect and Response.End</span></span>

<span data-ttu-id="21c88-252">Рекомендация: Следует учитывать различия в обработке поток после вызова метода [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span><span class="sxs-lookup"><span data-stu-id="21c88-252">Recommendation: Be aware of differences in how thread is handled after calling [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span></span>

<span data-ttu-id="21c88-253">[Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) метод вызывает метод Response.End.</span><span class="sxs-lookup"><span data-stu-id="21c88-253">The [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) method calls the Response.End method.</span></span> <span data-ttu-id="21c88-254">В синхронном процессе вызов Request.Redirect вызывает немедленно прерывание текущего потока.</span><span class="sxs-lookup"><span data-stu-id="21c88-254">In a synchronous process, calling Request.Redirect causes the current thread to immediately abort.</span></span> <span data-ttu-id="21c88-255">Тем не менее в асинхронном процессе, вызов Response.Redirect не будет прерываться текущего потока, поэтому выполнение кода продолжается для запроса.</span><span class="sxs-lookup"><span data-stu-id="21c88-255">However, in an asynchronous process, calling Response.Redirect does not abort the current thread, so code execution continues for the request.</span></span> <span data-ttu-id="21c88-256">В асинхронный процесс задача должна прекращать метод для остановки выполнения кода.</span><span class="sxs-lookup"><span data-stu-id="21c88-256">In an asynchronous process, you must return the Task from the method to stop the code execution.</span></span>

<span data-ttu-id="21c88-257">В проекте MVC не следует вызывать Response.Redirect.</span><span class="sxs-lookup"><span data-stu-id="21c88-257">In an MVC project, you should not call Response.Redirect.</span></span> <span data-ttu-id="21c88-258">Вместо этого возвращает RedirectResult.</span><span class="sxs-lookup"><span data-stu-id="21c88-258">Instead, return a RedirectResult.</span></span>

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a><span data-ttu-id="21c88-259">EnableViewState и ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="21c88-259">EnableViewState and ViewStateMode</span></span>

<span data-ttu-id="21c88-260">Рекомендация: ViewStateMode используйте вместо EnableViewState позволил выяснить, для обеспечения детального управления, по которому элементы управления используют состояние представления.</span><span class="sxs-lookup"><span data-stu-id="21c88-260">Recommendation: Use ViewStateMode, instead of EnableViewState, to provide granular control over which controls use view state.</span></span>

<span data-ttu-id="21c88-261">Если EnableViewState значение false в директиве Page, состояние представления отключено для всех элементов управления на странице и не может быть включен.</span><span class="sxs-lookup"><span data-stu-id="21c88-261">When you set EnableViewState to false in the Page directive, view state is disabled for all controls within the page and cannot be enabled.</span></span> <span data-ttu-id="21c88-262">Если вы хотите включить состояние просмотра только для определенных элементов управления на странице, установлен ViewStateMode отключено на странице.</span><span class="sxs-lookup"><span data-stu-id="21c88-262">If you want to enable view state for only certain controls in your page, set ViewStateMode to Disabled for the Page.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

<span data-ttu-id="21c88-263">Затем задайте ViewStateMode включена только элементов управления, которые фактически должны состояние представления.</span><span class="sxs-lookup"><span data-stu-id="21c88-263">Then, set ViewStateMode to Enabled on only the controls that actually need view state.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

<span data-ttu-id="21c88-264">Включение состояния представления только для элементов управления, которым она необходима, вы можете уменьшить размер состояния представления для веб-страниц.</span><span class="sxs-lookup"><span data-stu-id="21c88-264">By enabling view state for only the controls that need it, you can shrink the size of the view state for your web pages.</span></span>

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a><span data-ttu-id="21c88-265">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="21c88-265">SqlMembershipProvider</span></span>

<span data-ttu-id="21c88-266">Рекомендация: Используйте универсальные поставщики.</span><span class="sxs-lookup"><span data-stu-id="21c88-266">Recommendation: Use Universal Providers.</span></span>

<span data-ttu-id="21c88-267">В текущем шаблоны проектов, SqlMembershipProvider был заменен классом [универсальные поставщики ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.Providers), которая доступна как пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="21c88-267">In the current project templates, SqlMembershipProvider has been replaced by [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), which is available as a NuGet package.</span></span> <span data-ttu-id="21c88-268">Если вы используете SqlMembershipProvider в проект, который был построен с более ранней версии шаблонов, необходимо переключиться на универсальные поставщики.</span><span class="sxs-lookup"><span data-stu-id="21c88-268">If you are using SqlMembershipProvider in a project that was built with an earlier version of the templates, you should switch to Universal Providers.</span></span> <span data-ttu-id="21c88-269">Универсальные поставщики работать со всеми базами данных, которые поддерживаются платформой Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="21c88-269">The Universal Providers work with all databases that are supported by Entity Framework.</span></span>

<span data-ttu-id="21c88-270">Дополнительные сведения см. в разделе [введение в универсальные поставщики ASP.NET](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span><span class="sxs-lookup"><span data-stu-id="21c88-270">For more information, see [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span></span>

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a><span data-ttu-id="21c88-271">Длительные запросы (> 110 секунд)</span><span class="sxs-lookup"><span data-stu-id="21c88-271">Long-running Requests (>110 seconds)</span></span>

<span data-ttu-id="21c88-272">Рекомендация: Используйте [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) или [SignalR](../../../signalr/index.md) для подключенных клиентов и используйте асинхронные операции ввода-вывода.</span><span class="sxs-lookup"><span data-stu-id="21c88-272">Recommendation: Use [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) or [SignalR](../../../signalr/index.md) for connected clients, and use asynchronous I/O operations.</span></span>

<span data-ttu-id="21c88-273">Длительных запросов может привести к непредсказуемым результатам и снижению производительности веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="21c88-273">Long-running requests can cause unpredictable results and poor performance in your web application.</span></span> <span data-ttu-id="21c88-274">По умолчанию время ожидания для запроса установлено значение 110 секунд.</span><span class="sxs-lookup"><span data-stu-id="21c88-274">The default timeout setting for a request is 110 seconds.</span></span> <span data-ttu-id="21c88-275">При использовании состояния сеанса с помощью выполняющейся длительное время запроса, ASP.NET будет снимать блокировку для объекта сеанса через 110 секунд.</span><span class="sxs-lookup"><span data-stu-id="21c88-275">If you are using session state with a long-running request, ASP.NET will release the lock on the Session object after 110 seconds.</span></span> <span data-ttu-id="21c88-276">Тем не менее приложение может выполнять другие операции в объекте сеанса, когда блокировка снимается, и не может выполнить операцию.</span><span class="sxs-lookup"><span data-stu-id="21c88-276">However, your application might be in the middle of an operation on the Session object when the lock is released, and the operation might not complete successfully.</span></span> <span data-ttu-id="21c88-277">Если второй запрос от пользователя блокируется, пока первый запрос выполняется, второй запрос может получить доступ к объект сеанса в несогласованном состоянии.</span><span class="sxs-lookup"><span data-stu-id="21c88-277">If a second request from the user is blocked while the first request is running, the second request might access the Session object in an inconsistent state.</span></span>

<span data-ttu-id="21c88-278">Если приложение включает блокировки (или синхронный) операций ввода-вывода, приложение будет отвечать на запросы.</span><span class="sxs-lookup"><span data-stu-id="21c88-278">If your application includes blocking (or synchronous) I/O operations, the application will be unresponsive.</span></span>

<span data-ttu-id="21c88-279">Чтобы повысить производительность, используйте асинхронные операции ввода-вывода в .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="21c88-279">To improve performance, use the asynchronous I/O operations in the .NET Framework.</span></span> <span data-ttu-id="21c88-280">Кроме того при подключении клиентов к серверу используете доступа WebSocket или SignalR.</span><span class="sxs-lookup"><span data-stu-id="21c88-280">Also, use WebSockets or SignalR for connecting clients to the server.</span></span> <span data-ttu-id="21c88-281">Эти функции предназначены для эффективной обработки длительных запросов.</span><span class="sxs-lookup"><span data-stu-id="21c88-281">These features are designed to efficiently handle long-running requests.</span></span>
