---
title: "Приступая к работе с SignalR в ASP.NET Core"
author: rachelappel
description: "В этом учебнике создается приложение с помощью SignalR для ASP.NET Core."
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/16/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started-signalr-core
ms.openlocfilehash: 139da5a2d0dadf51fece94b7c54ccd531e0ae8c2
ms.sourcegitcommit: 6548a3dd0cd1e3e92ac2310dee757ddad9fd6456
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/16/2018
---
# <a name="tutorial-get-started-with-signalr-for-aspnet-core"></a><span data-ttu-id="7406d-103">Учебник: Приступая к работе с SignalR для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7406d-103">Tutorial: Get started with SignalR for ASP.NET Core</span></span>

<span data-ttu-id="7406d-104">Автор: [Рэйчел Аппель](https://twitter.com/rachelappel) (Rachel Appel)</span><span class="sxs-lookup"><span data-stu-id="7406d-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="7406d-105">В этом учебнике показано основы построения в режиме реального времени приложения с помощью SignalR для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7406d-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Решение](get-started-signalr-core/_static/signalr-get-started-finished.png)

<span data-ttu-id="7406d-107">В этом учебнике показано следующие задачи разработки SignalR:</span><span class="sxs-lookup"><span data-stu-id="7406d-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7406d-108">Создайте SignalR на веб-приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7406d-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="7406d-109">Создание концентратора SignalR для передачи содержимого клиентам.</span><span class="sxs-lookup"><span data-stu-id="7406d-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="7406d-110">Изменить `Startup` класс и настройки приложения.</span><span class="sxs-lookup"><span data-stu-id="7406d-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="7406d-111">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7406d-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="prerequisites"></a><span data-ttu-id="7406d-112">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="7406d-112">Prerequisites</span></span>

<span data-ttu-id="7406d-113">Установите следующее программное обеспечение:</span><span class="sxs-lookup"><span data-stu-id="7406d-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7406d-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7406d-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7406d-115">[.NET core 2.1.0 предварительного просмотра 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="7406d-115">[.NET Core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) or later</span></span>
* <span data-ttu-id="7406d-116">[Visual Studio 2017 г](https://www.visualstudio.com/downloads/) 15.6 или более поздней версии с **ASP.NET и веб-разработки** рабочей нагрузки</span><span class="sxs-lookup"><span data-stu-id="7406d-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.6 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="7406d-117">npm</span><span class="sxs-lookup"><span data-stu-id="7406d-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7406d-118">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="7406d-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="7406d-119">[.NET core 2.1.0 предварительного просмотра 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="7406d-119">[.NET Core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) or later</span></span>
* [<span data-ttu-id="7406d-120">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="7406d-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download) 
* [<span data-ttu-id="7406d-121">C# для кода Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7406d-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="7406d-122">npm</span><span class="sxs-lookup"><span data-stu-id="7406d-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="7406d-123">Создайте проект ASP.NET Core, на котором размещена SignalR клиента и сервера</span><span class="sxs-lookup"><span data-stu-id="7406d-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7406d-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7406d-124">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="7406d-125">Используйте **файл** > **новый проект** меню и выберите **веб-приложения ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="7406d-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="7406d-126">Назовите проект *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="7406d-126">Name the project *SignalRChat*.</span></span>

  ![Диалоговое окно нового проекта в Visual Studio](get-started-signalr-core/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="7406d-128">Выберите **веб-приложения** для создания проекта с помощью страниц Razor.</span><span class="sxs-lookup"><span data-stu-id="7406d-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="7406d-129">Выберите **ОК**.</span><span class="sxs-lookup"><span data-stu-id="7406d-129">Then select **OK**.</span></span> <span data-ttu-id="7406d-130">Убедитесь, что **ASP.NET Core 2.1** выбирается из селектора framework, то выполняется SignalR в предыдущих версиях .NET.</span><span class="sxs-lookup"><span data-stu-id="7406d-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

  ![Диалоговое окно нового проекта в Visual Studio](get-started-signalr-core/_static/signalr-new-project-choose-type.png)

3. <span data-ttu-id="7406d-132">Щелкните правой кнопкой мыши проект в **обозревателе решений** > **добавить** > **новый элемент** > **файл конфигурации npm** .</span><span class="sxs-lookup"><span data-stu-id="7406d-132">Right-click the project in **Solution Explorer** > **Add** > **New Item** > **npm Configuration File**.</span></span> <span data-ttu-id="7406d-133">Назовите файл *package.json*.</span><span class="sxs-lookup"><span data-stu-id="7406d-133">Name the file *package.json*.</span></span>

4. <span data-ttu-id="7406d-134">Выполните следующую команду **консоль диспетчера пакетов** окна, в корне проекта:</span><span class="sxs-lookup"><span data-stu-id="7406d-134">Run the following command in the **Package Manager Console** window, from the project root:</span></span>

    ```console
      npm install @aspnet/signalr
    ```
5. <span data-ttu-id="7406d-135">Копировать *signalr.js* файл из *node_modules\\ @aspnet\signalr\dist\browser*  для *wwwroot\lib* в папке проекта.</span><span class="sxs-lookup"><span data-stu-id="7406d-135">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to the *wwwroot\lib* folder in your project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7406d-136">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="7406d-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="7406d-137">Из **интеграции терминалов**, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="7406d-137">From the **Integrated Terminal**, run the following command:</span></span>
 
    ```console
      dotnet new razor -o SignalRChat
    ```

2. <span data-ttu-id="7406d-138">Установите библиотеку клиента JavaScript с помощью *npm*.</span><span class="sxs-lookup"><span data-stu-id="7406d-138">Install the JavaScript client library using *npm*.</span></span>

    ```
      npm init -y
      npm install @aspnet/signalr
    ```

-----

## <a name="create-the-signalr-hub"></a><span data-ttu-id="7406d-139">Создание концентратора SignalR</span><span class="sxs-lookup"><span data-stu-id="7406d-139">Create the SignalR Hub</span></span>

<span data-ttu-id="7406d-140">Концентратор является класс, который служит в качестве высокого уровня конвейера, который позволяет клиенту и серверу для вызова методов друг от друга.</span><span class="sxs-lookup"><span data-stu-id="7406d-140">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7406d-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7406d-141">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="7406d-142">Добавить класс в проект, выбрав **файл** > **New** > **файл** и выбрав **классов Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="7406d-142">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span>

1. <span data-ttu-id="7406d-143">Наследовать от `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="7406d-143">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="7406d-144">`Hub` Класс содержит свойства и события для управления подключения и группы, а также отправки и получения данных.</span><span class="sxs-lookup"><span data-stu-id="7406d-144">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

1. <span data-ttu-id="7406d-145">Создание `SendMessage` метод, который отправляет сообщение всем клиентам, подключенным чат.</span><span class="sxs-lookup"><span data-stu-id="7406d-145">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="7406d-146">Обратите внимание на то, она возвращает [задачи](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task(v=vs.110).aspx), так как SignalR является асинхронным.</span><span class="sxs-lookup"><span data-stu-id="7406d-146">Notice it returns a [Task](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="7406d-147">Лучше масштабируется асинхронного кода.</span><span class="sxs-lookup"><span data-stu-id="7406d-147">Asynchronous code scales better.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7406d-148">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="7406d-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="7406d-149">Откройте *SignalRChat* папки в коде Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7406d-149">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

1. <span data-ttu-id="7406d-150">Добавить класс в проект, выбрав **файл** > **новый файл** в меню.</span><span class="sxs-lookup"><span data-stu-id="7406d-150">Add a class to the project by selecting **File** > **New File** from the menu.</span></span>

1. <span data-ttu-id="7406d-151">Наследовать от `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="7406d-151">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="7406d-152">`Hub` Класс содержит свойства и события для управления подключения и группы, а также отправки и получения данных для клиентов.</span><span class="sxs-lookup"><span data-stu-id="7406d-152">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

1. <span data-ttu-id="7406d-153">Добавьте в класс метод `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="7406d-153">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="7406d-154">`SendMessage` Метод отправляет сообщение всем клиентам, подключенным чат.</span><span class="sxs-lookup"><span data-stu-id="7406d-154">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="7406d-155">Обратите внимание на то, она возвращает [задачи](/dotnet/api/system.threading.tasks.task), так как SignalR является асинхронным.</span><span class="sxs-lookup"><span data-stu-id="7406d-155">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="7406d-156">Лучше масштабируется асинхронного кода.</span><span class="sxs-lookup"><span data-stu-id="7406d-156">Asynchronous code scales better.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="7406d-157">Настроить проект для использования SignalR</span><span class="sxs-lookup"><span data-stu-id="7406d-157">Configure the project to use SignalR</span></span>

<span data-ttu-id="7406d-158">SignalR сервера необходимо настроить, чтобы он доверял передачи запросов для SignalR.</span><span class="sxs-lookup"><span data-stu-id="7406d-158">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="7406d-159">Чтобы настроить проект SignalR, работать с проектом `Startup.ConfigureServices` метод.</span><span class="sxs-lookup"><span data-stu-id="7406d-159">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

  <span data-ttu-id="7406d-160">`services.AddSignalR` Добавляет в составе SignalR [по промежуточного слоя](xref:fundamentals/middleware/index) конвейера.</span><span class="sxs-lookup"><span data-stu-id="7406d-160">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

1. <span data-ttu-id="7406d-161">Настроить маршруты для концентраторов с помощью `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="7406d-161">Configure routes to your hubs using `UseSignalR`.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="7406d-162">Создайте код клиента SignalR</span><span class="sxs-lookup"><span data-stu-id="7406d-162">Create the SignalR client code</span></span>

1. <span data-ttu-id="7406d-163">Замените содержимое в *Pages\Index.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="7406d-163">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](get-started-signalr-core/sample/Pages/Index.cshtml)]

  <span data-ttu-id="7406d-164">Предыдущий HTML отображает имя и поля сообщения и кнопка отправки.</span><span class="sxs-lookup"><span data-stu-id="7406d-164">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="7406d-165">Обратите внимание, ссылки на скрипты в нижней: ссылку на SignalR и *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="7406d-165">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

1. <span data-ttu-id="7406d-166">Добавьте файл JavaScript с именем *chat.js*в *wwwroot\js* папки.</span><span class="sxs-lookup"><span data-stu-id="7406d-166">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="7406d-167">Добавьте в этот файл следующий код:</span><span class="sxs-lookup"><span data-stu-id="7406d-167">Add the following code to it:</span></span>

  [!code-javascript[Index](get-started-signalr-core/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="7406d-168">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="7406d-168">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7406d-169">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7406d-169">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="7406d-170">Выберите **отладки** > **Запуск без отладки** запускает браузер и загрузить веб-сайт локально.</span><span class="sxs-lookup"><span data-stu-id="7406d-170">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="7406d-171">Скопируйте URL-адрес из адресной строки.</span><span class="sxs-lookup"><span data-stu-id="7406d-171">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="7406d-172">Откройте другой экземпляр браузера (любой браузер) и вставьте URL-адрес в адресной строке.</span><span class="sxs-lookup"><span data-stu-id="7406d-172">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="7406d-173">Выберите любой браузер, введите имя и сообщение и нажмите кнопку **отправки** кнопки.</span><span class="sxs-lookup"><span data-stu-id="7406d-173">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="7406d-174">Имя и сообщения отображаются на обеих страницах мгновенно.</span><span class="sxs-lookup"><span data-stu-id="7406d-174">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7406d-175">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="7406d-175">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="7406d-176">Нажмите клавишу **отладки** (F5), чтобы выполнить сборку программы и запустить ее.</span><span class="sxs-lookup"><span data-stu-id="7406d-176">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="7406d-177">Запуск программы открывает окно браузера.</span><span class="sxs-lookup"><span data-stu-id="7406d-177">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="7406d-178">Открытие другого окна браузера и загрузить локально в веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="7406d-178">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="7406d-179">Выберите любой браузер, введите имя и сообщение и нажмите кнопку **отправки** кнопки.</span><span class="sxs-lookup"><span data-stu-id="7406d-179">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="7406d-180">Имя и сообщения отображаются на обеих страницах мгновенно.</span><span class="sxs-lookup"><span data-stu-id="7406d-180">The name and message are displayed on both pages instantly.</span></span>

-----

  ![Решение](get-started-signalr-core/_static/signalr-get-started-finished.png)