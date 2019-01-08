---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: Учебник. Чат в реальном времени с SignalR 2 | Документация Майкрософт
author: pfletcher
description: В этом учебнике содержатся сведения об использовании SignalR для создания приложения разговора в режиме реального времени. Добавление пустой веб-приложения ASP.NET SignalR.
ms.author: riande
ms.date: 01/02/2019
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: aa015abc47bb2450e04e167c0404aaa1d119ba2c
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098628"
---
# <a name="tutorial-real-time-chat-with-signalr-2"></a><span data-ttu-id="ed4a3-104">Учебник. Чат в реальном времени с SignalR 2</span><span class="sxs-lookup"><span data-stu-id="ed4a3-104">Tutorial: Real-time chat with SignalR 2</span></span>

<span data-ttu-id="ed4a3-105">Этом руководстве показано, как использовать SignalR для создания приложения разговора в режиме реального времени.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-105">This tutorial shows you how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="ed4a3-106">Добавьте пустой веб-приложения ASP.NET SignalR и создайте страницу HTML для отправки и отображения сообщений.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-106">You add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>

<span data-ttu-id="ed4a3-107">В этом руководстве вы:</span><span class="sxs-lookup"><span data-stu-id="ed4a3-107">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ed4a3-108">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="ed4a3-108">Set up the project</span></span>
> * <span data-ttu-id="ed4a3-109">Запуск образца</span><span class="sxs-lookup"><span data-stu-id="ed4a3-109">Run the sample</span></span>
> * <span data-ttu-id="ed4a3-110">Изучите код</span><span class="sxs-lookup"><span data-stu-id="ed4a3-110">Examine the code</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="ed4a3-111">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="ed4a3-111">Prerequisites</span></span>

* <span data-ttu-id="ed4a3-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) с **ASP.NET и веб-разработка** рабочей нагрузки.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="ed4a3-113">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="ed4a3-113">Set up the Project</span></span>

<span data-ttu-id="ed4a3-114">В этом разделе показано, как использовать Visual Studio 2017 и SignalR 2 для создания пустой веб-приложения ASP.NET, добавьте SignalR и создание приложения чата.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-114">This section shows how to use Visual Studio 2017 and SignalR 2 to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

1. <span data-ttu-id="ed4a3-115">В Visual Studio создайте веб-приложения ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-115">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Создание веб-](tutorial-getting-started-with-signalr/_static/image2.png)

1. <span data-ttu-id="ed4a3-117">В **новый проект ASP.NET — SignalRChat** окне оставьте **пустой** затем выберите **ОК**.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-117">In the **New ASP.NET Project - SignalRChat** window, leave **Empty** selected and select **OK**.</span></span>

1. <span data-ttu-id="ed4a3-118">В **обозревателе решений**, щелкните правой кнопкой мыши проект и выберите **добавить** > **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-118">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="ed4a3-119">В **Добавление нового элемента — SignalRChat**выберите **установленные** > **Visual C#**   >  **Web**  >  **SignalR** , а затем выберите **класс концентратора SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-119">In **Add New Item - SignalRChat**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="ed4a3-120">Назовите класс *ChatHub* и добавьте его в проект.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-120">Name the class *ChatHub* and add it to the project.</span></span>

    <span data-ttu-id="ed4a3-121">На этом шаге создается *ChatHub.cs* файла и добавляет набор файлов сценариев и ссылки на сборки, которые поддерживают SignalR в проект.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-121">This step creates the *ChatHub.cs* class file and adds a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="ed4a3-122">Замените код в новом *ChatHub.cs* файл класса следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="ed4a3-122">Replace the code in the new *ChatHub.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]

1. <span data-ttu-id="ed4a3-123">В **обозревателе решений**, щелкните правой кнопкой мыши проект и выберите **добавить** > **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-123">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="ed4a3-124">В **Добавление нового элемента — SignalRChat** выберите **установленные** > **Visual C#**   >  **Web** и затем Выберите **класс запуска OWIN**.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-124">In **Add New Item - SignalRChat** select **Installed** > **Visual C#** > **Web**  and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="ed4a3-125">Назовите класс *запуска* и добавьте его в проект.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-125">Name the class *Startup* and add it to the project.</span></span>

1. <span data-ttu-id="ed4a3-126">В **обозревателе решений**, щелкните правой кнопкой мыши проект и выберите **добавить** > **HTML-страницу**.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-126">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="ed4a3-127">Имя новой страницы *индекс* и выберите **ОК**.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-127">Name the new page *index* and select **OK**.</span></span>

1. <span data-ttu-id="ed4a3-128">В **обозревателе решений**, щелкните правой кнопкой мыши созданную HTML-страницы и выберите **задать в качестве начальной страницы**.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-128">In **Solution Explorer**, right-click the HTML page you created and select **Set as Start Page**.</span></span>

1. <span data-ttu-id="ed4a3-129">Замените код по умолчанию в HTML-страницы этот код:</span><span class="sxs-lookup"><span data-stu-id="ed4a3-129">Replace the default code in the HTML page with this code:</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]

1. <span data-ttu-id="ed4a3-130">В **обозревателе решений**, разверните **сценариев**.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-130">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="ed4a3-131">Библиотеки скрипта для jQuery и SignalR, отображаются в проект.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-131">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="ed4a3-132">Диспетчер пакетов может установить более позднюю версию сценариев SignalR.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-132">The package manager may have installed a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="ed4a3-133">Убедитесь, что ссылки на скрипты в блоке кода соответствуют версиям файлов сценариев в проекте.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-133">Check that the script references in the code block correspond to the versions of the script files in the project.</span></span>

    <span data-ttu-id="ed4a3-134">Ссылки на скрипты из исходного блока кода:</span><span class="sxs-lookup"><span data-stu-id="ed4a3-134">Script references from the original code block:</span></span>
    ```html
    <!--Script references. -->
    <!--Reference the jQuery library. -->
    <script src="Scripts/jquery-3.1.1.min.js" ></script>
    <!--Reference the SignalR library. -->
    <script src="Scripts/jquery.signalR-2.2.1.min.js"></script>
    ```

1. <span data-ttu-id="ed4a3-135">Если они не совпадают, обновите *.html* файл.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-135">If they don't match, update the *.html* file.</span></span>

1. <span data-ttu-id="ed4a3-136">В строке меню выберите **файл** > **сохранить все**.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-136">From the menu bar, select **File** > **Save All**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="ed4a3-137">Запуск образца</span><span class="sxs-lookup"><span data-stu-id="ed4a3-137">Run the Sample</span></span>

1. <span data-ttu-id="ed4a3-138">На панели инструментов, включите **отладки скриптов** и затем нажмите кнопку воспроизведения, чтобы запустить пример в режиме отладки.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-138">In the toolbar, turn on **Script Debugging** and then select the play button to run the sample in Debug mode.</span></span>

    ![Введите имя пользователя](tutorial-getting-started-with-signalr/_static/image3.png)

1. <span data-ttu-id="ed4a3-140">Когда откроется браузер, введите имя для вашего удостоверения чата.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-140">When the browser opens, enter a name for your chat identity.</span></span>

1. <span data-ttu-id="ed4a3-141">Скопируйте URL-адрес из браузера, откройте два других браузеров и вставьте URL-адреса в панели адрес.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-141">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="ed4a3-142">В каждом браузере введите уникальное имя.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-142">In each browser, enter a unique name.</span></span>

1. <span data-ttu-id="ed4a3-143">Теперь добавьте комментарий и выберите **отправки**.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-143">Now, add a comment and select **Send**.</span></span> <span data-ttu-id="ed4a3-144">Повторите, в других браузерах.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-144">Repeat that in the other browsers.</span></span> <span data-ttu-id="ed4a3-145">Комментарии отображаются в режиме реального времени.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-145">The comments appear in real-time.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ed4a3-146">Это приложение на простом чате не ведет контекста обсуждения на сервере.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-146">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="ed4a3-147">Концентратор осуществляет широковещательную передачу комментарии для всех текущих пользователей.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-147">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="ed4a3-148">Пользователи, беседа позже будет видеть сообщения добавлены с момента их присоединения к.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-148">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="ed4a3-149">См. в разделе, запуск приложения чата в трех различных браузерах.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-149">See how the chat application runs in three different browsers.</span></span> <span data-ttu-id="ed4a3-150">Когда Tom Ананд и Сьюзан отправляют сообщения, все браузеры обновление в режиме реального времени:</span><span class="sxs-lookup"><span data-stu-id="ed4a3-150">When Tom, Anand, and Susan send messages, all browsers update in real time:</span></span>

    ![Все три браузеры отображают же журнала разговора](tutorial-getting-started-with-signalr/_static/image4.png)

1. <span data-ttu-id="ed4a3-152">В **обозревателе решений**, проверять **документы скриптов** узел для запущенного приложения.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-152">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="ed4a3-153">Файл скрипта с именем *концентраторов* , библиотека SignalR создает во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-153">There's a script file named *hubs* that the SignalR library generates at runtime.</span></span> <span data-ttu-id="ed4a3-154">Этот файл управляет обменом данных между jQuery-сценарий и код на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-154">This file manages the communication between jQuery script and server-side code.</span></span>

    ![автоматически созданное концентраторов сценария в узел документов скриптов](tutorial-getting-started-with-signalr/_static/image5.png)

## <a name="examine-the-code"></a><span data-ttu-id="ed4a3-156">Изучите код</span><span class="sxs-lookup"><span data-stu-id="ed4a3-156">Examine the Code</span></span>

<span data-ttu-id="ed4a3-157">Приложение SignalRChat демонстрирует две основные задачи разработки SignalR.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-157">The SignalRChat application demonstrates two basic SignalR development tasks.</span></span> <span data-ttu-id="ed4a3-158">Оно показано, как создать концентратор.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-158">It shows you how to create a hub.</span></span> <span data-ttu-id="ed4a3-159">Сервер использует этот концентратор как объект основного координации.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-159">The server uses that hub as the main coordination object.</span></span> <span data-ttu-id="ed4a3-160">Концентратор использует библиотеку jQuery SignalR для отправки и получения сообщений.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-160">The hub uses the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs-in-the-chathubcs"></a><span data-ttu-id="ed4a3-161">Концентраторы SignalR в ChatHub.cs</span><span class="sxs-lookup"><span data-stu-id="ed4a3-161">SignalR Hubs in the ChatHub.cs</span></span>

<span data-ttu-id="ed4a3-162">В приведенном выше примере кода `ChatHub` класс является производным от `Microsoft.AspNet.SignalR.Hub` класса.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-162">In the code sample above, the `ChatHub` class derives from the `Microsoft.AspNet.SignalR.Hub` class.</span></span> <span data-ttu-id="ed4a3-163">Наследование от `Hub` класс является эффективным методом для построения приложения SignalR.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-163">Deriving from the `Hub` class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="ed4a3-164">Можно создавать открытые методы в классе концентратора и затем использовать их, вызвав их из сценариев на веб-странице.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-164">You can create public methods on your hub class and then use those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="ed4a3-165">В коде чата, клиенты вызывают `ChatHub.Send` метод для отправки нового сообщения.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-165">In the chat code, clients call the `ChatHub.Send` method to send a new message.</span></span> <span data-ttu-id="ed4a3-166">Концентратор затем отправляет сообщение всем клиентам, вызвав `Clients.All.broadcastMessage`.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-166">The hub then sends the message to all clients by calling `Clients.All.broadcastMessage`.</span></span>

<span data-ttu-id="ed4a3-167">`Send` Метод демонстрирует несколько вещей понятия:</span><span class="sxs-lookup"><span data-stu-id="ed4a3-167">The `Send` method demonstrates several hub concepts:</span></span>

* <span data-ttu-id="ed4a3-168">Объявления открытых методов концентратора, таким образом, клиенты могут вызывать их.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-168">Declare public methods on a hub so that clients can call them.</span></span>

* <span data-ttu-id="ed4a3-169">Используйте `Microsoft.AspNet.SignalR.Hub.Clients` динамических свойств для взаимодействия со всеми клиентами, подключенных к этому концентратору.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-169">Use the `Microsoft.AspNet.SignalR.Hub.Clients` dynamic property to communicate with all clients connected to this hub.</span></span>

* <span data-ttu-id="ed4a3-170">Вызов функции на стороне клиента (например `broadcastMessage` функции) для обновления клиентов.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-170">Call a function on the client (like the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery-in-the-indexhtml"></a><span data-ttu-id="ed4a3-171">SignalR и jQuery в index.html</span><span class="sxs-lookup"><span data-stu-id="ed4a3-171">SignalR and jQuery in the index.html</span></span>

<span data-ttu-id="ed4a3-172">*Index.html* страницы в примере кода показано, как использовать библиотеку jQuery SignalR для взаимодействия с центром SignalR.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-172">The *index.html* page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="ed4a3-173">Код выполняет ряд важных задач.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-173">The code carries out many important tasks.</span></span> <span data-ttu-id="ed4a3-174">Он объявляется прокси-сервер для ссылки на концентратор, объявляет функцию, сервер может обратиться к содержимому Push-уведомлений для клиентов, что он запускает подключение для отправки сообщений в концентратор.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-174">It declares a proxy to reference the hub, declares a function that the server can call to push content to clients, and it starts a connection to send messages to the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> <span data-ttu-id="ed4a3-175">В JavaScript должно быть camelCase ссылку на класс сервера и его членах.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-175">In JavaScript the reference to the server class and its members has to be camelCase.</span></span> <span data-ttu-id="ed4a3-176">Ссылки на код примера C# *ChatHub* класс в JavaScript в виде `chatHub`.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-176">The code sample references the C# *ChatHub* class in JavaScript as `chatHub`.</span></span>

<span data-ttu-id="ed4a3-177">В этот блок кода как создать функцию обратного вызова в скрипте.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-177">In this code block, you create a callback function in the script.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

<span data-ttu-id="ed4a3-178">Класс концентратора на сервере вызывает эту функцию для отправки обновлений содержимого для каждого клиента.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-178">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="ed4a3-179">Две строки, HTML-кодирование содержимого перед его отображением являются необязательными и Показать хороший способ предотвратить внедрение скрипта.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-179">The two lines that HTML-encode the content before displaying it are optional and show a good way to prevent script injection.</span></span>

<span data-ttu-id="ed4a3-180">Этот код открывает соединение с концентратором.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-180">This code opens a connection with the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

> [!NOTE]
> <span data-ttu-id="ed4a3-181">Такой подход гарантирует, что код устанавливает соединение перед выполнением обработчика событий.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-181">This approach ensures that the code establishes a connection before the event handler executes.</span></span>

<span data-ttu-id="ed4a3-182">Код запускает подключение, а затем передает его функции, чтобы обрабатывать событие щелчка на **отправки** кнопку в HTML-страницы.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-182">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ed4a3-183">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="ed4a3-183">Additional resources</span></span>

<span data-ttu-id="ed4a3-184">Дополнительные сведения о SignalR см. следующие ресурсы:</span><span class="sxs-lookup"><span data-stu-id="ed4a3-184">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="ed4a3-185">Проект SignalR</span><span class="sxs-lookup"><span data-stu-id="ed4a3-185">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="ed4a3-186">SignalR Github и примерами</span><span class="sxs-lookup"><span data-stu-id="ed4a3-186">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="ed4a3-187">Вики-сайте SignalR</span><span class="sxs-lookup"><span data-stu-id="ed4a3-187">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="ed4a3-188">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="ed4a3-188">Next steps</span></span>

<span data-ttu-id="ed4a3-189">В этом руководстве вы:</span><span class="sxs-lookup"><span data-stu-id="ed4a3-189">In this tutorial you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ed4a3-190">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="ed4a3-190">Set up the project</span></span>
> * <span data-ttu-id="ed4a3-191">Запустили пример</span><span class="sxs-lookup"><span data-stu-id="ed4a3-191">Ran the sample</span></span>
> * <span data-ttu-id="ed4a3-192">Проверить код</span><span class="sxs-lookup"><span data-stu-id="ed4a3-192">Examined the code</span></span>

<span data-ttu-id="ed4a3-193">Перейдите к следующей статье, чтобы сведения об использовании SignalR и MVC 5.</span><span class="sxs-lookup"><span data-stu-id="ed4a3-193">Advance to the next article to learn how to use SignalR and MVC 5.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="ed4a3-194">SignalR 2 и MVC 5</span><span class="sxs-lookup"><span data-stu-id="ed4a3-194">SignalR 2 and MVC 5</span></span>](tutorial-getting-started-with-signalr-and-mvc.md)