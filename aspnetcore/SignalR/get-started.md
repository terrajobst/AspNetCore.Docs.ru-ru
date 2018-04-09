---
title: Приступая к работе с SignalR в ASP.NET Core
author: rachelappel
description: В этом учебнике создается приложение с помощью SignalR для ASP.NET Core.
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/16/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started
ms.openlocfilehash: cf120d535c85c7871f5b1f27039018ea2405b9cb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="d1990-103">Приступая к работе с SignalR в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d1990-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="d1990-104">Автор: [Рэйчел Аппель](https://twitter.com/rachelappel) (Rachel Appel)</span><span class="sxs-lookup"><span data-stu-id="d1990-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

[!INCLUDE [Version notice](../includes/signalr-version-notice.md)]

<span data-ttu-id="d1990-105">В этом учебнике показано основы построения в режиме реального времени приложения с помощью SignalR для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d1990-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Решение](get-started/_static/signalr-get-started-finished.png)

<span data-ttu-id="d1990-107">В этом учебнике показано следующие задачи разработки SignalR:</span><span class="sxs-lookup"><span data-stu-id="d1990-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d1990-108">Создайте SignalR на веб-приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d1990-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="d1990-109">Создание концентратора SignalR для передачи содержимого клиентам.</span><span class="sxs-lookup"><span data-stu-id="d1990-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="d1990-110">Изменить `Startup` класс и настройки приложения.</span><span class="sxs-lookup"><span data-stu-id="d1990-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="d1990-111">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d1990-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="prerequisites"></a><span data-ttu-id="d1990-112">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="d1990-112">Prerequisites</span></span>

<span data-ttu-id="d1990-113">Установите следующее программное обеспечение:</span><span class="sxs-lookup"><span data-stu-id="d1990-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d1990-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d1990-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d1990-115">[.NET core 2.1.0 предварительного просмотра 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="d1990-115">[.NET Core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) or later</span></span>
* <span data-ttu-id="d1990-116">[Visual Studio 2017 г](https://www.visualstudio.com/downloads/) 15.6 или более поздней версии с **ASP.NET и веб-разработки** рабочей нагрузки</span><span class="sxs-lookup"><span data-stu-id="d1990-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.6 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="d1990-117">npm</span><span class="sxs-lookup"><span data-stu-id="d1990-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d1990-118">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d1990-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="d1990-119">[.NET core 2.1.0 предварительного просмотра 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="d1990-119">[.NET Core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) or later</span></span>
* [<span data-ttu-id="d1990-120">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d1990-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download) 
* [<span data-ttu-id="d1990-121">C# для кода Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d1990-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="d1990-122">npm</span><span class="sxs-lookup"><span data-stu-id="d1990-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="d1990-123">Создайте проект ASP.NET Core, на котором размещена SignalR клиента и сервера</span><span class="sxs-lookup"><span data-stu-id="d1990-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

#### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d1990-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d1990-124">Visual Studio</span></span>](#tab/visual-studio/)
1. <span data-ttu-id="d1990-125">Используйте **файл** > **новый проект** меню и выберите **веб-приложения ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="d1990-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="d1990-126">Назовите проект *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="d1990-126">Name the project *SignalRChat*.</span></span>

   ![Диалоговое окно нового проекта в Visual Studio](get-started/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="d1990-128">Выберите **веб-приложения** для создания проекта с помощью страниц Razor.</span><span class="sxs-lookup"><span data-stu-id="d1990-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="d1990-129">Выберите **ОК**.</span><span class="sxs-lookup"><span data-stu-id="d1990-129">Then select **OK**.</span></span> <span data-ttu-id="d1990-130">Убедитесь, что **ASP.NET Core 2.1** выбирается из селектора framework, то выполняется SignalR в предыдущих версиях .NET.</span><span class="sxs-lookup"><span data-stu-id="d1990-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Диалоговое окно нового проекта в Visual Studio](get-started/_static/signalr-new-project-choose-type.png)

3. <span data-ttu-id="d1990-132">Щелкните правой кнопкой мыши проект в **обозревателе решений** > **добавить** > **новый элемент** > **файл конфигурации npm** .</span><span class="sxs-lookup"><span data-stu-id="d1990-132">Right-click the project in **Solution Explorer** > **Add** > **New Item** > **npm Configuration File**.</span></span> <span data-ttu-id="d1990-133">Назовите файл *package.json*.</span><span class="sxs-lookup"><span data-stu-id="d1990-133">Name the file *package.json*.</span></span>

4. <span data-ttu-id="d1990-134">Выполните следующую команду **консоль диспетчера пакетов** окна, в корне проекта:</span><span class="sxs-lookup"><span data-stu-id="d1990-134">Run the following command in the **Package Manager Console** window, from the project root:</span></span>

    ```console
      npm install @aspnet/signalr
    ```
5. <span data-ttu-id="d1990-135">Копировать <em>signalr.js</em> файл из <em>node_modules\\ @aspnet\signalr\dist\browser</em>  для <em>wwwroot\lib</em> в папке проекта.</span><span class="sxs-lookup"><span data-stu-id="d1990-135">Copy the <em>signalr.js</em> file from <em>node_modules\\@aspnet\signalr\dist\browser</em> to the <em>wwwroot\lib</em> folder in your project.</span></span>

#### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d1990-136">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d1990-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)
1. <span data-ttu-id="d1990-137">Из **интеграции терминалов**, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="d1990-137">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
      dotnet new razor -o SignalRChat
    ```

2. <span data-ttu-id="d1990-138">Установите библиотеку клиента JavaScript с помощью *npm*.</span><span class="sxs-lookup"><span data-stu-id="d1990-138">Install the JavaScript client library using *npm*.</span></span>

    ```
      npm init -y
      npm install @aspnet/signalr
    ```

* * *
## <a name="create-the-signalr-hub"></a><span data-ttu-id="d1990-139">Создание концентратора SignalR</span><span class="sxs-lookup"><span data-stu-id="d1990-139">Create the SignalR Hub</span></span>

<span data-ttu-id="d1990-140">Концентратор является класс, который служит в качестве высокого уровня конвейера, который позволяет клиенту и серверу для вызова методов друг от друга.</span><span class="sxs-lookup"><span data-stu-id="d1990-140">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

#### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d1990-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d1990-141">Visual Studio</span></span>](#tab/visual-studio/)
1. <span data-ttu-id="d1990-142">Добавить класс в проект, выбрав **файл** > **New** > **файл** и выбрав **классов Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="d1990-142">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span>

2. <span data-ttu-id="d1990-143">Наследовать от `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="d1990-143">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="d1990-144">`Hub` Класс содержит свойства и события для управления подключения и группы, а также отправки и получения данных.</span><span class="sxs-lookup"><span data-stu-id="d1990-144">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="d1990-145">Создание `SendMessage` метод, который отправляет сообщение всем клиентам, подключенным чат.</span><span class="sxs-lookup"><span data-stu-id="d1990-145">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="d1990-146">Обратите внимание на то, она возвращает [задачи](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task(v=vs.110).aspx), так как SignalR является асинхронным.</span><span class="sxs-lookup"><span data-stu-id="d1990-146">Notice it returns a [Task](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="d1990-147">Лучше масштабируется асинхронного кода.</span><span class="sxs-lookup"><span data-stu-id="d1990-147">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=7-14)]

#### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d1990-148">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d1990-148">Visual Studio Code</span></span>](#tab/visual-studio-code/)
1. <span data-ttu-id="d1990-149">Откройте *SignalRChat* папки в коде Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d1990-149">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="d1990-150">Добавить класс в проект, выбрав **файл** > **новый файл** в меню.</span><span class="sxs-lookup"><span data-stu-id="d1990-150">Add a class to the project by selecting **File** > **New File** from the menu.</span></span>

3. <span data-ttu-id="d1990-151">Наследовать от `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="d1990-151">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="d1990-152">`Hub` Класс содержит свойства и события для управления подключения и группы, а также отправки и получения данных для клиентов.</span><span class="sxs-lookup"><span data-stu-id="d1990-152">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="d1990-153">Добавьте в класс метод `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="d1990-153">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="d1990-154">`SendMessage` Метод отправляет сообщение всем клиентам, подключенным чат.</span><span class="sxs-lookup"><span data-stu-id="d1990-154">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="d1990-155">Обратите внимание на то, она возвращает [задачи](/dotnet/api/system.threading.tasks.task), так как SignalR является асинхронным.</span><span class="sxs-lookup"><span data-stu-id="d1990-155">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="d1990-156">Лучше масштабируется асинхронного кода.</span><span class="sxs-lookup"><span data-stu-id="d1990-156">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=7-14)]

* * *
## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="d1990-157">Настроить проект для использования SignalR</span><span class="sxs-lookup"><span data-stu-id="d1990-157">Configure the project to use SignalR</span></span>

<span data-ttu-id="d1990-158">SignalR сервера необходимо настроить, чтобы он доверял передачи запросов для SignalR.</span><span class="sxs-lookup"><span data-stu-id="d1990-158">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="d1990-159">Чтобы настроить проект SignalR, работать с проектом `Startup.ConfigureServices` метод.</span><span class="sxs-lookup"><span data-stu-id="d1990-159">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="d1990-160">`services.AddSignalR` Добавляет в составе SignalR [по промежуточного слоя](xref:fundamentals/middleware/index) конвейера.</span><span class="sxs-lookup"><span data-stu-id="d1990-160">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

2. <span data-ttu-id="d1990-161">Настроить маршруты для концентраторов с помощью `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="d1990-161">Configure routes to your hubs using `UseSignalR`.</span></span>

   [!code-csharp[Startup](get-started/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="d1990-162">Создайте код клиента SignalR</span><span class="sxs-lookup"><span data-stu-id="d1990-162">Create the SignalR client code</span></span>

1. <span data-ttu-id="d1990-163">Замените содержимое в *Pages\Index.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="d1990-163">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](get-started/sample/Pages/Index.cshtml)]

   <span data-ttu-id="d1990-164">Предыдущий HTML отображает имя и поля сообщения и кнопка отправки.</span><span class="sxs-lookup"><span data-stu-id="d1990-164">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="d1990-165">Обратите внимание, ссылки на скрипты в нижней: ссылку на SignalR и *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="d1990-165">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

2. <span data-ttu-id="d1990-166">Добавьте файл JavaScript с именем *chat.js*в *wwwroot\js* папки.</span><span class="sxs-lookup"><span data-stu-id="d1990-166">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="d1990-167">Добавьте в этот файл следующий код:</span><span class="sxs-lookup"><span data-stu-id="d1990-167">Add the following code to it:</span></span>

   [!code-javascript[Index](get-started/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="d1990-168">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="d1990-168">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d1990-169">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d1990-169">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="d1990-170">Выберите **отладки** > **Запуск без отладки** запускает браузер и загрузить веб-сайт локально.</span><span class="sxs-lookup"><span data-stu-id="d1990-170">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="d1990-171">Скопируйте URL-адрес из адресной строки.</span><span class="sxs-lookup"><span data-stu-id="d1990-171">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="d1990-172">Откройте другой экземпляр браузера (любой браузер) и вставьте URL-адрес в адресной строке.</span><span class="sxs-lookup"><span data-stu-id="d1990-172">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="d1990-173">Выберите любой браузер, введите имя и сообщение и нажмите кнопку **отправки** кнопки.</span><span class="sxs-lookup"><span data-stu-id="d1990-173">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="d1990-174">Имя и сообщения отображаются на обеих страницах мгновенно.</span><span class="sxs-lookup"><span data-stu-id="d1990-174">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d1990-175">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d1990-175">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="d1990-176">Нажмите клавишу **отладки** (F5), чтобы выполнить сборку программы и запустить ее.</span><span class="sxs-lookup"><span data-stu-id="d1990-176">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="d1990-177">Запуск программы открывает окно браузера.</span><span class="sxs-lookup"><span data-stu-id="d1990-177">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="d1990-178">Открытие другого окна браузера и загрузить локально в веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="d1990-178">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="d1990-179">Выберите любой браузер, введите имя и сообщение и нажмите кнопку **отправки** кнопки.</span><span class="sxs-lookup"><span data-stu-id="d1990-179">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="d1990-180">Имя и сообщения отображаются на обеих страницах мгновенно.</span><span class="sxs-lookup"><span data-stu-id="d1990-180">The name and message are displayed on both pages instantly.</span></span>

-----

  ![Решение](get-started/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="d1990-182">Связанные ресурсы</span><span class="sxs-lookup"><span data-stu-id="d1990-182">Related resources</span></span>

[<span data-ttu-id="d1990-183">Введение в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="d1990-183">Introduction to ASP.NET Core SignalR</span></span>](introduction.md)