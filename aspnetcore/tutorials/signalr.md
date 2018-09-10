---
title: Руководство. Начало работы с SignalR в ASP.NET Core
author: tdykstra
description: В этом руководстве создается приложение чата, которое использует SignalR для ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/20/2018
uid: tutorials/signalr
ms.openlocfilehash: a2573e2817a2d8921954264ca17bc3a7e2a010a8
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055836"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="2316d-103">Руководство. Начало работы с SignalR в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2316d-103">Tutorial: Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="2316d-104">В этом руководстве описаны основы создания приложения в режиме реального времени с помощью SignalR.</span><span class="sxs-lookup"><span data-stu-id="2316d-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="2316d-105">Вы научитесь:</span><span class="sxs-lookup"><span data-stu-id="2316d-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2316d-106">Создавать веб-приложение, которое использует SignalR в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2316d-106">Create a web app that uses SignalR on ASP.NET Core.</span></span>
> * <span data-ttu-id="2316d-107">Создавать концентратор SignalR на сервере.</span><span class="sxs-lookup"><span data-stu-id="2316d-107">Create a SignalR hub on the server.</span></span>
> * <span data-ttu-id="2316d-108">Подключаться к концентратору SignalR из клиентов JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2316d-108">Connect to the SignalR hub from JavaScript clients.</span></span>
> * <span data-ttu-id="2316d-109">Использовать концентратор для отправки сообщений из любого клиента всем подключенным клиентам.</span><span class="sxs-lookup"><span data-stu-id="2316d-109">Use the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="2316d-110">В итоге вы получите работающее приложение чата:</span><span class="sxs-lookup"><span data-stu-id="2316d-110">At the end, you'll have a working chat app:</span></span>

![Пример приложения SignalR](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="2316d-112">[Просмотреть или скачать пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([описание скачивания](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="2316d-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2316d-113">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="2316d-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2316d-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2316d-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2316d-115">[Visual Studio 2017 15.7.3 или более поздней версии](https://www.visualstudio.com/downloads/) с рабочей нагрузкой **ASP.NET и веб-разработка**</span><span class="sxs-lookup"><span data-stu-id="2316d-115">[Visual Studio 2017 version 15.7.3 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="2316d-116">Пакет SDK для .NET Core 2.1.или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="2316d-116">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="2316d-117">[npm](https://www.npmjs.com/get-npm) (диспетчер пакетов для Node.js, используемый для клиентской библиотеки SignalR JavaScript.)</span><span class="sxs-lookup"><span data-stu-id="2316d-117">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2316d-118">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="2316d-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="2316d-119">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="2316d-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="2316d-120">Пакет SDK для .NET Core 2.1.или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="2316d-120">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="2316d-121">C# для Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2316d-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="2316d-122">[npm](https://www.npmjs.com/get-npm) (диспетчер пакетов для Node.js, используемый для клиентской библиотеки SignalR JavaScript.)</span><span class="sxs-lookup"><span data-stu-id="2316d-122">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2316d-123">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="2316d-123">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="2316d-124">Visual Studio для Mac 7.5.4 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="2316d-124">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="2316d-125">[Пакет SDK для .NET Core 2.1 или более поздней версии](https://www.microsoft.com/net/download/all) (входит в установку Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="2316d-125">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>
* <span data-ttu-id="2316d-126">[npm](https://www.npmjs.com/get-npm) (диспетчер пакетов для Node.js, используемый для клиентской библиотеки SignalR JavaScript.)</span><span class="sxs-lookup"><span data-stu-id="2316d-126">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

---

## <a name="create-the-project"></a><span data-ttu-id="2316d-127">Создание проекта</span><span class="sxs-lookup"><span data-stu-id="2316d-127">Create the project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2316d-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2316d-128">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="2316d-129">В меню выберите **Файл > Создать проект**.</span><span class="sxs-lookup"><span data-stu-id="2316d-129">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="2316d-130">В диалоговом окне **Новый проект** выберите **Установленные > Visual C# > Веб > Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="2316d-130">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="2316d-131">Назовите проект *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="2316d-131">Name the project *SignalRChat*.</span></span>

  ![Диалоговое окно создания проекта в Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="2316d-133">Выберите **Веб-приложение**, чтобы создать проект, который использует Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="2316d-133">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="2316d-134">Выберите целевую платформу **.NET Core**, **ASP.NET Core 2.1** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="2316d-134">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Диалоговое окно создания проекта в Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2316d-136">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="2316d-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="2316d-137">Откройте папку, которую можно использовать для нового проекта.</span><span class="sxs-lookup"><span data-stu-id="2316d-137">Open a folder that you can use for a new project.</span></span>

* <span data-ttu-id="2316d-138">Во **встроенном терминале** выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="2316d-138">In the **Integrated Terminal**, run the following command:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   ```

   [!INCLUDE[](~/includes/webapp-alias-notice.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2316d-139">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="2316d-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="2316d-140">В меню выберите **Файл > Создать решение**.</span><span class="sxs-lookup"><span data-stu-id="2316d-140">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="2316d-141">Выберите **.NET Core > Приложение > Веб-приложение ASP.NET Core** (не выбирайте **Веб-приложение ASP.NET Core (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="2316d-141">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="2316d-142">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="2316d-142">Select **Next**.</span></span>

* <span data-ttu-id="2316d-143">Присвойте проекту имя *SignalRChat* и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="2316d-143">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="2316d-144">Добавление клиентской библиотеки SignalR</span><span class="sxs-lookup"><span data-stu-id="2316d-144">Add the SignalR client library</span></span>

<span data-ttu-id="2316d-145">Библиотека сервера SignalR входит в состав [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2316d-145">The SignalR server library is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="2316d-146">Однако вам нужно получить клиентскую библиотеку JavaScript с помощью [npm, диспетчера пакетов Node.js](https://www.npmjs.com/get-npm).</span><span class="sxs-lookup"><span data-stu-id="2316d-146">But you have to get the JavaScript client library from [npm, the Node.js package manager](https://www.npmjs.com/get-npm).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2316d-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2316d-147">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="2316d-148">В **консоли диспетчера пакетов** (PMC) перейдите в папку проекта (где расположен файл *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="2316d-148">In **Package Manager Console** (PMC), change to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

  ```console
  cd SignalRChat
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2316d-149">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="2316d-149">Visual Studio Code</span></span>](#tab/visual-studio-code/)

2. <span data-ttu-id="2316d-150">Перейдите к новой папке проекта.</span><span class="sxs-lookup"><span data-stu-id="2316d-150">Change to the new project folder.</span></span>

  ```console
  cd SignalRChat
  ``` 

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2316d-151">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="2316d-151">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="2316d-152">В **терминале** перейдите в папку проекта (где расположен файл *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="2316d-152">In the **Terminal**, navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

---

* <span data-ttu-id="2316d-153">Запустите инициализатор npm, чтобы создать файл *package.json*:</span><span class="sxs-lookup"><span data-stu-id="2316d-153">Run the npm initializer to create a *package.json* file:</span></span>

  ```console
  npm init -y
  ```

  <span data-ttu-id="2316d-154">Выходные данные этой команды выглядят примерно следующим образом:</span><span class="sxs-lookup"><span data-stu-id="2316d-154">The command creates output similar to the following example:</span></span>

  ```console
  Wrote to C:\tmp\SignalRChat\package.json:
  {
    "name": "SignalRChat",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1"
    },
    "keywords": [],
    "author": "",
    "license": "ISC"0
  }
  ```

* <span data-ttu-id="2316d-155">Установите пакет клиентской библиотеки:</span><span class="sxs-lookup"><span data-stu-id="2316d-155">Install the client library package:</span></span>

  ```console
  npm install @aspnet/signalr
  ```

  <span data-ttu-id="2316d-156">Выходные данные этой команды выглядят примерно следующим образом:</span><span class="sxs-lookup"><span data-stu-id="2316d-156">The command creates output similar to the following example:</span></span>

  ```
  npm notice created a lockfile as package-lock.json. You should commit this file.
  npm WARN signalrchat@1.0.0 No description
  npm WARN signalrchat@1.0.0 No repository field.

  + @aspnet/signalr@1.0.2
  added 1 package in 0.98s
  ```

<span data-ttu-id="2316d-157">Команда `npm install` загрузила клиентскую библиотеку JavaScript во вложенную папку в разделе *node_modules*.</span><span class="sxs-lookup"><span data-stu-id="2316d-157">The `npm install` command downloaded the JavaScript client library to a subfolder under *node_modules*.</span></span> <span data-ttu-id="2316d-158">Скопируйте ее оттуда в папку в разделе *wwwroot*, на которую можно ссылаться с веб-страницы приложения чата.</span><span class="sxs-lookup"><span data-stu-id="2316d-158">Copy it from there to a folder under *wwwroot* that you can reference from the chat app web page.</span></span>

* <span data-ttu-id="2316d-159">Создайте папку *signalr* в *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="2316d-159">Create a *signalr* folder in *wwwroot/lib*.</span></span>

* <span data-ttu-id="2316d-160">Скопируйте файл *signalr.js* из *node_modules/@aspnet/signalr/dist/browser* в новую папку *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="2316d-160">Copy the *signalr.js* file from *node_modules/@aspnet/signalr/dist/browser* to the new *wwwroot/lib/signalr* folder.</span></span>

## <a name="create-the-signalr-hub"></a><span data-ttu-id="2316d-161">Создание концентратора SignalR</span><span class="sxs-lookup"><span data-stu-id="2316d-161">Create the SignalR hub</span></span>

<span data-ttu-id="2316d-162">[hub](xref:signalr/hubs) — это класс, который служит в качестве конвейера высокого уровня для обработки взаимодействия между клиентом и сервером.</span><span class="sxs-lookup"><span data-stu-id="2316d-162">A [hub](xref:signalr/hubs) is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="2316d-163">В папке проекта SignalRChat создайте папку *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="2316d-163">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="2316d-164">В папке *Hubs* создайте файл *ChatHub.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="2316d-164">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="2316d-165">Класс `ChatHub` наследует от класса SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub).</span><span class="sxs-lookup"><span data-stu-id="2316d-165">The `ChatHub` class inherits from the SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) class.</span></span> <span data-ttu-id="2316d-166">Класс `Hub` управляет подключениями, группами и обменом сообщениями.</span><span class="sxs-lookup"><span data-stu-id="2316d-166">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="2316d-167">Метод `SendMessage` может вызываться любым подключенным клиентом.</span><span class="sxs-lookup"><span data-stu-id="2316d-167">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="2316d-168">Он отправляет полученное сообщение всем клиентам.</span><span class="sxs-lookup"><span data-stu-id="2316d-168">It sends the received message to all clients.</span></span> <span data-ttu-id="2316d-169">Код SignalR является асинхронным, поэтому обеспечивает максимальную масштабируемость.</span><span class="sxs-lookup"><span data-stu-id="2316d-169">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="2316d-170">Настройка проекта для использования SignalR</span><span class="sxs-lookup"><span data-stu-id="2316d-170">Configure the project to use SignalR</span></span>

<span data-ttu-id="2316d-171">Сервер SignalR необходимо настроить таким образом, чтобы он передавал запросы SignalR в SignalR.</span><span class="sxs-lookup"><span data-stu-id="2316d-171">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="2316d-172">Добавьте следующий выделенный код в файл *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="2316d-172">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="2316d-173">В результате SignalR будет добавлен в систему [внедрения зависимостей](xref:fundamentals/dependency-injection) и конвейер [ПО промежуточного слоя](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="2316d-173">These changes add SignalR to the [dependency injection](xref:fundamentals/dependency-injection) system and the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="2316d-174">Создание кода клиента SignalR</span><span class="sxs-lookup"><span data-stu-id="2316d-174">Create the SignalR client code</span></span>

* <span data-ttu-id="2316d-175">Замените содержимое в файле *Pages\Index.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="2316d-175">Replace the content in *Pages\Index.cshtml* with the following:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="2316d-176">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="2316d-176">The preceding code:</span></span>

  * <span data-ttu-id="2316d-177">Создает текстовые поля для имени и текста сообщения и кнопку отправки.</span><span class="sxs-lookup"><span data-stu-id="2316d-177">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="2316d-178">Создает список с `id="messagesList"` для отображения сообщений, полученных от концентратора SignalR.</span><span class="sxs-lookup"><span data-stu-id="2316d-178">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="2316d-179">Содержит ссылки на скрипты для SignalR и код приложения *chat.js*, который создается в следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="2316d-179">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="2316d-180">В папке *wwwroot/js* создайте файл *chat.js* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="2316d-180">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="2316d-181">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="2316d-181">The preceding code:</span></span>

  * <span data-ttu-id="2316d-182">Создает и запускает подключение.</span><span class="sxs-lookup"><span data-stu-id="2316d-182">Creates and starts a connection.</span></span>
  * <span data-ttu-id="2316d-183">Добавляет к кнопке отправки обработчик, который отправляет сообщения в концентратор.</span><span class="sxs-lookup"><span data-stu-id="2316d-183">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="2316d-184">Добавляет к объекту подключения обработчик, который получает сообщения из концентратора и добавляет их в список.</span><span class="sxs-lookup"><span data-stu-id="2316d-184">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="2316d-185">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="2316d-185">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2316d-186">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2316d-186">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2316d-187">Нажмите клавиши **CTRL+F5**, чтобы запустить приложение без отладки.</span><span class="sxs-lookup"><span data-stu-id="2316d-187">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2316d-188">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="2316d-188">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="2316d-189">Нажмите клавиши **CTRL+F5**, чтобы запустить приложение без отладки.</span><span class="sxs-lookup"><span data-stu-id="2316d-189">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2316d-190">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="2316d-190">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="2316d-191">В меню выберите **Запуск > Запуск без отладки**.</span><span class="sxs-lookup"><span data-stu-id="2316d-191">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="2316d-192">Скопируйте URL-адрес из адресной строки, откройте другой экземпляр или вкладку браузера и вставьте URL-адрес в адресную строку.</span><span class="sxs-lookup"><span data-stu-id="2316d-192">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="2316d-193">Выберите любой браузер, введите имя и сообщение и нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="2316d-193">Choose either browser, enter a name and message, and select the **Send** button.</span></span>

  <span data-ttu-id="2316d-194">Имя и сообщение отображаются на обеих страницах мгновенно.</span><span class="sxs-lookup"><span data-stu-id="2316d-194">The name and message are displayed on both pages instantly.</span></span>

  ![Пример приложения SignalR](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="2316d-196">Если приложение не работает, откройте средства разработчика для браузера (F12) и перейдите в консоль.</span><span class="sxs-lookup"><span data-stu-id="2316d-196">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="2316d-197">Вы можете увидеть ошибки, связанные с вашим кодом HTML и JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2316d-197">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="2316d-198">Предположим, вы поместили *signalr.js* не в ту папку, которую указали.</span><span class="sxs-lookup"><span data-stu-id="2316d-198">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="2316d-199">В этом случае ссылка на этот файл не будет работать, и вы увидите сообщение об ошибке 404 в консоли.</span><span class="sxs-lookup"><span data-stu-id="2316d-199">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="2316d-200">![ошибка: signalr.js не найден](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="2316d-200">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="2316d-201">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="2316d-201">Next steps</span></span>

<span data-ttu-id="2316d-202">Если вы хотите, чтобы клиенты могли подключаться к приложению SignalR из разных доменов, необходимо включить общий доступ к ресурсам независимо от источника (CORS).</span><span class="sxs-lookup"><span data-stu-id="2316d-202">If you want clients to connect to a SignalR app from different domains, you have to enable Cross-Origin Resource Sharing (CORS).</span></span> <span data-ttu-id="2316d-203">Дополнительные сведения см. в разделе [Общий доступ к ресурсам независимо от источника](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span><span class="sxs-lookup"><span data-stu-id="2316d-203">For more information, see [Cross-origin resource sharing](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span></span>

<span data-ttu-id="2316d-204">Чтобы узнать больше о SignalR, концентраторах и клиентах JavaScript, ознакомьтесь со следующими ресурсами:</span><span class="sxs-lookup"><span data-stu-id="2316d-204">To learn more about SignalR, hubs, and JavaScript clients, see these resources:</span></span>

* [<span data-ttu-id="2316d-205">Введение в SignalR для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2316d-205">Introduction to SignalR for ASP.NET Core</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="2316d-206">Использование концентраторов в SignalR для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2316d-206">Use hubs in SignalR for ASP.NET Core</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="2316d-207">Клиент ASP.NET Core SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="2316d-207">ASP.NET Core SignalR JavaScript client</span></span>](xref:signalr/javascript-client)
