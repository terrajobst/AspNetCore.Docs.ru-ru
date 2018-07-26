---
title: Начало работы с SignalR в ASP.NET Core
author: tdykstra
description: В этом руководстве создается приложение с помощью SignalR для ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: 83be28b30cf06eeea37e8d76b3f6444ffd9a10e8
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095495"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="3fc79-103">Начало работы с SignalR в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3fc79-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="3fc79-104">В этом руководстве описаны основы создания приложения в режиме реального времени с помощью SignalR для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3fc79-104">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Решение](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="3fc79-106">Ниже перечислены рассматриваемые в данном руководстве задачи разработки SignalR:</span><span class="sxs-lookup"><span data-stu-id="3fc79-106">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3fc79-107">Создание SignalR в веб-приложении ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3fc79-107">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="3fc79-108">Создание хаба SignalR для передачи содержимого клиентам.</span><span class="sxs-lookup"><span data-stu-id="3fc79-108">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="3fc79-109">Изменение класса `Startup` и настройка приложения.</span><span class="sxs-lookup"><span data-stu-id="3fc79-109">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="3fc79-110">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3fc79-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3fc79-111">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="3fc79-111">Prerequisites</span></span>

<span data-ttu-id="3fc79-112">Установите следующее программное обеспечение:</span><span class="sxs-lookup"><span data-stu-id="3fc79-112">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3fc79-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3fc79-113">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="3fc79-114">Пакет SDK для .NET Core 2.1.или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="3fc79-114">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="3fc79-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.7.3 или более поздней версии с рабочей нагрузкой **ASP.NET и веб-разработка**</span><span class="sxs-lookup"><span data-stu-id="3fc79-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7.3 or later with the **ASP.NET and web development** workload</span></span>
* <span data-ttu-id="3fc79-116">[npm](https://www.npmjs.com/get-npm) (диспетчер пакетов для Node.js)</span><span class="sxs-lookup"><span data-stu-id="3fc79-116">[npm](https://www.npmjs.com/get-npm) (package manager for Node.js)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3fc79-117">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="3fc79-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="3fc79-118">Пакет SDK для .NET Core 2.1.или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="3fc79-118">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="3fc79-119">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="3fc79-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="3fc79-120">C# для Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3fc79-120">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="3fc79-121">[npm](https://www.npmjs.com/get-npm) (диспетчер пакетов для Node.js)</span><span class="sxs-lookup"><span data-stu-id="3fc79-121">[npm](https://www.npmjs.com/get-npm) (package manager for Node.js)</span></span>

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="3fc79-122">Создайте проект ASP.NET Core, в котором размещен клиент и сервер SignalR</span><span class="sxs-lookup"><span data-stu-id="3fc79-122">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3fc79-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3fc79-123">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="3fc79-124">Перейдите в меню **Файл** > **Создать проект** и выберите **Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="3fc79-124">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="3fc79-125">Назовите проект *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="3fc79-125">Name the project *SignalRChat*.</span></span>

   ![Диалоговое окно создания проекта в Visual Studio](signalr/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="3fc79-127">Выберите **Веб-приложение**, чтобы создать проект с помощью Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="3fc79-127">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="3fc79-128">Нажмите **ОК**.</span><span class="sxs-lookup"><span data-stu-id="3fc79-128">Then select **OK**.</span></span> <span data-ttu-id="3fc79-129">Убедитесь, что в списке платформ выбрана **ASP.NET Core 2.1**, хотя SignalR выполняется и в предыдущих версиях .NET.</span><span class="sxs-lookup"><span data-stu-id="3fc79-129">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Диалоговое окно создания проекта в Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="3fc79-131">Visual Studio включает пакет `Microsoft.AspNetCore.SignalR`, содержащий библиотеки сервера в шаблоне **Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="3fc79-131">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="3fc79-132">Однако необходимо установить клиентскую библиотеку JavaScript для SignalR с помощью *npm*.</span><span class="sxs-lookup"><span data-stu-id="3fc79-132">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="3fc79-133">Выполните следующие команды в окне **Консоль диспетчера пакетов** в корневой папке проекта:</span><span class="sxs-lookup"><span data-stu-id="3fc79-133">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. <span data-ttu-id="3fc79-134">Создайте папку с именем signalr в папке *wwwroot/lib* в проекте.</span><span class="sxs-lookup"><span data-stu-id="3fc79-134">Create a new folder named "signalr" inside the  *wwwroot/lib* folder in your project.</span></span> <span data-ttu-id="3fc79-135">Скопируйте файл *signalr.js* из *node_modules\\@aspnet\signalr\dist\browser* в эту папку.</span><span class="sxs-lookup"><span data-stu-id="3fc79-135">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3fc79-136">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="3fc79-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="3fc79-137">Во **встроенном терминале** выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="3fc79-137">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. <span data-ttu-id="3fc79-138">Установите клиентскую библиотеку JavaScript с помощью *npm*.</span><span class="sxs-lookup"><span data-stu-id="3fc79-138">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="3fc79-139">Создайте новую папку с именем "signalr" в папке *lib* в проекте.</span><span class="sxs-lookup"><span data-stu-id="3fc79-139">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="3fc79-140">Скопируйте файл *signalr.js* из *node_modules\\@aspnet\signalr\dist\browser* в эту папку.</span><span class="sxs-lookup"><span data-stu-id="3fc79-140">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="3fc79-141">Создание хаба SignalR</span><span class="sxs-lookup"><span data-stu-id="3fc79-141">Create the SignalR Hub</span></span>

<span data-ttu-id="3fc79-142">Хаб является классом, который служит конвейером высокого уровня для вызова методов между клиентом и сервером.</span><span class="sxs-lookup"><span data-stu-id="3fc79-142">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3fc79-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3fc79-143">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="3fc79-144">Добавьте класс в проект, перейдя в меню **Файл** > **Создать** > **Файл** и выбрав **Класс Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="3fc79-144">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> <span data-ttu-id="3fc79-145">Присвойте классу имя `ChatHub`, а файлу имя *ChatHub.cs*.</span><span class="sxs-lookup"><span data-stu-id="3fc79-145">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

2. <span data-ttu-id="3fc79-146">Наследуйте от `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="3fc79-146">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="3fc79-147">Класс `Hub` содержит свойства и события для управления подключениями и группами, а также отправки и получения данных.</span><span class="sxs-lookup"><span data-stu-id="3fc79-147">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="3fc79-148">Создайте метод `SendMessage`, который отправляет сообщение всем подключенным клиентам чата.</span><span class="sxs-lookup"><span data-stu-id="3fc79-148">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="3fc79-149">Заметьте, что он возвращает [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), так как SignalR является асинхронным.</span><span class="sxs-lookup"><span data-stu-id="3fc79-149">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="3fc79-150">Асинхронный код лучше масштабируется.</span><span class="sxs-lookup"><span data-stu-id="3fc79-150">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3fc79-151">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="3fc79-151">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="3fc79-152">Откройте папку *SignalRChat* в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="3fc79-152">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="3fc79-153">Добавьте класс в проект, выбрав **Файл** > **Создать файл**.</span><span class="sxs-lookup"><span data-stu-id="3fc79-153">Add a class to the project by selecting **File** > **New File** from the menu.</span></span> <span data-ttu-id="3fc79-154">Присвойте классу имя `ChatHub`, а файлу имя *ChatHub.cs*.</span><span class="sxs-lookup"><span data-stu-id="3fc79-154">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

3. <span data-ttu-id="3fc79-155">Наследуйте от `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="3fc79-155">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="3fc79-156">Класс `Hub` содержит свойства и события для управления подключениями и группами, а также отправки и получения данных для клиентов.</span><span class="sxs-lookup"><span data-stu-id="3fc79-156">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="3fc79-157">Добавьте в класс метод `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="3fc79-157">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="3fc79-158">Метод `SendMessage` отправляет сообщение всем подключенным клиентам чата.</span><span class="sxs-lookup"><span data-stu-id="3fc79-158">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="3fc79-159">Заметьте, что он возвращает [Task](/dotnet/api/system.threading.tasks.task), так как SignalR является асинхронным.</span><span class="sxs-lookup"><span data-stu-id="3fc79-159">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="3fc79-160">Асинхронный код лучше масштабируется.</span><span class="sxs-lookup"><span data-stu-id="3fc79-160">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="3fc79-161">Настройка проекта для использования SignalR</span><span class="sxs-lookup"><span data-stu-id="3fc79-161">Configure the project to use SignalR</span></span>

<span data-ttu-id="3fc79-162">Сервер SignalR необходимо настроить таким образом, чтобы он передавал запросы SignalR.</span><span class="sxs-lookup"><span data-stu-id="3fc79-162">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="3fc79-163">Чтобы настроить проект SignalR, измените метод `Startup.ConfigureServices` проекта.</span><span class="sxs-lookup"><span data-stu-id="3fc79-163">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="3fc79-164">`services.AddSignalR` открывает системе [внедрения зависимостей](xref:fundamentals/dependency-injection) доступ к службам SignalR.</span><span class="sxs-lookup"><span data-stu-id="3fc79-164">`services.AddSignalR` makes the SignalR services available to the [dependency injection](xref:fundamentals/dependency-injection) system.</span></span>

1. <span data-ttu-id="3fc79-165">Настройте маршруты к хабам с помощью `UseSignalR` в методе `Configure`.</span><span class="sxs-lookup"><span data-stu-id="3fc79-165">Configure routes to your hubs with `UseSignalR` in the `Configure` method.</span></span> <span data-ttu-id="3fc79-166">Метод `app.UseSignalR` добавляет SignalR в конвейер [ПО промежуточного слоя](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="3fc79-166">`app.UseSignalR` adds SignalR to the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="3fc79-167">Создание кода клиента SignalR</span><span class="sxs-lookup"><span data-stu-id="3fc79-167">Create the SignalR client code</span></span>

1. <span data-ttu-id="3fc79-168">Добавьте файл JavaScript с именем *chat.js* в папку *wwwroot\js*.</span><span class="sxs-lookup"><span data-stu-id="3fc79-168">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="3fc79-169">Добавьте в этот файл следующий код:</span><span class="sxs-lookup"><span data-stu-id="3fc79-169">Add the following code to it:</span></span>

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. <span data-ttu-id="3fc79-170">Замените содержимое в файле *Pages\Index.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="3fc79-170">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   <span data-ttu-id="3fc79-171">Предыдущий HTML отображает поля имени и сообщения и кнопку отправки.</span><span class="sxs-lookup"><span data-stu-id="3fc79-171">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="3fc79-172">Обратите внимание на ссылки на скрипты в нижней части: ссылка на SignalR и *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="3fc79-172">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="3fc79-173">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="3fc79-173">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3fc79-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3fc79-174">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="3fc79-175">Выберите **Отладка** > **Запуск без отладки**, чтобы запустить браузер и загрузить веб-сайт локально.</span><span class="sxs-lookup"><span data-stu-id="3fc79-175">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="3fc79-176">Скопируйте URL-адрес из адресной строки.</span><span class="sxs-lookup"><span data-stu-id="3fc79-176">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="3fc79-177">Откройте другой экземпляр браузера (любого) и вставьте URL-адрес в адресную строку.</span><span class="sxs-lookup"><span data-stu-id="3fc79-177">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="3fc79-178">Выберите любой браузер, введите имя и сообщение и нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="3fc79-178">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="3fc79-179">Имя и сообщение отображаются на обеих страницах мгновенно.</span><span class="sxs-lookup"><span data-stu-id="3fc79-179">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3fc79-180">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="3fc79-180">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="3fc79-181">Нажмите клавишу **отладки** (F5), чтобы выполнить сборку программы и запустить ее.</span><span class="sxs-lookup"><span data-stu-id="3fc79-181">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="3fc79-182">Запуск программы открывает окно браузера.</span><span class="sxs-lookup"><span data-stu-id="3fc79-182">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="3fc79-183">Откройте другое окно браузера и локально загрузите веб-сайт.</span><span class="sxs-lookup"><span data-stu-id="3fc79-183">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="3fc79-184">Выберите любой браузер, введите имя и сообщение и нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="3fc79-184">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="3fc79-185">Имя и сообщение отображаются на обеих страницах мгновенно.</span><span class="sxs-lookup"><span data-stu-id="3fc79-185">The name and message are displayed on both pages instantly.</span></span>

---

  ![Решение](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="3fc79-187">Связанные ресурсы</span><span class="sxs-lookup"><span data-stu-id="3fc79-187">Related resources</span></span>

[<span data-ttu-id="3fc79-188">Введение в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="3fc79-188">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
