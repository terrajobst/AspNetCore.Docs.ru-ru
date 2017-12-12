---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: "Учебник: Приступая к работе с SignalR 1.x и MVC 4 | Документы Microsoft"
author: pfletcher
description: "Используйте ASP.NET SignalR и ASP.NET MVC 4 для построения приложения разговора в режиме реального времени."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/29/2013
ms.topic: article
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: e678c85520613fea2a8d00de60aca04d895d6307
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a><span data-ttu-id="93070-103">Учебник: Приступая к работе с SignalR 1.x и MVC 4</span><span class="sxs-lookup"><span data-stu-id="93070-103">Tutorial: Getting Started with SignalR 1.x and MVC 4</span></span>
====================
<span data-ttu-id="93070-104">по [Флетчера Патрик](https://github.com/pfletcher), [Тим Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="93070-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

> <span data-ttu-id="93070-105">Этого учебника показано, как использовать ASP.NET SignalR для создания приложения разговора в режиме реального времени.</span><span class="sxs-lookup"><span data-stu-id="93070-105">This tutorial shows how to use ASP.NET SignalR to create a real-time chat application.</span></span> <span data-ttu-id="93070-106">Будет добавить SignalR в приложение MVC 4 и создание представления чат для отправки и отображения сообщений.</span><span class="sxs-lookup"><span data-stu-id="93070-106">You will add SignalR to an MVC 4 application and create a chat view to send and display messages.</span></span>


## <a name="overview"></a><span data-ttu-id="93070-107">Обзор</span><span class="sxs-lookup"><span data-stu-id="93070-107">Overview</span></span>

<span data-ttu-id="93070-108">Этот учебник содержит описание разработки в реальном времени веб-приложений с ASP.NET SignalR и ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="93070-108">This tutorial introduces you to real-time web application development with ASP.NET SignalR and ASP.NET MVC 4.</span></span> <span data-ttu-id="93070-109">Учебник использует тот же код приложения разговора как [учебника SignalR Приступая к работе](tutorial-getting-started-with-signalr.md), но показано, как добавить его в приложение MVC 4, основанных на шаблоне Интернет.</span><span class="sxs-lookup"><span data-stu-id="93070-109">The tutorial uses the same chat application code as the [SignalR Getting Started tutorial](tutorial-getting-started-with-signalr.md), but shows how to add it to an MVC 4 application based on the Internet template.</span></span>

<span data-ttu-id="93070-110">В этом разделе вы узнаете, как следующие задачи разработки SignalR.</span><span class="sxs-lookup"><span data-stu-id="93070-110">In this topic you will learn the following SignalR development tasks:</span></span>

- <span data-ttu-id="93070-111">Добавление библиотеки SignalR в приложение MVC 4.</span><span class="sxs-lookup"><span data-stu-id="93070-111">Adding the SignalR library to an MVC 4 application.</span></span>
- <span data-ttu-id="93070-112">Создание класса для передачи содержимого клиентам концентратора.</span><span class="sxs-lookup"><span data-stu-id="93070-112">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="93070-113">С помощью библиотеки jQuery SignalR на веб-странице для отправки сообщений и отобразить обновления от концентратора.</span><span class="sxs-lookup"><span data-stu-id="93070-113">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="93070-114">На следующем снимке экрана показано завершенного чата приложение в браузере.</span><span class="sxs-lookup"><span data-stu-id="93070-114">The following screen shot shows the completed chat application running in a browser.</span></span>

![Экземпляры чата](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

<span data-ttu-id="93070-116">Разделы:</span><span class="sxs-lookup"><span data-stu-id="93070-116">Sections:</span></span>

- [<span data-ttu-id="93070-117">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="93070-117">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="93070-118">Выполнение образца</span><span class="sxs-lookup"><span data-stu-id="93070-118">Run the Sample</span></span>](#run)
- [<span data-ttu-id="93070-119">Анализ кода</span><span class="sxs-lookup"><span data-stu-id="93070-119">Examine the Code</span></span>](#code)
- [<span data-ttu-id="93070-120">Дальнейшие действия</span><span class="sxs-lookup"><span data-stu-id="93070-120">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="93070-121">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="93070-121">Set up the Project</span></span>

<span data-ttu-id="93070-122">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="93070-122">Prerequisites:</span></span>

- <span data-ttu-id="93070-123">С пакетом обновления 1 для Visual Studio 2010, Visual Studio 2012 или Visual Studio 2012 Express.</span><span class="sxs-lookup"><span data-stu-id="93070-123">Visual Studio 2010 SP1, Visual Studio 2012, or Visual Studio 2012 Express.</span></span> <span data-ttu-id="93070-124">Если у вас Visual Studio, см. раздел [загрузок ASP.NET](https://www.asp.net/downloads) для получения бесплатной Visual Studio 2012 Express средство разработки.</span><span class="sxs-lookup"><span data-stu-id="93070-124">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="93070-125">Visual Studio 2010, установите [ASP.NET MVC 4](https://www.microsoft.com/en-us/download/details.aspx?id=30683).</span><span class="sxs-lookup"><span data-stu-id="93070-125">For Visual Studio 2010, install [ASP.NET MVC 4](https://www.microsoft.com/en-us/download/details.aspx?id=30683).</span></span>

<span data-ttu-id="93070-126">В этом разделе показано, как создать приложение ASP.NET MVC 4, добавить библиотеку SignalR и создать приложения разговора.</span><span class="sxs-lookup"><span data-stu-id="93070-126">This section shows how to create an ASP.NET MVC 4 application, add the SignalR library, and create the chat application.</span></span>

1. 1. <span data-ttu-id="93070-127">В Visual Studio создать приложение ASP.NET MVC 4, присвойте ей имя SignalRChat и нажмите кнопку ОК.</span><span class="sxs-lookup"><span data-stu-id="93070-127">In Visual Studio create an ASP.NET MVC 4 application, name it SignalRChat, and click OK.</span></span>

        > [!NOTE]
        > <span data-ttu-id="93070-128">В VS 2010 выберите **.NET Framework 4** в раскрывающемся списке версии Framework.</span><span class="sxs-lookup"><span data-stu-id="93070-128">In VS 2010, select **.NET Framework 4** in the Framework version dropdown control.</span></span> <span data-ttu-id="93070-129">SignalR код выполняется на версии платформы .NET Framework 4 и 4.5.</span><span class="sxs-lookup"><span data-stu-id="93070-129">SignalR code runs on .NET Framework versions 4 and 4.5.</span></span>

        ![Создание веб-mvc](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
    2. <span data-ttu-id="93070-131">Выберите шаблон веб-приложение, снимите флажок для **Создание проекта модульного теста**и нажмите кнопку ОК.</span><span class="sxs-lookup"><span data-stu-id="93070-131">Select the Internet Application template, clear the option to **Create a unit test project**, and click OK.</span></span>

        ![Создание веб-сайта mvc](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
    3. <span data-ttu-id="93070-133">Откройте **инструменты | Диспетчер пакетов библиотеки | Консоль диспетчера пакетов** и выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="93070-133">Open the **Tools | Library Package Manager | Package Manager Console** and run the following command.</span></span> <span data-ttu-id="93070-134">Этот шаг добавляет в проект набор файлов сценариев и ссылки на сборки, включить функцию SignalR.</span><span class="sxs-lookup"><span data-stu-id="93070-134">This step adds to the project a set of script files and assembly references that enable SignalR functionality.</span></span>

        `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
    4. <span data-ttu-id="93070-135">В **обозревателе решений** разверните папку «скрипты».</span><span class="sxs-lookup"><span data-stu-id="93070-135">In **Solution Explorer** expand the Scripts folder.</span></span> <span data-ttu-id="93070-136">Обратите внимание, что библиотеки скриптов для SignalR были добавлены в проект.</span><span class="sxs-lookup"><span data-stu-id="93070-136">Note that script libraries for SignalR have been added to the project.</span></span>

        ![Ссылки на библиотеки](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
    5. <span data-ttu-id="93070-138">В **обозревателе решений**, щелкните правой кнопкой мыши проект, выберите **добавить | Новая папка**, и добавить новую папку с именем **концентраторов**.</span><span class="sxs-lookup"><span data-stu-id="93070-138">In **Solution Explorer**, right-click the project, select **Add | New Folder**, and add a new folder named **Hubs**.</span></span>
    6. <span data-ttu-id="93070-139">Щелкните правой кнопкой мыши **концентраторов** папку, нажмите кнопку **добавить | Класс**и создайте новый класс C# с именем **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="93070-139">Right-click the **Hubs** folder, click **Add | Class**, and create a new C# class named **ChatHub.cs**.</span></span> <span data-ttu-id="93070-140">Этот класс будет использоваться как к концентратору SignalR сервера, который отправляет сообщения для всех клиентов.</span><span class="sxs-lookup"><span data-stu-id="93070-140">You will use this class as a SignalR server hub that sends messages to all clients.</span></span>

> [!NOTE]
> <span data-ttu-id="93070-141">Если вы используете Visual Studio 2012 и установили [обновление ASP.NET и 2012.2 средства Web](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), новый шаблон элемента SignalR можно использовать для создания класса концентратора.</span><span class="sxs-lookup"><span data-stu-id="93070-141">If you use Visual Studio 2012 and have installed the [ASP.NET and Web Tools 2012.2 update](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), you can use the new SignalR item template to create the hub class.</span></span> <span data-ttu-id="93070-142">Чтобы сделать это, щелкните правой кнопкой мыши **концентраторов** папку, нажмите кнопку **добавить | Новый элемент**выберите **класс концентратора SignalR (v1)**и имя класса **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="93070-142">To do that, right-click the **Hubs** folder, click **Add | New Item**, select **SignalR Hub Class (v1)**, and name the class **ChatHub.cs**.</span></span>


1. <span data-ttu-id="93070-143">Замените код в **ChatHub** класса следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="93070-143">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. <span data-ttu-id="93070-144">Откройте **Global.asax** файл проекта и добавьте вызов метода `RouteTable.Routes.MapHubs();` в первой строке кода в `Application_Start` метод.</span><span class="sxs-lookup"><span data-stu-id="93070-144">Open the **Global.asax** file for the project, and add a call to the method `RouteTable.Routes.MapHubs();` as the first line of code in the `Application_Start` method.</span></span> <span data-ttu-id="93070-145">Этот код регистрирует маршрут по умолчанию для концентраторов SignalR и должен вызываться перед регистрацией любые другие маршруты.</span><span class="sxs-lookup"><span data-stu-id="93070-145">This code registers the default route for SignalR hubs and must be called before you register any other routes.</span></span> <span data-ttu-id="93070-146">Завершенный `Application_Start` метод выглядит как в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="93070-146">The completed `Application_Start` method looks like the following example.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. <span data-ttu-id="93070-147">Изменить `HomeController` класс найден в **Controllers/HomeController.cs** и добавьте следующий метод к классу.</span><span class="sxs-lookup"><span data-stu-id="93070-147">Edit the `HomeController` class found in **Controllers/HomeController.cs** and add the following method to the class.</span></span> <span data-ttu-id="93070-148">Этот метод возвращает **чат** представление, в котором будут созданы в более позднем этапе.</span><span class="sxs-lookup"><span data-stu-id="93070-148">This method returns the **Chat** view that you will create in a later step.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. <span data-ttu-id="93070-149">Щелкните правой кнопкой мыши в пределах `Chat` метод только что создан и нажмите кнопку **добавить представление** для создания файла представления.</span><span class="sxs-lookup"><span data-stu-id="93070-149">Right-click within the `Chat` method you just created, and click **Add View** to create a new view file.</span></span>
5. <span data-ttu-id="93070-150">В **добавить представление** диалогового окна, убедитесь, что установлен флажок **макета или главная страница** (снимите другие флажки) и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="93070-150">In the **Add View** dialog, make sure the check box is selected to **Use a layout or master page** (clear the other check boxes), and then click **Add**.</span></span>

    ![Добавление представления](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. <span data-ttu-id="93070-152">Отредактируйте новый файл представление с именем **Chat.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="93070-152">Edit the new view file named **Chat.cshtml**.</span></span> <span data-ttu-id="93070-153">После &lt;h2&gt; тег, вставьте следующий &lt;div&gt; раздел и `@section scripts` блок кода в страницу.</span><span class="sxs-lookup"><span data-stu-id="93070-153">After the &lt;h2&gt; tag, paste the following &lt;div&gt; section and `@section scripts` code block into the page.</span></span> <span data-ttu-id="93070-154">Этот сценарий позволяет возвращать страницу для отправки сообщения и отображать сообщения с сервера.</span><span class="sxs-lookup"><span data-stu-id="93070-154">This script enables the page to send chat messages and display messages from the server.</span></span> <span data-ttu-id="93070-155">Полный код для разговора представление отображается в следующем блоке кода.</span><span class="sxs-lookup"><span data-stu-id="93070-155">The complete code for the chat view appears in the following code block.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="93070-156">При добавлении SignalR и другие библиотеки скриптов в проект Visual Studio, диспетчер пакетов может установить версии скриптов, которые новее версии, приведенные в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="93070-156">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install versions of the scripts that are more recent than the versions shown in this topic.</span></span> <span data-ttu-id="93070-157">Убедитесь, что ссылки на скрипты в коде соответствуют версии библиотеки скриптов, установленные в проекте.</span><span class="sxs-lookup"><span data-stu-id="93070-157">Make sure that the script references in your code match the versions of the script libraries installed in your project.</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. <span data-ttu-id="93070-158">**Сохранить все** для проекта.</span><span class="sxs-lookup"><span data-stu-id="93070-158">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="93070-159">Выполнение образца</span><span class="sxs-lookup"><span data-stu-id="93070-159">Run the Sample</span></span>

1. <span data-ttu-id="93070-160">Нажмите клавишу F5 для запуска проекта в режиме отладки.</span><span class="sxs-lookup"><span data-stu-id="93070-160">Press F5 to run the project in debug mode.</span></span>
2. <span data-ttu-id="93070-161">В адресной строке браузера добавьте **/home/чата** URL-адрес страницы по умолчанию для проекта.</span><span class="sxs-lookup"><span data-stu-id="93070-161">In the browser address line, append **/home/chat** to the URL of the default page for the project.</span></span> <span data-ttu-id="93070-162">Страница чата загружается в экземпляре браузера и предлагает ввести имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="93070-162">The Chat page loads in a browser instance and prompts for a user name.</span></span>

    ![Введите имя пользователя](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. <span data-ttu-id="93070-164">Введите имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="93070-164">Enter a user name.</span></span>
4. <span data-ttu-id="93070-165">Копировать URL-адрес из адресной строки браузера и использовать ее для открытия две дополнительные экземпляры браузера.</span><span class="sxs-lookup"><span data-stu-id="93070-165">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="93070-166">В каждом экземпляре браузера введите уникальное имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="93070-166">In each browser instance, enter a unique user name.</span></span>
5. <span data-ttu-id="93070-167">В каждом экземпляре браузера, добавьте комментарий и щелкните **отправки**.</span><span class="sxs-lookup"><span data-stu-id="93070-167">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="93070-168">Комментарии должны отображаться в все экземпляры браузера.</span><span class="sxs-lookup"><span data-stu-id="93070-168">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="93070-169">Это приложение на простом чате; не поддерживает контекст обсуждения на сервере.</span><span class="sxs-lookup"><span data-stu-id="93070-169">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="93070-170">Концентратор осуществляет широковещательную рассылку комментариев для всех текущих пользователей.</span><span class="sxs-lookup"><span data-stu-id="93070-170">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="93070-171">Пользователей, которые позже принять участие в обсуждении будут отображены сообщения, добавлены с момента их соединения.</span><span class="sxs-lookup"><span data-stu-id="93070-171">Users who join the chat later will see messages added from the time they join.</span></span>
6. <span data-ttu-id="93070-172">На следующем снимке экрана показано чата приложение в браузере.</span><span class="sxs-lookup"><span data-stu-id="93070-172">The following screen shot shows the chat application running in a browser.</span></span>

    ![Браузеры чата](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. <span data-ttu-id="93070-174">В **обозревателе решений**, проверять **документов скрипта** узел для запущенного приложения.</span><span class="sxs-lookup"><span data-stu-id="93070-174">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="93070-175">При использовании Internet Explorer как браузер, этот узел отображается в режиме отладки.</span><span class="sxs-lookup"><span data-stu-id="93070-175">This node is visible in debug mode if you are using Internet Explorer as your browser.</span></span> <span data-ttu-id="93070-176">Отсутствует файл скрипта с именем **концентраторов** , библиотека SignalR динамически приводит к возникновению ошибки во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="93070-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="93070-177">Этот файл обеспечивает связь между jQuery сценарий и код на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="93070-177">This file manages the communication between jQuery script and server-side code.</span></span> <span data-ttu-id="93070-178">Если используется браузер, отличный от Internet Explorer, также можно использовать динамический **концентраторов** файла, перейдя к нему напрямую, например http://mywebsite/signalr/hubs.</span><span class="sxs-lookup"><span data-stu-id="93070-178">If you use a browser other than Internet Explorer, you can also access the dynamic **hubs** file by browsing to it directly, for example http://mywebsite/signalr/hubs.</span></span>

    ![Скрипт, созданный концентратора](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="93070-180">Анализ кода</span><span class="sxs-lookup"><span data-stu-id="93070-180">Examine the Code</span></span>

<span data-ttu-id="93070-181">Приложение разговора SignalR демонстрирует две основные задачи разработки SignalR: создание концентратора как объект основного координации на сервере, а с помощью библиотеки jQuery SignalR для отправки и получения сообщений.</span><span class="sxs-lookup"><span data-stu-id="93070-181">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="93070-182">Концентраторы SignalR</span><span class="sxs-lookup"><span data-stu-id="93070-182">SignalR Hubs</span></span>

<span data-ttu-id="93070-183">В следующем образце кода **ChatHub** класс является производным от **Microsoft.AspNet.SignalR.Hub** класса.</span><span class="sxs-lookup"><span data-stu-id="93070-183">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="93070-184">Наследование от **концентратора** класс является удобным способом для построения приложения SignalR.</span><span class="sxs-lookup"><span data-stu-id="93070-184">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="93070-185">Можно создавать открытые методы в классе концентратора и откройте эти методы, вызывать их из скриптов jQuery на веб-странице.</span><span class="sxs-lookup"><span data-stu-id="93070-185">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="93070-186">В коде чата клиенты вызывают **ChatHub.Send** метод для отправки сообщения.</span><span class="sxs-lookup"><span data-stu-id="93070-186">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="93070-187">Концентратор в свою очередь отправляет сообщение всем клиентам, вызвав **Clients.All.addNewMessageToPage**.</span><span class="sxs-lookup"><span data-stu-id="93070-187">The hub in turn sends the message to all clients by calling **Clients.All.addNewMessageToPage**.</span></span>

<span data-ttu-id="93070-188">**Отправки** метод показывает некоторые понятия концентратора:</span><span class="sxs-lookup"><span data-stu-id="93070-188">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="93070-189">Объявите открытых методов концентратора, чтобы их можно было вызывать клиенты.</span><span class="sxs-lookup"><span data-stu-id="93070-189">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="93070-190">Используйте **Microsoft.AspNet.SignalR.Hub.Clients** свойство для доступа к все клиенты подключены этого концентратора.</span><span class="sxs-lookup"><span data-stu-id="93070-190">Use the **Microsoft.AspNet.SignalR.Hub.Clients** property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="93070-191">Вызов функции jQuery для клиента (такие как `addNewMessageToPage` функции) для обновления клиентов.</span><span class="sxs-lookup"><span data-stu-id="93070-191">Call a jQuery function on the client (such as the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="93070-192">SignalR и jQuery</span><span class="sxs-lookup"><span data-stu-id="93070-192">SignalR and jQuery</span></span>

<span data-ttu-id="93070-193">**Chat.cshtml** файл представления в следующем образце кода показано, как использовать библиотеку jQuery SignalR для связи с концентратора SignalR.</span><span class="sxs-lookup"><span data-stu-id="93070-193">The **Chat.cshtml** view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="93070-194">Основные задачи в коде создается ссылка автоматически созданный прокси-сервер для концентратора, объявление функции, сервер может вызывать для push-содержимое для клиентов и подключения для отправки сообщений в концентратор.</span><span class="sxs-lookup"><span data-stu-id="93070-194">The essential tasks in the code are creating a reference to the auto-generated proxy for the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="93070-195">В следующем коде объявляется прокси-сервер для концентратора.</span><span class="sxs-lookup"><span data-stu-id="93070-195">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="93070-196">В jQuery ссылку на класс server и его членах находится в верхний регистр.</span><span class="sxs-lookup"><span data-stu-id="93070-196">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="93070-197">Ссылается на образец кода C# **ChatHub** класса в jQuery как **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="93070-197">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span> <span data-ttu-id="93070-198">Если требуется сослаться `ChatHub` класса в jQuery с обычной Pascal регистр, как и в C#, измените файл класса ChatHub.cs.</span><span class="sxs-lookup"><span data-stu-id="93070-198">If you want to reference the `ChatHub` class in jQuery with conventional Pascal casing as you would in C#, edit the ChatHub.cs class file.</span></span> <span data-ttu-id="93070-199">Добавить `using` инструкцию, чтобы ссылаться на `Microsoft.AspNet.SignalR.Hubs` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="93070-199">Add a `using` statement to reference the `Microsoft.AspNet.SignalR.Hubs` namespace.</span></span> <span data-ttu-id="93070-200">Затем добавьте `HubName` атрибут `ChatHub` классов, например `[HubName("ChatHub")]`.</span><span class="sxs-lookup"><span data-stu-id="93070-200">Then add the `HubName` attribute to the `ChatHub` class, for example `[HubName("ChatHub")]`.</span></span> <span data-ttu-id="93070-201">Наконец, обновите jQuery справочной информации для `ChatHub` класса.</span><span class="sxs-lookup"><span data-stu-id="93070-201">Finally, update your jQuery reference to the `ChatHub` class.</span></span>


<span data-ttu-id="93070-202">Следующий код показано, как создать функцию обратного вызова в скрипте.</span><span class="sxs-lookup"><span data-stu-id="93070-202">The following code shows how to create a callback function in the script.</span></span> <span data-ttu-id="93070-203">Класс концентратора на сервере вызывает эту функцию для принудительной отправки обновления содержимого для каждого клиента.</span><span class="sxs-lookup"><span data-stu-id="93070-203">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="93070-204">Дополнительный вызов `htmlEncode` функция отображает способ HTML кодирования содержимого сообщения перед его отображением на странице, чтобы предотвратить внедрение скрипта.</span><span class="sxs-lookup"><span data-stu-id="93070-204">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page, as a way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

<span data-ttu-id="93070-205">Ниже показано, как открыть соединение с концентратором.</span><span class="sxs-lookup"><span data-stu-id="93070-205">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="93070-206">Код запускает подключение, а затем передает его функцию для обработки события щелчка кнопкой мыши на **отправки** кнопки на странице чат.</span><span class="sxs-lookup"><span data-stu-id="93070-206">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

> [!NOTE]
> <span data-ttu-id="93070-207">Такой подход гарантирует, что соединение установлено до выполнения обработчика событий.</span><span class="sxs-lookup"><span data-stu-id="93070-207">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="93070-208">Дальнейшие действия</span><span class="sxs-lookup"><span data-stu-id="93070-208">Next Steps</span></span>

<span data-ttu-id="93070-209">Вы узнали, что SignalR — это платформа для построения в режиме реального времени веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="93070-209">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="93070-210">Вы также узнали несколько задач разработки SignalR: Добавление приложения ASP.NET SignalR, как создать класс концентратора и как отправлять и получать сообщения от концентратора.</span><span class="sxs-lookup"><span data-stu-id="93070-210">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="93070-211">Для получения более сложных понятиях новинках SignalR, посетите следующие сайты для SignalR исходный код и ресурсы:</span><span class="sxs-lookup"><span data-stu-id="93070-211">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources :</span></span>

- [<span data-ttu-id="93070-212">Проект SignalR</span><span class="sxs-lookup"><span data-stu-id="93070-212">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="93070-213">SignalR Github и образцы</span><span class="sxs-lookup"><span data-stu-id="93070-213">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="93070-214">Вики-сайте SignalR</span><span class="sxs-lookup"><span data-stu-id="93070-214">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
