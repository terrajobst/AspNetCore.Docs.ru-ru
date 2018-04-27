---
title: Приступая к работе с SignalR в ASP.NET Core
author: rachelappel
description: В этом учебнике создается приложение с помощью SignalR для ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 03/16/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started
ms.openlocfilehash: 03735359bb22cc3085ddc7b34372ecfc9501a940
ms.sourcegitcommit: 07903a1be39a99dcf538d57981161592d0e658b8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/20/2018
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="a9a21-103">Приступая к работе с SignalR в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a9a21-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="a9a21-104">Автор: [Рэйчел Аппель](https://twitter.com/rachelappel) (Rachel Appel)</span><span class="sxs-lookup"><span data-stu-id="a9a21-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

<span data-ttu-id="a9a21-105">В этом учебнике показано основы построения в режиме реального времени приложения с помощью SignalR для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a9a21-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Решение](get-started/_static/signalr-get-started-finished.png)

<span data-ttu-id="a9a21-107">В этом учебнике показано следующие задачи разработки SignalR:</span><span class="sxs-lookup"><span data-stu-id="a9a21-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a9a21-108">Создайте SignalR на веб-приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a9a21-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="a9a21-109">Создание концентратора SignalR для передачи содержимого клиентам.</span><span class="sxs-lookup"><span data-stu-id="a9a21-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="a9a21-110">Изменить `Startup` класс и настройки приложения.</span><span class="sxs-lookup"><span data-stu-id="a9a21-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="a9a21-111">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a9a21-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="prerequisites"></a><span data-ttu-id="a9a21-112">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="a9a21-112">Prerequisites</span></span>

<span data-ttu-id="a9a21-113">Установите следующее программное обеспечение:</span><span class="sxs-lookup"><span data-stu-id="a9a21-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a9a21-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9a21-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a9a21-115">[.NET core 2.1.0 предварительного просмотра 2 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview2) или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="a9a21-115">[.NET Core 2.1.0 Preview 2 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview2) or later</span></span>
* <span data-ttu-id="a9a21-116">[Visual Studio 2017 г](https://www.visualstudio.com/downloads/) 15,7 или более поздней версии с **ASP.NET и веб-разработки** рабочей нагрузки</span><span class="sxs-lookup"><span data-stu-id="a9a21-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="a9a21-117">npm</span><span class="sxs-lookup"><span data-stu-id="a9a21-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a9a21-118">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a9a21-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a9a21-119">[.NET core 2.1.0 предварительного просмотра 2 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview2) или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="a9a21-119">[.NET Core 2.1.0 Preview 2 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview2) or later</span></span>
* [<span data-ttu-id="a9a21-120">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a9a21-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="a9a21-121">C# для кода Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9a21-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="a9a21-122">npm</span><span class="sxs-lookup"><span data-stu-id="a9a21-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="a9a21-123">Создайте проект ASP.NET Core, на котором размещена SignalR клиента и сервера</span><span class="sxs-lookup"><span data-stu-id="a9a21-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a9a21-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9a21-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="a9a21-125">Используйте **файл** > **новый проект** меню и выберите **веб-приложения ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="a9a21-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="a9a21-126">Назовите проект *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="a9a21-126">Name the project *SignalRChat*.</span></span>

   ![Диалоговое окно нового проекта в Visual Studio](get-started/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="a9a21-128">Выберите **веб-приложения** для создания проекта с помощью страниц Razor.</span><span class="sxs-lookup"><span data-stu-id="a9a21-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="a9a21-129">Выберите **ОК**.</span><span class="sxs-lookup"><span data-stu-id="a9a21-129">Then select **OK**.</span></span> <span data-ttu-id="a9a21-130">Убедитесь, что **ASP.NET Core 2.1** выбирается из селектора framework, то выполняется SignalR в предыдущих версиях .NET.</span><span class="sxs-lookup"><span data-stu-id="a9a21-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Диалоговое окно нового проекта в Visual Studio](get-started/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="a9a21-132">Visual Studio включает `Microsoft.AspNetCore.SignalR` пакета, содержащего его сервера библиотек в рамках его **веб-приложения ASP.NET Core** шаблона.</span><span class="sxs-lookup"><span data-stu-id="a9a21-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="a9a21-133">Однако клиентская библиотека JavaScript для SignalR должны устанавливаться с помощью *npm*.</span><span class="sxs-lookup"><span data-stu-id="a9a21-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="a9a21-134">Выполните следующие команды **консоль диспетчера пакетов** окна, в корне проекта:</span><span class="sxs-lookup"><span data-stu-id="a9a21-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
      npm init -y
      npm install @aspnet/signalr
    ```     

4. <span data-ttu-id="a9a21-135">Копировать *signalr.js* файл из *node_modules\\ @aspnet\signalr\dist\browser*  для *lib* в папке проекта.</span><span class="sxs-lookup"><span data-stu-id="a9a21-135">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to the *lib* folder in your project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a9a21-136">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a9a21-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="a9a21-137">Из **интеграции терминалов**, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="a9a21-137">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
      dotnet new razor -o SignalRChat
    ```

2. <span data-ttu-id="a9a21-138">Установите библиотеку клиента JavaScript с помощью *npm*.</span><span class="sxs-lookup"><span data-stu-id="a9a21-138">Install the JavaScript client library using *npm*.</span></span>

    ```
      npm init -y
      npm install @aspnet/signalr
    ```

-----

## <a name="create-the-signalr-hub"></a><span data-ttu-id="a9a21-139">Создание концентратора SignalR</span><span class="sxs-lookup"><span data-stu-id="a9a21-139">Create the SignalR Hub</span></span>

<span data-ttu-id="a9a21-140">Концентратор является класс, который служит в качестве высокого уровня конвейера, который позволяет клиенту и серверу для вызова методов друг от друга.</span><span class="sxs-lookup"><span data-stu-id="a9a21-140">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a9a21-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9a21-141">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="a9a21-142">Добавить класс в проект, выбрав **файл** > **New** > **файл** и выбрав **классов Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="a9a21-142">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span>

2. <span data-ttu-id="a9a21-143">Наследовать от `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="a9a21-143">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="a9a21-144">`Hub` Класс содержит свойства и события для управления подключения и группы, а также отправки и получения данных.</span><span class="sxs-lookup"><span data-stu-id="a9a21-144">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="a9a21-145">Создание `SendMessage` метод, который отправляет сообщение всем клиентам, подключенным чат.</span><span class="sxs-lookup"><span data-stu-id="a9a21-145">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="a9a21-146">Обратите внимание на то, она возвращает [задачи](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), так как SignalR является асинхронным.</span><span class="sxs-lookup"><span data-stu-id="a9a21-146">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="a9a21-147">Лучше масштабируется асинхронного кода.</span><span class="sxs-lookup"><span data-stu-id="a9a21-147">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a9a21-148">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a9a21-148">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="a9a21-149">Откройте *SignalRChat* папки в коде Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a9a21-149">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="a9a21-150">Добавить класс в проект, выбрав **файл** > **новый файл** в меню.</span><span class="sxs-lookup"><span data-stu-id="a9a21-150">Add a class to the project by selecting **File** > **New File** from the menu.</span></span>

3. <span data-ttu-id="a9a21-151">Наследовать от `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="a9a21-151">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="a9a21-152">`Hub` Класс содержит свойства и события для управления подключения и группы, а также отправки и получения данных для клиентов.</span><span class="sxs-lookup"><span data-stu-id="a9a21-152">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="a9a21-153">Добавьте в класс метод `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="a9a21-153">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="a9a21-154">`SendMessage` Метод отправляет сообщение всем клиентам, подключенным чат.</span><span class="sxs-lookup"><span data-stu-id="a9a21-154">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="a9a21-155">Обратите внимание на то, она возвращает [задачи](/dotnet/api/system.threading.tasks.task), так как SignalR является асинхронным.</span><span class="sxs-lookup"><span data-stu-id="a9a21-155">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="a9a21-156">Лучше масштабируется асинхронного кода.</span><span class="sxs-lookup"><span data-stu-id="a9a21-156">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=6-12)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="a9a21-157">Настроить проект для использования SignalR</span><span class="sxs-lookup"><span data-stu-id="a9a21-157">Configure the project to use SignalR</span></span>

<span data-ttu-id="a9a21-158">SignalR сервера необходимо настроить, чтобы он доверял передачи запросов для SignalR.</span><span class="sxs-lookup"><span data-stu-id="a9a21-158">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="a9a21-159">Чтобы настроить проект SignalR, работать с проектом `Startup.ConfigureServices` метод.</span><span class="sxs-lookup"><span data-stu-id="a9a21-159">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="a9a21-160">`services.AddSignalR` Добавляет в составе SignalR [по промежуточного слоя](xref:fundamentals/middleware/index) конвейера.</span><span class="sxs-lookup"><span data-stu-id="a9a21-160">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

2. <span data-ttu-id="a9a21-161">Настроить маршруты для концентраторов с помощью `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="a9a21-161">Configure routes to your hubs using `UseSignalR`.</span></span>


   [!code-csharp[Startup](get-started/sample/Startup.cs?highlight=36,56-59)]


## <a name="create-the-signalr-client-code"></a><span data-ttu-id="a9a21-162">Создайте код клиента SignalR</span><span class="sxs-lookup"><span data-stu-id="a9a21-162">Create the SignalR client code</span></span>

1. <span data-ttu-id="a9a21-163">Замените содержимое в *Pages\Index.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="a9a21-163">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](get-started/sample/Pages/Index.cshtml)]

   <span data-ttu-id="a9a21-164">Предыдущий HTML отображает имя и поля сообщения и кнопка отправки.</span><span class="sxs-lookup"><span data-stu-id="a9a21-164">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="a9a21-165">Обратите внимание, ссылки на скрипты в нижней: ссылку на SignalR и *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="a9a21-165">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

2. <span data-ttu-id="a9a21-166">Добавьте файл JavaScript с именем *chat.js*в *wwwroot\js* папки.</span><span class="sxs-lookup"><span data-stu-id="a9a21-166">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="a9a21-167">Добавьте в этот файл следующий код:</span><span class="sxs-lookup"><span data-stu-id="a9a21-167">Add the following code to it:</span></span>

   [!code-javascript[Index](get-started/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="a9a21-168">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="a9a21-168">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a9a21-169">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9a21-169">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="a9a21-170">Выберите **отладки** > **Запуск без отладки** запускает браузер и загрузить веб-сайт локально.</span><span class="sxs-lookup"><span data-stu-id="a9a21-170">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="a9a21-171">Скопируйте URL-адрес из адресной строки.</span><span class="sxs-lookup"><span data-stu-id="a9a21-171">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="a9a21-172">Откройте другой экземпляр браузера (любой браузер) и вставьте URL-адрес в адресной строке.</span><span class="sxs-lookup"><span data-stu-id="a9a21-172">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="a9a21-173">Выберите любой браузер, введите имя и сообщение и нажмите кнопку **отправки** кнопки.</span><span class="sxs-lookup"><span data-stu-id="a9a21-173">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="a9a21-174">Имя и сообщения отображаются на обеих страницах мгновенно.</span><span class="sxs-lookup"><span data-stu-id="a9a21-174">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a9a21-175">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a9a21-175">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="a9a21-176">Нажмите клавишу **отладки** (F5), чтобы выполнить сборку программы и запустить ее.</span><span class="sxs-lookup"><span data-stu-id="a9a21-176">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="a9a21-177">Запуск программы открывает окно браузера.</span><span class="sxs-lookup"><span data-stu-id="a9a21-177">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="a9a21-178">Открытие другого окна браузера и загрузить локально в веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="a9a21-178">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="a9a21-179">Выберите любой браузер, введите имя и сообщение и нажмите кнопку **отправки** кнопки.</span><span class="sxs-lookup"><span data-stu-id="a9a21-179">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="a9a21-180">Имя и сообщения отображаются на обеих страницах мгновенно.</span><span class="sxs-lookup"><span data-stu-id="a9a21-180">The name and message are displayed on both pages instantly.</span></span>

-----

  ![Решение](get-started/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="a9a21-182">Связанные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a9a21-182">Related resources</span></span>

[<span data-ttu-id="a9a21-183">Введение в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="a9a21-183">Introduction to ASP.NET Core SignalR</span></span>](introduction.md)