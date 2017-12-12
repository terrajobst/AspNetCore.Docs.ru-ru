---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: "Визуализации ASP.NET Web Pages сайтов (Razor) для мобильных устройств | Документы Microsoft"
author: tfitzmac
description: "В этой статье описывается создание страниц на сайте ASP.NET Web Pages (Razor), будет отображаться соответствующим образом на мобильных устройствах. Вы узнаете: как вы..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: 08b714eb2ffaefc7c7e2e5c9a7428106b231e5b7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a><span data-ttu-id="66fc1-104">Подготовка к просмотру веб-страниц (Razor) сайтов ASP.NET для мобильных устройств</span><span class="sxs-lookup"><span data-stu-id="66fc1-104">Rendering ASP.NET Web Pages (Razor) Sites for Mobile Devices</span></span>
====================
<span data-ttu-id="66fc1-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="66fc1-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="66fc1-106">В этой статье описывается создание страниц на сайте ASP.NET Web Pages (Razor), будет отображаться соответствующим образом на мобильных устройствах.</span><span class="sxs-lookup"><span data-stu-id="66fc1-106">This article describes how to create pages in an ASP.NET Web Pages (Razor) site that will render appropriately on mobile devices.</span></span>
> 
> <span data-ttu-id="66fc1-107">Что вы узнаете следующее.</span><span class="sxs-lookup"><span data-stu-id="66fc1-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="66fc1-108">Как использовать соглашение об именовании позволяет указать, что страница разработан специально для мобильных устройств.</span><span class="sxs-lookup"><span data-stu-id="66fc1-108">How to use a naming convention to specify that a page is designed specifically for mobile devices.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="66fc1-109">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="66fc1-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="66fc1-110">Веб-страниц ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="66fc1-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="66fc1-111">Этот учебник также работает с веб-страницы ASP.NET 2.</span><span class="sxs-lookup"><span data-stu-id="66fc1-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="66fc1-112">Веб-страниц ASP.NET позволяет создавать настраиваемого отображения для отображения содержимого для мобильных устройств или на других устройствах.</span><span class="sxs-lookup"><span data-stu-id="66fc1-112">ASP.NET Web Pages lets you create custom displays for rendering content on mobile or other devices.</span></span>

<span data-ttu-id="66fc1-113">— Самый простой способ создать страницу конкретного устройства в узел веб-страниц ASP.NET с помощью имени файла шаблона, следующим образом: *FileName.* *Mobile**.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="66fc1-113">The simplest way to create device-specific page in an ASP.NET Web Pages site is by using a file-naming pattern like this: *FileName.**Mobile**.cshtml*.</span></span> <span data-ttu-id="66fc1-114">Можно создать две версии страницы (например, один с именем *MyFile.cshtml* и один с именем *MyFile.Mobile.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="66fc1-114">You can create two versions of a page (for example, one named *MyFile.cshtml* and one named *MyFile.Mobile.cshtml*).</span></span> <span data-ttu-id="66fc1-115">Во время выполнения, когда мобильное устройство запрашивает *MyFile.cshtml*, ASP.NET отображает содержимое, полученное от *MyFile.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="66fc1-115">At run time, when a mobile device requests *MyFile.cshtml*, ASP.NET renders the content from *MyFile.Mobile.cshtml*.</span></span> <span data-ttu-id="66fc1-116">В противном случае *MyFile.cshtml* подготавливается к просмотру.</span><span class="sxs-lookup"><span data-stu-id="66fc1-116">Otherwise, *MyFile.cshtml* is rendered.</span></span>

<span data-ttu-id="66fc1-117">Приведенный ниже показано, как включить подготовки мобильных устройств, добавив страницу содержимого для мобильных устройств.</span><span class="sxs-lookup"><span data-stu-id="66fc1-117">The following example shows how to enable mobile rendering by adding a content page for mobile devices.</span></span> <span data-ttu-id="66fc1-118">*Page1.cshtml* включает содержимое, а также боковая панель навигации.</span><span class="sxs-lookup"><span data-stu-id="66fc1-118">*Page1.cshtml* contains content plus a navigation sidebar.</span></span> <span data-ttu-id="66fc1-119">*Page1.Mobile.cshtml* с тем же содержимым, но пропускает боковой панели.</span><span class="sxs-lookup"><span data-stu-id="66fc1-119">*Page1.Mobile.cshtml* contains the same content, but omits the sidebar.</span></span>

1. <span data-ttu-id="66fc1-120">На сайте веб-страниц ASP.NET, создайте файл с именем *Page1.cshtml* и замените следующую текущего содержимого.</span><span class="sxs-lookup"><span data-stu-id="66fc1-120">In an ASP.NET Web Pages site, create a file named *Page1.cshtml* and replace the current content with following markup.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. <span data-ttu-id="66fc1-121">Создайте файл с именем *Page1.Mobile.cshtml* и заменить существующее содержимое следующей разметкой.</span><span class="sxs-lookup"><span data-stu-id="66fc1-121">Create a file named *Page1.Mobile.cshtml* and replace the existing content with the following markup.</span></span> <span data-ttu-id="66fc1-122">Обратите внимание, что версия мобильной страницы пропускает раздел навигации для повышения подготовки к просмотру на экране меньшего размера.</span><span class="sxs-lookup"><span data-stu-id="66fc1-122">Notice that the mobile version of the page omits the navigation section for better rendering on a smaller screen.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. <span data-ttu-id="66fc1-123">Запустить браузер для настольных компьютеров и перейдите к папке *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="66fc1-123">Run a desktop browser and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="66fc1-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="66fc1-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span></span>
4. <span data-ttu-id="66fc1-125">Мобильный браузер (или его эмулятор) и перейдите к *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="66fc1-125">Run a mobile browser (or a mobile device emulator) and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="66fc1-126">(Обратите внимание, что не следует включать *.mobile.*</span><span class="sxs-lookup"><span data-stu-id="66fc1-126">(Notice that you do not include *.mobile.*</span></span> <span data-ttu-id="66fc1-127">как часть URL-адреса). Несмотря на то, что этот запрос поступает *Page1.cshtml*, ASP.NET отображает *Page1.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="66fc1-127">as part of the URL.) Even though the request is to *Page1.cshtml*, ASP.NET renders *Page1.Mobile.cshtml*.</span></span>

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="66fc1-129">Для проверки страниц для мобильных устройств, можно использовать симулятор мобильного устройства, запускаемую на настольном компьютере.</span><span class="sxs-lookup"><span data-stu-id="66fc1-129">To test mobile pages, you can use a mobile device simulator that runs on a desktop computer.</span></span> <span data-ttu-id="66fc1-130">Это средство позволяет тестировать веб-страницы, как они выглядели на мобильных устройствах (то есть, обычно с намного меньше область).</span><span class="sxs-lookup"><span data-stu-id="66fc1-130">This tool lets you test web pages as they would look on mobile devices (that is, typically with a much smaller display area).</span></span> <span data-ttu-id="66fc1-131">Является одним из примеров симулятора [надстройки переключателя агента пользователя](http://addons.mozilla.org/en-us/firefox/addon/user-agent-switcher/) для Mozilla Firefox, который позволяет моделировать различные браузеры для мобильных устройств с рабочего стола версии Firefox.</span><span class="sxs-lookup"><span data-stu-id="66fc1-131">One example of a simulator is the [User Agent Switcher add-on](http://addons.mozilla.org/en-us/firefox/addon/user-agent-switcher/) for Mozilla Firefox, which lets you emulate various mobile browsers from a desktop version of Firefox.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="66fc1-132">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="66fc1-132">Additional Resources</span></span>


<span data-ttu-id="66fc1-133">[Эмулятор Windows Phone](https://msdn.microsoft.com/en-us/library/ff402563(v=VS.92).aspx)</span><span class="sxs-lookup"><span data-stu-id="66fc1-133">[Windows Phone Emulator](https://msdn.microsoft.com/en-us/library/ff402563(v=VS.92).aspx)</span></span>
