---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: Отслеживание сведений о посетителях (Analytics) для веб-страниц ASP.NET (Razor) сайта | Документация Майкрософт
author: Rick-Anderson
description: После вы азы освоите веб-сайт будет, можно анализировать трафик веб-сайта.
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 57e6a0d4681f147faa5e9ca3b6ed0ef287d6a381
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020888"
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="6f78d-103">Отслеживание сведений о посетителях (Analytics) для узла ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="6f78d-103">Tracking Visitor Information (Analytics) for an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="6f78d-104">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="6f78d-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="6f78d-105">В этой статье описывается, как использовать вспомогательный объект для добавления аналитики веб-сайтов на страницы на веб-сайте ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="6f78d-105">This article describes how to use a helper to add website analytics to pages in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="6f78d-106">Вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="6f78d-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="6f78d-107">Как отправлять сведения о трафике вашего веб-сайта в поставщик данных аналитики.</span><span class="sxs-lookup"><span data-stu-id="6f78d-107">How to send information about your website traffic to an analytics provider.</span></span>
> 
> <span data-ttu-id="6f78d-108">Ниже перечислены возможности, представленные в этой статье программирования ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="6f78d-108">These are the ASP.NET programming features introduced in the article:</span></span>
> 
> - <span data-ttu-id="6f78d-109">`Analytics` Вспомогательный.</span><span class="sxs-lookup"><span data-stu-id="6f78d-109">The `Analytics` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6f78d-110">Версии программного обеспечения, используемые в этом руководстве</span><span class="sxs-lookup"><span data-stu-id="6f78d-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="6f78d-111">Веб-страниц ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="6f78d-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="6f78d-112">Библиотеку ASP.NET (пакет NuGet)</span><span class="sxs-lookup"><span data-stu-id="6f78d-112">ASP.NET Web Helpers Library (NuGet package)</span></span>


<span data-ttu-id="6f78d-113">Аналитика — это общий термин для технология, которая измеряет трафик на веб-сайте, чтобы вы могли оценить, как пользователи работают с сайта.</span><span class="sxs-lookup"><span data-stu-id="6f78d-113">Analytics is a general term for technology that measures traffic on your website so you can understand how people use the site.</span></span> <span data-ttu-id="6f78d-114">Доступны многие службы аналитики, включая службы Google, Yahoo, StatCounter и другие.</span><span class="sxs-lookup"><span data-stu-id="6f78d-114">Many analytics services are available, including services from Google, Yahoo, StatCounter, and others.</span></span>

<span data-ttu-id="6f78d-115">Аналитики так работает, что вам зарегистрироваться для учетной записи с помощью поставщика аналитики, где выполняется регистрация узла, которые требуется отслеживать. Поставщик отправляет фрагмент кода JavaScript, который включает в себя идентификатор или код для вашей учетной записи трассировки.</span><span class="sxs-lookup"><span data-stu-id="6f78d-115">The way analytics works is that you sign up for an account with the analytics provider, where you register the site that you want to track. The provider sends you a snippet of JavaScript code that includes an ID or tracking code for your account.</span></span> <span data-ttu-id="6f78d-116">Добавьте фрагмент JavaScript на веб-страницы на сайте, который вы хотите отслеживать. (Обычно добавляется фрагмент аналитики на страницу макета или верхний колонтитул или другие HTML-разметка, отображался на каждой странице на сайте.) При запросе страницы, которая содержит один из этих фрагментов кода JavaScript, фрагмент кода отправляет сведения о текущей странице поставщика аналитики, который записывает различные сведения о странице.</span><span class="sxs-lookup"><span data-stu-id="6f78d-116">You add the JavaScript snippet to the web pages on the site that you want to track. (You typically add the analytics snippet to a footer or layout page or other HTML markup that appears on every page in your site.) When users request a page that contains one of these JavaScript snippets, the snippet sends information about the current page to the analytics provider, who records various details about the page.</span></span>

<span data-ttu-id="6f78d-117">Если вы хотите взглянуть на статистики сайта, вы Войдите на веб-сайт поставщика аналитики.</span><span class="sxs-lookup"><span data-stu-id="6f78d-117">When you want to have a look at your site statistics, you log into the analytics provider's website.</span></span> <span data-ttu-id="6f78d-118">Затем можно просмотреть все виды отчетов о веб-узле, например:</span><span class="sxs-lookup"><span data-stu-id="6f78d-118">You can then view all sorts of reports about your site, like:</span></span>

- <span data-ttu-id="6f78d-119">Число просмотров страниц для отдельных страниц.</span><span class="sxs-lookup"><span data-stu-id="6f78d-119">The number of page views for individual pages.</span></span> <span data-ttu-id="6f78d-120">Это означает, (приблизительно) Узнайте, сколько пользователей посещают сайт, и страницы на сайте используются наиболее часто.</span><span class="sxs-lookup"><span data-stu-id="6f78d-120">This tells you (roughly) how many people are visiting the site, and which pages on your site are the most popular.</span></span>
- <span data-ttu-id="6f78d-121">Сколько пользователей можно потратить на определенные страницы.</span><span class="sxs-lookup"><span data-stu-id="6f78d-121">How long people spend on specific pages.</span></span> <span data-ttu-id="6f78d-122">Это может сообщить, например мешает ли домашнюю страницу процент пользователей.</span><span class="sxs-lookup"><span data-stu-id="6f78d-122">This can tell you things like whether your home page is keeping people's interest.</span></span>
- <span data-ttu-id="6f78d-123">Тем, кто сайтов находились, прежде чем они посетит веб-узла.</span><span class="sxs-lookup"><span data-stu-id="6f78d-123">What sites people were on before they visited your site.</span></span> <span data-ttu-id="6f78d-124">Это поможет вам понять, поступил ли ваш трафик по ссылкам из поисковые запросы и т. д.</span><span class="sxs-lookup"><span data-stu-id="6f78d-124">This helps you understand whether your traffic is coming from links, from searches, and so on.</span></span>
- <span data-ttu-id="6f78d-125">При посещении веб-узла, и как долго они остаются.</span><span class="sxs-lookup"><span data-stu-id="6f78d-125">When people visit your site and how long they stay.</span></span>
- <span data-ttu-id="6f78d-126">Каких стран, к которой принадлежат посетителей.</span><span class="sxs-lookup"><span data-stu-id="6f78d-126">What countries your visitors are from.</span></span>
- <span data-ttu-id="6f78d-127">Какие браузеры и операционные системы при использовании посетителей.</span><span class="sxs-lookup"><span data-stu-id="6f78d-127">What browsers and operating systems your visitors are using.</span></span>

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a><span data-ttu-id="6f78d-129">Использование вспомогательного метода для добавления аналитики на страницу</span><span class="sxs-lookup"><span data-stu-id="6f78d-129">Using a Helper to Add Analytics to a Page</span></span>

<span data-ttu-id="6f78d-130">Веб-страниц ASP.NET включает в себя несколько вспомогательных функций аналитики (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, и `Analytics.GetStatCounterHtml`), облегчающие управление фрагменты кода JavaScript, используемых для аналитики.</span><span class="sxs-lookup"><span data-stu-id="6f78d-130">ASP.NET Web Pages includes several analytics helpers (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, and `Analytics.GetStatCounterHtml`) that make it easy to manage the JavaScript snippets used for analytics.</span></span> <span data-ttu-id="6f78d-131">Вместо придумать, как и где помещу код JavaScript, что необходимо сделать всего лишь добавить вспомогательную функцию для страницы.</span><span class="sxs-lookup"><span data-stu-id="6f78d-131">Instead of figuring out how and where to put the JavaScript code, all you have to do is add the helper to a page.</span></span> <span data-ttu-id="6f78d-132">Единственные сведения, необходимые для предоставления является ваши имя учетной записи, идентификатор или код трассировки.</span><span class="sxs-lookup"><span data-stu-id="6f78d-132">The only information you need to provide is your account name, ID, or tracking code.</span></span> <span data-ttu-id="6f78d-133">(Для StatCounter, также необходимо предоставить несколько дополнительных значений.)</span><span class="sxs-lookup"><span data-stu-id="6f78d-133">(For StatCounter, you also have to provide a few additional values.)</span></span>

<span data-ttu-id="6f78d-134">В этой процедуре вы создадите макет страницы, использующей `GetGoogleHtml` вспомогательный.</span><span class="sxs-lookup"><span data-stu-id="6f78d-134">In this procedure, you'll create a layout page that uses the `GetGoogleHtml` helper.</span></span> <span data-ttu-id="6f78d-135">Если уже есть учетная запись с одним из других поставщиков аналитики, можно вместо этого использовать эту учетную запись и необходимости вносить небольшие изменения.</span><span class="sxs-lookup"><span data-stu-id="6f78d-135">If you already have an account with one of the other analytics providers, you can use that account instead and make slight adjustments as needed.</span></span>

> [!NOTE]
> <span data-ttu-id="6f78d-136">При создании учетной записи аналитики, вы регистрируете URL-адрес сайта, который требуется отслеживать.</span><span class="sxs-lookup"><span data-stu-id="6f78d-136">When you create an analytics account, you register the URL of the site that you want to be tracking.</span></span> <span data-ttu-id="6f78d-137">Если вы тестируете все, что на локальном компьютере, не отслеживания фактического трафика (трафик только тех, которые), поэтому вы не сможете регистрировать и просматривать статистику сайта.</span><span class="sxs-lookup"><span data-stu-id="6f78d-137">If you're testing everything on your local computer, you won't be tracking actual traffic (the only traffic is you), so you won't be able to record and view site statistics.</span></span> <span data-ttu-id="6f78d-138">Но эта процедура показывает, как добавить вспомогательный аналитики на страницу.</span><span class="sxs-lookup"><span data-stu-id="6f78d-138">But this procedure shows how you add an analytics helper to a page.</span></span> <span data-ttu-id="6f78d-139">При публикации узла активного сайта будет отправлять сведения поставщику аналитики.</span><span class="sxs-lookup"><span data-stu-id="6f78d-139">When you publish your site, the live site will send information to your analytics provider.</span></span>


1. <span data-ttu-id="6f78d-140">Добавьте библиотеку ASP.NET на веб-сайт, как описано в [Установка вспомогательных функций на сайте веб-страниц ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), если не было добавлено.</span><span class="sxs-lookup"><span data-stu-id="6f78d-140">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="6f78d-141">Создание учетной записи с помощью Google Analytics и запишите имя учетной записи.</span><span class="sxs-lookup"><span data-stu-id="6f78d-141">Create an account with Google Analytics and record the account name.</span></span>
3. <span data-ttu-id="6f78d-142">Создание макета страницы с именем *Analytics.cshtml* и добавьте следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="6f78d-142">Create a layout page named *Analytics.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="6f78d-143">Необходимо поместить вызов `Analytics` вспомогательная функция в теле веб-страницы (до `</body>` тега).</span><span class="sxs-lookup"><span data-stu-id="6f78d-143">You must place the call to the `Analytics` helper in the body of your web page (before the `</body>` tag).</span></span> <span data-ttu-id="6f78d-144">В противном случае — обозреватель не запускает сценарий.</span><span class="sxs-lookup"><span data-stu-id="6f78d-144">Otherwise, the browser will not run the script.</span></span>

    <span data-ttu-id="6f78d-145">Если вы используете поставщик разных analytics, используйте один из следующих вспомогательных методов:</span><span class="sxs-lookup"><span data-stu-id="6f78d-145">If you're using a different analytics provider, use one of the following helpers instead:</span></span>

    - <span data-ttu-id="6f78d-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span><span class="sxs-lookup"><span data-stu-id="6f78d-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span></span>
    - <span data-ttu-id="6f78d-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span><span class="sxs-lookup"><span data-stu-id="6f78d-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span></span>
4. <span data-ttu-id="6f78d-148">Замените `myaccount` с именем учетной записи, идентификатор или код трассировки, созданный на шаге 1.</span><span class="sxs-lookup"><span data-stu-id="6f78d-148">Replace `myaccount` with the name of the account, ID, or tracking code that you created in step 1.</span></span>
5. <span data-ttu-id="6f78d-149">Откройте страницу в браузере.</span><span class="sxs-lookup"><span data-stu-id="6f78d-149">Run the page in the browser.</span></span> <span data-ttu-id="6f78d-150">(Убедитесь, что выбран страницы **файлы** рабочей области, прежде чем запускать его.)</span><span class="sxs-lookup"><span data-stu-id="6f78d-150">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
6. <span data-ttu-id="6f78d-151">В браузере просмотрите исходный код страницы.</span><span class="sxs-lookup"><span data-stu-id="6f78d-151">In the browser, view the page source.</span></span> <span data-ttu-id="6f78d-152">Вы сможете см. в разделе аналитики, готовый для просмотра кода:</span><span class="sxs-lookup"><span data-stu-id="6f78d-152">You'll be able to see the rendered analytics code:</span></span>

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. <span data-ttu-id="6f78d-153">Войдите на сайт Google Analytics и статистику для вашего сайта.</span><span class="sxs-lookup"><span data-stu-id="6f78d-153">Log onto the Google Analytics site and examine the statistics for your site.</span></span> <span data-ttu-id="6f78d-154">Если вы используете страницы на действующем сайте, вы увидите запись, которая регистрирует посетите страницу.</span><span class="sxs-lookup"><span data-stu-id="6f78d-154">If you're running the page on a live site, you see an entry that logs the visit to your page.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="6f78d-155">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="6f78d-155">Additional Resources</span></span>

- [<span data-ttu-id="6f78d-156">Сайт Google Analytics</span><span class="sxs-lookup"><span data-stu-id="6f78d-156">Google Analytics site</span></span>](https://www.google.com/analytics/)
- [<span data-ttu-id="6f78d-157">Yahoo! Web Analytics сайта</span><span class="sxs-lookup"><span data-stu-id="6f78d-157">Yahoo! Web Analytics site</span></span>](http://help.yahoo.com/l/us/yahoo/ywa/)
- [<span data-ttu-id="6f78d-158">StatCounter сайта</span><span class="sxs-lookup"><span data-stu-id="6f78d-158">StatCounter site</span></span>](http://statcounter.com/)
