---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Создание доступных для чтения URL-адресов в ASP.NET Web Pages сайтов (Razor) | Документация Майкрософт
author: tfitzmac
description: В этой статье описывается маршрутизации в на веб-сайте ASP.NET Web Pages (Razor), и как это позволяет использовать URL-адреса, которые являются более понятным и удобным для оптимизации поисковой системы. Вы будете...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: d027be2924f24c1080deb4ee2deacc235bbd1001
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385752"
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="2c737-104">Создание доступных для чтения URL-адресов на сайтах ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="2c737-104">Creating Readable URLs in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="2c737-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="2c737-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="2c737-106">В этой статье описывается маршрутизации в на веб-сайте ASP.NET Web Pages (Razor), и как это позволяет использовать URL-адреса, которые являются более понятным и удобным для оптимизации поисковой системы.</span><span class="sxs-lookup"><span data-stu-id="2c737-106">This article describes routing in an ASP.NET Web Pages (Razor) website, and how this lets you use URLs that are more readable and better for SEO.</span></span>
> 
> <span data-ttu-id="2c737-107">Вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="2c737-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="2c737-108">Как ASP.NET использует маршрутизацию для позволяют использовать более доступной для чтения и для поиска URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="2c737-108">How ASP.NET uses routing to let you use more readable and searchable URLs.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2c737-109">Версии программного обеспечения, используемые в этом руководстве</span><span class="sxs-lookup"><span data-stu-id="2c737-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="2c737-110">Веб-страниц ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="2c737-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="2c737-111">Этот учебник также работает с ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="2c737-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="about-routing"></a><span data-ttu-id="2c737-112">Сведения о маршрутизации</span><span class="sxs-lookup"><span data-stu-id="2c737-112">About Routing</span></span>

<span data-ttu-id="2c737-113">URL-адресов для страниц в веб-узла может негативно сказаться на насколько хорошо он работает.</span><span class="sxs-lookup"><span data-stu-id="2c737-113">The URLs for the pages in your site can have an impact on how well the site works.</span></span> <span data-ttu-id="2c737-114">URL-адрес, что все &quot;понятное&quot; может упростить для пользователей сайта.</span><span class="sxs-lookup"><span data-stu-id="2c737-114">A URL that's &quot;friendly&quot; can make it easier for people to use the site.</span></span> <span data-ttu-id="2c737-115">Он также может помочь в оптимизации поисковой системы (SEO) для сайта.</span><span class="sxs-lookup"><span data-stu-id="2c737-115">It can also help with search-engine optimization (SEO) for the site.</span></span> <span data-ttu-id="2c737-116">Веб-сайтов ASP.NET включают возможность использования удобных URL-адресов автоматически.</span><span class="sxs-lookup"><span data-stu-id="2c737-116">ASP.NET websites include the ability to use friendly URLs automatically.</span></span>

<span data-ttu-id="2c737-117">ASP.NET позволяет создавать значимые URL-адреса, которые описывают действия пользователя, а не просто указывает на файл на сервере.</span><span class="sxs-lookup"><span data-stu-id="2c737-117">ASP.NET lets you create meaningful URLs that describe user actions instead of just pointing to a file on the server.</span></span> <span data-ttu-id="2c737-118">Проверьте следующие URL-адреса для вымышленной блога:</span><span class="sxs-lookup"><span data-stu-id="2c737-118">Consider these URLs for a fictional blog:</span></span>

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

<span data-ttu-id="2c737-119">Сравните эти URL-адреса на приведенные ниже строки:</span><span class="sxs-lookup"><span data-stu-id="2c737-119">Compare those URLs to the following ones:</span></span>

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

<span data-ttu-id="2c737-120">В первой паре, пользователю нужно знать, что блога отображается с использованием *blog.cshtml* странице и после этого необходимо построить строку запроса, которое получает правый диапазон категории или даты.</span><span class="sxs-lookup"><span data-stu-id="2c737-120">In the first pair, a user would have to know that the blog is displayed using the *blog.cshtml* page, and would then have to construct a query string that gets the right category or date range.</span></span> <span data-ttu-id="2c737-121">Второй набор примеров гораздо проще для понимания и создания.</span><span class="sxs-lookup"><span data-stu-id="2c737-121">The second set of examples is much easier to comprehend and create.</span></span>

<span data-ttu-id="2c737-122">URL-адреса для первого примера также непосредственно указать на конкретный файл (*blog.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="2c737-122">The URLs for the first example also point directly to a specific file (*blog.cshtml*).</span></span> <span data-ttu-id="2c737-123">Если для какой-либо причине блога были перемещены в другую папку на сервере, или блог, были изменены для использования на другую страницу, ссылки были бы не так.</span><span class="sxs-lookup"><span data-stu-id="2c737-123">If for some reason the blog were moved to another folder on the server, or if the blog were rewritten to use a different page, the links would be wrong.</span></span> <span data-ttu-id="2c737-124">Второй набор URL-адреса не указывает на определенную страницу, поэтому, даже если реализация блога или расположение, URL-адреса, по-прежнему будут допустимы.</span><span class="sxs-lookup"><span data-stu-id="2c737-124">The second set of URLs doesn't point to a specific page, so even if the blog implementation or location changes, the URLs would still be valid.</span></span>

<span data-ttu-id="2c737-125">В ASP.NET Web Pages дружественный URL-адреса, как показано в приведенных выше примерах можно создать, так как ASP.NET использует *маршрутизации*.</span><span class="sxs-lookup"><span data-stu-id="2c737-125">In ASP.NET Web Pages, you can create friendlier URLs like those in the above examples because ASP.NET uses *routing*.</span></span> <span data-ttu-id="2c737-126">Маршрутизация создает логический сопоставления из URL-адрес страницы (или страниц), можно выполнить запрос.</span><span class="sxs-lookup"><span data-stu-id="2c737-126">Routing creates logical mapping from a URL to a page (or pages) that can fulfill the request.</span></span> <span data-ttu-id="2c737-127">Поскольку сопоставление является логическим (не физических к определенному файлу), маршрутизации обеспечивает большую гибкость в определение URL-адреса для узла.</span><span class="sxs-lookup"><span data-stu-id="2c737-127">Because the mapping is logical (not physical, to a specific file), routing provides great flexibility in how you define the URLs for your site.</span></span>

## <a name="how-routing-works"></a><span data-ttu-id="2c737-128">Как работает маршрутизация</span><span class="sxs-lookup"><span data-stu-id="2c737-128">How Routing Works</span></span>

<span data-ttu-id="2c737-129">Когда ASP.NET обрабатывает запрос, он считывает URL-адрес, чтобы определить, как его маршрутизировать.</span><span class="sxs-lookup"><span data-stu-id="2c737-129">When ASP.NET processes a request, it reads the URL to determine how to route it.</span></span> <span data-ttu-id="2c737-130">ASP.NET предпринимается попытка отдельные сегменты URL-адреса для файлов на диске, движения слева направо.</span><span class="sxs-lookup"><span data-stu-id="2c737-130">ASP.NET tries to match individual segments of the URL to files on disk, going from left to right.</span></span> <span data-ttu-id="2c737-131">Если есть совпадение, все оставшиеся в URL-адрес передается на ту страницу *сведения о пути*.</span><span class="sxs-lookup"><span data-stu-id="2c737-131">If there's a match, anything remaining in the URL is passed to the page as *path information*.</span></span>

<span data-ttu-id="2c737-132">Представьте, что кто-то делает запрос через этот URL-адрес:</span><span class="sxs-lookup"><span data-stu-id="2c737-132">Imagine that someone makes a request using this URL:</span></span>

`http://www.contoso.com/a/b/c`

<span data-ttu-id="2c737-133">Поиск выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="2c737-133">The search goes like this:</span></span>

1. <span data-ttu-id="2c737-134">Есть ли файл, путь и имя */a/b/c.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="2c737-134">Is there a file with the path and name of */a/b/c.cshtml*?</span></span> <span data-ttu-id="2c737-135">Если это так, запустите эту страницу и передать сведения отсутствуют.</span><span class="sxs-lookup"><span data-stu-id="2c737-135">If so, run that page and pass no information to it.</span></span> <span data-ttu-id="2c737-136">В противном случае...</span><span class="sxs-lookup"><span data-stu-id="2c737-136">Otherwise ...</span></span>
2. <span data-ttu-id="2c737-137">Есть ли файл, путь и имя */a/b.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="2c737-137">Is there a file with the path and name of */a/b.cshtml*?</span></span> <span data-ttu-id="2c737-138">Если так, запустите эту страницу и передать значение `c` к нему.</span><span class="sxs-lookup"><span data-stu-id="2c737-138">If so, run that page and pass the value `c` to it.</span></span> <span data-ttu-id="2c737-139">В противном случае...</span><span class="sxs-lookup"><span data-stu-id="2c737-139">Otherwise …</span></span>
3. <span data-ttu-id="2c737-140">Есть ли файл, путь и имя */a.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="2c737-140">Is there a file with the path and name of */a.cshtml*?</span></span> <span data-ttu-id="2c737-141">Если так, запустите эту страницу и передать значение `b/c` к нему.</span><span class="sxs-lookup"><span data-stu-id="2c737-141">If so, run that page and pass the value `b/c` to it.</span></span>

<span data-ttu-id="2c737-142">Если поиска был найден не точно соответствует *.cshtml* файлы в их указанные папки ASP.NET по-прежнему найти эти файлы в свою очередь:</span><span class="sxs-lookup"><span data-stu-id="2c737-142">If the search found no exact matches for *.cshtml* files in their specified folders, ASP.NET continues looking for these files in turn:</span></span>

1. <span data-ttu-id="2c737-143">*/a/b/c/Default.cshtml* (без сведений о пути).</span><span class="sxs-lookup"><span data-stu-id="2c737-143">*/a/b/c/default.cshtml* (no path information).</span></span>
2. <span data-ttu-id="2c737-144">*/a/b/c/index.cshtml* (без сведений о пути).</span><span class="sxs-lookup"><span data-stu-id="2c737-144">*/a/b/c/index.cshtml* (no path information).</span></span>

> [!NOTE]
> <span data-ttu-id="2c737-145">Следует уточнить, запросы для определенных страниц (то есть запросы, включающие *.cshtml* расширение имени файла) работать так же, как можно ожидать.</span><span class="sxs-lookup"><span data-stu-id="2c737-145">To be clear, requests for specific pages (that is, requests that include the *.cshtml* filename extension) work just like you'd expect.</span></span> <span data-ttu-id="2c737-146">Запрос, например `http://www.contoso.com/a/b.cshtml` необходимо открывать страницу *b.cshtml* просто прекрасно.</span><span class="sxs-lookup"><span data-stu-id="2c737-146">A request like `http://www.contoso.com/a/b.cshtml` will run the page *b.cshtml* just fine.</span></span>


<span data-ttu-id="2c737-147">Внутри страницы, можно получить сведения о пути на странице `UrlData` свойство, которое представляет собой словарь.</span><span class="sxs-lookup"><span data-stu-id="2c737-147">Inside a page, you can get the path information via the page's `UrlData` property, which is a dictionary.</span></span> <span data-ttu-id="2c737-148">Представьте, что у вас есть файл с именем *ViewCustomers.cshtml* и сайт получает этот запрос:</span><span class="sxs-lookup"><span data-stu-id="2c737-148">Imagine that you have a file named *ViewCustomers.cshtml* and your site gets this request:</span></span>

`http://mysite.com/myWebSite/ViewCustomers/1000`

<span data-ttu-id="2c737-149">Как описывалось выше правилам, запрос будет передан на страницу.</span><span class="sxs-lookup"><span data-stu-id="2c737-149">As described in the rules above, the request will go to your page.</span></span> <span data-ttu-id="2c737-150">На странице следующего кода можно использовать для получения и отображения сведений о пути (в данном случае, значение &quot;1000&quot;):</span><span class="sxs-lookup"><span data-stu-id="2c737-150">Inside the page, you can use code like the following to get and display the path information (in this case, the value &quot;1000&quot;):</span></span>

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> <span data-ttu-id="2c737-151">Так как маршрутизации не подразумевает полное имя файла, то возможна неоднозначности при наличии страниц с таким же расширений имен файлов, но с другим именем (например, *MyPage.cshtml* и *MyPage.html*) .</span><span class="sxs-lookup"><span data-stu-id="2c737-151">Because routing doesn't involve complete file names, there can be ambiguity if you have pages that have the same name but different file-name extensions (for example, *MyPage.cshtml* and *MyPage.html*).</span></span> <span data-ttu-id="2c737-152">Чтобы избежать проблем с маршрутизацией, лучше, чтобы убедиться в том, что нет страниц на узле, имена которых отличаются только их расширения.</span><span class="sxs-lookup"><span data-stu-id="2c737-152">In order to avoid problems with routing, it's best to make sure that you don't have pages in your site whose names differ only in their extension.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="2c737-153">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="2c737-153">Additional Resources</span></span>

<span data-ttu-id="2c737-154">[WebMatrix — URL-адреса, UrlData и маршрутизацию для SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span><span class="sxs-lookup"><span data-stu-id="2c737-154">[WebMatrix - URLs, UrlData and Routing for SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span></span> <span data-ttu-id="2c737-155">Этой записи блога, Майк Бринд содержит некоторые дополнительные сведения о как работает маршрутизация ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="2c737-155">This blog entry by Mike Brind provides some additional details on how routing works in ASP.NET Web Pages.</span></span>
