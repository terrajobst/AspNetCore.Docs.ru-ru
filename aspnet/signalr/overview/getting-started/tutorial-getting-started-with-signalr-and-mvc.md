---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: "Учебник: Приступая к работе с SignalR 2 и MVC 5 | Документы Microsoft"
author: pfletcher
description: "Этого учебника показано, как использовать ASP.NET SignalR 2 для создания приложения разговора в режиме реального времени. Будет добавить SignalR в приложение MVC 5 и создание представления \"Разговор\"..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.openlocfilehash: 96a6f6446b26d96b2bcffe4354375ab9c444bbbb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-getting-started-with-signalr-2-and-mvc-5"></a><span data-ttu-id="31cba-104">Учебник: Приступая к работе с SignalR 2 и MVC 5</span><span class="sxs-lookup"><span data-stu-id="31cba-104">Tutorial: Getting Started with SignalR 2 and MVC 5</span></span>
====================
<span data-ttu-id="31cba-105">по [Флетчера Патрик](https://github.com/pfletcher), [Тим Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="31cba-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

[<span data-ttu-id="31cba-106">Загрузка завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="31cba-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

> <span data-ttu-id="31cba-107">Этого учебника показано, как использовать ASP.NET SignalR 2 для создания приложения разговора в режиме реального времени.</span><span class="sxs-lookup"><span data-stu-id="31cba-107">This tutorial shows how to use ASP.NET SignalR 2 to create a real-time chat application.</span></span> <span data-ttu-id="31cba-108">Будет добавить SignalR в приложение MVC 5 и создание представления чат для отправки и отображения сообщений.</span><span class="sxs-lookup"><span data-stu-id="31cba-108">You will add SignalR to an MVC 5 application and create a chat view to send and display messages.</span></span> 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="31cba-109">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="31cba-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="31cba-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="31cba-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="31cba-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="31cba-111">.NET 4.5</span></span>
> - <span data-ttu-id="31cba-112">MVC 5</span><span class="sxs-lookup"><span data-stu-id="31cba-112">MVC 5</span></span>
> - <span data-ttu-id="31cba-113">SignalR версии 2</span><span class="sxs-lookup"><span data-stu-id="31cba-113">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="31cba-114">С помощью Visual Studio 2012 с помощью этого учебника</span><span class="sxs-lookup"><span data-stu-id="31cba-114">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="31cba-115">Чтобы использовать Visual Studio 2012 с помощью этого учебника, выполните следующее:</span><span class="sxs-lookup"><span data-stu-id="31cba-115">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="31cba-116">Обновление вашего [диспетчера пакетов](http://docs.nuget.org/docs/start-here/installing-nuget) до последней версии.</span><span class="sxs-lookup"><span data-stu-id="31cba-116">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="31cba-117">Установка [веб-установщика платформы](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="31cba-117">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="31cba-118">Установщик веб-платформы для поиска и установки **ASP.NET и 2013.1 Web Tools для Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="31cba-118">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="31cba-119">Это будет установки Visual Studio шаблоны для SignalR классов, таких как **концентратора**.</span><span class="sxs-lookup"><span data-stu-id="31cba-119">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="31cba-120">Некоторые шаблоны (такие как **класс запуска OWIN**) будет недоступен; в этом случае используйте файл класса.</span><span class="sxs-lookup"><span data-stu-id="31cba-120">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="31cba-121">Версии учебника</span><span class="sxs-lookup"><span data-stu-id="31cba-121">Tutorial Versions</span></span>
> 
> <span data-ttu-id="31cba-122">Сведения о более ранних версий SignalR см. в разделе [более ранних версий SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="31cba-122">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="31cba-123">Вопросы и комментарии</span><span class="sxs-lookup"><span data-stu-id="31cba-123">Questions and comments</span></span>
> 
> <span data-ttu-id="31cba-124">Оставьте отзыв на том, как вам понравилось этого учебника и что можно улучшить в комментарии в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="31cba-124">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="31cba-125">Если у вас есть вопросы, которые не связаны непосредственно для работы с учебником, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="31cba-125">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="31cba-126">Обзор</span><span class="sxs-lookup"><span data-stu-id="31cba-126">Overview</span></span>

<span data-ttu-id="31cba-127">Этот учебник содержит описание разработки в реальном времени веб-приложений с помощью ASP.NET SignalR 2 и ASP.NET MVC 5.</span><span class="sxs-lookup"><span data-stu-id="31cba-127">This tutorial introduces you to real-time web application development with ASP.NET SignalR 2 and ASP.NET MVC 5.</span></span> <span data-ttu-id="31cba-128">Учебник использует тот же код приложения разговора как [SignalR начало учебника](tutorial-getting-started-with-signalr.md), но показано, как добавить его в приложение MVC 5.</span><span class="sxs-lookup"><span data-stu-id="31cba-128">The tutorial uses the same chat application code as the [SignalR Getting Started tutorial](tutorial-getting-started-with-signalr.md), but shows how to add it to an MVC 5 application.</span></span>

<span data-ttu-id="31cba-129">В этом разделе вы узнаете, как следующие задачи разработки SignalR.</span><span class="sxs-lookup"><span data-stu-id="31cba-129">In this topic you will learn the following SignalR development tasks:</span></span>

- <span data-ttu-id="31cba-130">Добавление библиотеки SignalR в приложение MVC 5.</span><span class="sxs-lookup"><span data-stu-id="31cba-130">Adding the SignalR library to an MVC 5 application.</span></span>
- <span data-ttu-id="31cba-131">Создание концентратора и классы запуска OWIN для передачи содержимого клиентам.</span><span class="sxs-lookup"><span data-stu-id="31cba-131">Creating hub and OWIN startup classes to push content to clients.</span></span>
- <span data-ttu-id="31cba-132">С помощью библиотеки jQuery SignalR на веб-странице для отправки сообщений и отобразить обновления от концентратора.</span><span class="sxs-lookup"><span data-stu-id="31cba-132">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="31cba-133">На следующем снимке экрана показано завершенного чата приложение в браузере.</span><span class="sxs-lookup"><span data-stu-id="31cba-133">The following screen shot shows the completed chat application running in a browser.</span></span>

![Экземпляры чата](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

<span data-ttu-id="31cba-135">Разделы:</span><span class="sxs-lookup"><span data-stu-id="31cba-135">Sections:</span></span>

- [<span data-ttu-id="31cba-136">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="31cba-136">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="31cba-137">Выполнение образца</span><span class="sxs-lookup"><span data-stu-id="31cba-137">Run the Sample</span></span>](#run)
- [<span data-ttu-id="31cba-138">Анализ кода</span><span class="sxs-lookup"><span data-stu-id="31cba-138">Examine the Code</span></span>](#code)
- [<span data-ttu-id="31cba-139">Дальнейшие действия</span><span class="sxs-lookup"><span data-stu-id="31cba-139">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="31cba-140">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="31cba-140">Set up the Project</span></span>

<span data-ttu-id="31cba-141">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="31cba-141">Prerequisites:</span></span>

- <span data-ttu-id="31cba-142">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="31cba-142">Visual Studio 2013.</span></span> <span data-ttu-id="31cba-143">Если у вас Visual Studio, см. раздел [загрузок ASP.NET](https://www.asp.net/downloads) для получения бесплатной Visual Studio 2013 Express средство разработки.</span><span class="sxs-lookup"><span data-stu-id="31cba-143">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2013 Express Development Tool.</span></span>

<span data-ttu-id="31cba-144">В этом разделе показано, как создать приложение ASP.NET MVC 5, добавить библиотеку SignalR и создать приложения разговора.</span><span class="sxs-lookup"><span data-stu-id="31cba-144">This section shows how to create an ASP.NET MVC 5 application, add the SignalR library, and create the chat application.</span></span>

1. <span data-ttu-id="31cba-145">В Visual Studio создайте приложение C# ASP.NET, ориентированном на .NET Framework 4.5, присвойте ей имя SignalRChat и нажмите кнопку ОК.</span><span class="sxs-lookup"><span data-stu-id="31cba-145">In Visual Studio, create a C# ASP.NET application that targets .NET Framework 4.5, name it SignalRChat, and click OK.</span></span>

    ![Создание веб-](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)
2. <span data-ttu-id="31cba-147">В `New ASP.NET Project` диалогового окна, а затем выберите **MVC**и нажмите кнопку **изменить аутентификацию**.</span><span class="sxs-lookup"><span data-stu-id="31cba-147">In the `New ASP.NET Project` dialog, and select **MVC**, and click **Change Authentication**.</span></span>

    ![Создание веб-](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)
3. <span data-ttu-id="31cba-149">Выберите **без проверки подлинности** в **изменить аутентификацию** окна и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="31cba-149">Select **No Authentication** in the **Change Authentication** dialog, and click **OK**.</span></span>

    ![Выберите без проверки подлинности](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

    > [!NOTE]
    > <span data-ttu-id="31cba-151">При выборе другого поставщика проверки подлинности для приложения, `Startup.cs` класса будут создаваться автоматически; не потребуется создавать собственные `Startup.cs` класса на шаге 10 ниже.</span><span class="sxs-lookup"><span data-stu-id="31cba-151">If you select a different authentication provider for your application, a `Startup.cs` class will be created for you; you will not need to create your own `Startup.cs` class in step 10 below.</span></span>
4. <span data-ttu-id="31cba-152">Нажмите кнопку **ОК** в **новый проект ASP.NET** диалогового окна.</span><span class="sxs-lookup"><span data-stu-id="31cba-152">Click **OK** in the **New ASP.NET Project** dialog.</span></span>
5. <span data-ttu-id="31cba-153">Откройте **инструменты | Диспетчер пакетов библиотеки | Консоль диспетчера пакетов** и выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="31cba-153">Open the **Tools | Library Package Manager | Package Manager Console** and run the following command.</span></span> <span data-ttu-id="31cba-154">Этот шаг добавляет в проект набор файлов сценариев и ссылки на сборки, включить функцию SignalR.</span><span class="sxs-lookup"><span data-stu-id="31cba-154">This step adds to the project a set of script files and assembly references that enable SignalR functionality.</span></span>

    `install-package Microsoft.AspNet.SignalR`
6. <span data-ttu-id="31cba-155">В **обозревателе решений**, разверните папку «скрипты».</span><span class="sxs-lookup"><span data-stu-id="31cba-155">In **Solution Explorer**, expand the Scripts folder.</span></span> <span data-ttu-id="31cba-156">Обратите внимание, что библиотеки скриптов для SignalR были добавлены в проект.</span><span class="sxs-lookup"><span data-stu-id="31cba-156">Note that script libraries for SignalR have been added to the project.</span></span>

    ![Папка скриптов](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)
7. <span data-ttu-id="31cba-158">В **обозревателе решений**, щелкните правой кнопкой мыши проект, выберите **добавить | Новая папка**, и добавить новую папку с именем **концентраторов**.</span><span class="sxs-lookup"><span data-stu-id="31cba-158">In **Solution Explorer**, right-click the project, select **Add | New Folder**, and add a new folder named **Hubs**.</span></span>
8. <span data-ttu-id="31cba-159">Щелкните правой кнопкой мыши **концентраторов** папку, нажмите кнопку **добавить | Новый элемент**выберите **Visual C# | Web | SignalR** узел в **установленные** выберите **класс концентратора SignalR (v2)** из центральной области и создать новый узел с именем **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="31cba-159">Right-click the **Hubs** folder, click **Add | New Item**, select the **Visual C# | Web | SignalR** node in the **Installed** pane, select **SignalR Hub Class (v2)** from the center pane, and create a new hub named **ChatHub.cs**.</span></span> <span data-ttu-id="31cba-160">Этот класс будет использоваться как к концентратору SignalR сервера, который отправляет сообщения для всех клиентов.</span><span class="sxs-lookup"><span data-stu-id="31cba-160">You will use this class as a SignalR server hub that sends messages to all clients.</span></span>

    ![Создание нового концентратора](tutorial-getting-started-with-signalr-and-mvc/_static/image6.png)
9. <span data-ttu-id="31cba-162">Замените код в **ChatHub** класса следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="31cba-162">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]
10. <span data-ttu-id="31cba-163">Создайте новый класс с именем файла Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="31cba-163">Create a new class called Startup.cs.</span></span> <span data-ttu-id="31cba-164">Измените содержимое файла следующим образом.</span><span class="sxs-lookup"><span data-stu-id="31cba-164">Change the contents of the file to the following.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]
11. <span data-ttu-id="31cba-165">Изменить `HomeController` класс найден в **Controllers/HomeController.cs** и добавьте следующий метод к классу.</span><span class="sxs-lookup"><span data-stu-id="31cba-165">Edit the `HomeController` class found in **Controllers/HomeController.cs** and add the following method to the class.</span></span> <span data-ttu-id="31cba-166">Этот метод возвращает **чат** представление, в котором будут созданы в более позднем этапе.</span><span class="sxs-lookup"><span data-stu-id="31cba-166">This method returns the **Chat** view that you will create in a later step.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]
12. <span data-ttu-id="31cba-167">Щелкните правой кнопкой мыши **представления/домашние** папки, а затем выберите **добавить... | Представление**.</span><span class="sxs-lookup"><span data-stu-id="31cba-167">Right-click the **Views/Home** folder, and select **Add... | View**.</span></span>
13. <span data-ttu-id="31cba-168">В **добавить представление** диалога, имя нового представления **чат**.</span><span class="sxs-lookup"><span data-stu-id="31cba-168">In the **Add View** dialog, name the new view **Chat**.</span></span>

    ![Добавление представления](tutorial-getting-started-with-signalr-and-mvc/_static/image7.png)
14. <span data-ttu-id="31cba-170">Замените содержимое **Chat.cshtml** следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="31cba-170">Replace the contents of **Chat.cshtml** with the following code.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="31cba-171">При добавлении SignalR и другие библиотеки скриптов в проект Visual Studio, диспетчер пакетов может установить версию файла сценария SignalR, который имеет более новую версию, приведенные в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="31cba-171">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install a version of the SignalR script file that is more recent than the version shown in this topic.</span></span> <span data-ttu-id="31cba-172">Убедитесь, что ссылку на скрипт в коде соответствует версии библиотеки скриптов, установленные в проекте.</span><span class="sxs-lookup"><span data-stu-id="31cba-172">Make sure that the script reference in your code matches the version of the script library installed in your project.</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]
15. <span data-ttu-id="31cba-173">**Сохранить все** для проекта.</span><span class="sxs-lookup"><span data-stu-id="31cba-173">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="31cba-174">Выполнение образца</span><span class="sxs-lookup"><span data-stu-id="31cba-174">Run the Sample</span></span>

1. <span data-ttu-id="31cba-175">Нажмите клавишу F5 для запуска проекта в режиме отладки.</span><span class="sxs-lookup"><span data-stu-id="31cba-175">Press F5 to run the project in debug mode.</span></span>
2. <span data-ttu-id="31cba-176">В адресной строке браузера добавьте **/home/чата** URL-адрес страницы по умолчанию для проекта.</span><span class="sxs-lookup"><span data-stu-id="31cba-176">In the browser address line, append **/home/chat** to the URL of the default page for the project.</span></span> <span data-ttu-id="31cba-177">Страница чата загружается в экземпляре браузера и предлагает ввести имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="31cba-177">The Chat page loads in a browser instance and prompts for a user name.</span></span>

    ![Введите имя пользователя](tutorial-getting-started-with-signalr-and-mvc/_static/image8.png)
3. <span data-ttu-id="31cba-179">Введите имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="31cba-179">Enter a user name.</span></span>
4. <span data-ttu-id="31cba-180">Копировать URL-адрес из адресной строки браузера и использовать ее для открытия две дополнительные экземпляры браузера.</span><span class="sxs-lookup"><span data-stu-id="31cba-180">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="31cba-181">В каждом экземпляре браузера введите уникальное имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="31cba-181">In each browser instance, enter a unique user name.</span></span>
5. <span data-ttu-id="31cba-182">В каждом экземпляре браузера, добавьте комментарий и щелкните **отправки**.</span><span class="sxs-lookup"><span data-stu-id="31cba-182">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="31cba-183">Комментарии должны отображаться в все экземпляры браузера.</span><span class="sxs-lookup"><span data-stu-id="31cba-183">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="31cba-184">Это приложение на простом чате; не поддерживает контекст обсуждения на сервере.</span><span class="sxs-lookup"><span data-stu-id="31cba-184">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="31cba-185">Концентратор осуществляет широковещательную рассылку комментариев для всех текущих пользователей.</span><span class="sxs-lookup"><span data-stu-id="31cba-185">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="31cba-186">Пользователей, которые позже принять участие в обсуждении будут отображены сообщения, добавлены с момента их соединения.</span><span class="sxs-lookup"><span data-stu-id="31cba-186">Users who join the chat later will see messages added from the time they join.</span></span>
6. <span data-ttu-id="31cba-187">На следующем снимке экрана показано чата приложение в браузере.</span><span class="sxs-lookup"><span data-stu-id="31cba-187">The following screen shot shows the chat application running in a browser.</span></span>

    ![Браузеры чата](tutorial-getting-started-with-signalr-and-mvc/_static/image9.png)
7. <span data-ttu-id="31cba-189">В **обозревателе решений**, проверять **документов скрипта** узел для запущенного приложения.</span><span class="sxs-lookup"><span data-stu-id="31cba-189">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="31cba-190">При использовании Internet Explorer как браузер, этот узел отображается в режиме отладки.</span><span class="sxs-lookup"><span data-stu-id="31cba-190">This node is visible in debug mode if you are using Internet Explorer as your browser.</span></span> <span data-ttu-id="31cba-191">Отсутствует файл скрипта с именем **концентраторов** , библиотека SignalR динамически приводит к возникновению ошибки во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="31cba-191">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="31cba-192">Этот файл обеспечивает связь между jQuery сценарий и код на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="31cba-192">This file manages the communication between jQuery script and server-side code.</span></span> <span data-ttu-id="31cba-193">Если используется браузер, отличный от Internet Explorer, также можно использовать динамический **концентраторов** файла, перейдя к нему напрямую, например http://mywebsite/signalr/hubs.</span><span class="sxs-lookup"><span data-stu-id="31cba-193">If you use a browser other than Internet Explorer, you can also access the dynamic **hubs** file by browsing to it directly, for example http://mywebsite/signalr/hubs.</span></span>

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="31cba-194">Анализ кода</span><span class="sxs-lookup"><span data-stu-id="31cba-194">Examine the Code</span></span>

<span data-ttu-id="31cba-195">Приложение разговора SignalR демонстрирует две основные задачи разработки SignalR: создание концентратора как объект основного координации на сервере, а с помощью библиотеки jQuery SignalR для отправки и получения сообщений.</span><span class="sxs-lookup"><span data-stu-id="31cba-195">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="31cba-196">Концентраторы SignalR</span><span class="sxs-lookup"><span data-stu-id="31cba-196">SignalR Hubs</span></span>

<span data-ttu-id="31cba-197">В следующем образце кода **ChatHub** класс является производным от **Microsoft.AspNet.SignalR.Hub** класса.</span><span class="sxs-lookup"><span data-stu-id="31cba-197">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="31cba-198">Наследование от **концентратора** класс является удобным способом для построения приложения SignalR.</span><span class="sxs-lookup"><span data-stu-id="31cba-198">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="31cba-199">Можно создавать открытые методы в классе концентратора и откройте эти методы, вызывать их из скриптов на веб-странице.</span><span class="sxs-lookup"><span data-stu-id="31cba-199">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="31cba-200">В коде чата клиенты вызывают **ChatHub.Send** метод для отправки сообщения.</span><span class="sxs-lookup"><span data-stu-id="31cba-200">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="31cba-201">Концентратор в свою очередь отправляет сообщение всем клиентам, вызвав **Clients.All.addNewMessageToPage**.</span><span class="sxs-lookup"><span data-stu-id="31cba-201">The hub in turn sends the message to all clients by calling **Clients.All.addNewMessageToPage**.</span></span>

<span data-ttu-id="31cba-202">**Отправки** метод показывает некоторые понятия концентратора:</span><span class="sxs-lookup"><span data-stu-id="31cba-202">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="31cba-203">Объявите открытых методов концентратора, чтобы их можно было вызывать клиенты.</span><span class="sxs-lookup"><span data-stu-id="31cba-203">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="31cba-204">Используйте **Microsoft.AspNet.SignalR.Hub.Clients** свойство для доступа к все клиенты подключены этого концентратора.</span><span class="sxs-lookup"><span data-stu-id="31cba-204">Use the **Microsoft.AspNet.SignalR.Hub.Clients** property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="31cba-205">Вызов функции на стороне клиента (такие как `addNewMessageToPage` функции) для обновления клиентов.</span><span class="sxs-lookup"><span data-stu-id="31cba-205">Call a function on the client (such as the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="31cba-206">SignalR и jQuery</span><span class="sxs-lookup"><span data-stu-id="31cba-206">SignalR and jQuery</span></span>

<span data-ttu-id="31cba-207">**Chat.cshtml** файл представления в следующем образце кода показано, как использовать библиотеку jQuery SignalR для связи с концентратора SignalR.</span><span class="sxs-lookup"><span data-stu-id="31cba-207">The **Chat.cshtml** view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="31cba-208">Основные задачи в коде создается ссылка автоматически созданный прокси-сервер для концентратора, объявление функции, сервер может вызывать для push-содержимое для клиентов и подключения для отправки сообщений в концентратор.</span><span class="sxs-lookup"><span data-stu-id="31cba-208">The essential tasks in the code are creating a reference to the auto-generated proxy for the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="31cba-209">В следующем коде объявляется ссылку на прокси-сервера концентратора.</span><span class="sxs-lookup"><span data-stu-id="31cba-209">The following code declares a reference to a hub proxy.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="31cba-210">В JavaScript ссылку на класс server и его членах находится в верхний регистр.</span><span class="sxs-lookup"><span data-stu-id="31cba-210">In JavaScript the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="31cba-211">Ссылается на образец кода C# **ChatHub** класса в JavaScript как **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="31cba-211">The code sample references the C# **ChatHub** class in JavaScript as **chatHub**.</span></span> <span data-ttu-id="31cba-212">Если требуется сослаться `ChatHub` класса в jQuery с обычной Pascal регистр, как и в C#, измените файл класса ChatHub.cs.</span><span class="sxs-lookup"><span data-stu-id="31cba-212">If you want to reference the `ChatHub` class in jQuery with conventional Pascal casing as you would in C#, edit the ChatHub.cs class file.</span></span> <span data-ttu-id="31cba-213">Добавить `using` инструкцию, чтобы ссылаться на `Microsoft.AspNet.SignalR.Hubs` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="31cba-213">Add a `using` statement to reference the `Microsoft.AspNet.SignalR.Hubs` namespace.</span></span> <span data-ttu-id="31cba-214">Затем добавьте `HubName` атрибут `ChatHub` классов, например `[HubName("ChatHub")]`.</span><span class="sxs-lookup"><span data-stu-id="31cba-214">Then add the `HubName` attribute to the `ChatHub` class, for example `[HubName("ChatHub")]`.</span></span> <span data-ttu-id="31cba-215">Наконец, обновите jQuery справочной информации для `ChatHub` класса.</span><span class="sxs-lookup"><span data-stu-id="31cba-215">Finally, update your jQuery reference to the `ChatHub` class.</span></span>


<span data-ttu-id="31cba-216">Следующий код показано, как создать функцию обратного вызова в скрипте.</span><span class="sxs-lookup"><span data-stu-id="31cba-216">The following code shows how to create a callback function in the script.</span></span> <span data-ttu-id="31cba-217">Класс концентратора на сервере вызывает эту функцию для принудительной отправки обновления содержимого для каждого клиента.</span><span class="sxs-lookup"><span data-stu-id="31cba-217">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="31cba-218">Дополнительный вызов `htmlEncode` функция отображает способ HTML кодирования содержимого сообщения перед его отображением на странице, чтобы предотвратить внедрение скрипта.</span><span class="sxs-lookup"><span data-stu-id="31cba-218">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page, as a way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

<span data-ttu-id="31cba-219">Ниже показано, как открыть соединение с концентратором.</span><span class="sxs-lookup"><span data-stu-id="31cba-219">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="31cba-220">Код запускает подключение, а затем передает его функцию для обработки события щелчка кнопкой мыши на **отправки** кнопки на странице чат.</span><span class="sxs-lookup"><span data-stu-id="31cba-220">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

> [!NOTE]
> <span data-ttu-id="31cba-221">Такой подход гарантирует, что соединение установлено до выполнения обработчика событий.</span><span class="sxs-lookup"><span data-stu-id="31cba-221">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="31cba-222">Дальнейшие действия</span><span class="sxs-lookup"><span data-stu-id="31cba-222">Next Steps</span></span>

<span data-ttu-id="31cba-223">Вы узнали, что SignalR — это платформа для построения в режиме реального времени веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="31cba-223">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="31cba-224">Вы также узнали несколько задач разработки SignalR: Добавление приложения ASP.NET SignalR, как создать класс концентратора и как отправлять и получать сообщения от концентратора.</span><span class="sxs-lookup"><span data-stu-id="31cba-224">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="31cba-225">Пошаговое руководство по развертыванию SignalR примера приложения в Azure см. в разделе [SignalR с помощью с веб-приложений в службе приложений Azure](../deployment/using-signalr-with-azure-web-sites.md).</span><span class="sxs-lookup"><span data-stu-id="31cba-225">For a walkthrough on how to deploy the sample SignalR application to Azure, see [Using SignalR with Web Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span></span> <span data-ttu-id="31cba-226">Подробные сведения о развертывании веб-проекта Visual Studio для веб-сайта Windows Azure см. в разделе [создать веб-приложение ASP.NET в службе приложений Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="31cba-226">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-get-started/).</span></span>

<span data-ttu-id="31cba-227">Для получения более сложных понятиях новинках SignalR, посетите следующие сайты для SignalR исходный код и ресурсы:</span><span class="sxs-lookup"><span data-stu-id="31cba-227">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources :</span></span>

- [<span data-ttu-id="31cba-228">Проект SignalR</span><span class="sxs-lookup"><span data-stu-id="31cba-228">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="31cba-229">SignalR Github и образцы</span><span class="sxs-lookup"><span data-stu-id="31cba-229">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="31cba-230">Вики-сайте SignalR</span><span class="sxs-lookup"><span data-stu-id="31cba-230">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
