---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: Что не делать в ASP.NET и что нужно делать вместо этого | Документы Microsoft
author: tfitzmac
description: В этом разделе описаны некоторые распространенные ошибки, вносить пользователям в рамках веб-проектов ASP.NET. Оно предоставляет рекомендации по что делать, чтобы избежать этих commo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/08/2014
ms.topic: article
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 829f3a024bc15bec8b60b91193ba9bca37b78009
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
ms.locfileid: "28034924"
---
<a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a><span data-ttu-id="1ec74-104">Что не делать в ASP.NET и что нужно делать вместо этого</span><span class="sxs-lookup"><span data-stu-id="1ec74-104">What not to do in ASP.NET, and what to do instead</span></span>
====================
<span data-ttu-id="1ec74-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="1ec74-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="1ec74-106">В этом разделе описаны некоторые распространенные ошибки, вносить пользователям в рамках веб-проектов ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1ec74-106">This topic describes several common mistakes people make within ASP.NET web projects.</span></span> <span data-ttu-id="1ec74-107">Оно предоставляет рекомендации по что делать, чтобы избежать этих распространенных ошибок.</span><span class="sxs-lookup"><span data-stu-id="1ec74-107">It provides recommendations for what you should do to avoid these common mistakes.</span></span> <span data-ttu-id="1ec74-108">Он основан на [презентации](http://vimeo.com/68390507) по **Edwards — Дэмьен** норвежский конференции разработчиков.</span><span class="sxs-lookup"><span data-stu-id="1ec74-108">It is based on a [presentation](http://vimeo.com/68390507) by **Damian Edwards** at Norwegian Developers Conference.</span></span>


## <a name="disclaimer"></a><span data-ttu-id="1ec74-109">Отказ от ответственности</span><span class="sxs-lookup"><span data-stu-id="1ec74-109">Disclaimer</span></span>

<span data-ttu-id="1ec74-110">В этом разделе не предназначен как полное руководство по, чтобы убедиться, что приложение является безопасный и эффективный.</span><span class="sxs-lookup"><span data-stu-id="1ec74-110">This topic is not intended as a complete guide to ensure your application is secure and efficient.</span></span> <span data-ttu-id="1ec74-111">Необходимо по-прежнему следуйте рекомендациям по безопасности и производительности, которые не описаны в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="1ec74-111">You still need to follow best practices for security and performance that are not outlined in this topic.</span></span> <span data-ttu-id="1ec74-112">Предлагаются только как избежать распространенных ошибок, связанных с классами .NET и процессов.</span><span class="sxs-lookup"><span data-stu-id="1ec74-112">It only suggests how to avoid common mistakes related to .NET classes and processes.</span></span>

## <a name="overview"></a><span data-ttu-id="1ec74-113">Обзор</span><span class="sxs-lookup"><span data-stu-id="1ec74-113">Overview</span></span>

<span data-ttu-id="1ec74-114">В этом разделе содержатся следующие подразделы.</span><span class="sxs-lookup"><span data-stu-id="1ec74-114">This topic contains the following sections:</span></span>

- [<span data-ttu-id="1ec74-115">Соответствие стандартам</span><span class="sxs-lookup"><span data-stu-id="1ec74-115">Standards Compliance</span></span>](#standards)

    - [<span data-ttu-id="1ec74-116">Адаптеры элементов управления</span><span class="sxs-lookup"><span data-stu-id="1ec74-116">Control Adapters</span></span>](#adapters)
    - [<span data-ttu-id="1ec74-117">Свойства стиля для элементов управления</span><span class="sxs-lookup"><span data-stu-id="1ec74-117">Style Properties on Controls</span></span>](#styleprop)
    - [<span data-ttu-id="1ec74-118">Страницы и обратные вызовы для элемента управления</span><span class="sxs-lookup"><span data-stu-id="1ec74-118">Page and Control Callbacks</span></span>](#callback)
    - [<span data-ttu-id="1ec74-119">Определение возможностей браузера</span><span class="sxs-lookup"><span data-stu-id="1ec74-119">Browser Capability Detection</span></span>](#browsercap)
- [<span data-ttu-id="1ec74-120">Безопасность</span><span class="sxs-lookup"><span data-stu-id="1ec74-120">Security</span></span>](#security)

    - [<span data-ttu-id="1ec74-121">Проверка запросов</span><span class="sxs-lookup"><span data-stu-id="1ec74-121">Request Validation</span></span>](#validation)
    - [<span data-ttu-id="1ec74-122">Проверка подлинности форм без поддержки файлов cookie и сеансов</span><span class="sxs-lookup"><span data-stu-id="1ec74-122">Cookieless Forms Authentication and Session</span></span>](#cookieless)
    - [<span data-ttu-id="1ec74-123">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="1ec74-123">EnableViewStateMac</span></span>](#viewstatemac)
    - [<span data-ttu-id="1ec74-124">Средний уровень доверия</span><span class="sxs-lookup"><span data-stu-id="1ec74-124">Medium Trust</span></span>](#medium)
    - [<span data-ttu-id="1ec74-125">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="1ec74-125">&lt;appSettings&gt;</span></span>](#appsettings)
    - [<span data-ttu-id="1ec74-126">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="1ec74-126">UrlPathEncode</span></span>](#urlpathencode)
- [<span data-ttu-id="1ec74-127">Надежность и производительность</span><span class="sxs-lookup"><span data-stu-id="1ec74-127">Reliability and Performance</span></span>](#performance)

    - [<span data-ttu-id="1ec74-128">PreSendRequestHeaders и PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="1ec74-128">PreSendRequestHeaders and PreSendRequestContent</span></span>](#presend)
    - [<span data-ttu-id="1ec74-129">События асинхронной страницы с веб-форм</span><span class="sxs-lookup"><span data-stu-id="1ec74-129">Asynchronous Page Events with Web Forms</span></span>](#asyncevents)
    - [<span data-ttu-id="1ec74-130">Выстрелил и забыл работы</span><span class="sxs-lookup"><span data-stu-id="1ec74-130">Fire-and-Forget Work</span></span>](#fire)
    - [<span data-ttu-id="1ec74-131">Тело сущности запроса</span><span class="sxs-lookup"><span data-stu-id="1ec74-131">Request Entity Body</span></span>](#requestentity)
    - [<span data-ttu-id="1ec74-132">Response.Redirect и Response.End</span><span class="sxs-lookup"><span data-stu-id="1ec74-132">Response.Redirect and Response.End</span></span>](#redirect)
    - [<span data-ttu-id="1ec74-133">EnableViewState и ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="1ec74-133">EnableViewState and ViewStateMode</span></span>](#viewstatemode)
    - [<span data-ttu-id="1ec74-134">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="1ec74-134">SqlMembershipProvider</span></span>](#sqlprovider)
    - [<span data-ttu-id="1ec74-135">Длительные запросы, под управлением (> 110 секунд)</span><span class="sxs-lookup"><span data-stu-id="1ec74-135">Long Running Requests (>110 seconds)</span></span>](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a><span data-ttu-id="1ec74-136">Соответствие стандартам</span><span class="sxs-lookup"><span data-stu-id="1ec74-136">Standards Compliance</span></span>

<a id="adapters"></a>

### <a name="control-adapters"></a><span data-ttu-id="1ec74-137">Адаптеры элементов управления</span><span class="sxs-lookup"><span data-stu-id="1ec74-137">Control Adapters</span></span>

<span data-ttu-id="1ec74-138">Рекомендация: Остановить с помощью адаптеров элементов управления для адаптивной отрисовки и вместо этого используйте CSS медиа-запросами и совместимый со стандартами HTML.</span><span class="sxs-lookup"><span data-stu-id="1ec74-138">Recommendation: Stop using control adapters for adaptive rendering, and instead use CSS media queries and standards-compliant HTML.</span></span>

<span data-ttu-id="1ec74-139">Адаптеры элементов управления появились в .NET 2.0 для визуализации представления код, который был настроен для различных устройств и сред.</span><span class="sxs-lookup"><span data-stu-id="1ec74-139">Controls Adapters were introduced in .NET 2.0 to render presentation code that was customized for different devices and environments.</span></span> <span data-ttu-id="1ec74-140">Теперь этот адаптивной отрисовки может быть выполнено с CSS и HTML.</span><span class="sxs-lookup"><span data-stu-id="1ec74-140">Now, this adaptive rendering can be accomplished with CSS and HTML.</span></span> <span data-ttu-id="1ec74-141">Следует остановить с помощью адаптеров элементов управления и преобразуйте все существующие адаптеры CSS и HTML.</span><span class="sxs-lookup"><span data-stu-id="1ec74-141">You should stop using Control Adapters and convert any existing adapters to CSS and HTML.</span></span>

<span data-ttu-id="1ec74-142">Дополнительные сведения см. в разделе [медиа-запросами](http://www.w3.org/TR/css3-mediaqueries/) и [How To: Добавление страниц для мобильных устройств для вашего веб-форм ASP.NET и MVC-приложении](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="1ec74-142">For more information, see [Media Queries](http://www.w3.org/TR/css3-mediaqueries/) and [How To: Add Mobile Pages to Your ASP.NET Web Forms / MVC Application](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span></span>

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a><span data-ttu-id="1ec74-143">Свойства стиля для элементов управления</span><span class="sxs-lookup"><span data-stu-id="1ec74-143">Style Properties on Controls</span></span>

<span data-ttu-id="1ec74-144">Рекомендация: Остановить задание значений стиля в разметку элемента управления и вместо этого задать форматирование значений в таблицах стилей CSS.</span><span class="sxs-lookup"><span data-stu-id="1ec74-144">Recommendation: Stop setting style values in the control markup, and instead set formatting values in CSS stylesheets.</span></span>

<span data-ttu-id="1ec74-145">Веб-сервера управления содержат множество свойств, которые можно использовать для задания свойств стиля в строке.</span><span class="sxs-lookup"><span data-stu-id="1ec74-145">Web server controls contain dozens of properties which can be used to set in-line style properties.</span></span> <span data-ttu-id="1ec74-146">Например свойства ForeColor задает цвет текста для элемента управления.</span><span class="sxs-lookup"><span data-stu-id="1ec74-146">For example, the ForeColor property sets the color of the text for a control.</span></span> <span data-ttu-id="1ec74-147">Это можно сделать этого же эффекта эффективнее посредством таблицы стилей CSS.</span><span class="sxs-lookup"><span data-stu-id="1ec74-147">You can accomplish this same effect more efficiently through CSS stylesheets.</span></span> <span data-ttu-id="1ec74-148">Таблицы стилей позволяют централизовать значения стилей и не задавайте эти значения во всем приложении.</span><span class="sxs-lookup"><span data-stu-id="1ec74-148">Stylesheets enable you to centralize style values and avoid setting these values throughout your application.</span></span>

<span data-ttu-id="1ec74-149">Следующий пример показывает класс CSS наборов текст на красный.</span><span class="sxs-lookup"><span data-stu-id="1ec74-149">The following example shows a CSS class the sets text to red.</span></span>

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

<span data-ttu-id="1ec74-150">В следующем примере показано, как динамически применить класс CSS.</span><span class="sxs-lookup"><span data-stu-id="1ec74-150">The next example shows how to dynamically apply the CSS class.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a><span data-ttu-id="1ec74-151">Страницы и обратные вызовы для элемента управления</span><span class="sxs-lookup"><span data-stu-id="1ec74-151">Page and Control Callbacks</span></span>

<span data-ttu-id="1ec74-152">Рекомендация: Прекратить использование обратных вызовов страниц и элементов управления и вместо этого использовать любой из следующих: AJAX, UpdatePanel, методы действия MVC, веб-API или SignalR.</span><span class="sxs-lookup"><span data-stu-id="1ec74-152">Recommendation: Stop using page and control callbacks, and instead use any of the following: AJAX, UpdatePanel, MVC action methods, Web API, or SignalR.</span></span>

<span data-ttu-id="1ec74-153">В более ранних версиях ASP.NET страниц и элементов управления методы обратного вызова включено обновление части веб-страницы без обновления всей страницы.</span><span class="sxs-lookup"><span data-stu-id="1ec74-153">In earlier versions of ASP.NET, Page and Control callback methods enabled you to update part of the web page without refreshing an entire page.</span></span> <span data-ttu-id="1ec74-154">Теперь можно выполнить частичных обновлений через [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [веб-API](../../../web-api/index.md) или [SignalR](../../../signalr/index.md).</span><span class="sxs-lookup"><span data-stu-id="1ec74-154">You can now accomplish partial-page updates through [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web API](../../../web-api/index.md) or [SignalR](../../../signalr/index.md).</span></span> <span data-ttu-id="1ec74-155">Следует остановить с помощью методов обратного вызова, так как они могут вызвать проблемы с понятные URL-адреса и маршрутизацию.</span><span class="sxs-lookup"><span data-stu-id="1ec74-155">You should stop using callback methods because they can cause issues with friendly URLs and routing.</span></span> <span data-ttu-id="1ec74-156">По умолчанию элементы управления, не включайте методы обратного вызова, но если вы включили этот компонент в элементе управления, его следует отключить.</span><span class="sxs-lookup"><span data-stu-id="1ec74-156">By default, controls do not enable callback methods, but if you enabled this feature in a control, you should disable it.</span></span>

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a><span data-ttu-id="1ec74-157">Определение возможностей браузера</span><span class="sxs-lookup"><span data-stu-id="1ec74-157">Browser Capability Detection</span></span>

<span data-ttu-id="1ec74-158">Рекомендация: Прекратить использование статических браузера возможность обнаружения, а вместо этого используйте функцию динамического обнаружения.</span><span class="sxs-lookup"><span data-stu-id="1ec74-158">Recommendation: Stop using static browser capability detection, and instead use dynamic feature detection.</span></span>

<span data-ttu-id="1ec74-159">В более ранних версиях ASP.NET поддерживаемых функций для каждого браузера, хранятся в XML-файл.</span><span class="sxs-lookup"><span data-stu-id="1ec74-159">In earlier versions of ASP.NET, the supported features for each browser were stored in an XML file.</span></span> <span data-ttu-id="1ec74-160">Обнаружение поддержка функций через статический подстановки не лучшим подходом.</span><span class="sxs-lookup"><span data-stu-id="1ec74-160">Detecting feature support through a static lookup is not the best approach.</span></span> <span data-ttu-id="1ec74-161">Теперь можно динамически обнаруживать поддерживаемых возможностей браузера с помощью платформы обнаружение компонентов, таких как [Modernizr](http://modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="1ec74-161">Now, you can dynamically detect a browser's supported features by using a feature detection framework, such as [Modernizr](http://modernizr.com/).</span></span> <span data-ttu-id="1ec74-162">Функция обнаружения определяет поддержки попытка использовать метод или свойство, и затем проверив для просмотра, если браузер формируется нужного результата.</span><span class="sxs-lookup"><span data-stu-id="1ec74-162">Feature detection determines support by attempting to use a method or property and then checking to see if the browser produced the desired result.</span></span> <span data-ttu-id="1ec74-163">По умолчанию Modernizr состава шаблонов веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="1ec74-163">By default, Modernizr is included in the Web application templates.</span></span>

<a id="security"></a>

## <a name="security"></a><span data-ttu-id="1ec74-164">Безопасность</span><span class="sxs-lookup"><span data-stu-id="1ec74-164">Security</span></span>

<a id="validation"></a>

### <a name="request-validation"></a><span data-ttu-id="1ec74-165">Проверка запросов</span><span class="sxs-lookup"><span data-stu-id="1ec74-165">Request Validation</span></span>

<span data-ttu-id="1ec74-166">Рекомендация: Проверки пользовательского ввода и кодирование вывода от пользователей.</span><span class="sxs-lookup"><span data-stu-id="1ec74-166">Recommendation: Validate user input, and encode output from users.</span></span>

<span data-ttu-id="1ec74-167">Проверка запроса — это функция ASP.NET, проверяет каждый запрос и останавливает запрос при обнаружении предполагаемых угроз.</span><span class="sxs-lookup"><span data-stu-id="1ec74-167">Request validation is a feature of ASP.NET that inspects each request and stops the request if a perceived threat is found.</span></span> <span data-ttu-id="1ec74-168">Не полагайтесь на проверку запросов для защиты приложения от атак с использованием межсайтовых сценариев.</span><span class="sxs-lookup"><span data-stu-id="1ec74-168">Do not depend on request validation for securing your application against cross-site scripting attacks.</span></span> <span data-ttu-id="1ec74-169">Вместо этого проверяйте весь ввод от пользователей и кодирования выходных данных.</span><span class="sxs-lookup"><span data-stu-id="1ec74-169">Instead, validate all input from users and encode the output.</span></span> <span data-ttu-id="1ec74-170">В некоторых случаях можно использовать регулярные выражения для проверки входных данных, но в более сложных случаях следует проверять ввод данных пользователем с помощью классов .NET, которые определяют, если значение совпадает с допустимыми значениями.</span><span class="sxs-lookup"><span data-stu-id="1ec74-170">In some limited cases, you can use regular expressions to validate the input, but in more complicated cases you should validate user input by using .NET classes that determine if the value matches allowed values.</span></span>

<span data-ttu-id="1ec74-171">В следующем примере показано, как использовать статический метод в класс Uri, чтобы определить, является ли допустимым Uri, предоставленный пользователем.</span><span class="sxs-lookup"><span data-stu-id="1ec74-171">The following example shows how to use a static method in the Uri class to determine whether the Uri provided by a user is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

<span data-ttu-id="1ec74-172">Тем не менее, чтобы достаточно проверить Uri, обратитесь к убедитесь, что он указывает `http` или `https`.</span><span class="sxs-lookup"><span data-stu-id="1ec74-172">However, to sufficiently verify the Uri, you should also check to make sure it specifies `http` or `https`.</span></span> <span data-ttu-id="1ec74-173">В следующем примере методы экземпляра убедитесь, что Uri является допустимым.</span><span class="sxs-lookup"><span data-stu-id="1ec74-173">The following example uses instance methods to verify that the Uri is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

<span data-ttu-id="1ec74-174">Прежде чем визуализации ввод данных пользователем в формате HTML или включая пользовательский ввод SQL-запроса, кодирования значений, чтобы убедиться, что вредоносный код не был включен.</span><span class="sxs-lookup"><span data-stu-id="1ec74-174">Before rendering user input as HTML or including user input in a SQL query, encode the values to ensure malicious code is not included.</span></span>

<span data-ttu-id="1ec74-175">Вы можете HTML кодирования значения в разметке, с &lt;%: %&gt; синтаксис, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="1ec74-175">You can HTML encode the value in markup with the &lt;%: %&gt; syntax, as shown below.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

<span data-ttu-id="1ec74-176">В синтаксис Razor, вы также можете HTML кодирования с @, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="1ec74-176">Or, in Razor syntax, you can HTML encode with @, as shown below.</span></span>

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="1ec74-177">В следующем примере показан способ необходима кодировка HTML значение в коде.</span><span class="sxs-lookup"><span data-stu-id="1ec74-177">The next example shows how to HTML encode a value in code-behind.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

<span data-ttu-id="1ec74-178">Для безопасного кодирования значение для команды SQL, используйте параметры команды, например [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span><span class="sxs-lookup"><span data-stu-id="1ec74-178">To safely encode a value for SQL commands, use command parameters such as the [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span></span> <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a><span data-ttu-id="1ec74-179">Проверка подлинности форм без поддержки файлов cookie и сеансов</span><span class="sxs-lookup"><span data-stu-id="1ec74-179">Cookieless Forms Authentication and Session</span></span>

<span data-ttu-id="1ec74-180">Рекомендация: Требуется файлы cookie.</span><span class="sxs-lookup"><span data-stu-id="1ec74-180">Recommendation: Require cookies.</span></span>

<span data-ttu-id="1ec74-181">Передача данных проверки подлинности в строке запроса не является безопасным.</span><span class="sxs-lookup"><span data-stu-id="1ec74-181">Passing authentication information in the query string is not secure.</span></span> <span data-ttu-id="1ec74-182">Таким образом требуют файлы cookie, если приложение включает проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="1ec74-182">Therefore, require cookies when your application includes authentication.</span></span> <span data-ttu-id="1ec74-183">Если файлы cookie с компьютера хранятся конфиденциальные сведения, рассмотрите возможность применения SSL для файла cookie.</span><span class="sxs-lookup"><span data-stu-id="1ec74-183">If your cookie stores sensitive information, consider requiring SSL for the cookie.</span></span>

<span data-ttu-id="1ec74-184">Следующий пример показывает способ указания в файле Web.config, что проверка подлинности форм требуется файл cookie, отправленных по протоколу SSL.</span><span class="sxs-lookup"><span data-stu-id="1ec74-184">The following example shows how to specify in the Web.config file that Forms Authentication requires a cookie that is transmitted over SSL.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a><span data-ttu-id="1ec74-185">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="1ec74-185">EnableViewStateMac</span></span>

<span data-ttu-id="1ec74-186">Рекомендация: Никогда не задано значение false.</span><span class="sxs-lookup"><span data-stu-id="1ec74-186">Recommendation: Never set to false.</span></span>

<span data-ttu-id="1ec74-187">По умолчанию EnbableViewStateMac задано значение true.</span><span class="sxs-lookup"><span data-stu-id="1ec74-187">By default, EnbableViewStateMac is set to true.</span></span> <span data-ttu-id="1ec74-188">Даже если приложение не использует состояние просмотра, не устанавливайте EnableViewStateMac значение false.</span><span class="sxs-lookup"><span data-stu-id="1ec74-188">Even if your application is not using view state, do not set EnableViewStateMac to false.</span></span> <span data-ttu-id="1ec74-189">Установка этого значения равным false делает приложение уязвимым для межсайтовых сценариев.</span><span class="sxs-lookup"><span data-stu-id="1ec74-189">Setting this value to false will make your application vulnerable to cross-site scripting.</span></span>

<span data-ttu-id="1ec74-190">Начиная с ASP.NET 4.5.2, среда выполнения обеспечивает **EnableViewStateMac = true**.</span><span class="sxs-lookup"><span data-stu-id="1ec74-190">Starting with ASP.NET 4.5.2, the runtime enforces **EnableViewStateMac=true**.</span></span> <span data-ttu-id="1ec74-191">Даже если присвоить ему значение false, среда выполнения игнорирует это значение и выполняет заданное значение в значение true.</span><span class="sxs-lookup"><span data-stu-id="1ec74-191">Even if you set it to false, the runtime ignores this value and proceeds with the value set to true.</span></span> <span data-ttu-id="1ec74-192">Дополнительные сведения см. в разделе [ASP.NET 4.5.2 и EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span><span class="sxs-lookup"><span data-stu-id="1ec74-192">For more information, see [ASP.NET 4.5.2 and EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span></span>

<span data-ttu-id="1ec74-193">В следующем примере показано, как EnableViewStateMac присвоено значение true.</span><span class="sxs-lookup"><span data-stu-id="1ec74-193">The following example shows how to set EnableViewStateMac to true.</span></span> <span data-ttu-id="1ec74-194">Необходимо фактически задано значение true, так как он имеет значение true, если по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="1ec74-194">You do not need to actually set this value to true because it is true by default.</span></span> <span data-ttu-id="1ec74-195">Если вы настроили его значение false на любой странице в приложении, необходимо немедленно исправить это значение.</span><span class="sxs-lookup"><span data-stu-id="1ec74-195">However, if you have set it to false on any page in your application, you must immediately correct this value.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a><span data-ttu-id="1ec74-196">Средний уровень доверия</span><span class="sxs-lookup"><span data-stu-id="1ec74-196">Medium Trust</span></span>

<span data-ttu-id="1ec74-197">Рекомендация: Как защитная граница не полагаться на средний уровень доверия (или любом другом уровне доверия).</span><span class="sxs-lookup"><span data-stu-id="1ec74-197">Recommendation: Do not depend on Medium Trust (or any other trust level) as a security boundary.</span></span>

<span data-ttu-id="1ec74-198">Не могут обеспечить адекватную защиту приложения с частичным доверием и не должны использоваться.</span><span class="sxs-lookup"><span data-stu-id="1ec74-198">Partial trust does not adequately protect your application and should not be used.</span></span> <span data-ttu-id="1ec74-199">Вместо этого используйте полное доверие и изолировать без доверия приложения в разных пулах приложений.</span><span class="sxs-lookup"><span data-stu-id="1ec74-199">Instead, use Full Trust, and isolate untrusted applications in separate application pools.</span></span> <span data-ttu-id="1ec74-200">Кроме того запустите каждый пул приложений с уникальным удостоверением.</span><span class="sxs-lookup"><span data-stu-id="1ec74-200">Also, run each application pool under a unique identity.</span></span> <span data-ttu-id="1ec74-201">Дополнительные сведения см. в разделе [частичное доверие в ASP.NET не обеспечивает изоляцию приложений](https://support.microsoft.com/kb/2698981).</span><span class="sxs-lookup"><span data-stu-id="1ec74-201">For more information, see [ASP.NET Partial Trust does not guarantee application isolation](https://support.microsoft.com/kb/2698981).</span></span>

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a><span data-ttu-id="1ec74-202">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="1ec74-202">&lt;appSettings&gt;</span></span>

<span data-ttu-id="1ec74-203">Рекомендация: Не отключайте параметры безопасности в &lt;appSettings&gt; элемента.</span><span class="sxs-lookup"><span data-stu-id="1ec74-203">Recommendation: Do not disable security settings in &lt;appSettings&gt; element.</span></span>

<span data-ttu-id="1ec74-204">Элемент appSettings содержит много значений, которые необходимы для обновления для системы безопасности.</span><span class="sxs-lookup"><span data-stu-id="1ec74-204">The appSettings element contains many values which are required for security updates.</span></span> <span data-ttu-id="1ec74-205">Нельзя изменить или отключить эти значения.</span><span class="sxs-lookup"><span data-stu-id="1ec74-205">You should not change or disable these values.</span></span> <span data-ttu-id="1ec74-206">Если эти значения необходимо отключить при развертывании обновления, немедленно повторно включите после завершения развертывания.</span><span class="sxs-lookup"><span data-stu-id="1ec74-206">If you must disable these values when deploying an update, immediately re-enable after completing deployment.</span></span>

<span data-ttu-id="1ec74-207">Дополнительные сведения см. в разделе [элементу appSettings ASP.NET](https://msdn.microsoft.com/library/hh975440.aspx).</span><span class="sxs-lookup"><span data-stu-id="1ec74-207">For details, see [ASP.NET appSettings Element](https://msdn.microsoft.com/library/hh975440.aspx).</span></span>

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a><span data-ttu-id="1ec74-208">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="1ec74-208">UrlPathEncode</span></span>

<span data-ttu-id="1ec74-209">Рекомендации: Используйте [кодируют](https://msdn.microsoft.com/library/zttxte6w.aspx) вместо него.</span><span class="sxs-lookup"><span data-stu-id="1ec74-209">Recommendation: Use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) instead.</span></span>

<span data-ttu-id="1ec74-210">Метод UrlPathEncode была добавлена в .NET Framework, чтобы устранить проблему совместимости очень конкретного браузера.</span><span class="sxs-lookup"><span data-stu-id="1ec74-210">The UrlPathEncode method was added to the .NET Framework to resolve a very specific browser compatibility problem.</span></span> <span data-ttu-id="1ec74-211">Не кодирует адекватно URL-адреса и не обеспечивает дополнительную защиту от межсайтовых сценариев.</span><span class="sxs-lookup"><span data-stu-id="1ec74-211">It does not adequately encode a URL, and does not protect your application from cross-site scripting.</span></span> <span data-ttu-id="1ec74-212">Он никогда не следует использовать в приложении.</span><span class="sxs-lookup"><span data-stu-id="1ec74-212">You should never use it in your application.</span></span> <span data-ttu-id="1ec74-213">Вместо этого используйте [кодируют](https://msdn.microsoft.com/library/zttxte6w.aspx).</span><span class="sxs-lookup"><span data-stu-id="1ec74-213">Instead, use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span></span>

<span data-ttu-id="1ec74-214">В следующем примере показано, как передать закодированный URL-адрес в качестве параметра строки запроса для элемента управления hyperlink.</span><span class="sxs-lookup"><span data-stu-id="1ec74-214">The following example shows how to pass an encoded URL as a query string parameter for a hyperlink control.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a><span data-ttu-id="1ec74-215">Надежность и производительность</span><span class="sxs-lookup"><span data-stu-id="1ec74-215">Reliability and Performance</span></span>

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a><span data-ttu-id="1ec74-216">PreSendRequestHeaders и PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="1ec74-216">PreSendRequestHeaders and PreSendRequestContent</span></span>

<span data-ttu-id="1ec74-217">Рекомендация: Не используйте эти события с управляемых модулей.</span><span class="sxs-lookup"><span data-stu-id="1ec74-217">Recommendation: Do not use these events with managed modules.</span></span> <span data-ttu-id="1ec74-218">Вместо этого можно напишите собственный модуль IIS для выполнения требуемой задачи.</span><span class="sxs-lookup"><span data-stu-id="1ec74-218">Instead, write a native IIS module to perform the required task.</span></span> <span data-ttu-id="1ec74-219">В разделе [создания модулей машинного кода HTTP](https://msdn.microsoft.com/library/ms693629.aspx).</span><span class="sxs-lookup"><span data-stu-id="1ec74-219">See [Creating Native-Code HTTP Modules](https://msdn.microsoft.com/library/ms693629.aspx).</span></span>

<span data-ttu-id="1ec74-220">Можно использовать [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) и [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) событий с собственные модули IIS.</span><span class="sxs-lookup"><span data-stu-id="1ec74-220">You can use the [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) and [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) events with native IIS modules.</span></span>
> [!WARNING]
> <span data-ttu-id="1ec74-221">Не используйте `PreSendRequestHeaders` и `PreSendRequestContent` с управляемых модулей, которые реализуют `IHttpModule`.</span><span class="sxs-lookup"><span data-stu-id="1ec74-221">Do not use `PreSendRequestHeaders` and `PreSendRequestContent` with managed modules that implement `IHttpModule`.</span></span> <span data-ttu-id="1ec74-222">Настройка этих свойств может вызвать трудности в асинхронных запросов.</span><span class="sxs-lookup"><span data-stu-id="1ec74-222">Setting these properties can cause issues with asynchronous requests.</span></span> <span data-ttu-id="1ec74-223">Сочетание маршрутизации запрошенный приложений (ARR) и websockets может стать причиной исключения нарушения прав доступа, которые могут вызвать w3wp к сбою.</span><span class="sxs-lookup"><span data-stu-id="1ec74-223">The combination of Application Requested Routing (ARR) and websockets might lead to access violation exceptions that can cause w3wp to crash.</span></span> <span data-ttu-id="1ec74-224">Например, iiscore! W3_CONTEXT_BASE::GetIsLastNotification + 68 в iiscore.dll вызвала нарушение прав доступа (0xC0000005).</span><span class="sxs-lookup"><span data-stu-id="1ec74-224">For example, iiscore!W3_CONTEXT_BASE::GetIsLastNotification+68 in iiscore.dll has caused an access violation exception (0xC0000005).</span></span>

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a><span data-ttu-id="1ec74-225">События асинхронной страницы с веб-форм</span><span class="sxs-lookup"><span data-stu-id="1ec74-225">Asynchronous Page Events with Web Forms</span></span>

<span data-ttu-id="1ec74-226">Рекомендации: В веб-формы, не используйте запись async void методов, событий жизненного цикла страницы, а вместо этого использовать [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) для асинхронного кода.</span><span class="sxs-lookup"><span data-stu-id="1ec74-226">Recommendation: In Web Forms, avoid writing async void methods for Page lifecycle events, and instead use [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) for asynchronous code.</span></span>

<span data-ttu-id="1ec74-227">Если пометить события страницы с **async** и **void**, не может определить, после завершения асинхронного кода.</span><span class="sxs-lookup"><span data-stu-id="1ec74-227">When you mark a page event with **async** and **void**, you cannot determine when the asynchronous code has finished.</span></span> <span data-ttu-id="1ec74-228">Вместо этого используйте Page.RegisterAsyncTask для выполнения асинхронного кода в способом, который позволяет отслеживать его выполнение.</span><span class="sxs-lookup"><span data-stu-id="1ec74-228">Instead, use Page.RegisterAsyncTask to run the asynchronous code in a way that enables you to track its completion.</span></span>

<span data-ttu-id="1ec74-229">В следующем примере показан кнопки выберите обработчик, который содержит асинхронного кода.</span><span class="sxs-lookup"><span data-stu-id="1ec74-229">The following example shows a button click handler that contains asynchronous code.</span></span> <span data-ttu-id="1ec74-230">Данный пример включает асинхронно, считывая значение строки, предоставляемое только как упрощенный пример асинхронной задачи, а не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="1ec74-230">This example includes reading a string value asynchronously, which is provided only as a simplified example of an asynchronous task and not as a recommended practice.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

<span data-ttu-id="1ec74-231">При использовании асинхронных задач присвоено Http среды выполнения требуемая версия .NET framework 4.5 в файле Web.config.</span><span class="sxs-lookup"><span data-stu-id="1ec74-231">If you are using asynchronous Tasks, set the Http runtime target framework to 4.5 in the Web.config file.</span></span> <span data-ttu-id="1ec74-232">Настройке требуемой версии .NET framework 4.5 Включение на новый контекст синхронизации, была добавлена в .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="1ec74-232">Setting the target framework to 4.5 turns on the new synchronization context that was added in .NET 4.5.</span></span> <span data-ttu-id="1ec74-233">Это значение установлено значение по умолчанию в новых проектах в Visual Studio 2012, но не была настроена при работе с существующим проектом.</span><span class="sxs-lookup"><span data-stu-id="1ec74-233">This value is set by default in new projects in Visual Studio 2012, but is not be set if you are working with an existing project.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a><span data-ttu-id="1ec74-234">Выстрелил и забыл работы</span><span class="sxs-lookup"><span data-stu-id="1ec74-234">Fire-and-Forget Work</span></span>

<span data-ttu-id="1ec74-235">Рекомендация: При обработке запроса в ASP.NET, избегайте запуска рабочих выстрелил и забыл (такой вызов метода ThreadPool.QueueUserWorkItem или создания таймер, который многократно вызывает делегат).</span><span class="sxs-lookup"><span data-stu-id="1ec74-235">Recommendation: When handling a request within ASP.NET, avoid launching fire-and-forget work (such calling the ThreadPool.QueueUserWorkItem method or creating a timer that repeatedly calls a delegate).</span></span>

<span data-ttu-id="1ec74-236">Если приложение имеет выстрелил и забыл работы, которая выполняется в ASP.NET, приложение можно получить синхронизированы. В любое время домен приложения можно удалить это означает, что ваш текущий процесс больше не соответствует текущее состояние приложения.</span><span class="sxs-lookup"><span data-stu-id="1ec74-236">If your application has fire-and-forget work that runs within ASP.NET, your application can get out of sync. At any time, the app domain can be destroyed which means your ongoing process may no longer match the current state of the application.</span></span>

<span data-ttu-id="1ec74-237">Этот тип работы вне ASP.NET следует переместить.</span><span class="sxs-lookup"><span data-stu-id="1ec74-237">You should move this type of work outside of ASP.NET.</span></span> <span data-ttu-id="1ec74-238">Можно использовать веб-задания, службы Windows или рабочей роли в Azure для выполнения текущей работы и запускать этот код от другого процесса.</span><span class="sxs-lookup"><span data-stu-id="1ec74-238">You can use a Web Jobs, Windows Service or a Worker role in Azure to perform ongoing work, and run that code from another process.</span></span>

<span data-ttu-id="1ec74-239">Если необходимо выполнить эту работу в ASP.NET, можно добавить пакет Nuget вызывается [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) выполнение кода.</span><span class="sxs-lookup"><span data-stu-id="1ec74-239">If you must perform this work within ASP.NET, you can add the Nuget package called [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) to run the code.</span></span>

<a id="requestentity"></a>

### <a name="request-entity-body"></a><span data-ttu-id="1ec74-240">Тело сущности запроса</span><span class="sxs-lookup"><span data-stu-id="1ec74-240">Request Entity Body</span></span>

<span data-ttu-id="1ec74-241">Рекомендация: Избегайте чтение Request.Form или Request.InputStream до обработчика выполнить событие.</span><span class="sxs-lookup"><span data-stu-id="1ec74-241">Recommendation: Avoid reading Request.Form or Request.InputStream before the handler's execute event.</span></span>

<span data-ttu-id="1ec74-242">Самый ранний лимит должны считываться Request.Form или Request.InputStream во время работы обработчика выполнить событие.</span><span class="sxs-lookup"><span data-stu-id="1ec74-242">The earliest you should read from Request.Form or Request.InputStream is during the handler's execute event.</span></span> <span data-ttu-id="1ec74-243">В платформе MVC контроллер обработчик а execute события при выполнении метода.</span><span class="sxs-lookup"><span data-stu-id="1ec74-243">In MVC, the Controller is the handler and the execute event is when the action method runs.</span></span> <span data-ttu-id="1ec74-244">В веб-формы страница является обработчик и выполнить событие после возникновения события Page.Init.</span><span class="sxs-lookup"><span data-stu-id="1ec74-244">In Web Forms, the Page is the handler and the execute event is when the Page.Init event fires.</span></span> <span data-ttu-id="1ec74-245">Если более ранних, чем событие выполнения прочитать тело сущности запроса, помешать обработки запроса.</span><span class="sxs-lookup"><span data-stu-id="1ec74-245">If you read the request entity body earlier than the execute event, you interfere with the processing of the request.</span></span>

<span data-ttu-id="1ec74-246">Если вам нужно прочитать тело сущности запроса, прежде чем событие выполнения, используйте либо [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) или [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span><span class="sxs-lookup"><span data-stu-id="1ec74-246">If you need to read the request entity body before the execute event, use either [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) or [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span></span> <span data-ttu-id="1ec74-247">При использовании GetBufferlessInputStream, вы получаете исходный поток из запроса и принимают на себя ответственность за обработку всего запроса.</span><span class="sxs-lookup"><span data-stu-id="1ec74-247">When you use GetBufferlessInputStream, you get the raw stream from the request, and assume responsibility for processing the entire request.</span></span> <span data-ttu-id="1ec74-248">После вызова GetBufferlessInputStream, Request.Form и Request.InputStream недоступны, так как они не заполнены в ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1ec74-248">After calling GetBufferlessInputStream, Request.Form and Request.InputStream are not available because they have not been populated by ASP.NET.</span></span> <span data-ttu-id="1ec74-249">При использовании GetBufferedInputStream, можно получить копию потока из запроса.</span><span class="sxs-lookup"><span data-stu-id="1ec74-249">When you use GetBufferedInputStream, you get a copy of the stream from the request.</span></span> <span data-ttu-id="1ec74-250">Request.Form и Request.InputStream по-прежнему доступны в запрос, так как ASP.NET заполняет другие копии.</span><span class="sxs-lookup"><span data-stu-id="1ec74-250">Request.Form and Request.InputStream are still available later in the request because ASP.NET populates the other copy.</span></span>

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a><span data-ttu-id="1ec74-251">Response.Redirect и Response.End</span><span class="sxs-lookup"><span data-stu-id="1ec74-251">Response.Redirect and Response.End</span></span>

<span data-ttu-id="1ec74-252">Рекомендация: Следует учитывать различия в обработке потока после вызова [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span><span class="sxs-lookup"><span data-stu-id="1ec74-252">Recommendation: Be aware of differences in how thread is handled after calling [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span></span>

<span data-ttu-id="1ec74-253">[Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) метод вызывает метод Response.End.</span><span class="sxs-lookup"><span data-stu-id="1ec74-253">The [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) method calls the Response.End method.</span></span> <span data-ttu-id="1ec74-254">Синхронного процесса вызов Request.Redirect вызывает немедленно прерывание текущего потока.</span><span class="sxs-lookup"><span data-stu-id="1ec74-254">In a synchronous process, calling Request.Redirect causes the current thread to immediately abort.</span></span> <span data-ttu-id="1ec74-255">Тем не менее в асинхронном процессе, вызов Response.Redirect не будет прерываться текущего потока, поэтому выполнение кода продолжается для запроса.</span><span class="sxs-lookup"><span data-stu-id="1ec74-255">However, in an asynchronous process, calling Response.Redirect does not abort the current thread, so code execution continues for the request.</span></span> <span data-ttu-id="1ec74-256">В асинхронном процессе необходимо вернуть задачу из метода, для остановки выполнения кода.</span><span class="sxs-lookup"><span data-stu-id="1ec74-256">In an asynchronous process, you must return the Task from the method to stop the code execution.</span></span>

<span data-ttu-id="1ec74-257">В проект MVC не следует вызывать Response.Redirect.</span><span class="sxs-lookup"><span data-stu-id="1ec74-257">In an MVC project, you should not call Response.Redirect.</span></span> <span data-ttu-id="1ec74-258">Вместо этого возвращает RedirectResult.</span><span class="sxs-lookup"><span data-stu-id="1ec74-258">Instead, return a RedirectResult.</span></span>

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a><span data-ttu-id="1ec74-259">EnableViewState и ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="1ec74-259">EnableViewState and ViewStateMode</span></span>

<span data-ttu-id="1ec74-260">Рекомендация: ViewStateMode используйте вместо EnableViewState для предоставления детальный контроль над которой элементы управления используют состояние представления.</span><span class="sxs-lookup"><span data-stu-id="1ec74-260">Recommendation: Use ViewStateMode, instead of EnableViewState, to provide granular control over which controls use view state.</span></span>

<span data-ttu-id="1ec74-261">При EnableViewState задано значение false в директиве Page, состояние просмотра отключено для всех элементов управления на странице и не может быть включен.</span><span class="sxs-lookup"><span data-stu-id="1ec74-261">When you set EnableViewState to false in the Page directive, view state is disabled for all controls within the page and cannot be enabled.</span></span> <span data-ttu-id="1ec74-262">Если вы хотите включить состояние представления только для определенных элементов управления на странице, значение Disabled ViewStateMode для страницы.</span><span class="sxs-lookup"><span data-stu-id="1ec74-262">If you want to enable view state for only certain controls in your page, set ViewStateMode to Disabled for the Page.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

<span data-ttu-id="1ec74-263">Затем задайте ViewStateMode включена на только элементы управления, которые действительно нужны состояния представления.</span><span class="sxs-lookup"><span data-stu-id="1ec74-263">Then, set ViewStateMode to Enabled on only the controls that actually need view state.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

<span data-ttu-id="1ec74-264">Включить состояние представления только элементов управления, которым она необходима, можно уменьшить размер состояния представления для веб-страниц.</span><span class="sxs-lookup"><span data-stu-id="1ec74-264">By enabling view state for only the controls that need it, you can shrink the size of the view state for your web pages.</span></span>

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a><span data-ttu-id="1ec74-265">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="1ec74-265">SqlMembershipProvider</span></span>

<span data-ttu-id="1ec74-266">Рекомендация: Используйте универсальные поставщики.</span><span class="sxs-lookup"><span data-stu-id="1ec74-266">Recommendation: Use Universal Providers.</span></span>

<span data-ttu-id="1ec74-267">В текущем шаблоны проектов, SqlMembershipProvider будет заменен [универсальные поставщики ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.Providers), который доступен как пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="1ec74-267">In the current project templates, SqlMembershipProvider has been replaced by [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), which is available as a NuGet package.</span></span> <span data-ttu-id="1ec74-268">Если вы используете SqlMembershipProvider в проекте, который был создан с более ранней версией шаблонов, следует перейти к универсальные поставщики.</span><span class="sxs-lookup"><span data-stu-id="1ec74-268">If you are using SqlMembershipProvider in a project that was built with an earlier version of the templates, you should switch to Universal Providers.</span></span> <span data-ttu-id="1ec74-269">Универсальные поставщики работают всеми базами данных, которые поддерживаются платформой Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1ec74-269">The Universal Providers work with all databases that are supported by Entity Framework.</span></span>

<span data-ttu-id="1ec74-270">Дополнительные сведения см. в разделе [введение в универсальные поставщики ASP.NET](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span><span class="sxs-lookup"><span data-stu-id="1ec74-270">For more information, see [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span></span>

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a><span data-ttu-id="1ec74-271">Длительные запросы (> 110 секунд)</span><span class="sxs-lookup"><span data-stu-id="1ec74-271">Long-running Requests (>110 seconds)</span></span>

<span data-ttu-id="1ec74-272">Рекомендации: Используйте [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) или [SignalR](../../../signalr/index.md) для подключенных клиентов и используйте асинхронные операции ввода-вывода.</span><span class="sxs-lookup"><span data-stu-id="1ec74-272">Recommendation: Use [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) or [SignalR](../../../signalr/index.md) for connected clients, and use asynchronous I/O operations.</span></span>

<span data-ttu-id="1ec74-273">Длительные запросы может привести к непредсказуемым результатам и снижению производительности веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="1ec74-273">Long-running requests can cause unpredictable results and poor performance in your web application.</span></span> <span data-ttu-id="1ec74-274">Значение по умолчанию время ожидания для запроса составляет 110 секунд.</span><span class="sxs-lookup"><span data-stu-id="1ec74-274">The default timeout setting for a request is 110 seconds.</span></span> <span data-ttu-id="1ec74-275">При использовании состояния сеанса с помощью запроса долго выполняющихся ASP.NET будет снимать блокировку для объекта сеанса через 110 секунд.</span><span class="sxs-lookup"><span data-stu-id="1ec74-275">If you are using session state with a long-running request, ASP.NET will release the lock on the Session object after 110 seconds.</span></span> <span data-ttu-id="1ec74-276">Однако приложение может находиться в процессе операции в объекте сеанса после снятия блокировки и не может выполнить операцию.</span><span class="sxs-lookup"><span data-stu-id="1ec74-276">However, your application might be in the middle of an operation on the Session object when the lock is released, and the operation might not complete successfully.</span></span> <span data-ttu-id="1ec74-277">Если второй запрос от пользователя блокируется во время первого запроса, второй запрос может получить доступ к объекту сеанса в несогласованном состоянии.</span><span class="sxs-lookup"><span data-stu-id="1ec74-277">If a second request from the user is blocked while the first request is running, the second request might access the Session object in an inconsistent state.</span></span>

<span data-ttu-id="1ec74-278">Если приложение включает блокировки (или синхронный) операций ввода-вывода, приложение будет отвечать на запросы.</span><span class="sxs-lookup"><span data-stu-id="1ec74-278">If your application includes blocking (or synchronous) I/O operations, the application will be unresponsive.</span></span>

<span data-ttu-id="1ec74-279">Чтобы повысить производительность, используйте асинхронные операции ввода-вывода в .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="1ec74-279">To improve performance, use the asynchronous I/O operations in the .NET Framework.</span></span> <span data-ttu-id="1ec74-280">Кроме того при подключении клиентов к серверу используйте WebSockets или SignalR.</span><span class="sxs-lookup"><span data-stu-id="1ec74-280">Also, use WebSockets or SignalR for connecting clients to the server.</span></span> <span data-ttu-id="1ec74-281">Эти функции предназначены для оптимально долго выполняющихся запросов.</span><span class="sxs-lookup"><span data-stu-id="1ec74-281">These features are designed to efficiently handle long-running requests.</span></span>
