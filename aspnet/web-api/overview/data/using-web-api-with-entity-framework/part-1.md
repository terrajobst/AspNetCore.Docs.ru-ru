---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Использование веб-API 2 с Entity Framework 6 | Документы Microsoft
author: MikeWasson
description: Этот учебник поможет узнать, что основные принципы создания веб-приложения с ASP.NET Web API серверной части. В этом учебнике используется Entity Framework 6 для макета данных...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 8e6d381509a121e3036ca3af91ea3b9bd0be33c2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871978"
---
<a name="using-web-api-2-with-entity-framework-6"></a><span data-ttu-id="a6589-104">Использование веб-API 2 с Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="a6589-104">Using Web API 2 with Entity Framework 6</span></span>
====================
<span data-ttu-id="a6589-105">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a6589-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="a6589-106">Загрузка завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="a6589-106">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

> <span data-ttu-id="a6589-107">Этот учебник поможет узнать, что основные принципы создания веб-приложения с ASP.NET Web API серверной части.</span><span class="sxs-lookup"><span data-stu-id="a6589-107">This tutorial will teach you the basics of creating a web application with an ASP.NET Web API back end.</span></span> <span data-ttu-id="a6589-108">Учебника используется Entity Framework 6 уровень данных, а Knockout.js для клиентского приложения JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a6589-108">The tutorial uses Entity Framework 6 for the data layer, and Knockout.js for the client-side JavaScript application.</span></span> <span data-ttu-id="a6589-109">Учебник также показано, как развернуть приложение на веб-приложениях службы приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="a6589-109">The tutorial also shows how to deploy the app to Azure App Service Web Apps.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a6589-110">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="a6589-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="a6589-111">Веб-API 2.1</span><span class="sxs-lookup"><span data-stu-id="a6589-111">Web API 2.1</span></span>
> - [<span data-ttu-id="a6589-112">Visual Studio 2013 с обновлением 2</span><span class="sxs-lookup"><span data-stu-id="a6589-112">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="a6589-113">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="a6589-113">Entity Framework 6</span></span>
> - <span data-ttu-id="a6589-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="a6589-114">.NET 4.5</span></span>
> - <span data-ttu-id="a6589-115">[Knockout.js](http://knockoutjs.com/) 3.1</span><span class="sxs-lookup"><span data-stu-id="a6589-115">[Knockout.js](http://knockoutjs.com/) 3.1</span></span>


<span data-ttu-id="a6589-116">Этот учебник использует ASP.NET Web API 2 с Entity Framework 6 для создания веб-приложения, который управляет серверную базу данных.</span><span class="sxs-lookup"><span data-stu-id="a6589-116">This tutorial uses ASP.NET Web API 2 with Entity Framework 6 to create a web application that manipulates a back-end database.</span></span> <span data-ttu-id="a6589-117">Ниже приведен снимок экрана приложения, который будет создан.</span><span class="sxs-lookup"><span data-stu-id="a6589-117">Here is a screen shot of the application that you will create.</span></span>

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

<span data-ttu-id="a6589-118">Приложение использует конструктор одностраничного приложения (SPA).</span><span class="sxs-lookup"><span data-stu-id="a6589-118">The app uses a single-page application (SPA) design.</span></span> <span data-ttu-id="a6589-119">«Одностраничного приложения» — это общий термин для веб-приложения, которое загружает одной HTML-страницы и затем обновляет страницу динамически, вместо загрузки новой страницы.</span><span class="sxs-lookup"><span data-stu-id="a6589-119">"Single-page application" is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="a6589-120">После загрузки начальной страницы приложение обращается к серверу через запросы AJAX.</span><span class="sxs-lookup"><span data-stu-id="a6589-120">After the initial page load, the app talks with the server through AJAX requests.</span></span> <span data-ttu-id="a6589-121">AJAX запрашивает возвращаемых данных JSON, которые приложение использует для обновления пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="a6589-121">The AJAX requests return JSON data, which the app uses to update the UI.</span></span>

<span data-ttu-id="a6589-122">AJAX не новый, но на сегодняшний день существует платформ JavaScript, которые упрощают создание и обслуживание больших сложных приложений SPA.</span><span class="sxs-lookup"><span data-stu-id="a6589-122">AJAX isn't new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="a6589-123">В этом учебнике используется [Knockout.js](http://knockoutjs.com/), но можно использовать любую платформу клиента JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a6589-123">This tutorial uses [Knockout.js](http://knockoutjs.com/), but you can use any JavaScript client framework.</span></span>

<span data-ttu-id="a6589-124">Ниже приведены основные строительные блоки для этого приложения.</span><span class="sxs-lookup"><span data-stu-id="a6589-124">Here are the main building blocks for this app:</span></span>

- <span data-ttu-id="a6589-125">ASP.NET MVC создает HTML-страницы.</span><span class="sxs-lookup"><span data-stu-id="a6589-125">ASP.NET MVC creates the HTML page.</span></span>
- <span data-ttu-id="a6589-126">Веб-API ASP.NET обрабатывает запросы AJAX и возвращает данные JSON.</span><span class="sxs-lookup"><span data-stu-id="a6589-126">ASP.NET Web API handles the AJAX requests and returns JSON data.</span></span>
- <span data-ttu-id="a6589-127">Knockout.js привязывает данные HTML-элементов для данных JSON.</span><span class="sxs-lookup"><span data-stu-id="a6589-127">Knockout.js data-binds the HTML elements to the JSON data.</span></span>
- <span data-ttu-id="a6589-128">Платформа Entity Framework обращается к базе данных.</span><span class="sxs-lookup"><span data-stu-id="a6589-128">Entity Framework talks to the database.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="a6589-129">Это приложение, работающее на платформе Azure см.</span><span class="sxs-lookup"><span data-stu-id="a6589-129">See this App Running on Azure</span></span>

<span data-ttu-id="a6589-130">Вы действительно хотите см. по завершении узел, работающий как динамические веб-приложения?</span><span class="sxs-lookup"><span data-stu-id="a6589-130">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="a6589-131">Можно развернуть полную версию приложения для учетной записи Azure, просто нажав кнопку ниже.</span><span class="sxs-lookup"><span data-stu-id="a6589-131">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

<span data-ttu-id="a6589-132">Требуется учетная запись Azure для развертывания этого решения в Azure.</span><span class="sxs-lookup"><span data-stu-id="a6589-132">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="a6589-133">Если у вас еще нет учетной записи, у вас есть следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="a6589-133">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="a6589-134">[Открыть учетную запись Azure бесплатно](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -вы получаете кредиты можно использовать, чтобы испытать оплаты служб Azure и даже после того, они используются до можно хранить учетной записи, и используйте освободить служб Azure.</span><span class="sxs-lookup"><span data-stu-id="a6589-134">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="a6589-135">[Активировать преимущества для подписчиков MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -подписка Your MSDN дает кредиты каждого месяца, можно использовать для оплаты служб Azure.</span><span class="sxs-lookup"><span data-stu-id="a6589-135">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="a6589-136">Создание проекта</span><span class="sxs-lookup"><span data-stu-id="a6589-136">Create the Project</span></span>

<span data-ttu-id="a6589-137">Запустите Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a6589-137">Open Visual Studio.</span></span> <span data-ttu-id="a6589-138">Из **файл** последовательно выберите пункты **New**, а затем выберите **проекта**.</span><span class="sxs-lookup"><span data-stu-id="a6589-138">From the **File** menu, select **New**, then select **Project**.</span></span> <span data-ttu-id="a6589-139">(Или нажмите кнопку **новый проект** на начальной странице.)</span><span class="sxs-lookup"><span data-stu-id="a6589-139">(Or click **New Project** on the Start page.)</span></span>

<span data-ttu-id="a6589-140">В **новый проект** диалоговое окно, нажмите кнопку **Web** в левой области и **веб-приложение ASP.NET** в средней области.</span><span class="sxs-lookup"><span data-stu-id="a6589-140">In the **New Project** dialog, click **Web** in the left pane and **ASP.NET Web Application** in the middle pane.</span></span> <span data-ttu-id="a6589-141">Назовите проект BookService и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="a6589-141">Name the project BookService and click **OK**.</span></span>

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

<span data-ttu-id="a6589-142">В **новый проект ASP.NET** диалогового окна выберите **веб-API** шаблона.</span><span class="sxs-lookup"><span data-stu-id="a6589-142">In the **New ASP.NET Project** dialog, select the **Web API** template.</span></span>

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

<span data-ttu-id="a6589-143">Для размещения проекта в службе приложений Azure, оставьте **узел в облаке** флажок установлен.</span><span class="sxs-lookup"><span data-stu-id="a6589-143">If you want to host the project in a Azure App Service, leave the **Host in the cloud** box checked.</span></span>

<span data-ttu-id="a6589-144">Нажмите кнопку **ОК**, чтобы создать проект.</span><span class="sxs-lookup"><span data-stu-id="a6589-144">Click **OK** to create the project.</span></span>

## <a name="configure-azure-settings-optional"></a><span data-ttu-id="a6589-145">Настройте параметры Azure (необязательно)</span><span class="sxs-lookup"><span data-stu-id="a6589-145">Configure Azure Settings (Optional)</span></span>

<span data-ttu-id="a6589-146">Если оставить **узлов в облаке** флажок установлен, Visual Studio будет предложено войти в Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="a6589-146">If you left the **Host in Cloud** option checked, Visual Studio will prompt you to sign in to Microsoft Azure</span></span>

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

<span data-ttu-id="a6589-147">После входа в Azure, Visual Studio предложит настроить веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="a6589-147">After you sign in to Azure, Visual Studio prompts you to configure the web app.</span></span> <span data-ttu-id="a6589-148">Введите имя для сайта, выберите подписки Azure и выберите географический регион.</span><span class="sxs-lookup"><span data-stu-id="a6589-148">Enter a name for the site, select your Azure subscription, and select a geographical region.</span></span> <span data-ttu-id="a6589-149">В разделе **сервера базы данных**выберите **создать новый сервер**.</span><span class="sxs-lookup"><span data-stu-id="a6589-149">Under **Database server**, select **Create new server**.</span></span> <span data-ttu-id="a6589-150">Введите имя пользователя администратора и пароль.</span><span class="sxs-lookup"><span data-stu-id="a6589-150">Enter an administrator username and password.</span></span>

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="a6589-151">Вперед</span><span class="sxs-lookup"><span data-stu-id="a6589-151">Next</span></span>](part-2.md)
