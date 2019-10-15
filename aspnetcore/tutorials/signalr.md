---
title: Начало работы с SignalR ASP.NET Core
author: bradygaster
description: В этом руководстве создается приложение чата, которое использует SignalR для ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: bradyg
ms.custom: mvc
ms.date: 10/03/2019
uid: tutorials/signalr
ms.openlocfilehash: 843416cf00c9241f8c05b1aba041ebbe7a05cf80
ms.sourcegitcommit: 73a451e9a58ac7102f90b608d661d8c23dd9bbaf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/08/2019
ms.locfileid: "72037679"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="eda6b-103">Учебник. Начало работы с SignalR ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eda6b-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="eda6b-104">В этом руководстве описаны основы создания приложения в режиме реального времени с помощью SignalR.</span><span class="sxs-lookup"><span data-stu-id="eda6b-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="eda6b-105">Вы научитесь:</span><span class="sxs-lookup"><span data-stu-id="eda6b-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="eda6b-106">Создайте веб-проект.</span><span class="sxs-lookup"><span data-stu-id="eda6b-106">Create a web project.</span></span>
> * <span data-ttu-id="eda6b-107">добавлять клиентскую библиотеку SignalR;</span><span class="sxs-lookup"><span data-stu-id="eda6b-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="eda6b-108">создавать концентратор SignalR;</span><span class="sxs-lookup"><span data-stu-id="eda6b-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="eda6b-109">настраивать проект для использования SignalR;</span><span class="sxs-lookup"><span data-stu-id="eda6b-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="eda6b-110">Добавлять код для отправки сообщений из любого клиента всем подключенным клиентам.</span><span class="sxs-lookup"><span data-stu-id="eda6b-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="eda6b-111">В итоге вы получите работающее приложение чата:</span><span class="sxs-lookup"><span data-stu-id="eda6b-111">At the end, you'll have a working chat app:</span></span>

![Пример приложения SignalR](signalr/_static/3.x/signalr-get-started-finished.png)

## <a name="prerequisites"></a><span data-ttu-id="eda6b-113">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="eda6b-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="eda6b-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eda6b-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="eda6b-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="eda6b-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="eda6b-116">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="eda6b-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app-project"></a><span data-ttu-id="eda6b-117">Создание проекта веб-приложения</span><span class="sxs-lookup"><span data-stu-id="eda6b-117">Create a web app project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="eda6b-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eda6b-118">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="eda6b-119">В меню выберите **Файл > Создать проект**.</span><span class="sxs-lookup"><span data-stu-id="eda6b-119">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="eda6b-120">В диалоговом окне **Создать проект** выберите **Веб-приложение ASP.NET Core** и нажмите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="eda6b-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**, and then select **Next**.</span></span>

* <span data-ttu-id="eda6b-121">В диалоговом окне **Настроить новый проект** укажите имя проекта *SignalRChat*, а затем выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="eda6b-121">In the **Configure your new project** dialog, name the project *SignalRChat*, and then select **Create**.</span></span>

* <span data-ttu-id="eda6b-122">В диалоговом окне **Создание веб-приложения ASP.NET Core** выберите платформы **.NET Core** и **ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="eda6b-122">In the **Create a new ASP.NET Core web Application** dialog, select **.NET Core** and **ASP.NET Core 3.0**.</span></span> 

* <span data-ttu-id="eda6b-123">Выберите **Веб-приложение**, чтобы создать проект, который использует Razor Pages, и нажмите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="eda6b-123">Select **Web Application** to create a project that uses Razor Pages, and then select **Create**.</span></span>

  ![Диалоговое окно создания проекта в Visual Studio](signalr/_static/3.x/signalr-new-project-dialog.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="eda6b-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="eda6b-125">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="eda6b-126">Откройте [интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal) и перейдите в папку, в которой будет создаваться папка нового проекта.</span><span class="sxs-lookup"><span data-stu-id="eda6b-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="eda6b-127">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="eda6b-127">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="eda6b-128">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="eda6b-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="eda6b-129">В меню выберите **Файл > Создать решение**.</span><span class="sxs-lookup"><span data-stu-id="eda6b-129">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="eda6b-130">Выберите **.NET Core > Приложение > Веб-приложение** (не выбирайте **Веб-приложение (модель — представление — контроллер)** ), а затем выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="eda6b-130">Select **.NET Core > App > Web Application** (Don't select **Web Application (Model-View-Controller)**), and then select **Next**.</span></span>

* <span data-ttu-id="eda6b-131">Убедитесь, что для параметра **Требуемая версия .NET Framework** указано **.NET Core 3.0**, а затем выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="eda6b-131">Make sure the **Target Framework** is set to **.NET Core 3.0**, and then select **Next**.</span></span>

* <span data-ttu-id="eda6b-132">Присвойте проекту имя *SignalRChat* и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="eda6b-132">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="eda6b-133">Добавление клиентской библиотеки SignalR</span><span class="sxs-lookup"><span data-stu-id="eda6b-133">Add the SignalR client library</span></span>

<span data-ttu-id="eda6b-134">Библиотека сервера SignalR входит в состав общей платформы ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="eda6b-134">The SignalR server library is included in the ASP.NET Core 3.0 shared framework.</span></span> <span data-ttu-id="eda6b-135">Клиентская библиотека JavaScript не добавляется в проект автоматически.</span><span class="sxs-lookup"><span data-stu-id="eda6b-135">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="eda6b-136">В рамках этого руководства вы будете использовать диспетчер библиотек (LibMan), чтобы получить клиентскую библиотеку из *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="eda6b-136">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="eda6b-137">unpkg — это сеть доставки содержимого, которая позволяет доставить любое содержимое из npm (диспетчера пакетов Node.js).</span><span class="sxs-lookup"><span data-stu-id="eda6b-137">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="eda6b-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eda6b-138">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="eda6b-139">В **обозревателе решений** щелкните проект правой кнопкой мыши и выберите **Добавить** > **Client-Side Library** (Клиентская библиотека).</span><span class="sxs-lookup"><span data-stu-id="eda6b-139">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="eda6b-140">В диалоговом окне **Add Client-Side Library** (Добавить клиентскую библиотеку) для параметра **Поставщик** выберите **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="eda6b-140">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="eda6b-141">Для параметра **Библиотека** введите `@microsoft/signalr@latest`.</span><span class="sxs-lookup"><span data-stu-id="eda6b-141">For **Library**, enter `@microsoft/signalr@latest`.</span></span>

* <span data-ttu-id="eda6b-142">Щелкните **Choose specific files** (Выбрать определенные файлы), разверните папку *dist/browser* и выберите *signalr.js* и *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="eda6b-142">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="eda6b-143">В поле **Целевое расположение** укажите *wwwroot/js/signalr/* и нажмите кнопку **Установить**.</span><span class="sxs-lookup"><span data-stu-id="eda6b-143">Set **Target Location** to *wwwroot/js/signalr/*, and select **Install**.</span></span>

  ![Диалоговое окно добавления клиентской библиотеки — выбор библиотеки](signalr/_static/3.x/find-signalr-client-libs-select-files.png)

  <span data-ttu-id="eda6b-145">LibMan создает папку *wwwroot/js/signalr* и копирует в нее выбранные файлы.</span><span class="sxs-lookup"><span data-stu-id="eda6b-145">LibMan creates a *wwwroot/js/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="eda6b-146">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="eda6b-146">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="eda6b-147">В интегрированном терминале выполните следующую команду, чтобы установить LibMan.</span><span class="sxs-lookup"><span data-stu-id="eda6b-147">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="eda6b-148">Выполните приведенную ниже команду, чтобы получить клиентскую библиотеку SignalR с помощью LibMan.</span><span class="sxs-lookup"><span data-stu-id="eda6b-148">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="eda6b-149">Возможно, придется подождать несколько секунд, прежде чем появятся выходные данные.</span><span class="sxs-lookup"><span data-stu-id="eda6b-149">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @microsoft/signalr@latest -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="eda6b-150">В параметрах указываются следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="eda6b-150">The parameters specify the following options:</span></span>
  * <span data-ttu-id="eda6b-151">использование поставщика unpkg;</span><span class="sxs-lookup"><span data-stu-id="eda6b-151">Use the unpkg provider.</span></span>
  * <span data-ttu-id="eda6b-152">копирование файлов в назначение *wwwroot/js/signalr*;</span><span class="sxs-lookup"><span data-stu-id="eda6b-152">Copy files to the *wwwroot/js/signalr* destination.</span></span>
  * <span data-ttu-id="eda6b-153">копирование только выбранных файлов.</span><span class="sxs-lookup"><span data-stu-id="eda6b-153">Copy only the specified files.</span></span>

  <span data-ttu-id="eda6b-154">Выходные данные выглядят примерно так, как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="eda6b-154">The output looks like the following example:</span></span>

  ```console
  wwwroot/js/signalr/dist/browser/signalr.js written to disk
  wwwroot/js/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@microsoft/signalr@latest" to "wwwroot/js/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="eda6b-155">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="eda6b-155">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="eda6b-156">В **терминале** выполните следующую команду, чтобы установить LibMan.</span><span class="sxs-lookup"><span data-stu-id="eda6b-156">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="eda6b-157">Перейдите в папку проекта (где расположен файл *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="eda6b-157">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="eda6b-158">Выполните приведенную ниже команду, чтобы получить клиентскую библиотеку SignalR с помощью LibMan.</span><span class="sxs-lookup"><span data-stu-id="eda6b-158">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @microsoft/signalr@latest -p unpkg -d wwwroot/js/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="eda6b-159">В параметрах указываются следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="eda6b-159">The parameters specify the following options:</span></span>
  * <span data-ttu-id="eda6b-160">использование поставщика unpkg;</span><span class="sxs-lookup"><span data-stu-id="eda6b-160">Use the unpkg provider.</span></span>
  * <span data-ttu-id="eda6b-161">копирование файлов в назначение *wwwroot/js/signalr*;</span><span class="sxs-lookup"><span data-stu-id="eda6b-161">Copy files to the *wwwroot/js/signalr* destination.</span></span>
  * <span data-ttu-id="eda6b-162">копирование только выбранных файлов.</span><span class="sxs-lookup"><span data-stu-id="eda6b-162">Copy only the specified files.</span></span>

  <span data-ttu-id="eda6b-163">Выходные данные выглядят примерно так, как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="eda6b-163">The output looks like the following example:</span></span>

  ```console
  wwwroot/js/signalr/dist/browser/signalr.js written to disk
  wwwroot/js/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@microsoft/signalr@latest" to "wwwroot/js/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="eda6b-164">Создание концентратора SignalR</span><span class="sxs-lookup"><span data-stu-id="eda6b-164">Create a SignalR hub</span></span>

<span data-ttu-id="eda6b-165">*hub* — это класс, который служит в качестве конвейера высокого уровня для обработки взаимодействия между клиентом и сервером.</span><span class="sxs-lookup"><span data-stu-id="eda6b-165">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="eda6b-166">В папке проекта SignalRChat создайте папку *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="eda6b-166">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="eda6b-167">В папке *Hubs* создайте файл *ChatHub.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="eda6b-167">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[ChatHub](signalr/sample-snapshot/3.x/ChatHub.cs)]

  <span data-ttu-id="eda6b-168">Класс `ChatHub` наследует от класса `Hub` SignalR.</span><span class="sxs-lookup"><span data-stu-id="eda6b-168">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="eda6b-169">Класс `Hub` управляет подключениями, группами и обменом сообщениями.</span><span class="sxs-lookup"><span data-stu-id="eda6b-169">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="eda6b-170">Метод `SendMessage` может вызываться подключенным клиентом, чтобы отправить сообщение всем клиентам.</span><span class="sxs-lookup"><span data-stu-id="eda6b-170">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="eda6b-171">Далее в этом учебника показан клиентский код JavaScript, который вызывает метод.</span><span class="sxs-lookup"><span data-stu-id="eda6b-171">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="eda6b-172">Код SignalR является асинхронным, поэтому обеспечивает максимальную масштабируемость.</span><span class="sxs-lookup"><span data-stu-id="eda6b-172">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="eda6b-173">Настройка SignalR</span><span class="sxs-lookup"><span data-stu-id="eda6b-173">Configure SignalR</span></span>

<span data-ttu-id="eda6b-174">Сервер SignalR необходимо настроить таким образом, чтобы он передавал запросы SignalR в SignalR.</span><span class="sxs-lookup"><span data-stu-id="eda6b-174">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="eda6b-175">Добавьте следующий выделенный код в файл *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="eda6b-175">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/3.x/Startup.cs?highlight=11,28,55)]

  <span data-ttu-id="eda6b-176">В результате SignalR будет добавлен в системы внедрения зависимостей и маршрутизации ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eda6b-176">These changes add SignalR to the ASP.NET Core dependency injection and routing systems.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="eda6b-177">Добавление кода клиента SignalR</span><span class="sxs-lookup"><span data-stu-id="eda6b-177">Add SignalR client code</span></span>

* <span data-ttu-id="eda6b-178">Замените содержимое в файле *Pages\Index.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="eda6b-178">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample-snapshot/3.x/Index.cshtml)]

  <span data-ttu-id="eda6b-179">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="eda6b-179">The preceding code:</span></span>

  * <span data-ttu-id="eda6b-180">Создает текстовые поля для имени и текста сообщения и кнопку отправки.</span><span class="sxs-lookup"><span data-stu-id="eda6b-180">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="eda6b-181">Создает список с `id="messagesList"` для отображения сообщений, полученных от концентратора SignalR.</span><span class="sxs-lookup"><span data-stu-id="eda6b-181">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="eda6b-182">Содержит ссылки на скрипты для SignalR и код приложения *chat.js*, который создается в следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="eda6b-182">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="eda6b-183">В папке *wwwroot/js* создайте файл *chat.js* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="eda6b-183">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[chat](signalr/sample-snapshot/3.x/chat.js)]

  <span data-ttu-id="eda6b-184">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="eda6b-184">The preceding code:</span></span>

  * <span data-ttu-id="eda6b-185">Создает и запускает подключение.</span><span class="sxs-lookup"><span data-stu-id="eda6b-185">Creates and starts a connection.</span></span>
  * <span data-ttu-id="eda6b-186">Добавляет к кнопке отправки обработчик, который отправляет сообщения в концентратор.</span><span class="sxs-lookup"><span data-stu-id="eda6b-186">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="eda6b-187">Добавляет к объекту подключения обработчик, который получает сообщения из концентратора и добавляет их в список.</span><span class="sxs-lookup"><span data-stu-id="eda6b-187">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="eda6b-188">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="eda6b-188">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="eda6b-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eda6b-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="eda6b-190">Нажмите клавиши **CTRL+F5**, чтобы запустить приложение без отладки.</span><span class="sxs-lookup"><span data-stu-id="eda6b-190">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="eda6b-191">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="eda6b-191">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="eda6b-192">В интегрированном терминале выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="eda6b-192">In the integrated terminal, run the following command:</span></span>

  ```dotnetcli
  dotnet watch run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="eda6b-193">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="eda6b-193">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="eda6b-194">В меню выберите **Запуск > Запуск без отладки**.</span><span class="sxs-lookup"><span data-stu-id="eda6b-194">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="eda6b-195">Скопируйте URL-адрес из адресной строки, откройте другой экземпляр или вкладку браузера и вставьте URL-адрес в адресную строку.</span><span class="sxs-lookup"><span data-stu-id="eda6b-195">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="eda6b-196">Выберите любой браузер, введите имя и сообщение и нажмите кнопку **Отправить сообщение**.</span><span class="sxs-lookup"><span data-stu-id="eda6b-196">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="eda6b-197">Имя и сообщение отображаются на обеих страницах мгновенно.</span><span class="sxs-lookup"><span data-stu-id="eda6b-197">The name and message are displayed on both pages instantly.</span></span>

  ![Пример приложения SignalR](signalr/_static/3.x/signalr-get-started-finished.png)

> [!TIP]
> * <span data-ttu-id="eda6b-199">Если приложение не работает, откройте средства разработчика для браузера (F12) и перейдите в консоль.</span><span class="sxs-lookup"><span data-stu-id="eda6b-199">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="eda6b-200">Вы можете увидеть ошибки, связанные с вашим кодом HTML и JavaScript.</span><span class="sxs-lookup"><span data-stu-id="eda6b-200">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="eda6b-201">Предположим, вы поместили *signalr.js* не в ту папку, которую указали.</span><span class="sxs-lookup"><span data-stu-id="eda6b-201">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="eda6b-202">В этом случае ссылка на этот файл не будет работать, и вы увидите сообщение об ошибке 404 в консоли.</span><span class="sxs-lookup"><span data-stu-id="eda6b-202">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
>   <span data-ttu-id="eda6b-203">![ошибка: signalr.js не найден](signalr/_static/3.x/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="eda6b-203">![signalr.js not found error](signalr/_static/3.x/f12-console.png)</span></span>
> * <span data-ttu-id="eda6b-204">Если возникает ошибка ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY в Chrome, выполните эти команды, чтобы обновить сертификат разработки:</span><span class="sxs-lookup"><span data-stu-id="eda6b-204">If you get the error ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY in Chrome, run these commands to update your development certificate:</span></span>
>
>   ```dotnetcli
>   dotnet dev-certs https --clean
>   dotnet dev-certs https --trust
>   ```

## <a name="next-steps"></a><span data-ttu-id="eda6b-205">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="eda6b-205">Next steps</span></span>

<span data-ttu-id="eda6b-206">Дополнительные сведения о SignalR см. во введении:</span><span class="sxs-lookup"><span data-stu-id="eda6b-206">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="eda6b-207">Введение в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="eda6b-207">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
