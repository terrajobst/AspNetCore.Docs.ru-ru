---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: Отображение карт веб-ASP.NET страниц узла (Razor) | Документация Майкрософт
author: Rick-Anderson
description: В этой статье объясняется, как отображение интерактивной карты на страницах в на веб-сайте ASP.NET Web Pages (Razor), на базе сопоставлений служб, предоставляемых Bing, Google, Ma...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: cde27c54b11ee91b193dffd61e3a354c6cf2449a
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020992"
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="8c9df-103">Отображение карт на сайте ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="8c9df-103">Displaying Maps in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="8c9df-104">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="8c9df-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="8c9df-105">В этой статье объясняется, как отображение интерактивной карты на страницах в на веб-сайте ASP.NET Web Pages (Razor), на базе служб, предоставляемых Bing, Google, Yahoo и MapQuest сопоставлений.</span><span class="sxs-lookup"><span data-stu-id="8c9df-105">This article explains how to display interactive maps on pages in an ASP.NET Web Pages (Razor) website based on mapping services provided by Bing, Google, MapQuest, and Yahoo.</span></span>
> 
> <span data-ttu-id="8c9df-106">Вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="8c9df-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="8c9df-107">Как создать карту, на основе адреса.</span><span class="sxs-lookup"><span data-stu-id="8c9df-107">How to generate a map based on an address.</span></span>
> - <span data-ttu-id="8c9df-108">Как создать карту, на основе координат широты и долготы.</span><span class="sxs-lookup"><span data-stu-id="8c9df-108">How to generate a map based on latitude and longitude coordinates.</span></span>
> - <span data-ttu-id="8c9df-109">Как зарегистрировать учетную запись разработчика Bing Maps и получить ключ для использования с помощью Bing Maps.</span><span class="sxs-lookup"><span data-stu-id="8c9df-109">How to register a Bing Maps Developer Account and get a key to use with Bing Maps.</span></span>
> 
> <span data-ttu-id="8c9df-110">Это функция ASP.NET, впервые представленная в этой статье:</span><span class="sxs-lookup"><span data-stu-id="8c9df-110">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="8c9df-111">`Maps` Вспомогательный.</span><span class="sxs-lookup"><span data-stu-id="8c9df-111">The `Maps` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8c9df-112">Версии программного обеспечения, используемые в этом руководстве</span><span class="sxs-lookup"><span data-stu-id="8c9df-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="8c9df-113">Веб-страниц ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="8c9df-113">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="8c9df-114">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="8c9df-114">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="8c9df-115">Этот учебник также работает с WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="8c9df-115">This tutorial also works with WebMatrix 3.</span></span>


<span data-ttu-id="8c9df-116">На веб-страницах, можно отобразить карты на странице с помощью `Maps` вспомогательный.</span><span class="sxs-lookup"><span data-stu-id="8c9df-116">In Web Pages, you can display maps on a page by using `Maps` helper.</span></span> <span data-ttu-id="8c9df-117">Вы можете создать сопоставления, либо на основе адреса либо набор координат широты и долготы.</span><span class="sxs-lookup"><span data-stu-id="8c9df-117">You can generate maps based either on an address or on a set of longitude and latitude coordinates.</span></span> <span data-ttu-id="8c9df-118">`Maps` Класса позволяет вызывать в том числе Bing, Google, Yahoo и MapQuest модули популярных карты.</span><span class="sxs-lookup"><span data-stu-id="8c9df-118">The `Maps` class lets you call into popular map engines including Bing, Google, MapQuest, and Yahoo.</span></span>

<span data-ttu-id="8c9df-119">Инструкции по добавлению сопоставления на страницу одинаковы независимо от того, какое ядро карты вы вызываете.</span><span class="sxs-lookup"><span data-stu-id="8c9df-119">The steps for adding mapping to a page are the same regardless of which of the map engines you call.</span></span> <span data-ttu-id="8c9df-120">Просто добавьте ссылку на файл JavaScript, который делает доступные методы для отображения карты, а затем вызвать методы `Maps` вспомогательный.</span><span class="sxs-lookup"><span data-stu-id="8c9df-120">You just add a JavaScript file reference that makes available methods to display the map, and then you call methods of the `Maps` helper.</span></span>

<span data-ttu-id="8c9df-121">Выберите схемы услуги, на основании которого `Maps` используется вспомогательный метод.</span><span class="sxs-lookup"><span data-stu-id="8c9df-121">You choose a map service based on which `Maps` helper method you use.</span></span> <span data-ttu-id="8c9df-122">Можно использовать любой из следующих:</span><span class="sxs-lookup"><span data-stu-id="8c9df-122">You can use any of these:</span></span>

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a><span data-ttu-id="8c9df-123">Установка необходимых компонентов</span><span class="sxs-lookup"><span data-stu-id="8c9df-123">Installing the Pieces You Need</span></span>

<span data-ttu-id="8c9df-124">Чтобы отобразить карты, вам потребуется все эти элементы:</span><span class="sxs-lookup"><span data-stu-id="8c9df-124">To display maps, you need these pieces:</span></span>

- <span data-ttu-id="8c9df-125">`Maps` Вспомогательный.</span><span class="sxs-lookup"><span data-stu-id="8c9df-125">The `Maps` helper.</span></span> <span data-ttu-id="8c9df-126">Во второй версии библиотеку ASP.NET — этого вспомогательного объекта.</span><span class="sxs-lookup"><span data-stu-id="8c9df-126">This helper is in version 2 of the ASP.NET Web Helpers Library.</span></span> <span data-ttu-id="8c9df-127">Если вы еще не уже добавили библиотеку, можно установить на сайте как пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="8c9df-127">If you haven't already added the library, you can install it in your site as a NuGet package.</span></span> <span data-ttu-id="8c9df-128">Дополнительные сведения см. в разделе [Установка вспомогательных функций на сайте веб-страниц ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372).</span><span class="sxs-lookup"><span data-stu-id="8c9df-128">For details, see [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372).</span></span> <span data-ttu-id="8c9df-129">(В коллекции, поиск `microsoft-web-helpers` пакета.)</span><span class="sxs-lookup"><span data-stu-id="8c9df-129">(In the Gallery, search for the `microsoft-web-helpers` package.)</span></span>
- <span data-ttu-id="8c9df-130">Библиотека jQuery.</span><span class="sxs-lookup"><span data-stu-id="8c9df-130">The jQuery library.</span></span> <span data-ttu-id="8c9df-131">Несколько шаблонов узла WebMatrix уже включают библиотеки jQuery в их *скрипт* папки.</span><span class="sxs-lookup"><span data-stu-id="8c9df-131">Several of the WebMatrix site templates already include jQuery libraries in their *Script* folders.</span></span> <span data-ttu-id="8c9df-132">Если у вас нет этих библиотек, можно загрузить последнюю версию библиотеки jQuery непосредственно из [jQuery.org](http://jQuery.org) сайта.</span><span class="sxs-lookup"><span data-stu-id="8c9df-132">If you do not have these libraries, you can download the latest jQuery library directly from the [jQuery.org](http://jQuery.org) site.</span></span> <span data-ttu-id="8c9df-133">Или можно создать новый сайт с помощью шаблона (например, **начального сайта** шаблона) и скопируйте файлы jQuery с этого сайта в свой текущий сайт.</span><span class="sxs-lookup"><span data-stu-id="8c9df-133">Or you can create a new site using a template (for example, the **Starter Site** template) and then copy the jQuery files from that site to your current site.</span></span>

<span data-ttu-id="8c9df-134">Наконец Если вы хотите использовать Bing maps, необходимо сначала создать учетную запись (бесплатно) и получить ключ.</span><span class="sxs-lookup"><span data-stu-id="8c9df-134">Finally, if you want to use Bing maps, you must first create a (free) account and get a key.</span></span> <span data-ttu-id="8c9df-135">Чтобы получить ключ, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="8c9df-135">To get a key, follow these steps:</span></span>

1. <span data-ttu-id="8c9df-136">Создание учетной записи на [учетной записи разработчика Bing Maps](https://www.microsoft.com/maps/developers/web.aspx).</span><span class="sxs-lookup"><span data-stu-id="8c9df-136">Create an account on the [Bing Maps Developer Account](https://www.microsoft.com/maps/developers/web.aspx).</span></span> <span data-ttu-id="8c9df-137">Необходимо иметь учетную запись Майкрософт (Windows Live ID) также.</span><span class="sxs-lookup"><span data-stu-id="8c9df-137">You must have a Microsoft account (Windows Live ID) as well.</span></span>

    <span data-ttu-id="8c9df-138">Можно указать, что вы хотите использовать ключ для **оценки и тестирования**.</span><span class="sxs-lookup"><span data-stu-id="8c9df-138">You can specify that you want to use the key for **Evaluation/Test**.</span></span> <span data-ttu-id="8c9df-139">Если вы тестируете функцию сопоставления на компьютере с использованием WebMatrix и IIS Express, перейдите **сайта** рабочей области и запишите URL-адрес веб-сайта (например, `http://localhost:50408`, несмотря на то, что этот номер порта, скорее всего, будут отличаться).</span><span class="sxs-lookup"><span data-stu-id="8c9df-139">If you are testing the mapping function on your own computer using WebMatrix and IIS Express, go the **Site** workspace and note the URL of your site (for example, `http://localhost:50408`, although your port number will probably be different).</span></span> <span data-ttu-id="8c9df-140">Это можно использовать *localhost* адрес узла при регистрации.</span><span class="sxs-lookup"><span data-stu-id="8c9df-140">You can use this *localhost* address as the site when you register.</span></span>
2. <span data-ttu-id="8c9df-141">После регистрации для учетной записи, перейдите в центр учетных записей Bing Maps и нажмите кнопку **Create или view ключи**:</span><span class="sxs-lookup"><span data-stu-id="8c9df-141">After you have registered for an account, go to the Bing Maps Account Center and click **Create or view keys**:</span></span>

    ![сопоставление-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. <span data-ttu-id="8c9df-143">Запишите ключ, который создает Bing.</span><span class="sxs-lookup"><span data-stu-id="8c9df-143">Record the key that Bing creates.</span></span>

## <a name="creating-a-map-based-on-an-address-using-google"></a><span data-ttu-id="8c9df-144">Создание карты на основе адреса (с помощью Google)</span><span class="sxs-lookup"><span data-stu-id="8c9df-144">Creating a Map Based on an Address (Using Google)</span></span>

<span data-ttu-id="8c9df-145">В следующем примере показано, как создать страницу, которая подготавливает к просмотру схему, на основе адреса.</span><span class="sxs-lookup"><span data-stu-id="8c9df-145">The following example shows how to create a page that renders a map based on an address.</span></span> <span data-ttu-id="8c9df-146">В этом примере показано, как использовать Google Maps.</span><span class="sxs-lookup"><span data-stu-id="8c9df-146">This example shows how to use Google Maps.</span></span>

1. <span data-ttu-id="8c9df-147">Создайте файл с именем *MapAddress.cshtml* в корень сайта.</span><span class="sxs-lookup"><span data-stu-id="8c9df-147">Create a file named *MapAddress.cshtml* in the root of the site.</span></span> <span data-ttu-id="8c9df-148">Эта страница будет создать карту, на основе адреса, который передается в него.</span><span class="sxs-lookup"><span data-stu-id="8c9df-148">This page will generate a map based on an address that you pass to it.</span></span>
2. <span data-ttu-id="8c9df-149">Скопируйте следующий код в файл, перезаписав существующее содержимое.</span><span class="sxs-lookup"><span data-stu-id="8c9df-149">Copy the following code into the file, overwriting the existing content.</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="8c9df-150">Обратите внимание, что следующие функции страницы:</span><span class="sxs-lookup"><span data-stu-id="8c9df-150">Notice the following features of the page:</span></span>

    - <span data-ttu-id="8c9df-151">`<script>` Элемент в `<head>` элемент.</span><span class="sxs-lookup"><span data-stu-id="8c9df-151">The `<script>` element in the `<head>` element.</span></span> <span data-ttu-id="8c9df-152">В примере `<script>` ссылок на элементы *jquery-1.6.4.min.js* файл, который представляет собой минифицированные (сжатого) версию библиотеки jQuery, версии 1.6.4.</span><span class="sxs-lookup"><span data-stu-id="8c9df-152">In the example, the `<script>` element references the *jquery-1.6.4.min.js* file, which is a minified (compressed) version of the jQuery library, version 1.6.4.</span></span> <span data-ttu-id="8c9df-153">Обратите внимание, что ссылка предполагает, что *.js* файл находится в *сценариев* папки сайта.</span><span class="sxs-lookup"><span data-stu-id="8c9df-153">Note that the reference assumes that the *.js* file is in the *Scripts* folder of your site.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="8c9df-154">Если вы используете другую версию библиотеки jQuery, убедитесь, что вы ссылаются на эту версию правильно.</span><span class="sxs-lookup"><span data-stu-id="8c9df-154">If you're using a different version of the jQuery library, just make sure that you're pointing to that version correctly.</span></span>
    - <span data-ttu-id="8c9df-155">Вызов `@Maps.GetGoogleHtml` в основной области страницы.</span><span class="sxs-lookup"><span data-stu-id="8c9df-155">The call to the `@Maps.GetGoogleHtml` in the body of the page.</span></span> <span data-ttu-id="8c9df-156">Для сопоставления адреса, необходимо передать строку адреса.</span><span class="sxs-lookup"><span data-stu-id="8c9df-156">To map an address, you must pass an address string.</span></span> <span data-ttu-id="8c9df-157">Методы для других модулей карты работать так же, как (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span><span class="sxs-lookup"><span data-stu-id="8c9df-157">The methods for the other map engines work in a similar way (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span></span>
3. <span data-ttu-id="8c9df-158">Откройте страницу и ввести адрес.</span><span class="sxs-lookup"><span data-stu-id="8c9df-158">Run the page and enter an address.</span></span> <span data-ttu-id="8c9df-159">На странице отображается карту, в зависимости от Google карты, отображающего указанное расположение.</span><span class="sxs-lookup"><span data-stu-id="8c9df-159">The page displays a map, based on Google Maps, that shows the location that you specified.</span></span>

     ![сопоставление-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a><span data-ttu-id="8c9df-161">Создание карты на основе широты и долготы координирует (с помощью Bing)</span><span class="sxs-lookup"><span data-stu-id="8c9df-161">Creating a Map Based on Latitude and Longitude Coordinates (Using Bing)</span></span>

<span data-ttu-id="8c9df-162">В этом примере показано, как создать карту, на основе координат.</span><span class="sxs-lookup"><span data-stu-id="8c9df-162">This example shows how to create a map based on coordinates.</span></span> <span data-ttu-id="8c9df-163">В этом примере показано, как использовать карты Bing и как включить ваш ключ Bing.</span><span class="sxs-lookup"><span data-stu-id="8c9df-163">This example shows how to use Bing maps and how to include your Bing key.</span></span> <span data-ttu-id="8c9df-164">(Можно создать карту, на основе координат, кроме того, с помощью других модулей карты без использования ключа Bing.)</span><span class="sxs-lookup"><span data-stu-id="8c9df-164">(You can create a map based on coordinates using the other map engines also, without using a Bing key.)</span></span>

1. <span data-ttu-id="8c9df-165">Создайте файл с именем *MapCoordinates.cshtml* в корневом каталоге сайта и заменить существующее содержимое со следующими кода и разметки:</span><span class="sxs-lookup"><span data-stu-id="8c9df-165">Create a file named *MapCoordinates.cshtml* in the root of the site and replace the existing content with the following code and markup:</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. <span data-ttu-id="8c9df-166">Замените `your-key-here` на ключ Bing Maps, созданные в более ранних версий.</span><span class="sxs-lookup"><span data-stu-id="8c9df-166">Replace `your-key-here` with the Bing Maps key that you generated earlier.</span></span>
3. <span data-ttu-id="8c9df-167">Запустите *MapCoordinates.cshtml* странице, введите координаты широты и долготы и нажмите кнопку **Map It!**</span><span class="sxs-lookup"><span data-stu-id="8c9df-167">Run the *MapCoordinates.cshtml* page, enter latitude and longitude coordinates, and then click the **Map It!**</span></span> <span data-ttu-id="8c9df-168">.</span><span class="sxs-lookup"><span data-stu-id="8c9df-168">button.</span></span> <span data-ttu-id="8c9df-169">(Если вы не знаете все координаты, сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="8c9df-169">(If you don't know any coordinates, try the following.</span></span> <span data-ttu-id="8c9df-170">Это расположение на Microsoft Редмонд.)</span><span class="sxs-lookup"><span data-stu-id="8c9df-170">This is a location on the Microsoft Redmond campus.)</span></span>

   - <span data-ttu-id="8c9df-171">Широта: 47.6781005859375</span><span class="sxs-lookup"><span data-stu-id="8c9df-171">Latitude: 47.6781005859375</span></span>
   - <span data-ttu-id="8c9df-172">Долготы:-122.158317565918</span><span class="sxs-lookup"><span data-stu-id="8c9df-172">Longitude: -122.158317565918</span></span>

     <span data-ttu-id="8c9df-173">Эта страница отображается с помощью координат, которые вы указали.</span><span class="sxs-lookup"><span data-stu-id="8c9df-173">The page is displayed using the coordinates that you specified.</span></span>

     ![сопоставление-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="8c9df-175">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="8c9df-175">Additional Resources</span></span>


[<span data-ttu-id="8c9df-176">Справочник по API Microsoft.Maps</span><span class="sxs-lookup"><span data-stu-id="8c9df-176">Microsoft.Maps API Reference</span></span>](https://msdn.microsoft.com/library/gg427611.aspx)
