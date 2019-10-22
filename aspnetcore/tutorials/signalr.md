---
title: Начало работы с SignalR ASP.NET Core
author: bradygaster
description: В этом руководстве создается приложение чата, которое использует SignalR для ASP.NET Core.
ms.author: bradyg
ms.custom: mvc
ms.date: 10/03/2019
uid: tutorials/signalr
ms.openlocfilehash: 078f1875d22a90f90575826e6f212205cd4b3d5b
ms.sourcegitcommit: e71b6a85b0e94a600af607107e298f932924c849
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/17/2019
ms.locfileid: "72519176"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="e7ddb-103">Учебник. Начало работы с SignalR ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e7ddb-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e7ddb-104">В этом руководстве описаны основы создания приложения в режиме реального времени с помощью SignalR.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="e7ddb-105">Вы научитесь:</span><span class="sxs-lookup"><span data-stu-id="e7ddb-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e7ddb-106">Создайте веб-проект.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-106">Create a web project.</span></span>
> * <span data-ttu-id="e7ddb-107">добавлять клиентскую библиотеку SignalR;</span><span class="sxs-lookup"><span data-stu-id="e7ddb-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="e7ddb-108">создавать концентратор SignalR;</span><span class="sxs-lookup"><span data-stu-id="e7ddb-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="e7ddb-109">настраивать проект для использования SignalR;</span><span class="sxs-lookup"><span data-stu-id="e7ddb-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="e7ddb-110">Добавлять код для отправки сообщений из любого клиента всем подключенным клиентам.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="e7ddb-111">В итоге вы получите работающее приложение чата:</span><span class="sxs-lookup"><span data-stu-id="e7ddb-111">At the end, you'll have a working chat app:</span></span>

![Пример приложения SignalR](signalr/_static/3.x/signalr-get-started-finished.png)

## <a name="prerequisites"></a><span data-ttu-id="e7ddb-113">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="e7ddb-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7ddb-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7ddb-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e7ddb-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e7ddb-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e7ddb-116">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="e7ddb-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app-project"></a><span data-ttu-id="e7ddb-117">Создание проекта веб-приложения</span><span class="sxs-lookup"><span data-stu-id="e7ddb-117">Create a web app project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7ddb-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7ddb-118">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="e7ddb-119">В меню выберите **Файл > Создать проект**.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-119">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="e7ddb-120">В диалоговом окне **Создать проект** выберите **Веб-приложение ASP.NET Core** и нажмите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**, and then select **Next**.</span></span>

* <span data-ttu-id="e7ddb-121">В диалоговом окне **Настроить новый проект** укажите имя проекта *SignalRChat*, а затем выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-121">In the **Configure your new project** dialog, name the project *SignalRChat*, and then select **Create**.</span></span>

* <span data-ttu-id="e7ddb-122">В диалоговом окне **Создание веб-приложения ASP.NET Core** выберите платформы **.NET Core** и **ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-122">In the **Create a new ASP.NET Core web Application** dialog, select **.NET Core** and **ASP.NET Core 3.0**.</span></span> 

* <span data-ttu-id="e7ddb-123">Выберите **Веб-приложение**, чтобы создать проект, который использует Razor Pages, и нажмите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-123">Select **Web Application** to create a project that uses Razor Pages, and then select **Create**.</span></span>

  ![Диалоговое окно создания проекта в Visual Studio](signalr/_static/3.x/signalr-new-project-dialog.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e7ddb-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e7ddb-125">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="e7ddb-126">Откройте [интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal) и перейдите в папку, в которой будет создаваться папка нового проекта.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="e7ddb-127">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="e7ddb-127">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e7ddb-128">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="e7ddb-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e7ddb-129">В меню выберите **Файл > Создать решение**.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-129">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="e7ddb-130">Выберите **.NET Core > Приложение > Веб-приложение** (не выбирайте **Веб-приложение (модель — представление — контроллер)** ), а затем выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-130">Select **.NET Core > App > Web Application** (Don't select **Web Application (Model-View-Controller)**), and then select **Next**.</span></span>

* <span data-ttu-id="e7ddb-131">Убедитесь, что для параметра **Требуемая версия .NET Framework** указано **.NET Core 3.0**, а затем выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-131">Make sure the **Target Framework** is set to **.NET Core 3.0**, and then select **Next**.</span></span>

* <span data-ttu-id="e7ddb-132">Присвойте проекту имя *SignalRChat* и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-132">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="e7ddb-133">Добавление клиентской библиотеки SignalR</span><span class="sxs-lookup"><span data-stu-id="e7ddb-133">Add the SignalR client library</span></span>

<span data-ttu-id="e7ddb-134">Библиотека сервера SignalR входит в состав общей платформы ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-134">The SignalR server library is included in the ASP.NET Core 3.0 shared framework.</span></span> <span data-ttu-id="e7ddb-135">Клиентская библиотека JavaScript не добавляется в проект автоматически.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-135">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="e7ddb-136">В рамках этого руководства вы будете использовать диспетчер библиотек (LibMan), чтобы получить клиентскую библиотеку из *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-136">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="e7ddb-137">unpkg — это сеть доставки содержимого, которая позволяет доставить любое содержимое из npm (диспетчера пакетов Node.js).</span><span class="sxs-lookup"><span data-stu-id="e7ddb-137">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7ddb-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7ddb-138">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="e7ddb-139">В **обозревателе решений** щелкните проект правой кнопкой мыши и выберите **Добавить** > **Клиентская библиотека**.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-139">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="e7ddb-140">В диалоговом окне **Add Client-Side Library** (Добавить клиентскую библиотеку) для параметра **Поставщик** выберите **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-140">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="e7ddb-141">Для параметра **Библиотека** введите `@microsoft/signalr@latest`.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-141">For **Library**, enter `@microsoft/signalr@latest`.</span></span>

* <span data-ttu-id="e7ddb-142">Щелкните **Choose specific files** (Выбрать определенные файлы), разверните папку *dist/browser* и выберите *signalr.js* и *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-142">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="e7ddb-143">В поле **Целевое расположение** укажите *wwwroot/js/signalr/* и нажмите кнопку **Установить**.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-143">Set **Target Location** to *wwwroot/js/signalr/*, and select **Install**.</span></span>

  ![Диалоговое окно добавления клиентской библиотеки — выбор библиотеки](signalr/_static/3.x/find-signalr-client-libs-select-files.png)

  <span data-ttu-id="e7ddb-145">LibMan создает папку *wwwroot/js/signalr* и копирует в нее выбранные файлы.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-145">LibMan creates a *wwwroot/js/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e7ddb-146">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e7ddb-146">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="e7ddb-147">В интегрированном терминале выполните следующую команду, чтобы установить LibMan.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-147">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="e7ddb-148">Выполните приведенную ниже команду, чтобы получить клиентскую библиотеку SignalR с помощью LibMan.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-148">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="e7ddb-149">Возможно, придется подождать несколько секунд, прежде чем появятся выходные данные.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-149">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @microsoft/signalr@latest -p unpkg -d wwwroot/js/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="e7ddb-150">В параметрах указываются следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="e7ddb-150">The parameters specify the following options:</span></span>
  * <span data-ttu-id="e7ddb-151">использование поставщика unpkg;</span><span class="sxs-lookup"><span data-stu-id="e7ddb-151">Use the unpkg provider.</span></span>
  * <span data-ttu-id="e7ddb-152">копирование файлов в назначение *wwwroot/js/signalr*;</span><span class="sxs-lookup"><span data-stu-id="e7ddb-152">Copy files to the *wwwroot/js/signalr* destination.</span></span>
  * <span data-ttu-id="e7ddb-153">копирование только выбранных файлов.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-153">Copy only the specified files.</span></span>

  <span data-ttu-id="e7ddb-154">Выходные данные выглядят примерно так, как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="e7ddb-154">The output looks like the following example:</span></span>

  ```console
  wwwroot/js/signalr/dist/browser/signalr.js written to disk
  wwwroot/js/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@microsoft/signalr@latest" to "wwwroot/js/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e7ddb-155">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="e7ddb-155">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e7ddb-156">В **терминале** выполните следующую команду, чтобы установить LibMan.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-156">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="e7ddb-157">Перейдите в папку проекта (где расположен файл *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="e7ddb-157">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="e7ddb-158">Выполните приведенную ниже команду, чтобы получить клиентскую библиотеку SignalR с помощью LibMan.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-158">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @microsoft/signalr@latest -p unpkg -d wwwroot/js/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="e7ddb-159">В параметрах указываются следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="e7ddb-159">The parameters specify the following options:</span></span>
  * <span data-ttu-id="e7ddb-160">использование поставщика unpkg;</span><span class="sxs-lookup"><span data-stu-id="e7ddb-160">Use the unpkg provider.</span></span>
  * <span data-ttu-id="e7ddb-161">копирование файлов в назначение *wwwroot/js/signalr*;</span><span class="sxs-lookup"><span data-stu-id="e7ddb-161">Copy files to the *wwwroot/js/signalr* destination.</span></span>
  * <span data-ttu-id="e7ddb-162">копирование только выбранных файлов.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-162">Copy only the specified files.</span></span>

  <span data-ttu-id="e7ddb-163">Выходные данные выглядят примерно так, как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="e7ddb-163">The output looks like the following example:</span></span>

  ```console
  wwwroot/js/signalr/dist/browser/signalr.js written to disk
  wwwroot/js/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@microsoft/signalr@latest" to "wwwroot/js/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="e7ddb-164">Создание концентратора SignalR</span><span class="sxs-lookup"><span data-stu-id="e7ddb-164">Create a SignalR hub</span></span>

<span data-ttu-id="e7ddb-165">*hub* — это класс, который служит в качестве конвейера высокого уровня для обработки взаимодействия между клиентом и сервером.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-165">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="e7ddb-166">В папке проекта SignalRChat создайте папку *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-166">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="e7ddb-167">В папке *Hubs* создайте файл *ChatHub.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="e7ddb-167">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[ChatHub](signalr/sample-snapshot/3.x/ChatHub.cs)]

  <span data-ttu-id="e7ddb-168">Класс `ChatHub` наследует от класса `Hub` SignalR.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-168">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="e7ddb-169">Класс `Hub` управляет подключениями, группами и обменом сообщениями.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-169">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="e7ddb-170">Метод `SendMessage` может вызываться подключенным клиентом, чтобы отправить сообщение всем клиентам.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-170">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="e7ddb-171">Далее в этом учебника показан клиентский код JavaScript, который вызывает метод.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-171">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="e7ddb-172">Код SignalR является асинхронным, поэтому обеспечивает максимальную масштабируемость.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-172">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="e7ddb-173">Настройка SignalR</span><span class="sxs-lookup"><span data-stu-id="e7ddb-173">Configure SignalR</span></span>

<span data-ttu-id="e7ddb-174">Сервер SignalR необходимо настроить таким образом, чтобы он передавал запросы SignalR в SignalR.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-174">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="e7ddb-175">Добавьте следующий выделенный код в файл *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-175">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/3.x/Startup.cs?highlight=11,28,55)]

  <span data-ttu-id="e7ddb-176">В результате SignalR будет добавлен в системы внедрения зависимостей и маршрутизации ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-176">These changes add SignalR to the ASP.NET Core dependency injection and routing systems.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="e7ddb-177">Добавление кода клиента SignalR</span><span class="sxs-lookup"><span data-stu-id="e7ddb-177">Add SignalR client code</span></span>

* <span data-ttu-id="e7ddb-178">Замените содержимое в файле *Pages\Index.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="e7ddb-178">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample-snapshot/3.x/Index.cshtml)]

  <span data-ttu-id="e7ddb-179">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="e7ddb-179">The preceding code:</span></span>

  * <span data-ttu-id="e7ddb-180">Создает текстовые поля для имени и текста сообщения и кнопку отправки.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-180">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="e7ddb-181">Создает список с `id="messagesList"` для отображения сообщений, полученных от концентратора SignalR.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-181">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="e7ddb-182">Содержит ссылки на скрипты для SignalR и код приложения *chat.js*, который создается в следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-182">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="e7ddb-183">В папке *wwwroot/js* создайте файл *chat.js* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="e7ddb-183">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[chat](signalr/sample-snapshot/3.x/chat.js)]

  <span data-ttu-id="e7ddb-184">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="e7ddb-184">The preceding code:</span></span>

  * <span data-ttu-id="e7ddb-185">Создает и запускает подключение.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-185">Creates and starts a connection.</span></span>
  * <span data-ttu-id="e7ddb-186">Добавляет к кнопке отправки обработчик, который отправляет сообщения в концентратор.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-186">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="e7ddb-187">Добавляет к объекту подключения обработчик, который получает сообщения из концентратора и добавляет их в список.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-187">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="e7ddb-188">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="e7ddb-188">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7ddb-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7ddb-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e7ddb-190">Нажмите клавиши **CTRL+F5**, чтобы запустить приложение без отладки.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-190">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e7ddb-191">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e7ddb-191">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e7ddb-192">В интегрированном терминале выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="e7ddb-192">In the integrated terminal, run the following command:</span></span>

  ```dotnetcli
  dotnet watch run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e7ddb-193">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="e7ddb-193">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e7ddb-194">В меню выберите **Запуск > Запуск без отладки**.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-194">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="e7ddb-195">Скопируйте URL-адрес из адресной строки, откройте другой экземпляр или вкладку браузера и вставьте URL-адрес в адресную строку.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-195">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="e7ddb-196">Выберите любой браузер, введите имя и сообщение и нажмите кнопку **Отправить сообщение**.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-196">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="e7ddb-197">Имя и сообщение отображаются на обеих страницах мгновенно.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-197">The name and message are displayed on both pages instantly.</span></span>

  ![Пример приложения SignalR](signalr/_static/3.x/signalr-get-started-finished.png)

> [!TIP]
> * <span data-ttu-id="e7ddb-199">Если приложение не работает, откройте средства разработчика для браузера (F12) и перейдите в консоль.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-199">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="e7ddb-200">Вы можете увидеть ошибки, связанные с вашим кодом HTML и JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-200">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="e7ddb-201">Предположим, вы поместили *signalr.js* не в ту папку, которую указали.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-201">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="e7ddb-202">В этом случае ссылка на этот файл не будет работать, и вы увидите сообщение об ошибке 404 в консоли.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-202">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
>   <span data-ttu-id="e7ddb-203">![ошибка: signalr.js не найден](signalr/_static/3.x/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="e7ddb-203">![signalr.js not found error](signalr/_static/3.x/f12-console.png)</span></span>
> * <span data-ttu-id="e7ddb-204">Если возникает ошибка ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY в Chrome, выполните эти команды, чтобы обновить сертификат разработки:</span><span class="sxs-lookup"><span data-stu-id="e7ddb-204">If you get the error ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY in Chrome, run these commands to update your development certificate:</span></span>
>
>   ```dotnetcli
>   dotnet dev-certs https --clean
>   dotnet dev-certs https --trust
>   ```

## <a name="next-steps"></a><span data-ttu-id="e7ddb-205">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="e7ddb-205">Next steps</span></span>

<span data-ttu-id="e7ddb-206">Дополнительные сведения о SignalR см. во введении:</span><span class="sxs-lookup"><span data-stu-id="e7ddb-206">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="e7ddb-207">Введение в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="e7ddb-207">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e7ddb-208">В этом руководстве описаны основы создания приложения в режиме реального времени с помощью SignalR.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-208">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="e7ddb-209">Вы научитесь:</span><span class="sxs-lookup"><span data-stu-id="e7ddb-209">You learn how to:</span></span>   

> [!div class="checklist"]  
> * <span data-ttu-id="e7ddb-210">Создайте веб-проект.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-210">Create a web project.</span></span>   
> * <span data-ttu-id="e7ddb-211">добавлять клиентскую библиотеку SignalR;</span><span class="sxs-lookup"><span data-stu-id="e7ddb-211">Add the SignalR client library.</span></span> 
> * <span data-ttu-id="e7ddb-212">создавать концентратор SignalR;</span><span class="sxs-lookup"><span data-stu-id="e7ddb-212">Create a SignalR hub.</span></span>   
> * <span data-ttu-id="e7ddb-213">настраивать проект для использования SignalR;</span><span class="sxs-lookup"><span data-stu-id="e7ddb-213">Configure the project to use SignalR.</span></span>   
> * <span data-ttu-id="e7ddb-214">Добавлять код для отправки сообщений из любого клиента всем подключенным клиентам.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-214">Add code that sends messages from any client to all connected clients.</span></span>  
<span data-ttu-id="e7ddb-215">В итоге вы получите работающее приложение чата: ![Пример приложения SignalR](signalr/_static/2.x/signalr-get-started-finished.png)</span><span class="sxs-lookup"><span data-stu-id="e7ddb-215">At the end, you'll have a working chat app: ![SignalR sample app](signalr/_static/2.x/signalr-get-started-finished.png)</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="e7ddb-216">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="e7ddb-216">Prerequisites</span></span>    

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7ddb-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7ddb-217">Visual Studio</span></span>](#tab/visual-studio)   

[!INCLUDE[](~/includes/net-core-prereqs-vs2017-2.2.md)] 

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e7ddb-218">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e7ddb-218">Visual Studio Code</span></span>](#tab/visual-studio-code) 

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]    

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e7ddb-219">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="e7ddb-219">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)   

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]    

--- 

## <a name="create-a-web-project"></a><span data-ttu-id="e7ddb-220">Создайте веб-проект.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-220">Create a web project</span></span> 

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7ddb-221">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7ddb-221">Visual Studio</span></span>](#tab/visual-studio/)  

* <span data-ttu-id="e7ddb-222">В меню выберите **Файл > Создать проект**.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-222">From the menu, select **File > New Project**.</span></span> 

* <span data-ttu-id="e7ddb-223">В диалоговом окне **Новый проект** выберите **Установленные > Visual C# > Веб > Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-223">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="e7ddb-224">Назовите проект *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-224">Name the project *SignalRChat*.</span></span> 

  ![Диалоговое окно создания проекта в Visual Studio](signalr/_static/2.x/signalr-new-project-dialog.png)    

* <span data-ttu-id="e7ddb-226">Выберите **Веб-приложение**, чтобы создать проект, который использует Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-226">Select **Web Application** to create a project that uses Razor Pages.</span></span> 

* <span data-ttu-id="e7ddb-227">Выберите целевую платформу **.NET Core**, **ASP.NET Core 2.2** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-227">Select a target framework of **.NET Core**, select **ASP.NET Core 2.2**, and click **OK**.</span></span>    

  ![Диалоговое окно создания проекта в Visual Studio](signalr/_static/2.x/signalr-new-project-choose-type.png)   

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e7ddb-229">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e7ddb-229">Visual Studio Code</span></span>](#tab/visual-studio-code/)    

* <span data-ttu-id="e7ddb-230">Откройте [интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal) и перейдите в папку, в которой будет создаваться папка нового проекта.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-230">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>  

* <span data-ttu-id="e7ddb-231">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="e7ddb-231">Run the following commands:</span></span>   

   ```dotnetcli 
   dotnet new webapp -o SignalRChat 
   code -r SignalRChat  
   ```  

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e7ddb-232">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="e7ddb-232">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)   

* <span data-ttu-id="e7ddb-233">В меню выберите **Файл > Создать решение**.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-233">From the menu, select **File > New Solution**.</span></span>    

* <span data-ttu-id="e7ddb-234">Выберите **.NET Core > Приложение > Веб-приложение ASP.NET Core** (не выбирайте **Веб-приложение ASP.NET Core (MVC)** ).</span><span class="sxs-lookup"><span data-stu-id="e7ddb-234">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>  

* <span data-ttu-id="e7ddb-235">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-235">Select **Next**.</span></span>  

* <span data-ttu-id="e7ddb-236">Присвойте проекту имя *SignalRChat* и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-236">Name the project *SignalRChat*, and then select **Create**.</span></span>   

--- 

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="e7ddb-237">Добавление клиентской библиотеки SignalR</span><span class="sxs-lookup"><span data-stu-id="e7ddb-237">Add the SignalR client library</span></span>   

<span data-ttu-id="e7ddb-238">Библиотека сервера SignalR входит в состав метапакета `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-238">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="e7ddb-239">Клиентская библиотека JavaScript не добавляется в проект автоматически.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-239">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="e7ddb-240">В рамках этого руководства вы будете использовать диспетчер библиотек (LibMan), чтобы получить клиентскую библиотеку из *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-240">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="e7ddb-241">unpkg — это сеть доставки содержимого, которая позволяет доставить любое содержимое из npm (диспетчера пакетов Node.js).</span><span class="sxs-lookup"><span data-stu-id="e7ddb-241">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>    

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7ddb-242">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7ddb-242">Visual Studio</span></span>](#tab/visual-studio/)  

* <span data-ttu-id="e7ddb-243">В **обозревателе решений** щелкните проект правой кнопкой мыши и выберите **Добавить** > **Клиентская библиотека**.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-243">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>  

* <span data-ttu-id="e7ddb-244">В диалоговом окне **Add Client-Side Library** (Добавить клиентскую библиотеку) для параметра **Поставщик** выберите **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-244">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="e7ddb-245">В поле **Библиотека** введите `@aspnet/signalr@1` и выберите последнюю версию, но не предварительную.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-245">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span> 

  ![Диалоговое окно добавления клиентской библиотеки — выбор библиотеки](signalr/_static/2.x/libman1.png)   

* <span data-ttu-id="e7ddb-247">Щелкните **Choose specific files** (Выбрать определенные файлы), разверните папку *dist/browser* и выберите *signalr.js* и *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-247">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span> 

* <span data-ttu-id="e7ddb-248">В поле **Целевое расположение** укажите *wwwroot/lib/signalr/* и нажмите кнопку **Установить**.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-248">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>    

  ![Диалоговое окно добавления клиентской библиотеки — выбор файлов и назначения](signalr/_static/2.x/libman2.png) 

  <span data-ttu-id="e7ddb-250">LibMan создает папку *wwwroot/lib/signalr* и копирует в нее выбранные файлы.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-250">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>    

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e7ddb-251">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e7ddb-251">Visual Studio Code</span></span>](#tab/visual-studio-code/)    

* <span data-ttu-id="e7ddb-252">В интегрированном терминале выполните следующую команду, чтобы установить LibMan.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-252">In the integrated terminal, run the following command to install LibMan.</span></span>  

  ```dotnetcli  
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli   
  ```   

* <span data-ttu-id="e7ddb-253">Выполните приведенную ниже команду, чтобы получить клиентскую библиотеку SignalR с помощью LibMan.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-253">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="e7ddb-254">Возможно, придется подождать несколько секунд, прежде чем появятся выходные данные.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-254">You might have to wait a few seconds before seeing output.</span></span>   

  ```console    
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js    
  ```   

  <span data-ttu-id="e7ddb-255">В параметрах указываются следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="e7ddb-255">The parameters specify the following options:</span></span> 
  * <span data-ttu-id="e7ddb-256">использование поставщика unpkg;</span><span class="sxs-lookup"><span data-stu-id="e7ddb-256">Use the unpkg provider.</span></span> 
  * <span data-ttu-id="e7ddb-257">копирование файлов в назначение *wwwroot/lib/signalr*;</span><span class="sxs-lookup"><span data-stu-id="e7ddb-257">Copy files to the *wwwroot/lib/signalr* destination.</span></span>    
  * <span data-ttu-id="e7ddb-258">копирование только выбранных файлов.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-258">Copy only the specified files.</span></span>  

  <span data-ttu-id="e7ddb-259">Выходные данные выглядят примерно так, как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="e7ddb-259">The output looks like the following example:</span></span>  

  ```console    
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk   
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk   
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"    
  ```   

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e7ddb-260">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="e7ddb-260">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)   

* <span data-ttu-id="e7ddb-261">В **терминале** выполните следующую команду, чтобы установить LibMan.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-261">In the **Terminal**, run the following command to install LibMan.</span></span> 

  ```dotnetcli  
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli   
  ```   

* <span data-ttu-id="e7ddb-262">Перейдите в папку проекта (где расположен файл *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="e7ddb-262">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span> 

* <span data-ttu-id="e7ddb-263">Выполните приведенную ниже команду, чтобы получить клиентскую библиотеку SignalR с помощью LibMan.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-263">Run the following command to get the SignalR client library by using LibMan.</span></span>  

  ```console    
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js    
  ```   

  <span data-ttu-id="e7ddb-264">В параметрах указываются следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="e7ddb-264">The parameters specify the following options:</span></span> 
  * <span data-ttu-id="e7ddb-265">использование поставщика unpkg;</span><span class="sxs-lookup"><span data-stu-id="e7ddb-265">Use the unpkg provider.</span></span> 
  * <span data-ttu-id="e7ddb-266">копирование файлов в назначение *wwwroot/lib/signalr*;</span><span class="sxs-lookup"><span data-stu-id="e7ddb-266">Copy files to the *wwwroot/lib/signalr* destination.</span></span>    
  * <span data-ttu-id="e7ddb-267">копирование только выбранных файлов.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-267">Copy only the specified files.</span></span>  

  <span data-ttu-id="e7ddb-268">Выходные данные выглядят примерно так, как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="e7ddb-268">The output looks like the following example:</span></span>  

  ```console    
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk   
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk   
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"    
  ```   

--- 

## <a name="create-a-signalr-hub"></a><span data-ttu-id="e7ddb-269">Создание концентратора SignalR</span><span class="sxs-lookup"><span data-stu-id="e7ddb-269">Create a SignalR hub</span></span> 

<span data-ttu-id="e7ddb-270">*hub* — это класс, который служит в качестве конвейера высокого уровня для обработки взаимодействия между клиентом и сервером.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-270">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>   

* <span data-ttu-id="e7ddb-271">В папке проекта SignalRChat создайте папку *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-271">In the SignalRChat project folder, create a *Hubs* folder.</span></span>    

* <span data-ttu-id="e7ddb-272">В папке *Hubs* создайте файл *ChatHub.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="e7ddb-272">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span> 

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/ChatHub.cs)]   

  <span data-ttu-id="e7ddb-273">Класс `ChatHub` наследует от класса `Hub` SignalR.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-273">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="e7ddb-274">Класс `Hub` управляет подключениями, группами и обменом сообщениями.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-274">The `Hub` class manages connections, groups, and messaging.</span></span>    

  <span data-ttu-id="e7ddb-275">Метод `SendMessage` может вызываться подключенным клиентом, чтобы отправить сообщение всем клиентам.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-275">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="e7ddb-276">Далее в этом учебника показан клиентский код JavaScript, который вызывает метод.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-276">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="e7ddb-277">Код SignalR является асинхронным, поэтому обеспечивает максимальную масштабируемость.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-277">SignalR code is asynchronous to provide maximum scalability.</span></span>  

## <a name="configure-signalr"></a><span data-ttu-id="e7ddb-278">Настройка SignalR</span><span class="sxs-lookup"><span data-stu-id="e7ddb-278">Configure SignalR</span></span>    

<span data-ttu-id="e7ddb-279">Сервер SignalR необходимо настроить таким образом, чтобы он передавал запросы SignalR в SignalR.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-279">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>  

* <span data-ttu-id="e7ddb-280">Добавьте следующий выделенный код в файл *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-280">Add the following highlighted code to the *Startup.cs* file.</span></span>  

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/Startup.cs?highlight=7,33,52-55)]  

  <span data-ttu-id="e7ddb-281">В результате SignalR будет добавлен в систему внедрения зависимостей ASP.NET Core и конвейер ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-281">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>    

## <a name="add-signalr-client-code"></a><span data-ttu-id="e7ddb-282">Добавление кода клиента SignalR</span><span class="sxs-lookup"><span data-stu-id="e7ddb-282">Add SignalR client code</span></span>  

* <span data-ttu-id="e7ddb-283">Замените содержимое в файле *Pages\Index.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="e7ddb-283">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>  

  [!code-cshtml[Index](signalr/sample-snapshot/2.x/Index.cshtml)]   

  <span data-ttu-id="e7ddb-284">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="e7ddb-284">The preceding code:</span></span>   

  * <span data-ttu-id="e7ddb-285">Создает текстовые поля для имени и текста сообщения и кнопку отправки.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-285">Creates text boxes for name and message text, and a submit button.</span></span>  
  * <span data-ttu-id="e7ddb-286">Создает список с `id="messagesList"` для отображения сообщений, полученных от концентратора SignalR.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-286">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span> 
  * <span data-ttu-id="e7ddb-287">Содержит ссылки на скрипты для SignalR и код приложения *chat.js*, который создается в следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-287">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>  

* <span data-ttu-id="e7ddb-288">В папке *wwwroot/js* создайте файл *chat.js* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="e7ddb-288">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>  

  [!code-javascript[Index](signalr/sample-snapshot/2.x/chat.js)]    

  <span data-ttu-id="e7ddb-289">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="e7ddb-289">The preceding code:</span></span>   

  * <span data-ttu-id="e7ddb-290">Создает и запускает подключение.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-290">Creates and starts a connection.</span></span>    
  * <span data-ttu-id="e7ddb-291">Добавляет к кнопке отправки обработчик, который отправляет сообщения в концентратор.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-291">Adds to the submit button a handler that sends messages to the hub.</span></span> 
  * <span data-ttu-id="e7ddb-292">Добавляет к объекту подключения обработчик, который получает сообщения из концентратора и добавляет их в список.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-292">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>  

## <a name="run-the-app"></a><span data-ttu-id="e7ddb-293">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="e7ddb-293">Run the app</span></span>  

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7ddb-294">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7ddb-294">Visual Studio</span></span>](#tab/visual-studio)   

* <span data-ttu-id="e7ddb-295">Нажмите клавиши **CTRL+F5**, чтобы запустить приложение без отладки.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-295">Press **CTRL+F5** to run the app without debugging.</span></span>   

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e7ddb-296">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e7ddb-296">Visual Studio Code</span></span>](#tab/visual-studio-code) 

* <span data-ttu-id="e7ddb-297">В интегрированном терминале выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="e7ddb-297">In the integrated terminal, run the following command:</span></span>    

  ```dotnetcli  
  dotnet run -p SignalRChat.csproj  
  ```   

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e7ddb-298">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="e7ddb-298">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)   

* <span data-ttu-id="e7ddb-299">В меню выберите **Запуск > Запуск без отладки**.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-299">From the menu, select **Run > Start Without Debugging**.</span></span>  

--- 

* <span data-ttu-id="e7ddb-300">Скопируйте URL-адрес из адресной строки, откройте другой экземпляр или вкладку браузера и вставьте URL-адрес в адресную строку.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-300">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>    

* <span data-ttu-id="e7ddb-301">Выберите любой браузер, введите имя и сообщение и нажмите кнопку **Отправить сообщение**.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-301">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>  

  <span data-ttu-id="e7ddb-302">Имя и сообщение отображаются на обеих страницах мгновенно.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-302">The name and message are displayed on both pages instantly.</span></span>   

  ![Пример приложения SignalR](signalr/_static/2.x/signalr-get-started-finished.png)   

> [!TIP]    
> <span data-ttu-id="e7ddb-304">Если приложение не работает, откройте средства разработчика для браузера (F12) и перейдите в консоль.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-304">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="e7ddb-305">Вы можете увидеть ошибки, связанные с вашим кодом HTML и JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-305">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="e7ddb-306">Предположим, вы поместили *signalr.js* не в ту папку, которую указали.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-306">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="e7ddb-307">В этом случае ссылка на этот файл не будет работать, и вы увидите сообщение об ошибке 404 в консоли.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-307">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>   
> <span data-ttu-id="e7ddb-308">![ошибка: signalr.js не найден](signalr/_static/2.x/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="e7ddb-308">![signalr.js not found error](signalr/_static/2.x/f12-console.png)</span></span>    
## <a name="additional-resources"></a><span data-ttu-id="e7ddb-309">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="e7ddb-309">Additional resources</span></span> 
* [<span data-ttu-id="e7ddb-310">Версия руководства на YouTube</span><span class="sxs-lookup"><span data-stu-id="e7ddb-310">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=iKlVmu-r0JQ)   

## <a name="next-steps"></a><span data-ttu-id="e7ddb-311">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="e7ddb-311">Next steps</span></span>   

<span data-ttu-id="e7ddb-312">В этом руководстве вы узнали, как:</span><span class="sxs-lookup"><span data-stu-id="e7ddb-312">In this tutorial, you learned how to:</span></span>   

> [!div class="checklist"]  
> * <span data-ttu-id="e7ddb-313">создавать проект веб-приложения;</span><span class="sxs-lookup"><span data-stu-id="e7ddb-313">Create a web app project.</span></span>   
> * <span data-ttu-id="e7ddb-314">добавлять клиентскую библиотеку SignalR;</span><span class="sxs-lookup"><span data-stu-id="e7ddb-314">Add the SignalR client library.</span></span> 
> * <span data-ttu-id="e7ddb-315">создавать концентратор SignalR;</span><span class="sxs-lookup"><span data-stu-id="e7ddb-315">Create a SignalR hub.</span></span>   
> * <span data-ttu-id="e7ddb-316">настраивать проект для использования SignalR;</span><span class="sxs-lookup"><span data-stu-id="e7ddb-316">Configure the project to use SignalR.</span></span>   
> * <span data-ttu-id="e7ddb-317">добавлять код, использующий концентратор для отправки сообщений из любого клиента всем подключенным клиентам.</span><span class="sxs-lookup"><span data-stu-id="e7ddb-317">Add code that uses the hub to send messages from any client to all connected clients.</span></span>   
<span data-ttu-id="e7ddb-318">Дополнительные сведения о SignalR см. во введении:</span><span class="sxs-lookup"><span data-stu-id="e7ddb-318">To learn more about SignalR, see the introduction:</span></span>  
> [!div class="nextstepaction"] 
> [<span data-ttu-id="e7ddb-319">Введение в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="e7ddb-319">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction) 
::: moniker-end

