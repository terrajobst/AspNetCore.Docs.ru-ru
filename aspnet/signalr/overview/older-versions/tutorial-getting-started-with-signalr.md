---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'Учебник: Начало работы с SignalR 1.x | Документация Майкрософт'
author: pfletcher
description: Использование ASP.NET SignalR для создания приложения разговора в режиме реального времени на странице HTML.
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 13d33ff7e3cfff996a9849cfccfcc43754c8234e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838631"
---
<a name="tutorial-getting-started-with-signalr-1x"></a><span data-ttu-id="33285-103">Учебник: Начало работы с SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="33285-103">Tutorial: Getting Started with SignalR 1.x</span></span>
====================
<span data-ttu-id="33285-104">по [Флетчера Патрик](https://github.com/pfletcher), [Teebken Тим](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="33285-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

> <span data-ttu-id="33285-105">В этом учебнике содержатся сведения об использовании SignalR для создания приложения разговора в режиме реального времени.</span><span class="sxs-lookup"><span data-stu-id="33285-105">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="33285-106">Следует добавить пустой веб-приложения ASP.NET SignalR и создать HTML-страницы для отправки и отображения сообщений.</span><span class="sxs-lookup"><span data-stu-id="33285-106">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>


## <a name="overview"></a><span data-ttu-id="33285-107">Обзор</span><span class="sxs-lookup"><span data-stu-id="33285-107">Overview</span></span>

<span data-ttu-id="33285-108">В этом учебнике представлена разработки SignalR, демонстрирующий создание приложения простой чата на основе веб-обозревателя.</span><span class="sxs-lookup"><span data-stu-id="33285-108">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="33285-109">Будет добавьте библиотеку SignalR в пустой веб-приложения ASP.NET, создайте класс концентратора для отправки сообщений клиентам и создать HTML-страницу, которая позволяет пользователям отправлять и получать сообщения.</span><span class="sxs-lookup"><span data-stu-id="33285-109">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="33285-110">Похожий учебник, демонстрирующий создание приложения чата в MVC 4 с помощью представления MVC, см. в разделе [начало работы с SignalR и MVC 4](index.md).</span><span class="sxs-lookup"><span data-stu-id="33285-110">For a similar tutorial that shows how to create a chat application in MVC 4 using an MVC view, see [Getting Started with SignalR and MVC 4](index.md).</span></span>

> [!NOTE]
> <span data-ttu-id="33285-111">Этом руководстве используется версия выпуска (1.x) SignalR.</span><span class="sxs-lookup"><span data-stu-id="33285-111">This tutorial uses the release (1.x) version of SignalR.</span></span> <span data-ttu-id="33285-112">Сведения об изменениях между SignalR 1.x и 2.0, см. в разделе [проектов обновление SignalR 1.x](../releases/upgrading-signalr-1x-projects-to-20.md).</span><span class="sxs-lookup"><span data-stu-id="33285-112">For details on changes between SignalR 1.x and 2.0, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md).</span></span>

<span data-ttu-id="33285-113">SignalR является библиотекой .NET с открытым исходным кодом для создания веб-приложений, требующих взаимодействия с пользователем в реальном времени или обновления данных в реальном времени.</span><span class="sxs-lookup"><span data-stu-id="33285-113">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="33285-114">Примеры включают социальных приложений, многопользовательские игры, совместной работы и новости, погода бизнеса или финансовых обновления приложений.</span><span class="sxs-lookup"><span data-stu-id="33285-114">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="33285-115">Они часто называются приложениями в реальном времени.</span><span class="sxs-lookup"><span data-stu-id="33285-115">These are often called real-time applications.</span></span>

<span data-ttu-id="33285-116">SignalR упрощает процесс создания приложений в реальном времени.</span><span class="sxs-lookup"><span data-stu-id="33285-116">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="33285-117">Он включает библиотеку ASP.NET сервера и клиентскую библиотеку JavaScript, чтобы упростить управление соединениями между клиентом и сервером и Push-обновления содержимого для клиентов.</span><span class="sxs-lookup"><span data-stu-id="33285-117">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="33285-118">Библиотека SignalR можно добавить в существующее приложение ASP.NET для достижения функциональности в режиме реального времени.</span><span class="sxs-lookup"><span data-stu-id="33285-118">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="33285-119">В данном учебнике показано следующие задачи разработки SignalR:</span><span class="sxs-lookup"><span data-stu-id="33285-119">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="33285-120">Добавление библиотеки SignalR в веб-приложения ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="33285-120">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="33285-121">Создание класса концентратора для отправки содержимого клиентам.</span><span class="sxs-lookup"><span data-stu-id="33285-121">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="33285-122">С помощью библиотеки jQuery SignalR на веб-странице для отправки сообщений и отобразить обновления от концентратора.</span><span class="sxs-lookup"><span data-stu-id="33285-122">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="33285-123">На следующем снимке экрана показано приложение чата, работающих в браузере.</span><span class="sxs-lookup"><span data-stu-id="33285-123">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="33285-124">Каждого нового пользователя можно отправить комментарии и комментарии, добавленные после пользователь не присоединит чате см. в разделе.</span><span class="sxs-lookup"><span data-stu-id="33285-124">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![Экземпляры чата](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="33285-126">Разделы:</span><span class="sxs-lookup"><span data-stu-id="33285-126">Sections:</span></span>

- [<span data-ttu-id="33285-127">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="33285-127">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="33285-128">Запуск образца</span><span class="sxs-lookup"><span data-stu-id="33285-128">Run the Sample</span></span>](#run)
- [<span data-ttu-id="33285-129">Изучите код</span><span class="sxs-lookup"><span data-stu-id="33285-129">Examine the Code</span></span>](#code)
- [<span data-ttu-id="33285-130">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="33285-130">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="33285-131">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="33285-131">Set up the Project</span></span>

<span data-ttu-id="33285-132">В этом разделе показано, как создать пустой веб-приложения ASP.NET, добавьте SignalR и создание приложения чата.</span><span class="sxs-lookup"><span data-stu-id="33285-132">This section shows how to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="33285-133">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="33285-133">Prerequisites:</span></span>

- <span data-ttu-id="33285-134">Visual Studio 2010 SP1 или 2012.</span><span class="sxs-lookup"><span data-stu-id="33285-134">Visual Studio 2010 SP1 or 2012.</span></span> <span data-ttu-id="33285-135">Если у вас нет Visual Studio, см. в разделе [загрузок ASP.NET](https://www.asp.net/downloads) для получения бесплатного Visual Studio 2012 Express средства разработки.</span><span class="sxs-lookup"><span data-stu-id="33285-135">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="33285-136">[Microsoft ASP.NET и веб-инструменты 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span><span class="sxs-lookup"><span data-stu-id="33285-136">[Microsoft ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span></span> <span data-ttu-id="33285-137">Для Visual Studio 2012 этот установщик добавляет новые функции ASP.NET, включая шаблоны SignalR для Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="33285-137">For Visual Studio 2012, this installer adds new ASP.NET features including SignalR templates to Visual Studio.</span></span> <span data-ttu-id="33285-138">Для Visual Studio 2010 с пакетом обновления 1 установщик не доступен, но вам изучить учебник, установив пакет SignalR NuGet, как описано на шагах установки.</span><span class="sxs-lookup"><span data-stu-id="33285-138">For Visual Studio 2010 SP1, an installer is not available but you can complete the tutorial by installing the SignalR NuGet package as described in the setup steps.</span></span>

<span data-ttu-id="33285-139">В следующих действиях используется Visual Studio 2012, чтобы создать пустое веб-приложение ASP.NET и добавить библиотека SignalR:</span><span class="sxs-lookup"><span data-stu-id="33285-139">The following steps use Visual Studio 2012 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="33285-140">В Visual Studio создайте пустое веб-приложение ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="33285-140">In Visual Studio create an ASP.NET Empty Web Application.</span></span>

    ![Создать пустой веб-узел](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="33285-142">Откройте **консоль диспетчера пакетов** , выбрав **инструменты | Диспетчер пакетов библиотеки | Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="33285-142">Open the **Package Manager Console** by selecting **Tools | Library Package Manager | Package Manager Console**.</span></span> <span data-ttu-id="33285-143">Введите следующую команду в окне консоли:</span><span class="sxs-lookup"><span data-stu-id="33285-143">Enter the following command into the console window:</span></span>

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    <span data-ttu-id="33285-144">Эта команда устанавливает последнюю версию SignalR 1.x.</span><span class="sxs-lookup"><span data-stu-id="33285-144">This command installs the latest version of SignalR 1.x.</span></span>
3. <span data-ttu-id="33285-145">В **обозревателе решений**, щелкните правой кнопкой мыши проект, выберите **Add | Класс**.</span><span class="sxs-lookup"><span data-stu-id="33285-145">In **Solution Explorer**, right-click the project, select **Add | Class**.</span></span> <span data-ttu-id="33285-146">Назовите новый класс **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="33285-146">Name the new class **ChatHub**.</span></span>
4. <span data-ttu-id="33285-147">В **обозревателе решений** узел скриптов.</span><span class="sxs-lookup"><span data-stu-id="33285-147">In **Solution Explorer** expand the Scripts node.</span></span> <span data-ttu-id="33285-148">Библиотеки скрипта для jQuery и SignalR, отображаются в проект.</span><span class="sxs-lookup"><span data-stu-id="33285-148">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    ![Ссылки на библиотеку](tutorial-getting-started-with-signalr/_static/image3.png)
5. <span data-ttu-id="33285-150">Замените код в **ChatHub** класса следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="33285-150">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="33285-151">В **обозревателе решений**, щелкните правой кнопкой мыши проект, а затем нажмите кнопку **Add | Новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="33285-151">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="33285-152">В **Добавление нового элемента** диалоговом окне выберите **глобальный класс приложения** и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="33285-152">In the **Add New Item** dialog, select **Global Application Class** and click **Add**.</span></span>

    ![Добавление глобального](tutorial-getting-started-with-signalr/_static/image4.png)
7. <span data-ttu-id="33285-154">Добавьте следующий `using` инструкций после указанных `using` инструкций в классе Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="33285-154">Add the following `using` statements after the provided `using` statements in the Global.asax.cs class.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="33285-155">Добавьте следующую строку кода в `Application_Start` метод глобального класса, чтобы зарегистрировать маршрут по умолчанию для концентраторов SignalR.</span><span class="sxs-lookup"><span data-stu-id="33285-155">Add the following line of code in the `Application_Start` method of the Global class to register the default route for SignalR hubs.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. <span data-ttu-id="33285-156">В **обозревателе решений**, щелкните правой кнопкой мыши проект, а затем нажмите кнопку **Add | Новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="33285-156">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="33285-157">В **Добавление нового элемента** HTML-страницу и щелкните диалоговое окно, выберите **добавить**.</span><span class="sxs-lookup"><span data-stu-id="33285-157">In the **Add New Item** dialog, select Html Page and click **Add**.</span></span>
10. <span data-ttu-id="33285-158">В **обозревателе решений**, щелкните правой кнопкой мыши только что созданный HTML-страницы и нажмите кнопку **задать в качестве начальной страницы**.</span><span class="sxs-lookup"><span data-stu-id="33285-158">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
11. <span data-ttu-id="33285-159">Замените код по умолчанию в HTML-страницу следующий код.</span><span class="sxs-lookup"><span data-stu-id="33285-159">Replace the default code in the HTML page with the following code.</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. <span data-ttu-id="33285-160">**Сохранить все** для проекта.</span><span class="sxs-lookup"><span data-stu-id="33285-160">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="33285-161">Запуск образца</span><span class="sxs-lookup"><span data-stu-id="33285-161">Run the Sample</span></span>

1. <span data-ttu-id="33285-162">Нажмите клавишу F5, чтобы запустить проект в режиме отладки.</span><span class="sxs-lookup"><span data-stu-id="33285-162">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="33285-163">HTML-страница загружается в экземпляре обозревателя и предлагает ввести имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="33285-163">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![Введите имя пользователя](tutorial-getting-started-with-signalr/_static/image5.png)
2. <span data-ttu-id="33285-165">Введите имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="33285-165">Enter a user name.</span></span>
3. <span data-ttu-id="33285-166">Скопируйте URL-адрес в адресной строке браузера и использовать его для открытия двух экземпляров дополнительные браузера.</span><span class="sxs-lookup"><span data-stu-id="33285-166">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="33285-167">В каждом экземпляре обозревателя введите уникальное имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="33285-167">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="33285-168">В каждом экземпляре браузера добавьте комментарий и нажмите кнопку **отправки**.</span><span class="sxs-lookup"><span data-stu-id="33285-168">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="33285-169">Комментарии должны отображаться в все экземпляры браузера.</span><span class="sxs-lookup"><span data-stu-id="33285-169">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="33285-170">Это приложение на простом чате не ведет контекста обсуждения на сервере.</span><span class="sxs-lookup"><span data-stu-id="33285-170">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="33285-171">Концентратор осуществляет широковещательную передачу комментарии для всех текущих пользователей.</span><span class="sxs-lookup"><span data-stu-id="33285-171">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="33285-172">Пользователи, беседа позже будет видеть сообщения добавлены с момента их присоединения к.</span><span class="sxs-lookup"><span data-stu-id="33285-172">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="33285-173">На следующем снимке экрана показано приложение чата в трех экземплярах обозревателя, все из которых обновляются, когда один экземпляр отправляет сообщение:</span><span class="sxs-lookup"><span data-stu-id="33285-173">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![Браузеры чата](tutorial-getting-started-with-signalr/_static/image6.png)
5. <span data-ttu-id="33285-175">В **обозревателе решений**, проверять **документы скриптов** узел для запущенного приложения.</span><span class="sxs-lookup"><span data-stu-id="33285-175">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="33285-176">Файл скрипта с именем **концентраторов** , библиотека SignalR динамически создает во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="33285-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="33285-177">Этот файл управляет обменом данных между jQuery-сценарий и код на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="33285-177">This file manages the communication between jQuery script and server-side code.</span></span>

    ![Созданный Центр сценариев](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="33285-179">Изучите код</span><span class="sxs-lookup"><span data-stu-id="33285-179">Examine the Code</span></span>

<span data-ttu-id="33285-180">Приложение чата SignalR демонстрирует две основные задачи разработки SignalR: создание концентратора в качестве основной координации объекта на сервере и с помощью библиотеки jQuery SignalR для отправки и получения сообщений.</span><span class="sxs-lookup"><span data-stu-id="33285-180">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="33285-181">Концентраторы SignalR</span><span class="sxs-lookup"><span data-stu-id="33285-181">SignalR Hubs</span></span>

<span data-ttu-id="33285-182">В следующем образце кода **ChatHub** класс является производным от **Microsoft.AspNet.SignalR.Hub** класса.</span><span class="sxs-lookup"><span data-stu-id="33285-182">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="33285-183">Наследование от **концентратора** класс является эффективным методом для построения приложения SignalR.</span><span class="sxs-lookup"><span data-stu-id="33285-183">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="33285-184">Можно создавать открытые методы в классе концентратора и затем получить доступ к этих методов, вызывая их с помощью сценариев jQuery в веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="33285-184">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="33285-185">В коде чата, клиенты вызывают **ChatHub.Send** метод для отправки нового сообщения.</span><span class="sxs-lookup"><span data-stu-id="33285-185">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="33285-186">Концентратор в свою очередь отправляет сообщение всем клиентам, вызвав **Clients.All.broadcastMessage**.</span><span class="sxs-lookup"><span data-stu-id="33285-186">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="33285-187">**Отправки** метод демонстрирует несколько вещей понятия:</span><span class="sxs-lookup"><span data-stu-id="33285-187">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="33285-188">Объявления открытых методов концентратора, таким образом, клиенты могут вызывать их.</span><span class="sxs-lookup"><span data-stu-id="33285-188">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="33285-189">Используйте **Microsoft.AspNet.SignalR.Hub.Clients** динамических свойств для доступа к всех клиентов, подключенных к этой вещей.</span><span class="sxs-lookup"><span data-stu-id="33285-189">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="33285-190">Вызов функции jQuery на стороне клиента (например, `broadcastMessage` функции) для обновления клиентов.</span><span class="sxs-lookup"><span data-stu-id="33285-190">Call a jQuery function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="33285-191">SignalR и jQuery</span><span class="sxs-lookup"><span data-stu-id="33285-191">SignalR and jQuery</span></span>

<span data-ttu-id="33285-192">HTML-страницы в примере кода показано, как использовать библиотеку jQuery SignalR для взаимодействия с центром SignalR.</span><span class="sxs-lookup"><span data-stu-id="33285-192">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="33285-193">Основные задачи в коде объявляется прокси-сервер для ссылки на центр, объявление функции, который сервер может обратиться к содержимому Push-уведомлений для клиентов и устанавливается подключение для отправки сообщений в концентратор.</span><span class="sxs-lookup"><span data-stu-id="33285-193">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="33285-194">В следующем коде объявляется прокси-сервер для концентратора.</span><span class="sxs-lookup"><span data-stu-id="33285-194">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="33285-195">В jQuery ссылку на класс сервера и его членах находится в верхний регистр.</span><span class="sxs-lookup"><span data-stu-id="33285-195">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="33285-196">Ссылается на пример кода C# **ChatHub** класс в jQuery как **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="33285-196">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span>


<span data-ttu-id="33285-197">Следующий код является, как создать функцию обратного вызова в скрипте.</span><span class="sxs-lookup"><span data-stu-id="33285-197">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="33285-198">Класс концентратора на сервере вызывает эту функцию для отправки обновлений содержимого для каждого клиента.</span><span class="sxs-lookup"><span data-stu-id="33285-198">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="33285-199">Две строки, кодировать в HTML содержимое перед его отображением являются необязательными и Показать простой способ предотвратить внедрение скрипта.</span><span class="sxs-lookup"><span data-stu-id="33285-199">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

<span data-ttu-id="33285-200">Ниже показано, как открыть соединение с концентратором.</span><span class="sxs-lookup"><span data-stu-id="33285-200">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="33285-201">Код запускает подключение, а затем передает его функции, чтобы обрабатывать событие щелчка на **отправки** кнопку в HTML-страницы.</span><span class="sxs-lookup"><span data-stu-id="33285-201">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="33285-202">Такой подход гарантирует, что соединение установлено, перед выполнением обработчика событий.</span><span class="sxs-lookup"><span data-stu-id="33285-202">This approach insures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="33285-203">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="33285-203">Next Steps</span></span>

<span data-ttu-id="33285-204">Вы узнали, что SignalR — это платформа для построения в режиме реального времени веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="33285-204">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="33285-205">Вы также узнали несколько задач разработки SignalR: Добавление в приложение ASP.NET SignalR, как создать класс концентратора и как отправлять и получать сообщения из концентратора.</span><span class="sxs-lookup"><span data-stu-id="33285-205">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="33285-206">Вы можете предоставить пример приложения в этом руководстве или других приложений SignalR через Интернет, развернув их у поставщика услуг размещения.</span><span class="sxs-lookup"><span data-stu-id="33285-206">You can make the sample application in this tutorial or other SignalR applications available over the Internet by deploying them to a hosting provider.</span></span> <span data-ttu-id="33285-207">Корпорация Майкрософт предлагает бесплатные услуг хостинга до 10 веб-сайтов в бесплатной [пробную учетную запись Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="33285-207">Microsoft offers free web hosting for up to 10 web sites in a free [Windows Azure trial account](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="33285-208">Пошаговое руководство о том, как развернуть пример приложения SignalR, см. в разделе [публикации SignalR примера для начала работы как веб-сайта Windows Azure](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span><span class="sxs-lookup"><span data-stu-id="33285-208">For a walkthrough on how to deploy the sample SignalR application, see [Publish the SignalR Getting Started Sample as a Windows Azure Web Site](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span></span> <span data-ttu-id="33285-209">Подробные сведения о развертывании веб-проекта Visual Studio для веб-сайта Windows Azure см. в разделе [развертывание приложения ASP.NET для веб-сайта Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="33285-209">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Deploying an ASP.NET Application to a Windows Azure Web Site](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span></span> <span data-ttu-id="33285-210">(Примечание: транспорт WebSocket в настоящее время не поддерживается для Windows Azure веб-сайтов.</span><span class="sxs-lookup"><span data-stu-id="33285-210">(Note: The WebSocket transport is not currently supported for Windows Azure Web Sites.</span></span> <span data-ttu-id="33285-211">Транспорт WebSocket, когда недоступен, SignalR использует другие доступные типы транспорта, как описано в разделе транспортов [введение к разделу SignalR](index.md).)</span><span class="sxs-lookup"><span data-stu-id="33285-211">When WebSocket transport is not available, SignalR uses the other available transports as described in the Transports section of the [Introduction to SignalR topic](index.md).)</span></span>

<span data-ttu-id="33285-212">Для более сложных концепций разработки SignalR см. на следующих узлах SignalR исходный код и ресурсы:</span><span class="sxs-lookup"><span data-stu-id="33285-212">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="33285-213">Проект SignalR</span><span class="sxs-lookup"><span data-stu-id="33285-213">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="33285-214">SignalR Github и примерами</span><span class="sxs-lookup"><span data-stu-id="33285-214">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="33285-215">Вики-сайте SignalR</span><span class="sxs-lookup"><span data-stu-id="33285-215">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
