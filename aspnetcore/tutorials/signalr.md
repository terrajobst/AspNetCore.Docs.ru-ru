---
title: Начало работы с SignalR ASP.NET Core
author: bradygaster
description: В этом руководстве создается приложение чата, которое использует SignalR для ASP.NET Core.
ms.author: bradyg
ms.custom: mvc
ms.date: 11/30/2018
uid: tutorials/signalr
ms.openlocfilehash: 9a77460cfd8201ca357aad3415725d4b9a30b187
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64885049"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="dfc06-103">Учебник. Начало работы с SignalR ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dfc06-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="dfc06-104">В этом руководстве описаны основы создания приложения в режиме реального времени с помощью SignalR.</span><span class="sxs-lookup"><span data-stu-id="dfc06-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="dfc06-105">Вы научитесь:</span><span class="sxs-lookup"><span data-stu-id="dfc06-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="dfc06-106">Создайте веб-проект.</span><span class="sxs-lookup"><span data-stu-id="dfc06-106">Create a web project.</span></span>
> * <span data-ttu-id="dfc06-107">добавлять клиентскую библиотеку SignalR;</span><span class="sxs-lookup"><span data-stu-id="dfc06-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="dfc06-108">создавать концентратор SignalR;</span><span class="sxs-lookup"><span data-stu-id="dfc06-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="dfc06-109">настраивать проект для использования SignalR;</span><span class="sxs-lookup"><span data-stu-id="dfc06-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="dfc06-110">Добавлять код для отправки сообщений из любого клиента всем подключенным клиентам.</span><span class="sxs-lookup"><span data-stu-id="dfc06-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="dfc06-111">В итоге вы получите работающее приложение чата:</span><span class="sxs-lookup"><span data-stu-id="dfc06-111">At the end, you'll have a working chat app:</span></span>

![Пример приложения SignalR](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="dfc06-113">[Просмотреть или скачать пример кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="dfc06-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

[!INCLUDE [|Prerequisites](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-project"></a><span data-ttu-id="dfc06-114">Создайте веб-проект.</span><span class="sxs-lookup"><span data-stu-id="dfc06-114">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dfc06-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dfc06-115">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="dfc06-116">В меню выберите **Файл > Создать проект**.</span><span class="sxs-lookup"><span data-stu-id="dfc06-116">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="dfc06-117">В диалоговом окне **Новый проект** выберите **Установленные > Visual C# > Веб > Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="dfc06-117">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="dfc06-118">Назовите проект *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="dfc06-118">Name the project *SignalRChat*.</span></span>

  ![Диалоговое окно создания проекта в Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="dfc06-120">Выберите **Веб-приложение**, чтобы создать проект, который использует Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="dfc06-120">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="dfc06-121">Выберите целевую платформу **.NET Core**, **ASP.NET Core 2.2** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="dfc06-121">Select a target framework of **.NET Core**, select **ASP.NET Core 2.2**, and click **OK**.</span></span>

  ![Диалоговое окно создания проекта в Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dfc06-123">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="dfc06-123">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="dfc06-124">Откройте [интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal) и перейдите в папку, в которой будет создаваться папка нового проекта.</span><span class="sxs-lookup"><span data-stu-id="dfc06-124">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="dfc06-125">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="dfc06-125">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="dfc06-126">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="dfc06-126">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="dfc06-127">В меню выберите **Файл > Создать решение**.</span><span class="sxs-lookup"><span data-stu-id="dfc06-127">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="dfc06-128">Выберите **.NET Core > Приложение > Веб-приложение ASP.NET Core** (не выбирайте **Веб-приложение ASP.NET Core (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="dfc06-128">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="dfc06-129">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="dfc06-129">Select **Next**.</span></span>

* <span data-ttu-id="dfc06-130">Присвойте проекту имя *SignalRChat* и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="dfc06-130">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="dfc06-131">Добавление клиентской библиотеки SignalR</span><span class="sxs-lookup"><span data-stu-id="dfc06-131">Add the SignalR client library</span></span>

<span data-ttu-id="dfc06-132">Библиотека сервера SignalR входит в состав метапакета `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="dfc06-132">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="dfc06-133">Клиентская библиотека JavaScript не добавляется в проект автоматически.</span><span class="sxs-lookup"><span data-stu-id="dfc06-133">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="dfc06-134">В рамках этого руководства вы будете использовать диспетчер библиотек (LibMan), чтобы получить клиентскую библиотеку из *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="dfc06-134">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="dfc06-135">unpkg — это сеть доставки содержимого, которая позволяет доставить любое содержимое из npm (диспетчера пакетов Node.js).</span><span class="sxs-lookup"><span data-stu-id="dfc06-135">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dfc06-136">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dfc06-136">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="dfc06-137">В **обозревателе решений** щелкните проект правой кнопкой мыши и выберите **Добавить** > **Client-Side Library** (Клиентская библиотека).</span><span class="sxs-lookup"><span data-stu-id="dfc06-137">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="dfc06-138">В диалоговом окне **Add Client-Side Library** (Добавить клиентскую библиотеку) для параметра **Поставщик** выберите **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="dfc06-138">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="dfc06-139">В поле **Библиотека** введите `@aspnet/signalr@1` и выберите последнюю версию, но не предварительную.</span><span class="sxs-lookup"><span data-stu-id="dfc06-139">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Диалоговое окно добавления клиентской библиотеки — выбор библиотеки](signalr/_static/libman1.png)

* <span data-ttu-id="dfc06-141">Щелкните **Choose specific files** (Выбрать определенные файлы), разверните папку *dist/browser* и выберите *signalr.js* и *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="dfc06-141">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="dfc06-142">В поле **Целевое расположение** укажите *wwwroot/lib/signalr/* и нажмите кнопку **Установить**.</span><span class="sxs-lookup"><span data-stu-id="dfc06-142">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Диалоговое окно добавления клиентской библиотеки — выбор файлов и назначения](signalr/_static/libman2.png)

  <span data-ttu-id="dfc06-144">LibMan создает папку *wwwroot/lib/signalr* и копирует в нее выбранные файлы.</span><span class="sxs-lookup"><span data-stu-id="dfc06-144">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dfc06-145">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="dfc06-145">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="dfc06-146">В интегрированном терминале выполните следующую команду, чтобы установить LibMan.</span><span class="sxs-lookup"><span data-stu-id="dfc06-146">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="dfc06-147">Выполните приведенную ниже команду, чтобы получить клиентскую библиотеку SignalR с помощью LibMan.</span><span class="sxs-lookup"><span data-stu-id="dfc06-147">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="dfc06-148">Возможно, придется подождать несколько секунд, прежде чем появятся выходные данные.</span><span class="sxs-lookup"><span data-stu-id="dfc06-148">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="dfc06-149">В параметрах указываются следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="dfc06-149">The parameters specify the following options:</span></span>
  * <span data-ttu-id="dfc06-150">использование поставщика unpkg;</span><span class="sxs-lookup"><span data-stu-id="dfc06-150">Use the unpkg provider.</span></span>
  * <span data-ttu-id="dfc06-151">копирование файлов в назначение *wwwroot/lib/signalr*;</span><span class="sxs-lookup"><span data-stu-id="dfc06-151">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="dfc06-152">копирование только выбранных файлов.</span><span class="sxs-lookup"><span data-stu-id="dfc06-152">Copy only the specified files.</span></span>

  <span data-ttu-id="dfc06-153">Выходные данные выглядят примерно так, как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="dfc06-153">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="dfc06-154">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="dfc06-154">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="dfc06-155">В **терминале** выполните следующую команду, чтобы установить LibMan.</span><span class="sxs-lookup"><span data-stu-id="dfc06-155">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="dfc06-156">Перейдите в папку проекта (где расположен файл *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="dfc06-156">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="dfc06-157">Выполните приведенную ниже команду, чтобы получить клиентскую библиотеку SignalR с помощью LibMan.</span><span class="sxs-lookup"><span data-stu-id="dfc06-157">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="dfc06-158">В параметрах указываются следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="dfc06-158">The parameters specify the following options:</span></span>
  * <span data-ttu-id="dfc06-159">использование поставщика unpkg;</span><span class="sxs-lookup"><span data-stu-id="dfc06-159">Use the unpkg provider.</span></span>
  * <span data-ttu-id="dfc06-160">копирование файлов в назначение *wwwroot/lib/signalr*;</span><span class="sxs-lookup"><span data-stu-id="dfc06-160">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="dfc06-161">копирование только выбранных файлов.</span><span class="sxs-lookup"><span data-stu-id="dfc06-161">Copy only the specified files.</span></span>

  <span data-ttu-id="dfc06-162">Выходные данные выглядят примерно так, как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="dfc06-162">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="dfc06-163">Создание концентратора SignalR</span><span class="sxs-lookup"><span data-stu-id="dfc06-163">Create a SignalR hub</span></span>

<span data-ttu-id="dfc06-164">*hub* — это класс, который служит в качестве конвейера высокого уровня для обработки взаимодействия между клиентом и сервером.</span><span class="sxs-lookup"><span data-stu-id="dfc06-164">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="dfc06-165">В папке проекта SignalRChat создайте папку *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="dfc06-165">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="dfc06-166">В папке *Hubs* создайте файл *ChatHub.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="dfc06-166">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="dfc06-167">Класс `ChatHub` наследует от класса `Hub` SignalR.</span><span class="sxs-lookup"><span data-stu-id="dfc06-167">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="dfc06-168">Класс `Hub` управляет подключениями, группами и обменом сообщениями.</span><span class="sxs-lookup"><span data-stu-id="dfc06-168">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="dfc06-169">Метод `SendMessage` может вызываться подключенным клиентом, чтобы отправить сообщение всем клиентам.</span><span class="sxs-lookup"><span data-stu-id="dfc06-169">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="dfc06-170">Далее в этом учебника показан клиентский код JavaScript, который вызывает метод.</span><span class="sxs-lookup"><span data-stu-id="dfc06-170">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="dfc06-171">Код SignalR является асинхронным, поэтому обеспечивает максимальную масштабируемость.</span><span class="sxs-lookup"><span data-stu-id="dfc06-171">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="dfc06-172">Настройка SignalR</span><span class="sxs-lookup"><span data-stu-id="dfc06-172">Configure SignalR</span></span>

<span data-ttu-id="dfc06-173">Сервер SignalR необходимо настроить таким образом, чтобы он передавал запросы SignalR в SignalR.</span><span class="sxs-lookup"><span data-stu-id="dfc06-173">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="dfc06-174">Добавьте следующий выделенный код в файл *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="dfc06-174">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="dfc06-175">В результате SignalR будет добавлен в систему внедрения зависимостей ASP.NET Core и конвейер ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="dfc06-175">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="dfc06-176">Добавление кода клиента SignalR</span><span class="sxs-lookup"><span data-stu-id="dfc06-176">Add SignalR client code</span></span>

* <span data-ttu-id="dfc06-177">Замените содержимое в файле *Pages\Index.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="dfc06-177">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="dfc06-178">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="dfc06-178">The preceding code:</span></span>

  * <span data-ttu-id="dfc06-179">Создает текстовые поля для имени и текста сообщения и кнопку отправки.</span><span class="sxs-lookup"><span data-stu-id="dfc06-179">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="dfc06-180">Создает список с `id="messagesList"` для отображения сообщений, полученных от концентратора SignalR.</span><span class="sxs-lookup"><span data-stu-id="dfc06-180">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="dfc06-181">Содержит ссылки на скрипты для SignalR и код приложения *chat.js*, который создается в следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="dfc06-181">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="dfc06-182">В папке *wwwroot/js* создайте файл *chat.js* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="dfc06-182">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="dfc06-183">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="dfc06-183">The preceding code:</span></span>

  * <span data-ttu-id="dfc06-184">Создает и запускает подключение.</span><span class="sxs-lookup"><span data-stu-id="dfc06-184">Creates and starts a connection.</span></span>
  * <span data-ttu-id="dfc06-185">Добавляет к кнопке отправки обработчик, который отправляет сообщения в концентратор.</span><span class="sxs-lookup"><span data-stu-id="dfc06-185">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="dfc06-186">Добавляет к объекту подключения обработчик, который получает сообщения из концентратора и добавляет их в список.</span><span class="sxs-lookup"><span data-stu-id="dfc06-186">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="dfc06-187">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="dfc06-187">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dfc06-188">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dfc06-188">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="dfc06-189">Нажмите клавиши **CTRL+F5**, чтобы запустить приложение без отладки.</span><span class="sxs-lookup"><span data-stu-id="dfc06-189">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dfc06-190">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="dfc06-190">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="dfc06-191">В интегрированном терминале выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="dfc06-191">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="dfc06-192">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="dfc06-192">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="dfc06-193">В меню выберите **Запуск > Запуск без отладки**.</span><span class="sxs-lookup"><span data-stu-id="dfc06-193">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="dfc06-194">Скопируйте URL-адрес из адресной строки, откройте другой экземпляр или вкладку браузера и вставьте URL-адрес в адресную строку.</span><span class="sxs-lookup"><span data-stu-id="dfc06-194">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="dfc06-195">Выберите любой браузер, введите имя и сообщение и нажмите кнопку **Отправить сообщение**.</span><span class="sxs-lookup"><span data-stu-id="dfc06-195">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="dfc06-196">Имя и сообщение отображаются на обеих страницах мгновенно.</span><span class="sxs-lookup"><span data-stu-id="dfc06-196">The name and message are displayed on both pages instantly.</span></span>

  ![Пример приложения SignalR](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="dfc06-198">Если приложение не работает, откройте средства разработчика для браузера (F12) и перейдите в консоль.</span><span class="sxs-lookup"><span data-stu-id="dfc06-198">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="dfc06-199">Вы можете увидеть ошибки, связанные с вашим кодом HTML и JavaScript.</span><span class="sxs-lookup"><span data-stu-id="dfc06-199">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="dfc06-200">Предположим, вы поместили *signalr.js* не в ту папку, которую указали.</span><span class="sxs-lookup"><span data-stu-id="dfc06-200">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="dfc06-201">В этом случае ссылка на этот файл не будет работать, и вы увидите сообщение об ошибке 404 в консоли.</span><span class="sxs-lookup"><span data-stu-id="dfc06-201">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="dfc06-202">![ошибка: signalr.js не найден](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="dfc06-202">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="dfc06-203">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="dfc06-203">Next steps</span></span>

<span data-ttu-id="dfc06-204">В этом руководстве вы узнали, как:</span><span class="sxs-lookup"><span data-stu-id="dfc06-204">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="dfc06-205">создавать проект веб-приложения;</span><span class="sxs-lookup"><span data-stu-id="dfc06-205">Create a web app project.</span></span>
> * <span data-ttu-id="dfc06-206">добавлять клиентскую библиотеку SignalR;</span><span class="sxs-lookup"><span data-stu-id="dfc06-206">Add the SignalR client library.</span></span>
> * <span data-ttu-id="dfc06-207">создавать концентратор SignalR;</span><span class="sxs-lookup"><span data-stu-id="dfc06-207">Create a SignalR hub.</span></span>
> * <span data-ttu-id="dfc06-208">настраивать проект для использования SignalR;</span><span class="sxs-lookup"><span data-stu-id="dfc06-208">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="dfc06-209">добавлять код, использующий концентратор для отправки сообщений из любого клиента всем подключенным клиентам.</span><span class="sxs-lookup"><span data-stu-id="dfc06-209">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="dfc06-210">Дополнительные сведения о SignalR см. во введении:</span><span class="sxs-lookup"><span data-stu-id="dfc06-210">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="dfc06-211">Введение в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="dfc06-211">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
