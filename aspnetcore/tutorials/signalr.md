---
title: Начало работы с SignalR в ASP.NET Core
author: rachelappel
description: В этом руководстве создается приложение с помощью SignalR для ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: 8762a4be1032d58014dd32dfdd3707197e14c6f9
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36297203"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="6c793-103">Начало работы с SignalR в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6c793-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="6c793-104">Автор: [Рэйчел Аппель](https://twitter.com/rachelappel) (Rachel Appel)</span><span class="sxs-lookup"><span data-stu-id="6c793-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="6c793-105">В этом руководстве описаны основы создания приложения в режиме реального времени с помощью SignalR для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6c793-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Решение](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="6c793-107">Ниже перечислены рассматриваемые в данном руководстве задачи разработки SignalR:</span><span class="sxs-lookup"><span data-stu-id="6c793-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6c793-108">Создание SignalR в веб-приложении ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6c793-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="6c793-109">Создание хаба SignalR для передачи содержимого клиентам.</span><span class="sxs-lookup"><span data-stu-id="6c793-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="6c793-110">Изменение класса `Startup` и настройка приложения.</span><span class="sxs-lookup"><span data-stu-id="6c793-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="6c793-111">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6c793-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="prerequisites"></a><span data-ttu-id="6c793-112">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="6c793-112">Prerequisites</span></span>

<span data-ttu-id="6c793-113">Установите следующее программное обеспечение:</span><span class="sxs-lookup"><span data-stu-id="6c793-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6c793-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6c793-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="6c793-115">Пакет SDK для .NET Core 2.1.или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="6c793-115">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="6c793-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.7 или более поздней версии с рабочей нагрузкой **ASP.NET и веб-разработка**</span><span class="sxs-lookup"><span data-stu-id="6c793-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="6c793-117">npm</span><span class="sxs-lookup"><span data-stu-id="6c793-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6c793-118">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6c793-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="6c793-119">Пакет SDK для .NET Core 2.1.или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="6c793-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="6c793-120">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6c793-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="6c793-121">C# для Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6c793-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="6c793-122">npm</span><span class="sxs-lookup"><span data-stu-id="6c793-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="6c793-123">Создайте проект ASP.NET Core, в котором размещен клиент и сервер SignalR</span><span class="sxs-lookup"><span data-stu-id="6c793-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6c793-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6c793-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="6c793-125">Перейдите в меню **Файл** > **Создать проект** и выберите **Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="6c793-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="6c793-126">Назовите проект *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="6c793-126">Name the project *SignalRChat*.</span></span>

   ![Диалоговое окно создания проекта в Visual Studio](signalr/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="6c793-128">Выберите **Веб-приложение**, чтобы создать проект с помощью Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="6c793-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="6c793-129">Нажмите **ОК**.</span><span class="sxs-lookup"><span data-stu-id="6c793-129">Then select **OK**.</span></span> <span data-ttu-id="6c793-130">Убедитесь, что в списке платформ выбрана **ASP.NET Core 2.1**, хотя SignalR выполняется и в предыдущих версиях .NET.</span><span class="sxs-lookup"><span data-stu-id="6c793-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Диалоговое окно создания проекта в Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="6c793-132">Visual Studio включает пакет `Microsoft.AspNetCore.SignalR`, содержащий библиотеки сервера в шаблоне **Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="6c793-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="6c793-133">Однако необходимо установить клиентскую библиотеку JavaScript для SignalR с помощью *npm*.</span><span class="sxs-lookup"><span data-stu-id="6c793-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="6c793-134">Выполните следующие команды в окне **Консоль диспетчера пакетов** в корневой папке проекта:</span><span class="sxs-lookup"><span data-stu-id="6c793-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. <span data-ttu-id="6c793-135">Создайте новую папку с именем "signalr" в папке *lib* в проекте.</span><span class="sxs-lookup"><span data-stu-id="6c793-135">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="6c793-136">Скопируйте файл *signalr.js* из *node_modules\\@aspnet\signalr\dist\browser* в эту папку.</span><span class="sxs-lookup"><span data-stu-id="6c793-136">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6c793-137">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6c793-137">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="6c793-138">Во **встроенном терминале** выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="6c793-138">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. <span data-ttu-id="6c793-139">Установите клиентскую библиотеку JavaScript с помощью *npm*.</span><span class="sxs-lookup"><span data-stu-id="6c793-139">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="6c793-140">Создайте новую папку с именем "signalr" в папке *lib* в проекте.</span><span class="sxs-lookup"><span data-stu-id="6c793-140">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="6c793-141">Скопируйте файл *signalr.js* из *node_modules\\@aspnet\signalr\dist\browser* в эту папку.</span><span class="sxs-lookup"><span data-stu-id="6c793-141">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="6c793-142">Создание хаба SignalR</span><span class="sxs-lookup"><span data-stu-id="6c793-142">Create the SignalR Hub</span></span>

<span data-ttu-id="6c793-143">Хаб является классом, который служит конвейером высокого уровня для вызова методов между клиентом и сервером.</span><span class="sxs-lookup"><span data-stu-id="6c793-143">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6c793-144">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6c793-144">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="6c793-145">Добавьте класс в проект, перейдя в меню **Файл** > **Создать** > **Файл** и выбрав **Класс Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="6c793-145">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> <span data-ttu-id="6c793-146">Назовите файл *ChatHub*.</span><span class="sxs-lookup"><span data-stu-id="6c793-146">Name the file *ChatHub*.</span></span>

2. <span data-ttu-id="6c793-147">Наследуйте от `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="6c793-147">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="6c793-148">Класс `Hub` содержит свойства и события для управления подключениями и группами, а также отправки и получения данных.</span><span class="sxs-lookup"><span data-stu-id="6c793-148">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="6c793-149">Создайте метод `SendMessage`, который отправляет сообщение всем подключенным клиентам чата.</span><span class="sxs-lookup"><span data-stu-id="6c793-149">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="6c793-150">Заметьте, что он возвращает [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), так как SignalR является асинхронным.</span><span class="sxs-lookup"><span data-stu-id="6c793-150">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="6c793-151">Асинхронный код лучше масштабируется.</span><span class="sxs-lookup"><span data-stu-id="6c793-151">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6c793-152">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6c793-152">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="6c793-153">Откройте папку *SignalRChat* в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6c793-153">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="6c793-154">Добавьте класс в проект, выбрав **Файл** > **Создать файл**.</span><span class="sxs-lookup"><span data-stu-id="6c793-154">Add a class to the project by selecting **File** > **New File** from the menu.</span></span>

3. <span data-ttu-id="6c793-155">Наследуйте от `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="6c793-155">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="6c793-156">Класс `Hub` содержит свойства и события для управления подключениями и группами, а также отправки и получения данных для клиентов.</span><span class="sxs-lookup"><span data-stu-id="6c793-156">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="6c793-157">Добавьте в класс метод `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="6c793-157">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="6c793-158">Метод `SendMessage` отправляет сообщение всем подключенным клиентам чата.</span><span class="sxs-lookup"><span data-stu-id="6c793-158">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="6c793-159">Заметьте, что он возвращает [Task](/dotnet/api/system.threading.tasks.task), так как SignalR является асинхронным.</span><span class="sxs-lookup"><span data-stu-id="6c793-159">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="6c793-160">Асинхронный код лучше масштабируется.</span><span class="sxs-lookup"><span data-stu-id="6c793-160">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs?range=6-12)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="6c793-161">Настройка проекта для использования SignalR</span><span class="sxs-lookup"><span data-stu-id="6c793-161">Configure the project to use SignalR</span></span>

<span data-ttu-id="6c793-162">Сервер SignalR необходимо настроить таким образом, чтобы он передавал запросы SignalR.</span><span class="sxs-lookup"><span data-stu-id="6c793-162">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="6c793-163">Чтобы настроить проект SignalR, измените метод `Startup.ConfigureServices` проекта.</span><span class="sxs-lookup"><span data-stu-id="6c793-163">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="6c793-164">`services.AddSignalR` добавляет SignalR в конвейер [ПО промежуточного слоя](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="6c793-164">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

2. <span data-ttu-id="6c793-165">Настройте маршруты к хабам с помощью `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="6c793-165">Configure routes to your hubs using `UseSignalR`.</span></span>

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="6c793-166">Создание кода клиента SignalR</span><span class="sxs-lookup"><span data-stu-id="6c793-166">Create the SignalR client code</span></span>

1. <span data-ttu-id="6c793-167">Добавьте файл JavaScript с именем *chat.js* в папку *wwwroot\js*.</span><span class="sxs-lookup"><span data-stu-id="6c793-167">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="6c793-168">Добавьте в этот файл следующий код:</span><span class="sxs-lookup"><span data-stu-id="6c793-168">Add the following code to it:</span></span>

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. <span data-ttu-id="6c793-169">Замените содержимое в файле *Pages\Index.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="6c793-169">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   <span data-ttu-id="6c793-170">Предыдущий HTML отображает поля имени и сообщения и кнопку отправки.</span><span class="sxs-lookup"><span data-stu-id="6c793-170">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="6c793-171">Обратите внимание на ссылки на скрипты в нижней части: ссылка на SignalR и *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="6c793-171">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="6c793-172">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="6c793-172">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6c793-173">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6c793-173">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="6c793-174">Выберите **Отладка** > **Запуск без отладки**, чтобы запустить браузер и загрузить веб-сайт локально.</span><span class="sxs-lookup"><span data-stu-id="6c793-174">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="6c793-175">Скопируйте URL-адрес из адресной строки.</span><span class="sxs-lookup"><span data-stu-id="6c793-175">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="6c793-176">Откройте другой экземпляр браузера (любого) и вставьте URL-адрес в адресную строку.</span><span class="sxs-lookup"><span data-stu-id="6c793-176">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="6c793-177">Выберите любой браузер, введите имя и сообщение и нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="6c793-177">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="6c793-178">Имя и сообщение отображаются на обеих страницах мгновенно.</span><span class="sxs-lookup"><span data-stu-id="6c793-178">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6c793-179">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6c793-179">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="6c793-180">Нажмите клавишу **отладки** (F5), чтобы выполнить сборку программы и запустить ее.</span><span class="sxs-lookup"><span data-stu-id="6c793-180">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="6c793-181">Запуск программы открывает окно браузера.</span><span class="sxs-lookup"><span data-stu-id="6c793-181">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="6c793-182">Откройте другое окно браузера и локально загрузите веб-сайт.</span><span class="sxs-lookup"><span data-stu-id="6c793-182">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="6c793-183">Выберите любой браузер, введите имя и сообщение и нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="6c793-183">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="6c793-184">Имя и сообщение отображаются на обеих страницах мгновенно.</span><span class="sxs-lookup"><span data-stu-id="6c793-184">The name and message are displayed on both pages instantly.</span></span>

---

  ![Решение](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="6c793-186">Связанные ресурсы</span><span class="sxs-lookup"><span data-stu-id="6c793-186">Related resources</span></span>

[<span data-ttu-id="6c793-187">Введение в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="6c793-187">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
