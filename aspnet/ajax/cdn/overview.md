---
uid: ajax/cdn/overview
title: Сеть доставки содержимого Microsoft Ajax | Документация Майкрософт
author: rick-anderson
description: ''
ms.author: riande
ms.date: 10/14/2017
ms.assetid: 8935bf14-ca6d-4a4e-9dbe-b96ce74cef49
msc.legacyurl: /ajax/cdn
msc.type: content
ms.openlocfilehash: 2445d788df8ae80ccaad2dfeb19d1ccb91954d40
ms.sourcegitcommit: ec71fd5a988f927ae301813aae5ff764feb3bb6a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2019
ms.locfileid: "54249585"
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="7aa9d-102">Сеть доставки содержимого Microsoft Ajax</span><span class="sxs-lookup"><span data-stu-id="7aa9d-102">Microsoft Ajax Content Delivery Network</span></span>
====================
> [!WARNING]
> <span data-ttu-id="7aa9d-103">Рабочие приложения не должен принимать жесткие зависимости на ресурсов CDN.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-103">Production applications should not take a hard dependency on CDN assets.</span></span> <span data-ttu-id="7aa9d-104">Приложения должны проверить ссылки на средства CDN и использовать резервный ресурс, если CDN недоступен.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-104">Applications should test for the CDN asset referenced, and use a fallback asset when the CDN is not available.</span></span> 
>
> <span data-ttu-id="7aa9d-105">Сети доставки Содержимого Microsoft Ajax есть соглашение кода с использованием Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-105">The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>
>
> <span data-ttu-id="7aa9d-106">Используйте [проблема GitHub](https://github.com/aspnet/Docs/issues/5832) сообщить о проблемах с помощью сети доставки Содержимого Microsoft Ajax.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-106">Use [this GitHub issue](https://github.com/aspnet/Docs/issues/5832) to report problems with the Microsoft Ajax CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="7aa9d-107">Содержание</span><span class="sxs-lookup"><span data-stu-id="7aa9d-107">Table of Contents</span></span>

<span data-ttu-id="7aa9d-108">**[переименован в ajax.aspnetcdn.com AJAX.Microsoft.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="7aa9d-108">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="7aa9d-109">**[Поддержка .vsdoc Visual Studio](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="7aa9d-109">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="7aa9d-110">**[С помощью ASP.NET Ajax из сети CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="7aa9d-110">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="7aa9d-111">**[С помощью jQuery из сети CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="7aa9d-111">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="7aa9d-112">**[С помощью jQuery пользовательского интерфейса из сети CDN](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="7aa9d-112">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="7aa9d-113">**[Файлы в сети доставки Содержимого](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="7aa9d-113">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="7aa9d-114">Выпусков jQuery в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-114">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="7aa9d-115">Перенос выпусков jQuery, в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-115">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="7aa9d-116">jQuery выпуски пользовательского интерфейса в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-116">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="7aa9d-117">jQuery выпуски проверки в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-117">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="7aa9d-118">jQuery Mobile выпусков в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-118">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="7aa9d-119">jQuery шаблоны выпусков в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-119">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="7aa9d-120">jQuery цикла выпусков в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-120">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="7aa9d-121">jQuery DataTables выпусков в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-121">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="7aa9d-122">Выпуски Modernizr в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-122">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="7aa9d-123">Выпуски JSHint в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-123">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="7aa9d-124">Выпуски Knockout в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-124">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="7aa9d-125">Глобализация выпусков в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-125">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="7aa9d-126">Ответ выпусков в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-126">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="7aa9d-127">Выпуски начальной загрузки в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-127">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="7aa9d-128">Выпуски TouchCarousel начальной загрузки в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-128">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="7aa9d-129">Выпуски Hammer.js в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-129">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="7aa9d-130">Веб-форм ASP.NET и Ajax выпусков в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-130">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="7aa9d-131">Освобождает ASP.NET MVC в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-131">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="7aa9d-132">Освобождает ASP.NET SignalR в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-132">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="7aa9d-133">Microsoft Ajax доставки содержимого сети (CDN) размещает популярных сторонних библиотек JavaScript, например jQuery и позволяет легко добавлять их к веб-приложениям.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-133">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="7aa9d-134">Например, можно запустить с помощью jQuery, которая размещается в этой сети доставки Содержимого путем простого добавления &lt;скрипт&gt; тег на страницу, которая указывает ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-134">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="7aa9d-135">Используя преимущества сети CDN, может значительно повысить производительность приложений Ajax.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-135">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="7aa9d-136">Содержимое CDN кэшируется на серверах, расположенных по всему миру.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-136">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="7aa9d-137">Кроме того сеть позволяет браузерам повторно использовать кэшированные сторонних файлов JavaScript для веб-сайтов, которые находятся в разных доменах.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-137">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="7aa9d-138">CDN поддерживает SSL (HTTPS), при необходимости для обслуживания веб-страницы, используя протокол SSL.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-138">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="7aa9d-139">CDN размещает следующие библиотеки сценария третьих лиц, которых были переданы и пользователя есть лицензия, владельцами этих библиотек:</span><span class="sxs-lookup"><span data-stu-id="7aa9d-139">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="7aa9d-140">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="7aa9d-140">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="7aa9d-141">пользовательский Интерфейс (www.jqueryui.com) jQuery</span><span class="sxs-lookup"><span data-stu-id="7aa9d-141">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="7aa9d-142">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="7aa9d-142">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="7aa9d-143">jQuery Validation (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="7aa9d-143">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="7aa9d-144">Подключаемый модуль jQuery Cycle (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="7aa9d-144">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="7aa9d-145">jQuery DataTables (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="7aa9d-145">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="7aa9d-146">Сети доставки Содержимого Microsoft Ajax также включает в себя следующие библиотеки, которые были переданы корпорацией Майкрософт:</span><span class="sxs-lookup"><span data-stu-id="7aa9d-146">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="7aa9d-147">ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="7aa9d-147">ASP.NET Ajax</span></span>
- <span data-ttu-id="7aa9d-148">Файлы JavaScript в ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="7aa9d-148">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="7aa9d-149">Файлы ASP.NET SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="7aa9d-149">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="7aa9d-150">Microsoft не предъявляет прав собственности на любые сторонние библиотеки, размещенных в этой сети CDN.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-150">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="7aa9d-151">Владельцам авторских прав, библиотек Лицензирование эти библиотеки для вас.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-151">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="7aa9d-152">Все права, которые необходимо загрузить и использовать такие библиотеки предоставляются исключительно с владельцев авторских прав.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-152">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="7aa9d-153">Так как это не библиотеки корпорации Майкрософт, корпорация Майкрософт предоставляет не сопровождаются никакими лицензии права интеллектуальной собственности (включая не подразумеваемых патентные права) для сторонних библиотек, размещенных в этой сети CDN.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-153">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="7aa9d-154">Если вы хотите отправить библиотеки JavaScript и библиотеки является одним из верхней библиотеки JavaScript (как указано на http://trends.builtwith.com) или расширений или подключаемых модулей для этих библиотек, которые являются (a) популярных; или (б) полезно для использования в ASP.NET, а затем обратитесь в службу AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-154">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="7aa9d-155">переименован в ajax.aspnetcdn.com AJAX.Microsoft.com</span><span class="sxs-lookup"><span data-stu-id="7aa9d-155">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="7aa9d-156">CDN позволяет использовать имя домена microsoft.com, а также был изменен для использования имени домена aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-156">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="7aa9d-157">Это изменение было внесено для повышения производительности, поскольку при обращении к домена microsoft.com браузер отправляет все файлы cookie из этого домена по каналу связи с каждым запросом.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-157">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="7aa9d-158">Путем переименования домена имя, отличное от microsoft.com производительности можно увеличить, возможную на 25%.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-158">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="7aa9d-159">Обратите внимание на то, ajax.microsoft.com будет продолжать работать, но рекомендуется ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-159">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="7aa9d-160">Старый формат: https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="7aa9d-160">Old Format: https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="7aa9d-161">Новый формат: https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="7aa9d-161">New Format: https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="7aa9d-162">Поддержка .vsdoc Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7aa9d-162">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="7aa9d-163">Использовать файлы .vsdoc должным образом с помощью Visual Studio 2008, необходимо убедиться, что у вас есть VS 2008 SP1 установлены и было установлено исправление для vsdoc файлов.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-163">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="7aa9d-164">Их можно получить здесь:</span><span class="sxs-lookup"><span data-stu-id="7aa9d-164">You can get these from here:</span></span>

- [<span data-ttu-id="7aa9d-165">Скачайте Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-165">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "скачать Visual Studio 2008 с пакетом обновления 1")
- [<span data-ttu-id="7aa9d-166">Загрузки .vsdoc исправления для Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-166">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "загрузки .vsdoc исправления для Visual Studio 2008 с пакетом обновления 1")

<span data-ttu-id="7aa9d-167">Visual Studio 2010 поддерживает файлы .vsdoc без любые дополнительные исправления.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-167">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="7aa9d-168">С помощью ASP.NET Ajax из сети CDN</span><span class="sxs-lookup"><span data-stu-id="7aa9d-168">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="7aa9d-169">При использовании ASP.NET 4, можно перенаправлять все запросы на сценарии ASP.NET framework в CDN.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-169">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="7aa9d-170">Получение сценариев из сети CDN, вместо локального веб-сервера может существенно повысить производительность общедоступных веб-сайтов ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-170">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="7aa9d-171">Используйте свойство ScriptManager EnableCDN для перенаправления всех запросов скриптов ASP.NET framework в Microsoft Ajax CDN:</span><span class="sxs-lookup"><span data-stu-id="7aa9d-171">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="7aa9d-172">С помощью jQuery из сети CDN</span><span class="sxs-lookup"><span data-stu-id="7aa9d-172">Using jQuery from the CDN</span></span>

<span data-ttu-id="7aa9d-173">Можно использовать сценарии jQuery, размещенной в сети доставки Содержимого в веб-приложения путем добавления следующего элемента сценария на страницу:</span><span class="sxs-lookup"><span data-stu-id="7aa9d-173">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="7aa9d-174">CDN также включает в себя минифицированные версию jQuery скрипт, который можно получить с помощью следующего элемента:</span><span class="sxs-lookup"><span data-stu-id="7aa9d-174">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="7aa9d-175">Чтобы разрешить страницу, чтобы возврат к загрузке jQuery из локальный путь на свой веб-сайт, если CDN недоступен, добавьте следующий элемент сразу после элемента, ссылающегося на CDN:</span><span class="sxs-lookup"><span data-stu-id="7aa9d-175">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="7aa9d-176">На следующей странице образец использует CDN версию библиотеки jQuery (с резервным подключением локальную копию) для отображения содержимого элемента div, при нажатии кнопки.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-176">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="7aa9d-177">Можно узнать больше о jQuery и загрузить локальную копию jQuery, посетив [jQuery](http://jquery.com/) веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-177">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="7aa9d-178">С помощью jQuery пользовательского интерфейса из сети CDN</span><span class="sxs-lookup"><span data-stu-id="7aa9d-178">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="7aa9d-179">CDN также содержит библиотеки jQuery UI.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-179">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="7aa9d-180">Библиотека пользовательского интерфейса jQuery включает широкий набор мини-приложений и эффекты, которые можно использовать в приложениях ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-180">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="7aa9d-181">Например следующая страница иллюстрирует, как jQuery Datepicker пользовательского интерфейса в контексте приложения веб-форм ASP.NET можно использовать для отображения всплывающего календаря:</span><span class="sxs-lookup"><span data-stu-id="7aa9d-181">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="7aa9d-182">При перемещении фокуса в текстовое поле, с помощью клавиатуры, отображается календарь:</span><span class="sxs-lookup"><span data-stu-id="7aa9d-182">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Всплывающего календаря, созданных с помощью Datepicker](overview/_static/image1.png)

<span data-ttu-id="7aa9d-184">Обратите внимание на то, что должен включать три файла из сети CDN в приведенном выше коде:</span><span class="sxs-lookup"><span data-stu-id="7aa9d-184">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="7aa9d-185">Библиотека jQuery &mdash; библиотеки jQuery UI зависит от библиотеки jQuery.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-185">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="7aa9d-186">Библиотека jQuery необходимо добавить на страницу перед добавлением библиотеки jQuery UI.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-186">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="7aa9d-187">Библиотека пользовательского интерфейса jQuery &mdash; библиотеки jQuery UI содержит все эффекты пользовательского интерфейса jQuery и мини-приложений, таких как Datepicker мини-приложения, используемые в странице выше.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-187">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="7aa9d-188">Тема пользовательского интерфейса jQuery &mdash; пользовательский Интерфейс jQuery поддерживает различные темы.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-188">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="7aa9d-189">На странице выше ссылка на CSS-файл для импорта Redmond темы.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-189">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="7aa9d-190">Все темы пользовательского интерфейса standard jQuery размещаются в сети доставки Содержимого.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-190">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="7aa9d-191">[Эта страница содержит](jquery-ui/cdnjqueryui1910.md "пользовательский Интерфейс jQuery 1.8.10 в сети доставки Содержимого Microsoft Ajax") для просмотра эскизов для каждой темы.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-191">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="7aa9d-192">Дополнительные сведения о библиотеке пользовательского интерфейса jQuery, посетите официальный [веб-сайт пользовательского интерфейса jQuery](http://jQueryUI.com "веб-сайт пользовательского интерфейса jQuery").</span><span class="sxs-lookup"><span data-stu-id="7aa9d-192">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="7aa9d-193">Файлы в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-193">Third-Party Files on the CDN</span></span>

<span data-ttu-id="7aa9d-194">CDN размещает некоторые из наиболее популярных библиотек JavaScript третьих лиц.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-194">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="7aa9d-195">Microsoft не предъявляет прав собственности на любые сторонние библиотеки, размещенных в этой сети CDN.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-195">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="7aa9d-196">Владельцам авторских прав, библиотек Лицензирование эти библиотеки для вас.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-196">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="7aa9d-197">Все права, которые необходимо загрузить и использовать такие библиотеки предоставляются исключительно с владельцев авторских прав.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-197">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="7aa9d-198">Так как это не библиотеки корпорации Майкрософт, корпорация Майкрософт предоставляет не сопровождаются никакими лицензии права интеллектуальной собственности (включая не подразумеваемых патентные права) для сторонних библиотек, размещенных в этой сети CDN.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-198">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="7aa9d-199">Выпусков jQuery в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-199">jQuery Releases on the CDN</span></span>

<span data-ttu-id="7aa9d-200">Следующие версии jQuery размещаются в сети доставки Содержимого:</span><span class="sxs-lookup"><span data-stu-id="7aa9d-200">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-331"></a><span data-ttu-id="7aa9d-201">версия jQuery 3.3.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-201">jQuery version 3.3.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map

#### <a name="jquery-version-321"></a><span data-ttu-id="7aa9d-202">jQuery версии 3.2.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-202">jQuery version 3.2.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map

#### <a name="jquery-version-320"></a><span data-ttu-id="7aa9d-203">jQuery версии 3.2.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-203">jQuery version 3.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map

#### <a name="jquery-version-311"></a><span data-ttu-id="7aa9d-204">версия jQuery 3.1.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-204">jQuery version 3.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map

#### <a name="jquery-version-310"></a><span data-ttu-id="7aa9d-205">jQuery версии 3.1.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-205">jQuery version 3.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map

#### <a name="jquery-version-300"></a><span data-ttu-id="7aa9d-206">jQuery версии 3.0.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-206">jQuery version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map

#### <a name="jquery-version-224"></a><span data-ttu-id="7aa9d-207">версия jQuery 2.2.4</span><span class="sxs-lookup"><span data-stu-id="7aa9d-207">jQuery version 2.2.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map

#### <a name="jquery-version-223"></a><span data-ttu-id="7aa9d-208">jQuery версии 2.2.3</span><span class="sxs-lookup"><span data-stu-id="7aa9d-208">jQuery version 2.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map

#### <a name="jquery-version-222"></a><span data-ttu-id="7aa9d-209">jQuery версии 2.2.2</span><span class="sxs-lookup"><span data-stu-id="7aa9d-209">jQuery version 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map

#### <a name="jquery-version-221"></a><span data-ttu-id="7aa9d-210">версия jQuery 2.2.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-210">jQuery version 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map

#### <a name="jquery-version-220"></a><span data-ttu-id="7aa9d-211">jQuery версии 2.2.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-211">jQuery version 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map

#### <a name="jquery-version-214"></a><span data-ttu-id="7aa9d-212">jQuery версия 2.1.4</span><span class="sxs-lookup"><span data-stu-id="7aa9d-212">jQuery version 2.1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map

#### <a name="jquery-version-213"></a><span data-ttu-id="7aa9d-213">jQuery версии 2.1.3</span><span class="sxs-lookup"><span data-stu-id="7aa9d-213">jQuery version 2.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map

#### <a name="jquery-version-212"></a><span data-ttu-id="7aa9d-214">jQuery версии 2.1.2</span><span class="sxs-lookup"><span data-stu-id="7aa9d-214">jQuery version 2.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js

#### <a name="jquery-version-211"></a><span data-ttu-id="7aa9d-215">версия jQuery 2.1.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-215">jQuery version 2.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map

#### <a name="jquery-version-210"></a><span data-ttu-id="7aa9d-216">jQuery версии 2.1.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-216">jQuery version 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map

#### <a name="jquery-version-203"></a><span data-ttu-id="7aa9d-217">jQuery версии 2.0.3</span><span class="sxs-lookup"><span data-stu-id="7aa9d-217">jQuery version 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map

#### <a name="jquery-version-202"></a><span data-ttu-id="7aa9d-218">jQuery версии 2.0.2</span><span class="sxs-lookup"><span data-stu-id="7aa9d-218">jQuery version 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map

#### <a name="jquery-version-201"></a><span data-ttu-id="7aa9d-219">jQuery версии 2.0.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-219">jQuery version 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map

#### <a name="jquery-version-200"></a><span data-ttu-id="7aa9d-220">jQuery версии 2.0.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-220">jQuery version 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map

#### <a name="jquery-version-1124"></a><span data-ttu-id="7aa9d-221">версия jQuery 1.12.4</span><span class="sxs-lookup"><span data-stu-id="7aa9d-221">jQuery version 1.12.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map

#### <a name="jquery-version-1123"></a><span data-ttu-id="7aa9d-222">версия jQuery 1.12.3</span><span class="sxs-lookup"><span data-stu-id="7aa9d-222">jQuery version 1.12.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map

#### <a name="jquery-version-1122"></a><span data-ttu-id="7aa9d-223">версия jQuery 1.12.2</span><span class="sxs-lookup"><span data-stu-id="7aa9d-223">jQuery version 1.12.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map

#### <a name="jquery-version-1121"></a><span data-ttu-id="7aa9d-224">версия jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-224">jQuery version 1.12.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map

#### <a name="jquery-version-1120"></a><span data-ttu-id="7aa9d-225">версия jQuery 1.12.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-225">jQuery version 1.12.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map

#### <a name="jquery-version-1113"></a><span data-ttu-id="7aa9d-226">версия jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="7aa9d-226">jQuery version 1.11.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map

#### <a name="jquery-version-1112"></a><span data-ttu-id="7aa9d-227">версия jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="7aa9d-227">jQuery version 1.11.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map

#### <a name="jquery-version-1111"></a><span data-ttu-id="7aa9d-228">версия jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-228">jQuery version 1.11.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map

#### <a name="jquery-version-1110"></a><span data-ttu-id="7aa9d-229">jQuery версии 1.11.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-229">jQuery version 1.11.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map

#### <a name="jquery-version-1102"></a><span data-ttu-id="7aa9d-230">версия jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="7aa9d-230">jQuery version 1.10.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map

#### <a name="jquery-version-1101"></a><span data-ttu-id="7aa9d-231">версия jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-231">jQuery version 1.10.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map

#### <a name="jquery-version-1100"></a><span data-ttu-id="7aa9d-232">версия jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-232">jQuery version 1.10.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map

#### <a name="jquery-version-191"></a><span data-ttu-id="7aa9d-233">версия jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-233">jQuery version 1.9.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map

#### <a name="jquery-version-190"></a><span data-ttu-id="7aa9d-234">версия jQuery 1.9.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-234">jQuery version 1.9.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map

#### <a name="jquery-version-183"></a><span data-ttu-id="7aa9d-235">версия jQuery 1.8.3</span><span class="sxs-lookup"><span data-stu-id="7aa9d-235">jQuery version 1.8.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js

#### <a name="jquery-version-182"></a><span data-ttu-id="7aa9d-236">версия jQuery 1.8.2</span><span class="sxs-lookup"><span data-stu-id="7aa9d-236">jQuery version 1.8.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js

#### <a name="jquery-version-181"></a><span data-ttu-id="7aa9d-237">jQuery версии 1.8.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-237">jQuery version 1.8.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js

#### <a name="jquery-version-180"></a><span data-ttu-id="7aa9d-238">jQuery версии 1.8.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-238">jQuery version 1.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js

#### <a name="jquery-version-172"></a><span data-ttu-id="7aa9d-239">версия jQuery 1.7.2</span><span class="sxs-lookup"><span data-stu-id="7aa9d-239">jQuery version 1.7.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js

#### <a name="jquery-version-171"></a><span data-ttu-id="7aa9d-240">версия jQuery 1.7.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-240">jQuery version 1.7.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js

#### <a name="jquery-version-17"></a><span data-ttu-id="7aa9d-241">версия jQuery 1.7</span><span class="sxs-lookup"><span data-stu-id="7aa9d-241">jQuery version 1.7</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js

#### <a name="jquery-version-164"></a><span data-ttu-id="7aa9d-242">версия jQuery 1.6.4</span><span class="sxs-lookup"><span data-stu-id="7aa9d-242">jQuery version 1.6.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js

#### <a name="jquery-version-163"></a><span data-ttu-id="7aa9d-243">версия jQuery 1.6.3</span><span class="sxs-lookup"><span data-stu-id="7aa9d-243">jQuery version 1.6.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js

#### <a name="jquery-version-162"></a><span data-ttu-id="7aa9d-244">версия jQuery 1.6.2</span><span class="sxs-lookup"><span data-stu-id="7aa9d-244">jQuery version 1.6.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js

#### <a name="jquery-version-161"></a><span data-ttu-id="7aa9d-245">jQuery версии 1.6.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-245">jQuery version 1.6.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js

#### <a name="jquery-version-16"></a><span data-ttu-id="7aa9d-246">jQuery версии 1.6</span><span class="sxs-lookup"><span data-stu-id="7aa9d-246">jQuery version 1.6</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js

#### <a name="jquery-version-152"></a><span data-ttu-id="7aa9d-247">версия jQuery 1.5.2</span><span class="sxs-lookup"><span data-stu-id="7aa9d-247">jQuery version 1.5.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js

#### <a name="jquery-version-151"></a><span data-ttu-id="7aa9d-248">версия jQuery 1.5.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-248">jQuery version 1.5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js

#### <a name="jquery-version-15"></a><span data-ttu-id="7aa9d-249">версии jQuery 1.5</span><span class="sxs-lookup"><span data-stu-id="7aa9d-249">jQuery version 1.5</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js

#### <a name="jquery-version-144"></a><span data-ttu-id="7aa9d-250">версия jQuery 1.4.4</span><span class="sxs-lookup"><span data-stu-id="7aa9d-250">jQuery version 1.4.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js

#### <a name="jquery-version-143"></a><span data-ttu-id="7aa9d-251">версия jQuery 1.4.3</span><span class="sxs-lookup"><span data-stu-id="7aa9d-251">jQuery version 1.4.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js

#### <a name="jquery-version-142"></a><span data-ttu-id="7aa9d-252">версия jQuery 1.4.2</span><span class="sxs-lookup"><span data-stu-id="7aa9d-252">jQuery version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js

#### <a name="jquery-version-141"></a><span data-ttu-id="7aa9d-253">версия jQuery 1.4.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-253">jQuery version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js

#### <a name="jquery-version-14"></a><span data-ttu-id="7aa9d-254">jQuery версии 1.4</span><span class="sxs-lookup"><span data-stu-id="7aa9d-254">jQuery version 1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js

#### <a name="jquery-version-132"></a><span data-ttu-id="7aa9d-255">jQuery версия 1.3.2</span><span class="sxs-lookup"><span data-stu-id="7aa9d-255">jQuery version 1.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="7aa9d-256">Перенос выпусков jQuery, в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-256">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="7aa9d-257">Следующие версии jQuery миграции размещаются в сети доставки Содержимого:</span><span class="sxs-lookup"><span data-stu-id="7aa9d-257">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="7aa9d-258">jQuery миграции версии 3.0.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-258">jQuery Migrate version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="7aa9d-259">jQuery версии 1.2.1 "Миграция"</span><span class="sxs-lookup"><span data-stu-id="7aa9d-259">jQuery Migrate version 1.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js

<span data-ttu-id="7aa9d-260">jQuery версии 1.2.0 "Миграция"</span><span class="sxs-lookup"><span data-stu-id="7aa9d-260">jQuery Migrate version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="7aa9d-261">jQuery версии 1.1.1 "Миграция"</span><span class="sxs-lookup"><span data-stu-id="7aa9d-261">jQuery Migrate version 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="7aa9d-262">jQuery версии 1.1.0 "Миграция"</span><span class="sxs-lookup"><span data-stu-id="7aa9d-262">jQuery Migrate version 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="7aa9d-263">jQuery версии 1.0.0 "Миграция"</span><span class="sxs-lookup"><span data-stu-id="7aa9d-263">jQuery Migrate version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="7aa9d-264">jQuery выпуски пользовательского интерфейса в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-264">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="7aa9d-265">Следующие версии библиотеки jQuery UI размещаются в этой сети доставки Содержимого.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-265">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="7aa9d-266">Щелкните каждый ссылку, чтобы просмотреть фактический список файлов.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-266">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="7aa9d-267">пользовательский Интерфейс jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-267">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "пользовательский Интерфейс jQuery 1.12.1 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-268">пользовательский Интерфейс jQuery 1.12.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-268">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "пользовательский Интерфейс jQuery 1.12.0 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-269">пользовательский Интерфейс jQuery 1.11.4</span><span class="sxs-lookup"><span data-stu-id="7aa9d-269">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "пользовательский Интерфейс jQuery 1.11.4 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-270">пользовательский Интерфейс jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="7aa9d-270">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "пользовательский Интерфейс jQuery 1.11.3 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-271">пользовательский Интерфейс jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="7aa9d-271">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "пользовательский Интерфейс jQuery 1.11.2 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-272">пользовательский Интерфейс jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-272">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "пользовательский Интерфейс jQuery 1.11.1 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-273">пользовательский Интерфейс 1.11.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="7aa9d-273">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "пользовательский Интерфейс jQuery 1.11.0 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-274">пользовательский Интерфейс jQuery 1.10.4</span><span class="sxs-lookup"><span data-stu-id="7aa9d-274">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "пользовательский Интерфейс jQuery 1.10.4 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-275">пользовательский Интерфейс jQuery 1.10.3</span><span class="sxs-lookup"><span data-stu-id="7aa9d-275">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "пользовательский Интерфейс jQuery 1.10.3 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-276">пользовательский Интерфейс jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="7aa9d-276">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "пользовательский Интерфейс jQuery 1.10.2 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-277">пользовательский Интерфейс jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-277">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "пользовательский Интерфейс jQuery 1.10.1 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-278">пользовательский Интерфейс jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-278">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "пользовательский Интерфейс jQuery 1.10.0 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-279">пользовательский Интерфейс jQuery 1.9.2</span><span class="sxs-lookup"><span data-stu-id="7aa9d-279">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "пользовательский Интерфейс jQuery 1.9.2 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-280">пользовательский Интерфейс jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-280">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "пользовательский Интерфейс jQuery 1.9.1 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-281">пользовательский Интерфейс jQuery 1.9.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-281">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "пользовательский Интерфейс jQuery 1.9.0 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-282">пользовательский Интерфейс jQuery 1.8.24</span><span class="sxs-lookup"><span data-stu-id="7aa9d-282">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "пользовательский Интерфейс jQuery 1.8.24 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-283">пользовательский Интерфейс jQuery 1.8.23</span><span class="sxs-lookup"><span data-stu-id="7aa9d-283">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "пользовательский Интерфейс jQuery 1.8.23 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-284">пользовательский Интерфейс jQuery 1.8.22</span><span class="sxs-lookup"><span data-stu-id="7aa9d-284">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "пользовательский Интерфейс jQuery 1.8.22 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-285">пользовательский Интерфейс jQuery 1.8.21</span><span class="sxs-lookup"><span data-stu-id="7aa9d-285">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "пользовательский Интерфейс jQuery 1.8.21 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-286">пользовательский Интерфейс jQuery 1.8.20</span><span class="sxs-lookup"><span data-stu-id="7aa9d-286">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "пользовательский Интерфейс jQuery 1.8.20 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-287">пользовательский Интерфейс jQuery 1.8.19</span><span class="sxs-lookup"><span data-stu-id="7aa9d-287">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "пользовательский Интерфейс jQuery 1.8.19 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-288">пользовательский Интерфейс jQuery 1.8.18</span><span class="sxs-lookup"><span data-stu-id="7aa9d-288">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "пользовательский Интерфейс jQuery 1.8.18 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-289">пользовательский Интерфейс jQuery 1.8.17</span><span class="sxs-lookup"><span data-stu-id="7aa9d-289">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "пользовательский Интерфейс jQuery 1.8.17 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-290">пользовательский Интерфейс jQuery 1.8.16</span><span class="sxs-lookup"><span data-stu-id="7aa9d-290">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "пользовательский Интерфейс jQuery 1.8.16 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-291">пользовательский Интерфейс jQuery 1.8.15</span><span class="sxs-lookup"><span data-stu-id="7aa9d-291">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "пользовательский Интерфейс jQuery 1.8.15 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-292">пользовательский Интерфейс jQuery 1.8.14</span><span class="sxs-lookup"><span data-stu-id="7aa9d-292">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "пользовательский Интерфейс jQuery 1.8.14 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-293">пользовательский Интерфейс jQuery 1.8.13</span><span class="sxs-lookup"><span data-stu-id="7aa9d-293">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "пользовательский Интерфейс jQuery 1.8.13 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-294">пользовательский Интерфейс jQuery 1.8.12</span><span class="sxs-lookup"><span data-stu-id="7aa9d-294">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "пользовательский Интерфейс jQuery 1.8.12 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-295">пользовательский Интерфейс jQuery 1.8.11</span><span class="sxs-lookup"><span data-stu-id="7aa9d-295">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "пользовательский Интерфейс jQuery 1.8.11 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-296">пользовательский Интерфейс jQuery 1.8.10</span><span class="sxs-lookup"><span data-stu-id="7aa9d-296">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "пользовательский Интерфейс jQuery 1.8.10 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-297">пользовательский Интерфейс jQuery 1.8.9</span><span class="sxs-lookup"><span data-stu-id="7aa9d-297">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "пользовательский Интерфейс jQuery 1.8.9 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-298">пользовательский Интерфейс jQuery 1.8.8</span><span class="sxs-lookup"><span data-stu-id="7aa9d-298">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "пользовательский Интерфейс jQuery 1.8.8 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-299">пользовательский Интерфейс jQuery 1.8.7</span><span class="sxs-lookup"><span data-stu-id="7aa9d-299">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "пользовательский Интерфейс jQuery 1.8.7 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-300">пользовательский Интерфейс jQuery 1.8.6</span><span class="sxs-lookup"><span data-stu-id="7aa9d-300">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "пользовательский Интерфейс jQuery 1.8.6 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-301">пользовательский Интерфейс jQuery 1.8.5</span><span class="sxs-lookup"><span data-stu-id="7aa9d-301">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "пользовательский Интерфейс jQuery 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="7aa9d-302">jQuery выпуски проверки в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-302">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="7aa9d-303">Следующие версии библиотеки проверки jQuery размещаются в этой сети доставки Содержимого.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-303">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="7aa9d-304">Щелкните каждый ссылку, чтобы просмотреть фактический список файлов.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-304">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="7aa9d-305">Подключаемый модуль jQuery Validate 1.19.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-305">jQuery Validate 1.19.0</span></span>](jquery-validate/cdnjqueryvalidate1190.md "проверки 1.19.0 jQuery")
- [<span data-ttu-id="7aa9d-306">Подключаемый модуль jQuery Validate 1.17.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-306">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "проверки 1.17.0 jQuery")
- [<span data-ttu-id="7aa9d-307">Подключаемый модуль jQuery Validate версии 1.16.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-307">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery Validate 1.16.0")
- [<span data-ttu-id="7aa9d-308">Подключаемый модуль jQuery Validate 1.15.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-308">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery Validate 1.15.1")
- [<span data-ttu-id="7aa9d-309">Подключаемый модуль jQuery Validate 1.15.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-309">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery Validate 1.15.0")
- [<span data-ttu-id="7aa9d-310">Подключаемый модуль jQuery Validate 1.14.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-310">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery Validate 1.14.0")
- [<span data-ttu-id="7aa9d-311">Подключаемый модуль jQuery Validate 1.13.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-311">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery Validate 1.13.1")
- [<span data-ttu-id="7aa9d-312">Подключаемый модуль jQuery Validate 1.13.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-312">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "проверки 1.13.0 jQuery")
- [<span data-ttu-id="7aa9d-313">Подключаемый модуль jQuery Validate 1.12.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-313">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery 1.12.0 проверки")
- [<span data-ttu-id="7aa9d-314">Подключаемый модуль jQuery Validate 1.11.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-314">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery 1.11.1 проверки")
- [<span data-ttu-id="7aa9d-315">Подключаемый модуль jQuery Validate 1.11.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-315">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "проверки 1.11.0 jQuery")
- [<span data-ttu-id="7aa9d-316">Подключаемый модуль jQuery Validate 1.10.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-316">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery 1.10.0 проверки")
- [<span data-ttu-id="7aa9d-317">Подключаемый модуль jQuery Validate 1.9</span><span class="sxs-lookup"><span data-stu-id="7aa9d-317">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate версии 1.9")
- [<span data-ttu-id="7aa9d-318">Подключаемый модуль jQuery Validate 1.8.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-318">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate версии 1.8.1")
- [<span data-ttu-id="7aa9d-319">Подключаемый модуль jQuery Validate 1.8</span><span class="sxs-lookup"><span data-stu-id="7aa9d-319">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate версии 1.8")
- [<span data-ttu-id="7aa9d-320">Подключаемый модуль jQuery Validate 1.7</span><span class="sxs-lookup"><span data-stu-id="7aa9d-320">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate версии 1.7")
- [<span data-ttu-id="7aa9d-321">Подключаемый модуль jQuery Validate 1.6</span><span class="sxs-lookup"><span data-stu-id="7aa9d-321">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "подключаемый модуль jQuery Validate 1.6")
- [<span data-ttu-id="7aa9d-322">Подключаемый модуль jQuery Validate 1.5.5</span><span class="sxs-lookup"><span data-stu-id="7aa9d-322">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "подключаемый модуль jQuery Validate 1.5.5.")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="7aa9d-323">jQuery Mobile выпусков в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-323">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="7aa9d-324">Следующие версии библиотеки jQuery мобильных размещаются в этой сети доставки Содержимого.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-324">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="7aa9d-325">Щелкните каждый ссылку, чтобы просмотреть фактический список файлов.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-325">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="7aa9d-326">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="7aa9d-326">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-327">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="7aa9d-327">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-328">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-328">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-329">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-329">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-330">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="7aa9d-330">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-331">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-331">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-332">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-332">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-333">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-333">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-334">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="7aa9d-334">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-335">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-335">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-336">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-336">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-337">jQuery Mobile 1.1.0 версия-Кандидат 2</span><span class="sxs-lookup"><span data-stu-id="7aa9d-337">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-338">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-338">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-339">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-339">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-340">jQuery Mobile 1.0 версии-Кандидата 2</span><span class="sxs-lookup"><span data-stu-id="7aa9d-340">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-341">jQuery Mobile 1.0 версии-Кандидата 1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-341">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 в сети доставки Содержимого Microsoft Ajax")
- [<span data-ttu-id="7aa9d-342">jQuery Mobile 1.0 бета-версия 3</span><span class="sxs-lookup"><span data-stu-id="7aa9d-342">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 бета-версии 3 в сети доставки Содержимого Microsoft Ajax")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="7aa9d-343">jQuery шаблоны выпусков в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-343">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="7aa9d-344">Следующие версии подключаемого модуля jQuery шаблоны размещаются в этой сети доставки Содержимого.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-344">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="7aa9d-345">Щелкните каждый ссылку, чтобы просмотреть фактический список файлов.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-345">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="7aa9d-346">jQuery шаблоны бета-версия 1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-346">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery шаблоны бета-версии 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="7aa9d-347">jQuery цикла выпусков в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-347">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="7aa9d-348">Следующие версии подключаемого модуля цикл jQuery размещаются в этой сети доставки Содержимого.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-348">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="7aa9d-349">Щелкните каждый ссылку, чтобы просмотреть фактический список файлов.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-349">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="7aa9d-350">jQuery цикла 2.99</span><span class="sxs-lookup"><span data-stu-id="7aa9d-350">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery цикла 2.99")
- [<span data-ttu-id="7aa9d-351">jQuery цикла 2.94</span><span class="sxs-lookup"><span data-stu-id="7aa9d-351">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery 2.94 цикла")
- [<span data-ttu-id="7aa9d-352">jQuery 2,88 цикла</span><span class="sxs-lookup"><span data-stu-id="7aa9d-352">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery 2,88 цикла")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="7aa9d-353">jQuery DataTables выпусков в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-353">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="7aa9d-354">В следующих выпусках подключаемый модуль jQuery DataTables размещаются в этой сети доставки Содержимого.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-354">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="7aa9d-355">Щелкните каждый ссылку, чтобы просмотреть фактический список файлов.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-355">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="7aa9d-356">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="7aa9d-356">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="7aa9d-357">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="7aa9d-357">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="7aa9d-358">jQuery DataTables 1.9.4</span><span class="sxs-lookup"><span data-stu-id="7aa9d-358">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [<span data-ttu-id="7aa9d-359">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="7aa9d-359">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="7aa9d-360">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="7aa9d-360">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="7aa9d-361">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-361">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="7aa9d-362">jQuery DataTables 1.9.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-362">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [<span data-ttu-id="7aa9d-363">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="7aa9d-363">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="7aa9d-364">Выпуски Modernizr в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-364">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="7aa9d-365">Следующие выпуски [Modernizr](http://www.modernizr.com "Modernizr") размещаются в сети доставки Содержимого:</span><span class="sxs-lookup"><span data-stu-id="7aa9d-365">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-3.5.0.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="7aa9d-366">Выпуски JSHint в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-366">JSHint Releases on the CDN</span></span>

<span data-ttu-id="7aa9d-367">Следующие выпуски [JSHint](http://www.jshint.com "JSHint") размещаются в сети доставки Содержимого:</span><span class="sxs-lookup"><span data-stu-id="7aa9d-367">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="7aa9d-368">Выпуски Knockout в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-368">Knockout Releases on the CDN</span></span>

<span data-ttu-id="7aa9d-369">Следующие выпуски [Knockout](http://www.knockoutjs.com "Knockout") размещаются в сети доставки Содержимого:</span><span class="sxs-lookup"><span data-stu-id="7aa9d-369">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="7aa9d-370">Глобализация выпусков в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-370">Globalize Releases on the CDN</span></span>

<span data-ttu-id="7aa9d-371">Следующие выпуски [Globalize](https://github.com/jquery/globalize "Globalize") размещаются в сети доставки Содержимого:</span><span class="sxs-lookup"><span data-stu-id="7aa9d-371">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="7aa9d-372">Глобализация версии 1.0.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-372">Globalize version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js

#### <a name="globalize-version-011"></a><span data-ttu-id="7aa9d-373">Глобализация версии 0.1.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-373">Globalize version 0.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js

    - <span data-ttu-id="7aa9d-374">Все языки и региональные параметры</span><span class="sxs-lookup"><span data-stu-id="7aa9d-374">all cultures</span></span>
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js

    - <span data-ttu-id="7aa9d-375">Замените «{языка и региональных параметров — код}» с кодом нужного языка и региональных параметров, например Microsoft globalize.culture.en GB.js== файлов в CDN == эти библиотеки были отправлены корпорацией Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-375">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="7aa9d-376">Ответ выпусков в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-376">Respond Releases on the CDN</span></span>

<span data-ttu-id="7aa9d-377">Следующие выпуски [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") ответ размещаются в сети доставки Содержимого:</span><span class="sxs-lookup"><span data-stu-id="7aa9d-377">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="7aa9d-378">Ответ версии 1.4.2</span><span class="sxs-lookup"><span data-stu-id="7aa9d-378">Respond version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js

#### <a name="respond-version-141"></a><span data-ttu-id="7aa9d-379">Ответ версии 1.4.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-379">Respond version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js

#### <a name="respond-version-140"></a><span data-ttu-id="7aa9d-380">Ответ версии 1.4.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-380">Respond version 1.4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js

#### <a name="respond-version-130"></a><span data-ttu-id="7aa9d-381">Ответ версии 1.3.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-381">Respond version 1.3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js

#### <a name="respond-version-120"></a><span data-ttu-id="7aa9d-382">Ответ версии 1.2.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-382">Respond version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="7aa9d-383">Выпуски начальной загрузки в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-383">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="7aa9d-384">Следующие выпуски [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap размещаются в сети доставки Содержимого:</span><span class="sxs-lookup"><span data-stu-id="7aa9d-384">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-411"></a><span data-ttu-id="7aa9d-385">Начальной загрузки версии 4.1.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-385">Bootstrap version 4.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-400"></a><span data-ttu-id="7aa9d-386">Начальной загрузки версии 4.0.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-386">Bootstrap version 4.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-337"></a><span data-ttu-id="7aa9d-387">Начальной загрузки версии 3.3.7</span><span class="sxs-lookup"><span data-stu-id="7aa9d-387">Bootstrap version 3.3.7</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-336"></a><span data-ttu-id="7aa9d-388">Начальной загрузки версии 3.3.6</span><span class="sxs-lookup"><span data-stu-id="7aa9d-388">Bootstrap version 3.3.6</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-335"></a><span data-ttu-id="7aa9d-389">Начальной загрузки версии 3.3.5</span><span class="sxs-lookup"><span data-stu-id="7aa9d-389">Bootstrap version 3.3.5</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-334"></a><span data-ttu-id="7aa9d-390">Начальной загрузки версии 3.3.4</span><span class="sxs-lookup"><span data-stu-id="7aa9d-390">Bootstrap version 3.3.4</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-332"></a><span data-ttu-id="7aa9d-391">Начальной загрузки версии 3.3.2</span><span class="sxs-lookup"><span data-stu-id="7aa9d-391">Bootstrap version 3.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-331"></a><span data-ttu-id="7aa9d-392">Начальной загрузки версии 3.3.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-392">Bootstrap version 3.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-330"></a><span data-ttu-id="7aa9d-393">Начальной загрузки версии 3.3.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-393">Bootstrap version 3.3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-320"></a><span data-ttu-id="7aa9d-394">Начальной загрузки версии 3.2.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-394">Bootstrap version 3.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-311"></a><span data-ttu-id="7aa9d-395">Начальной загрузки версии 3.1.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-395">Bootstrap version 3.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-310"></a><span data-ttu-id="7aa9d-396">Начальной загрузки версии 3.1.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-396">Bootstrap version 3.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-303"></a><span data-ttu-id="7aa9d-397">Начальной загрузки версии 3.0.3</span><span class="sxs-lookup"><span data-stu-id="7aa9d-397">Bootstrap version 3.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-302"></a><span data-ttu-id="7aa9d-398">Начальной загрузки версии 3.0.2</span><span class="sxs-lookup"><span data-stu-id="7aa9d-398">Bootstrap version 3.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-301"></a><span data-ttu-id="7aa9d-399">Начальной загрузки версии 3.0.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-399">Bootstrap version 3.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-300"></a><span data-ttu-id="7aa9d-400">Начальной загрузки версии 3.0.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-400">Bootstrap version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-232"></a><span data-ttu-id="7aa9d-401">Начальной загрузки версии 2.3.2</span><span class="sxs-lookup"><span data-stu-id="7aa9d-401">Bootstrap version 2.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png

#### <a name="bootstrap-version-231"></a><span data-ttu-id="7aa9d-402">Начальной загрузки версии 2.3.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-402">Bootstrap version 2.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="7aa9d-403">Выпуски TouchCarousel начальной загрузки в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-403">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="7aa9d-404">Следующие выпуски [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel выпуски размещаются в сети доставки Содержимого:</span><span class="sxs-lookup"><span data-stu-id="7aa9d-404">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="7aa9d-405">TouchCarousel начальной загрузки версии 0.8.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-405">Bootstrap TouchCarousel version 0.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css
- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="7aa9d-406">Выпуски Hammer.js в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-406">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="7aa9d-407">Следующие выпуски [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js выпуски размещаются в сети доставки Содержимого:</span><span class="sxs-lookup"><span data-stu-id="7aa9d-407">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="7aa9d-408">Hammer.js версии 2.0.4</span><span class="sxs-lookup"><span data-stu-id="7aa9d-408">Hammer.js version 2.0.4</span></span>

- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="7aa9d-409">Веб-форм ASP.NET и Ajax выпусков в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-409">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="7aa9d-410">Следующие версии ASP.NET Ajax Library размещаются в сети доставки Содержимого.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-410">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="7aa9d-411">Щелкните каждый ссылку, чтобы просмотреть фактический список файлов.</span><span class="sxs-lookup"><span data-stu-id="7aa9d-411">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="7aa9d-412">Версии веб-форм ASP.NET и Ajax 4.5.2</span><span class="sxs-lookup"><span data-stu-id="7aa9d-412">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "веб-форм ASP.NET и Ajax 4.5.2")
- [<span data-ttu-id="7aa9d-413">Версии веб-форм ASP.NET и Ajax 4</span><span class="sxs-lookup"><span data-stu-id="7aa9d-413">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "веб-форм ASP.NET и Ajax 4")
- [<span data-ttu-id="7aa9d-414">Ajax для ASP.NET версии 3.5</span><span class="sxs-lookup"><span data-stu-id="7aa9d-414">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "Ajax для ASP.NET 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="7aa9d-415">Освобождает ASP.NET MVC в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-415">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="7aa9d-416">Следующие файлы ASP.NET MVC JavaScript, размещенных в этой сети доставки Содержимого:</span><span class="sxs-lookup"><span data-stu-id="7aa9d-416">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="7aa9d-417">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="7aa9d-417">ASP.NET MVC 5.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="7aa9d-418">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-418">ASP.NET MVC 5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="7aa9d-419">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-419">ASP.NET MVC 5.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="7aa9d-420">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-420">ASP.NET MVC 4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="7aa9d-421">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-421">ASP.NET MVC 3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js
- https://ajax.aspnetcdn.com/ajax/jquery.unobtrusive-ajax/3.2.5/jquery.unobtrusive-ajax.js
- https://ajax.aspnetcdn.com/ajax/jquery.unobtrusive-ajax/3.2.5/jquery.unobtrusive-ajax.min.js 
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js
- https://ajax.aspnetcdn.com/ajax/jquery.validation.unobtrusive/3.2.10/jquery.validate.unobtrusive.js 
- https://ajax.aspnetcdn.com/ajax/jquery.validation.unobtrusive/3.2.10/jquery.validate.unobtrusive.min.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="7aa9d-422">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-422">ASP.NET MVC 2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="7aa9d-423">ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-423">ASP.NET MVC 1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="7aa9d-424">Освобождает ASP.NET SignalR в сети доставки Содержимого</span><span class="sxs-lookup"><span data-stu-id="7aa9d-424">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="7aa9d-425">Следующие файлы ASP.NET SignalR JavaScript размещаются в этой сети доставки Содержимого:</span><span class="sxs-lookup"><span data-stu-id="7aa9d-425">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="7aa9d-426">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="7aa9d-426">ASP.NET SignalR 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="7aa9d-427">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-427">ASP.NET SignalR 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="7aa9d-428">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-428">ASP.NET SignalR 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="7aa9d-429">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-429">ASP.NET SignalR 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="7aa9d-430">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="7aa9d-430">ASP.NET SignalR 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="7aa9d-431">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="7aa9d-431">ASP.NET SignalR 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="7aa9d-432">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-432">ASP.NET SignalR 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="7aa9d-433">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-433">ASP.NET SignalR 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="7aa9d-434">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="7aa9d-434">ASP.NET SignalR 1.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="7aa9d-435">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="7aa9d-435">ASP.NET SignalR 1.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="7aa9d-436">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-436">ASP.NET SignalR 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="7aa9d-437">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="7aa9d-437">ASP.NET SignalR 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="7aa9d-438">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="7aa9d-438">ASP.NET SignalR 1.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js

<span data-ttu-id="7aa9d-439">Сведения об условиях использования сети CDN, см. в разделе [Microsoft Ajax CDN условия использования](https://www.asp.net/terms-of-use "Microsoft Ajax CDN условия использования").</span><span class="sxs-lookup"><span data-stu-id="7aa9d-439">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
