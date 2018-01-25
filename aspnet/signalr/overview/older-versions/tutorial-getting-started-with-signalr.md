---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: "Учебник: Приступая к работе с SignalR 1.x | Документы Microsoft"
author: pfletcher
description: "Используйте ASP.NET SignalR для построения приложения разговора в режиме реального времени на HTML-странице."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: ce4953a0abf64af28ef4dbc5a62bb2d989343d99
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-getting-started-with-signalr-1x"></a><span data-ttu-id="3f7dd-103">Учебник: Приступая к работе с SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="3f7dd-103">Tutorial: Getting Started with SignalR 1.x</span></span>
====================
<span data-ttu-id="3f7dd-104">по [Флетчера Патрик](https://github.com/pfletcher), [Тим Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="3f7dd-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

> <span data-ttu-id="3f7dd-105">В этом учебнике содержатся сведения об использовании SignalR для создания приложения разговора в режиме реального времени.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-105">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="3f7dd-106">Будет добавить SignalR пустое веб-приложение ASP.NET и создать HTML-страницу для отправки и отображения сообщений.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-106">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>


## <a name="overview"></a><span data-ttu-id="3f7dd-107">Обзор</span><span class="sxs-lookup"><span data-stu-id="3f7dd-107">Overview</span></span>

<span data-ttu-id="3f7dd-108">В этом учебнике рассказывается разработки SignalR, показывая, как создать приложение простом чате на основе браузера.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-108">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="3f7dd-109">Будет добавить эту библиотеку SignalR в пустое веб-приложение ASP.NET, создайте класс концентратора для отправки сообщений для клиентов и создать HTML-страницу, которая позволяет пользователям отправлять и получать сообщения.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-109">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="3f7dd-110">Похожий учебник, демонстрирующий создание приложения разговора в MVC 4 с помощью представление MVC, в разделе [Приступая к работе с SignalR и MVC 4](index.md).</span><span class="sxs-lookup"><span data-stu-id="3f7dd-110">For a similar tutorial that shows how to create a chat application in MVC 4 using an MVC view, see [Getting Started with SignalR and MVC 4](index.md).</span></span>

> [!NOTE]
> <span data-ttu-id="3f7dd-111">В этом учебнике используется версии (1.x) SignalR.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-111">This tutorial uses the release (1.x) version of SignalR.</span></span> <span data-ttu-id="3f7dd-112">Дополнительные сведения об изменениях между SignalR 1.x и 2.0, см. [SignalR обновление проектов 1.x](../releases/upgrading-signalr-1x-projects-to-20.md).</span><span class="sxs-lookup"><span data-stu-id="3f7dd-112">For details on changes between SignalR 1.x and 2.0, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md).</span></span>

<span data-ttu-id="3f7dd-113">SignalR — это библиотека .NET открытым исходным кодом, для создания веб-приложений, требующих взаимодействия с пользователем в режиме реального времени или обновления данных в режиме реального времени.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-113">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="3f7dd-114">Примеры приложений социальных сетей, многопользовательских игр, совместной работы и новости, погода бизнес или финансовые обновления приложений.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-114">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="3f7dd-115">Они часто называются приложениями в реальном времени.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-115">These are often called real-time applications.</span></span>

<span data-ttu-id="3f7dd-116">SignalR упрощает процесс создания приложений в режиме реального времени.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-116">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="3f7dd-117">Он включает библиотеку сервера ASP.NET и клиентской библиотеке JavaScript, чтобы упростить управление соединениями сервера клиента и отправить обновления содержимого для клиентов.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-117">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="3f7dd-118">Библиотека SignalR можно добавить к приложению ASP.NET для достижения функциональности в режиме реального времени.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-118">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="3f7dd-119">Учебник демонстрирует следующие задачи разработки SignalR:</span><span class="sxs-lookup"><span data-stu-id="3f7dd-119">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="3f7dd-120">Добавление библиотеки SignalR веб-приложения ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-120">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="3f7dd-121">Создание класса для передачи содержимого клиентам концентратора.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-121">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="3f7dd-122">С помощью библиотеки jQuery SignalR на веб-странице для отправки сообщений и отобразить обновления от концентратора.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-122">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="3f7dd-123">На следующем снимке экрана показано чата приложение в браузере.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-123">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="3f7dd-124">Каждый новый пользователь можно отправлять комментарии и добавлены после пользователь не присоединит чат.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-124">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![Экземпляры чата](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="3f7dd-126">Разделы:</span><span class="sxs-lookup"><span data-stu-id="3f7dd-126">Sections:</span></span>

- [<span data-ttu-id="3f7dd-127">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="3f7dd-127">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="3f7dd-128">Выполнение образца</span><span class="sxs-lookup"><span data-stu-id="3f7dd-128">Run the Sample</span></span>](#run)
- [<span data-ttu-id="3f7dd-129">Анализ кода</span><span class="sxs-lookup"><span data-stu-id="3f7dd-129">Examine the Code</span></span>](#code)
- [<span data-ttu-id="3f7dd-130">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="3f7dd-130">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="3f7dd-131">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="3f7dd-131">Set up the Project</span></span>

<span data-ttu-id="3f7dd-132">В этом разделе показано, как создать пустой веб-приложение ASP.NET, добавить SignalR и создать приложения разговора.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-132">This section shows how to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="3f7dd-133">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="3f7dd-133">Prerequisites:</span></span>

- <span data-ttu-id="3f7dd-134">Visual Studio 2010 SP1 или 2012.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-134">Visual Studio 2010 SP1 or 2012.</span></span> <span data-ttu-id="3f7dd-135">Если у вас Visual Studio, см. раздел [загрузок ASP.NET](https://www.asp.net/downloads) для получения бесплатной Visual Studio 2012 Express средство разработки.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-135">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="3f7dd-136">[Microsoft ASP.NET и веб-средств 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span><span class="sxs-lookup"><span data-stu-id="3f7dd-136">[Microsoft ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span></span> <span data-ttu-id="3f7dd-137">Для Visual Studio 2012 этот установщик добавляет новые возможности ASP.NET, включая шаблоны SignalR в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-137">For Visual Studio 2012, this installer adds new ASP.NET features including SignalR templates to Visual Studio.</span></span> <span data-ttu-id="3f7dd-138">Для Visual Studio 2010 с пакетом обновления 1 установщик не доступен, но завершения этого учебника можно путем установки пакета SignalR NuGet, как описано в руководстве по установке.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-138">For Visual Studio 2010 SP1, an installer is not available but you can complete the tutorial by installing the SignalR NuGet package as described in the setup steps.</span></span>

<span data-ttu-id="3f7dd-139">В следующих действиях используется Visual Studio 2012 для создания пустой веб-приложения ASP.NET и добавьте библиотеку SignalR:</span><span class="sxs-lookup"><span data-stu-id="3f7dd-139">The following steps use Visual Studio 2012 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="3f7dd-140">В Visual Studio создайте пустое веб-приложение ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-140">In Visual Studio create an ASP.NET Empty Web Application.</span></span>

    ![Создать пустой веб-узел](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="3f7dd-142">Откройте **консоль диспетчера пакетов** , выбрав **инструменты | Диспетчер пакетов библиотеки | Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-142">Open the **Package Manager Console** by selecting **Tools | Library Package Manager | Package Manager Console**.</span></span> <span data-ttu-id="3f7dd-143">Введите следующую команду в окне консоли:</span><span class="sxs-lookup"><span data-stu-id="3f7dd-143">Enter the following command into the console window:</span></span>

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    <span data-ttu-id="3f7dd-144">Эта команда устанавливает последнюю версию SignalR 1.x.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-144">This command installs the latest version of SignalR 1.x.</span></span>
3. <span data-ttu-id="3f7dd-145">В **обозревателе решений**, щелкните правой кнопкой мыши проект, выберите **добавить | Класс**.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-145">In **Solution Explorer**, right-click the project, select **Add | Class**.</span></span> <span data-ttu-id="3f7dd-146">Имя для нового класса **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-146">Name the new class **ChatHub**.</span></span>
4. <span data-ttu-id="3f7dd-147">В **обозревателе решений** разверните узел «скрипты».</span><span class="sxs-lookup"><span data-stu-id="3f7dd-147">In **Solution Explorer** expand the Scripts node.</span></span> <span data-ttu-id="3f7dd-148">Библиотеки скриптов jQuery и SignalR, отображаются в проект.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-148">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    ![Ссылки на библиотеки](tutorial-getting-started-with-signalr/_static/image3.png)
5. <span data-ttu-id="3f7dd-150">Замените код в **ChatHub** класса следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-150">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="3f7dd-151">В **обозревателе решений**, щелкните правой кнопкой мыши проект, а затем нажмите кнопку **добавить | Новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-151">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="3f7dd-152">В **Добавление нового элемента** диалогового окна выберите **глобальный класс приложения** и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-152">In the **Add New Item** dialog, select **Global Application Class** and click **Add**.</span></span>

    ![Добавление глобальных](tutorial-getting-started-with-signalr/_static/image4.png)
7. <span data-ttu-id="3f7dd-154">Добавьте следующие `using` инструкции после указанных `using` инструкций в классе Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-154">Add the following `using` statements after the provided `using` statements in the Global.asax.cs class.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="3f7dd-155">Добавьте следующую строку кода в `Application_Start` метод глобального класса, чтобы зарегистрировать маршрут по умолчанию для концентраторов SignalR.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-155">Add the following line of code in the `Application_Start` method of the Global class to register the default route for SignalR hubs.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. <span data-ttu-id="3f7dd-156">В **обозревателе решений**, щелкните правой кнопкой мыши проект, а затем нажмите кнопку **добавить | Новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-156">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="3f7dd-157">В **Добавление нового элемента** окна, выберите HTML-страницы и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-157">In the **Add New Item** dialog, select Html Page and click **Add**.</span></span>
10. <span data-ttu-id="3f7dd-158">В **обозревателе решений**, щелкните правой кнопкой мыши только что созданный HTML-страницы и нажмите кнопку **задать в качестве начальной страницы**.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-158">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
11. <span data-ttu-id="3f7dd-159">Замените код по умолчанию в HTML-страницу с помощью следующего кода.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-159">Replace the default code in the HTML page with the following code.</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. <span data-ttu-id="3f7dd-160">**Сохранить все** для проекта.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-160">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="3f7dd-161">Выполнение образца</span><span class="sxs-lookup"><span data-stu-id="3f7dd-161">Run the Sample</span></span>

1. <span data-ttu-id="3f7dd-162">Нажмите клавишу F5 для запуска проекта в режиме отладки.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-162">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="3f7dd-163">HTML-страницы загружает в экземпляре браузера и предлагает ввести имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-163">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![Введите имя пользователя](tutorial-getting-started-with-signalr/_static/image5.png)
2. <span data-ttu-id="3f7dd-165">Введите имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-165">Enter a user name.</span></span>
3. <span data-ttu-id="3f7dd-166">Копировать URL-адрес из адресной строки браузера и использовать ее для открытия две дополнительные экземпляры браузера.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-166">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="3f7dd-167">В каждом экземпляре браузера введите уникальное имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-167">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="3f7dd-168">В каждом экземпляре браузера, добавьте комментарий и щелкните **отправки**.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-168">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="3f7dd-169">Комментарии должны отображаться в все экземпляры браузера.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-169">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3f7dd-170">Это приложение на простом чате; не поддерживает контекст обсуждения на сервере.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-170">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="3f7dd-171">Концентратор осуществляет широковещательную рассылку комментариев для всех текущих пользователей.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-171">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="3f7dd-172">Пользователей, которые позже принять участие в обсуждении будут отображены сообщения, добавлены с момента их соединения.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-172">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="3f7dd-173">На следующем снимке экрана показано чата приложения, работающего в трех экземпляров браузера, все из которых обновляются, когда один экземпляр отправляет сообщение.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-173">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![Браузеры чата](tutorial-getting-started-with-signalr/_static/image6.png)
5. <span data-ttu-id="3f7dd-175">В **обозревателе решений**, проверять **документов скрипта** узел для запущенного приложения.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-175">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="3f7dd-176">Отсутствует файл скрипта с именем **концентраторов** , библиотека SignalR динамически приводит к возникновению ошибки во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="3f7dd-177">Этот файл обеспечивает связь между jQuery сценарий и код на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-177">This file manages the communication between jQuery script and server-side code.</span></span>

    ![Скрипт, созданный концентратора](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="3f7dd-179">Анализ кода</span><span class="sxs-lookup"><span data-stu-id="3f7dd-179">Examine the Code</span></span>

<span data-ttu-id="3f7dd-180">Приложение разговора SignalR демонстрирует две основные задачи разработки SignalR: создание концентратора как объект основного координации на сервере, а с помощью библиотеки jQuery SignalR для отправки и получения сообщений.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-180">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="3f7dd-181">Концентраторы SignalR</span><span class="sxs-lookup"><span data-stu-id="3f7dd-181">SignalR Hubs</span></span>

<span data-ttu-id="3f7dd-182">В следующем образце кода **ChatHub** класс является производным от **Microsoft.AspNet.SignalR.Hub** класса.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-182">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="3f7dd-183">Наследование от **концентратора** класс является удобным способом для построения приложения SignalR.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-183">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="3f7dd-184">Можно создавать открытые методы в классе концентратора и откройте эти методы, вызывать их из скриптов jQuery на веб-странице.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-184">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="3f7dd-185">В коде чата клиенты вызывают **ChatHub.Send** метод для отправки сообщения.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-185">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="3f7dd-186">Концентратор в свою очередь отправляет сообщение всем клиентам, вызвав **Clients.All.broadcastMessage**.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-186">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="3f7dd-187">**Отправки** метод показывает некоторые понятия концентратора:</span><span class="sxs-lookup"><span data-stu-id="3f7dd-187">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="3f7dd-188">Объявите открытых методов концентратора, чтобы их можно было вызывать клиенты.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-188">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="3f7dd-189">Используйте **Microsoft.AspNet.SignalR.Hub.Clients** динамическое свойство обращается ко всем клиентам подключения этого концентратора.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-189">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="3f7dd-190">Вызов функции jQuery для клиента (такие как `broadcastMessage` функции) для обновления клиентов.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-190">Call a jQuery function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="3f7dd-191">SignalR и jQuery</span><span class="sxs-lookup"><span data-stu-id="3f7dd-191">SignalR and jQuery</span></span>

<span data-ttu-id="3f7dd-192">HTML-страницы в следующем образце кода показано, как использовать библиотеку jQuery SignalR для связи с концентратора SignalR.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-192">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="3f7dd-193">Основные задачи в коде объявляется прокси-сервер для ссылки на концентратор, объявление функции, сервер может вызывать для push-содержимое для клиентов и запуска подключения для отправки сообщений в концентратор.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-193">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="3f7dd-194">В следующем коде объявляется прокси-сервер для концентратора.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-194">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="3f7dd-195">В jQuery ссылку на класс server и его членах находится в верхний регистр.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-195">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="3f7dd-196">Ссылается на образец кода C# **ChatHub** класса в jQuery как **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-196">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span>


<span data-ttu-id="3f7dd-197">Следующий код является, как создать функции обратного вызова в скрипте.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-197">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="3f7dd-198">Класс концентратора на сервере вызывает эту функцию для принудительной отправки обновления содержимого для каждого клиента.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-198">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="3f7dd-199">Две строки, что кодировка HTML содержимое перед его отображением являются необязательными и Показать простой способ предотвратить внедрение скрипта.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-199">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

<span data-ttu-id="3f7dd-200">Ниже показано, как открыть соединение с концентратором.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-200">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="3f7dd-201">Код запускает подключение, а затем передает его функцию для обработки события щелчка кнопкой мыши на **отправки** кнопки на HTML-странице.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-201">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="3f7dd-202">Такой подход гарантирует, что соединение установлено до выполнения обработчика событий.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-202">This approach insures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="3f7dd-203">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="3f7dd-203">Next Steps</span></span>

<span data-ttu-id="3f7dd-204">Вы узнали, что SignalR — это платформа для построения в режиме реального времени веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-204">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="3f7dd-205">Вы также узнали несколько задач разработки SignalR: Добавление приложения ASP.NET SignalR, как создать класс концентратора и как отправлять и получать сообщения от концентратора.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-205">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="3f7dd-206">Выполненные примера приложения в этом учебнике или других приложений SignalR доступны через Интернет с их развертыванием на поставщике услуг размещения.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-206">You can make the sample application in this tutorial or other SignalR applications available over the Internet by deploying them to a hosting provider.</span></span> <span data-ttu-id="3f7dd-207">Корпорация Майкрософт предлагает бесплатные услуг хостинга до 10 веб-сайтов в бесплатной [пробную учетную запись Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="3f7dd-207">Microsoft offers free web hosting for up to 10 web sites in a free [Windows Azure trial account](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="3f7dd-208">Пошаговое руководство по развертыванию SignalR пример приложения см. в разделе [публикации SignalR примере для начала работы как веб-сайта Windows Azure](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span><span class="sxs-lookup"><span data-stu-id="3f7dd-208">For a walkthrough on how to deploy the sample SignalR application, see [Publish the SignalR Getting Started Sample as a Windows Azure Web Site](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span></span> <span data-ttu-id="3f7dd-209">Подробные сведения о развертывании веб-проекта Visual Studio для веб-сайта Windows Azure см. в разделе [развертывание приложения ASP.NET для веб-сайта Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="3f7dd-209">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Deploying an ASP.NET Application to a Windows Azure Web Site](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span></span> <span data-ttu-id="3f7dd-210">(Примечание: транспорт WebSocket в настоящее время не поддерживается для Windows Azure веб-сайтов.</span><span class="sxs-lookup"><span data-stu-id="3f7dd-210">(Note: The WebSocket transport is not currently supported for Windows Azure Web Sites.</span></span> <span data-ttu-id="3f7dd-211">Транспорт WebSocket при недоступна, SignalR использует другие доступные типы транспорта, как описано в разделе транспортов [введение в раздел SignalR](index.md).)</span><span class="sxs-lookup"><span data-stu-id="3f7dd-211">When WebSocket transport is not available, SignalR uses the other available transports as described in the Transports section of the [Introduction to SignalR topic](index.md).)</span></span>

<span data-ttu-id="3f7dd-212">Для получения более сложных понятиях новинках SignalR, посетите следующие сайты для SignalR исходный код и ресурсы:</span><span class="sxs-lookup"><span data-stu-id="3f7dd-212">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="3f7dd-213">Проект SignalR</span><span class="sxs-lookup"><span data-stu-id="3f7dd-213">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="3f7dd-214">SignalR Github и образцы</span><span class="sxs-lookup"><span data-stu-id="3f7dd-214">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="3f7dd-215">Вики-сайте SignalR</span><span class="sxs-lookup"><span data-stu-id="3f7dd-215">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
