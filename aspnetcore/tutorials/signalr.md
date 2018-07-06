---
title: Начало работы с SignalR в ASP.NET Core
author: rachelappel
description: В этом руководстве создается приложение с помощью SignalR для ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: ca9145d9e16c23e34bbc1d84ff01ce02709187ce
ms.sourcegitcommit: 08f1a9baa97060da5168840b332c9c0805b5f901
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2018
ms.locfileid: "37144876"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="22707-103">Начало работы с SignalR в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="22707-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="22707-104">Автор: [Рэйчел Аппель](https://twitter.com/rachelappel) (Rachel Appel)</span><span class="sxs-lookup"><span data-stu-id="22707-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="22707-105">В этом руководстве описаны основы создания приложения в режиме реального времени с помощью SignalR для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="22707-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Решение](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="22707-107">Ниже перечислены рассматриваемые в данном руководстве задачи разработки SignalR:</span><span class="sxs-lookup"><span data-stu-id="22707-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="22707-108">Создание SignalR в веб-приложении ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="22707-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="22707-109">Создание хаба SignalR для передачи содержимого клиентам.</span><span class="sxs-lookup"><span data-stu-id="22707-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="22707-110">Изменение класса `Startup` и настройка приложения.</span><span class="sxs-lookup"><span data-stu-id="22707-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="22707-111">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="22707-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="22707-112">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="22707-112">Prerequisites</span></span>

<span data-ttu-id="22707-113">Установите следующее программное обеспечение:</span><span class="sxs-lookup"><span data-stu-id="22707-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="22707-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="22707-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="22707-115">Пакет SDK для .NET Core 2.1.или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="22707-115">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="22707-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.7 или более поздней версии с рабочей нагрузкой **ASP.NET и веб-разработка**</span><span class="sxs-lookup"><span data-stu-id="22707-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="22707-117">npm</span><span class="sxs-lookup"><span data-stu-id="22707-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="22707-118">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="22707-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="22707-119">Пакет SDK для .NET Core 2.1.или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="22707-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="22707-120">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="22707-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="22707-121">C# для Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="22707-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="22707-122">npm</span><span class="sxs-lookup"><span data-stu-id="22707-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="22707-123">Создайте проект ASP.NET Core, в котором размещен клиент и сервер SignalR</span><span class="sxs-lookup"><span data-stu-id="22707-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="22707-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="22707-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="22707-125">Перейдите в меню **Файл** > **Создать проект** и выберите **Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="22707-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="22707-126">Назовите проект *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="22707-126">Name the project *SignalRChat*.</span></span>

   ![Диалоговое окно создания проекта в Visual Studio](signalr/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="22707-128">Выберите **Веб-приложение**, чтобы создать проект с помощью Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="22707-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="22707-129">Нажмите **ОК**.</span><span class="sxs-lookup"><span data-stu-id="22707-129">Then select **OK**.</span></span> <span data-ttu-id="22707-130">Убедитесь, что в списке платформ выбрана **ASP.NET Core 2.1**, хотя SignalR выполняется и в предыдущих версиях .NET.</span><span class="sxs-lookup"><span data-stu-id="22707-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Диалоговое окно создания проекта в Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="22707-132">Visual Studio включает пакет `Microsoft.AspNetCore.SignalR`, содержащий библиотеки сервера в шаблоне **Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="22707-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="22707-133">Однако необходимо установить клиентскую библиотеку JavaScript для SignalR с помощью *npm*.</span><span class="sxs-lookup"><span data-stu-id="22707-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="22707-134">Выполните следующие команды в окне **Консоль диспетчера пакетов** в корневой папке проекта:</span><span class="sxs-lookup"><span data-stu-id="22707-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. <span data-ttu-id="22707-135">Создайте новую папку с именем "signalr" в папке *lib* в проекте.</span><span class="sxs-lookup"><span data-stu-id="22707-135">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="22707-136">Скопируйте файл *signalr.js* из *node_modules\\@aspnet\signalr\dist\browser* в эту папку.</span><span class="sxs-lookup"><span data-stu-id="22707-136">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="22707-137">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="22707-137">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="22707-138">Во **встроенном терминале** выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="22707-138">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. <span data-ttu-id="22707-139">Установите клиентскую библиотеку JavaScript с помощью *npm*.</span><span class="sxs-lookup"><span data-stu-id="22707-139">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="22707-140">Создайте новую папку с именем "signalr" в папке *lib* в проекте.</span><span class="sxs-lookup"><span data-stu-id="22707-140">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="22707-141">Скопируйте файл *signalr.js* из *node_modules\\@aspnet\signalr\dist\browser* в эту папку.</span><span class="sxs-lookup"><span data-stu-id="22707-141">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="22707-142">Создание хаба SignalR</span><span class="sxs-lookup"><span data-stu-id="22707-142">Create the SignalR Hub</span></span>

<span data-ttu-id="22707-143">Хаб является классом, который служит конвейером высокого уровня для вызова методов между клиентом и сервером.</span><span class="sxs-lookup"><span data-stu-id="22707-143">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="22707-144">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="22707-144">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="22707-145">Добавьте класс в проект, перейдя в меню **Файл** > **Создать** > **Файл** и выбрав **Класс Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="22707-145">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> <span data-ttu-id="22707-146">Присвойте классу имя `ChatHub`, а файлу имя *ChatHub.cs*.</span><span class="sxs-lookup"><span data-stu-id="22707-146">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

2. <span data-ttu-id="22707-147">Наследуйте от `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="22707-147">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="22707-148">Класс `Hub` содержит свойства и события для управления подключениями и группами, а также отправки и получения данных.</span><span class="sxs-lookup"><span data-stu-id="22707-148">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="22707-149">Создайте метод `SendMessage`, который отправляет сообщение всем подключенным клиентам чата.</span><span class="sxs-lookup"><span data-stu-id="22707-149">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="22707-150">Заметьте, что он возвращает [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), так как SignalR является асинхронным.</span><span class="sxs-lookup"><span data-stu-id="22707-150">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="22707-151">Асинхронный код лучше масштабируется.</span><span class="sxs-lookup"><span data-stu-id="22707-151">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="22707-152">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="22707-152">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="22707-153">Откройте папку *SignalRChat* в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="22707-153">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="22707-154">Добавьте класс в проект, выбрав **Файл** > **Создать файл**.</span><span class="sxs-lookup"><span data-stu-id="22707-154">Add a class to the project by selecting **File** > **New File** from the menu.</span></span> <span data-ttu-id="22707-155">Присвойте классу имя `ChatHub`, а файлу имя *ChatHub.cs*.</span><span class="sxs-lookup"><span data-stu-id="22707-155">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

3. <span data-ttu-id="22707-156">Наследуйте от `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="22707-156">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="22707-157">Класс `Hub` содержит свойства и события для управления подключениями и группами, а также отправки и получения данных для клиентов.</span><span class="sxs-lookup"><span data-stu-id="22707-157">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="22707-158">Добавьте в класс метод `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="22707-158">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="22707-159">Метод `SendMessage` отправляет сообщение всем подключенным клиентам чата.</span><span class="sxs-lookup"><span data-stu-id="22707-159">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="22707-160">Заметьте, что он возвращает [Task](/dotnet/api/system.threading.tasks.task), так как SignalR является асинхронным.</span><span class="sxs-lookup"><span data-stu-id="22707-160">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="22707-161">Асинхронный код лучше масштабируется.</span><span class="sxs-lookup"><span data-stu-id="22707-161">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="22707-162">Настройка проекта для использования SignalR</span><span class="sxs-lookup"><span data-stu-id="22707-162">Configure the project to use SignalR</span></span>

<span data-ttu-id="22707-163">Сервер SignalR необходимо настроить таким образом, чтобы он передавал запросы SignalR.</span><span class="sxs-lookup"><span data-stu-id="22707-163">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="22707-164">Чтобы настроить проект SignalR, измените метод `Startup.ConfigureServices` проекта.</span><span class="sxs-lookup"><span data-stu-id="22707-164">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="22707-165">`services.AddSignalR` открывает системе [внедрения зависимостей](xref:fundamentals/dependency-injection) доступ к службам SignalR.</span><span class="sxs-lookup"><span data-stu-id="22707-165">`services.AddSignalR` makes the SignalR services available to the [dependency injection](xref:fundamentals/dependency-injection) system.</span></span>

1. <span data-ttu-id="22707-166">Настройте маршруты к хабам с помощью `UseSignalR` в методе `Configure`.</span><span class="sxs-lookup"><span data-stu-id="22707-166">Configure routes to your hubs with `UseSignalR` in the `Configure` method.</span></span> <span data-ttu-id="22707-167">Метод `app.UseSignalR` добавляет SignalR в конвейер [ПО промежуточного слоя](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="22707-167">`app.UseSignalR` adds SignalR to the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="22707-168">Создание кода клиента SignalR</span><span class="sxs-lookup"><span data-stu-id="22707-168">Create the SignalR client code</span></span>

1. <span data-ttu-id="22707-169">Добавьте файл JavaScript с именем *chat.js* в папку *wwwroot\js*.</span><span class="sxs-lookup"><span data-stu-id="22707-169">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="22707-170">Добавьте в этот файл следующий код:</span><span class="sxs-lookup"><span data-stu-id="22707-170">Add the following code to it:</span></span>

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. <span data-ttu-id="22707-171">Замените содержимое в файле *Pages\Index.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="22707-171">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   <span data-ttu-id="22707-172">Предыдущий HTML отображает поля имени и сообщения и кнопку отправки.</span><span class="sxs-lookup"><span data-stu-id="22707-172">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="22707-173">Обратите внимание на ссылки на скрипты в нижней части: ссылка на SignalR и *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="22707-173">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="22707-174">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="22707-174">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="22707-175">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="22707-175">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="22707-176">Выберите **Отладка** > **Запуск без отладки**, чтобы запустить браузер и загрузить веб-сайт локально.</span><span class="sxs-lookup"><span data-stu-id="22707-176">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="22707-177">Скопируйте URL-адрес из адресной строки.</span><span class="sxs-lookup"><span data-stu-id="22707-177">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="22707-178">Откройте другой экземпляр браузера (любого) и вставьте URL-адрес в адресную строку.</span><span class="sxs-lookup"><span data-stu-id="22707-178">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="22707-179">Выберите любой браузер, введите имя и сообщение и нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="22707-179">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="22707-180">Имя и сообщение отображаются на обеих страницах мгновенно.</span><span class="sxs-lookup"><span data-stu-id="22707-180">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="22707-181">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="22707-181">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="22707-182">Нажмите клавишу **отладки** (F5), чтобы выполнить сборку программы и запустить ее.</span><span class="sxs-lookup"><span data-stu-id="22707-182">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="22707-183">Запуск программы открывает окно браузера.</span><span class="sxs-lookup"><span data-stu-id="22707-183">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="22707-184">Откройте другое окно браузера и локально загрузите веб-сайт.</span><span class="sxs-lookup"><span data-stu-id="22707-184">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="22707-185">Выберите любой браузер, введите имя и сообщение и нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="22707-185">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="22707-186">Имя и сообщение отображаются на обеих страницах мгновенно.</span><span class="sxs-lookup"><span data-stu-id="22707-186">The name and message are displayed on both pages instantly.</span></span>

---

  ![Решение](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="22707-188">Связанные ресурсы</span><span class="sxs-lookup"><span data-stu-id="22707-188">Related resources</span></span>

[<span data-ttu-id="22707-189">Введение в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="22707-189">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
