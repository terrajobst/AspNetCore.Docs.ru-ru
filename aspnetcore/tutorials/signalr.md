---
title: Начало работы с SignalR ASP.NET Core
author: bradygaster
description: В этом руководстве создается приложение чата, которое использует SignalR для ASP.NET Core.
ms.author: bradyg
ms.custom: mvc
ms.date: 11/30/2018
uid: tutorials/signalr
ms.openlocfilehash: 9a4296550a17ac2c348f2406e9f5b39877b02b59
ms.sourcegitcommit: d6e51c60439f03a8992bda70cc982ddb15d3f100
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555930"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="1b7a3-103">Учебник. Начало работы с SignalR ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1b7a3-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="1b7a3-104">В этом руководстве описаны основы создания приложения в режиме реального времени с помощью SignalR.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="1b7a3-105">Вы научитесь:</span><span class="sxs-lookup"><span data-stu-id="1b7a3-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1b7a3-106">Создайте веб-проект.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-106">Create a web project.</span></span>
> * <span data-ttu-id="1b7a3-107">добавлять клиентскую библиотеку SignalR;</span><span class="sxs-lookup"><span data-stu-id="1b7a3-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="1b7a3-108">создавать концентратор SignalR;</span><span class="sxs-lookup"><span data-stu-id="1b7a3-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="1b7a3-109">настраивать проект для использования SignalR;</span><span class="sxs-lookup"><span data-stu-id="1b7a3-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="1b7a3-110">Добавлять код для отправки сообщений из любого клиента всем подключенным клиентам.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="1b7a3-111">В итоге вы получите работающее приложение чата:</span><span class="sxs-lookup"><span data-stu-id="1b7a3-111">At the end, you'll have a working chat app:</span></span>

![Пример приложения SignalR](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="1b7a3-113">[Просмотреть или скачать пример кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="1b7a3-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1b7a3-114">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="1b7a3-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1b7a3-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1b7a3-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2017-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1b7a3-116">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1b7a3-117">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="1b7a3-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="1b7a3-118">Создайте веб-проект.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-118">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1b7a3-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1b7a3-119">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="1b7a3-120">В меню выберите **Файл > Создать проект**.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-120">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="1b7a3-121">В диалоговом окне **Новый проект** выберите **Установленные > Visual C# > Веб > Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-121">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="1b7a3-122">Назовите проект *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-122">Name the project *SignalRChat*.</span></span>

  ![Диалоговое окно создания проекта в Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="1b7a3-124">Выберите **Веб-приложение**, чтобы создать проект, который использует Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-124">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="1b7a3-125">Выберите целевую платформу **.NET Core**, **ASP.NET Core 2.2** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-125">Select a target framework of **.NET Core**, select **ASP.NET Core 2.2**, and click **OK**.</span></span>

  ![Диалоговое окно создания проекта в Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1b7a3-127">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-127">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="1b7a3-128">Откройте [интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal) и перейдите в папку, в которой будет создаваться папка нового проекта.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-128">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="1b7a3-129">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="1b7a3-129">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1b7a3-130">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="1b7a3-130">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="1b7a3-131">В меню выберите **Файл > Создать решение**.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-131">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="1b7a3-132">Выберите **.NET Core > Приложение > Веб-приложение ASP.NET Core** (не выбирайте **Веб-приложение ASP.NET Core (MVC)** ).</span><span class="sxs-lookup"><span data-stu-id="1b7a3-132">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="1b7a3-133">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-133">Select **Next**.</span></span>

* <span data-ttu-id="1b7a3-134">Присвойте проекту имя *SignalRChat* и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-134">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="1b7a3-135">Добавление клиентской библиотеки SignalR</span><span class="sxs-lookup"><span data-stu-id="1b7a3-135">Add the SignalR client library</span></span>

<span data-ttu-id="1b7a3-136">Библиотека сервера SignalR входит в состав метапакета `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-136">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="1b7a3-137">Клиентская библиотека JavaScript не добавляется в проект автоматически.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-137">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="1b7a3-138">В рамках этого руководства вы будете использовать диспетчер библиотек (LibMan), чтобы получить клиентскую библиотеку из *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-138">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="1b7a3-139">unpkg — это сеть доставки содержимого, которая позволяет доставить любое содержимое из npm (диспетчера пакетов Node.js).</span><span class="sxs-lookup"><span data-stu-id="1b7a3-139">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1b7a3-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1b7a3-140">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="1b7a3-141">В **обозревателе решений** щелкните проект правой кнопкой мыши и выберите **Добавить** > **Client-Side Library** (Клиентская библиотека).</span><span class="sxs-lookup"><span data-stu-id="1b7a3-141">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="1b7a3-142">В диалоговом окне **Add Client-Side Library** (Добавить клиентскую библиотеку) для параметра **Поставщик** выберите **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-142">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="1b7a3-143">В поле **Библиотека** введите `@aspnet/signalr@1` и выберите последнюю версию, но не предварительную.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-143">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Диалоговое окно добавления клиентской библиотеки — выбор библиотеки](signalr/_static/libman1.png)

* <span data-ttu-id="1b7a3-145">Щелкните **Choose specific files** (Выбрать определенные файлы), разверните папку *dist/browser* и выберите *signalr.js* и *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-145">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="1b7a3-146">В поле **Целевое расположение** укажите *wwwroot/lib/signalr/* и нажмите кнопку **Установить**.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-146">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Диалоговое окно добавления клиентской библиотеки — выбор файлов и назначения](signalr/_static/libman2.png)

  <span data-ttu-id="1b7a3-148">LibMan создает папку *wwwroot/lib/signalr* и копирует в нее выбранные файлы.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-148">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1b7a3-149">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-149">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="1b7a3-150">В интегрированном терминале выполните следующую команду, чтобы установить LibMan.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-150">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="1b7a3-151">Выполните приведенную ниже команду, чтобы получить клиентскую библиотеку SignalR с помощью LibMan.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-151">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="1b7a3-152">Возможно, придется подождать несколько секунд, прежде чем появятся выходные данные.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-152">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="1b7a3-153">В параметрах указываются следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="1b7a3-153">The parameters specify the following options:</span></span>
  * <span data-ttu-id="1b7a3-154">использование поставщика unpkg;</span><span class="sxs-lookup"><span data-stu-id="1b7a3-154">Use the unpkg provider.</span></span>
  * <span data-ttu-id="1b7a3-155">копирование файлов в назначение *wwwroot/lib/signalr*;</span><span class="sxs-lookup"><span data-stu-id="1b7a3-155">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="1b7a3-156">копирование только выбранных файлов.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-156">Copy only the specified files.</span></span>

  <span data-ttu-id="1b7a3-157">Выходные данные выглядят примерно так, как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="1b7a3-157">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1b7a3-158">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="1b7a3-158">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="1b7a3-159">В **терминале** выполните следующую команду, чтобы установить LibMan.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-159">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="1b7a3-160">Перейдите в папку проекта (где расположен файл *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="1b7a3-160">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="1b7a3-161">Выполните приведенную ниже команду, чтобы получить клиентскую библиотеку SignalR с помощью LibMan.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-161">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="1b7a3-162">В параметрах указываются следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="1b7a3-162">The parameters specify the following options:</span></span>
  * <span data-ttu-id="1b7a3-163">использование поставщика unpkg;</span><span class="sxs-lookup"><span data-stu-id="1b7a3-163">Use the unpkg provider.</span></span>
  * <span data-ttu-id="1b7a3-164">копирование файлов в назначение *wwwroot/lib/signalr*;</span><span class="sxs-lookup"><span data-stu-id="1b7a3-164">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="1b7a3-165">копирование только выбранных файлов.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-165">Copy only the specified files.</span></span>

  <span data-ttu-id="1b7a3-166">Выходные данные выглядят примерно так, как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="1b7a3-166">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="1b7a3-167">Создание концентратора SignalR</span><span class="sxs-lookup"><span data-stu-id="1b7a3-167">Create a SignalR hub</span></span>

<span data-ttu-id="1b7a3-168">*hub* — это класс, который служит в качестве конвейера высокого уровня для обработки взаимодействия между клиентом и сервером.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-168">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="1b7a3-169">В папке проекта SignalRChat создайте папку *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-169">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="1b7a3-170">В папке *Hubs* создайте файл *ChatHub.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="1b7a3-170">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="1b7a3-171">Класс `ChatHub` наследует от класса `Hub` SignalR.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-171">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="1b7a3-172">Класс `Hub` управляет подключениями, группами и обменом сообщениями.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-172">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="1b7a3-173">Метод `SendMessage` может вызываться подключенным клиентом, чтобы отправить сообщение всем клиентам.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-173">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="1b7a3-174">Далее в этом учебника показан клиентский код JavaScript, который вызывает метод.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-174">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="1b7a3-175">Код SignalR является асинхронным, поэтому обеспечивает максимальную масштабируемость.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-175">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="1b7a3-176">Настройка SignalR</span><span class="sxs-lookup"><span data-stu-id="1b7a3-176">Configure SignalR</span></span>

<span data-ttu-id="1b7a3-177">Сервер SignalR необходимо настроить таким образом, чтобы он передавал запросы SignalR в SignalR.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-177">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="1b7a3-178">Добавьте следующий выделенный код в файл *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-178">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="1b7a3-179">В результате SignalR будет добавлен в систему внедрения зависимостей ASP.NET Core и конвейер ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-179">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="1b7a3-180">Добавление кода клиента SignalR</span><span class="sxs-lookup"><span data-stu-id="1b7a3-180">Add SignalR client code</span></span>

* <span data-ttu-id="1b7a3-181">Замените содержимое в файле *Pages\Index.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="1b7a3-181">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="1b7a3-182">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="1b7a3-182">The preceding code:</span></span>

  * <span data-ttu-id="1b7a3-183">Создает текстовые поля для имени и текста сообщения и кнопку отправки.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-183">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="1b7a3-184">Создает список с `id="messagesList"` для отображения сообщений, полученных от концентратора SignalR.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-184">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="1b7a3-185">Содержит ссылки на скрипты для SignalR и код приложения *chat.js*, который создается в следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-185">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="1b7a3-186">В папке *wwwroot/js* создайте файл *chat.js* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="1b7a3-186">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="1b7a3-187">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="1b7a3-187">The preceding code:</span></span>

  * <span data-ttu-id="1b7a3-188">Создает и запускает подключение.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-188">Creates and starts a connection.</span></span>
  * <span data-ttu-id="1b7a3-189">Добавляет к кнопке отправки обработчик, который отправляет сообщения в концентратор.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-189">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="1b7a3-190">Добавляет к объекту подключения обработчик, который получает сообщения из концентратора и добавляет их в список.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-190">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="1b7a3-191">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="1b7a3-191">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1b7a3-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1b7a3-192">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1b7a3-193">Нажмите клавиши **CTRL+F5**, чтобы запустить приложение без отладки.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-193">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1b7a3-194">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-194">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="1b7a3-195">В интегрированном терминале выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="1b7a3-195">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1b7a3-196">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="1b7a3-196">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="1b7a3-197">В меню выберите **Запуск > Запуск без отладки**.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-197">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="1b7a3-198">Скопируйте URL-адрес из адресной строки, откройте другой экземпляр или вкладку браузера и вставьте URL-адрес в адресную строку.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-198">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="1b7a3-199">Выберите любой браузер, введите имя и сообщение и нажмите кнопку **Отправить сообщение**.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-199">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="1b7a3-200">Имя и сообщение отображаются на обеих страницах мгновенно.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-200">The name and message are displayed on both pages instantly.</span></span>

  ![Пример приложения SignalR](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="1b7a3-202">Если приложение не работает, откройте средства разработчика для браузера (F12) и перейдите в консоль.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-202">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="1b7a3-203">Вы можете увидеть ошибки, связанные с вашим кодом HTML и JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-203">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="1b7a3-204">Предположим, вы поместили *signalr.js* не в ту папку, которую указали.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-204">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="1b7a3-205">В этом случае ссылка на этот файл не будет работать, и вы увидите сообщение об ошибке 404 в консоли.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-205">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="1b7a3-206">![ошибка: signalr.js не найден](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="1b7a3-206">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="1b7a3-207">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="1b7a3-207">Next steps</span></span>

<span data-ttu-id="1b7a3-208">В этом руководстве вы узнали, как:</span><span class="sxs-lookup"><span data-stu-id="1b7a3-208">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1b7a3-209">создавать проект веб-приложения;</span><span class="sxs-lookup"><span data-stu-id="1b7a3-209">Create a web app project.</span></span>
> * <span data-ttu-id="1b7a3-210">добавлять клиентскую библиотеку SignalR;</span><span class="sxs-lookup"><span data-stu-id="1b7a3-210">Add the SignalR client library.</span></span>
> * <span data-ttu-id="1b7a3-211">создавать концентратор SignalR;</span><span class="sxs-lookup"><span data-stu-id="1b7a3-211">Create a SignalR hub.</span></span>
> * <span data-ttu-id="1b7a3-212">настраивать проект для использования SignalR;</span><span class="sxs-lookup"><span data-stu-id="1b7a3-212">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="1b7a3-213">добавлять код, использующий концентратор для отправки сообщений из любого клиента всем подключенным клиентам.</span><span class="sxs-lookup"><span data-stu-id="1b7a3-213">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="1b7a3-214">Дополнительные сведения о SignalR см. во введении:</span><span class="sxs-lookup"><span data-stu-id="1b7a3-214">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1b7a3-215">Введение в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="1b7a3-215">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
