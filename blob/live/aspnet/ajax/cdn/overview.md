---
uid: ajax/cdn/overview
title: "Сеть доставки содержимого Microsoft Ajax | Документы Microsoft"
author: rick-anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/14/2017
ms.topic: article
ms.assetid: 8935bf14-ca6d-4a4e-9dbe-b96ce74cef49
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /ajax/cdn
msc.type: content
ms.openlocfilehash: f69f707ba64d13fc372b7bc44718c9dcf8cec6e2
ms.sourcegitcommit: 3f491f887074310fc0f145cd01a670aa63b969e3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/22/2018
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="27b0b-102">Сеть доставки содержимого Microsoft Ajax</span><span class="sxs-lookup"><span data-stu-id="27b0b-102">Microsoft Ajax Content Delivery Network</span></span>
====================
<span data-ttu-id="27b0b-103">Примечание: Сети Microsoft Ajax CDN есть кода с использованием Azure CDN не соглашения об уровне ОБСЛУЖИВАНИЯ.</span><span class="sxs-lookup"><span data-stu-id="27b0b-103">Note: The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="27b0b-104">Содержание</span><span class="sxs-lookup"><span data-stu-id="27b0b-104">Table of Contents</span></span>

<span data-ttu-id="27b0b-105">**[переименована в ajax.aspnetcdn.com AJAX.Microsoft.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="27b0b-105">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="27b0b-106">**[Поддержка .vsdoc Visual Studio](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="27b0b-106">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="27b0b-107">**[С помощью ASP.NET Ajax из CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="27b0b-107">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="27b0b-108">**[С помощью jQuery из CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="27b0b-108">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="27b0b-109">**[С помощью jQuery UI из CDN](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="27b0b-109">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="27b0b-110">**[Сторонние файлы на CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="27b0b-110">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="27b0b-111">Выпусков jQuery в CDN</span><span class="sxs-lookup"><span data-stu-id="27b0b-111">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="27b0b-112">Перенос выпусков jQuery в CDN</span><span class="sxs-lookup"><span data-stu-id="27b0b-112">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="27b0b-113">jQuery с CDN выпуски пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="27b0b-113">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="27b0b-114">jQuery с CDN выпуски проверки</span><span class="sxs-lookup"><span data-stu-id="27b0b-114">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="27b0b-115">jQuery Mobile выпуски на CDN</span><span class="sxs-lookup"><span data-stu-id="27b0b-115">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="27b0b-116">jQuery с CDN выпуски шаблонов</span><span class="sxs-lookup"><span data-stu-id="27b0b-116">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="27b0b-117">jQuery с CDN выпуски цикла</span><span class="sxs-lookup"><span data-stu-id="27b0b-117">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="27b0b-118">jQuery с CDN выпуски таблицы данных</span><span class="sxs-lookup"><span data-stu-id="27b0b-118">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="27b0b-119">Выпуски Modernizr на CDN</span><span class="sxs-lookup"><span data-stu-id="27b0b-119">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="27b0b-120">Выпуски JSHint на CDN</span><span class="sxs-lookup"><span data-stu-id="27b0b-120">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="27b0b-121">Выпуски для маскирования на CDN</span><span class="sxs-lookup"><span data-stu-id="27b0b-121">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="27b0b-122">Глобализация с CDN выпуски</span><span class="sxs-lookup"><span data-stu-id="27b0b-122">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="27b0b-123">Ответ с CDN выпуски</span><span class="sxs-lookup"><span data-stu-id="27b0b-123">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="27b0b-124">Выпуски для начальной загрузки на CDN</span><span class="sxs-lookup"><span data-stu-id="27b0b-124">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="27b0b-125">Выпуски для начальной загрузки TouchCarousel на CDN</span><span class="sxs-lookup"><span data-stu-id="27b0b-125">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="27b0b-126">Выпуски Hammer.js на CDN</span><span class="sxs-lookup"><span data-stu-id="27b0b-126">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="27b0b-127">Веб-форм ASP.NET и Ajax CDN выпуска</span><span class="sxs-lookup"><span data-stu-id="27b0b-127">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="27b0b-128">Освобождает ASP.NET MVC в CDN</span><span class="sxs-lookup"><span data-stu-id="27b0b-128">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="27b0b-129">Освобождает ASP.NET SignalR на CDN</span><span class="sxs-lookup"><span data-stu-id="27b0b-129">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="27b0b-130">Microsoft Ajax доставки содержимого сети (CDN) размещает популярных сторонних библиотек JavaScript, например jQuery и позволяет легко добавлять их к веб-приложениям.</span><span class="sxs-lookup"><span data-stu-id="27b0b-130">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="27b0b-131">Например, можно приступить к использованию jQuery, который размещается на этом CDN, добавив &lt;сценарий&gt; тег, указывающий на ajax.aspnetcdn.com страницу.</span><span class="sxs-lookup"><span data-stu-id="27b0b-131">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="27b0b-132">Используя преимущества сети CDN, может значительно повысить производительность приложений Ajax.</span><span class="sxs-lookup"><span data-stu-id="27b0b-132">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="27b0b-133">Содержимое CDN кэшируется на серверах по всему миру.</span><span class="sxs-lookup"><span data-stu-id="27b0b-133">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="27b0b-134">Кроме того сеть позволяет браузерам повторно использовать файлы JavaScript кэшированных сторонних производителей для веб-сайтов, которые находятся в разных доменах.</span><span class="sxs-lookup"><span data-stu-id="27b0b-134">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="27b0b-135">CDN поддерживает SSL (HTTPS), на случай необходимости обслуживания веб-страницы, с помощью протокола SSL.</span><span class="sxs-lookup"><span data-stu-id="27b0b-135">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="27b0b-136">CDN размещает следующие библиотеки сценариев сторонних производителей были отправлены, а лицензируется для вас, владельцы этих библиотек:</span><span class="sxs-lookup"><span data-stu-id="27b0b-136">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="27b0b-137">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="27b0b-137">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="27b0b-138">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="27b0b-138">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="27b0b-139">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="27b0b-139">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="27b0b-140">jQuery проверки (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="27b0b-140">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="27b0b-141">jQuery цикла (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="27b0b-141">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="27b0b-142">jQuery DataTables (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="27b0b-142">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="27b0b-143">Microsoft Ajax CDN также включает следующие библиотеки, которые были отправлены корпорацией Майкрософт:</span><span class="sxs-lookup"><span data-stu-id="27b0b-143">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="27b0b-144">ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="27b0b-144">ASP.NET Ajax</span></span>
- <span data-ttu-id="27b0b-145">Файлы JavaScript в ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="27b0b-145">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="27b0b-146">Файлы ASP.NET SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="27b0b-146">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="27b0b-147">Корпорация Майкрософт не претендует владения библиотек сторонних разработчиков, размещенных на этом CDN.</span><span class="sxs-lookup"><span data-stu-id="27b0b-147">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="27b0b-148">Владельцы авторских прав библиотек Лицензирование этих библиотек.</span><span class="sxs-lookup"><span data-stu-id="27b0b-148">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="27b0b-149">Все права, которые необходимо загрузить и использовать такие библиотеки предоставляются исключительно с владельцев авторских прав.</span><span class="sxs-lookup"><span data-stu-id="27b0b-149">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="27b0b-150">Так как они не библиотек Майкрософт, корпорация Майкрософт предоставляет библиотеки сторонних производителей, размещенные на этом CDN нет никаких гарантий или лицензии права интеллектуальной собственности (включая патентные права, не подразумеваемых).</span><span class="sxs-lookup"><span data-stu-id="27b0b-150">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="27b0b-151">Если вы хотите отправить библиотеки JavaScript и библиотеки является одним из верхнего библиотек JavaScript (как указано на http://trends.builtwith.com) или расширения и подключаемые модули для этих библиотек, (a) популярных; или (б) для использования в ASP.NET, а затем обратитесь в службу AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="27b0b-151">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="27b0b-152">переименована в ajax.aspnetcdn.com AJAX.Microsoft.com</span><span class="sxs-lookup"><span data-stu-id="27b0b-152">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="27b0b-153">CDN позволяет использовать доменное имя microsoft.com и был изменен для использования имени домена aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="27b0b-153">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="27b0b-154">Это изменение было внесено в целях повышения производительности, поскольку при обращении к браузера домена microsoft.com требовалось отправить все файлы cookie из этого домена по каналу связи с каждым запросом.</span><span class="sxs-lookup"><span data-stu-id="27b0b-154">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="27b0b-155">Переименовав доменное имя, отличное от microsoft.com производительности может быть увеличена путем их 25%.</span><span class="sxs-lookup"><span data-stu-id="27b0b-155">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="27b0b-156">Обратите внимание, ajax.microsoft.com будет продолжать работать, но рекомендуется ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="27b0b-156">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="27b0b-157">Старый формат: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-157">Old Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="27b0b-158">Новый формат: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-158">New Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="27b0b-159">Поддержка .vsdoc Visual Studio</span><span class="sxs-lookup"><span data-stu-id="27b0b-159">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="27b0b-160">Файлы .vsdoc правильно использовать с Visual Studio 2008, необходимо иметь VS 2008 SP1 установлены и установлено исправление для файлов vsdoc.</span><span class="sxs-lookup"><span data-stu-id="27b0b-160">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="27b0b-161">Чтобы узнать эти здесь:</span><span class="sxs-lookup"><span data-stu-id="27b0b-161">You can get these from here:</span></span>

- [<span data-ttu-id="27b0b-162">Загрузите Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="27b0b-162">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "загрузки Visual Studio 2008 с пакетом обновления 1")
- [<span data-ttu-id="27b0b-163">Загрузка .vsdoc исправления для Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="27b0b-163">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "загрузки .vsdoc исправления для Visual Studio 2008 с пакетом обновления 1")

<span data-ttu-id="27b0b-164">Visual Studio 2010 поддерживает файлы .vsdoc без дополнительных исправлений.</span><span class="sxs-lookup"><span data-stu-id="27b0b-164">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="27b0b-165">С помощью ASP.NET Ajax из CDN</span><span class="sxs-lookup"><span data-stu-id="27b0b-165">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="27b0b-166">При использовании ASP.NET 4, можно перенаправить все запросы на сценарии ASP.NET framework CDN.</span><span class="sxs-lookup"><span data-stu-id="27b0b-166">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="27b0b-167">Получение скриптов из CDN вместо локального веб-сервера может значительно повысить производительность открытого веб-сайтов ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="27b0b-167">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="27b0b-168">Используйте свойство ScriptManager EnableCDN для перенаправления всех запросов скрипт framework ASP.NET сети Microsoft Ajax CDN:</span><span class="sxs-lookup"><span data-stu-id="27b0b-168">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="27b0b-169">С помощью jQuery из CDN</span><span class="sxs-lookup"><span data-stu-id="27b0b-169">Using jQuery from the CDN</span></span>

<span data-ttu-id="27b0b-170">Можно использовать сценарии jQuery, размещенные в CDN в веб-приложении, добавив следующий элемент скрипта к странице.</span><span class="sxs-lookup"><span data-stu-id="27b0b-170">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="27b0b-171">CDN также уменьшенный версии скриптов jQuery, который можно получить с помощью следующего элемента:</span><span class="sxs-lookup"><span data-stu-id="27b0b-171">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="27b0b-172">Чтобы разрешить страницу, чтобы возврат к загрузке jQuery из локальный путь на собственных веб-сайта, если CDN недоступен, добавьте следующий элемент сразу после элемента, ссылающиеся на CDN:</span><span class="sxs-lookup"><span data-stu-id="27b0b-172">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="27b0b-173">На следующей странице образец использует версию CDN библиотеки jQuery (с резервным подключением локальной копии) для отображения содержимого элемента div, при нажатии кнопки.</span><span class="sxs-lookup"><span data-stu-id="27b0b-173">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="27b0b-174">Дополнительные сведения о jQuery и загрузить локальную копию jQuery, посетив [jQuery](http://jquery.com/) веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="27b0b-174">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="27b0b-175">С помощью jQuery UI из CDN</span><span class="sxs-lookup"><span data-stu-id="27b0b-175">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="27b0b-176">CDN также содержит библиотеки jQuery UI.</span><span class="sxs-lookup"><span data-stu-id="27b0b-176">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="27b0b-177">Библиотека пользовательского интерфейса jQuery включает широкий набор мини-приложений и эффектов, которые можно использовать в приложениях ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="27b0b-177">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="27b0b-178">Например следующая страница иллюстрирует, как jQuery Datepicker пользовательского интерфейса в контексте приложения веб-форм ASP.NET можно использовать для отображения раскрывающийся календарь:</span><span class="sxs-lookup"><span data-stu-id="27b0b-178">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="27b0b-179">При перемещении фокуса в текстовое поле с помощью клавиатуры отображается календарь.</span><span class="sxs-lookup"><span data-stu-id="27b0b-179">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Раскрывающегося календаря с выбором дат](overview/_static/image1.png)

<span data-ttu-id="27b0b-181">Обратите внимание, что в приведенном выше коде необходимо включить три файла из CDN:</span><span class="sxs-lookup"><span data-stu-id="27b0b-181">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="27b0b-182">Библиотеки jQuery &mdash; библиотеки jQuery UI зависит от библиотеки jQuery.</span><span class="sxs-lookup"><span data-stu-id="27b0b-182">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="27b0b-183">Библиотеки jQuery необходимо добавить на страницу перед добавлением библиотеки jQuery UI.</span><span class="sxs-lookup"><span data-stu-id="27b0b-183">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="27b0b-184">Библиотека пользовательского интерфейса jQuery &mdash; библиотеки jQuery UI содержит все эффекты пользовательского интерфейса jQuery и мини-приложения, такие как Datepicker мини-приложение используется на странице выше.</span><span class="sxs-lookup"><span data-stu-id="27b0b-184">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="27b0b-185">Тема пользовательского интерфейса jQuery &mdash; jQuery UI поддерживает различные темы.</span><span class="sxs-lookup"><span data-stu-id="27b0b-185">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="27b0b-186">Страницу выше ссылка на CSS-файл для импорта Redmond темы.</span><span class="sxs-lookup"><span data-stu-id="27b0b-186">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="27b0b-187">Все темы пользовательского интерфейса standard jQuery размещаются на CDN.</span><span class="sxs-lookup"><span data-stu-id="27b0b-187">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="27b0b-188">[Эта страница содержит](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 в сети Microsoft Ajax CDN") Просмотр эскизов для каждой темы.</span><span class="sxs-lookup"><span data-stu-id="27b0b-188">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="27b0b-189">Дополнительные сведения о библиотеки jQuery UI посетите официальные [веб-сайта пользовательского интерфейса jQuery](http://jQueryUI.com "веб-сайта пользовательского интерфейса jQuery").</span><span class="sxs-lookup"><span data-stu-id="27b0b-189">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="27b0b-190">Сторонние файлы на CDN</span><span class="sxs-lookup"><span data-stu-id="27b0b-190">Third-Party Files on the CDN</span></span>

<span data-ttu-id="27b0b-191">CDN размещает некоторые из наиболее популярных сторонних библиотек JavaScript.</span><span class="sxs-lookup"><span data-stu-id="27b0b-191">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="27b0b-192">Корпорация Майкрософт не претендует владения библиотек сторонних разработчиков, размещенных на этом CDN.</span><span class="sxs-lookup"><span data-stu-id="27b0b-192">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="27b0b-193">Владельцы авторских прав библиотек Лицензирование этих библиотек.</span><span class="sxs-lookup"><span data-stu-id="27b0b-193">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="27b0b-194">Все права, которые необходимо загрузить и использовать такие библиотеки предоставляются исключительно с владельцев авторских прав.</span><span class="sxs-lookup"><span data-stu-id="27b0b-194">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="27b0b-195">Так как они не библиотек Майкрософт, корпорация Майкрософт предоставляет библиотеки сторонних производителей, размещенные на этом CDN нет никаких гарантий или лицензии права интеллектуальной собственности (включая патентные права, не подразумеваемых).</span><span class="sxs-lookup"><span data-stu-id="27b0b-195">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="27b0b-196">Выпусков jQuery в CDN</span><span class="sxs-lookup"><span data-stu-id="27b0b-196">jQuery Releases on the CDN</span></span>

<span data-ttu-id="27b0b-197">В следующих выпусках jQuery размещаются в CDN:</span><span class="sxs-lookup"><span data-stu-id="27b0b-197">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-331"></a><span data-ttu-id="27b0b-198">версия jQuery 3.3.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-198">jQuery version 3.3.1</span></span>
- <span data-ttu-id="27b0b-199">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-199">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js</span></span>
- <span data-ttu-id="27b0b-200">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-200">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js</span></span>
- <span data-ttu-id="27b0b-201">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-201">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map</span></span>
- <span data-ttu-id="27b0b-202">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.Slim.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-202">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js</span></span>
- <span data-ttu-id="27b0b-203">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-203">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js</span></span>
- <span data-ttu-id="27b0b-204">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.Slim.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-204">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map</span></span>

#### <a name="jquery-version-321"></a><span data-ttu-id="27b0b-205">версия jQuery 3.2.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-205">jQuery version 3.2.1</span></span>
- <span data-ttu-id="27b0b-206">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-206">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js</span></span>
- <span data-ttu-id="27b0b-207">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-207">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js</span></span>
- <span data-ttu-id="27b0b-208">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-208">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map</span></span>
- <span data-ttu-id="27b0b-209">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.Slim.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-209">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js</span></span>
- <span data-ttu-id="27b0b-210">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-210">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js</span></span>
- <span data-ttu-id="27b0b-211">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.Slim.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-211">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map</span></span>

#### <a name="jquery-version-320"></a><span data-ttu-id="27b0b-212">версия jQuery 3.2.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-212">jQuery version 3.2.0</span></span>

- <span data-ttu-id="27b0b-213">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-213">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js</span></span>
- <span data-ttu-id="27b0b-214">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-214">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js</span></span>
- <span data-ttu-id="27b0b-215">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-215">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map</span></span>
- <span data-ttu-id="27b0b-216">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.Slim.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-216">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js</span></span>
- <span data-ttu-id="27b0b-217">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-217">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js</span></span>
- <span data-ttu-id="27b0b-218">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.Slim.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-218">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map</span></span>

#### <a name="jquery-version-311"></a><span data-ttu-id="27b0b-219">версия jQuery 3.1.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-219">jQuery version 3.1.1</span></span>

- <span data-ttu-id="27b0b-220">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-220">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js</span></span>
- <span data-ttu-id="27b0b-221">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-221">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js</span></span>
- <span data-ttu-id="27b0b-222">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-222">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map</span></span>
- <span data-ttu-id="27b0b-223">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.Slim.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-223">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js</span></span>
- <span data-ttu-id="27b0b-224">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-224">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js</span></span>
- <span data-ttu-id="27b0b-225">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.Slim.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-225">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map</span></span>

#### <a name="jquery-version-310"></a><span data-ttu-id="27b0b-226">jQuery версии 3.1.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-226">jQuery version 3.1.0</span></span>

- <span data-ttu-id="27b0b-227">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-227">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js</span></span>
- <span data-ttu-id="27b0b-228">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-228">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js</span></span>
- <span data-ttu-id="27b0b-229">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-229">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map</span></span>
- <span data-ttu-id="27b0b-230">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.Slim.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-230">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js</span></span>
- <span data-ttu-id="27b0b-231">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-231">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js</span></span>
- <span data-ttu-id="27b0b-232">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.Slim.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-232">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map</span></span>

#### <a name="jquery-version-300"></a><span data-ttu-id="27b0b-233">версия jQuery 3.0.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-233">jQuery version 3.0.0</span></span>

- <span data-ttu-id="27b0b-234">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-234">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js</span></span>
- <span data-ttu-id="27b0b-235">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-235">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js</span></span>
- <span data-ttu-id="27b0b-236">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-236">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map</span></span>
- <span data-ttu-id="27b0b-237">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.Slim.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-237">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js</span></span>
- <span data-ttu-id="27b0b-238">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-238">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js</span></span>
- <span data-ttu-id="27b0b-239">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.Slim.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-239">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map</span></span>

#### <a name="jquery-version-224"></a><span data-ttu-id="27b0b-240">версия jQuery 2.2.4</span><span class="sxs-lookup"><span data-stu-id="27b0b-240">jQuery version 2.2.4</span></span>

- <span data-ttu-id="27b0b-241">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-241">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js</span></span>
- <span data-ttu-id="27b0b-242">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-242">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js</span></span>
- <span data-ttu-id="27b0b-243">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-243">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map</span></span>

#### <a name="jquery-version-223"></a><span data-ttu-id="27b0b-244">версия jQuery 2.2.3</span><span class="sxs-lookup"><span data-stu-id="27b0b-244">jQuery version 2.2.3</span></span>

- <span data-ttu-id="27b0b-245">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-245">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js</span></span>
- <span data-ttu-id="27b0b-246">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-246">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js</span></span>
- <span data-ttu-id="27b0b-247">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-247">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map</span></span>

#### <a name="jquery-version-222"></a><span data-ttu-id="27b0b-248">версия jQuery 2.2.2</span><span class="sxs-lookup"><span data-stu-id="27b0b-248">jQuery version 2.2.2</span></span>

- <span data-ttu-id="27b0b-249">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-249">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js</span></span>
- <span data-ttu-id="27b0b-250">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-250">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js</span></span>
- <span data-ttu-id="27b0b-251">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-251">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map</span></span>

#### <a name="jquery-version-221"></a><span data-ttu-id="27b0b-252">версия jQuery 2.2.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-252">jQuery version 2.2.1</span></span>

- <span data-ttu-id="27b0b-253">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-253">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js</span></span>
- <span data-ttu-id="27b0b-254">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-254">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js</span></span>
- <span data-ttu-id="27b0b-255">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-255">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map</span></span>

#### <a name="jquery-version-220"></a><span data-ttu-id="27b0b-256">версия jQuery 2.2.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-256">jQuery version 2.2.0</span></span>

- <span data-ttu-id="27b0b-257">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-257">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js</span></span>
- <span data-ttu-id="27b0b-258">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-258">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js</span></span>
- <span data-ttu-id="27b0b-259">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-259">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map</span></span>

#### <a name="jquery-version-214"></a><span data-ttu-id="27b0b-260">версия jQuery 2.1.4 Установка</span><span class="sxs-lookup"><span data-stu-id="27b0b-260">jQuery version 2.1.4</span></span>

- <span data-ttu-id="27b0b-261">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-261">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js</span></span>
- <span data-ttu-id="27b0b-262">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-262">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js</span></span>
- <span data-ttu-id="27b0b-263">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-263">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map</span></span>

#### <a name="jquery-version-213"></a><span data-ttu-id="27b0b-264">версия jQuery 2.1.3</span><span class="sxs-lookup"><span data-stu-id="27b0b-264">jQuery version 2.1.3</span></span>

- <span data-ttu-id="27b0b-265">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-265">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js</span></span>
- <span data-ttu-id="27b0b-266">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-266">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js</span></span>
- <span data-ttu-id="27b0b-267">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-267">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map</span></span>

#### <a name="jquery-version-212"></a><span data-ttu-id="27b0b-268">версия jQuery 2.1.2</span><span class="sxs-lookup"><span data-stu-id="27b0b-268">jQuery version 2.1.2</span></span>

- <span data-ttu-id="27b0b-269">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.2.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-269">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js</span></span>
- <span data-ttu-id="27b0b-270">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.2.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-270">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js</span></span>

#### <a name="jquery-version-211"></a><span data-ttu-id="27b0b-271">версия jQuery 2.1.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-271">jQuery version 2.1.1</span></span>

- <span data-ttu-id="27b0b-272">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-272">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js</span></span>
- <span data-ttu-id="27b0b-273">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-273">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js</span></span>
- <span data-ttu-id="27b0b-274">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-274">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map</span></span>

#### <a name="jquery-version-210"></a><span data-ttu-id="27b0b-275">версия jQuery 2.1.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-275">jQuery version 2.1.0</span></span>

- <span data-ttu-id="27b0b-276">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-276">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js</span></span>
- <span data-ttu-id="27b0b-277">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-277">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js</span></span>
- <span data-ttu-id="27b0b-278">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-278">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js</span></span>
- <span data-ttu-id="27b0b-279">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-279">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map</span></span>

#### <a name="jquery-version-203"></a><span data-ttu-id="27b0b-280">версия jQuery 2.0.3</span><span class="sxs-lookup"><span data-stu-id="27b0b-280">jQuery version 2.0.3</span></span>

- <span data-ttu-id="27b0b-281">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-281">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js</span></span>
- <span data-ttu-id="27b0b-282">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-282">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js</span></span>
- <span data-ttu-id="27b0b-283">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-283">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js</span></span>
- <span data-ttu-id="27b0b-284">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-284">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map</span></span>

#### <a name="jquery-version-202"></a><span data-ttu-id="27b0b-285">jQuery версии 2.0.2</span><span class="sxs-lookup"><span data-stu-id="27b0b-285">jQuery version 2.0.2</span></span>

- <span data-ttu-id="27b0b-286">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-286">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js</span></span>
- <span data-ttu-id="27b0b-287">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-287">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js</span></span>
- <span data-ttu-id="27b0b-288">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-288">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js</span></span>
- <span data-ttu-id="27b0b-289">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-289">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map</span></span>

#### <a name="jquery-version-201"></a><span data-ttu-id="27b0b-290">версия jQuery 2.0.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-290">jQuery version 2.0.1</span></span>

- <span data-ttu-id="27b0b-291">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-291">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js</span></span>
- <span data-ttu-id="27b0b-292">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-292">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js</span></span>
- <span data-ttu-id="27b0b-293">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-293">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js</span></span>
- <span data-ttu-id="27b0b-294">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-294">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map</span></span>

#### <a name="jquery-version-200"></a><span data-ttu-id="27b0b-295">jQuery версии 2.0.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-295">jQuery version 2.0.0</span></span>

- <span data-ttu-id="27b0b-296">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-296">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js</span></span>
- <span data-ttu-id="27b0b-297">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-297">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js</span></span>
- <span data-ttu-id="27b0b-298">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-298">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js</span></span>
- <span data-ttu-id="27b0b-299">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-299">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map</span></span>

#### <a name="jquery-version-1124"></a><span data-ttu-id="27b0b-300">версия jQuery 1.12.4</span><span class="sxs-lookup"><span data-stu-id="27b0b-300">jQuery version 1.12.4</span></span>

- <span data-ttu-id="27b0b-301">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-301">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js</span></span>
- <span data-ttu-id="27b0b-302">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-302">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js</span></span>
- <span data-ttu-id="27b0b-303">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-303">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map</span></span>

#### <a name="jquery-version-1123"></a><span data-ttu-id="27b0b-304">версия jQuery 1.12.3</span><span class="sxs-lookup"><span data-stu-id="27b0b-304">jQuery version 1.12.3</span></span>

- <span data-ttu-id="27b0b-305">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-305">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js</span></span>
- <span data-ttu-id="27b0b-306">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-306">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js</span></span>
- <span data-ttu-id="27b0b-307">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-307">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map</span></span>

#### <a name="jquery-version-1122"></a><span data-ttu-id="27b0b-308">версия jQuery 1.12.2</span><span class="sxs-lookup"><span data-stu-id="27b0b-308">jQuery version 1.12.2</span></span>

- <span data-ttu-id="27b0b-309">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-309">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js</span></span>
- <span data-ttu-id="27b0b-310">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-310">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js</span></span>
- <span data-ttu-id="27b0b-311">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-311">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map</span></span>

#### <a name="jquery-version-1121"></a><span data-ttu-id="27b0b-312">версия jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-312">jQuery version 1.12.1</span></span>

- <span data-ttu-id="27b0b-313">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-313">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js</span></span>
- <span data-ttu-id="27b0b-314">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-314">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js</span></span>
- <span data-ttu-id="27b0b-315">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-315">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map</span></span>

#### <a name="jquery-version-1120"></a><span data-ttu-id="27b0b-316">версия jQuery 1.12.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-316">jQuery version 1.12.0</span></span>

- <span data-ttu-id="27b0b-317">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-317">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js</span></span>
- <span data-ttu-id="27b0b-318">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-318">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js</span></span>
- <span data-ttu-id="27b0b-319">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-319">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map</span></span>

#### <a name="jquery-version-1113"></a><span data-ttu-id="27b0b-320">версия jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="27b0b-320">jQuery version 1.11.3</span></span>

- <span data-ttu-id="27b0b-321">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-321">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js</span></span>
- <span data-ttu-id="27b0b-322">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-322">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js</span></span>
- <span data-ttu-id="27b0b-323">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-323">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map</span></span>

#### <a name="jquery-version-1112"></a><span data-ttu-id="27b0b-324">версия jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="27b0b-324">jQuery version 1.11.2</span></span>

- <span data-ttu-id="27b0b-325">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-325">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js</span></span>
- <span data-ttu-id="27b0b-326">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-326">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js</span></span>
- <span data-ttu-id="27b0b-327">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-327">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map</span></span>

#### <a name="jquery-version-1111"></a><span data-ttu-id="27b0b-328">версия jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-328">jQuery version 1.11.1</span></span>

- <span data-ttu-id="27b0b-329">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-329">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js</span></span>
- <span data-ttu-id="27b0b-330">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-330">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js</span></span>
- <span data-ttu-id="27b0b-331">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-331">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map</span></span>

#### <a name="jquery-version-1110"></a><span data-ttu-id="27b0b-332">версия jQuery 1.11.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-332">jQuery version 1.11.0</span></span>

- <span data-ttu-id="27b0b-333">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-333">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js</span></span>
- <span data-ttu-id="27b0b-334">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-334">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js</span></span>
- <span data-ttu-id="27b0b-335">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-335">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js</span></span>
- <span data-ttu-id="27b0b-336">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-336">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map</span></span>

#### <a name="jquery-version-1102"></a><span data-ttu-id="27b0b-337">версия jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="27b0b-337">jQuery version 1.10.2</span></span>

- <span data-ttu-id="27b0b-338">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-338">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js</span></span>
- <span data-ttu-id="27b0b-339">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-339">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js</span></span>
- <span data-ttu-id="27b0b-340">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-340">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js</span></span>
- <span data-ttu-id="27b0b-341">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-341">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map</span></span>

#### <a name="jquery-version-1101"></a><span data-ttu-id="27b0b-342">версия jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-342">jQuery version 1.10.1</span></span>

- <span data-ttu-id="27b0b-343">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-343">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js</span></span>
- <span data-ttu-id="27b0b-344">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-344">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js</span></span>
- <span data-ttu-id="27b0b-345">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-345">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js</span></span>
- <span data-ttu-id="27b0b-346">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-346">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map</span></span>

#### <a name="jquery-version-1100"></a><span data-ttu-id="27b0b-347">версия jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-347">jQuery version 1.10.0</span></span>

- <span data-ttu-id="27b0b-348">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-348">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js</span></span>
- <span data-ttu-id="27b0b-349">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-349">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js</span></span>
- <span data-ttu-id="27b0b-350">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-350">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js</span></span>
- <span data-ttu-id="27b0b-351">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-351">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map</span></span>

#### <a name="jquery-version-191"></a><span data-ttu-id="27b0b-352">версия jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-352">jQuery version 1.9.1</span></span>

- <span data-ttu-id="27b0b-353">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-353">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js</span></span>
- <span data-ttu-id="27b0b-354">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-354">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js</span></span>
- <span data-ttu-id="27b0b-355">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-355">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js</span></span>
- <span data-ttu-id="27b0b-356">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-356">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map</span></span>

#### <a name="jquery-version-190"></a><span data-ttu-id="27b0b-357">версия jQuery 1.9.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-357">jQuery version 1.9.0</span></span>

- <span data-ttu-id="27b0b-358">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-358">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js</span></span>
- <span data-ttu-id="27b0b-359">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-359">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js</span></span>
- <span data-ttu-id="27b0b-360">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-360">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js</span></span>
- <span data-ttu-id="27b0b-361">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-361">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map</span></span>

#### <a name="jquery-version-183"></a><span data-ttu-id="27b0b-362">версия jQuery 1.8.3</span><span class="sxs-lookup"><span data-stu-id="27b0b-362">jQuery version 1.8.3</span></span>

- <span data-ttu-id="27b0b-363">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-363">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js</span></span>
- <span data-ttu-id="27b0b-364">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-364">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js</span></span>
- <span data-ttu-id="27b0b-365">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-365">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js</span></span>

#### <a name="jquery-version-182"></a><span data-ttu-id="27b0b-366">версия jQuery 1.8.2</span><span class="sxs-lookup"><span data-stu-id="27b0b-366">jQuery version 1.8.2</span></span>

- <span data-ttu-id="27b0b-367">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-367">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js</span></span>
- <span data-ttu-id="27b0b-368">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-368">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js</span></span>
- <span data-ttu-id="27b0b-369">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-369">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js</span></span>

#### <a name="jquery-version-181"></a><span data-ttu-id="27b0b-370">версия jQuery 1.8.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-370">jQuery version 1.8.1</span></span>

- <span data-ttu-id="27b0b-371">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-371">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js</span></span>
- <span data-ttu-id="27b0b-372">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-372">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js</span></span>
- <span data-ttu-id="27b0b-373">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-373">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js</span></span>

#### <a name="jquery-version-180"></a><span data-ttu-id="27b0b-374">версия jQuery 1.8.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-374">jQuery version 1.8.0</span></span>

- <span data-ttu-id="27b0b-375">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-375">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="27b0b-376">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-376">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js</span></span>
- <span data-ttu-id="27b0b-377">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-377">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js</span></span>

#### <a name="jquery-version-172"></a><span data-ttu-id="27b0b-378">версия jQuery 1.7.2</span><span class="sxs-lookup"><span data-stu-id="27b0b-378">jQuery version 1.7.2</span></span>

- <span data-ttu-id="27b0b-379">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.2.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-379">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js</span></span>
- <span data-ttu-id="27b0b-380">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.2.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-380">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js</span></span>

#### <a name="jquery-version-171"></a><span data-ttu-id="27b0b-381">версия jQuery 1.7.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-381">jQuery version 1.7.1</span></span>

- <span data-ttu-id="27b0b-382">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-382">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js</span></span>
- <span data-ttu-id="27b0b-383">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-383">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js</span></span>
- <span data-ttu-id="27b0b-384">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-384">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js</span></span>

#### <a name="jquery-version-17"></a><span data-ttu-id="27b0b-385">jQuery версия 1.7</span><span class="sxs-lookup"><span data-stu-id="27b0b-385">jQuery version 1.7</span></span>

- <span data-ttu-id="27b0b-386">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-386">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js</span></span>
- <span data-ttu-id="27b0b-387">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-387">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js</span></span>
- <span data-ttu-id="27b0b-388">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-388">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js</span></span>

#### <a name="jquery-version-164"></a><span data-ttu-id="27b0b-389">версия jQuery 1.6.4</span><span class="sxs-lookup"><span data-stu-id="27b0b-389">jQuery version 1.6.4</span></span>

- <span data-ttu-id="27b0b-390">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-390">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js</span></span>
- <span data-ttu-id="27b0b-391">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-391">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js</span></span>
- <span data-ttu-id="27b0b-392">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-392">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js</span></span>

#### <a name="jquery-version-163"></a><span data-ttu-id="27b0b-393">версия jQuery 1.6.3</span><span class="sxs-lookup"><span data-stu-id="27b0b-393">jQuery version 1.6.3</span></span>

- <span data-ttu-id="27b0b-394">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-394">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js</span></span>
- <span data-ttu-id="27b0b-395">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-395">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js</span></span>
- <span data-ttu-id="27b0b-396">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-396">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js</span></span>

#### <a name="jquery-version-162"></a><span data-ttu-id="27b0b-397">версия jQuery 1.6.2</span><span class="sxs-lookup"><span data-stu-id="27b0b-397">jQuery version 1.6.2</span></span>

- <span data-ttu-id="27b0b-398">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-398">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js</span></span>
- <span data-ttu-id="27b0b-399">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-399">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js</span></span>
- <span data-ttu-id="27b0b-400">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-400">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js</span></span>

#### <a name="jquery-version-161"></a><span data-ttu-id="27b0b-401">версия jQuery 1.6.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-401">jQuery version 1.6.1</span></span>

- <span data-ttu-id="27b0b-402">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-402">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js</span></span>
- <span data-ttu-id="27b0b-403">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-403">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js</span></span>
- <span data-ttu-id="27b0b-404">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-404">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js</span></span>

#### <a name="jquery-version-16"></a><span data-ttu-id="27b0b-405">jQuery версии 1.6</span><span class="sxs-lookup"><span data-stu-id="27b0b-405">jQuery version 1.6</span></span>

- <span data-ttu-id="27b0b-406">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-406">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js</span></span>
- <span data-ttu-id="27b0b-407">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-407">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js</span></span>
- <span data-ttu-id="27b0b-408">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-408">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js</span></span>

#### <a name="jquery-version-152"></a><span data-ttu-id="27b0b-409">версия jQuery 1.5.2</span><span class="sxs-lookup"><span data-stu-id="27b0b-409">jQuery version 1.5.2</span></span>

- <span data-ttu-id="27b0b-410">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-410">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js</span></span>
- <span data-ttu-id="27b0b-411">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-411">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js</span></span>
- <span data-ttu-id="27b0b-412">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-412">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js</span></span>

#### <a name="jquery-version-151"></a><span data-ttu-id="27b0b-413">версия jQuery 1.5.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-413">jQuery version 1.5.1</span></span>

- <span data-ttu-id="27b0b-414">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-414">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js</span></span>
- <span data-ttu-id="27b0b-415">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-415">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js</span></span>
- <span data-ttu-id="27b0b-416">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-416">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js</span></span>

#### <a name="jquery-version-15"></a><span data-ttu-id="27b0b-417">версии jQuery 1.5</span><span class="sxs-lookup"><span data-stu-id="27b0b-417">jQuery version 1.5</span></span>

- <span data-ttu-id="27b0b-418">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-418">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js</span></span>
- <span data-ttu-id="27b0b-419">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-419">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js</span></span>
- <span data-ttu-id="27b0b-420">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-420">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js</span></span>

#### <a name="jquery-version-144"></a><span data-ttu-id="27b0b-421">версия jQuery 1.4.4</span><span class="sxs-lookup"><span data-stu-id="27b0b-421">jQuery version 1.4.4</span></span>

- <span data-ttu-id="27b0b-422">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-422">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js</span></span>
- <span data-ttu-id="27b0b-423">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-423">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js</span></span>
- <span data-ttu-id="27b0b-424">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-424">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js</span></span>

#### <a name="jquery-version-143"></a><span data-ttu-id="27b0b-425">версия jQuery 1.4.3</span><span class="sxs-lookup"><span data-stu-id="27b0b-425">jQuery version 1.4.3</span></span>

- <span data-ttu-id="27b0b-426">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-426">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js</span></span>
- <span data-ttu-id="27b0b-427">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-427">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js</span></span>
- <span data-ttu-id="27b0b-428">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-428">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js</span></span>

#### <a name="jquery-version-142"></a><span data-ttu-id="27b0b-429">версия jQuery 1.4.2</span><span class="sxs-lookup"><span data-stu-id="27b0b-429">jQuery version 1.4.2</span></span>

- <span data-ttu-id="27b0b-430">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-430">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js</span></span>
- <span data-ttu-id="27b0b-431">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-431">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js</span></span>
- <span data-ttu-id="27b0b-432">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-432">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js</span></span>

#### <a name="jquery-version-141"></a><span data-ttu-id="27b0b-433">версия jQuery 1.4.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-433">jQuery version 1.4.1</span></span>

- <span data-ttu-id="27b0b-434">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-434">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js</span></span>
- <span data-ttu-id="27b0b-435">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-435">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js</span></span>
- <span data-ttu-id="27b0b-436">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-436">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js</span></span>

#### <a name="jquery-version-14"></a><span data-ttu-id="27b0b-437">jQuery версии 1.4</span><span class="sxs-lookup"><span data-stu-id="27b0b-437">jQuery version 1.4</span></span>

- <span data-ttu-id="27b0b-438">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-438">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js</span></span>
- <span data-ttu-id="27b0b-439">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-439">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js</span></span>

#### <a name="jquery-version-132"></a><span data-ttu-id="27b0b-440">версия jQuery 1.3.2</span><span class="sxs-lookup"><span data-stu-id="27b0b-440">jQuery version 1.3.2</span></span>

- <span data-ttu-id="27b0b-441">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-441">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js</span></span>
- <span data-ttu-id="27b0b-442">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-442">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js</span></span>
- <span data-ttu-id="27b0b-443">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-443">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js</span></span>
- <span data-ttu-id="27b0b-444">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.min-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-444">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js</span></span>

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="27b0b-445">Перенос выпусков jQuery в CDN</span><span class="sxs-lookup"><span data-stu-id="27b0b-445">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="27b0b-446">В следующих выпусках jQuery миграции размещаются в CDN:</span><span class="sxs-lookup"><span data-stu-id="27b0b-446">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="27b0b-447">jQuery версия 3.0.0 миграции</span><span class="sxs-lookup"><span data-stu-id="27b0b-447">jQuery Migrate version 3.0.0</span></span>

- <span data-ttu-id="27b0b-448">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-448">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js</span></span>
- <span data-ttu-id="27b0b-449">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-3.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-449">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js</span></span>

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="27b0b-450">jQuery версия 1.2.1 миграции</span><span class="sxs-lookup"><span data-stu-id="27b0b-450">jQuery Migrate version 1.2.1</span></span>

- <span data-ttu-id="27b0b-451">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.1.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-451">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js</span></span>
- <span data-ttu-id="27b0b-452">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-452">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js</span></span>

<span data-ttu-id="27b0b-453">jQuery миграции версии 1.2.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-453">jQuery Migrate version 1.2.0</span></span>

- <span data-ttu-id="27b0b-454">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.0.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-454">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js</span></span>
- <span data-ttu-id="27b0b-455">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-455">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js</span></span>

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="27b0b-456">jQuery миграции версии 1.1.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-456">jQuery Migrate version 1.1.1</span></span>

- <span data-ttu-id="27b0b-457">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-457">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js</span></span>
- <span data-ttu-id="27b0b-458">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-458">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js</span></span>

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="27b0b-459">jQuery миграции версии 1.1.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-459">jQuery Migrate version 1.1.0</span></span>

- <span data-ttu-id="27b0b-460">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-460">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js</span></span>
- <span data-ttu-id="27b0b-461">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-461">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js</span></span>

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="27b0b-462">jQuery версия 1.0.0 миграции</span><span class="sxs-lookup"><span data-stu-id="27b0b-462">jQuery Migrate version 1.0.0</span></span>

- <span data-ttu-id="27b0b-463">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.0.0.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-463">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js</span></span>
- <span data-ttu-id="27b0b-464">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-464">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js</span></span>

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="27b0b-465">jQuery с CDN выпуски пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="27b0b-465">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="27b0b-466">В следующих выпусках библиотеки jQuery UI размещаются на этом CDN.</span><span class="sxs-lookup"><span data-stu-id="27b0b-466">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="27b0b-467">Щелкните каждый ссылку, чтобы просмотреть фактический список файлов.</span><span class="sxs-lookup"><span data-stu-id="27b0b-467">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="27b0b-468">jQuery UI 1.12.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-468">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-469">jQuery UI 1.12.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-469">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-470">jQuery UI 1.11.4</span><span class="sxs-lookup"><span data-stu-id="27b0b-470">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-471">jQuery UI 1.11.3</span><span class="sxs-lookup"><span data-stu-id="27b0b-471">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-472">jQuery UI 1.11.2</span><span class="sxs-lookup"><span data-stu-id="27b0b-472">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-473">jQuery UI 1.11.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-473">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-474">jQuery UI 1.11.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-474">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-475">jQuery UI 1.10.4</span><span class="sxs-lookup"><span data-stu-id="27b0b-475">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-476">jQuery UI 1.10.3</span><span class="sxs-lookup"><span data-stu-id="27b0b-476">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-477">jQuery UI 1.10.2</span><span class="sxs-lookup"><span data-stu-id="27b0b-477">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery 1.10.2 пользовательского интерфейса на Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-478">jQuery UI 1.10.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-478">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-479">jQuery UI 1.10.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-479">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-480">jQuery UI 1.9.2</span><span class="sxs-lookup"><span data-stu-id="27b0b-480">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-481">jQuery UI 1.9.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-481">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-482">jQuery UI 1.9.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-482">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI 1.9.0 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-483">jQuery UI 1.8.24</span><span class="sxs-lookup"><span data-stu-id="27b0b-483">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-484">jQuery UI 1.8.23</span><span class="sxs-lookup"><span data-stu-id="27b0b-484">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-485">jQuery UI 1.8.22</span><span class="sxs-lookup"><span data-stu-id="27b0b-485">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-486">jQuery UI 1.8.21</span><span class="sxs-lookup"><span data-stu-id="27b0b-486">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-487">jQuery UI 1.8.20</span><span class="sxs-lookup"><span data-stu-id="27b0b-487">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-488">jQuery UI 1.8.19</span><span class="sxs-lookup"><span data-stu-id="27b0b-488">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-489">jQuery UI 1.8.18</span><span class="sxs-lookup"><span data-stu-id="27b0b-489">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-490">jQuery UI 1.8.17</span><span class="sxs-lookup"><span data-stu-id="27b0b-490">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-491">jQuery UI 1.8.16</span><span class="sxs-lookup"><span data-stu-id="27b0b-491">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-492">jQuery UI 1.8.15</span><span class="sxs-lookup"><span data-stu-id="27b0b-492">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-493">jQuery UI 1.8.14</span><span class="sxs-lookup"><span data-stu-id="27b0b-493">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-494">jQuery UI 1.8.13</span><span class="sxs-lookup"><span data-stu-id="27b0b-494">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-495">jQuery UI 1.8.12</span><span class="sxs-lookup"><span data-stu-id="27b0b-495">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-496">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="27b0b-496">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-497">jQuery UI 1.8.10</span><span class="sxs-lookup"><span data-stu-id="27b0b-497">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-498">jQuery UI 1.8.9</span><span class="sxs-lookup"><span data-stu-id="27b0b-498">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-499">jQuery UI 1.8.8</span><span class="sxs-lookup"><span data-stu-id="27b0b-499">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-500">jQuery UI 1.8.7</span><span class="sxs-lookup"><span data-stu-id="27b0b-500">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-501">jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="27b0b-501">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-502">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="27b0b-502">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="27b0b-503">jQuery с CDN выпуски проверки</span><span class="sxs-lookup"><span data-stu-id="27b0b-503">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="27b0b-504">Следующие выпуски библиотеки jQuery проверки размещаются на этом CDN.</span><span class="sxs-lookup"><span data-stu-id="27b0b-504">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="27b0b-505">Щелкните каждый ссылку, чтобы просмотреть фактический список файлов.</span><span class="sxs-lookup"><span data-stu-id="27b0b-505">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="27b0b-506">jQuery проверки 1.17.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-506">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery проверки 1.17.0")
- [<span data-ttu-id="27b0b-507">jQuery проверки 1.16.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-507">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery проверки 1.16.0")
- [<span data-ttu-id="27b0b-508">jQuery проверки 1.15.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-508">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery проверки 1.15.1")
- [<span data-ttu-id="27b0b-509">jQuery проверки 1.15.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-509">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery проверки 1.15.0")
- [<span data-ttu-id="27b0b-510">jQuery проверки 1.14.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-510">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery проверки 1.14.0")
- [<span data-ttu-id="27b0b-511">jQuery проверки 1.13.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-511">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery проверки 1.13.1")
- [<span data-ttu-id="27b0b-512">jQuery проверки 1.13.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-512">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery проверки 1.13.0")
- [<span data-ttu-id="27b0b-513">jQuery проверки 1.12.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-513">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery проверки 1.12.0")
- [<span data-ttu-id="27b0b-514">jQuery проверки 1.11.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-514">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery проверки 1.11.1")
- [<span data-ttu-id="27b0b-515">jQuery проверки 1.11.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-515">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery проверки 1.11.0")
- [<span data-ttu-id="27b0b-516">jQuery проверки 1.10.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-516">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery проверки 1.10.0")
- [<span data-ttu-id="27b0b-517">jQuery проверки 1.9</span><span class="sxs-lookup"><span data-stu-id="27b0b-517">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate версия 1.9")
- [<span data-ttu-id="27b0b-518">jQuery проверки 1.8.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-518">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate версии 1.8.1")
- [<span data-ttu-id="27b0b-519">jQuery проверки 1.8</span><span class="sxs-lookup"><span data-stu-id="27b0b-519">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate версии 1.8")
- [<span data-ttu-id="27b0b-520">jQuery проверки 1.7</span><span class="sxs-lookup"><span data-stu-id="27b0b-520">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate версии 1.7")
- [<span data-ttu-id="27b0b-521">jQuery проверки 1.6</span><span class="sxs-lookup"><span data-stu-id="27b0b-521">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery проверки 1.6")
- [<span data-ttu-id="27b0b-522">jQuery проверки 1.5.5</span><span class="sxs-lookup"><span data-stu-id="27b0b-522">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery проверки 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="27b0b-523">jQuery Mobile выпуски на CDN</span><span class="sxs-lookup"><span data-stu-id="27b0b-523">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="27b0b-524">Следующие выпуски мобильных библиотеки jQuery размещаются на этом CDN.</span><span class="sxs-lookup"><span data-stu-id="27b0b-524">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="27b0b-525">Щелкните каждый ссылку, чтобы просмотреть фактический список файлов.</span><span class="sxs-lookup"><span data-stu-id="27b0b-525">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="27b0b-526">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="27b0b-526">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-527">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="27b0b-527">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-528">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-528">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-529">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-529">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-530">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="27b0b-530">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-531">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-531">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-532">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-532">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-533">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-533">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-534">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="27b0b-534">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-535">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-535">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-536">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-536">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-537">jQuery Mobile 1.1.0 версии-Кандидате 2</span><span class="sxs-lookup"><span data-stu-id="27b0b-537">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-538">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-538">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-539">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-539">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-540">jQuery Mobile 1.0 версии-Кандидате 2</span><span class="sxs-lookup"><span data-stu-id="27b0b-540">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-541">jQuery Mobile 1.0 версии-Кандидата 1</span><span class="sxs-lookup"><span data-stu-id="27b0b-541">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 в сети Microsoft Ajax CDN")
- [<span data-ttu-id="27b0b-542">jQuery Mobile 1.0 бета-версии 3</span><span class="sxs-lookup"><span data-stu-id="27b0b-542">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 бета-версии 3 в сети Microsoft Ajax CDN")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="27b0b-543">jQuery с CDN выпуски шаблонов</span><span class="sxs-lookup"><span data-stu-id="27b0b-543">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="27b0b-544">Следующие выпуски подключаемый модуль шаблонов jQuery размещаются на этом CDN.</span><span class="sxs-lookup"><span data-stu-id="27b0b-544">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="27b0b-545">Щелкните каждый ссылку, чтобы просмотреть фактический список файлов.</span><span class="sxs-lookup"><span data-stu-id="27b0b-545">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="27b0b-546">jQuery шаблоны бета-версии 1</span><span class="sxs-lookup"><span data-stu-id="27b0b-546">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery шаблоны бета-версия 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="27b0b-547">jQuery с CDN выпуски цикла</span><span class="sxs-lookup"><span data-stu-id="27b0b-547">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="27b0b-548">Следующие выпуски цикл подключаемого модуля jQuery размещаются на этом CDN.</span><span class="sxs-lookup"><span data-stu-id="27b0b-548">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="27b0b-549">Щелкните каждый ссылку, чтобы просмотреть фактический список файлов.</span><span class="sxs-lookup"><span data-stu-id="27b0b-549">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="27b0b-550">jQuery цикла 2,99</span><span class="sxs-lookup"><span data-stu-id="27b0b-550">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery 2,99 цикла")
- [<span data-ttu-id="27b0b-551">jQuery цикла 2.94</span><span class="sxs-lookup"><span data-stu-id="27b0b-551">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery 2.94 цикла")
- [<span data-ttu-id="27b0b-552">jQuery 2,88 цикла</span><span class="sxs-lookup"><span data-stu-id="27b0b-552">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery 2,88 цикла")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="27b0b-553">jQuery с CDN выпуски таблицы данных</span><span class="sxs-lookup"><span data-stu-id="27b0b-553">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="27b0b-554">Следующие выпуски подключаемый модуль jQuery DataTables размещаются на этом CDN.</span><span class="sxs-lookup"><span data-stu-id="27b0b-554">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="27b0b-555">Щелкните каждый ссылку, чтобы просмотреть фактический список файлов.</span><span class="sxs-lookup"><span data-stu-id="27b0b-555">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="27b0b-556">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="27b0b-556">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="27b0b-557">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="27b0b-557">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="27b0b-558">jQuery DataTables 1.9.4</span><span class="sxs-lookup"><span data-stu-id="27b0b-558">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [<span data-ttu-id="27b0b-559">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="27b0b-559">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="27b0b-560">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="27b0b-560">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="27b0b-561">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-561">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="27b0b-562">jQuery DataTables 1.9.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-562">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [<span data-ttu-id="27b0b-563">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="27b0b-563">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="27b0b-564">Выпуски Modernizr на CDN</span><span class="sxs-lookup"><span data-stu-id="27b0b-564">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="27b0b-565">Следующие выпуски [Modernizr](http://www.modernizr.com "Modernizr") размещаются в CDN:</span><span class="sxs-lookup"><span data-stu-id="27b0b-565">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- <span data-ttu-id="27b0b-566">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.8.3.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-566">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js</span></span>
- <span data-ttu-id="27b0b-567">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.2.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-567">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js</span></span>
- <span data-ttu-id="27b0b-568">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.1.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-568">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js</span></span>
- <span data-ttu-id="27b0b-569">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.6.2.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-569">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js</span></span>
- <span data-ttu-id="27b0b-570">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-1.7-Development-only.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-570">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js</span></span>
- <span data-ttu-id="27b0b-571">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.0.6-Development-only.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-571">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js</span></span>

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="27b0b-572">Выпуски JSHint на CDN</span><span class="sxs-lookup"><span data-stu-id="27b0b-572">JSHint Releases on the CDN</span></span>

<span data-ttu-id="27b0b-573">Следующие выпуски [JSHint](http://www.jshint.com "JSHint") размещаются в CDN:</span><span class="sxs-lookup"><span data-stu-id="27b0b-573">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- <span data-ttu-id="27b0b-574">http://AJAX.aspnetcdn.com/AJAX/jshint/r07/jshint.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-574">http://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js</span></span>

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="27b0b-575">Выпуски для маскирования на CDN</span><span class="sxs-lookup"><span data-stu-id="27b0b-575">Knockout Releases on the CDN</span></span>

<span data-ttu-id="27b0b-576">Следующие выпуски [Knockout](http://www.knockoutjs.com "Knockout") размещаются в CDN:</span><span class="sxs-lookup"><span data-stu-id="27b0b-576">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- <span data-ttu-id="27b0b-577">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-577">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js</span></span>
- <span data-ttu-id="27b0b-578">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.1.Debug.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-578">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js</span></span>
- <span data-ttu-id="27b0b-579">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-579">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js</span></span>
- <span data-ttu-id="27b0b-580">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-580">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js</span></span>
- <span data-ttu-id="27b0b-581">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-581">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js</span></span>
- <span data-ttu-id="27b0b-582">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.1.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-582">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js</span></span>
- <span data-ttu-id="27b0b-583">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-583">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js</span></span>
- <span data-ttu-id="27b0b-584">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.0.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-584">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js</span></span>
- <span data-ttu-id="27b0b-585">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-585">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js</span></span>
- <span data-ttu-id="27b0b-586">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.1.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-586">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js</span></span>
- <span data-ttu-id="27b0b-587">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-587">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js</span></span>
- <span data-ttu-id="27b0b-588">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.2.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-588">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js</span></span>
- <span data-ttu-id="27b0b-589">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.3.0.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-589">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span></span>
- <span data-ttu-id="27b0b-590">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.3.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-590">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js</span></span>
- <span data-ttu-id="27b0b-591">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.0.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-591">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js</span></span>
- <span data-ttu-id="27b0b-592">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-592">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js</span></span>
- <span data-ttu-id="27b0b-593">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.1.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-593">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js</span></span>
- <span data-ttu-id="27b0b-594">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.1.Debug.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-594">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js</span></span>
- <span data-ttu-id="27b0b-595">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.2.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-595">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js</span></span>
- <span data-ttu-id="27b0b-596">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.2.Debug.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-596">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js</span></span>

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="27b0b-597">Глобализация с CDN выпуски</span><span class="sxs-lookup"><span data-stu-id="27b0b-597">Globalize Releases on the CDN</span></span>

<span data-ttu-id="27b0b-598">Следующие выпуски [Globalize](https://github.com/jquery/globalize "Globalize") размещаются в CDN:</span><span class="sxs-lookup"><span data-stu-id="27b0b-598">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="27b0b-599">Глобализация версии 1.0.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-599">Globalize version 1.0.0</span></span>

- <span data-ttu-id="27b0b-600">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-600">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js</span></span>
- <span data-ttu-id="27b0b-601">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/node-Main.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-601">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js</span></span>
- <span data-ttu-id="27b0b-602">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Currency.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-602">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js</span></span>
- <span data-ttu-id="27b0b-603">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Date.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-603">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js</span></span>
- <span data-ttu-id="27b0b-604">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Message.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-604">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js</span></span>
- <span data-ttu-id="27b0b-605">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Number.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-605">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js</span></span>
- <span data-ttu-id="27b0b-606">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/plural.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-606">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js</span></span>
- <span data-ttu-id="27b0b-607">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Relative-Time.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-607">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js</span></span>

#### <a name="globalize-version-011"></a><span data-ttu-id="27b0b-608">Глобализация версии 0.1.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-608">Globalize version 0.1.1</span></span>

- <span data-ttu-id="27b0b-609">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/globalize.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-609">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js</span></span>
- <span data-ttu-id="27b0b-610">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/globalize.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-610">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js</span></span>
- <span data-ttu-id="27b0b-611">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/cultures/globalize.cultures.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-611">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js</span></span>

    - <span data-ttu-id="27b0b-612">Все языки и региональные параметры</span><span class="sxs-lookup"><span data-stu-id="27b0b-612">all cultures</span></span>
- <span data-ttu-id="27b0b-613">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/cultures/globalize.Culture. {язык и региональные параметры кода} .js</span><span class="sxs-lookup"><span data-stu-id="27b0b-613">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js</span></span>

    - <span data-ttu-id="27b0b-614">Заменить «{язык и региональные параметры — код}» с кодом нужный язык и региональные параметры, например файлов Microsoft globalize.culture.en GB.js== в CDN == эти библиотеки были отправлены корпорацией Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="27b0b-614">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="27b0b-615">Ответ с CDN выпуски</span><span class="sxs-lookup"><span data-stu-id="27b0b-615">Respond Releases on the CDN</span></span>

<span data-ttu-id="27b0b-616">Следующие выпуски [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") ответ размещаются в CDN:</span><span class="sxs-lookup"><span data-stu-id="27b0b-616">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="27b0b-617">Ответ версии 1.4.2</span><span class="sxs-lookup"><span data-stu-id="27b0b-617">Respond version 1.4.2</span></span>

- <span data-ttu-id="27b0b-618">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.2/respond.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-618">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js</span></span>
- <span data-ttu-id="27b0b-619">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.2/respond.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-619">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js</span></span>
- <span data-ttu-id="27b0b-620">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.2/respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-620">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="27b0b-621">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.2/respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-621">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-141"></a><span data-ttu-id="27b0b-622">Ответ версии 1.4.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-622">Respond version 1.4.1</span></span>

- <span data-ttu-id="27b0b-623">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.1/respond.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-623">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js</span></span>
- <span data-ttu-id="27b0b-624">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.1/respond.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-624">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js</span></span>
- <span data-ttu-id="27b0b-625">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.1/respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-625">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="27b0b-626">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.1/respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-626">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-140"></a><span data-ttu-id="27b0b-627">Ответ версии 1.4.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-627">Respond version 1.4.0</span></span>

- <span data-ttu-id="27b0b-628">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.0/respond.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-628">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js</span></span>
- <span data-ttu-id="27b0b-629">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.0/respond.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-629">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js</span></span>
- <span data-ttu-id="27b0b-630">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.0/respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-630">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="27b0b-631">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.0/respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-631">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-130"></a><span data-ttu-id="27b0b-632">Ответ версии 1.3.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-632">Respond version 1.3.0</span></span>

- <span data-ttu-id="27b0b-633">http://AJAX.aspnetcdn.com/AJAX/respond/1.3.0/respond.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-633">http://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js</span></span>

#### <a name="respond-version-120"></a><span data-ttu-id="27b0b-634">Ответ версии 1.2.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-634">Respond version 1.2.0</span></span>

- <span data-ttu-id="27b0b-635">http://AJAX.aspnetcdn.com/AJAX/respond/1.2.0/respond.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-635">http://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js</span></span>

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="27b0b-636">Выпуски для начальной загрузки на CDN</span><span class="sxs-lookup"><span data-stu-id="27b0b-636">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="27b0b-637">Следующие выпуски [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") начальной загрузки размещаются в CDN:</span><span class="sxs-lookup"><span data-stu-id="27b0b-637">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-337"></a><span data-ttu-id="27b0b-638">Начальной загрузки версии 3.3.7</span><span class="sxs-lookup"><span data-stu-id="27b0b-638">Bootstrap version 3.3.7</span></span>

- <span data-ttu-id="27b0b-639">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-639">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js</span></span>
- <span data-ttu-id="27b0b-640">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-640">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js</span></span>
- <span data-ttu-id="27b0b-641">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-641">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css</span></span>
- <span data-ttu-id="27b0b-642">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-642">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map</span></span>
- <span data-ttu-id="27b0b-643">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-643">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css</span></span>
- <span data-ttu-id="27b0b-644">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-THEME.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-644">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="27b0b-645">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-THEME.CSS.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-645">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="27b0b-646">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-THEME.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-646">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="27b0b-647">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="27b0b-647">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="27b0b-648">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="27b0b-648">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="27b0b-649">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="27b0b-649">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="27b0b-650">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="27b0b-650">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="27b0b-651">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="27b0b-651">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-336"></a><span data-ttu-id="27b0b-652">Версия 3.3.6 начальной загрузки</span><span class="sxs-lookup"><span data-stu-id="27b0b-652">Bootstrap version 3.3.6</span></span>

- <span data-ttu-id="27b0b-653">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-653">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js</span></span>
- <span data-ttu-id="27b0b-654">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-654">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js</span></span>
- <span data-ttu-id="27b0b-655">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-655">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css</span></span>
- <span data-ttu-id="27b0b-656">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-656">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map</span></span>
- <span data-ttu-id="27b0b-657">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-657">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css</span></span>
- <span data-ttu-id="27b0b-658">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-THEME.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-658">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="27b0b-659">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-THEME.CSS.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-659">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="27b0b-660">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-THEME.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-660">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="27b0b-661">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="27b0b-661">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="27b0b-662">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="27b0b-662">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="27b0b-663">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="27b0b-663">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="27b0b-664">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="27b0b-664">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="27b0b-665">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="27b0b-665">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-335"></a><span data-ttu-id="27b0b-666">Начальной загрузки версии 3.3.5</span><span class="sxs-lookup"><span data-stu-id="27b0b-666">Bootstrap version 3.3.5</span></span>

- <span data-ttu-id="27b0b-667">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-667">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js</span></span>
- <span data-ttu-id="27b0b-668">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-668">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js</span></span>
- <span data-ttu-id="27b0b-669">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-669">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css</span></span>
- <span data-ttu-id="27b0b-670">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-670">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map</span></span>
- <span data-ttu-id="27b0b-671">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-671">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css</span></span>
- <span data-ttu-id="27b0b-672">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-THEME.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-672">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="27b0b-673">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-THEME.CSS.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-673">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="27b0b-674">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-THEME.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-674">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="27b0b-675">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="27b0b-675">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="27b0b-676">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="27b0b-676">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="27b0b-677">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="27b0b-677">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="27b0b-678">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="27b0b-678">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="27b0b-679">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="27b0b-679">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-334"></a><span data-ttu-id="27b0b-680">Начальной загрузки версии 3.3.4</span><span class="sxs-lookup"><span data-stu-id="27b0b-680">Bootstrap version 3.3.4</span></span>

- <span data-ttu-id="27b0b-681">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-681">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js</span></span>
- <span data-ttu-id="27b0b-682">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-682">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js</span></span>
- <span data-ttu-id="27b0b-683">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-683">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css</span></span>
- <span data-ttu-id="27b0b-684">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-684">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map</span></span>
- <span data-ttu-id="27b0b-685">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-685">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css</span></span>
- <span data-ttu-id="27b0b-686">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-THEME.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-686">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="27b0b-687">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-THEME.CSS.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-687">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="27b0b-688">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-THEME.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-688">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="27b0b-689">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="27b0b-689">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="27b0b-690">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="27b0b-690">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="27b0b-691">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="27b0b-691">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="27b0b-692">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="27b0b-692">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="27b0b-693">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="27b0b-693">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-332"></a><span data-ttu-id="27b0b-694">Версия 3.3.2 начальной загрузки</span><span class="sxs-lookup"><span data-stu-id="27b0b-694">Bootstrap version 3.3.2</span></span>

- <span data-ttu-id="27b0b-695">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-695">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js</span></span>
- <span data-ttu-id="27b0b-696">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-696">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="27b0b-697">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-697">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="27b0b-698">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-698">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map</span></span>
- <span data-ttu-id="27b0b-699">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-699">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="27b0b-700">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-THEME.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-700">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="27b0b-701">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-THEME.CSS.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-701">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="27b0b-702">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-THEME.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-702">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="27b0b-703">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="27b0b-703">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="27b0b-704">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="27b0b-704">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="27b0b-705">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="27b0b-705">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="27b0b-706">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="27b0b-706">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="27b0b-707">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="27b0b-707">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-331"></a><span data-ttu-id="27b0b-708">Версия 3.3.1 начальной загрузки</span><span class="sxs-lookup"><span data-stu-id="27b0b-708">Bootstrap version 3.3.1</span></span>

- <span data-ttu-id="27b0b-709">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-709">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js</span></span>
- <span data-ttu-id="27b0b-710">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-710">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="27b0b-711">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-711">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="27b0b-712">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-712">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="27b0b-713">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-713">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="27b0b-714">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-THEME.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-714">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="27b0b-715">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-THEME.CSS.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-715">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="27b0b-716">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-THEME.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-716">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="27b0b-717">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="27b0b-717">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="27b0b-718">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="27b0b-718">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="27b0b-719">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="27b0b-719">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="27b0b-720">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="27b0b-720">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-330"></a><span data-ttu-id="27b0b-721">Начальной загрузки версии 3.3.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-721">Bootstrap version 3.3.0</span></span>

- <span data-ttu-id="27b0b-722">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-722">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js</span></span>
- <span data-ttu-id="27b0b-723">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-723">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js</span></span>
- <span data-ttu-id="27b0b-724">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-724">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css</span></span>
- <span data-ttu-id="27b0b-725">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-725">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="27b0b-726">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-726">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="27b0b-727">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-THEME.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-727">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="27b0b-728">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-THEME.CSS.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-728">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="27b0b-729">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-THEME.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-729">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="27b0b-730">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="27b0b-730">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="27b0b-731">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="27b0b-731">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="27b0b-732">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="27b0b-732">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="27b0b-733">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="27b0b-733">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-320"></a><span data-ttu-id="27b0b-734">Начальной загрузки версии 3.2.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-734">Bootstrap version 3.2.0</span></span>

- <span data-ttu-id="27b0b-735">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-735">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js</span></span>
- <span data-ttu-id="27b0b-736">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-736">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js</span></span>
- <span data-ttu-id="27b0b-737">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-737">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css</span></span>
- <span data-ttu-id="27b0b-738">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-738">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="27b0b-739">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-739">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="27b0b-740">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-THEME.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-740">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="27b0b-741">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-THEME.CSS.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-741">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="27b0b-742">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-THEME.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-742">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="27b0b-743">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="27b0b-743">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="27b0b-744">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="27b0b-744">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="27b0b-745">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="27b0b-745">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="27b0b-746">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="27b0b-746">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-311"></a><span data-ttu-id="27b0b-747">Начальной загрузки версии 3.1.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-747">Bootstrap version 3.1.1</span></span>

- <span data-ttu-id="27b0b-748">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-748">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js</span></span>
- <span data-ttu-id="27b0b-749">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-749">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js</span></span>
- <span data-ttu-id="27b0b-750">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-750">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css</span></span>
- <span data-ttu-id="27b0b-751">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-751">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="27b0b-752">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-752">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="27b0b-753">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-THEME.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-753">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="27b0b-754">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-THEME.CSS.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-754">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="27b0b-755">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-THEME.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-755">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="27b0b-756">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="27b0b-756">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="27b0b-757">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="27b0b-757">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="27b0b-758">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="27b0b-758">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="27b0b-759">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="27b0b-759">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-310"></a><span data-ttu-id="27b0b-760">Начальной загрузки версии 3.1.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-760">Bootstrap version 3.1.0</span></span>

- <span data-ttu-id="27b0b-761">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-761">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js</span></span>
- <span data-ttu-id="27b0b-762">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-762">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js</span></span>
- <span data-ttu-id="27b0b-763">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-763">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css</span></span>
- <span data-ttu-id="27b0b-764">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-764">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="27b0b-765">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-765">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="27b0b-766">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-THEME.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-766">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="27b0b-767">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-THEME.CSS.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-767">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="27b0b-768">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-THEME.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-768">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="27b0b-769">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="27b0b-769">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="27b0b-770">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="27b0b-770">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="27b0b-771">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="27b0b-771">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="27b0b-772">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="27b0b-772">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-303"></a><span data-ttu-id="27b0b-773">Начальной загрузки версии 3.0.3</span><span class="sxs-lookup"><span data-stu-id="27b0b-773">Bootstrap version 3.0.3</span></span>

- <span data-ttu-id="27b0b-774">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-774">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js</span></span>
- <span data-ttu-id="27b0b-775">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-775">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js</span></span>
- <span data-ttu-id="27b0b-776">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-776">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css</span></span>
- <span data-ttu-id="27b0b-777">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-777">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css</span></span>
- <span data-ttu-id="27b0b-778">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap-THEME.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-778">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="27b0b-779">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap-THEME.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-779">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="27b0b-780">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="27b0b-780">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="27b0b-781">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="27b0b-781">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="27b0b-782">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="27b0b-782">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="27b0b-783">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="27b0b-783">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-302"></a><span data-ttu-id="27b0b-784">Версия 3.0.2 начальной загрузки</span><span class="sxs-lookup"><span data-stu-id="27b0b-784">Bootstrap version 3.0.2</span></span>

- <span data-ttu-id="27b0b-785">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-785">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js</span></span>
- <span data-ttu-id="27b0b-786">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-786">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js</span></span>
- <span data-ttu-id="27b0b-787">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-787">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css</span></span>
- <span data-ttu-id="27b0b-788">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-788">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="27b0b-789">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap-THEME.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-789">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="27b0b-790">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap-THEME.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-790">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="27b0b-791">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="27b0b-791">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="27b0b-792">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="27b0b-792">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="27b0b-793">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="27b0b-793">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="27b0b-794">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="27b0b-794">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-301"></a><span data-ttu-id="27b0b-795">Начальной загрузки версии 3.0.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-795">Bootstrap version 3.0.1</span></span>

- <span data-ttu-id="27b0b-796">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-796">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js</span></span>
- <span data-ttu-id="27b0b-797">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-797">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js</span></span>
- <span data-ttu-id="27b0b-798">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-798">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css</span></span>
- <span data-ttu-id="27b0b-799">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-799">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="27b0b-800">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap-THEME.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-800">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="27b0b-801">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap-THEME.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-801">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="27b0b-802">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="27b0b-802">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="27b0b-803">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="27b0b-803">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="27b0b-804">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="27b0b-804">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="27b0b-805">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="27b0b-805">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-300"></a><span data-ttu-id="27b0b-806">Начальной загрузки версии 3.0.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-806">Bootstrap version 3.0.0</span></span>

- <span data-ttu-id="27b0b-807">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-807">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js</span></span>
- <span data-ttu-id="27b0b-808">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-808">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js</span></span>
- <span data-ttu-id="27b0b-809">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-809">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css</span></span>
- <span data-ttu-id="27b0b-810">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-810">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="27b0b-811">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap-THEME.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-811">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="27b0b-812">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap-THEME.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-812">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="27b0b-813">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="27b0b-813">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="27b0b-814">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="27b0b-814">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="27b0b-815">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="27b0b-815">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="27b0b-816">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="27b0b-816">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-232"></a><span data-ttu-id="27b0b-817">Начальной загрузки версии 2.3.2</span><span class="sxs-lookup"><span data-stu-id="27b0b-817">Bootstrap version 2.3.2</span></span>

- <span data-ttu-id="27b0b-818">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-818">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js</span></span>
- <span data-ttu-id="27b0b-819">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-819">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="27b0b-820">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-820">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="27b0b-821">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-821">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="27b0b-822">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap-responsive.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-822">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="27b0b-823">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap-responsive.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-823">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="27b0b-824">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/img/glyphicons-halflings.PNG</span><span class="sxs-lookup"><span data-stu-id="27b0b-824">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="27b0b-825">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/img/glyphicons-halflings-White.PNG</span><span class="sxs-lookup"><span data-stu-id="27b0b-825">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png</span></span>

#### <a name="bootstrap-version-231"></a><span data-ttu-id="27b0b-826">Начальной загрузки версии 2.3.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-826">Bootstrap version 2.3.1</span></span>

- <span data-ttu-id="27b0b-827">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-827">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js</span></span>
- <span data-ttu-id="27b0b-828">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/Bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-828">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="27b0b-829">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-829">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="27b0b-830">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-830">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="27b0b-831">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap-responsive.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-831">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="27b0b-832">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap-responsive.min.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-832">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="27b0b-833">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/img/glyphicons-halflings.PNG</span><span class="sxs-lookup"><span data-stu-id="27b0b-833">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="27b0b-834">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/img/glyphicons-halflings-White.PNG</span><span class="sxs-lookup"><span data-stu-id="27b0b-834">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png</span></span>

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="27b0b-835">Выпуски для начальной загрузки TouchCarousel на CDN</span><span class="sxs-lookup"><span data-stu-id="27b0b-835">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="27b0b-836">Следующие выпуски [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") TouchCarousel начальной загрузки выпуски размещаются в CDN :</span><span class="sxs-lookup"><span data-stu-id="27b0b-836">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="27b0b-837">Начальной загрузки TouchCarousel версии 0.8.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-837">Bootstrap TouchCarousel version 0.8.0</span></span>

- <span data-ttu-id="27b0b-838">http://AJAX.aspnetcdn.com/AJAX/Bootstrap-Touch-carousel/0.8.0/CSS/Bootstrap-Touch-carousel.CSS</span><span class="sxs-lookup"><span data-stu-id="27b0b-838">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css</span></span>
- <span data-ttu-id="27b0b-839">http://AJAX.aspnetcdn.com/AJAX/Bootstrap-Touch-carousel/0.8.0/js/Bootstrap-Touch-carousel.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-839">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js</span></span>

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="27b0b-840">Выпуски Hammer.js на CDN</span><span class="sxs-lookup"><span data-stu-id="27b0b-840">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="27b0b-841">Следующие выпуски [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js выпуски размещаются в CDN:</span><span class="sxs-lookup"><span data-stu-id="27b0b-841">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="27b0b-842">Версия Hammer.js 2.0.4</span><span class="sxs-lookup"><span data-stu-id="27b0b-842">Hammer.js version 2.0.4</span></span>

- <span data-ttu-id="27b0b-843">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-843">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js</span></span>
- <span data-ttu-id="27b0b-844">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-844">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js</span></span>
- <span data-ttu-id="27b0b-845">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.min.map</span><span class="sxs-lookup"><span data-stu-id="27b0b-845">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map</span></span>

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="27b0b-846">Веб-форм ASP.NET и Ajax CDN выпуска</span><span class="sxs-lookup"><span data-stu-id="27b0b-846">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="27b0b-847">В следующих выпусках библиотеки ASP.NET Ajax, размещаются на CDN.</span><span class="sxs-lookup"><span data-stu-id="27b0b-847">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="27b0b-848">Щелкните каждый ссылку, чтобы просмотреть фактический список файлов.</span><span class="sxs-lookup"><span data-stu-id="27b0b-848">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="27b0b-849">Веб-форм ASP.NET и Ajax версии 4.5.2</span><span class="sxs-lookup"><span data-stu-id="27b0b-849">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "веб-форм ASP.NET и Ajax 4.5.2")
- [<span data-ttu-id="27b0b-850">Веб-форм ASP.NET и Ajax версии 4</span><span class="sxs-lookup"><span data-stu-id="27b0b-850">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "веб-форм ASP.NET и Ajax 4")
- [<span data-ttu-id="27b0b-851">ASP.NET Ajax версии 3.5</span><span class="sxs-lookup"><span data-stu-id="27b0b-851">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "ASP.NET Ajax 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="27b0b-852">Освобождает ASP.NET MVC в CDN</span><span class="sxs-lookup"><span data-stu-id="27b0b-852">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="27b0b-853">Следующие файлы ASP.NET MVC JavaScript, размещенных на этом CDN:</span><span class="sxs-lookup"><span data-stu-id="27b0b-853">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="27b0b-854">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="27b0b-854">ASP.NET MVC 5.2.3</span></span>

- <span data-ttu-id="27b0b-855">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-855">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="27b0b-856">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-856">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="27b0b-857">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-857">ASP.NET MVC 5.1</span></span>

- <span data-ttu-id="27b0b-858">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-858">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="27b0b-859">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-859">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="27b0b-860">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-860">ASP.NET MVC 5.0</span></span>

- <span data-ttu-id="27b0b-861">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-861">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="27b0b-862">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-862">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="27b0b-863">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-863">ASP.NET MVC 4.0</span></span>

- <span data-ttu-id="27b0b-864">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-864">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="27b0b-865">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-865">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="27b0b-866">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-866">ASP.NET MVC 3.0</span></span>

- <span data-ttu-id="27b0b-867">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.unobtrusive-AJAX.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-867">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js</span></span>
- <span data-ttu-id="27b0b-868">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.unobtrusive-AJAX.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-868">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js</span></span>
- <span data-ttu-id="27b0b-869">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-869">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="27b0b-870">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-870">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js</span></span>
- <span data-ttu-id="27b0b-871">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-871">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="27b0b-872">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-872">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="27b0b-873">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-873">ASP.NET MVC 2.0</span></span>

- <span data-ttu-id="27b0b-874">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-874">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="27b0b-875">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-875">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="27b0b-876">ASP.NET MVC 1,0</span><span class="sxs-lookup"><span data-stu-id="27b0b-876">ASP.NET MVC 1.0</span></span>

- <span data-ttu-id="27b0b-877">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-877">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="27b0b-878">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-878">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js</span></span>

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="27b0b-879">Освобождает ASP.NET SignalR на CDN</span><span class="sxs-lookup"><span data-stu-id="27b0b-879">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="27b0b-880">Следующие файлы ASP.NET SignalR JavaScript размещенных на этом CDN:</span><span class="sxs-lookup"><span data-stu-id="27b0b-880">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="27b0b-881">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="27b0b-881">ASP.NET SignalR 2.2.2</span></span>

- <span data-ttu-id="27b0b-882">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.2.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-882">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js</span></span>
- <span data-ttu-id="27b0b-883">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-883">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js</span></span>

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="27b0b-884">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-884">ASP.NET SignalR 2.2.1</span></span>

- <span data-ttu-id="27b0b-885">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-885">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js</span></span>
- <span data-ttu-id="27b0b-886">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-886">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js</span></span>

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="27b0b-887">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-887">ASP.NET SignalR 2.2.0</span></span>

- <span data-ttu-id="27b0b-888">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-888">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js</span></span>
- <span data-ttu-id="27b0b-889">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-889">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js</span></span>

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="27b0b-890">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-890">ASP.NET SignalR 2.1.0</span></span>

- <span data-ttu-id="27b0b-891">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-891">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js</span></span>
- <span data-ttu-id="27b0b-892">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-892">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js</span></span>

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="27b0b-893">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="27b0b-893">ASP.NET SignalR 2.0.3</span></span>

- <span data-ttu-id="27b0b-894">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.3.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-894">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js</span></span>
- <span data-ttu-id="27b0b-895">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-895">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js</span></span>

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="27b0b-896">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="27b0b-896">ASP.NET SignalR 2.0.2</span></span>

- <span data-ttu-id="27b0b-897">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.2.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-897">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js</span></span>
- <span data-ttu-id="27b0b-898">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-898">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js</span></span>

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="27b0b-899">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-899">ASP.NET SignalR 2.0.1</span></span>

- <span data-ttu-id="27b0b-900">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-900">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js</span></span>
- <span data-ttu-id="27b0b-901">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-901">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js</span></span>

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="27b0b-902">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-902">ASP.NET SignalR 2.0.0</span></span>

- <span data-ttu-id="27b0b-903">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-903">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js</span></span>
- <span data-ttu-id="27b0b-904">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-904">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js</span></span>

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="27b0b-905">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="27b0b-905">ASP.NET SignalR 1.1.3</span></span>

- <span data-ttu-id="27b0b-906">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.3.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-906">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js</span></span>
- <span data-ttu-id="27b0b-907">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.3.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-907">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js</span></span>

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="27b0b-908">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="27b0b-908">ASP.NET SignalR 1.1.2</span></span>

- <span data-ttu-id="27b0b-909">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.2.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-909">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js</span></span>
- <span data-ttu-id="27b0b-910">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.2.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-910">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js</span></span>

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="27b0b-911">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-911">ASP.NET SignalR 1.1.1</span></span>

- <span data-ttu-id="27b0b-912">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-912">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js</span></span>
- <span data-ttu-id="27b0b-913">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-913">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js</span></span>

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="27b0b-914">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="27b0b-914">ASP.NET SignalR 1.1.0</span></span>

- <span data-ttu-id="27b0b-915">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-915">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js</span></span>
- <span data-ttu-id="27b0b-916">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-916">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js</span></span>

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="27b0b-917">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="27b0b-917">ASP.NET SignalR 1.0.1</span></span>

- <span data-ttu-id="27b0b-918">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-918">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js</span></span>
- <span data-ttu-id="27b0b-919">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.0.1.js</span><span class="sxs-lookup"><span data-stu-id="27b0b-919">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js</span></span>

<span data-ttu-id="27b0b-920">Сведения об условиях использования CDN см. в разделе [Microsoft Ajax CDN условия использования](https://www.asp.net/terms-of-use "Microsoft Ajax CDN условия использования").</span><span class="sxs-lookup"><span data-stu-id="27b0b-920">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
