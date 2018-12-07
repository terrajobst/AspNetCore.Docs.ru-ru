---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
title: Знакомство с веб-страниц ASP.NET — публикация сайта с помощью WebMatrix | Документация Майкрософт
author: Rick-Anderson
description: Это руководство представляет собой заключительной учебного набора, которые представлены веб-страниц ASP.NET и Microsoft WebMatrix. В этом примере рассматривается публикация веб-узла t...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 7e85c70e-1a88-4408-8b3d-29611c7713ed
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
msc.type: authoredcontent
ms.openlocfilehash: bd6611a03ee4940f5d4176ce23464f313b9ec884
ms.sourcegitcommit: eef99d14d96dc8c3c1bb0e2c4cb14da152f8a952
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/07/2018
ms.locfileid: "53022509"
---
<a name="introducing-aspnet-web-pages---publishing-a-site-by-using-webmatrix"></a><span data-ttu-id="0809e-104">Знакомство с веб-страниц ASP.NET — публикация сайта с помощью WebMatrix</span><span class="sxs-lookup"><span data-stu-id="0809e-104">Introducing ASP.NET Web Pages - Publishing a Site by Using WebMatrix</span></span>
====================
<span data-ttu-id="0809e-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="0809e-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="0809e-106">Это руководство представляет собой заключительной учебного набора, которые представлены веб-страниц ASP.NET и Microsoft WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="0809e-106">This tutorial is the final installment in the tutorial set that introduces ASP.NET Web Pages and Microsoft WebMatrix.</span></span> <span data-ttu-id="0809e-107">Он описывает, как для публикации своего сайта к Интернету, чтобы другие пользователи могли работать с ним.</span><span class="sxs-lookup"><span data-stu-id="0809e-107">It discusses how to publish your site to the Internet so that others can work with it.</span></span> <span data-ttu-id="0809e-108">Предполагается, вы выполнили рядов через [Создание согласованного поиска для сайтов веб-страниц ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251585).</span><span class="sxs-lookup"><span data-stu-id="0809e-108">It assumes you have completed the series through [Creating a Consistent Look for ASP.NET Web Pages Sites](https://go.microsoft.com/fwlink/?LinkId=251585).</span></span>
> 
> <span data-ttu-id="0809e-109">Вы узнаете, как для публикации своего сайта с помощью:</span><span class="sxs-lookup"><span data-stu-id="0809e-109">You'll learn how to publish your site using:</span></span>
> 
> - <span data-ttu-id="0809e-110">Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="0809e-110">Microsoft Azure</span></span>
> - <span data-ttu-id="0809e-111">Веб-хостинга</span><span class="sxs-lookup"><span data-stu-id="0809e-111">Web Hosting Company</span></span>


## <a name="about-publishing-your-site"></a><span data-ttu-id="0809e-112">О публикации веб-узла</span><span class="sxs-lookup"><span data-stu-id="0809e-112">About Publishing Your Site</span></span>

<span data-ttu-id="0809e-113">Пока мы сделали всю работу на локальном компьютере, включая тестирование страниц.</span><span class="sxs-lookup"><span data-stu-id="0809e-113">Up to now, you've done all your work on a local computer, including testing your pages.</span></span> <span data-ttu-id="0809e-114">Для запуска вашего<em>.cshtml</em> страниц, вы использовали веб-сервер, который встроен в WebMatrix, а именно: IIS Express.</span><span class="sxs-lookup"><span data-stu-id="0809e-114">To run your<em>.cshtml</em> pages, you've used the web server that's built into WebMatrix, namely IIS Express.</span></span> <span data-ttu-id="0809e-115">Но само собой никто можно см. на сайте, созданные за исключением того, вы.</span><span class="sxs-lookup"><span data-stu-id="0809e-115">But of course no one can see the site you've created except you.</span></span> <span data-ttu-id="0809e-116">Другим пользователям работать с веб-узла, необходимо опубликовать его в Интернете.</span><span class="sxs-lookup"><span data-stu-id="0809e-116">To let others work with your site, you have to publish it to the Internet.</span></span>

<span data-ttu-id="0809e-117">Если уже имеется доступ к общедоступной веб-сервер публикации означает, что учетная запись с *облачная платформа* или *поставщика услуг размещения*.</span><span class="sxs-lookup"><span data-stu-id="0809e-117">Unless you have access to a public web server already, publishing means that you have to have an account with a *cloud platform* or a *hosting provider*.</span></span> <span data-ttu-id="0809e-118">Облачная платформа, например Microsoft Azure предоставляет инфраструктуру по запросу для ваших приложений.</span><span class="sxs-lookup"><span data-stu-id="0809e-118">A cloud platform, such as Microsoft Azure, provides on-demand infrastructure for your applications.</span></span> <span data-ttu-id="0809e-119">Поставщик услуг размещения — компании, которому принадлежит общедоступный веб-серверов и который будет арендовать вы пространства для узла.</span><span class="sxs-lookup"><span data-stu-id="0809e-119">A hosting provider is a company that owns publicly accessible web servers and that will rent you space for your site.</span></span> <span data-ttu-id="0809e-120">Планы выполнения из за несколько долларов в месяц (или бесплатно) размещения для небольших узлов в несколько сотен долларов в месяц для крупномасштабных коммерческих веб-сайтов.</span><span class="sxs-lookup"><span data-stu-id="0809e-120">Hosting plans run from a few dollars a month (or even free) for small sites to many hundreds of dollars a month for high-volume commercial websites.</span></span>

> [!NOTE]
> <span data-ttu-id="0809e-121">Возможно, доступ к общедоступной веб-сервера через поставщик услуг Интернета (ISP), можно использовать для получения услуг Интернета дома.</span><span class="sxs-lookup"><span data-stu-id="0809e-121">You might have access to a public web server via the internet service provider (ISP) that you use to get internet service at home.</span></span> <span data-ttu-id="0809e-122">Тем не менее ваш поставщик услуг размещения должен поддерживать веб-страниц ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0809e-122">However, your hosting provider must support ASP.NET Web Pages.</span></span> <span data-ttu-id="0809e-123">Не многие поставщики услуг Интернета, а это всегда стоит посмотреть.</span><span class="sxs-lookup"><span data-stu-id="0809e-123">Many ISPs don't, but it's always worth checking.</span></span>


<span data-ttu-id="0809e-124">В этом руководстве мы предоставим вам обзор того, как для публикации.</span><span class="sxs-lookup"><span data-stu-id="0809e-124">In this tutorial, we'll give you an overview of how to publish.</span></span> <span data-ttu-id="0809e-125">Это не непрактично предоставлять точные сведения для всех операций, так как процесс немного отличается для каждого поставщика услуг размещения.</span><span class="sxs-lookup"><span data-stu-id="0809e-125">It's not practical to provide exact details for everything, because the process differs a bit for every hosting provider.</span></span> <span data-ttu-id="0809e-126">Но вы получите хорошее представление о том, как работает этот процесс.</span><span class="sxs-lookup"><span data-stu-id="0809e-126">But you'll get a good idea of how the process works.</span></span>

<span data-ttu-id="0809e-127">Этот учебник содержит четыре раздела:</span><span class="sxs-lookup"><span data-stu-id="0809e-127">This tutorial contains four sections:</span></span>

1. [<span data-ttu-id="0809e-128">Настройка страницы по умолчанию</span><span class="sxs-lookup"><span data-stu-id="0809e-128">Setting up the default page</span></span>](#defaultpage)
2. <span data-ttu-id="0809e-129">Публикация (выберите один из следующих)</span><span class="sxs-lookup"><span data-stu-id="0809e-129">Publishing (choose one of the following)</span></span>  
 <span data-ttu-id="0809e-130">1.</span><span class="sxs-lookup"><span data-stu-id="0809e-130">a.</span></span> [<span data-ttu-id="0809e-131">Публикация узла в Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="0809e-131">Publishing Your Site to Microsoft Azure</span></span>](#azure)  
 <span data-ttu-id="0809e-132">2.</span><span class="sxs-lookup"><span data-stu-id="0809e-132">b.</span></span> [<span data-ttu-id="0809e-133">Публикация узла для веб-хостинга</span><span class="sxs-lookup"><span data-stu-id="0809e-133">Publishing Your Site to a Web Hosting Company</span></span>](#host)
3. [<span data-ttu-id="0809e-134">Обновление активного сайта: повторная публикация</span><span class="sxs-lookup"><span data-stu-id="0809e-134">Updating the Live Site: Republishing</span></span>](#update)

<a id="defaultpage"></a>
## <a name="setting-up-the-default-page"></a><span data-ttu-id="0809e-135">Настройка страницы по умолчанию</span><span class="sxs-lookup"><span data-stu-id="0809e-135">Setting up the default page</span></span>

<span data-ttu-id="0809e-136">При переходе к базовому адресу для веб-сайт, для пользователя отображается страница по умолчанию для веб-узла.</span><span class="sxs-lookup"><span data-stu-id="0809e-136">When a user navigates to the base address for your web site, the default page for your site is displayed to the user.</span></span> <span data-ttu-id="0809e-137">Например, если *Default.htm* имеет значение по умолчанию для сайта по адресу `www.contoso.com`, затем открыть меню `www.contoso.com` совпадает со значением переход к `www.contoso.com/Default.htm`.</span><span class="sxs-lookup"><span data-stu-id="0809e-137">For example, when *Default.htm* is set as the default page for the site at `www.contoso.com`, then navigating to `www.contoso.com` is the same as navigating to `www.contoso.com/Default.htm`.</span></span>

<span data-ttu-id="0809e-138">В настоящее время веб-сайт использует **Default.cshtml** страницей по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0809e-138">Currently, your site uses **Default.cshtml** as the default page.</span></span> <span data-ttu-id="0809e-139">Эта страница работает для страницы по умолчанию, но в этом руководстве вы не добавили любое содержимое на эту страницу, поэтому он будет отображаться пустая страница.</span><span class="sxs-lookup"><span data-stu-id="0809e-139">This page is fine for your default page, but in this tutorial you have not added any content to that page so it would display a blank page.</span></span> <span data-ttu-id="0809e-140">Откройте Default.cshtml и замените содержимое следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="0809e-140">Open Default.cshtml and replace the content with the following code.</span></span>

[!code-cshtml[Main](publishing/samples/sample1.cshtml)]

<span data-ttu-id="0809e-141">Теперь веб-узла будет готова к публикации.</span><span class="sxs-lookup"><span data-stu-id="0809e-141">Now your site is ready for publication.</span></span> <span data-ttu-id="0809e-142">Во-первых вы увидите, как развернуть сайт в Azure, а затем развернуть его в веб-хостинга.</span><span class="sxs-lookup"><span data-stu-id="0809e-142">First, you will see how to deploy the site to Azure, and then how to deploy it to a web hosting company.</span></span> <span data-ttu-id="0809e-143">Либо вариант работает в веб-сайт, а также требуется только один из вариантов развертывания.</span><span class="sxs-lookup"><span data-stu-id="0809e-143">Either option works for your web site, and you only need to follow one of the deployment options.</span></span>

<a id="azure"></a>
## <a name="publishing-your-site-to-microsoft-azure"></a><span data-ttu-id="0809e-144">Публикация узла в Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="0809e-144">Publishing Your Site to Microsoft Azure</span></span>

<span data-ttu-id="0809e-145">Этот учебник будет сначала показано, как для развертывания сайта в Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="0809e-145">This tutorial will first show you how to deploy your site to Microsoft Azure.</span></span> <span data-ttu-id="0809e-146">Выполнив вход с учетной записью Майкрософт, можно создать до 10 бесплатных сайтов в Azure.</span><span class="sxs-lookup"><span data-stu-id="0809e-146">By signing in with a Microsoft account, you can create up to 10 free sites on Azure.</span></span> <span data-ttu-id="0809e-147">Эти бесплатные сайты предоставляют удобный способ для тестирования веб-сайтов.</span><span class="sxs-lookup"><span data-stu-id="0809e-147">These free sites provide a convenient way to test your sites.</span></span> <span data-ttu-id="0809e-148">Вы всегда можете удалить этот пример веб-сайт позже старайтесь не использовать все бесплатные узлов.</span><span class="sxs-lookup"><span data-stu-id="0809e-148">You can always delete this example site later to avoid using all of your free sites.</span></span> <span data-ttu-id="0809e-149">Можно создать бесплатную пробную учетную запись всего за несколько минут.</span><span class="sxs-lookup"><span data-stu-id="0809e-149">You can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="0809e-150">Дополнительные сведения см. в разделе [бесплатной пробной версии Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="0809e-150">For details, see [Azure Free Trial](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span>

<span data-ttu-id="0809e-151">На ленте WebMatrix, щелкните **публикации** кнопки.</span><span class="sxs-lookup"><span data-stu-id="0809e-151">In the WebMatrix ribbon, click the **Publish** button.</span></span>

![Кнопка «Опубликовать» в ленте WebMatrix](publishing/_static/image1.png)

<span data-ttu-id="0809e-153">**Опубликовать свой сайт** диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="0809e-153">The **Publish Your Site** dialog box is displayed.</span></span> <span data-ttu-id="0809e-154">Если не вошли учетную запись Майкрософт будет содержать окно **приступить к работе с Azure** ссылку.</span><span class="sxs-lookup"><span data-stu-id="0809e-154">If you have not signed in to your Microsoft account, the dialog box will contain a **Get started with Azure** link.</span></span> <span data-ttu-id="0809e-155">Щелкните эту ссылку.</span><span class="sxs-lookup"><span data-stu-id="0809e-155">Click this link.</span></span>

![Опубликовать сайт](publishing/_static/image2.png)

<span data-ttu-id="0809e-157">Если не вошли учетную запись Майкрософт вы снова предоставляется возможность выполнить вход.</span><span class="sxs-lookup"><span data-stu-id="0809e-157">If you have not signed in to a Microsoft account, you are again given the opportunity to sign in.</span></span> <span data-ttu-id="0809e-158">Чтобы опубликовать сайт в Azure необходимо войти с учетной записью Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="0809e-158">You must sign in to a Microsoft account to publish your site on Azure.</span></span>

![Вход](publishing/_static/image3.png)

<span data-ttu-id="0809e-160">После входа в учетную запись Майкрософт, диалоговое окно содержит ссылки на создание нового сайта в Azure или подключиться к одному из существующих узлов в Azure.</span><span class="sxs-lookup"><span data-stu-id="0809e-160">After signing in to your Microsoft account, the dialog box contains links to create a new site on Azure or connect to one of your existing sites on Azure.</span></span>

![Создание нового сайта](publishing/_static/image4.png)

<span data-ttu-id="0809e-162">Выберите **создать новый сайт**.</span><span class="sxs-lookup"><span data-stu-id="0809e-162">Select **Create a new site**.</span></span>

<span data-ttu-id="0809e-163">Если название проекта **WebPagesMovies**, по умолчанию для веб-узла будет называться **webpagesmovies.azurewebsites.net**.</span><span class="sxs-lookup"><span data-stu-id="0809e-163">If you named your project **WebPagesMovies**, the default name for your site will be **webpagesmovies.azurewebsites.net**.</span></span> <span data-ttu-id="0809e-164">Это имя по умолчанию является скорее всего, не доступен, как указано в красный восклицательный знак.</span><span class="sxs-lookup"><span data-stu-id="0809e-164">This default name is most likely not available, as indicated by the red exclamation mark.</span></span>

![Имя веб-сайта по умолчанию](publishing/_static/image5.png)

<span data-ttu-id="0809e-166">Изменить имя узла, на что-нибудь, которая доступна и выберите расположение, которое находится недалеко от вашего расположения.</span><span class="sxs-lookup"><span data-stu-id="0809e-166">Change the site name to something that is available, and select a location that is close to your location.</span></span>

![Имя измененного узла.](publishing/_static/image6.png)

<span data-ttu-id="0809e-168">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="0809e-168">Click **OK**.</span></span>

<span data-ttu-id="0809e-169">WebMatrix performss тест, чтобы определить, совместима ли сервер с веб-узла.</span><span class="sxs-lookup"><span data-stu-id="0809e-169">WebMatrix performss a test to determine if the server is compatible with your site.</span></span>

![тест на совместимость](publishing/_static/image7.png)

<span data-ttu-id="0809e-171">Выберите **по-прежнему**.</span><span class="sxs-lookup"><span data-stu-id="0809e-171">Select **Continue**.</span></span>

<span data-ttu-id="0809e-172">Отображаются результаты проверки совместимости.</span><span class="sxs-lookup"><span data-stu-id="0809e-172">The results of the compatibility test are displayed.</span></span>

![результат совместимости](publishing/_static/image8.png)

<span data-ttu-id="0809e-174">Выберите **по-прежнему**.</span><span class="sxs-lookup"><span data-stu-id="0809e-174">Select **Continue**.</span></span>

<span data-ttu-id="0809e-175">WebMatrix отображает файлы и базы данных, которые будут опубликованы на сайте.</span><span class="sxs-lookup"><span data-stu-id="0809e-175">WebMatrix displays the files and databases that will be published to the site.</span></span> <span data-ttu-id="0809e-176">Так как это первый раз, при публикации сайта, перечислены все файлы.</span><span class="sxs-lookup"><span data-stu-id="0809e-176">Since this is the first time you are publishing the site, all of the files are listed.</span></span> <span data-ttu-id="0809e-177">Можно снять флажок файла, который еще не готов для публикации.</span><span class="sxs-lookup"><span data-stu-id="0809e-177">You can uncheck a file that is not ready to be published.</span></span> <span data-ttu-id="0809e-178">В последующих публикаций будет отображаться только файлы, которые были изменены.</span><span class="sxs-lookup"><span data-stu-id="0809e-178">In the subsequent publications, only the files that have changed will be displayed.</span></span> <span data-ttu-id="0809e-179">См. в разделе [обновление на действующем сайте: повторная публикация](#update).</span><span class="sxs-lookup"><span data-stu-id="0809e-179">See [Updating the Live Site: Republishing](#update).</span></span>

![Предварительный просмотр публикации](publishing/_static/image9.png)

<span data-ttu-id="0809e-181">Выберите **по-прежнему**.</span><span class="sxs-lookup"><span data-stu-id="0809e-181">Select **Continue**.</span></span>

<span data-ttu-id="0809e-182">После развертывания сайта в Azure, отображается сообщение, которое указывает, что развертывание будет завершено.</span><span class="sxs-lookup"><span data-stu-id="0809e-182">After the site has been deployed to Azure, a message is displayed that indicates the deployment has completed.</span></span>

![Публикация завершена](publishing/_static/image10.png)

<span data-ttu-id="0809e-184">Сайта и базы данных были опубликованы в Azure и теперь доступны всем желающим.</span><span class="sxs-lookup"><span data-stu-id="0809e-184">Your site and database have been published to Azure, and are now available to the public.</span></span> <span data-ttu-id="0809e-185">Щелкните ссылку в сообщение, указывающее завершения публикации, и теперь вы увидите развернутого сайта.</span><span class="sxs-lookup"><span data-stu-id="0809e-185">Click the link in the message indicating publishing has completed, and you will now see your deployed site.</span></span> <span data-ttu-id="0809e-186">Вы или любой пользователь с доступом к Интернету можно добавить или изменить записи в базе данных.</span><span class="sxs-lookup"><span data-stu-id="0809e-186">You or anyone with Internet access can add or modify records in the database.</span></span>

![](publishing/_static/image11.png)

<a id="host"></a>
## <a name="publishing-your-site-to-a-web-hosting-company"></a><span data-ttu-id="0809e-187">Публикация узла для веб-хостинга</span><span class="sxs-lookup"><span data-stu-id="0809e-187">Publishing Your Site to a Web Hosting Company</span></span>

<span data-ttu-id="0809e-188">Если вы решили не публиковать в Azure, можно вместо этого опубликовать сайт для веб-хостинга.</span><span class="sxs-lookup"><span data-stu-id="0809e-188">If you decide to not publish to Azure, you can instead publish your site to a web hosting company.</span></span>

<span data-ttu-id="0809e-189">Нажмите кнопку **найти веб-размещения** ссылку.</span><span class="sxs-lookup"><span data-stu-id="0809e-189">Click the **Find web hosting** link.</span></span>

![Кнопка «Найти в Интернете» в диалоговом окне Параметры публикации](publishing/_static/image12.png)

<span data-ttu-id="0809e-191">Перейдите на страницу на сайте Майкрософт со списком поставщиков услуг размещения, которые поддерживают ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0809e-191">You go to a page on the Microsoft site that lists hosting providers that support ASP.NET.</span></span>

![Страницу на сайте Microsoft, в которой перечислены поставщики услуг размещения](publishing/_static/image13.png)

<span data-ttu-id="0809e-193">Очевидно, что может быть сложно предсказать, теперь точно какие размещения функции, может потребоваться в долгосрочной перспективе.</span><span class="sxs-lookup"><span data-stu-id="0809e-193">Obviously, it can be difficult to know now exactly what hosting features you might require over the long term.</span></span> <span data-ttu-id="0809e-194">Вот несколько моментов, которые следует учитывать.</span><span class="sxs-lookup"><span data-stu-id="0809e-194">Here are a couple of things to consider:</span></span>

- <span data-ttu-id="0809e-195">В целях WebPagesMovies сайта не нужно иметь отдельный дополнительный модуль для SQL Server, который часто стоит очень.</span><span class="sxs-lookup"><span data-stu-id="0809e-195">For purposes of the WebPagesMovies site, you don't have to have a separate add-on for SQL Server, which often costs extra.</span></span> <span data-ttu-id="0809e-196">На сайте вы используете SQL Server Compact Edition, который является автономным.</span><span class="sxs-lookup"><span data-stu-id="0809e-196">In your site, you're using SQL Server Compact Edition, which is self-contained.</span></span> <span data-ttu-id="0809e-197">Тем не менее может потребоваться доступ к SQL Server для некоторых будущих веб-сайта в работе.</span><span class="sxs-lookup"><span data-stu-id="0809e-197">However, you might need SQL Server access for some future website work you do.</span></span> <span data-ttu-id="0809e-198">Если вы считаете, что может оказаться, убедитесь, что возможности SQL Server можно добавить позже.</span><span class="sxs-lookup"><span data-stu-id="0809e-198">If you think you might, make sure that you can add SQL Server capability later.</span></span>
- <span data-ttu-id="0809e-199">Проверьте, поддерживает ли поставщик услуг размещения протокол публикации веб-развертывания.</span><span class="sxs-lookup"><span data-stu-id="0809e-199">Check whether the hosting provider supports the Web Deploy publishing protocol.</span></span> <span data-ttu-id="0809e-200">Можно опубликовать с помощью протокола FTP, но более удобным для использования веб-развертывания.</span><span class="sxs-lookup"><span data-stu-id="0809e-200">You can publish by using FTP protocol, but it's more convenient to use Web Deploy.</span></span>

<span data-ttu-id="0809e-201">Некоторые сайты предлагают бесплатный пробный период.</span><span class="sxs-lookup"><span data-stu-id="0809e-201">Some sites offer a free trial period.</span></span> <span data-ttu-id="0809e-202">Бесплатная пробная версия — хороший способ попробуйте отправить публикацию и размещение при вы по-прежнему будете экспериментировать с WebMatrix и ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="0809e-202">A free trial is a good way to try publishing and hosting while you're still experimenting with WebMatrix and ASP.NET Web Pages.</span></span>

<span data-ttu-id="0809e-203">Выберите один, что вам нравится.</span><span class="sxs-lookup"><span data-stu-id="0809e-203">Pick one that you like.</span></span> <span data-ttu-id="0809e-204">В этом учебнике мы выбрали компании DiscountASP.NET, так как пока мы Создание руководства, эту компанию рекламной акции, сотрудники могут легко создать узел бесплатно в течение нескольких месяцев.</span><span class="sxs-lookup"><span data-stu-id="0809e-204">For this tutorial, we selected DiscountASP.NET, because while we were creating the tutorial, that company had a promotion that let people host a site free for a few months.</span></span>

> [!NOTE]
> <span data-ttu-id="0809e-205">На выбор поставщика услуг размещения в этом руководстве не должны интерпретироваться как является одобрением эту компанию какой-либо.</span><span class="sxs-lookup"><span data-stu-id="0809e-205">Our choice of a hosting provider for this tutorial shouldn't be interpreted as an endorsement of that company over any other.</span></span> <span data-ttu-id="0809e-206">Но нам пришлось выбрать один для иллюстрации и компании DiscountASP.NET является одной из многих компаний, которые поддерживает ASP.NET Web Pages и протокол Web Deploy для публикации.</span><span class="sxs-lookup"><span data-stu-id="0809e-206">But we had to pick one for illustration, and DiscountASP.NET is one of the many companies that supports ASP.NET Web Pages and the Web Deploy protocol for publishing.</span></span>


<span data-ttu-id="0809e-207">Как правило после вы зарегистрировались с помощью поставщика услуг размещения, компания отправляет сообщение электронной почты, который содержит имя пользователя и пароль, URL-адрес веб-сервер и т. д.</span><span class="sxs-lookup"><span data-stu-id="0809e-207">Typically, after you've signed up with the hosting provider, the company sends you an email that contains a user name and password, the URL of the web server, and so on.</span></span> <span data-ttu-id="0809e-208">Если услуг размещения поддерживает протокол веб-развертывания, они может отправить вам файл, содержащий параметры публикации, или позволяют загрузить одну.</span><span class="sxs-lookup"><span data-stu-id="0809e-208">If the hosting company supports Web Deploy protocol, they might send you a file that contains publish settings, or let you download one.</span></span> <span data-ttu-id="0809e-209">Файл параметров публикации упрощает для вас.</span><span class="sxs-lookup"><span data-stu-id="0809e-209">A publish settings file simplifies the process for you.</span></span>

<span data-ttu-id="0809e-210">Если вы зарегистрировались Готово к публикации, нажмите кнопку **публикации** кнопка на ленте WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="0809e-210">When you've signed up and are ready to publish, click the **Publish** button in the WebMatrix ribbon.</span></span> <span data-ttu-id="0809e-211">**Параметры публикации** диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="0809e-211">The **Publish Settings** dialog box is displayed.</span></span>

<span data-ttu-id="0809e-212">Если поставщик услуг размещения отправить файл параметров публикации, нажмите кнопку **Импорт параметров публикации** ссылку и импортируйте файл.</span><span class="sxs-lookup"><span data-stu-id="0809e-212">If the hosting provider sent you a publish settings file, click the **Import publish settings** link and import the file.</span></span> <span data-ttu-id="0809e-213">Если у вас нет файла параметров публикации, заполните поля, используя значения, которые хостинг-компания отправлены вам по электронной почте.</span><span class="sxs-lookup"><span data-stu-id="0809e-213">If you don't have a publish settings file, fill in the fields by using the values that the hosting company sent you in email.</span></span> <span data-ttu-id="0809e-214">Вот что **параметры публикации** диалоговое окно может выглядеть после выполнения:</span><span class="sxs-lookup"><span data-stu-id="0809e-214">Here's what the **Publish Settings** dialog box might look like when you're done:</span></span>

![Заполнить параметры публикации в диалоговом окне «Параметры публикации»](publishing/_static/image14.png)

<span data-ttu-id="0809e-216">Нажмите кнопку **Проверка подключения**.</span><span class="sxs-lookup"><span data-stu-id="0809e-216">Click **Validate Connection**.</span></span> <span data-ttu-id="0809e-217">Если все, что норме, диалоговое окно сообщает **успешно подключен**, то есть может взаимодействовать с сервером поставщика услуг размещения.</span><span class="sxs-lookup"><span data-stu-id="0809e-217">If everything is ok, the dialog box reports **Connected successfully**, which means it can communicate with the hosting provider's server.</span></span>

![Успех сообщение, если публикация параметры настроены правильно](publishing/_static/image15.png)

<span data-ttu-id="0809e-219">Если имеется проблема, WebMatrix старается не сообщит о том, какие именно:</span><span class="sxs-lookup"><span data-stu-id="0809e-219">If there's a problem, WebMatrix does its best to tell you what the problem is:</span></span>

![Сообщение об ошибке, если имеется проблема с помощью параметров публикации](publishing/_static/image16.png)

<span data-ttu-id="0809e-221">Нажмите кнопку **Сохранить** для сохранения параметров.</span><span class="sxs-lookup"><span data-stu-id="0809e-221">Click **Save** to save your settings.</span></span> <span data-ttu-id="0809e-222">В WebMatrix присутствует для выполнения тестов, чтобы убедиться в том, что он мог обмениваться данными правильно узел размещения:</span><span class="sxs-lookup"><span data-stu-id="0809e-222">WebMatrix offers to perform a test to make sure that it can communicate correctly with the hosting site:</span></span>

![Сообщение, предлагающее выполнить тестирование процесса публикации](publishing/_static/image17.png)

<span data-ttu-id="0809e-224">Нажмите кнопку **Да**.</span><span class="sxs-lookup"><span data-stu-id="0809e-224">Click **Yes**.</span></span> <span data-ttu-id="0809e-225">WebMatrix передает файлов с образцами поставщика услуг размещения.</span><span class="sxs-lookup"><span data-stu-id="0809e-225">WebMatrix uploads some sample files to the hosting provider.</span></span> <span data-ttu-id="0809e-226">По завершении проверки совместимости WebMatrix сообщает о результатах:</span><span class="sxs-lookup"><span data-stu-id="0809e-226">When the compatibility test is done, WebMatrix reports the results:</span></span>

![Результаты публикации теста](publishing/_static/image18.png)

<span data-ttu-id="0809e-228">Если вы готовы, щелкнем **Продолжить** реальную запустить процесс публикации.</span><span class="sxs-lookup"><span data-stu-id="0809e-228">If you're ready to go, go ahead and click **Continue** to start the publish process for real.</span></span> <span data-ttu-id="0809e-229">WebMatrix решает, что файлы находятся в веб-узла и уже находятся на сервере узла (а прямо сейчас нет) и можно просмотреть в процессе публикации:</span><span class="sxs-lookup"><span data-stu-id="0809e-229">WebMatrix figures out what files are in your site and are already on the host server (right now, none) and gives you a preview of the publish process:</span></span>

![Предварительная версия, какие файлы, загруженные в процессе публикации будет](publishing/_static/image19.png)

<span data-ttu-id="0809e-231">Список файлов для публикации включает в себя веб-страниц, созданных как *Movies.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0809e-231">The list of files to publish includes the web pages that you've created like *Movies.cshtml*.</span></span> <span data-ttu-id="0809e-232">Список также включает файлы для вспомогательных функций, которые вы установили, файлы для запуска SQL Server Compact Edition для вашей базы данных и т. д.</span><span class="sxs-lookup"><span data-stu-id="0809e-232">The list also includes files for helpers that you've installed, the files to run SQL Server Compact Edition for your database, and so on.</span></span> <span data-ttu-id="0809e-233">В результате процесс публикации начального может оказаться значительной.</span><span class="sxs-lookup"><span data-stu-id="0809e-233">As a result, the initial publish process can be substantial.</span></span>

<span data-ttu-id="0809e-234">Нажмите кнопку **Продолжить**.</span><span class="sxs-lookup"><span data-stu-id="0809e-234">Click **Continue**.</span></span> <span data-ttu-id="0809e-235">WebMatrix копирует файлы на сервер поставщика услуг размещения.</span><span class="sxs-lookup"><span data-stu-id="0809e-235">WebMatrix copies your files to the hosting provider's server.</span></span> <span data-ttu-id="0809e-236">По завершении, результаты отображаются в строке состояния:</span><span class="sxs-lookup"><span data-stu-id="0809e-236">When it's done, the results are reported in the status bar:</span></span>

![Сообщение о состоянии панели после успешного завершения процесса публикации](publishing/_static/image20.png)

<span data-ttu-id="0809e-238">Чтобы просмотреть активного сайта, щелкните ссылку в строке состояния.</span><span class="sxs-lookup"><span data-stu-id="0809e-238">To see your live site, click the link in the status bar.</span></span> <span data-ttu-id="0809e-239">Добавить *фильмы* URL-адрес, и вы увидите *Movies.cshtml* созданный файл:</span><span class="sxs-lookup"><span data-stu-id="0809e-239">Add *Movies* to the URL, and you'll see the *Movies.cshtml* file that you created:</span></span>

![Отображение страницы «Movies» активного сайта](publishing/_static/image21.png)

<a id="update"></a>
## <a name="updating-the-live-site-republishing"></a><span data-ttu-id="0809e-241">Обновление активного сайта: повторная публикация</span><span class="sxs-lookup"><span data-stu-id="0809e-241">Updating the Live Site: Republishing</span></span>

<span data-ttu-id="0809e-242">После публикации веб-узла (в Azure или веб-хостинга), существуют две копии &mdash; версии на компьютере и версией на поставщик услуг.</span><span class="sxs-lookup"><span data-stu-id="0809e-242">Once you've published your site (to either Azure or a web hosting company), there are two copies of it &mdash; the version on your computer and the version on the service provider.</span></span> <span data-ttu-id="0809e-243">Возможно, стоит продолжить развитие сайта (если нет ничего, как часть следующего учебного курса).</span><span class="sxs-lookup"><span data-stu-id="0809e-243">You'll probably want to continue developing the site (if nothing else, as part of the next tutorial set).</span></span> <span data-ttu-id="0809e-244">При этом вам нужно опубликовать веб-узел, чтобы скопировать изменения на компьютере поставщика услуг.</span><span class="sxs-lookup"><span data-stu-id="0809e-244">When you do, you have to republish your site in order to copy changes from your computer to the service provider.</span></span> <span data-ttu-id="0809e-245">Процесс публикации в WebMatrix можно определить, какие файлы изменились на узле и публиковать только эти файлы.</span><span class="sxs-lookup"><span data-stu-id="0809e-245">The publish process in WebMatrix can determine what files have changed on your site and publish just those files.</span></span>

<span data-ttu-id="0809e-246">Чтобы увидеть, как работает повторной публикации, откройте *Movies.cshtml* сайта, сделать некоторые небольшое изменение и затем сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="0809e-246">To see how republishing works, open the *Movies.cshtml* site, make some small change, and then save the file.</span></span> <span data-ttu-id="0809e-247">Например, измените заголовок на `Movies - Updated`.</span><span class="sxs-lookup"><span data-stu-id="0809e-247">For example, change the title to `Movies - Updated`.</span></span>

<span data-ttu-id="0809e-248">Нажмите кнопку **публикации** кнопка на ленте.</span><span class="sxs-lookup"><span data-stu-id="0809e-248">Click the **Publish** button in the ribbon.</span></span> <span data-ttu-id="0809e-249">WebMatrix определяет наличие изменений и предварительный просмотр файлов, которые он будет публиковать.</span><span class="sxs-lookup"><span data-stu-id="0809e-249">WebMatrix determines what's changed and shows you a preview of the files it will publish.</span></span>

![Диалоговое окно «Опубликовать», показывает измененные файлы, готовые для переиздания](publishing/_static/image22.png)

> [!IMPORTANT] 
> 
> <span data-ttu-id="0809e-251">По умолчанию WebMatrix публикует базы данных (*.sdf* файла) только для первой публикации сайта.</span><span class="sxs-lookup"><span data-stu-id="0809e-251">By default, WebMatrix publishes your database (*.sdf* file) only the first time you publish the site.</span></span> <span data-ttu-id="0809e-252">После публикации веб-узла и взаимодействие с веб-сайта люди, базы данных на действующем сайте обычно имеет реальные данные сайта.</span><span class="sxs-lookup"><span data-stu-id="0809e-252">Once your site is published and people are interacting with the website, the database on the live site typically has the site's real data.</span></span> <span data-ttu-id="0809e-253">Необходимо быть очень осторожным, чтобы не перезаписать активной базы данных с помощью *.sdf* файл, который находится на компьютере, который обычно содержит только тестовые данные.</span><span class="sxs-lookup"><span data-stu-id="0809e-253">You have to be very careful not to overwrite the live database with the *.sdf* file that's on your computer, which usually contains only test data.</span></span> <span data-ttu-id="0809e-254">Вот почему вы увидите предупреждение **публикации приведет к перезаписи всех удаленных баз данных**, и почему флажок для *WebPagesMovies.sdf* по умолчанию снят.</span><span class="sxs-lookup"><span data-stu-id="0809e-254">That's why you see the warning **Publishing will overwrite any remote databases**, and why the check box for *WebPagesMovies.sdf* is cleared by default.</span></span>


<span data-ttu-id="0809e-255">Нажмите кнопку **Продолжить**.</span><span class="sxs-lookup"><span data-stu-id="0809e-255">Click **Continue**.</span></span> <span data-ttu-id="0809e-256">WebMatrix публикует измененных файлов и показано сообщение об успешном выполнении, точно так же в первый раз вы опубликовали.</span><span class="sxs-lookup"><span data-stu-id="0809e-256">WebMatrix publishes the changed files and shows you a success message, like it did the first time you published.</span></span>

<span data-ttu-id="0809e-257">Перейдите на действующем сайте (вы щелкните ссылку в сообщение об успешном выполнении, если она по-прежнему отображается) и убедитесь, что изменения были опубликованы.</span><span class="sxs-lookup"><span data-stu-id="0809e-257">Go to the live site (you can click the link in the success message if it's still showing) and verify that your change has been published.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="0809e-258">**Удаленное редактирование файлов**</span><span class="sxs-lookup"><span data-stu-id="0809e-258">**Editing Files Remotely**</span></span>
> 
> <span data-ttu-id="0809e-259">Вместо изменения веб-узла и последующая повторная публикация можно редактировать удаленные файлы непосредственно в WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="0809e-259">As an alternative to changing your site and then republishing, you can edit remote files directly in WebMatrix.</span></span> <span data-ttu-id="0809e-260">В этом случае при открытии файла, к поставщику услуг и WebMatrix загружает копию его для редактирования.</span><span class="sxs-lookup"><span data-stu-id="0809e-260">In this scenario, you open a file that's on the service provider, and WebMatrix downloads a copy of it for you to edit.</span></span> <span data-ttu-id="0809e-261">Каждый раз при сохранении файла, WebMatrix отправляет изменения на сайт.</span><span class="sxs-lookup"><span data-stu-id="0809e-261">Every time you save the file, WebMatrix sends the changes to the site.</span></span>
> 
> <span data-ttu-id="0809e-262">Удаленного редактирования позволяет быстро и удобно для изменения активного сайта.</span><span class="sxs-lookup"><span data-stu-id="0809e-262">Remote editing is an easy way to make changes to your live site.</span></span> <span data-ttu-id="0809e-263">Однако изменения, внесенные таким образом не синхронизированы с файлами на локальном узле.</span><span class="sxs-lookup"><span data-stu-id="0809e-263">However, the changes you make this way aren't synchronized with the files in your local site.</span></span> <span data-ttu-id="0809e-264">Чтобы синхронизировать локальные файлы с удаленного узла, можно загрузить удаленные файлы.</span><span class="sxs-lookup"><span data-stu-id="0809e-264">To synchronize the local files with the remote site, you can download the remote files.</span></span> <span data-ttu-id="0809e-265">Этот процесс работает очень похоже на публикации, за исключением в обратном порядке.</span><span class="sxs-lookup"><span data-stu-id="0809e-265">This process works much like publishing, except in reverse.</span></span>
> 
> <span data-ttu-id="0809e-266">Мы не описать более удаленного редактирования и удаленной загрузки средств WebMatrix здесь.</span><span class="sxs-lookup"><span data-stu-id="0809e-266">We won't describe more about the remote-editing and remote-download facilities of WebMatrix here.</span></span> <span data-ttu-id="0809e-267">Они весьма полезен в том случае, если несколько человек был работать на том же узле, на разных компьютерах.</span><span class="sxs-lookup"><span data-stu-id="0809e-267">They're quite useful if multiple people have to work on the same site on different computers.</span></span> <span data-ttu-id="0809e-268">Дополнительные сведения см. в разделе [публикации и редактирования на удаленный сайт с помощью бета-версии 2 WebMatrix](https://go.microsoft.com/fwlink/?LinkId=251591).</span><span class="sxs-lookup"><span data-stu-id="0809e-268">For more information, see [Publish and Edit a Remote Site with WebMatrix 2 Beta](https://go.microsoft.com/fwlink/?LinkId=251591).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="0809e-269">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="0809e-269">Additional Resources</span></span>

- <span data-ttu-id="0809e-270">[Форум по ASP.NET WebMatrix ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages), это прекрасная возможность публиковать вопросы и ответы.</span><span class="sxs-lookup"><span data-stu-id="0809e-270">[ASP.NET WebMatrix ASP.NET Web Pages forum](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages), a great place to post questions and get answers.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0809e-271">Назад</span><span class="sxs-lookup"><span data-stu-id="0809e-271">Previous</span></span>](layouts.md)
