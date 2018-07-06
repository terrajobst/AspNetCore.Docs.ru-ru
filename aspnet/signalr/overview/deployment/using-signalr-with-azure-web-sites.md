---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: С помощью SignalR с веб-приложениями в службе приложений Azure | Документация Майкрософт
author: pfletcher
description: В этом документе описываются способы настройки приложения SignalR, которое работает на базе Microsoft Azure. Версии программного обеспечения используется в этом руководстве, Visual Studio 2013 или Vis...
ms.author: aspnetcontent
ms.date: 07/01/2015
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 03db2debe6bf5935ced926ae8278df8b9d47c86a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822170"
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a><span data-ttu-id="c3c78-104">С помощью SignalR с веб-приложениями в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="c3c78-104">Using SignalR with Web Apps in Azure App Service</span></span>
====================
<span data-ttu-id="c3c78-105">по [Патрик Флетчера](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="c3c78-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="c3c78-106">В этом документе описываются способы настройки приложения SignalR, которое работает на базе Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="c3c78-106">This document describes how to configure a SignalR application that runs on Microsoft Azure.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c3c78-107">Версии программного обеспечения, используемые в этом руководстве</span><span class="sxs-lookup"><span data-stu-id="c3c78-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="c3c78-108">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) или Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="c3c78-108">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) or Visual Studio 2012</span></span>
> - <span data-ttu-id="c3c78-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="c3c78-109">.NET 4.5</span></span>
> - <span data-ttu-id="c3c78-110">SignalR версии 2</span><span class="sxs-lookup"><span data-stu-id="c3c78-110">SignalR version 2</span></span>
> - <span data-ttu-id="c3c78-111">Пакет Azure SDK 2.3 для Visual Studio 2013 или 2012</span><span class="sxs-lookup"><span data-stu-id="c3c78-111">Azure SDK 2.3 for Visual Studio 2013 or 2012</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="c3c78-112">Вопросы и комментарии</span><span class="sxs-lookup"><span data-stu-id="c3c78-112">Questions and comments</span></span>
> 
> <span data-ttu-id="c3c78-113">Оставьте свои отзывы на том, как вам понравилось, и этот учебник и что можно улучшить в комментариях в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="c3c78-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="c3c78-114">Если у вас есть вопросы, которые не имеют отношения к руководству, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), или [форумы Microsoft Azure](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span><span class="sxs-lookup"><span data-stu-id="c3c78-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), or the [Microsoft Azure forums](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span></span>


## <a name="table-of-contents"></a><span data-ttu-id="c3c78-115">Содержание</span><span class="sxs-lookup"><span data-stu-id="c3c78-115">Table of Contents</span></span>

- [<span data-ttu-id="c3c78-116">Введение</span><span class="sxs-lookup"><span data-stu-id="c3c78-116">Introduction</span></span>](#introduction)
- [<span data-ttu-id="c3c78-117">Развертывание SignalR веб-приложения в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="c3c78-117">Deploying a SignalR Web App to Azure App Service</span></span>](#deploying)
- [<span data-ttu-id="c3c78-118">Включение WebSockets в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="c3c78-118">Enabling WebSockets on Azure App Service</span></span>](#websocket)
- [<span data-ttu-id="c3c78-119">Использование объединительной платы кэша redis для Azure</span><span class="sxs-lookup"><span data-stu-id="c3c78-119">Using the Azure Redis Cache Backplane</span></span>](#backplane)
- [<span data-ttu-id="c3c78-120">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="c3c78-120">Next Steps</span></span>](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a><span data-ttu-id="c3c78-121">Вступление</span><span class="sxs-lookup"><span data-stu-id="c3c78-121">Introduction</span></span>

<span data-ttu-id="c3c78-122">ASP.NET SignalR можно использовать для переноса на новый уровень взаимодействия между серверами "и" веб "или" клиентов .NET.</span><span class="sxs-lookup"><span data-stu-id="c3c78-122">ASP.NET SignalR can be used to bring a new level of interactivity between servers and web or .NET clients.</span></span> <span data-ttu-id="c3c78-123">При размещении в Azure приложений SignalR можно воспользоваться преимуществами высокой доступности, масштабируемый и предоставляет среду с высокой производительностью, работающее в облаке.</span><span class="sxs-lookup"><span data-stu-id="c3c78-123">When hosted in Azure, SignalR applications can take advantage of the highly available, scalable, and performant environment that running in the cloud provides.</span></span>

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a><span data-ttu-id="c3c78-124">Развертывание SignalR веб-приложения в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="c3c78-124">Deploying a SignalR Web App to Azure App Service</span></span>

<span data-ttu-id="c3c78-125">SignalR не добавляет всяких осложнений, определенного с развертыванием приложения в Azure и развертывание на локальном сервере.</span><span class="sxs-lookup"><span data-stu-id="c3c78-125">SignalR doesn't add any particular complications to deploying an application to Azure versus deploying to an on-premises server.</span></span> <span data-ttu-id="c3c78-126">Приложения, использующего SignalR может размещаться в Azure без изменений в конфигурации и других параметров (хотя поддержка WebSockets, см. в разделе [Включение WebSockets в службе приложений Azure](#websocket) ниже.) В этом руководстве вы развернете приложение, созданное в [Приступая к работе](../getting-started/tutorial-getting-started-with-signalr.md) в Azure.</span><span class="sxs-lookup"><span data-stu-id="c3c78-126">An application that uses SignalR can be hosted in Azure without any changes in configuration or other settings (though for WebSockets support, see [Enabling WebSockets on Azure App Service](#websocket) below.) For this tutorial, you'll deploy the application created in the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md) to Azure.</span></span>

<span data-ttu-id="c3c78-127">**Необходимые компоненты**</span><span class="sxs-lookup"><span data-stu-id="c3c78-127">**Prerequisites**</span></span>

- <span data-ttu-id="c3c78-128">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="c3c78-128">Visual Studio 2013.</span></span> <span data-ttu-id="c3c78-129">Если у вас нет Visual Studio, Visual Studio 2013 Express для Web входит в состав установки пакета Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="c3c78-129">If you don't have Visual Studio, Visual Studio 2013 Express for Web is included in the install of the Azure SDK.</span></span>
- <span data-ttu-id="c3c78-130">[Пакет Azure SDK 2.3 для Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) или [2.3 пакета Azure SDK для Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span><span class="sxs-lookup"><span data-stu-id="c3c78-130">[Azure SDK 2.3 for Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) or [Azure SDK 2.3 for Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span></span>
- <span data-ttu-id="c3c78-131">Для работы с этим руководством требуется подписка Azure.</span><span class="sxs-lookup"><span data-stu-id="c3c78-131">To complete this tutorial, you will need an Azure subscription.</span></span> <span data-ttu-id="c3c78-132">Вы можете [активируйте преимущества для подписчиков MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), или [зарегистрироваться для получения пробной подписки](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c3c78-132">You can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), or [sign up for a trial subscription](https://azure.microsoft.com/pricing/free-trial/).</span></span>

### <a name="deploying-a-signalr-web-app-to-azure"></a><span data-ttu-id="c3c78-133">Развертывание веб-приложения SignalR в Azure</span><span class="sxs-lookup"><span data-stu-id="c3c78-133">Deploying a SignalR web app to Azure</span></span>

1. <span data-ttu-id="c3c78-134">Завершить [Приступая к работе](../getting-started/tutorial-getting-started-with-signalr.md), или загрузить готовый проект из [коллекции исходных кодов](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="c3c78-134">Complete the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the finished project from [Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
2. <span data-ttu-id="c3c78-135">В Visual Studio выберите **построения**, **публикации SignalR Chat**.</span><span class="sxs-lookup"><span data-stu-id="c3c78-135">In Visual Studio, select **Build**, **Publish SignalR Chat**.</span></span>
3. <span data-ttu-id="c3c78-136">В диалоговом окне «Публикация веб-сайта» выберите «веб-сайтов Windows Azure».</span><span class="sxs-lookup"><span data-stu-id="c3c78-136">In the "Publish Web" dialog, select "Windows Azure Web Sites".</span></span>

    ![Выберите веб-сайты Azure](using-signalr-with-azure-web-sites/_static/image1.png)
4. <span data-ttu-id="c3c78-138">Если вы не вошли учетную запись Майкрософт, нажмите кнопку **вход...**  в диалоговое окно «выберите существующий веб-сайт» и входа.</span><span class="sxs-lookup"><span data-stu-id="c3c78-138">If you aren't signed in to your Microsoft account, click **Sign In...** in the "Select Existing Web Site" dialog, and sign in.</span></span>

    ![Выберите существующий веб-сайт](using-signalr-with-azure-web-sites/_static/image2.png)    ![Вход в Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. <span data-ttu-id="c3c78-141">В диалоговом окне «выберите существующий веб-сайт» нажмите кнопку **New**.</span><span class="sxs-lookup"><span data-stu-id="c3c78-141">In the "Select Existing Web Site" dialog, click **New**.</span></span>

    ![Новый веб-сайт](using-signalr-with-azure-web-sites/_static/image4.png)
6. <span data-ttu-id="c3c78-143">В диалоговом окне «Создание сайта в Windows Azure» введите уникальное имя.</span><span class="sxs-lookup"><span data-stu-id="c3c78-143">In the "Create site on Windows Azure" dialog, enter a unique app name.</span></span> <span data-ttu-id="c3c78-144">Выберите ближайший к вам регион в раскрывающемся списке регион.</span><span class="sxs-lookup"><span data-stu-id="c3c78-144">Select the region closest to you in the Region dropdown.</span></span> <span data-ttu-id="c3c78-145">Нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="c3c78-145">Click **Create**.</span></span>

    ![Создание сайта в Azure](using-signalr-with-azure-web-sites/_static/image5.png)
7. <span data-ttu-id="c3c78-147">В диалоговом окне «Публикация веб-сайта» щелкните **публикации**.</span><span class="sxs-lookup"><span data-stu-id="c3c78-147">In the "Publish Web" dialog, click **Publish**.</span></span>

    ![Опубликовать сайт](using-signalr-with-azure-web-sites/_static/image6.png)
8. <span data-ttu-id="c3c78-149">После завершения публикации приложения чата SignalR приложение, размещенное в веб-приложениях службы приложений Azure откроется в браузере.</span><span class="sxs-lookup"><span data-stu-id="c3c78-149">When the app has completed publishing, the SignalR Chat application hosted in Azure App Service Web Apps will open in a browser.</span></span>

    ![Узел, открыв в браузере](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a><span data-ttu-id="c3c78-151">Включение WebSockets в веб-приложениях службы приложений Azure</span><span class="sxs-lookup"><span data-stu-id="c3c78-151">Enabling WebSockets on Azure App Service Web Apps</span></span>

<span data-ttu-id="c3c78-152">WebSockets необходимо явно включить в веб-приложения для использования в приложении SignalR; в противном случае будет использоваться другие протоколы (см. в разделе [транспорта и в случае ошибки](../getting-started/introduction-to-signalr.md#transports) сведения).</span><span class="sxs-lookup"><span data-stu-id="c3c78-152">WebSockets needs to be explicitly enabled in your web app to be used in a SignalR application; otherwise, other protocols will be used (See [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports) for details).</span></span>

<span data-ttu-id="c3c78-153">Чтобы использовать WebSocket в веб-приложениях службы приложений Azure, включите его в разделе конфигурации веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="c3c78-153">In order to use WebSockets on Azure App Service Web Apps, enable it in the configuration section of the web app.</span></span> <span data-ttu-id="c3c78-154">Чтобы сделать это, откройте веб-приложение в [портала управления Azure](https://manage.windowsazure.com/)и выберите «настроить».</span><span class="sxs-lookup"><span data-stu-id="c3c78-154">To do this, open your web app in the [Azure Management Portal](https://manage.windowsazure.com/), and select Configure.</span></span>

![Вкладка "Настройка"](using-signalr-with-azure-web-sites/_static/image8.png)

<span data-ttu-id="c3c78-156">В верхней части страницы конфигурации убедитесь, что для веб-приложения используется .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="c3c78-156">At the top of the configuration page, ensure that .NET 4.5 is used for your web app.</span></span>

![.NET framework версии 4.5 параметр](using-signalr-with-azure-web-sites/_static/image9.png)

<span data-ttu-id="c3c78-158">На странице «Конфигурация» в **WebSockets** выберите **на**.</span><span class="sxs-lookup"><span data-stu-id="c3c78-158">On the configuration page, in the **WebSockets** setting, select **On**.</span></span>

![Параметр WebSockets: на](using-signalr-with-azure-web-sites/_static/image10.png)

<span data-ttu-id="c3c78-160">В нижней части страницы конфигурации, выберите **Сохранить** для сохранения изменений.</span><span class="sxs-lookup"><span data-stu-id="c3c78-160">At the bottom of the Configuration page, select **Save** to save your changes.</span></span>

![Сохранить параметры](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a><span data-ttu-id="c3c78-162">Использование объединительной платы кэша redis для Azure</span><span class="sxs-lookup"><span data-stu-id="c3c78-162">Using the Azure Redis Cache Backplane</span></span>

<span data-ttu-id="c3c78-163">Если используется несколько экземпляров для веб-приложения и пользователи этих экземпляров должны взаимодействовать друг с другом (позволяя, например, сообщения чата, созданные в одном экземпляре обращаться пользователей, подключенных к другим экземплярам), [кэша Redis для Azure Задняя панель](../performance/scaleout-with-redis.md) должен быть реализован в приложении.</span><span class="sxs-lookup"><span data-stu-id="c3c78-163">If you use multiple instances for your web app, and the users of those instances need to interact with one another (so that, for instance, chat messages created in one instance can reach the users connected to other instances), the [Azure Redis Cache backplane](../performance/scaleout-with-redis.md) must be implemented in your application.</span></span>

<a id="nextsteps"></a>
## <a name="next-steps"></a><span data-ttu-id="c3c78-164">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="c3c78-164">Next Steps</span></span>

<span data-ttu-id="c3c78-165">Дополнительные сведения о веб-приложений в службе приложений Azure, см. в разделе [Обзор веб-приложений](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span><span class="sxs-lookup"><span data-stu-id="c3c78-165">For more information on Web Apps in Azure App Service, see [Web Apps overview](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span></span>
