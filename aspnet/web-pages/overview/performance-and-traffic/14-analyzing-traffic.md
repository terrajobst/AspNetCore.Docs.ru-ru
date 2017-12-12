---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: "Отслеживание сведений о посетителях (аналитика) для веб-страниц ASP.NET (Razor) сайта | Документы Microsoft"
author: tfitzmac
description: "После принял веб-сайт будет можно анализировать данный трафик веб-сайта."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 9a381ebaed30325fdfa5f0f558910d3002c61559
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="edac1-103">Отслеживание сведений о посетителях (аналитика) для веб-страниц (Razor) узла ASP.NET</span><span class="sxs-lookup"><span data-stu-id="edac1-103">Tracking Visitor Information (Analytics) for an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="edac1-104">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="edac1-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="edac1-105">В этой статье описывается использование вспомогательный объект для добавления веб-сайта аналитика на страницы на веб-сайт ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="edac1-105">This article describes how to use a helper to add website analytics to pages in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="edac1-106">Что вы узнаете следующее.</span><span class="sxs-lookup"><span data-stu-id="edac1-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="edac1-107">Как отправить сведения о веб-сайте трафика поставщик данных аналитики.</span><span class="sxs-lookup"><span data-stu-id="edac1-107">How to send information about your website traffic to an analytics provider.</span></span>
> 
> <span data-ttu-id="edac1-108">Ниже перечислены возможности, представленные в статье программирования ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="edac1-108">These are the ASP.NET programming features introduced in the article:</span></span>
> 
> - <span data-ttu-id="edac1-109">`Analytics` Вспомогательные.</span><span class="sxs-lookup"><span data-stu-id="edac1-109">The `Analytics` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="edac1-110">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="edac1-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="edac1-111">Веб-страниц ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="edac1-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="edac1-112">Библиотека вспомогательных методов ASP.NET Web (пакет NuGet)</span><span class="sxs-lookup"><span data-stu-id="edac1-112">ASP.NET Web Helpers Library (NuGet package)</span></span>


<span data-ttu-id="edac1-113">Аналитика — это общий термин для технологий, измеряет трафика на веб-сайте, чтобы понять, как люди используйте сайт.</span><span class="sxs-lookup"><span data-stu-id="edac1-113">Analytics is a general term for technology that measures traffic on your website so you can understand how people use the site.</span></span> <span data-ttu-id="edac1-114">Многие службы аналитики доступны, включая службы от Google, Yahoo, StatCounter и другим пользователям.</span><span class="sxs-lookup"><span data-stu-id="edac1-114">Many analytics services are available, including services from Google, Yahoo, StatCounter, and others.</span></span>

<span data-ttu-id="edac1-115">Способом анализа, работает, что вы регистрируетесь для учетной записи с поставщиком аналитику, когда зарегистрировать веб-узел, который необходимо отслеживать. Поставщик отправляет фрагмент кода JavaScript, включая идентификатор, или код для вашей учетной записи трассировки.</span><span class="sxs-lookup"><span data-stu-id="edac1-115">The way analytics works is that you sign up for an account with the analytics provider, where you register the site that you want to track. The provider sends you a snippet of JavaScript code that includes an ID or tracking code for your account.</span></span> <span data-ttu-id="edac1-116">Добавить фрагмент JavaScript на веб-страницы на сайте, который требуется отслеживать. (Обычно добавляется фрагмент аналитика в нижний колонтитул или макета страницы или других разметки HTML, который отображается на каждой странице веб-узла.) Когда пользователи запрашивают страницы, содержащей один из этих фрагментов кода JavaScript, фрагмент отправляет сведения о текущей страницы поставщику аналитики, который записывает различные сведения о странице.</span><span class="sxs-lookup"><span data-stu-id="edac1-116">You add the JavaScript snippet to the web pages on the site that you want to track. (You typically add the analytics snippet to a footer or layout page or other HTML markup that appears on every page in your site.) When users request a page that contains one of these JavaScript snippets, the snippet sends information about the current page to the analytics provider, who records various details about the page.</span></span>

<span data-ttu-id="edac1-117">Посмотрите на статистике сайта следует войти веб-сайта поставщика analytics.</span><span class="sxs-lookup"><span data-stu-id="edac1-117">When you want to have a look at your site statistics, you log into the analytics provider's website.</span></span> <span data-ttu-id="edac1-118">Как можно просмотреть все виды отчеты о веб-сайта:</span><span class="sxs-lookup"><span data-stu-id="edac1-118">You can then view all sorts of reports about your site, like:</span></span>

- <span data-ttu-id="edac1-119">Число просмотров страницы для отдельных страниц.</span><span class="sxs-lookup"><span data-stu-id="edac1-119">The number of page views for individual pages.</span></span> <span data-ttu-id="edac1-120">Это означает (приблизительно) сколько людей посетив этот сайт, а страниц веб-узла, которые наиболее популярных.</span><span class="sxs-lookup"><span data-stu-id="edac1-120">This tells you (roughly) how many people are visiting the site, and which pages on your site are the most popular.</span></span>
- <span data-ttu-id="edac1-121">Как долго люди тратят на выполнение отдельных страниц.</span><span class="sxs-lookup"><span data-stu-id="edac1-121">How long people spend on specific pages.</span></span> <span data-ttu-id="edac1-122">Можно определить таких вещей, как мешает ли домашнюю страницу процент пользователей.</span><span class="sxs-lookup"><span data-stu-id="edac1-122">This can tell you things like whether your home page is keeping people's interest.</span></span>
- <span data-ttu-id="edac1-123">Какие люди сайты были на Перед посещении веб-узла.</span><span class="sxs-lookup"><span data-stu-id="edac1-123">What sites people were on before they visited your site.</span></span> <span data-ttu-id="edac1-124">Это поможет вам понять, поступил ли трафика с помощью ссылок из поиска и т. д.</span><span class="sxs-lookup"><span data-stu-id="edac1-124">This helps you understand whether your traffic is coming from links, from searches, and so on.</span></span>
- <span data-ttu-id="edac1-125">При посещении веб-узла и как долго они остаются.</span><span class="sxs-lookup"><span data-stu-id="edac1-125">When people visit your site and how long they stay.</span></span>
- <span data-ttu-id="edac1-126">Какие странах, к которой принадлежат посетителей.</span><span class="sxs-lookup"><span data-stu-id="edac1-126">What countries your visitors are from.</span></span>
- <span data-ttu-id="edac1-127">Какие браузеры и операционных систем используется посетителей.</span><span class="sxs-lookup"><span data-stu-id="edac1-127">What browsers and operating systems your visitors are using.</span></span>

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a><span data-ttu-id="edac1-129">Добавление аналитики на страницу с помощью вспомогательного класса</span><span class="sxs-lookup"><span data-stu-id="edac1-129">Using a Helper to Add Analytics to a Page</span></span>

<span data-ttu-id="edac1-130">Веб-страниц ASP.NET включает несколько вспомогательных методов аналитика (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, и `Analytics.GetStatCounterHtml`), которые упрощают управление фрагменты кода JavaScript, используемый для анализа.</span><span class="sxs-lookup"><span data-stu-id="edac1-130">ASP.NET Web Pages includes several analytics helpers (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, and `Analytics.GetStatCounterHtml`) that make it easy to manage the JavaScript snippets used for analytics.</span></span> <span data-ttu-id="edac1-131">Вместо помог понять, как и где следует поместить код JavaScript, необходимо сделать достаточно добавить вспомогательный объект на страницу.</span><span class="sxs-lookup"><span data-stu-id="edac1-131">Instead of figuring out how and where to put the JavaScript code, all you have to do is add the helper to a page.</span></span> <span data-ttu-id="edac1-132">Единственные сведения, которые требуются для предоставления является вашей имя учетной записи, идентификатор или код трассировки.</span><span class="sxs-lookup"><span data-stu-id="edac1-132">The only information you need to provide is your account name, ID, or tracking code.</span></span> <span data-ttu-id="edac1-133">(Для StatCounter, вы также должны предоставить несколько дополнительных значений.)</span><span class="sxs-lookup"><span data-stu-id="edac1-133">(For StatCounter, you also have to provide a few additional values.)</span></span>

<span data-ttu-id="edac1-134">В этой процедуре вы создадите макет страницы, использующей `GetGoogleHtml` вспомогательный.</span><span class="sxs-lookup"><span data-stu-id="edac1-134">In this procedure, you'll create a layout page that uses the `GetGoogleHtml` helper.</span></span> <span data-ttu-id="edac1-135">Если уже имеется учетная запись с одним из других поставщиков аналитика, можно вместо этого использовать эту учетную запись и при необходимости, вносить небольшие изменения.</span><span class="sxs-lookup"><span data-stu-id="edac1-135">If you already have an account with one of the other analytics providers, you can use that account instead and make slight adjustments as needed.</span></span>

> [!NOTE]
> <span data-ttu-id="edac1-136">При создании учетной записи аналитики регистрации URL-адрес сайта, который требуется отслеживать.</span><span class="sxs-lookup"><span data-stu-id="edac1-136">When you create an analytics account, you register the URL of the site that you want to be tracking.</span></span> <span data-ttu-id="edac1-137">Если вы тестируете все данные на локальном компьютере, не отслеживания фактического трафика (трафик только это вы), поэтому нельзя будет запись и Просмотр сайта статистику.</span><span class="sxs-lookup"><span data-stu-id="edac1-137">If you're testing everything on your local computer, you won't be tracking actual traffic (the only traffic is you), so you won't be able to record and view site statistics.</span></span> <span data-ttu-id="edac1-138">Однако эта процедура показывает, как вспомогательный analytics добавить на страницу.</span><span class="sxs-lookup"><span data-stu-id="edac1-138">But this procedure shows how you add an analytics helper to a page.</span></span> <span data-ttu-id="edac1-139">При публикации веб-узла, действующем сайте будет отправлять сведения с поставщиком analytics.</span><span class="sxs-lookup"><span data-stu-id="edac1-139">When you publish your site, the live site will send information to your analytics provider.</span></span>


1. <span data-ttu-id="edac1-140">Добавить библиотека вспомогательных методов ASP.NET Web веб-сайта, как описано в [Установка помощников в узел веб-страниц ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), если не было добавлено.</span><span class="sxs-lookup"><span data-stu-id="edac1-140">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="edac1-141">Создать учетную запись с Google Analytics и запишите имя учетной записи.</span><span class="sxs-lookup"><span data-stu-id="edac1-141">Create an account with Google Analytics and record the account name.</span></span>
3. <span data-ttu-id="edac1-142">Создание макета страницы с именем *Analytics.cshtml* и добавьте следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="edac1-142">Create a layout page named *Analytics.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="edac1-143">Необходимо разместить вызова `Analytics` вспомогательный в тексте веб-страницы (перед `</body>` тега).</span><span class="sxs-lookup"><span data-stu-id="edac1-143">You must place the call to the `Analytics` helper in the body of your web page (before the `</body>` tag).</span></span> <span data-ttu-id="edac1-144">В противном случае браузер не будет выполняться сценарий.</span><span class="sxs-lookup"><span data-stu-id="edac1-144">Otherwise, the browser will not run the script.</span></span>

    <span data-ttu-id="edac1-145">При использовании поставщика различных analytics вместо этого используйте один из следующих вспомогательных методов:</span><span class="sxs-lookup"><span data-stu-id="edac1-145">If you're using a different analytics provider, use one of the following helpers instead:</span></span>

    - <span data-ttu-id="edac1-146">(Yahoo)`@Analytics.GetYahooHtml("myaccount")`</span><span class="sxs-lookup"><span data-stu-id="edac1-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span></span>
    - <span data-ttu-id="edac1-147">(StatCounter)`@Analytics.GetStatCounterHtml("project", "security")`</span><span class="sxs-lookup"><span data-stu-id="edac1-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span></span>
4. <span data-ttu-id="edac1-148">Замените `myaccount` с именем учетной записи, идентификатор или код трассировки, созданный на шаге 1.</span><span class="sxs-lookup"><span data-stu-id="edac1-148">Replace `myaccount` with the name of the account, ID, or tracking code that you created in step 1.</span></span>
5. <span data-ttu-id="edac1-149">Запустите страницу в браузере.</span><span class="sxs-lookup"><span data-stu-id="edac1-149">Run the page in the browser.</span></span> <span data-ttu-id="edac1-150">(Убедитесь, что выбран страницы **файлы** рабочей области, прежде чем запускать его.)</span><span class="sxs-lookup"><span data-stu-id="edac1-150">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
6. <span data-ttu-id="edac1-151">В браузере просмотрите исходный код страницы.</span><span class="sxs-lookup"><span data-stu-id="edac1-151">In the browser, view the page source.</span></span> <span data-ttu-id="edac1-152">Можно будет просматривать код отображаемой аналитика:</span><span class="sxs-lookup"><span data-stu-id="edac1-152">You'll be able to see the rendered analytics code:</span></span>

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. <span data-ttu-id="edac1-153">Войдите на сайт Google Analytics и изучите статистику для веб-узла.</span><span class="sxs-lookup"><span data-stu-id="edac1-153">Log onto the Google Analytics site and examine the statistics for your site.</span></span> <span data-ttu-id="edac1-154">При запуске страницы на действующем сайте появляется запись, которая регистрирует перейдите на страницу.</span><span class="sxs-lookup"><span data-stu-id="edac1-154">If you're running the page on a live site, you see an entry that logs the visit to your page.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="edac1-155">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="edac1-155">Additional Resources</span></span>

- [<span data-ttu-id="edac1-156">Узел Google Analytics</span><span class="sxs-lookup"><span data-stu-id="edac1-156">Google Analytics site</span></span>](https://www.google.com/analytics/)
- [<span data-ttu-id="edac1-157">Yahoo! Веб-сайт аналитика</span><span class="sxs-lookup"><span data-stu-id="edac1-157">Yahoo! Web Analytics site</span></span>](http://help.yahoo.com/l/us/yahoo/ywa/)
- [<span data-ttu-id="edac1-158">StatCounter сайта</span><span class="sxs-lookup"><span data-stu-id="edac1-158">StatCounter site</span></span>](http://statcounter.com/)
