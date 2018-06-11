---
title: Приступая к работе с SignalR в ASP.NET Core
author: rachelappel
description: В этом учебнике создается приложение с помощью SignalR для ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started
ms.openlocfilehash: c71d98f86c15a4c6fbbe400f912123419b4ad076
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252208"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="eaeb8-103">Приступая к работе с SignalR в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eaeb8-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="eaeb8-104">Автор: [Рэйчел Аппель](https://twitter.com/rachelappel) (Rachel Appel)</span><span class="sxs-lookup"><span data-stu-id="eaeb8-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="eaeb8-105">В этом учебнике показано основы построения в режиме реального времени приложения с помощью SignalR для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Решение](get-started/_static/signalr-get-started-finished.png)

<span data-ttu-id="eaeb8-107">В этом учебнике показано следующие задачи разработки SignalR:</span><span class="sxs-lookup"><span data-stu-id="eaeb8-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="eaeb8-108">Создайте SignalR на веб-приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="eaeb8-109">Создание концентратора SignalR для передачи содержимого клиентам.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="eaeb8-110">Изменить `Startup` класс и настройки приложения.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="eaeb8-111">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="eaeb8-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="prerequisites"></a><span data-ttu-id="eaeb8-112">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="eaeb8-112">Prerequisites</span></span>

<span data-ttu-id="eaeb8-113">Установите следующее программное обеспечение:</span><span class="sxs-lookup"><span data-stu-id="eaeb8-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="eaeb8-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eaeb8-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="eaeb8-115">.NET core SDK 2.1 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="eaeb8-115">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="eaeb8-116">[Visual Studio 2017 г](https://www.visualstudio.com/downloads/) 15,7 или более поздней версии с **ASP.NET и веб-разработки** рабочей нагрузки</span><span class="sxs-lookup"><span data-stu-id="eaeb8-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="eaeb8-117">npm</span><span class="sxs-lookup"><span data-stu-id="eaeb8-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="eaeb8-118">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="eaeb8-119">.NET core SDK 2.1 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="eaeb8-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="eaeb8-120">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="eaeb8-121">C# для кода Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eaeb8-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="eaeb8-122">npm</span><span class="sxs-lookup"><span data-stu-id="eaeb8-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="eaeb8-123">Создайте проект ASP.NET Core, на котором размещена SignalR клиента и сервера</span><span class="sxs-lookup"><span data-stu-id="eaeb8-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="eaeb8-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eaeb8-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="eaeb8-125">Используйте **файл** > **новый проект** меню и выберите **веб-приложения ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="eaeb8-126">Назовите проект *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-126">Name the project *SignalRChat*.</span></span>

   ![Диалоговое окно нового проекта в Visual Studio](get-started/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="eaeb8-128">Выберите **веб-приложения** для создания проекта с помощью страниц Razor.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="eaeb8-129">Выберите **ОК**.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-129">Then select **OK**.</span></span> <span data-ttu-id="eaeb8-130">Убедитесь, что **ASP.NET Core 2.1** выбирается из селектора framework, то выполняется SignalR в предыдущих версиях .NET.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Диалоговое окно нового проекта в Visual Studio](get-started/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="eaeb8-132">Visual Studio включает `Microsoft.AspNetCore.SignalR` пакета, содержащего его сервера библиотек в рамках его **веб-приложения ASP.NET Core** шаблона.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="eaeb8-133">Однако клиентская библиотека JavaScript для SignalR должны устанавливаться с помощью *npm*.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="eaeb8-134">Выполните следующие команды **консоль диспетчера пакетов** окна, в корне проекта:</span><span class="sxs-lookup"><span data-stu-id="eaeb8-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```     

4. <span data-ttu-id="eaeb8-135">Создайте новую папку с именем «signalr» внутри *lib* в папке проекта.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-135">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="eaeb8-136">Копировать *signalr.js* файл из *node_modules\\ @aspnet\signalr\dist\browser*  в эту папку.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-136">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="eaeb8-137">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-137">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="eaeb8-138">Из **интеграции терминалов**, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="eaeb8-138">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. <span data-ttu-id="eaeb8-139">Установите библиотеку клиента JavaScript с помощью *npm*.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-139">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="eaeb8-140">Создайте новую папку с именем «signalr» внутри *lib* в папке проекта.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-140">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="eaeb8-141">Копировать *signalr.js* файл из *node_modules\\ @aspnet\signalr\dist\browser*  в эту папку.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-141">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="eaeb8-142">Создание концентратора SignalR</span><span class="sxs-lookup"><span data-stu-id="eaeb8-142">Create the SignalR Hub</span></span>

<span data-ttu-id="eaeb8-143">Концентратор является класс, который служит в качестве высокого уровня конвейера, который позволяет клиенту и серверу для вызова методов друг от друга.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-143">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="eaeb8-144">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eaeb8-144">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="eaeb8-145">Добавить класс в проект, выбрав **файл** > **New** > **файл** и выбрав **классов Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-145">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> <span data-ttu-id="eaeb8-146">Назовите файл *ChatHub*.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-146">Name the file *ChatHub*.</span></span> 

2. <span data-ttu-id="eaeb8-147">Наследовать от `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-147">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="eaeb8-148">`Hub` Класс содержит свойства и события для управления подключения и группы, а также отправки и получения данных.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-148">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="eaeb8-149">Создание `SendMessage` метод, который отправляет сообщение всем клиентам, подключенным чат.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-149">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="eaeb8-150">Обратите внимание на то, она возвращает [задачи](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), так как SignalR является асинхронным.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-150">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="eaeb8-151">Лучше масштабируется асинхронного кода.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-151">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="eaeb8-152">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-152">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="eaeb8-153">Откройте *SignalRChat* папки в коде Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-153">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="eaeb8-154">Добавить класс в проект, выбрав **файл** > **новый файл** в меню.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-154">Add a class to the project by selecting **File** > **New File** from the menu.</span></span>

3. <span data-ttu-id="eaeb8-155">Наследовать от `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-155">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="eaeb8-156">`Hub` Класс содержит свойства и события для управления подключения и группы, а также отправки и получения данных для клиентов.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-156">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="eaeb8-157">Добавьте в класс метод `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-157">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="eaeb8-158">`SendMessage` Метод отправляет сообщение всем клиентам, подключенным чат.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-158">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="eaeb8-159">Обратите внимание на то, она возвращает [задачи](/dotnet/api/system.threading.tasks.task), так как SignalR является асинхронным.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-159">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="eaeb8-160">Лучше масштабируется асинхронного кода.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-160">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=6-12)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="eaeb8-161">Настроить проект для использования SignalR</span><span class="sxs-lookup"><span data-stu-id="eaeb8-161">Configure the project to use SignalR</span></span>

<span data-ttu-id="eaeb8-162">SignalR сервера необходимо настроить, чтобы он доверял передачи запросов для SignalR.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-162">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="eaeb8-163">Чтобы настроить проект SignalR, работать с проектом `Startup.ConfigureServices` метод.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-163">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="eaeb8-164">`services.AddSignalR` Добавляет в составе SignalR [по промежуточного слоя](xref:fundamentals/middleware/index) конвейера.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-164">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

2. <span data-ttu-id="eaeb8-165">Настроить маршруты для концентраторов с помощью `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-165">Configure routes to your hubs using `UseSignalR`.</span></span>


   [!code-csharp[Startup](get-started/sample/Startup.cs?highlight=37,57-60)]


## <a name="create-the-signalr-client-code"></a><span data-ttu-id="eaeb8-166">Создайте код клиента SignalR</span><span class="sxs-lookup"><span data-stu-id="eaeb8-166">Create the SignalR client code</span></span>

1. <span data-ttu-id="eaeb8-167">Добавьте файл JavaScript с именем *chat.js*в *wwwroot\js* папки.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-167">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="eaeb8-168">Добавьте в этот файл следующий код:</span><span class="sxs-lookup"><span data-stu-id="eaeb8-168">Add the following code to it:</span></span>

   [!code-javascript[Index](get-started/sample/wwwroot/js/chat.js)]

2. <span data-ttu-id="eaeb8-169">Замените содержимое в *Pages\Index.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="eaeb8-169">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](get-started/sample/Pages/Index.cshtml)]

   <span data-ttu-id="eaeb8-170">Предыдущий HTML отображает имя и поля сообщения и кнопка отправки.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-170">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="eaeb8-171">Обратите внимание, ссылки на скрипты в нижней: ссылку на SignalR и *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-171">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>


## <a name="run-the-app"></a><span data-ttu-id="eaeb8-172">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="eaeb8-172">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="eaeb8-173">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eaeb8-173">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="eaeb8-174">Выберите **отладки** > **Запуск без отладки** запускает браузер и загрузить веб-сайт локально.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-174">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="eaeb8-175">Скопируйте URL-адрес из адресной строки.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-175">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="eaeb8-176">Откройте другой экземпляр браузера (любой браузер) и вставьте URL-адрес в адресной строке.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-176">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="eaeb8-177">Выберите любой браузер, введите имя и сообщение и нажмите кнопку **отправки** кнопки.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-177">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="eaeb8-178">Имя и сообщения отображаются на обеих страницах мгновенно.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-178">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="eaeb8-179">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-179">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="eaeb8-180">Нажмите клавишу **отладки** (F5), чтобы выполнить сборку программы и запустить ее.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-180">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="eaeb8-181">Запуск программы открывает окно браузера.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-181">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="eaeb8-182">Открытие другого окна браузера и загрузить локально в веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-182">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="eaeb8-183">Выберите любой браузер, введите имя и сообщение и нажмите кнопку **отправки** кнопки.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-183">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="eaeb8-184">Имя и сообщения отображаются на обеих страницах мгновенно.</span><span class="sxs-lookup"><span data-stu-id="eaeb8-184">The name and message are displayed on both pages instantly.</span></span>

---

  ![Решение](get-started/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="eaeb8-186">Связанные ресурсы</span><span class="sxs-lookup"><span data-stu-id="eaeb8-186">Related resources</span></span>

[<span data-ttu-id="eaeb8-187">Введение в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="eaeb8-187">Introduction to ASP.NET Core SignalR</span></span>](introduction.md)
