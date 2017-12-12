---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
title: "Введение в ASP.NET Web Pages — публикации узла с помощью WebMatrix | Документы Microsoft"
author: tfitzmac
description: "Этот учебник предназначен последнем выпуске учебника набора, представляет веб-страниц ASP.NET и Microsoft WebMatrix. В этом примере рассматривается публикация веб-узла t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 7e85c70e-1a88-4408-8b3d-29611c7713ed
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
msc.type: authoredcontent
ms.openlocfilehash: 1e718c92a2f94df50fcf7af6859917746a4982ac
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---publishing-a-site-by-using-webmatrix"></a><span data-ttu-id="eb7af-104">Общие сведения о веб-страницах ASP.NET - публикации узла с помощью WebMatrix</span><span class="sxs-lookup"><span data-stu-id="eb7af-104">Introducing ASP.NET Web Pages - Publishing a Site by Using WebMatrix</span></span>
====================
<span data-ttu-id="eb7af-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="eb7af-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="eb7af-106">Этот учебник предназначен последнем выпуске учебника набора, представляет веб-страниц ASP.NET и Microsoft WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="eb7af-106">This tutorial is the final installment in the tutorial set that introduces ASP.NET Web Pages and Microsoft WebMatrix.</span></span> <span data-ttu-id="eb7af-107">В этом примере рассматривается публикация веб-узла в Интернете, чтобы другие пользователи могли работать с ним.</span><span class="sxs-lookup"><span data-stu-id="eb7af-107">It discusses how to publish your site to the Internet so that others can work with it.</span></span> <span data-ttu-id="eb7af-108">Предполагается, что завершена ряда через [Создание согласованного поиска для сайтов веб-страниц ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251585).</span><span class="sxs-lookup"><span data-stu-id="eb7af-108">It assumes you have completed the series through [Creating a Consistent Look for ASP.NET Web Pages Sites](https://go.microsoft.com/fwlink/?LinkId=251585).</span></span>
> 
> <span data-ttu-id="eb7af-109">Вы узнаете, как публикация сайта с помощью:</span><span class="sxs-lookup"><span data-stu-id="eb7af-109">You'll learn how to publish your site using:</span></span>
> 
> - <span data-ttu-id="eb7af-110">Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="eb7af-110">Microsoft Azure</span></span>
> - <span data-ttu-id="eb7af-111">Веб-хостинга</span><span class="sxs-lookup"><span data-stu-id="eb7af-111">Web Hosting Company</span></span>


## <a name="about-publishing-your-site"></a><span data-ttu-id="eb7af-112">О публикации веб-узла</span><span class="sxs-lookup"><span data-stu-id="eb7af-112">About Publishing Your Site</span></span>

<span data-ttu-id="eb7af-113">До сих выполнить всю работу на локальном компьютере, в том числе тестировании страниц.</span><span class="sxs-lookup"><span data-stu-id="eb7af-113">Up to now, you've done all your work on a local computer, including testing your pages.</span></span> <span data-ttu-id="eb7af-114">Для запуска вашего*.cshtml* страниц, вы использовали веб-сервер, который встроен в WebMatrix, а именно IIS Express.</span><span class="sxs-lookup"><span data-stu-id="eb7af-114">To run your*.cshtml* pages, you've used the web server that's built into WebMatrix, namely IIS Express.</span></span> <span data-ttu-id="eb7af-115">Но конечно никто видеть узла, который вы создали только.</span><span class="sxs-lookup"><span data-stu-id="eb7af-115">But of course no one can see the site you've created except you.</span></span> <span data-ttu-id="eb7af-116">Другим пользователям работать с веб-узла, необходимо опубликовать в Интернете.</span><span class="sxs-lookup"><span data-stu-id="eb7af-116">To let others work with your site, you have to publish it to the Internet.</span></span>

<span data-ttu-id="eb7af-117">Если уже имеется доступ на открытый веб-сервере, публикация означает, что учетная запись с *облачная платформа* или *поставщика услуг размещения*.</span><span class="sxs-lookup"><span data-stu-id="eb7af-117">Unless you have access to a public web server already, publishing means that you have to have an account with a *cloud platform* or a *hosting provider*.</span></span> <span data-ttu-id="eb7af-118">Облачная платформа, например Microsoft Azure предоставляет инфраструктуру по запросу для ваших приложений.</span><span class="sxs-lookup"><span data-stu-id="eb7af-118">A cloud platform, such as Microsoft Azure, provides on-demand infrastructure for your applications.</span></span> <span data-ttu-id="eb7af-119">Поставщик услуг размещения — компании, которому принадлежит общедоступных веб-серверов и, будет сдавать в аренду вы пространство для веб-узла.</span><span class="sxs-lookup"><span data-stu-id="eb7af-119">A hosting provider is a company that owns publicly accessible web servers and that will rent you space for your site.</span></span> <span data-ttu-id="eb7af-120">Размещение планы выполнения из нескольких долларов в месяц (или даже бесплатно) для небольших сайтов для сотен долларов в месяц для крупномасштабных коммерческих веб-сайтов.</span><span class="sxs-lookup"><span data-stu-id="eb7af-120">Hosting plans run from a few dollars a month (or even free) for small sites to many hundreds of dollars a month for high-volume commercial websites.</span></span>

> [!NOTE]
> <span data-ttu-id="eb7af-121">Может потребоваться доступ к серверу через поставщик услуг Интернета (ISP), который позволяет получить службы IIS дома общедоступной сети.</span><span class="sxs-lookup"><span data-stu-id="eb7af-121">You might have access to a public web server via the internet service provider (ISP) that you use to get internet service at home.</span></span> <span data-ttu-id="eb7af-122">Однако у поставщика услуг размещения должен поддерживать веб-страниц ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="eb7af-122">However, your hosting provider must support ASP.NET Web Pages.</span></span> <span data-ttu-id="eb7af-123">Многие поставщики услуг Интернета не, но он всегда следует.</span><span class="sxs-lookup"><span data-stu-id="eb7af-123">Many ISPs don't, but it's always worth checking.</span></span>


<span data-ttu-id="eb7af-124">В этом учебнике мы указываем Общие сведения для публикации.</span><span class="sxs-lookup"><span data-stu-id="eb7af-124">In this tutorial, we'll give you an overview of how to publish.</span></span> <span data-ttu-id="eb7af-125">Он не имеет смысла для предоставления точных сведений для всех функций, так как процесс немного отличается для каждого поставщика услуг размещения.</span><span class="sxs-lookup"><span data-stu-id="eb7af-125">It's not practical to provide exact details for everything, because the process differs a bit for every hosting provider.</span></span> <span data-ttu-id="eb7af-126">Однако вы сможете получить хорошее представление о том, как работает этот процесс.</span><span class="sxs-lookup"><span data-stu-id="eb7af-126">But you'll get a good idea of how the process works.</span></span>

<span data-ttu-id="eb7af-127">Этот учебник содержит четыре раздела:</span><span class="sxs-lookup"><span data-stu-id="eb7af-127">This tutorial contains four sections:</span></span>

1. [<span data-ttu-id="eb7af-128">Настройка страницы по умолчанию</span><span class="sxs-lookup"><span data-stu-id="eb7af-128">Setting up the default page</span></span>](#defaultpage)
2. <span data-ttu-id="eb7af-129">Публикация (выберите один из следующих)</span><span class="sxs-lookup"><span data-stu-id="eb7af-129">Publishing (choose one of the following)</span></span>  
 <span data-ttu-id="eb7af-130">1.</span><span class="sxs-lookup"><span data-stu-id="eb7af-130">a.</span></span> [<span data-ttu-id="eb7af-131">Публикация веб-узла в Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="eb7af-131">Publishing Your Site to Microsoft Azure</span></span>](#azure)  
 <span data-ttu-id="eb7af-132">2.</span><span class="sxs-lookup"><span data-stu-id="eb7af-132">b.</span></span> [<span data-ttu-id="eb7af-133">Публикация веб-узла в веб-хостинга</span><span class="sxs-lookup"><span data-stu-id="eb7af-133">Publishing Your Site to a Web Hosting Company</span></span>](#host)
3. [<span data-ttu-id="eb7af-134">Обновление на действующем сайте: повторная публикация</span><span class="sxs-lookup"><span data-stu-id="eb7af-134">Updating the Live Site: Republishing</span></span>](#update)

<a id="defaultpage"></a>
## <a name="setting-up-the-default-page"></a><span data-ttu-id="eb7af-135">Настройка страницы по умолчанию</span><span class="sxs-lookup"><span data-stu-id="eb7af-135">Setting up the default page</span></span>

<span data-ttu-id="eb7af-136">При переходе к базовому адресу для веб-сайта, для пользователя отображается страница по умолчанию для веб-узла.</span><span class="sxs-lookup"><span data-stu-id="eb7af-136">When a user navigates to the base address for your web site, the default page for your site is displayed to the user.</span></span> <span data-ttu-id="eb7af-137">Например, когда Default.htm устанавливается по умолчанию для сайта во www.contoso.com, затем перейдите в меню **www.contoso.com** является таким же, как переход к **www.contoso.com/Default.htm**.</span><span class="sxs-lookup"><span data-stu-id="eb7af-137">For example, when Default.htm is set as the default page for the site at www.contoso.com, then navigating to **www.contoso.com** is the same as navigating to **www.contoso.com/Default.htm**.</span></span>

<span data-ttu-id="eb7af-138">В настоящее время используется сайтом **Default.cshtml** со страницей по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="eb7af-138">Currently, your site uses **Default.cshtml** as the default page.</span></span> <span data-ttu-id="eb7af-139">Эта страница подходит в качестве страницы по умолчанию, но в этом учебнике вы не добавили все содержимое на эту страницу, поэтому он будет отображаться пустая страница.</span><span class="sxs-lookup"><span data-stu-id="eb7af-139">This page is fine for your default page, but in this tutorial you have not added any content to that page so it would display a blank page.</span></span> <span data-ttu-id="eb7af-140">Откройте Default.cshtml и замените содержимое следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="eb7af-140">Open Default.cshtml and replace the content with the following code.</span></span>

[!code-cshtml[Main](publishing/samples/sample1.cshtml)]

<span data-ttu-id="eb7af-141">Теперь все готово для публикации веб-узла.</span><span class="sxs-lookup"><span data-stu-id="eb7af-141">Now your site is ready for publication.</span></span> <span data-ttu-id="eb7af-142">Во-первых вы увидите, как развертывание узла в Azure, а затем развернуть его на веб-хостинга.</span><span class="sxs-lookup"><span data-stu-id="eb7af-142">First, you will see how to deploy the site to Azure, and then how to deploy it to a web hosting company.</span></span> <span data-ttu-id="eb7af-143">Любой вариант работает в веб-сайте, и требуется выполнять только один из параметров развертывания.</span><span class="sxs-lookup"><span data-stu-id="eb7af-143">Either option works for your web site, and you only need to follow one of the deployment options.</span></span>

<a id="azure"></a>
## <a name="publishing-your-site-to-microsoft-azure"></a><span data-ttu-id="eb7af-144">Публикация веб-узла в Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="eb7af-144">Publishing Your Site to Microsoft Azure</span></span>

<span data-ttu-id="eb7af-145">Этот учебник будет сначала показано, как развертывание веб-узла в Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="eb7af-145">This tutorial will first show you how to deploy your site to Microsoft Azure.</span></span> <span data-ttu-id="eb7af-146">При входе с учетной записью Майкрософт, можно создать до 10 бесплатных узлов в Azure.</span><span class="sxs-lookup"><span data-stu-id="eb7af-146">By signing in with a Microsoft account, you can create up to 10 free sites on Azure.</span></span> <span data-ttu-id="eb7af-147">Эти бесплатные веб-сайты предоставляют удобный способ для тестирования веб-сайтов.</span><span class="sxs-lookup"><span data-stu-id="eb7af-147">These free sites provide a convenient way to test your sites.</span></span> <span data-ttu-id="eb7af-148">Всегда можно удалить этот сайт примере позже можно отказаться от всех свободных узлов.</span><span class="sxs-lookup"><span data-stu-id="eb7af-148">You can always delete this example site later to avoid using all of your free sites.</span></span> <span data-ttu-id="eb7af-149">Можно создать бесплатную пробную учетную запись всего за несколько минут.</span><span class="sxs-lookup"><span data-stu-id="eb7af-149">You can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="eb7af-150">Дополнительные сведения см. в разделе [бесплатная пробная версия](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="eb7af-150">For details, see [Azure Free Trial](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span>

<span data-ttu-id="eb7af-151">На ленте WebMatrix щелкните **публикации** кнопки.</span><span class="sxs-lookup"><span data-stu-id="eb7af-151">In the WebMatrix ribbon, click the **Publish** button.</span></span>

![Публикация кнопки на ленте WebMatrix](publishing/_static/image1.png)

<span data-ttu-id="eb7af-153">**Опубликовать свой сайт** диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="eb7af-153">The **Publish Your Site** dialog box is displayed.</span></span> <span data-ttu-id="eb7af-154">Если не вошли в учетную запись Майкрософт диалоговое окно будет содержать **Приступая к работе с Azure** ссылку.</span><span class="sxs-lookup"><span data-stu-id="eb7af-154">If you have not signed in to your Microsoft account, the dialog box will contain a **Get started with Azure** link.</span></span> <span data-ttu-id="eb7af-155">Щелкните эту ссылку.</span><span class="sxs-lookup"><span data-stu-id="eb7af-155">Click this link.</span></span>

![Публикация веб-узла](publishing/_static/image2.png)

<span data-ttu-id="eb7af-157">Не вошли в учетную запись Майкрософт снова предоставляется возможность выполнить вход.</span><span class="sxs-lookup"><span data-stu-id="eb7af-157">If you have not signed in to a Microsoft account, you are again given the opportunity to sign in.</span></span> <span data-ttu-id="eb7af-158">Для публикации сайта в Azure необходимо войти в учетную запись Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="eb7af-158">You must sign in to a Microsoft account to publish your site on Azure.</span></span>

![Войти](publishing/_static/image3.png)

<span data-ttu-id="eb7af-160">После входа в учетную запись Майкрософт, диалоговое окно содержит ссылки для создания нового сайта в Azure или подключиться к одной из существующих узлов в Azure.</span><span class="sxs-lookup"><span data-stu-id="eb7af-160">After signing in to your Microsoft account, the dialog box contains links to create a new site on Azure or connect to one of your existing sites on Azure.</span></span>

![Создание нового сайта](publishing/_static/image4.png)

<span data-ttu-id="eb7af-162">Выберите **создать новый сайт**.</span><span class="sxs-lookup"><span data-stu-id="eb7af-162">Select **Create a new site**.</span></span>

<span data-ttu-id="eb7af-163">Если название проекта **WebPagesMovies**, имя по умолчанию для веб-узла будет **webpagesmovies.azurewebsites.net**.</span><span class="sxs-lookup"><span data-stu-id="eb7af-163">If you named your project **WebPagesMovies**, the default name for your site will be **webpagesmovies.azurewebsites.net**.</span></span> <span data-ttu-id="eb7af-164">Скорее всего, это имя по умолчанию не доступна, что указывает красный восклицательный знак.</span><span class="sxs-lookup"><span data-stu-id="eb7af-164">This default name is most likely not available, as indicated by the red exclamation mark.</span></span>

![Имя веб-сайта по умолчанию](publishing/_static/image5.png)

<span data-ttu-id="eb7af-166">Изменить имя узла в то, что доступен и выберите расположение, близкий к папке.</span><span class="sxs-lookup"><span data-stu-id="eb7af-166">Change the site name to something that is available, and select a location that is close to your location.</span></span>

![Имя измененного сайта](publishing/_static/image6.png)

<span data-ttu-id="eb7af-168">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="eb7af-168">Click **OK**.</span></span>

<span data-ttu-id="eb7af-169">WebMatrix performss тест, чтобы определить, совместима ли сервер с веб-узла.</span><span class="sxs-lookup"><span data-stu-id="eb7af-169">WebMatrix performss a test to determine if the server is compatible with your site.</span></span>

![проверки совместимости](publishing/_static/image7.png)

<span data-ttu-id="eb7af-171">Выберите **Продолжить**.</span><span class="sxs-lookup"><span data-stu-id="eb7af-171">Select **Continue**.</span></span>

<span data-ttu-id="eb7af-172">Отображаются результаты проверки совместимости.</span><span class="sxs-lookup"><span data-stu-id="eb7af-172">The results of the compatibility test are displayed.</span></span>

![результат совместимости](publishing/_static/image8.png)

<span data-ttu-id="eb7af-174">Выберите **Продолжить**.</span><span class="sxs-lookup"><span data-stu-id="eb7af-174">Select **Continue**.</span></span>

<span data-ttu-id="eb7af-175">WebMatrix отображает файлы и базы данных, которые будут опубликованы на сайте.</span><span class="sxs-lookup"><span data-stu-id="eb7af-175">WebMatrix displays the files and databases that will be published to the site.</span></span> <span data-ttu-id="eb7af-176">Поскольку это первый раз, публикации сайта, перечислены все файлы.</span><span class="sxs-lookup"><span data-stu-id="eb7af-176">Since this is the first time you are publishing the site, all of the files are listed.</span></span> <span data-ttu-id="eb7af-177">Файл, который еще не готов для публикации, можно снять.</span><span class="sxs-lookup"><span data-stu-id="eb7af-177">You can uncheck a file that is not ready to be published.</span></span> <span data-ttu-id="eb7af-178">В последующих публикаций будет отображаться только измененные файлы.</span><span class="sxs-lookup"><span data-stu-id="eb7af-178">In the subsequent publications, only the files that have changed will be displayed.</span></span> <span data-ttu-id="eb7af-179">В разделе [обновление на действующем сайте: повторная публикация](#update).</span><span class="sxs-lookup"><span data-stu-id="eb7af-179">See [Updating the Live Site: Republishing](#update).</span></span>

![Предварительный просмотр публикации](publishing/_static/image9.png)

<span data-ttu-id="eb7af-181">Выберите **Продолжить**.</span><span class="sxs-lookup"><span data-stu-id="eb7af-181">Select **Continue**.</span></span>

<span data-ttu-id="eb7af-182">После развертывания узла Azure отображается сообщение, которое указывает, что развертывание завершено.</span><span class="sxs-lookup"><span data-stu-id="eb7af-182">After the site has been deployed to Azure, a message is displayed that indicates the deployment has completed.</span></span>

![Публикация завершения](publishing/_static/image10.png)

<span data-ttu-id="eb7af-184">Сайта и базы данных были опубликованы в Azure и теперь доступны для просмотра.</span><span class="sxs-lookup"><span data-stu-id="eb7af-184">Your site and database have been published to Azure, and are now available to the public.</span></span> <span data-ttu-id="eb7af-185">Щелкните ссылку в сообщение, указывающее завершения публикации, и вы увидите развернутого узла.</span><span class="sxs-lookup"><span data-stu-id="eb7af-185">Click the link in the message indicating publishing has completed, and you will now see your deployed site.</span></span> <span data-ttu-id="eb7af-186">Вы или все, кто имеет доступ к Интернету можно добавить или изменить записи в базе данных.</span><span class="sxs-lookup"><span data-stu-id="eb7af-186">You or anyone with Internet access can add or modify records in the database.</span></span>

![](publishing/_static/image11.png)

<a id="host"></a>
## <a name="publishing-your-site-to-a-web-hosting-company"></a><span data-ttu-id="eb7af-187">Публикация веб-узла в веб-хостинга</span><span class="sxs-lookup"><span data-stu-id="eb7af-187">Publishing Your Site to a Web Hosting Company</span></span>

<span data-ttu-id="eb7af-188">Если вы решили не опубликовать в Azure, вместо этого можно опубликовать веб-узла для веб-хостинга.</span><span class="sxs-lookup"><span data-stu-id="eb7af-188">If you decide to not publish to Azure, you can instead publish your site to a web hosting company.</span></span>

<span data-ttu-id="eb7af-189">Нажмите кнопку **найти веб-размещения** ссылку.</span><span class="sxs-lookup"><span data-stu-id="eb7af-189">Click the **Find web hosting** link.</span></span>

![Кнопка «Найти размещения веб-сайтов» в диалоговом окне «Параметры публикации»](publishing/_static/image12.png)

<span data-ttu-id="eb7af-191">Перейдите на страницу на сайте Майкрософт со списком поставщиков услуг размещения, которые поддерживают ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="eb7af-191">You go to a page on the Microsoft site that lists hosting providers that support ASP.NET.</span></span>

![На сайте Microsoft со списком поставщиков услуг размещения](publishing/_static/image13.png)

<span data-ttu-id="eb7af-193">Очевидно, что может быть сложно понять теперь точно размещения возможностях могут потребоваться в долгосрочной перспективе.</span><span class="sxs-lookup"><span data-stu-id="eb7af-193">Obviously, it can be difficult to know now exactly what hosting features you might require over the long term.</span></span> <span data-ttu-id="eb7af-194">Ниже приведены несколько факторов, которые следует учитывать.</span><span class="sxs-lookup"><span data-stu-id="eb7af-194">Here are a couple of things to consider:</span></span>

- <span data-ttu-id="eb7af-195">В целях WebPagesMovies сайта не нужно иметь отдельный дополнительный для SQL Server, который часто дополнительные затраты.</span><span class="sxs-lookup"><span data-stu-id="eb7af-195">For purposes of the WebPagesMovies site, you don't have to have a separate add-on for SQL Server, which often costs extra.</span></span> <span data-ttu-id="eb7af-196">На сайте вы используете SQL Server Compact Edition, который является самодостаточным.</span><span class="sxs-lookup"><span data-stu-id="eb7af-196">In your site, you're using SQL Server Compact Edition, which is self-contained.</span></span> <span data-ttu-id="eb7af-197">Тем не менее может потребоваться доступ к SQL Server для некоторых будущих веб-сайта в работе.</span><span class="sxs-lookup"><span data-stu-id="eb7af-197">However, you might need SQL Server access for some future website work you do.</span></span> <span data-ttu-id="eb7af-198">Если вы считаете, что может оказаться, убедитесь, что можно добавить возможности SQL Server более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="eb7af-198">If you think you might, make sure that you can add SQL Server capability later.</span></span>
- <span data-ttu-id="eb7af-199">Проверьте, поддерживает ли поставщик услуг размещения протокола публикации веб-развертывания.</span><span class="sxs-lookup"><span data-stu-id="eb7af-199">Check whether the hosting provider supports the Web Deploy publishing protocol.</span></span> <span data-ttu-id="eb7af-200">Можно опубликовать с помощью протокола FTP, но он является более удобным для использования веб-развертывания.</span><span class="sxs-lookup"><span data-stu-id="eb7af-200">You can publish by using FTP protocol, but it's more convenient to use Web Deploy.</span></span>

<span data-ttu-id="eb7af-201">Некоторые сайты предоставляют бесплатный пробный период.</span><span class="sxs-lookup"><span data-stu-id="eb7af-201">Some sites offer a free trial period.</span></span> <span data-ttu-id="eb7af-202">Бесплатная пробная версия — хороший способ повторите публикацию и размещение при его по-прежнему будете экспериментировать с WebMatrix и ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="eb7af-202">A free trial is a good way to try publishing and hosting while you're still experimenting with WebMatrix and ASP.NET Web Pages.</span></span>

<span data-ttu-id="eb7af-203">Выберите один, что вам нравится.</span><span class="sxs-lookup"><span data-stu-id="eb7af-203">Pick one that you like.</span></span> <span data-ttu-id="eb7af-204">В этом учебнике мы выбрали DiscountASP.NET, так как у пока мы создали учебника, эту компанию рекламной акции, позволяют создать узел свободного несколько месяцев.</span><span class="sxs-lookup"><span data-stu-id="eb7af-204">For this tutorial, we selected DiscountASP.NET, because while we were creating the tutorial, that company had a promotion that let people host a site free for a few months.</span></span>

> [!NOTE]
> <span data-ttu-id="eb7af-205">Выбор поставщика услуг размещения в этом учебнике не должны интерпретироваться как является одобрением эту компанию какой-либо.</span><span class="sxs-lookup"><span data-stu-id="eb7af-205">Our choice of a hosting provider for this tutorial shouldn't be interpreted as an endorsement of that company over any other.</span></span> <span data-ttu-id="eb7af-206">Однако нам было необходимо выбрать одну для иллюстрации и DiscountASP.NET является одним из многих компаний, поддерживаемых веб-страниц ASP.NET, протокол веб-развертывания для публикации.</span><span class="sxs-lookup"><span data-stu-id="eb7af-206">But we had to pick one for illustration, and DiscountASP.NET is one of the many companies that supports ASP.NET Web Pages and the Web Deploy protocol for publishing.</span></span>


<span data-ttu-id="eb7af-207">Как правило после зарегистрировались с помощью поставщика услуг размещения, компания отправляет сообщение электронной почты, содержащий имя пользователя и пароль, URL-адрес веб-сервера и т. д.</span><span class="sxs-lookup"><span data-stu-id="eb7af-207">Typically, after you've signed up with the hosting provider, the company sends you an email that contains a user name and password, the URL of the web server, and so on.</span></span> <span data-ttu-id="eb7af-208">Если хостинга поддерживает протокол веб-развертывания, они может отправить вам файл, содержащий параметры публикации или позволяют загрузить.</span><span class="sxs-lookup"><span data-stu-id="eb7af-208">If the hosting company supports Web Deploy protocol, they might send you a file that contains publish settings, or let you download one.</span></span> <span data-ttu-id="eb7af-209">Файл параметров публикации упрощает процесс для вас.</span><span class="sxs-lookup"><span data-stu-id="eb7af-209">A publish settings file simplifies the process for you.</span></span>

<span data-ttu-id="eb7af-210">После зарегистрировались и готов к публикации, нажмите кнопку **публикации** кнопку на ленте WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="eb7af-210">When you've signed up and are ready to publish, click the **Publish** button in the WebMatrix ribbon.</span></span> <span data-ttu-id="eb7af-211">**Параметры публикации** диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="eb7af-211">The **Publish Settings** dialog box is displayed.</span></span>

<span data-ttu-id="eb7af-212">Если поставщик услуг размещения отправить файл параметров публикации, нажмите кнопку **импортировать параметры публикации** связывания и импорта файла.</span><span class="sxs-lookup"><span data-stu-id="eb7af-212">If the hosting provider sent you a publish settings file, click the **Import publish settings** link and import the file.</span></span> <span data-ttu-id="eb7af-213">При отсутствии файла параметров публикации, заполните поля, используя значения, которые хостинга отправил вам по электронной почте.</span><span class="sxs-lookup"><span data-stu-id="eb7af-213">If you don't have a publish settings file, fill in the fields by using the values that the hosting company sent you in email.</span></span> <span data-ttu-id="eb7af-214">Вот **параметры публикации** диалоговое окно будет выглядеть после выполнения:</span><span class="sxs-lookup"><span data-stu-id="eb7af-214">Here's what the **Publish Settings** dialog box might look like when you're done:</span></span>

![Параметры публикации заполнено, в диалоговом окне «Параметры публикации»](publishing/_static/image14.png)

<span data-ttu-id="eb7af-216">Нажмите кнопку **Проверка подключения**.</span><span class="sxs-lookup"><span data-stu-id="eb7af-216">Click **Validate Connection**.</span></span> <span data-ttu-id="eb7af-217">Если все компоненты норме, диалоговое окно сообщает **успешное подключение**, то есть могут взаимодействовать с сервером поставщика услуг размещения.</span><span class="sxs-lookup"><span data-stu-id="eb7af-217">If everything is ok, the dialog box reports **Connected successfully**, which means it can communicate with the hosting provider's server.</span></span>

![Сообщение успешно, если публикация настройки верны](publishing/_static/image15.png)

<span data-ttu-id="eb7af-219">Если проблема, WebMatrix старается не сообщит о том, какие именно:</span><span class="sxs-lookup"><span data-stu-id="eb7af-219">If there's a problem, WebMatrix does its best to tell you what the problem is:</span></span>

![Сообщение об ошибке в случае проблем с параметрами публикации](publishing/_static/image16.png)

<span data-ttu-id="eb7af-221">Нажмите кнопку **Сохранить** для сохранения параметров.</span><span class="sxs-lookup"><span data-stu-id="eb7af-221">Click **Save** to save your settings.</span></span> <span data-ttu-id="eb7af-222">В WebMatrix присутствует для выполнения тестов, чтобы убедиться в том, что он может взаимодействовать правильно узел размещения:</span><span class="sxs-lookup"><span data-stu-id="eb7af-222">WebMatrix offers to perform a test to make sure that it can communicate correctly with the hosting site:</span></span>

![Сообщение для проверки процесса публикации предложения](publishing/_static/image17.png)

<span data-ttu-id="eb7af-224">Нажмите кнопку **Да**.</span><span class="sxs-lookup"><span data-stu-id="eb7af-224">Click **Yes**.</span></span> <span data-ttu-id="eb7af-225">WebMatrix передает некоторые файлы примеров с поставщиком услуг размещения.</span><span class="sxs-lookup"><span data-stu-id="eb7af-225">WebMatrix uploads some sample files to the hosting provider.</span></span> <span data-ttu-id="eb7af-226">По завершении проверки совместимости WebMatrix сообщает о результатах:</span><span class="sxs-lookup"><span data-stu-id="eb7af-226">When the compatibility test is done, WebMatrix reports the results:</span></span>

![Результаты теста публикации](publishing/_static/image18.png)

<span data-ttu-id="eb7af-228">Если вы готовы пойти дальше и нажмите кнопку **Продолжить** в самом начать процесс публикации.</span><span class="sxs-lookup"><span data-stu-id="eb7af-228">If you're ready to go, go ahead and click **Continue** to start the publish process for real.</span></span> <span data-ttu-id="eb7af-229">WebMatrix решает, что файлы находятся в веб-узла и уже находятся на сервере узла (а прямо сейчас нет) и отобразится окно предварительного просмотра в процессе публикации:</span><span class="sxs-lookup"><span data-stu-id="eb7af-229">WebMatrix figures out what files are in your site and are already on the host server (right now, none) and gives you a preview of the publish process:</span></span>

![Предварительный просмотр какие файлы, которые будет отправлен в процессе публикации](publishing/_static/image19.png)

<span data-ttu-id="eb7af-231">Включает в себя список файлов для публикации веб-страниц, созданных как *Movies.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="eb7af-231">The list of files to publish includes the web pages that you've created like *Movies.cshtml*.</span></span> <span data-ttu-id="eb7af-232">Список также содержит файлы для помощников, которые вы уже установили, файлы для запуска SQL Server Compact Edition для своей базы данных и т. д.</span><span class="sxs-lookup"><span data-stu-id="eb7af-232">The list also includes files for helpers that you've installed, the files to run SQL Server Compact Edition for your database, and so on.</span></span> <span data-ttu-id="eb7af-233">В результате процесса начальной публикации может быть значительной.</span><span class="sxs-lookup"><span data-stu-id="eb7af-233">As a result, the initial publish process can be substantial.</span></span>

<span data-ttu-id="eb7af-234">Нажмите кнопку **Продолжить**.</span><span class="sxs-lookup"><span data-stu-id="eb7af-234">Click **Continue**.</span></span> <span data-ttu-id="eb7af-235">WebMatrix копирует файлы на сервер поставщика услуг размещения.</span><span class="sxs-lookup"><span data-stu-id="eb7af-235">WebMatrix copies your files to the hosting provider's server.</span></span> <span data-ttu-id="eb7af-236">При завершении его результаты отображаются в строке состояния:</span><span class="sxs-lookup"><span data-stu-id="eb7af-236">When it's done, the results are reported in the status bar:</span></span>

![Сообщение о состоянии панели при успешном завершении процесса публикации](publishing/_static/image20.png)

<span data-ttu-id="eb7af-238">Чтобы увидеть ваш действующий сайт, щелкните ссылку в строке состояния.</span><span class="sxs-lookup"><span data-stu-id="eb7af-238">To see your live site, click the link in the status bar.</span></span> <span data-ttu-id="eb7af-239">Добавить *фильмов* URL-адрес, и вы увидите *Movies.cshtml* созданный файл:</span><span class="sxs-lookup"><span data-stu-id="eb7af-239">Add *Movies* to the URL, and you'll see the *Movies.cshtml* file that you created:</span></span>

![Действующем сайте, показывающий страницу фильмов](publishing/_static/image21.png)

<a id="update"></a>
## <a name="updating-the-live-site-republishing"></a><span data-ttu-id="eb7af-241">Обновление на действующем сайте: повторная публикация</span><span class="sxs-lookup"><span data-stu-id="eb7af-241">Updating the Live Site: Republishing</span></span>

<span data-ttu-id="eb7af-242">После публикации веб-узла (в Azure или веб-хостинга) существуют две копии &mdash; версию на компьютере и на доступ к службе.</span><span class="sxs-lookup"><span data-stu-id="eb7af-242">Once you've published your site (to either Azure or a web hosting company), there are two copies of it &mdash; the version on your computer and the version on the service provider.</span></span> <span data-ttu-id="eb7af-243">Возможно, потребуется продолжать разработку узла (в подобной ситуации, в рамках следующего набора данных учебника).</span><span class="sxs-lookup"><span data-stu-id="eb7af-243">You'll probably want to continue developing the site (if nothing else, as part of the next tutorial set).</span></span> <span data-ttu-id="eb7af-244">При этом необходимо опубликовать веб-узел, чтобы скопировать изменения от компьютера поставщика услуг.</span><span class="sxs-lookup"><span data-stu-id="eb7af-244">When you do, you have to republish your site in order to copy changes from your computer to the service provider.</span></span> <span data-ttu-id="eb7af-245">Процесс публикации в WebMatrix можно определить, какие файлы были изменены на веб-узла и опубликовать только те файлы.</span><span class="sxs-lookup"><span data-stu-id="eb7af-245">The publish process in WebMatrix can determine what files have changed on your site and publish just those files.</span></span>

<span data-ttu-id="eb7af-246">Чтобы посмотреть, как работает переиздание откройте *Movies.cshtml* сайта, внесите изменения малых, а затем сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="eb7af-246">To see how republishing works, open the *Movies.cshtml* site, make some small change, and then save the file.</span></span> <span data-ttu-id="eb7af-247">Например, измените заголовок на `Movies - Updated`.</span><span class="sxs-lookup"><span data-stu-id="eb7af-247">For example, change the title to `Movies - Updated`.</span></span>

<span data-ttu-id="eb7af-248">Нажмите кнопку **публикации** кнопку на ленте.</span><span class="sxs-lookup"><span data-stu-id="eb7af-248">Click the **Publish** button in the ribbon.</span></span> <span data-ttu-id="eb7af-249">WebMatrix определяет наличие изменений и предварительный просмотр файлов, которые она будет опубликована.</span><span class="sxs-lookup"><span data-stu-id="eb7af-249">WebMatrix determines what's changed and shows you a preview of the files it will publish.</span></span>

![Диалоговое окно «Опубликовать», показывает измененные файлы, готовые для переиздания](publishing/_static/image22.png)

> [!IMPORTANT] 
> 
> <span data-ttu-id="eb7af-251">По умолчанию WebMatrix публикует базы данных (*.sdf* файла) только для первой публикации сайта.</span><span class="sxs-lookup"><span data-stu-id="eb7af-251">By default, WebMatrix publishes your database (*.sdf* file) only the first time you publish the site.</span></span> <span data-ttu-id="eb7af-252">После публикации веб-узла и пользователи взаимодействуют с веб-сайта, базы данных на действующем сайте обычно есть реальные данные с веб-узла.</span><span class="sxs-lookup"><span data-stu-id="eb7af-252">Once your site is published and people are interacting with the website, the database on the live site typically has the site's real data.</span></span> <span data-ttu-id="eb7af-253">Вы должны быть очень осторожность, чтобы не перезаписать действующую базу данных с *.sdf* файл, который находится на компьютере, который обычно содержит только данные теста.</span><span class="sxs-lookup"><span data-stu-id="eb7af-253">You have to be very careful not to overwrite the live database with the *.sdf* file that's on your computer, which usually contains only test data.</span></span> <span data-ttu-id="eb7af-254">Именно поэтому предупреждение о **публикация приведет к перезаписи всех удаленных баз данных**, и почему флажок *WebPagesMovies.sdf* по умолчанию снят.</span><span class="sxs-lookup"><span data-stu-id="eb7af-254">That's why you see the warning **Publishing will overwrite any remote databases**, and why the check box for *WebPagesMovies.sdf* is cleared by default.</span></span>


<span data-ttu-id="eb7af-255">Нажмите кнопку **Продолжить**.</span><span class="sxs-lookup"><span data-stu-id="eb7af-255">Click **Continue**.</span></span> <span data-ttu-id="eb7af-256">WebMatrix публикует измененных файлов и показано сообщение об успешном выполнении, как и при первом запуске публикации.</span><span class="sxs-lookup"><span data-stu-id="eb7af-256">WebMatrix publishes the changed files and shows you a success message, like it did the first time you published.</span></span>

<span data-ttu-id="eb7af-257">Перейдите на действующем сайте (если она по-прежнему отображается, могут щелкнуть ссылку в сообщение об успешном выполнении) и убедитесь, что изменения были опубликованы.</span><span class="sxs-lookup"><span data-stu-id="eb7af-257">Go to the live site (you can click the link in the success message if it's still showing) and verify that your change has been published.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="eb7af-258">**Удаленное редактирование файлов**</span><span class="sxs-lookup"><span data-stu-id="eb7af-258">**Editing Files Remotely**</span></span>
> 
> <span data-ttu-id="eb7af-259">В качестве альтернативы для изменения веб-узла и последующая повторная публикация можно редактировать удаленные файлы непосредственно в WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="eb7af-259">As an alternative to changing your site and then republishing, you can edit remote files directly in WebMatrix.</span></span> <span data-ttu-id="eb7af-260">В этом случае откройте файл, к поставщику услуг и WebMatrix загружает копию его для редактирования.</span><span class="sxs-lookup"><span data-stu-id="eb7af-260">In this scenario, you open a file that's on the service provider, and WebMatrix downloads a copy of it for you to edit.</span></span> <span data-ttu-id="eb7af-261">Каждый раз при сохранении файла, WebMatrix отправляет изменения на сайт.</span><span class="sxs-lookup"><span data-stu-id="eb7af-261">Every time you save the file, WebMatrix sends the changes to the site.</span></span>
> 
> <span data-ttu-id="eb7af-262">Изменение удаленного — легко вносить изменения в ваш действующий сайт.</span><span class="sxs-lookup"><span data-stu-id="eb7af-262">Remote editing is an easy way to make changes to your live site.</span></span> <span data-ttu-id="eb7af-263">Однако изменения, внесенные таким образом не синхронизированы с файлами в локальном сайте.</span><span class="sxs-lookup"><span data-stu-id="eb7af-263">However, the changes you make this way aren't synchronized with the files in your local site.</span></span> <span data-ttu-id="eb7af-264">Чтобы синхронизировать локальные файлы с удаленного узла, можно загрузить удаленные файлы.</span><span class="sxs-lookup"><span data-stu-id="eb7af-264">To synchronize the local files with the remote site, you can download the remote files.</span></span> <span data-ttu-id="eb7af-265">Этот процесс работает очень похоже на публикации, за исключением того, в обратном порядке.</span><span class="sxs-lookup"><span data-stu-id="eb7af-265">This process works much like publishing, except in reverse.</span></span>
> 
> <span data-ttu-id="eb7af-266">Мы не описываются более о удаленное редактирование и удаленной загрузки средств WebMatrix здесь.</span><span class="sxs-lookup"><span data-stu-id="eb7af-266">We won't describe more about the remote-editing and remote-download facilities of WebMatrix here.</span></span> <span data-ttu-id="eb7af-267">Они весьма полезно в том случае, если несколько пользователей должны работать на том же узле, на разных компьютерах.</span><span class="sxs-lookup"><span data-stu-id="eb7af-267">They're quite useful if multiple people have to work on the same site on different computers.</span></span> <span data-ttu-id="eb7af-268">Дополнительные сведения см. в разделе [публикации и изменять удаленный сайт в WebMatrix 2 бета-версии](https://go.microsoft.com/fwlink/?LinkId=251591).</span><span class="sxs-lookup"><span data-stu-id="eb7af-268">For more information, see [Publish and Edit a Remote Site with WebMatrix 2 Beta](https://go.microsoft.com/fwlink/?LinkId=251591).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="eb7af-269">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="eb7af-269">Additional Resources</span></span>

- <span data-ttu-id="eb7af-270">[Форум по ASP.NET WebMatrix ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages), отлично подходят для учета вопросы и ответы.</span><span class="sxs-lookup"><span data-stu-id="eb7af-270">[ASP.NET WebMatrix ASP.NET Web Pages forum](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages), a great place to post questions and get answers.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="eb7af-271">Назад</span><span class="sxs-lookup"><span data-stu-id="eb7af-271">Previous</span></span>](layouts.md)
