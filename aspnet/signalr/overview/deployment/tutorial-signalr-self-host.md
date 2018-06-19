---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Учебник: SignalR резидентной | Документы Microsoft'
author: pfletcher
description: Этот учебник показывает, как создание резидентной SignalR 2 сервера и способ подключения с помощью клиента JavaScript. Версии программного обеспечения, используемая в этом учебнике V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: d38e6fbc3407e4beca6942bbdefcaa8258ebc5ad
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036782"
---
<a name="tutorial-signalr-self-host"></a><span data-ttu-id="318c3-104">Учебник: SignalR резидентной</span><span class="sxs-lookup"><span data-stu-id="318c3-104">Tutorial: SignalR Self-Host</span></span>
====================
<span data-ttu-id="318c3-105">по [Патрик Флетчера](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="318c3-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[<span data-ttu-id="318c3-106">Загрузка завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="318c3-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> <span data-ttu-id="318c3-107">Этот учебник показывает, как создание резидентной SignalR 2 сервера и способ подключения с помощью клиента JavaScript.</span><span class="sxs-lookup"><span data-stu-id="318c3-107">This tutorial shows how to create a self-hosted SignalR 2 server, and how to connect to it with a JavaScript client.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="318c3-108">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="318c3-108">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="318c3-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="318c3-109">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="318c3-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="318c3-110">.NET 4.5</span></span>
> - <span data-ttu-id="318c3-111">SignalR версии 2</span><span class="sxs-lookup"><span data-stu-id="318c3-111">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="318c3-112">С помощью Visual Studio 2012 с помощью этого учебника</span><span class="sxs-lookup"><span data-stu-id="318c3-112">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="318c3-113">Чтобы использовать Visual Studio 2012 с помощью этого учебника, выполните следующее:</span><span class="sxs-lookup"><span data-stu-id="318c3-113">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="318c3-114">Обновление вашего [диспетчера пакетов](http://docs.nuget.org/docs/start-here/installing-nuget) до последней версии.</span><span class="sxs-lookup"><span data-stu-id="318c3-114">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="318c3-115">Установка [веб-установщика платформы](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="318c3-115">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="318c3-116">Установщик веб-платформы для поиска и установки **ASP.NET и 2013.1 Web Tools для Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="318c3-116">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="318c3-117">Это будет установки Visual Studio шаблоны для SignalR классов, таких как **концентратора**.</span><span class="sxs-lookup"><span data-stu-id="318c3-117">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="318c3-118">Некоторые шаблоны (такие как **класс запуска OWIN**) будет недоступен; в этом случае используйте файл класса.</span><span class="sxs-lookup"><span data-stu-id="318c3-118">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="318c3-119">Вопросы и комментарии</span><span class="sxs-lookup"><span data-stu-id="318c3-119">Questions and comments</span></span>
> 
> <span data-ttu-id="318c3-120">Оставьте отзыв на том, как вам понравилось этого учебника и что можно улучшить в комментарии в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="318c3-120">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="318c3-121">Если у вас есть вопросы, которые не связаны непосредственно для работы с учебником, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="318c3-121">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="318c3-122">Обзор</span><span class="sxs-lookup"><span data-stu-id="318c3-122">Overview</span></span>

<span data-ttu-id="318c3-123">Сервер SignalR, как правило, размещенного в приложении ASP.NET в IIS, но также может быть резидентной (например консольное приложение или служба Windows) с помощью библиотеки резидентной.</span><span class="sxs-lookup"><span data-stu-id="318c3-123">A SignalR server is usually hosted in an ASP.NET application in IIS, but it can also be self-hosted (such as in a console application or Windows service) using the self-host library.</span></span> <span data-ttu-id="318c3-124">Эта библиотека, как и всех SignalR 2, основана на OWIN ([открыть веб-интерфейс .NET](http://owin.org)).</span><span class="sxs-lookup"><span data-stu-id="318c3-124">This library, like all of SignalR 2, is built on OWIN ([Open Web Interface for .NET](http://owin.org)).</span></span> <span data-ttu-id="318c3-125">OWIN определяет абстракцию между .NET веб-серверов и веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="318c3-125">OWIN defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="318c3-126">OWIN разрывает связь веб-приложения с сервера, благодаря чему идеально подходит для размещения веб-приложения в свой собственный процесс, за пределами служб IIS на собственном сервере OWIN.</span><span class="sxs-lookup"><span data-stu-id="318c3-126">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>

<span data-ttu-id="318c3-127">Не размещено в службах IIS причины:</span><span class="sxs-lookup"><span data-stu-id="318c3-127">Reasons for not hosting in IIS include:</span></span>

- <span data-ttu-id="318c3-128">Средах, где IIS не доступен или желательно, такие как существующую ферму серверов без IIS.</span><span class="sxs-lookup"><span data-stu-id="318c3-128">Environments where IIS is not available or desirable, such as an existing server farm without IIS.</span></span>
- <span data-ttu-id="318c3-129">Производительность служб IIS необходимо избегать.</span><span class="sxs-lookup"><span data-stu-id="318c3-129">The performance overhead of IIS needs to be avoided.</span></span>
- <span data-ttu-id="318c3-130">Функциональные возможности SignalR — для добавления exising приложения, которое выполняется в службе Windows, рабочей роли Azure или другой процесс.</span><span class="sxs-lookup"><span data-stu-id="318c3-130">SignalR functionality is to be added to an exising application that runs in a Windows Service, Azure worker role, or other process.</span></span>

<span data-ttu-id="318c3-131">Если решение разработано как резидентного размещения для повышения производительности, рекомендуется также тестирования приложения, размещенные в IIS, для определения выигрыш в производительности.</span><span class="sxs-lookup"><span data-stu-id="318c3-131">If a solution is being developed as self-host for performance reasons, it's recommended to also test the application hosted in IIS to determine the performance benefit.</span></span>

<span data-ttu-id="318c3-132">Этот учебник содержит следующие разделы:</span><span class="sxs-lookup"><span data-stu-id="318c3-132">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="318c3-133">При создании сервера</span><span class="sxs-lookup"><span data-stu-id="318c3-133">Creating the server</span></span>](#server)
- [<span data-ttu-id="318c3-134">Доступ к серверу с помощью клиента JavaScript</span><span class="sxs-lookup"><span data-stu-id="318c3-134">Accessing the server with a JavaScript client</span></span>](#js)

<a id="server"></a>

## <a name="creating-the-server"></a><span data-ttu-id="318c3-135">При создании сервера</span><span class="sxs-lookup"><span data-stu-id="318c3-135">Creating the server</span></span>

<span data-ttu-id="318c3-136">В этом учебнике вы создадите сервера, размещенного в консольном приложении, но сервер может размещаться в каких-либо процесса, таких как служба Windows или рабочей роли Azure.</span><span class="sxs-lookup"><span data-stu-id="318c3-136">In this tutorial, you'll create a server that's hosted in a console application, but the server can be hosted in any sort of process, such as a Windows service or Azure worker role.</span></span> <span data-ttu-id="318c3-137">Образец кода для размещения сервера SignalR в службе Windows, см. [Self-Hosting SignalR в службе Windows](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span><span class="sxs-lookup"><span data-stu-id="318c3-137">For sample code for hosting a SignalR server in a Windows Service, see [Self-Hosting SignalR in a Windows Service](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span></span>

1. <span data-ttu-id="318c3-138">Откройте Visual Studio 2013 с правами администратора.</span><span class="sxs-lookup"><span data-stu-id="318c3-138">Open Visual Studio 2013 with administrator privileges.</span></span> <span data-ttu-id="318c3-139">Выберите **файл**, **новый проект**.</span><span class="sxs-lookup"><span data-stu-id="318c3-139">Select **File**, **New Project**.</span></span> <span data-ttu-id="318c3-140">Выберите **Windows** под **Visual C#** узел в **шаблоны** и выберите **консольное приложение** шаблона.</span><span class="sxs-lookup"><span data-stu-id="318c3-140">Select **Windows** under the **Visual C#** node in the **Templates** pane, and select the **Console Application** template.</span></span> <span data-ttu-id="318c3-141">Имя нового проекта «SignalRSelfHost» и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="318c3-141">Name the new project "SignalRSelfHost" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image1.png)
2. <span data-ttu-id="318c3-142">Откройте консоль диспетчера пакетов библиотеку, выбрав **средства**, **диспетчер пакетов библиотеки**, **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="318c3-142">Open the library package manager console by selecting **Tools**, **Library Package Manager**, **Package Manager Console**.</span></span>
3. <span data-ttu-id="318c3-143">В консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="318c3-143">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    <span data-ttu-id="318c3-144">Эта команда добавляет в проект библиотеки SignalR 2 Self-Host.</span><span class="sxs-lookup"><span data-stu-id="318c3-144">This command adds the SignalR 2 Self-Host libraries to the project.</span></span>
4. <span data-ttu-id="318c3-145">В консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="318c3-145">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    <span data-ttu-id="318c3-146">Эта команда добавляет Microsoft.Owin.Cors библиотеки в проект.</span><span class="sxs-lookup"><span data-stu-id="318c3-146">This command adds the Microsoft.Owin.Cors library to the project.</span></span> <span data-ttu-id="318c3-147">Эта библиотека будет использоваться для поддержки между доменами, необходимый для приложения, размещающие SignalR и веб-страницы клиента в разных доменах.</span><span class="sxs-lookup"><span data-stu-id="318c3-147">This library will be used for cross-domain support, which is required for applications that host SignalR and a web page client in different domains.</span></span> <span data-ttu-id="318c3-148">Поскольку вы будете размещение сервера SignalR и веб-клиента по различным портам, это означает, что для связи между этими компонентами должен быть включен, между доменами.</span><span class="sxs-lookup"><span data-stu-id="318c3-148">Since you'll be hosting the SignalR server and the web client on different ports, this means that cross-domain must be enabled for communication between these components.</span></span>
5. <span data-ttu-id="318c3-149">Замените содержимое Program.cs кодом из этого примера.</span><span class="sxs-lookup"><span data-stu-id="318c3-149">Replace the contents of Program.cs with the following code.</span></span>

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    <span data-ttu-id="318c3-150">Приведенный выше код включает в себя три класса:</span><span class="sxs-lookup"><span data-stu-id="318c3-150">The above code includes three classes:</span></span>

    - <span data-ttu-id="318c3-151">**Программа**, в том числе **Main** метод определение первичного пути выполнения.</span><span class="sxs-lookup"><span data-stu-id="318c3-151">**Program**, including the **Main** method defining the primary path of execution.</span></span> <span data-ttu-id="318c3-152">В этом методе веб-приложения типа **запуска** запускаются по указанному URL-АДРЕСУ (`http://localhost:8080`).</span><span class="sxs-lookup"><span data-stu-id="318c3-152">In this method, a web application of type **Startup** is started at the specified URL (`http://localhost:8080`).</span></span> <span data-ttu-id="318c3-153">При необходимости безопасности на конечную точку SSL можно реализовать.</span><span class="sxs-lookup"><span data-stu-id="318c3-153">If security is required on the endpoint, SSL can be implemented.</span></span> <span data-ttu-id="318c3-154">В разделе [как: Настройка порта с SSL-сертификат](https://msdn.microsoft.com/library/ms733791.aspx) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="318c3-154">See [How to: Configure a Port with an SSL Certificate](https://msdn.microsoft.com/library/ms733791.aspx) for more information.</span></span>
    - <span data-ttu-id="318c3-155">**При запуске**, класс, содержащий конфигурацию сервера SignalR (в этом учебнике используется только для таких конфигураций, это вызов `UseCors`) и вызов `MapSignalR`, в проекте, который создает маршруты для любых объектов концентратора.</span><span class="sxs-lookup"><span data-stu-id="318c3-155">**Startup**, the class containing the configuration for the SignalR server (the only configuration this tutorial uses is the call to `UseCors`), and the call to `MapSignalR`, which creates routes for any Hub objects in the project.</span></span>
    - <span data-ttu-id="318c3-156">**MyHub**, класс концентратора SignalR, приложение будет предоставлять клиентам.</span><span class="sxs-lookup"><span data-stu-id="318c3-156">**MyHub**, the SignalR Hub class that the application will provide to clients.</span></span> <span data-ttu-id="318c3-157">Этот класс содержит один метод, **отправки**, что клиенты будут вызывать для доставки сообщения на всех остальных подключенных клиентов.</span><span class="sxs-lookup"><span data-stu-id="318c3-157">This class has a single method, **Send**, that clients will call to broadcast a message to all other connected clients.</span></span>
6. <span data-ttu-id="318c3-158">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="318c3-158">Compile and run the application.</span></span> <span data-ttu-id="318c3-159">Адрес, на котором выполняется сервер должно отображаться в окне консоли.</span><span class="sxs-lookup"><span data-stu-id="318c3-159">The address that the server is running should show in a console window.</span></span>

    ![](tutorial-signalr-self-host/_static/image2.png)
7. <span data-ttu-id="318c3-160">Если происходит сбой выполнения с исключением `System.Reflection.TargetInvocationException was unhandled`, вам потребуется перезапустить Visual Studio с правами администратора.</span><span class="sxs-lookup"><span data-stu-id="318c3-160">If execution fails with the exception `System.Reflection.TargetInvocationException was unhandled`, you will need to restart Visual Studio with administrator privileges.</span></span>
8. <span data-ttu-id="318c3-161">Остановите приложение перед переходом к следующему разделу.</span><span class="sxs-lookup"><span data-stu-id="318c3-161">Stop the application before proceeding to the next section.</span></span>

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a><span data-ttu-id="318c3-162">Доступ к серверу с помощью клиента JavaScript</span><span class="sxs-lookup"><span data-stu-id="318c3-162">Accessing the server with a JavaScript client</span></span>

<span data-ttu-id="318c3-163">В этом разделе будет использование одного клиента JavaScript из [учебник по началу работы](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="318c3-163">In this section, you'll use the same JavaScript client from the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="318c3-164">Мы выполним только одно изменение на клиенте, который является явно задать URL-адрес концентратора.</span><span class="sxs-lookup"><span data-stu-id="318c3-164">We'll only make one modification to the client, which is to explicitly define the hub URL.</span></span> <span data-ttu-id="318c3-165">С резидентного приложения сервер может не обязательно на тот же адрес, URL-адрес подключения (из-за обратных прокси-серверов и подсистемы балансировки нагрузки), поэтому URL-адрес, необходимо явно определенных.</span><span class="sxs-lookup"><span data-stu-id="318c3-165">With a self-hosted application, the server may not necessarily be at the same address as the connection URL (due to reverse proxies and load balancers), so the URL needs to be defined explicitly.</span></span>

1. <span data-ttu-id="318c3-166">В **обозревателе решений**, щелкните правой кнопкой мыши решение и выберите **добавить**, **новый проект**.</span><span class="sxs-lookup"><span data-stu-id="318c3-166">In **Solution Explorer**, right-click on the solution and select **Add**, **New Project**.</span></span> <span data-ttu-id="318c3-167">Выберите **Web** , а затем установите **веб-приложение ASP.NET** шаблона.</span><span class="sxs-lookup"><span data-stu-id="318c3-167">Select the **Web** node, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="318c3-168">Назовите проект «JavascriptClient» и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="318c3-168">Name the project "JavascriptClient" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image3.png)
2. <span data-ttu-id="318c3-169">Выберите **пустой** шаблона и оставляет, остальные параметры не выбрано.</span><span class="sxs-lookup"><span data-stu-id="318c3-169">Select the **Empty** template, and leave the remaining options unselected.</span></span> <span data-ttu-id="318c3-170">Выберите **создания проекта**.</span><span class="sxs-lookup"><span data-stu-id="318c3-170">Select **Create Project**.</span></span>

    ![](tutorial-signalr-self-host/_static/image4.png)
3. <span data-ttu-id="318c3-171">В консоли диспетчера пакетов, выберите проект «JavascriptClient» в **проекта по умолчанию** раскрывающийся список и выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="318c3-171">In the package manager console, select the "JavascriptClient" project in the **Default project** drop-down, and execute the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    <span data-ttu-id="318c3-172">Эта команда устанавливает SignalR и JQuery библиотек, которые нужно в клиенте.</span><span class="sxs-lookup"><span data-stu-id="318c3-172">This command installs the SignalR and JQuery libraries that you'll need in the client.</span></span>
4. <span data-ttu-id="318c3-173">Щелкните правой кнопкой мыши проект и выберите пункт **добавить**, **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="318c3-173">Right-click on your project and select **Add**, **New Item**.</span></span> <span data-ttu-id="318c3-174">Выберите **Web** узел и выберите HTML-страницу.</span><span class="sxs-lookup"><span data-stu-id="318c3-174">Select the **Web** node, and select HTML Page.</span></span> <span data-ttu-id="318c3-175">Назовите страницу **Default.html**.</span><span class="sxs-lookup"><span data-stu-id="318c3-175">Name the page **Default.html**.</span></span>

    ![](tutorial-signalr-self-host/_static/image5.png)
5. <span data-ttu-id="318c3-176">Замените содержимое нового HTML-страницы с помощью следующего кода.</span><span class="sxs-lookup"><span data-stu-id="318c3-176">Replace the contents of the new HTML page with the following code.</span></span> <span data-ttu-id="318c3-177">Проверьте соответствие здесь ссылки на скрипты в папке «Сценарии» проекта сценариев.</span><span class="sxs-lookup"><span data-stu-id="318c3-177">Verify that the script references here match the scripts in the Scripts folder of the project.</span></span>

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    <span data-ttu-id="318c3-178">Следующий код (выделены в приведенном выше примере) является добавление, заданные для клиента, используемая в этом учебнике начало Stared (в дополнение к обновлению кода для SignalR бета-версия 2).</span><span class="sxs-lookup"><span data-stu-id="318c3-178">The following code (highlighted in the code sample above) is the addition that you've made to the client used in the Getting Stared tutorial (in addition to upgrading the code to SignalR version 2 beta).</span></span> <span data-ttu-id="318c3-179">Эта строка кода явно задает URL-адрес базового подключения для SignalR на сервере.</span><span class="sxs-lookup"><span data-stu-id="318c3-179">This line of code explicitly sets the base connection URL for SignalR on the server.</span></span>

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. <span data-ttu-id="318c3-180">Щелкните правой кнопкой мыши решение и выберите **назначить запускаемые проекты...** . Выберите **несколько запускаемых проектов** переключатель и задайте обоим проектам **действия** для **запустить**.</span><span class="sxs-lookup"><span data-stu-id="318c3-180">Right-click on the solution, and select **Set Startup Projects...**. Select the **Multiple startup projects** radio button, and set both projects' **Action** to **Start**.</span></span>

    ![](tutorial-signalr-self-host/_static/image6.png)
7. <span data-ttu-id="318c3-181">Щелкните правой кнопкой мыши «Default.html» и выберите **задать в качестве начальной страницы**.</span><span class="sxs-lookup"><span data-stu-id="318c3-181">Right-click on "Default.html" and select **Set As Start Page**.</span></span>
8. <span data-ttu-id="318c3-182">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="318c3-182">Run the application.</span></span> <span data-ttu-id="318c3-183">Будет запущен сервер и страницы.</span><span class="sxs-lookup"><span data-stu-id="318c3-183">The server and page will launch.</span></span> <span data-ttu-id="318c3-184">Возможно, потребуется перезагрузка веб-страницы (или выберите **Продолжить** в отладчике) при загрузке страницы до запуска сервера.</span><span class="sxs-lookup"><span data-stu-id="318c3-184">You may need to reload the web page (or select **Continue** in the debugger) if the page loads before the server is started.</span></span>
9. <span data-ttu-id="318c3-185">В браузере введите имя пользователя, при появлении запроса.</span><span class="sxs-lookup"><span data-stu-id="318c3-185">In the browser, provide a username when prompted.</span></span> <span data-ttu-id="318c3-186">Скопируйте URL-адрес страницы в другой вкладке или окне обозревателя и укажите другое имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="318c3-186">Copy the page's URL into another browser tab or window and provide a different username.</span></span> <span data-ttu-id="318c3-187">Можно отправлять сообщения из одной области в другую, как показано в учебнике Приступая к работе.</span><span class="sxs-lookup"><span data-stu-id="318c3-187">You will be able to send messages from one browser pane to the other, as in the Getting Started tutorial.</span></span>
