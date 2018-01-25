---
uid: mvc/overview/getting-started/introduction/getting-started
title: "Приступая к работе с ASP.NET MVC 5 | Документы Microsoft"
author: Rick-Anderson
description: "Примечание: Обновленную версию этого учебника доступен с помощью Visual Studio 2015. Новое в этом учебнике используется ASP.NET Core MVC 6, которая предоставляет много improvem..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 1616b238612fa9f519418f583c40a46b9d81d8ce
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-aspnet-mvc-5"></a><span data-ttu-id="507bc-104">Начало работы с ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="507bc-104">Getting Started with ASP.NET MVC 5</span></span>
====================
<span data-ttu-id="507bc-105">По [Рик Андерсон](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="507bc-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[consider RP](../../../../includes/razor.md)]

 
 <span data-ttu-id="507bc-106">Этот учебник поможет узнать основы создания приложений веб-ASP.NET MVC 5 с помощью [2017 г. Visual Studio](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="507bc-106">This tutorial will teach you the basics of building an ASP.NET MVC 5 web app using [Visual Studio 2017](https://www.visualstudio.com/).</span></span> <span data-ttu-id="507bc-107">Окончательный источника для учебника, расположенных на [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)</span><span class="sxs-lookup"><span data-stu-id="507bc-107">Final Source for tutorial located on [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)</span></span>
 
 
 <span data-ttu-id="507bc-108">Это руководство было написано с [Скотт Гатри](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [Скотт Хансельман](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) ) , и [Рик Андерсон](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )</span><span class="sxs-lookup"><span data-stu-id="507bc-108">This tutorial was written by [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[@scottgu](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](https://twitter.com/shanselman) ), and [Rick Anderson](https://twitter.com/RickAndMSFT) ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) )</span></span>
 
 <span data-ttu-id="507bc-109">Требуется учетная запись Azure для развертывания этого приложения в Azure:</span><span class="sxs-lookup"><span data-stu-id="507bc-109">You need an Azure account to deploy this app to Azure:</span></span>
 
 - <span data-ttu-id="507bc-110">Вы можете [открыть учетную запись Azure бесплатно](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -вы получаете кредиты можно использовать, чтобы испытать оплаты служб Azure и даже после того, они используются до можно хранить учетной записи, и используйте освободить служб Azure.</span><span class="sxs-lookup"><span data-stu-id="507bc-110">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
 - <span data-ttu-id="507bc-111">Вы можете [активировать преимущества для подписчиков MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -подписка Your MSDN дает кредиты каждого месяца, можно использовать для оплаты служб Azure.</span><span class="sxs-lookup"><span data-stu-id="507bc-111">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>


## <a name="getting-started"></a><span data-ttu-id="507bc-112">Начало работы</span><span class="sxs-lookup"><span data-stu-id="507bc-112">Getting Started</span></span>

<span data-ttu-id="507bc-113">Начните с установки и запуска [2017 г. Visual Studio](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="507bc-113">Start by installing and running [Visual Studio 2017](https://www.visualstudio.com/).</span></span>

<span data-ttu-id="507bc-114">Visual Studio — это интегрированная среда разработки, или интегрированной среды разработки.</span><span class="sxs-lookup"><span data-stu-id="507bc-114">Visual Studio is an IDE, or integrated development environment.</span></span> <span data-ttu-id="507bc-115">Так же, как используется Microsoft Word для записи документов, воспользуемся интегрированной среды разработки для создания приложений.</span><span class="sxs-lookup"><span data-stu-id="507bc-115">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="507bc-116">В Visual Studio имеется список вдоль нижней отображаются различные параметры, доступные для вас.</span><span class="sxs-lookup"><span data-stu-id="507bc-116">In Visual Studio there's a list along the bottom showing various options available to you.</span></span> <span data-ttu-id="507bc-117">Кроме того, имеется меню, которое предоставляет еще один способ выполнения задач в Интегрированной среде разработки.</span><span class="sxs-lookup"><span data-stu-id="507bc-117">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="507bc-118">(Например, вместо выбора **новый проект** из **запустить** страницы, можно использовать меню и выбрать **файл** &gt; **новый проект**.)</span><span class="sxs-lookup"><span data-stu-id="507bc-118">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>

   
![](getting-started/_static/image1.png)  
 

## <a name="creating-your-first-application"></a><span data-ttu-id="507bc-119">Создание первого приложения</span><span class="sxs-lookup"><span data-stu-id="507bc-119">Creating Your First Application</span></span>

<span data-ttu-id="507bc-120">Нажмите кнопку **новый проект**, выберите Visual C# слева, затем **Web** , а затем выберите **веб-приложения ASP.NET (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="507bc-120">Click **New Project**, then select Visual C# on the left, then **Web** and then select **ASP.NET Web Application (.NET Framework)**.</span></span> <span data-ttu-id="507bc-121">Назовите проект «MvcMovie» и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="507bc-121">Name your project "MvcMovie" and then click **OK**.</span></span>

![](getting-started/_static/image2.png)

<span data-ttu-id="507bc-122">В **новый проект ASP.NET** диалоговое окно, нажмите кнопку **MVC** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="507bc-122">In the **New ASP.NET Project** dialog, click **MVC** and then click **OK**.</span></span>

![](getting-started/_static/image3.png)

<span data-ttu-id="507bc-123">Visual Studio используется шаблон по умолчанию для проекта ASP.NET MVC, который вы только что создали, поэтому в данный момент есть рабочее приложение, не выполняя никаких действий!</span><span class="sxs-lookup"><span data-stu-id="507bc-123">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="507bc-124">Это простое «Hello World!»</span><span class="sxs-lookup"><span data-stu-id="507bc-124">This is a simple "Hello World!"</span></span> <span data-ttu-id="507bc-125">проекта, а вот хорошая можно запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="507bc-125">project, and it's a good place to start your application.</span></span>

![](getting-started/_static/image4.png)

<span data-ttu-id="507bc-126">Нажмите F5 для запуска отладки.</span><span class="sxs-lookup"><span data-stu-id="507bc-126">Click F5 to start debugging.</span></span> <span data-ttu-id="507bc-127">F5 приводит Visual Studio для запуска [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) и запуска веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="507bc-127">F5 causes Visual Studio to start [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) and run your web app.</span></span> <span data-ttu-id="507bc-128">Затем Visual Studio запускает браузер и открывает домашнюю страницу приложения.</span><span class="sxs-lookup"><span data-stu-id="507bc-128">Visual Studio then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="507bc-129">Обратите внимание, что в адресной строке браузера говорит `localhost:port#` , а не то, как `example.com`.</span><span class="sxs-lookup"><span data-stu-id="507bc-129">Notice that the address bar of the browser says `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="507bc-130">Причина этого заключается в `localhost` всегда указывает на локальном компьютере, работающая в этом случае просто создано приложение.</span><span class="sxs-lookup"><span data-stu-id="507bc-130">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="507bc-131">При запуске веб-проекта Visual Studio для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="507bc-131">When Visual Studio runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="507bc-132">На рисунке ниже номер порта — 1234.</span><span class="sxs-lookup"><span data-stu-id="507bc-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="507bc-133">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="507bc-133">When you run the application, you'll see a different port number.</span></span>

![](getting-started/_static/image5.png)

<span data-ttu-id="507bc-134">Сразу после распаковки этот шаблон по умолчанию дает дома, контактов и о страницы.</span><span class="sxs-lookup"><span data-stu-id="507bc-134">Right out of the box this default template gives you Home, Contact and About pages.</span></span> <span data-ttu-id="507bc-135">На рисунке выше не показывает **Главная**, **о** и **контакт** ссылки.</span><span class="sxs-lookup"><span data-stu-id="507bc-135">The image above doesn't show the **Home**, **About** and **Contact** links.</span></span> <span data-ttu-id="507bc-136">В зависимости от размера окна браузера может потребоваться щелкните значок навигации, чтобы найти по следующим ссылкам.</span><span class="sxs-lookup"><span data-stu-id="507bc-136">Depending on the size of your browser window, you might need to click the navigation icon to see these links.</span></span>

![](getting-started/_static/image6.png)  

<span data-ttu-id="507bc-137">Приложение также поддерживает для регистрации и входа.</span><span class="sxs-lookup"><span data-stu-id="507bc-137">The application also provides support to register and log in.</span></span> <span data-ttu-id="507bc-138">Следующий шаг — изменить работу этого приложения и немного узнать о ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="507bc-138">The next step is to change how this application works and learn a little bit about ASP.NET MVC.</span></span> <span data-ttu-id="507bc-139">Закройте приложение ASP.NET MVC и изменим некоторый код.</span><span class="sxs-lookup"><span data-stu-id="507bc-139">Close the ASP.NET MVC application and let's change some code.</span></span>

<span data-ttu-id="507bc-140">Список текущего учебники см. в разделе [MVC рекомендуется статьи](../mvc-learning-sequence.md).</span><span class="sxs-lookup"><span data-stu-id="507bc-140">For a list of current tutorials, see [MVC recommended articles](../mvc-learning-sequence.md).</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="507bc-141">Это приложение, работающее на платформе Azure см.</span><span class="sxs-lookup"><span data-stu-id="507bc-141">See this App Running on Azure</span></span>

<span data-ttu-id="507bc-142">Вы действительно хотите см. по завершении узел, работающий как динамические веб-приложения?</span><span class="sxs-lookup"><span data-stu-id="507bc-142">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="507bc-143">Можно развернуть полную версию приложения для учетной записи Azure, просто нажав кнопку ниже.</span><span class="sxs-lookup"><span data-stu-id="507bc-143">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

<span data-ttu-id="507bc-144">Требуется учетная запись Azure для развертывания этого решения в Azure.</span><span class="sxs-lookup"><span data-stu-id="507bc-144">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="507bc-145">Если у вас еще нет учетной записи, у вас есть следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="507bc-145">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="507bc-146">[Открыть учетную запись Azure бесплатно](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -вы получаете кредиты можно использовать, чтобы испытать оплаты служб Azure и даже после того, они используются до можно хранить учетной записи, и используйте освободить служб Azure.</span><span class="sxs-lookup"><span data-stu-id="507bc-146">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="507bc-147">[Активировать преимущества для подписчиков MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -подписка Your MSDN дает кредиты каждого месяца, можно использовать для оплаты служб Azure.</span><span class="sxs-lookup"><span data-stu-id="507bc-147">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="507bc-148">Вперед</span><span class="sxs-lookup"><span data-stu-id="507bc-148">Next</span></span>](adding-a-controller.md)
