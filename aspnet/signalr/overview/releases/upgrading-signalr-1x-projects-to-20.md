---
uid: signalr/overview/releases/upgrading-signalr-1x-projects-to-20
title: "Обновление проектов SignalR 1.x до версии 2 | Документы Microsoft"
author: pfletcher
description: "В этом разделе описывается обновление существующего проекта 1.x SignalR для SignalR 2.x и способы устранения неполадок, которые могут возникнуть во время обновления..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: adcfef99-9bc5-489d-a91b-9b7c2bc35e04
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/releases/upgrading-signalr-1x-projects-to-20
msc.type: authoredcontent
ms.openlocfilehash: e372275ae5dd4bbf354db2d02e4407f8c513b7a3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="upgrading-signalr-1x-projects-to-version-2"></a><span data-ttu-id="02129-103">Обновление проектов SignalR 1.x до версии 2</span><span class="sxs-lookup"><span data-stu-id="02129-103">Upgrading SignalR 1.x Projects to version 2</span></span>
====================
<span data-ttu-id="02129-104">по [Патрик Флетчера](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="02129-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="02129-105">В этом разделе описывается обновление существующего проекта 1.x SignalR для SignalR 2.x и способы устранения проблем, которые могут возникнуть во время процесса обновления.</span><span class="sxs-lookup"><span data-stu-id="02129-105">This topic describes how to upgrade an existing SignalR 1.x project to SignalR 2.x, and how to troubleshoot issues that may arise during the upgrade process.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="02129-106">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="02129-106">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="02129-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="02129-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="02129-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="02129-108">.NET 4.5</span></span>
> - <span data-ttu-id="02129-109">SignalR версий 1 и 2</span><span class="sxs-lookup"><span data-stu-id="02129-109">SignalR versions 1 and 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="02129-110">С помощью Visual Studio 2012 с помощью этого учебника</span><span class="sxs-lookup"><span data-stu-id="02129-110">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="02129-111">Чтобы использовать Visual Studio 2012 с помощью этого учебника, выполните следующее:</span><span class="sxs-lookup"><span data-stu-id="02129-111">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="02129-112">Обновление вашего [диспетчера пакетов](http://docs.nuget.org/docs/start-here/installing-nuget) до последней версии.</span><span class="sxs-lookup"><span data-stu-id="02129-112">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="02129-113">Установка [веб-установщика платформы](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="02129-113">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="02129-114">Установщик веб-платформы для поиска и установки **ASP.NET и 2013.1 Web Tools для Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="02129-114">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="02129-115">Это будет установки Visual Studio шаблоны для SignalR классов, таких как **концентратора**.</span><span class="sxs-lookup"><span data-stu-id="02129-115">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="02129-116">Некоторые шаблоны (такие как **класс запуска OWIN**) будет недоступен; в этом случае используйте файл класса.</span><span class="sxs-lookup"><span data-stu-id="02129-116">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="02129-117">Вопросы и комментарии</span><span class="sxs-lookup"><span data-stu-id="02129-117">Questions and comments</span></span>
> 
> <span data-ttu-id="02129-118">Оставьте отзыв на том, как вам понравилось этого учебника и что можно улучшить в комментарии в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="02129-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="02129-119">Если у вас есть вопросы, которые не связаны непосредственно для работы с учебником, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="02129-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="02129-120">SignalR 2 обеспечивает согласованность при работе в серверных платформ с помощью [OWIN](http://owin.org).</span><span class="sxs-lookup"><span data-stu-id="02129-120">SignalR 2 offers a consistent development experience across server platforms using [OWIN](http://owin.org).</span></span> <span data-ttu-id="02129-121">В этой статье описывается несколько шагов, которые необходимы для обновления приложения 1.x SignalR до версии 2.</span><span class="sxs-lookup"><span data-stu-id="02129-121">This article describes the few steps that are needed to update a SignalR 1.x application to version 2.</span></span>

<span data-ttu-id="02129-122">Во время его рекомендуется обновить приложения на SignalR 2, SignalR 1.x будет по-прежнему поддерживаться.</span><span class="sxs-lookup"><span data-stu-id="02129-122">While it is encouraged to upgrade applications to SignalR 2, SignalR 1.x will still be supported.</span></span>

<span data-ttu-id="02129-123">Этот учебник описывает обновление приложения на веб сервере 2 SignalR.</span><span class="sxs-lookup"><span data-stu-id="02129-123">This tutorial describes how to upgrade a web-hosted application to SignalR 2.</span></span> <span data-ttu-id="02129-124">Резидентных приложений (для которых размещения на сервере в консольном приложении, службы Windows или другой процесс) теперь поддерживаются в группе SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="02129-124">Self-hosted applications (those that host a server in a console application, Windows service, or other process) are now supported under SignalR 2.</span></span> <span data-ttu-id="02129-125">Сведения о том, как приступить к созданию резидентного приложения с SignalR 2 см. в разделе [учебника: SignalR резидентной](../deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="02129-125">For information on how to get started creating a self-hosted application with SignalR 2, see [Tutorial: SignalR Self-Host](../deployment/tutorial-signalr-self-host.md).</span></span>

## <a name="contents"></a><span data-ttu-id="02129-126">Описание</span><span class="sxs-lookup"><span data-stu-id="02129-126">Contents</span></span>

<span data-ttu-id="02129-127">В следующих разделах описаны задачи, связанные с обновлением проектов SignalR и устранение неполадок, которые могут возникнуть.</span><span class="sxs-lookup"><span data-stu-id="02129-127">The following sections describe tasks involved with upgrading SignalR projects, and how to troubleshoot issues that may arise.</span></span>

- [<span data-ttu-id="02129-128">Пример: Учебник для начала обновления до SignalR 2</span><span class="sxs-lookup"><span data-stu-id="02129-128">Example: Upgrading the Getting Started tutorial to SignalR 2</span></span>](#example)
- [<span data-ttu-id="02129-129">Устранение ошибок, возникших во время обновления</span><span class="sxs-lookup"><span data-stu-id="02129-129">Troubleshooting errors encountered during upgrading</span></span>](#troubleshooting)

<a id="example"></a>

## <a name="example-upgrading-the-getting-started-tutorial-application-to-signalr-2"></a><span data-ttu-id="02129-130">Пример: Обновление Приступая к работе учебного приложения для SignalR 2</span><span class="sxs-lookup"><span data-stu-id="02129-130">Example: Upgrading the Getting Started tutorial application to SignalR 2</span></span>

<span data-ttu-id="02129-131">В этом разделе будет обновлять приложение, созданное в [SignalR версии 1.x учебник по началу работы](../older-versions/index.md) для использования SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="02129-131">In this section, you'll update the application created in the [SignalR 1.x version of the Getting Started Tutorial](../older-versions/index.md) to use SignalR 2.</span></span>

1. <span data-ttu-id="02129-132">Закончив учебника Приступая к работе, щелкните правой кнопкой мыши проект и выберите **свойства**.</span><span class="sxs-lookup"><span data-stu-id="02129-132">Once you've finished the Getting Started tutorial, right-click on the project, and select **Properties**.</span></span> <span data-ttu-id="02129-133">Убедитесь, что **требуемой версии .NET framework** равно **.NET Framework 4.5.**</span><span class="sxs-lookup"><span data-stu-id="02129-133">Verify that the **Target framework** is set to **.NET Framework 4.5.**</span></span>
2. <span data-ttu-id="02129-134">Откройте консоль диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="02129-134">Open the Package Manager Console.</span></span> <span data-ttu-id="02129-135">Удалить SignalR 1.x из проекта, используя следующую команду:</span><span class="sxs-lookup"><span data-stu-id="02129-135">Remove SignalR 1.x from the project using the following command:</span></span>

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample1.ps1)]
3. <span data-ttu-id="02129-136">Установите SignalR 2, используя следующую команду:</span><span class="sxs-lookup"><span data-stu-id="02129-136">Install SignalR 2 using the following command:</span></span>

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample2.ps1)]
4. <span data-ttu-id="02129-137">В HTML-страницы обновите ссылку на скрипт для SignalR в соответствии с версией теперь включены в проект скрипта.</span><span class="sxs-lookup"><span data-stu-id="02129-137">In the HTML page, update the script reference for SignalR to match the version of the script now included in the project.</span></span>

    [!code-html[Main](upgrading-signalr-1x-projects-to-20/samples/sample3.html)]
5. <span data-ttu-id="02129-138">В глобального класса приложения удалите вызов MapHubs.</span><span class="sxs-lookup"><span data-stu-id="02129-138">In the global application class, remove the call to MapHubs.</span></span>

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample4.cs)]
6. <span data-ttu-id="02129-139">Щелкните правой кнопкой мыши решение и выберите **добавить**, **новый элемент...** . В диалоговом окне выберите **класс запуска Owin**.</span><span class="sxs-lookup"><span data-stu-id="02129-139">Right-click the solution, and select **Add**, **New Item...**. In the dialog, select **Owin Startup Class**.</span></span> <span data-ttu-id="02129-140">Имя для нового класса **файла Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="02129-140">Name the new class **Startup.cs**.</span></span>

    ![](upgrading-signalr-1x-projects-to-20/_static/image1.png)
7. <span data-ttu-id="02129-141">Замените содержимое файла Startup.cs следующий код:</span><span class="sxs-lookup"><span data-stu-id="02129-141">Replace the contents of Startup.cs with the following code:</span></span>

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample5.cs)]

    <span data-ttu-id="02129-142">Добавляет атрибут сборки класса в Owin при запуске процесса, который выполняет `Configuration` метод при запуске Owin.</span><span class="sxs-lookup"><span data-stu-id="02129-142">The assembly attribute adds the class to Owin's startup process, which executes the `Configuration` method when Owin starts up.</span></span> <span data-ttu-id="02129-143">Это в свою очередь вызывает `MapSignalR` метод, который создает маршруты для всех концентраторов SignalR в приложении.</span><span class="sxs-lookup"><span data-stu-id="02129-143">This in turn calls the `MapSignalR` method, which creates routes for all SignalR hubs in the application.</span></span>
8. <span data-ttu-id="02129-144">Запустите проект и скопируйте URL-адрес главной страницы в другой браузер или область обозревателя, как раньше.</span><span class="sxs-lookup"><span data-stu-id="02129-144">Run the project, and copy the URL of the main page into another browser or browser pane, as before.</span></span> <span data-ttu-id="02129-145">Каждая страница будет запрашивать имя пользователя и сообщений, отправленных с каждой страницы должны быть видимыми в обеих областях браузера.</span><span class="sxs-lookup"><span data-stu-id="02129-145">Each page will ask for a username, and messages sent from each page should be visible in both browser panes.</span></span>

<a id="troubleshooting"></a>

## <a name="troubleshooting-errors-encountered-during-upgrading"></a><span data-ttu-id="02129-146">Устранение ошибок, возникших во время обновления</span><span class="sxs-lookup"><span data-stu-id="02129-146">Troubleshooting errors encountered during upgrading</span></span>

<span data-ttu-id="02129-147">В этом разделе описываются проблемы, которые могут возникнуть во время обновления.</span><span class="sxs-lookup"><span data-stu-id="02129-147">This section describes issues that may arise during upgrading.</span></span> <span data-ttu-id="02129-148">Более полный список ошибок и проблем, которые могут возникнуть при использовании приложения SignalR см. в разделе [SignalR Устранение неполадок](../testing-and-debugging/troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="02129-148">For a more comprehensive list of errors and issues that may occur with a SignalR application, see [SignalR Troubleshooting](../testing-and-debugging/troubleshooting.md).</span></span>

### <a name="the-call-is-ambiguous-between-the-following-methods-or-properties"></a><span data-ttu-id="02129-149">«Вызов определяется неоднозначно между следующие методы или свойства»</span><span class="sxs-lookup"><span data-stu-id="02129-149">'The call is ambiguous between the following methods or properties'</span></span>

<span data-ttu-id="02129-150">Эта ошибка возникает, если ссылку на `Microsoft.AspNet.SignalR.Owin` не удаляется.</span><span class="sxs-lookup"><span data-stu-id="02129-150">This error will occur if a reference to `Microsoft.AspNet.SignalR.Owin` is not removed.</span></span> <span data-ttu-id="02129-151">Этот пакет является устаревшей; необходимо удалить ссылку, и версии 1.x SelfHost пакета должны быть удалены.</span><span class="sxs-lookup"><span data-stu-id="02129-151">This package is deprecated; the reference must be removed and the 1.x version of the SelfHost package must be uninstalled.</span></span>

### <a name="hub-methods-fail-silently"></a><span data-ttu-id="02129-152">Методы концентратора сбоем без уведомления</span><span class="sxs-lookup"><span data-stu-id="02129-152">Hub methods fail silently</span></span>

<span data-ttu-id="02129-153">Убедитесь, что ссылки на скрипты в клиентском приложении до даты и что `OwinStartup` атрибут для запуска класс имеет правильный класс и имя сборки проекта.</span><span class="sxs-lookup"><span data-stu-id="02129-153">Verify that the script references in your client are up to date, and that the `OwinStartup` attribute for your Startup class has the correct class and assembly names for your project.</span></span> <span data-ttu-id="02129-154">Кроме того попытайтесь открыть адреса концентраторов (/ signalr/концентраторов), в браузере; любое сообщение об ошибке будет предлагать дополнительные сведения о том, что пошло не так.</span><span class="sxs-lookup"><span data-stu-id="02129-154">Also, try opening up the hubs address (/signalr/hubs) in your browser; any error that appears will offer more information about what's going wrong.</span></span>
