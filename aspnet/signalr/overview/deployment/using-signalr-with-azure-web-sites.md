---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: "Использование SignalR с веб-приложений в службе приложений Azure | Документы Microsoft"
author: pfletcher
description: "В этом документе описываются способы настройки приложения SignalR, которое выполняется в Microsoft Azure. Версии программного обеспечения используется в учебнике по Visual Studio 2013 или Vis...."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/01/2015
ms.topic: article
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 414701159b4e1fa3da9597503b14281a1e9991de
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a><span data-ttu-id="2a9ab-104">Использование SignalR с веб-приложений в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="2a9ab-104">Using SignalR with Web Apps in Azure App Service</span></span>
====================
<span data-ttu-id="2a9ab-105">по [Патрик Флетчера](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="2a9ab-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="2a9ab-106">В этом документе описываются способы настройки приложения SignalR, которое выполняется в Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="2a9ab-106">This document describes how to configure a SignalR application that runs on Microsoft Azure.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2a9ab-107">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="2a9ab-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="2a9ab-108">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) или Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="2a9ab-108">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) or Visual Studio 2012</span></span>
> - <span data-ttu-id="2a9ab-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="2a9ab-109">.NET 4.5</span></span>
> - <span data-ttu-id="2a9ab-110">SignalR версии 2</span><span class="sxs-lookup"><span data-stu-id="2a9ab-110">SignalR version 2</span></span>
> - <span data-ttu-id="2a9ab-111">2.3 пакета Azure SDK для Visual Studio 2013 или 2012</span><span class="sxs-lookup"><span data-stu-id="2a9ab-111">Azure SDK 2.3 for Visual Studio 2013 or 2012</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="2a9ab-112">Вопросы и комментарии</span><span class="sxs-lookup"><span data-stu-id="2a9ab-112">Questions and comments</span></span>
> 
> <span data-ttu-id="2a9ab-113">Оставьте отзыв на том, как вам понравилось этого учебника и что можно улучшить в комментарии в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="2a9ab-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="2a9ab-114">Если у вас есть вопросы, которые не связаны непосредственно для работы с учебником, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), или [форумы Microsoft Azure](https://social.msdn.microsoft.com/Forums/windowsazure/en-US/home?category=windowsazureplatform).</span><span class="sxs-lookup"><span data-stu-id="2a9ab-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), or the [Microsoft Azure forums](https://social.msdn.microsoft.com/Forums/windowsazure/en-US/home?category=windowsazureplatform).</span></span>


## <a name="table-of-contents"></a><span data-ttu-id="2a9ab-115">Содержание</span><span class="sxs-lookup"><span data-stu-id="2a9ab-115">Table of Contents</span></span>

- [<span data-ttu-id="2a9ab-116">Введение</span><span class="sxs-lookup"><span data-stu-id="2a9ab-116">Introduction</span></span>](#introduction)
- [<span data-ttu-id="2a9ab-117">Развертывание SignalR веб-приложения в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="2a9ab-117">Deploying a SignalR Web App to Azure App Service</span></span>](#deploying)
- [<span data-ttu-id="2a9ab-118">Включение соединений WebSocket на службы приложений Azure</span><span class="sxs-lookup"><span data-stu-id="2a9ab-118">Enabling WebSockets on Azure App Service</span></span>](#websocket)
- [<span data-ttu-id="2a9ab-119">Использование объединительной платы кэша Redis для Azure</span><span class="sxs-lookup"><span data-stu-id="2a9ab-119">Using the Azure Redis Cache Backplane</span></span>](#backplane)
- [<span data-ttu-id="2a9ab-120">Дальнейшие действия</span><span class="sxs-lookup"><span data-stu-id="2a9ab-120">Next Steps</span></span>](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a><span data-ttu-id="2a9ab-121">Вступление</span><span class="sxs-lookup"><span data-stu-id="2a9ab-121">Introduction</span></span>

<span data-ttu-id="2a9ab-122">ASP.NET SignalR можно использовать для переноса на новый уровень взаимодействия между серверами и веб- или клиентов .NET.</span><span class="sxs-lookup"><span data-stu-id="2a9ab-122">ASP.NET SignalR can be used to bring a new level of interactivity between servers and web or .NET clients.</span></span> <span data-ttu-id="2a9ab-123">При размещении в Azure приложений SignalR можно воспользоваться преимуществами высокой надежности, масштабируемых и высокопроизводительных среда, в облаке предоставляет.</span><span class="sxs-lookup"><span data-stu-id="2a9ab-123">When hosted in Azure, SignalR applications can take advantage of the highly available, scalable, and performant environment that running in the cloud provides.</span></span>

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a><span data-ttu-id="2a9ab-124">Развертывание SignalR веб-приложения в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="2a9ab-124">Deploying a SignalR Web App to Azure App Service</span></span>

<span data-ttu-id="2a9ab-125">SignalR не добавляет всяких осложнений, определенного для развертывания приложения в Azure и развертывание на локальном сервере.</span><span class="sxs-lookup"><span data-stu-id="2a9ab-125">SignalR doesn't add any particular complications to deploying an application to Azure versus deploying to an on-premises server.</span></span> <span data-ttu-id="2a9ab-126">Приложение, использующее SignalR может размещаться в Azure без изменений в конфигурации и других параметров (то, что поддержка WebSockets. в разделе [Включение соединений WebSocket на службе приложений Azure](#websocket) ниже.) В этом учебнике вы развернете приложение, созданное в [учебник по началу работы](../getting-started/tutorial-getting-started-with-signalr.md) в Azure.</span><span class="sxs-lookup"><span data-stu-id="2a9ab-126">An application that uses SignalR can be hosted in Azure without any changes in configuration or other settings (though for WebSockets support, see [Enabling WebSockets on Azure App Service](#websocket) below.) For this tutorial, you'll deploy the application created in the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md) to Azure.</span></span>

<span data-ttu-id="2a9ab-127">**Необходимые компоненты**</span><span class="sxs-lookup"><span data-stu-id="2a9ab-127">**Prerequisites**</span></span>

- <span data-ttu-id="2a9ab-128">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="2a9ab-128">Visual Studio 2013.</span></span> <span data-ttu-id="2a9ab-129">Если у вас нет Visual Studio, Visual Studio 2013 Express для Web включается в установку пакета Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="2a9ab-129">If you don't have Visual Studio, Visual Studio 2013 Express for Web is included in the install of the Azure SDK.</span></span>
- <span data-ttu-id="2a9ab-130">[2.3 пакета Azure SDK для Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) или [2.3 пакета Azure SDK для Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span><span class="sxs-lookup"><span data-stu-id="2a9ab-130">[Azure SDK 2.3 for Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) or [Azure SDK 2.3 for Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span></span>
- <span data-ttu-id="2a9ab-131">Для выполнения заданий данного учебника, вам потребуется подписка Azure.</span><span class="sxs-lookup"><span data-stu-id="2a9ab-131">To complete this tutorial, you will need an Azure subscription.</span></span> <span data-ttu-id="2a9ab-132">Вы можете [активировать преимущества для подписчиков MSDN](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/), или [зарегистрироваться для пробной подписки](https://azure.microsoft.com/en-us/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2a9ab-132">You can [activate your MSDN subscriber benefits](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/), or [sign up for a trial subscription](https://azure.microsoft.com/en-us/pricing/free-trial/).</span></span>

### <a name="deploying-a-signalr-web-app-to-azure"></a><span data-ttu-id="2a9ab-133">Развертывание веб-приложения SignalR в Azure</span><span class="sxs-lookup"><span data-stu-id="2a9ab-133">Deploying a SignalR web app to Azure</span></span>

1. <span data-ttu-id="2a9ab-134">Завершить [учебник по началу работы](../getting-started/tutorial-getting-started-with-signalr.md), или загрузить завершенного проекта из [коллекции кода](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="2a9ab-134">Complete the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the finished project from [Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
2. <span data-ttu-id="2a9ab-135">В Visual Studio, выберите **построения**, **публикации чат SignalR**.</span><span class="sxs-lookup"><span data-stu-id="2a9ab-135">In Visual Studio, select **Build**, **Publish SignalR Chat**.</span></span>
3. <span data-ttu-id="2a9ab-136">В диалоговом окне «Публикация веб-сайта» выберите «веб-сайтов Windows Azure».</span><span class="sxs-lookup"><span data-stu-id="2a9ab-136">In the "Publish Web" dialog, select "Windows Azure Web Sites".</span></span>

    ![Выберите веб-сайты Azure](using-signalr-with-azure-web-sites/_static/image1.png)
4. <span data-ttu-id="2a9ab-138">Если вы не вошли свою учетную запись Майкрософт, нажмите кнопку **входа...**  в диалоговое окно «выберите существующий веб-сайт», а вход.</span><span class="sxs-lookup"><span data-stu-id="2a9ab-138">If you aren't signed in to your Microsoft account, click **Sign In...** in the "Select Existing Web Site" dialog, and sign in.</span></span>

    ![Выберите существующий веб-сайт](using-signalr-with-azure-web-sites/_static/image2.png)    ![Войдите в Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. <span data-ttu-id="2a9ab-141">В диалоговом окне «выберите существующий веб-сайт» выберите **New**.</span><span class="sxs-lookup"><span data-stu-id="2a9ab-141">In the "Select Existing Web Site" dialog, click **New**.</span></span>

    ![Новый веб-сайт](using-signalr-with-azure-web-sites/_static/image4.png)
6. <span data-ttu-id="2a9ab-143">В диалоговом окне «Создание сайта в Windows Azure» введите имя уникальным приложения.</span><span class="sxs-lookup"><span data-stu-id="2a9ab-143">In the "Create site on Windows Azure" dialog, enter a unique app name.</span></span> <span data-ttu-id="2a9ab-144">Выберите регион, ближайший к вам в раскрывающемся списке регион.</span><span class="sxs-lookup"><span data-stu-id="2a9ab-144">Select the region closest to you in the Region dropdown.</span></span> <span data-ttu-id="2a9ab-145">Нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="2a9ab-145">Click **Create**.</span></span>

    ![Создание сайта в Azure](using-signalr-with-azure-web-sites/_static/image5.png)
7. <span data-ttu-id="2a9ab-147">В диалоговом окне «Публикация веб-сайта» выберите **публикации**.</span><span class="sxs-lookup"><span data-stu-id="2a9ab-147">In the "Publish Web" dialog, click **Publish**.</span></span>

    ![опубликовать сайт](using-signalr-with-azure-web-sites/_static/image6.png)
8. <span data-ttu-id="2a9ab-149">После завершения публикации приложения в браузере откроется чата SignalR приложение, размещенное в веб-приложениях службы приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="2a9ab-149">When the app has completed publishing, the SignalR Chat application hosted in Azure App Service Web Apps will open in a browser.</span></span>

    ![Узел, открывая в браузере](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a><span data-ttu-id="2a9ab-151">Включение соединений WebSocket в веб-приложениях службы приложений Azure</span><span class="sxs-lookup"><span data-stu-id="2a9ab-151">Enabling WebSockets on Azure App Service Web Apps</span></span>

<span data-ttu-id="2a9ab-152">WebSockets необходимо явно включить в веб-приложения для использования в приложении SignalR; в противном случае будут использоваться другие протоколы (см. [транспортов и в случае ошибки](../getting-started/introduction-to-signalr.md#transports) подробные сведения).</span><span class="sxs-lookup"><span data-stu-id="2a9ab-152">WebSockets needs to be explicitly enabled in your web app to be used in a SignalR application; otherwise, other protocols will be used (See [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports) for details).</span></span>

<span data-ttu-id="2a9ab-153">Чтобы использовать соединения WebSocket на веб-приложениях службы приложений Azure, необходимо включите его в раздел конфигурации веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="2a9ab-153">In order to use WebSockets on Azure App Service Web Apps, enable it in the configuration section of the web app.</span></span> <span data-ttu-id="2a9ab-154">Чтобы сделать это, откройте веб-приложения в [портала управления Azure](https://manage.windowsazure.com/)и выберите пункт Настройка.</span><span class="sxs-lookup"><span data-stu-id="2a9ab-154">To do this, open your web app in the [Azure Management Portal](https://manage.windowsazure.com/), and select Configure.</span></span>

![Вкладка "Настройка"](using-signalr-with-azure-web-sites/_static/image8.png)

<span data-ttu-id="2a9ab-156">В верхней части страницы конфигурации убедитесь, .NET 4.5 используется для веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="2a9ab-156">At the top of the configuration page, ensure that .NET 4.5 is used for your web app.</span></span>

![.NET framework версии 4.5 параметр](using-signalr-with-azure-web-sites/_static/image9.png)

<span data-ttu-id="2a9ab-158">На странице «Конфигурация» в **WebSockets** выберите **на**.</span><span class="sxs-lookup"><span data-stu-id="2a9ab-158">On the configuration page, in the **WebSockets** setting, select **On**.</span></span>

![Параметр WebSockets: на](using-signalr-with-azure-web-sites/_static/image10.png)

<span data-ttu-id="2a9ab-160">В нижней части страницы конфигурации, выберите **Сохранить** для сохранения изменений.</span><span class="sxs-lookup"><span data-stu-id="2a9ab-160">At the bottom of the Configuration page, select **Save** to save your changes.</span></span>

![Сохранить параметры](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a><span data-ttu-id="2a9ab-162">Использование объединительной платы кэша Redis для Azure</span><span class="sxs-lookup"><span data-stu-id="2a9ab-162">Using the Azure Redis Cache Backplane</span></span>

<span data-ttu-id="2a9ab-163">Если вы используете несколько экземпляров веб-приложения, и пользователи этих экземпляров должны взаимодействовать друг с другом (, чтобы, например, мгновенных сообщений, созданных в один экземпляр может достигать пользователей, подключенных к другим экземплярам), [кэша Redis для Azure Задняя панель](../performance/scaleout-with-redis.md) должен быть реализован в вашем приложении.</span><span class="sxs-lookup"><span data-stu-id="2a9ab-163">If you use multiple instances for your web app, and the users of those instances need to interact with one another (so that, for instance, chat messages created in one instance can reach the users connected to other instances), the [Azure Redis Cache backplane](../performance/scaleout-with-redis.md) must be implemented in your application.</span></span>

<a id="nextsteps"></a>
## <a name="next-steps"></a><span data-ttu-id="2a9ab-164">Дальнейшие действия</span><span class="sxs-lookup"><span data-stu-id="2a9ab-164">Next Steps</span></span>

<span data-ttu-id="2a9ab-165">Дополнительные сведения о веб-приложений в службе приложений Azure см. в разделе [Обзор веб-приложений](https://azure.microsoft.com/en-us/documentation/articles/app-service-web-overview/).</span><span class="sxs-lookup"><span data-stu-id="2a9ab-165">For more information on Web Apps in Azure App Service, see [Web Apps overview](https://azure.microsoft.com/en-us/documentation/articles/app-service-web-overview/).</span></span>
