---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: Учебник. Чат в реальном времени с SignalR 2 и MVC 5 | Документация Майкрософт
author: pfletcher
description: Этом руководстве показано, как использовать ASP.NET SignalR 2 для создания приложения разговора в режиме реального времени. SignalR добавьте в приложение MVC 5.
ms.author: riande
ms.date: 01/02/2019
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: eb4b7e1403f4070d65702b756bf98c5294c7fb17
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098608"
---
# <a name="tutorial-real-time-chat-with-signalr-2-and-mvc-5"></a><span data-ttu-id="6ef81-104">Учебник. Чат в реальном времени с SignalR 2 и MVC 5</span><span class="sxs-lookup"><span data-stu-id="6ef81-104">Tutorial: Real-time chat with SignalR 2 and MVC 5</span></span>

<span data-ttu-id="6ef81-105">Этом руководстве показано, как использовать ASP.NET SignalR 2 для создания приложения разговора в режиме реального времени.</span><span class="sxs-lookup"><span data-stu-id="6ef81-105">This tutorial shows how to use ASP.NET SignalR 2 to create a real-time chat application.</span></span> <span data-ttu-id="6ef81-106">Добавление SignalR в приложение MVC 5 и создание представления чата для отправки и отображения сообщений.</span><span class="sxs-lookup"><span data-stu-id="6ef81-106">You add SignalR to an MVC 5 application and create a chat view to send and display messages.</span></span>

<span data-ttu-id="6ef81-107">В этом руководстве вы:</span><span class="sxs-lookup"><span data-stu-id="6ef81-107">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6ef81-108">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="6ef81-108">Set up the project</span></span>
> * <span data-ttu-id="6ef81-109">Запуск образца</span><span class="sxs-lookup"><span data-stu-id="6ef81-109">Run the sample</span></span>
> * <span data-ttu-id="6ef81-110">Изучите код</span><span class="sxs-lookup"><span data-stu-id="6ef81-110">Examine the code</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="6ef81-111">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="6ef81-111">Prerequisites</span></span>

* <span data-ttu-id="6ef81-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) с **ASP.NET и веб-разработка** рабочей нагрузки.</span><span class="sxs-lookup"><span data-stu-id="6ef81-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="6ef81-113">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="6ef81-113">Set up the Project</span></span>

<span data-ttu-id="6ef81-114">В этом разделе показано, как использовать Visual Studio 2017 и SignalR 2 для создания пустого приложения ASP.NET MVC 5, добавить библиотеку SignalR и создание приложения чата.</span><span class="sxs-lookup"><span data-stu-id="6ef81-114">This section shows how to use Visual Studio 2017 and SignalR 2 to create an empty ASP.NET MVC 5 application, add the SignalR library, and create the chat application.</span></span>

1. <span data-ttu-id="6ef81-115">В Visual Studio создайте приложение ASP.NET на C#, ориентированном на .NET Framework 4.5, назовите его SignalRChat и нажмите кнопку ОК.</span><span class="sxs-lookup"><span data-stu-id="6ef81-115">In Visual Studio, create a C# ASP.NET application that targets .NET Framework 4.5, name it SignalRChat, and click OK.</span></span>

    ![Создание веб-](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

1. <span data-ttu-id="6ef81-117">В **новый веб-приложение ASP.NET - SignalRMvcChat**выберите **MVC** , а затем выберите **изменить способ проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="6ef81-117">In **New ASP.NET Web Application - SignalRMvcChat**, select **MVC** and then select **Change Authentication**.</span></span>

1. <span data-ttu-id="6ef81-118">В **изменить способ проверки подлинности**выберите **без проверки подлинности** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="6ef81-118">In **Change Authentication**, select **No Authentication** and click **OK**.</span></span>

    ![Выберите без проверки подлинности](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)

1. <span data-ttu-id="6ef81-120">В **новый веб-приложение ASP.NET - SignalRMvcChat**выберите **ОК**.</span><span class="sxs-lookup"><span data-stu-id="6ef81-120">In **New ASP.NET Web Application - SignalRMvcChat**, select **OK**.</span></span>

1. <span data-ttu-id="6ef81-121">В **обозревателе решений**, щелкните правой кнопкой мыши проект и выберите **добавить** > **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="6ef81-121">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="6ef81-122">В **Добавление нового элемента — SignalRChat**выберите **установленные** > **Visual C#**   >  **Web**  >  **SignalR** , а затем выберите **класс концентратора SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="6ef81-122">In **Add New Item - SignalRChat**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="6ef81-123">Назовите класс *ChatHub* и добавьте его в проект.</span><span class="sxs-lookup"><span data-stu-id="6ef81-123">Name the class *ChatHub* and add it to the project.</span></span>

    <span data-ttu-id="6ef81-124">На этом шаге создается *ChatHub.cs* файла и добавляет набор файлов сценариев и ссылки на сборки, которые поддерживают SignalR в проект.</span><span class="sxs-lookup"><span data-stu-id="6ef81-124">This step creates the *ChatHub.cs* class file and adds a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="6ef81-125">Замените код в новом *ChatHub.cs* файл класса следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="6ef81-125">Replace the code in the new *ChatHub.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]

1. <span data-ttu-id="6ef81-126">В **обозревателе решений**, щелкните правой кнопкой мыши проект и выберите **добавить** > **класс**.</span><span class="sxs-lookup"><span data-stu-id="6ef81-126">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="6ef81-127">Назовите новый класс *запуска* и добавьте его в проект.</span><span class="sxs-lookup"><span data-stu-id="6ef81-127">Name the new class *Startup* and add it to the project.</span></span>

1. <span data-ttu-id="6ef81-128">Замените код в *Startup.cs* файл класса следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="6ef81-128">Replace the code in the *Startup.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]

1. <span data-ttu-id="6ef81-129">В **обозревателе решений**выберите **контроллеров** > **HomeController.cs**.</span><span class="sxs-lookup"><span data-stu-id="6ef81-129">In **Solution Explorer**, select **Controllers** > **HomeController.cs**.</span></span>

1. <span data-ttu-id="6ef81-130">Добавьте следующий метод для *HomeController.cs*.</span><span class="sxs-lookup"><span data-stu-id="6ef81-130">Add this method to the *HomeController.cs*.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]

    <span data-ttu-id="6ef81-131">Этот метод возвращает **Chat** представление, которое создается на более позднем этапе.</span><span class="sxs-lookup"><span data-stu-id="6ef81-131">This method returns the **Chat** view that you create in a later step.</span></span>

1. <span data-ttu-id="6ef81-132">В **обозревателе решений**, щелкните правой кнопкой мыши **представления** > **Главная**и выберите **добавить**  >    **Представление**.</span><span class="sxs-lookup"><span data-stu-id="6ef81-132">In **Solution Explorer**, right-click **Views** > **Home**, and select **Add** >  **View**.</span></span>

1. <span data-ttu-id="6ef81-133">В **Добавление представления**, новому представлению имя **Chat** и выберите **добавить**.</span><span class="sxs-lookup"><span data-stu-id="6ef81-133">In **Add View**, name the new view **Chat** and select **Add**.</span></span>

1. <span data-ttu-id="6ef81-134">Замените содержимое файла **Chat.cshtml** следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="6ef81-134">Replace the contents of **Chat.cshtml** with this code:</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]

1. <span data-ttu-id="6ef81-135">В **обозревателе решений**, разверните **сценариев**.</span><span class="sxs-lookup"><span data-stu-id="6ef81-135">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="6ef81-136">Библиотеки скрипта для jQuery и SignalR, отображаются в проект.</span><span class="sxs-lookup"><span data-stu-id="6ef81-136">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="6ef81-137">Диспетчер пакетов может установить более позднюю версию сценариев SignalR.</span><span class="sxs-lookup"><span data-stu-id="6ef81-137">The package manager may have installed a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="6ef81-138">Убедитесь, что ссылки на скрипты в блоке кода соответствуют версиям файлов сценариев в проекте.</span><span class="sxs-lookup"><span data-stu-id="6ef81-138">Check that the script references in the code block correspond to the versions of the script files in the project.</span></span>

    <span data-ttu-id="6ef81-139">Ссылки на скрипты из исходного блока кода:</span><span class="sxs-lookup"><span data-stu-id="6ef81-139">Script references from the original code block:</span></span>

    ```cshtml
    <!--Script references. -->
    <!--The jQuery library is required and is referenced by default in _Layout.cshtml. -->
    <!--Reference the SignalR library. -->
    <script src="~/Scripts/jquery.signalR-2.1.0.min.js"></script>
    ```

1. <span data-ttu-id="6ef81-140">Если они не совпадают, обновите *.cshtml* файла.</span><span class="sxs-lookup"><span data-stu-id="6ef81-140">If they don't match, update the *.cshtml* file.</span></span>

1. <span data-ttu-id="6ef81-141">В строке меню выберите **файл** > **сохранить все**.</span><span class="sxs-lookup"><span data-stu-id="6ef81-141">From the menu bar, select **File** > **Save All**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="6ef81-142">Запуск образца</span><span class="sxs-lookup"><span data-stu-id="6ef81-142">Run the Sample</span></span>

1. <span data-ttu-id="6ef81-143">На панели инструментов, включите **отладки скриптов** и затем нажмите кнопку воспроизведения, чтобы запустить пример в режиме отладки.</span><span class="sxs-lookup"><span data-stu-id="6ef81-143">In the toolbar, turn on **Script Debugging** and then select the play button to run the sample in Debug mode.</span></span>

    ![Введите имя пользователя](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)

1. <span data-ttu-id="6ef81-145">Когда откроется браузер, введите имя для вашего удостоверения чата.</span><span class="sxs-lookup"><span data-stu-id="6ef81-145">When the browser opens, enter a name for your chat identity.</span></span>

1. <span data-ttu-id="6ef81-146">Скопируйте URL-адрес из браузера, откройте два других браузеров и вставьте URL-адреса в панели адрес.</span><span class="sxs-lookup"><span data-stu-id="6ef81-146">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="6ef81-147">В каждом браузере введите уникальное имя.</span><span class="sxs-lookup"><span data-stu-id="6ef81-147">In each browser, enter a unique name.</span></span>

1. <span data-ttu-id="6ef81-148">Теперь добавьте комментарий и выберите **отправки**.</span><span class="sxs-lookup"><span data-stu-id="6ef81-148">Now, add a comment and select **Send**.</span></span> <span data-ttu-id="6ef81-149">Повторите, в других браузерах.</span><span class="sxs-lookup"><span data-stu-id="6ef81-149">Repeat that in the other browsers.</span></span> <span data-ttu-id="6ef81-150">Комментарии отображаются в режиме реального времени.</span><span class="sxs-lookup"><span data-stu-id="6ef81-150">The comments appear in real time.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6ef81-151">Это приложение на простом чате не ведет контекста обсуждения на сервере.</span><span class="sxs-lookup"><span data-stu-id="6ef81-151">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="6ef81-152">Концентратор осуществляет широковещательную передачу комментарии для всех текущих пользователей.</span><span class="sxs-lookup"><span data-stu-id="6ef81-152">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="6ef81-153">Пользователи, беседа позже будет видеть сообщения добавлены с момента их присоединения к.</span><span class="sxs-lookup"><span data-stu-id="6ef81-153">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="6ef81-154">См. в разделе, запуск приложения чата в трех различных браузерах.</span><span class="sxs-lookup"><span data-stu-id="6ef81-154">See how the chat application runs in three different browsers.</span></span> <span data-ttu-id="6ef81-155">Когда Tom Ананд и Сьюзан отправляют сообщения, все браузеры обновление в режиме реального времени:</span><span class="sxs-lookup"><span data-stu-id="6ef81-155">When Tom, Anand, and Susan send messages, all browsers update in real time:</span></span>

    ![Все три браузеры отображают же журнала разговора](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

1. <span data-ttu-id="6ef81-157">В **обозревателе решений**, проверять **документы скриптов** узел для запущенного приложения.</span><span class="sxs-lookup"><span data-stu-id="6ef81-157">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="6ef81-158">Файл скрипта с именем *концентраторов* , библиотека SignalR создает во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="6ef81-158">There's a script file named *hubs* that the SignalR library generates at runtime.</span></span> <span data-ttu-id="6ef81-159">Этот файл управляет обменом данных между jQuery-сценарий и код на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="6ef81-159">This file manages the communication between jQuery script and server-side code.</span></span>

    ![автоматически созданное концентраторов сценария в узел документов скриптов](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)

## <a name="examine-the-code"></a><span data-ttu-id="6ef81-161">Изучите код</span><span class="sxs-lookup"><span data-stu-id="6ef81-161">Examine the Code</span></span>

<span data-ttu-id="6ef81-162">Приложение чата SignalR демонстрирует две основные задачи разработки SignalR.</span><span class="sxs-lookup"><span data-stu-id="6ef81-162">The SignalR chat application demonstrates two basic SignalR development tasks.</span></span> <span data-ttu-id="6ef81-163">Оно показано, как создать концентратор.</span><span class="sxs-lookup"><span data-stu-id="6ef81-163">It shows you how to create a hub.</span></span> <span data-ttu-id="6ef81-164">Сервер использует этот концентратор как объект основного координации.</span><span class="sxs-lookup"><span data-stu-id="6ef81-164">The server uses that hub as the main coordination object.</span></span> <span data-ttu-id="6ef81-165">Концентратор использует библиотеку jQuery SignalR для отправки и получения сообщений.</span><span class="sxs-lookup"><span data-stu-id="6ef81-165">The hub uses the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs-in-the-chathubcs"></a><span data-ttu-id="6ef81-166">Концентраторы SignalR в ChatHub.cs</span><span class="sxs-lookup"><span data-stu-id="6ef81-166">SignalR Hubs in the ChatHub.cs</span></span>

<span data-ttu-id="6ef81-167">В образце кода `ChatHub` класс является производным от `Microsoft.AspNet.SignalR.Hub` класса.</span><span class="sxs-lookup"><span data-stu-id="6ef81-167">In the code sample, the `ChatHub` class derives from the `Microsoft.AspNet.SignalR.Hub` class.</span></span> <span data-ttu-id="6ef81-168">Наследование от `Hub` класс является эффективным методом для построения приложения SignalR.</span><span class="sxs-lookup"><span data-stu-id="6ef81-168">Deriving from the `Hub` class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="6ef81-169">Можно создавать открытые методы в классе концентратора и затем получить доступ к этих методов, вызывая их с помощью сценариев на веб-странице.</span><span class="sxs-lookup"><span data-stu-id="6ef81-169">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="6ef81-170">В коде чата, клиенты вызывают `ChatHub.Send` метод для отправки нового сообщения.</span><span class="sxs-lookup"><span data-stu-id="6ef81-170">In the chat code, clients call the `ChatHub.Send` method to send a new message.</span></span> <span data-ttu-id="6ef81-171">Концентратор в свою очередь отправляет сообщение всем клиентам, вызвав `Clients.All.addNewMessageToPage`.</span><span class="sxs-lookup"><span data-stu-id="6ef81-171">The hub in turn sends the message to all clients by calling `Clients.All.addNewMessageToPage`.</span></span>

<span data-ttu-id="6ef81-172">`Send` Метод демонстрирует несколько вещей понятия:</span><span class="sxs-lookup"><span data-stu-id="6ef81-172">The `Send` method demonstrates several hub concepts:</span></span>

* <span data-ttu-id="6ef81-173">Объявления открытых методов концентратора, таким образом, клиенты могут вызывать их.</span><span class="sxs-lookup"><span data-stu-id="6ef81-173">Declare public methods on a hub so that clients can call them.</span></span>

* <span data-ttu-id="6ef81-174">Используйте `Microsoft.AspNet.SignalR.Hub.Clients` динамических свойств для взаимодействия со всеми клиентами, подключенных к этому концентратору.</span><span class="sxs-lookup"><span data-stu-id="6ef81-174">Use the `Microsoft.AspNet.SignalR.Hub.Clients` dynamic property to communicate with all clients connected to this hub.</span></span>

* <span data-ttu-id="6ef81-175">Вызов функции на стороне клиента (например `addNewMessageToPage` функции) для обновления клиентов.</span><span class="sxs-lookup"><span data-stu-id="6ef81-175">Call a function on the client (like the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery-chatcshtml"></a><span data-ttu-id="6ef81-176">SignalR и jQuery Chat.cshtml</span><span class="sxs-lookup"><span data-stu-id="6ef81-176">SignalR and jQuery Chat.cshtml</span></span>

<span data-ttu-id="6ef81-177">*Chat.cshtml* файл представления в примере кода показано, как использовать библиотеку jQuery SignalR для взаимодействия с центром SignalR.</span><span class="sxs-lookup"><span data-stu-id="6ef81-177">The *Chat.cshtml* view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span>  <span data-ttu-id="6ef81-178">Код выполняет ряд важных задач.</span><span class="sxs-lookup"><span data-stu-id="6ef81-178">The code carries out many important tasks.</span></span> <span data-ttu-id="6ef81-179">Она создает ссылку на автоматически созданные прокси-сервер для центра, объявляет функцию, сервер может обратиться для отправки содержимого клиентам, что он запускает подключение для отправки сообщений в концентратор.</span><span class="sxs-lookup"><span data-stu-id="6ef81-179">It creates a reference to the autogenerated proxy for the hub, declares a function that the server can call to push content to clients, and it starts a connection to send messages to the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="6ef81-180">В JavaScript ссылку на класс сервера и его членах находится в camelCase.</span><span class="sxs-lookup"><span data-stu-id="6ef81-180">In JavaScript, the reference to the server class and its members is in camelCase.</span></span> <span data-ttu-id="6ef81-181">Ссылки на код примера C# `ChatHub` класс в JavaScript в виде `chatHub`.</span><span class="sxs-lookup"><span data-stu-id="6ef81-181">The code sample references the C# `ChatHub` class in JavaScript as `chatHub`.</span></span>

<span data-ttu-id="6ef81-182">В этот блок кода как создать функцию обратного вызова в скрипте.</span><span class="sxs-lookup"><span data-stu-id="6ef81-182">In this code block, you create a callback function in the script.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

<span data-ttu-id="6ef81-183">Класс концентратора на сервере вызывает эту функцию для отправки обновлений содержимого для каждого клиента.</span><span class="sxs-lookup"><span data-stu-id="6ef81-183">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="6ef81-184">Дополнительного обращения к `htmlEncode` функция отображает способ HTML кодирования содержимое сообщения перед его отображением на странице.</span><span class="sxs-lookup"><span data-stu-id="6ef81-184">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page.</span></span> <span data-ttu-id="6ef81-185">Это позволяет предотвратить внедрение скрипта.</span><span class="sxs-lookup"><span data-stu-id="6ef81-185">It's a way to prevent script injection.</span></span>

<span data-ttu-id="6ef81-186">Этот код открывает соединение с концентратором.</span><span class="sxs-lookup"><span data-stu-id="6ef81-186">This code opens a connection with the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

> [!NOTE]
> <span data-ttu-id="6ef81-187">Такой подход гарантирует, что перед выполнением обработчика событий установить соединение.</span><span class="sxs-lookup"><span data-stu-id="6ef81-187">This approach ensures that you establish a connection before the event handler executes.</span></span>

<span data-ttu-id="6ef81-188">Код запускает подключение, а затем передает его функции, чтобы обрабатывать событие щелчка на **отправки** кнопку на данной странице чата.</span><span class="sxs-lookup"><span data-stu-id="6ef81-188">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6ef81-189">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="6ef81-189">Additional resources</span></span>

<span data-ttu-id="6ef81-190">Дополнительные сведения о SignalR см. следующие ресурсы:</span><span class="sxs-lookup"><span data-stu-id="6ef81-190">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="6ef81-191">Проект SignalR</span><span class="sxs-lookup"><span data-stu-id="6ef81-191">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="6ef81-192">SignalR GitHub и примерами</span><span class="sxs-lookup"><span data-stu-id="6ef81-192">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="6ef81-193">Вики-сайте SignalR</span><span class="sxs-lookup"><span data-stu-id="6ef81-193">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="6ef81-194">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="6ef81-194">Next steps</span></span>

<span data-ttu-id="6ef81-195">В этом руководстве вы:</span><span class="sxs-lookup"><span data-stu-id="6ef81-195">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6ef81-196">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="6ef81-196">Set up the project</span></span>
> * <span data-ttu-id="6ef81-197">Запустили пример</span><span class="sxs-lookup"><span data-stu-id="6ef81-197">Ran the sample</span></span>
> * <span data-ttu-id="6ef81-198">Проверить код</span><span class="sxs-lookup"><span data-stu-id="6ef81-198">Examined the code</span></span>

<span data-ttu-id="6ef81-199">Перейдите к следующей статье, чтобы научиться создавать веб-приложения, использующего ASP.NET SignalR 2 для предоставления функции обмена сообщениями с высокой частотой.</span><span class="sxs-lookup"><span data-stu-id="6ef81-199">Advance to the next article to learn how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="6ef81-200">Веб-приложения с высокой частотой обмена сообщениями</span><span class="sxs-lookup"><span data-stu-id="6ef81-200">Web app with high-frequency messaging</span></span>](tutorial-high-frequency-realtime-with-signalr.md)