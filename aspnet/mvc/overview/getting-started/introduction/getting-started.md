---
uid: mvc/overview/getting-started/introduction/getting-started
title: Приступая к работе с ASP.NET MVC 5 | Документация Майкрософт
author: Rick-Anderson
description: 'Примечание: Обновленную версию этого учебника доступен здесь с помощью Visual Studio 2015. Новое учебное использует ASP.NET Core MVC 6, которая предоставляет новых преимуществах...'
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 85d5d3292ff99ade6995c710e2728c41255def4c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823220"
---
<a name="getting-started-with-aspnet-mvc-5"></a><span data-ttu-id="b9fd6-104">Начало работы с ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="b9fd6-104">Getting Started with ASP.NET MVC 5</span></span>
====================
<span data-ttu-id="b9fd6-105">по [Рик Андерсон](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="b9fd6-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [consider RP](../../../../includes/razor.md)]

 <span data-ttu-id="b9fd6-106">Этот учебник поможет основы создания приложения web ASP.NET MVC 5 с помощью [Visual Studio 2017](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="b9fd6-106">This tutorial will teach you the basics of building an ASP.NET MVC 5 web app using [Visual Studio 2017](https://www.visualstudio.com/).</span></span> <span data-ttu-id="b9fd6-107">Окончательный источника для работы с руководством, расположенных на [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)</span><span class="sxs-lookup"><span data-stu-id="b9fd6-107">Final Source for tutorial located on [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)</span></span>


 <span data-ttu-id="b9fd6-108">Это руководство было написано с [Скотт Гатри](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [(Scott hanselman)](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) ) , и [Рик Андерсон](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )</span><span class="sxs-lookup"><span data-stu-id="b9fd6-108">This tutorial was written by [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[@scottgu](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](https://twitter.com/shanselman) ), and [Rick Anderson](https://twitter.com/RickAndMSFT) ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) )</span></span>

 <span data-ttu-id="b9fd6-109">Вам потребуется учетная запись Azure, чтобы развернуть это приложение в Azure:</span><span class="sxs-lookup"><span data-stu-id="b9fd6-109">You need an Azure account to deploy this app to Azure:</span></span>

 - <span data-ttu-id="b9fd6-110">Вы можете [открыть учетную запись Azure бесплатно](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -вы получаете кредиты можно использовать для опробования платных служб Azure и даже в том случае, если они используются, вы сохраняете учетную запись и использовать бесплатные службы Azure.</span><span class="sxs-lookup"><span data-stu-id="b9fd6-110">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
 - <span data-ttu-id="b9fd6-111">Вы можете [активировать преимущества подписчика MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -ваша подписка MSDN предоставляет вам кредиты каждый месяц, который можно использовать для оплаты служб Azure.</span><span class="sxs-lookup"><span data-stu-id="b9fd6-111">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>


## <a name="getting-started"></a><span data-ttu-id="b9fd6-112">Начало работы</span><span class="sxs-lookup"><span data-stu-id="b9fd6-112">Getting Started</span></span>

<span data-ttu-id="b9fd6-113">Начните с установки и запуска [Visual Studio 2017](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="b9fd6-113">Start by installing and running [Visual Studio 2017](https://www.visualstudio.com/).</span></span>

<span data-ttu-id="b9fd6-114">Visual Studio является интегрированной среды разработки или интегрированной среды разработки.</span><span class="sxs-lookup"><span data-stu-id="b9fd6-114">Visual Studio is an IDE, or integrated development environment.</span></span> <span data-ttu-id="b9fd6-115">Так же, как использовать Microsoft Word для записи документов, вы используете интегрированную среду разработки для создания приложений.</span><span class="sxs-lookup"><span data-stu-id="b9fd6-115">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="b9fd6-116">В Visual Studio имеется список внизу отображаются различные параметры, доступные для вас.</span><span class="sxs-lookup"><span data-stu-id="b9fd6-116">In Visual Studio there's a list along the bottom showing various options available to you.</span></span> <span data-ttu-id="b9fd6-117">Имеется также меню, которое предоставляет еще один способ выполнения задач в интегрированной среде разработки.</span><span class="sxs-lookup"><span data-stu-id="b9fd6-117">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="b9fd6-118">(Например, вместо выбора **новый проект** из **запустить** страницы, можно использовать меню и выберите **файл** &gt; **новый проект**.)</span><span class="sxs-lookup"><span data-stu-id="b9fd6-118">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>


![](getting-started/_static/image1.png)  


## <a name="creating-your-first-application"></a><span data-ttu-id="b9fd6-119">Создание первого приложения</span><span class="sxs-lookup"><span data-stu-id="b9fd6-119">Creating Your First Application</span></span>

<span data-ttu-id="b9fd6-120">Нажмите кнопку **новый проект**, затем выберите Visual C# в левой части, а затем **Web** , а затем выберите **веб-приложение ASP.NET (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="b9fd6-120">Click **New Project**, then select Visual C# on the left, then **Web** and then select **ASP.NET Web Application (.NET Framework)**.</span></span> <span data-ttu-id="b9fd6-121">Назовите проект «MvcMovie» и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="b9fd6-121">Name your project "MvcMovie" and then click **OK**.</span></span>

![](getting-started/_static/image2.png)

<span data-ttu-id="b9fd6-122">В **новый проект ASP.NET** диалоговое окно, нажмите кнопку **MVC** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="b9fd6-122">In the **New ASP.NET Project** dialog, click **MVC** and then click **OK**.</span></span>

![](getting-started/_static/image3.png)

<span data-ttu-id="b9fd6-123">Visual Studio используется шаблон по умолчанию для проекта ASP.NET MVC, который вы только что создали, поэтому у вас есть рабочее приложение прямо сейчас не выполняя никаких действий!</span><span class="sxs-lookup"><span data-stu-id="b9fd6-123">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="b9fd6-124">Это связано с простых «Hello World!»</span><span class="sxs-lookup"><span data-stu-id="b9fd6-124">This is a simple "Hello World!"</span></span> <span data-ttu-id="b9fd6-125">проекта, а вот отлично подходит для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="b9fd6-125">project, and it's a good place to start your application.</span></span>

![](getting-started/_static/image4.png)

<span data-ttu-id="b9fd6-126">Нажмите F5, чтобы начать отладку.</span><span class="sxs-lookup"><span data-stu-id="b9fd6-126">Click F5 to start debugging.</span></span> <span data-ttu-id="b9fd6-127">F5 приводит к Visual Studio запускать [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) и запуска веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="b9fd6-127">F5 causes Visual Studio to start [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) and run your web app.</span></span> <span data-ttu-id="b9fd6-128">Затем Visual Studio запустит браузер и откроется домашняя страница приложения.</span><span class="sxs-lookup"><span data-stu-id="b9fd6-128">Visual Studio then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="b9fd6-129">Обратите внимание, что говорит в адресной строке браузера `localhost:port#` и не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="b9fd6-129">Notice that the address bar of the browser says `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="b9fd6-130">Это потому, что `localhost` всегда указывает на локальном компьютере, который выполняется, в этом случае приложение, вы только что выполнили.</span><span class="sxs-lookup"><span data-stu-id="b9fd6-130">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="b9fd6-131">При запуске веб-проекта Visual Studio для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="b9fd6-131">When Visual Studio runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="b9fd6-132">На рисунке ниже номер порта — 1234.</span><span class="sxs-lookup"><span data-stu-id="b9fd6-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="b9fd6-133">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="b9fd6-133">When you run the application, you'll see a different port number.</span></span>

![](getting-started/_static/image5.png)

<span data-ttu-id="b9fd6-134">Готовые этот шаблон по умолчанию позволяет дома, контактов и о страницы.</span><span class="sxs-lookup"><span data-stu-id="b9fd6-134">Right out of the box this default template gives you Home, Contact and About pages.</span></span> <span data-ttu-id="b9fd6-135">На рисунке выше не показывает **Главная**, **о** и **контакт** ссылки.</span><span class="sxs-lookup"><span data-stu-id="b9fd6-135">The image above doesn't show the **Home**, **About** and **Contact** links.</span></span> <span data-ttu-id="b9fd6-136">В зависимости от размера окна браузера может потребоваться щелкнуть значок навигации, чтобы найти по следующим ссылкам.</span><span class="sxs-lookup"><span data-stu-id="b9fd6-136">Depending on the size of your browser window, you might need to click the navigation icon to see these links.</span></span>

![](getting-started/_static/image6.png)  

<span data-ttu-id="b9fd6-137">Приложение также предоставляет поддержку для регистрации и входа.</span><span class="sxs-lookup"><span data-stu-id="b9fd6-137">The application also provides support to register and log in.</span></span> <span data-ttu-id="b9fd6-138">Следующий шаг — изменить работу этого приложения и немного поговорим об ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b9fd6-138">The next step is to change how this application works and learn a little bit about ASP.NET MVC.</span></span> <span data-ttu-id="b9fd6-139">Закройте приложение ASP.NET MVC и изменим код.</span><span class="sxs-lookup"><span data-stu-id="b9fd6-139">Close the ASP.NET MVC application and let's change some code.</span></span>

<span data-ttu-id="b9fd6-140">Список действующие руководства, см. в разделе [MVC рекомендуется статьи](../mvc-learning-sequence.md).</span><span class="sxs-lookup"><span data-stu-id="b9fd6-140">For a list of current tutorials, see [MVC recommended articles](../mvc-learning-sequence.md).</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="b9fd6-141">Это приложение работает в Azure см. в разделе</span><span class="sxs-lookup"><span data-stu-id="b9fd6-141">See this App Running on Azure</span></span>

<span data-ttu-id="b9fd6-142">Вы действительно хотите см. по завершении сайт, запущенный в качестве активного веб-приложения?</span><span class="sxs-lookup"><span data-stu-id="b9fd6-142">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="b9fd6-143">Полную версию приложения можно развернуть учетную запись Azure, просто нажав кнопку ниже.</span><span class="sxs-lookup"><span data-stu-id="b9fd6-143">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

<span data-ttu-id="b9fd6-144">Требуется учетная запись Azure для развертывания этого решения в Azure.</span><span class="sxs-lookup"><span data-stu-id="b9fd6-144">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="b9fd6-145">Если у вас еще нет учетной записи, у вас есть следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="b9fd6-145">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="b9fd6-146">[Открыть учетную запись Azure бесплатно](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -вы получаете кредиты можно использовать для опробования платных служб Azure и даже в том случае, если они используются, вы сохраняете учетную запись и использовать бесплатные службы Azure.</span><span class="sxs-lookup"><span data-stu-id="b9fd6-146">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="b9fd6-147">[Активировать преимущества подписчика MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -ваша подписка MSDN предоставляет вам кредиты каждый месяц, который можно использовать для оплаты служб Azure.</span><span class="sxs-lookup"><span data-stu-id="b9fd6-147">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b9fd6-148">Вперед</span><span class="sxs-lookup"><span data-stu-id="b9fd6-148">Next</span></span>](adding-a-controller.md)
