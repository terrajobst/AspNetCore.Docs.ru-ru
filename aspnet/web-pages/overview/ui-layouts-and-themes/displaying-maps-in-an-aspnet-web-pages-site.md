---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: Отображение карты в ASP.NET Web Pages (Razor) сайта | Документы Microsoft
author: tfitzmac
description: В этой статье объясняется, как отображать интерактивные карты на страницах в зависимости от служб, предоставляемых Bing, Google, Ma сопоставление веб-сайта ASP.NET Web Pages (Razor)...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 608dab8760bad7b877ab6fd4f89b21e980f5b1db
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30893866"
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="4a737-103">Отображение карты в веб-страницы (Razor) узла ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4a737-103">Displaying Maps in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="4a737-104">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="4a737-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="4a737-105">В этой статье объясняется, как для отображения интерактивных maps на страницах в зависимости от сопоставления служб, предоставляемых Bing, Google, Yahoo и MapQuest веб-сайта ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="4a737-105">This article explains how to display interactive maps on pages in an ASP.NET Web Pages (Razor) website based on mapping services provided by Bing, Google, MapQuest, and Yahoo.</span></span>
> 
> <span data-ttu-id="4a737-106">Что вы узнаете следующее.</span><span class="sxs-lookup"><span data-stu-id="4a737-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="4a737-107">Как создать схему на основе адреса.</span><span class="sxs-lookup"><span data-stu-id="4a737-107">How to generate a map based on an address.</span></span>
> - <span data-ttu-id="4a737-108">Как создать карту, в зависимости от координаты широты и долготы.</span><span class="sxs-lookup"><span data-stu-id="4a737-108">How to generate a map based on latitude and longitude coordinates.</span></span>
> - <span data-ttu-id="4a737-109">Как зарегистрировать учетную запись разработчика Bing Maps и получить ключ, используемый с Bing Maps.</span><span class="sxs-lookup"><span data-stu-id="4a737-109">How to register a Bing Maps Developer Account and get a key to use with Bing Maps.</span></span>
> 
> <span data-ttu-id="4a737-110">Это функция ASP.NET, представленные в статье:</span><span class="sxs-lookup"><span data-stu-id="4a737-110">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="4a737-111">`Maps` Вспомогательные.</span><span class="sxs-lookup"><span data-stu-id="4a737-111">The `Maps` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="4a737-112">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="4a737-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="4a737-113">Веб-страниц ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="4a737-113">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="4a737-114">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="4a737-114">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="4a737-115">Этот учебник также работает с WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="4a737-115">This tutorial also works with WebMatrix 3.</span></span>


<span data-ttu-id="4a737-116">В веб-страницы, можно отобразить сопоставления на странице с помощью `Maps` поддержки.</span><span class="sxs-lookup"><span data-stu-id="4a737-116">In Web Pages, you can display maps on a page by using `Maps` helper.</span></span> <span data-ttu-id="4a737-117">Можно создавать схемы, либо на основе адреса или набор координат широты и долготы.</span><span class="sxs-lookup"><span data-stu-id="4a737-117">You can generate maps based either on an address or on a set of longitude and latitude coordinates.</span></span> <span data-ttu-id="4a737-118">`Maps` Класса позволяет вызывать обработчики популярных карты, включая Bing, Google, Yahoo и MapQuest.</span><span class="sxs-lookup"><span data-stu-id="4a737-118">The `Maps` class lets you call into popular map engines including Bing, Google, MapQuest, and Yahoo.</span></span>

<span data-ttu-id="4a737-119">Шаги для добавления сопоставления на странице одинаковы независимо от того, какое ядро карты вызова.</span><span class="sxs-lookup"><span data-stu-id="4a737-119">The steps for adding mapping to a page are the same regardless of which of the map engines you call.</span></span> <span data-ttu-id="4a737-120">Просто добавьте ссылку на файл JavaScript, который делает доступных методов для отображения карты, а затем вызвать методы `Maps` вспомогательный.</span><span class="sxs-lookup"><span data-stu-id="4a737-120">You just add a JavaScript file reference that makes available methods to display the map, and then you call methods of the `Maps` helper.</span></span>

<span data-ttu-id="4a737-121">Выберите службы карты, в зависимости от `Maps` используется вспомогательный метод.</span><span class="sxs-lookup"><span data-stu-id="4a737-121">You choose a map service based on which `Maps` helper method you use.</span></span> <span data-ttu-id="4a737-122">Можно использовать любой из них:</span><span class="sxs-lookup"><span data-stu-id="4a737-122">You can use any of these:</span></span>

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a><span data-ttu-id="4a737-123">Установка необходимых компонентов</span><span class="sxs-lookup"><span data-stu-id="4a737-123">Installing the Pieces You Need</span></span>

<span data-ttu-id="4a737-124">Для отображения карты, следует эти элементы:</span><span class="sxs-lookup"><span data-stu-id="4a737-124">To display maps, you need these pieces:</span></span>

- <span data-ttu-id="4a737-125">`Maps` Вспомогательные.</span><span class="sxs-lookup"><span data-stu-id="4a737-125">The `Maps` helper.</span></span> <span data-ttu-id="4a737-126">В версии 2 Библиотека вспомогательных методов ASP.NET Web — этого вспомогательного объекта.</span><span class="sxs-lookup"><span data-stu-id="4a737-126">This helper is in version 2 of the ASP.NET Web Helpers Library.</span></span> <span data-ttu-id="4a737-127">Если не было добавлено библиотеки, можно установить на сайте как пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="4a737-127">If you haven't already added the library, you can install it in your site as a NuGet package.</span></span> <span data-ttu-id="4a737-128">Дополнительные сведения см. в разделе [Установка помощников в узел веб-страниц ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372).</span><span class="sxs-lookup"><span data-stu-id="4a737-128">For details, see [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372).</span></span> <span data-ttu-id="4a737-129">(В галерее, поиск `microsoft-web-helpers` пакета.)</span><span class="sxs-lookup"><span data-stu-id="4a737-129">(In the Gallery, search for the `microsoft-web-helpers` package.)</span></span>
- <span data-ttu-id="4a737-130">Библиотеки jQuery.</span><span class="sxs-lookup"><span data-stu-id="4a737-130">The jQuery library.</span></span> <span data-ttu-id="4a737-131">Несколько шаблонов узла WebMatrix уже включены библиотеки jQuery в их *сценарий* папки.</span><span class="sxs-lookup"><span data-stu-id="4a737-131">Several of the WebMatrix site templates already include jQuery libraries in their *Script* folders.</span></span> <span data-ttu-id="4a737-132">Если у вас этих библиотек, можно загрузить последние библиотеки jQuery непосредственно из [jQuery.org](http://jQuery.org) сайта.</span><span class="sxs-lookup"><span data-stu-id="4a737-132">If you do not have these libraries, you can download the latest jQuery library directly from the [jQuery.org](http://jQuery.org) site.</span></span> <span data-ttu-id="4a737-133">Или можно создать новый сайт с помощью шаблона (например, **начального сайта** шаблон) и скопируйте файлы jQuery с этого сайта, для текущего веб-узла.</span><span class="sxs-lookup"><span data-stu-id="4a737-133">Or you can create a new site using a template (for example, the **Starter Site** template) and then copy the jQuery files from that site to your current site.</span></span>

<span data-ttu-id="4a737-134">Наконец Если вы хотите использовать службу Bing maps, необходимо создать учетную запись (бесплатно) и получить ключ.</span><span class="sxs-lookup"><span data-stu-id="4a737-134">Finally, if you want to use Bing maps, you must first create a (free) account and get a key.</span></span> <span data-ttu-id="4a737-135">Чтобы получить ключ, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="4a737-135">To get a key, follow these steps:</span></span>

1. <span data-ttu-id="4a737-136">Создать учетную запись на [учетную запись разработчика Bing Maps](https://www.microsoft.com/maps/developers/web.aspx).</span><span class="sxs-lookup"><span data-stu-id="4a737-136">Create an account on the [Bing Maps Developer Account](https://www.microsoft.com/maps/developers/web.aspx).</span></span> <span data-ttu-id="4a737-137">Необходимо иметь учетную запись Майкрософт (Windows Live ID) также.</span><span class="sxs-lookup"><span data-stu-id="4a737-137">You must have a Microsoft account (Windows Live ID) as well.</span></span>

    <span data-ttu-id="4a737-138">Можно указать, что вы хотите использовать ключ для **оценки и тестирования**.</span><span class="sxs-lookup"><span data-stu-id="4a737-138">You can specify that you want to use the key for **Evaluation/Test**.</span></span> <span data-ttu-id="4a737-139">Если вы тестируете функции сопоставления на компьютере с помощью IIS Express и WebMatrix, перейдите **сайта** рабочей области и Примечание URL-адрес веб-узла (например, `http://localhost:50408`, хотя этот номер порта, скорее всего, будут разными).</span><span class="sxs-lookup"><span data-stu-id="4a737-139">If you are testing the mapping function on your own computer using WebMatrix and IIS Express, go the **Site** workspace and note the URL of your site (for example, `http://localhost:50408`, although your port number will probably be different).</span></span> <span data-ttu-id="4a737-140">Эту функцию можно использовать *localhost* адрес в качестве узла при регистрации.</span><span class="sxs-lookup"><span data-stu-id="4a737-140">You can use this *localhost* address as the site when you register.</span></span>
2. <span data-ttu-id="4a737-141">После регистрации учетной записи, перейдите в центр учетных записей Bing Maps и щелкните **Создание или просмотр ключей**:</span><span class="sxs-lookup"><span data-stu-id="4a737-141">After you have registered for an account, go to the Bing Maps Account Center and click **Create or view keys**:</span></span>

    ![сопоставление 2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. <span data-ttu-id="4a737-143">Запишите ключ, который создает Bing.</span><span class="sxs-lookup"><span data-stu-id="4a737-143">Record the key that Bing creates.</span></span>

## <a name="creating-a-map-based-on-an-address-using-google"></a><span data-ttu-id="4a737-144">Создание карты по адресам (с помощью Google)</span><span class="sxs-lookup"><span data-stu-id="4a737-144">Creating a Map Based on an Address (Using Google)</span></span>

<span data-ttu-id="4a737-145">Приведенный ниже показано, как создать страницу, которая подготавливает к просмотру схему по адресам.</span><span class="sxs-lookup"><span data-stu-id="4a737-145">The following example shows how to create a page that renders a map based on an address.</span></span> <span data-ttu-id="4a737-146">В этом примере показано, как использовать карты Google.</span><span class="sxs-lookup"><span data-stu-id="4a737-146">This example shows how to use Google Maps.</span></span>

1. <span data-ttu-id="4a737-147">Создайте файл с именем *MapAddress.cshtml* в корневом узле.</span><span class="sxs-lookup"><span data-stu-id="4a737-147">Create a file named *MapAddress.cshtml* in the root of the site.</span></span> <span data-ttu-id="4a737-148">Эта страница будет создать карту, исходя из адреса, который можно передать.</span><span class="sxs-lookup"><span data-stu-id="4a737-148">This page will generate a map based on an address that you pass to it.</span></span>
2. <span data-ttu-id="4a737-149">Скопируйте следующий код в файл, перезапись существующего содержимого.</span><span class="sxs-lookup"><span data-stu-id="4a737-149">Copy the following code into the file, overwriting the existing content.</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="4a737-150">Обратите внимание, следующие функции страницы:</span><span class="sxs-lookup"><span data-stu-id="4a737-150">Notice the following features of the page:</span></span>

    - <span data-ttu-id="4a737-151">`<script>` Элемент в `<head>` элемент.</span><span class="sxs-lookup"><span data-stu-id="4a737-151">The `<script>` element in the `<head>` element.</span></span> <span data-ttu-id="4a737-152">В примере `<script>` ссылок на элементы *jquery 1.6.4.min.js* файла, являющегося уменьшенный (сжатие) версия библиотеки jQuery версия 1.6.4.</span><span class="sxs-lookup"><span data-stu-id="4a737-152">In the example, the `<script>` element references the *jquery-1.6.4.min.js* file, which is a minified (compressed) version of the jQuery library, version 1.6.4.</span></span> <span data-ttu-id="4a737-153">Обратите внимание, что ссылка предполагается, что *.js* файл находится в *сценариев* папки сайта.</span><span class="sxs-lookup"><span data-stu-id="4a737-153">Note that the reference assumes that the *.js* file is in the *Scripts* folder of your site.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="4a737-154">Если вы используете другой версии библиотеки jQuery, убедитесь, что вы неправильно указывают этой версии.</span><span class="sxs-lookup"><span data-stu-id="4a737-154">If you're using a different version of the jQuery library, just make sure that you're pointing to that version correctly.</span></span>
    - <span data-ttu-id="4a737-155">Вызов `@Maps.GetGoogleHtml` в основной области страницы.</span><span class="sxs-lookup"><span data-stu-id="4a737-155">The call to the `@Maps.GetGoogleHtml` in the body of the page.</span></span> <span data-ttu-id="4a737-156">Для сопоставления адреса, необходимо передать строка адреса.</span><span class="sxs-lookup"><span data-stu-id="4a737-156">To map an address, you must pass an address string.</span></span> <span data-ttu-id="4a737-157">Методы для других модулей, карты работают аналогичным образом (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span><span class="sxs-lookup"><span data-stu-id="4a737-157">The methods for the other map engines work in a similar way (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span></span>
3. <span data-ttu-id="4a737-158">Запустите страницу и введите адрес.</span><span class="sxs-lookup"><span data-stu-id="4a737-158">Run the page and enter an address.</span></span> <span data-ttu-id="4a737-159">На странице отображается карта, в зависимости от карты Google, который показывает расположение, указанное.</span><span class="sxs-lookup"><span data-stu-id="4a737-159">The page displays a map, based on Google Maps, that shows the location that you specified.</span></span>

     ![сопоставление 1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a><span data-ttu-id="4a737-161">Создание карты на основе широты и долготы координаты (с помощью Bing)</span><span class="sxs-lookup"><span data-stu-id="4a737-161">Creating a Map Based on Latitude and Longitude Coordinates (Using Bing)</span></span>

<span data-ttu-id="4a737-162">В этом примере показано, как создать карту, в соответствии с координатами.</span><span class="sxs-lookup"><span data-stu-id="4a737-162">This example shows how to create a map based on coordinates.</span></span> <span data-ttu-id="4a737-163">В этом примере показано, как для работы с картами Bing и включение ключа Bing.</span><span class="sxs-lookup"><span data-stu-id="4a737-163">This example shows how to use Bing maps and how to include your Bing key.</span></span> <span data-ttu-id="4a737-164">(Можно создать карты, в соответствии с координатами, кроме того, с помощью других модулей, карты без использования ключа Bing.)</span><span class="sxs-lookup"><span data-stu-id="4a737-164">(You can create a map based on coordinates using the other map engines also, without using a Bing key.)</span></span>

1. <span data-ttu-id="4a737-165">Создайте файл с именем *MapCoordinates.cshtml* в корневой каталог сайта и заменить существующее содержимое со следующими кода и разметки:</span><span class="sxs-lookup"><span data-stu-id="4a737-165">Create a file named *MapCoordinates.cshtml* in the root of the site and replace the existing content with the following code and markup:</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. <span data-ttu-id="4a737-166">Замените `your-key-here` значением ключа Bing Maps, созданные в более ранних версий.</span><span class="sxs-lookup"><span data-stu-id="4a737-166">Replace `your-key-here` with the Bing Maps key that you generated earlier.</span></span>
3. <span data-ttu-id="4a737-167">Запустите *MapCoordinates.cshtml* введите координаты широты и долготы и нажмите кнопку **Map It!**</span><span class="sxs-lookup"><span data-stu-id="4a737-167">Run the *MapCoordinates.cshtml* page, enter latitude and longitude coordinates, and then click the **Map It!**</span></span> <span data-ttu-id="4a737-168">.</span><span class="sxs-lookup"><span data-stu-id="4a737-168">button.</span></span> <span data-ttu-id="4a737-169">(Если вы не знаете все координаты, попробуйте следующее.</span><span class="sxs-lookup"><span data-stu-id="4a737-169">(If you don't know any coordinates, try the following.</span></span> <span data-ttu-id="4a737-170">Это расположение на Microsoft Редмонд).</span><span class="sxs-lookup"><span data-stu-id="4a737-170">This is a location on the Microsoft Redmond campus.)</span></span>

   - <span data-ttu-id="4a737-171">Широта: 47.6781005859375</span><span class="sxs-lookup"><span data-stu-id="4a737-171">Latitude: 47.6781005859375</span></span>
   - <span data-ttu-id="4a737-172">Долготы:-122.158317565918</span><span class="sxs-lookup"><span data-stu-id="4a737-172">Longitude: -122.158317565918</span></span>

     <span data-ttu-id="4a737-173">Эта страница отображается с помощью координат, заданные вами.</span><span class="sxs-lookup"><span data-stu-id="4a737-173">The page is displayed using the coordinates that you specified.</span></span>

     ![сопоставление 3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="4a737-175">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="4a737-175">Additional Resources</span></span>


[<span data-ttu-id="4a737-176">Справочник по API Microsoft.Maps</span><span class="sxs-lookup"><span data-stu-id="4a737-176">Microsoft.Maps API Reference</span></span>](https://msdn.microsoft.com/library/gg427611.aspx)
