---
title: Начало работы с SignalR ASP.NET Core
author: tdykstra
description: В этом руководстве создается приложение чата, которое использует SignalR для ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/13/2018
uid: tutorials/signalr
ms.openlocfilehash: 190717dc6e6f9f2766ba92aa7472f4cdea9b6827
ms.sourcegitcommit: e7fafb153b9de7595c2558a0133f8d1c33a3bddb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/28/2018
ms.locfileid: "52458534"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="d7415-103">Руководство. Начало работы с SignalR ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d7415-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="d7415-104">В этом руководстве описаны основы создания приложения в режиме реального времени с помощью SignalR.</span><span class="sxs-lookup"><span data-stu-id="d7415-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="d7415-105">Вы научитесь:</span><span class="sxs-lookup"><span data-stu-id="d7415-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d7415-106">Создайте веб-проект.</span><span class="sxs-lookup"><span data-stu-id="d7415-106">Create a web project.</span></span>
> * <span data-ttu-id="d7415-107">добавлять клиентскую библиотеку SignalR;</span><span class="sxs-lookup"><span data-stu-id="d7415-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="d7415-108">создавать концентратор SignalR;</span><span class="sxs-lookup"><span data-stu-id="d7415-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="d7415-109">настраивать проект для использования SignalR;</span><span class="sxs-lookup"><span data-stu-id="d7415-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="d7415-110">Добавлять код для отправки сообщений из любого клиента всем подключенным клиентам.</span><span class="sxs-lookup"><span data-stu-id="d7415-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="d7415-111">В итоге вы получите работающее приложение чата:</span><span class="sxs-lookup"><span data-stu-id="d7415-111">At the end, you'll have a working chat app:</span></span>

![Пример приложения SignalR](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="d7415-113">[Просмотреть или скачать пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="d7415-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

> [!NOTE]
> <span data-ttu-id="d7415-114">Мы тестируем удобство использования новой предлагаемой структуры оглавления для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d7415-114">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="d7415-115">Если у вас есть несколько минут, и вы хотите попробовать найти семь разных тем в существующей или планируемой структуре оглавления, [щелкните здесь, чтобы принять участие в исследовании](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span><span class="sxs-lookup"><span data-stu-id="d7415-115">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d7415-116">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="d7415-116">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d7415-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d7415-117">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d7415-118">[Visual Studio 2017 15.8 или более поздней версии](https://www.visualstudio.com/downloads/) с рабочей нагрузкой **ASP.NET и веб-разработка**</span><span class="sxs-lookup"><span data-stu-id="d7415-118">[Visual Studio 2017 version 15.8 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="d7415-119">Пакет SDK для .NET Core 2.1.или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="d7415-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d7415-120">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d7415-120">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="d7415-121">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d7415-121">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="d7415-122">Пакет SDK для .NET Core 2.1.или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="d7415-122">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="d7415-123">C# для Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d7415-123">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d7415-124">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="d7415-124">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="d7415-125">Visual Studio для Mac 7.5.4 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="d7415-125">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="d7415-126">[Пакет SDK для .NET Core 2.1 или более поздней версии](https://www.microsoft.com/net/download/all) (входит в установку Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="d7415-126">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>

---

## <a name="create-a-web-project"></a><span data-ttu-id="d7415-127">Создайте веб-проект.</span><span class="sxs-lookup"><span data-stu-id="d7415-127">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d7415-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d7415-128">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="d7415-129">В меню выберите **Файл > Создать проект**.</span><span class="sxs-lookup"><span data-stu-id="d7415-129">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="d7415-130">В диалоговом окне **Новый проект** выберите **Установленные > Visual C# > Веб > Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="d7415-130">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="d7415-131">Назовите проект *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="d7415-131">Name the project *SignalRChat*.</span></span>

  ![Диалоговое окно создания проекта в Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="d7415-133">Выберите **Веб-приложение**, чтобы создать проект, который использует Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="d7415-133">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="d7415-134">Выберите целевую платформу **.NET Core**, **ASP.NET Core 2.1** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="d7415-134">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Диалоговое окно создания проекта в Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d7415-136">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d7415-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="d7415-137">Откройте [интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal) и перейдите в папку, в которой будет создаваться папка нового проекта.</span><span class="sxs-lookup"><span data-stu-id="d7415-137">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="d7415-138">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="d7415-138">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d7415-139">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="d7415-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="d7415-140">В меню выберите **Файл > Создать решение**.</span><span class="sxs-lookup"><span data-stu-id="d7415-140">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="d7415-141">Выберите **.NET Core > Приложение > Веб-приложение ASP.NET Core** (не выбирайте **Веб-приложение ASP.NET Core (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="d7415-141">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="d7415-142">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="d7415-142">Select **Next**.</span></span>

* <span data-ttu-id="d7415-143">Присвойте проекту имя *SignalRChat* и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="d7415-143">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="d7415-144">Добавление клиентской библиотеки SignalR</span><span class="sxs-lookup"><span data-stu-id="d7415-144">Add the SignalR client library</span></span>

<span data-ttu-id="d7415-145">Библиотека сервера SignalR входит в состав метапакета `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="d7415-145">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="d7415-146">Клиентская библиотека JavaScript не добавляется в проект автоматически.</span><span class="sxs-lookup"><span data-stu-id="d7415-146">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="d7415-147">В рамках этого руководства вы будете использовать диспетчер библиотек (LibMan), чтобы получить клиентскую библиотеку из *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="d7415-147">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="d7415-148">unpkg — это сеть доставки содержимого, которая позволяет доставить любое содержимое из npm (диспетчера пакетов Node.js).</span><span class="sxs-lookup"><span data-stu-id="d7415-148">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d7415-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d7415-149">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="d7415-150">В **обозревателе решений** щелкните проект правой кнопкой мыши и выберите **Добавить** > **Client-Side Library** (Клиентская библиотека).</span><span class="sxs-lookup"><span data-stu-id="d7415-150">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="d7415-151">В диалоговом окне **Add Client-Side Library** (Добавить клиентскую библиотеку) для параметра **Поставщик** выберите **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="d7415-151">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="d7415-152">В поле **Библиотека** введите `@aspnet/signalr@1` и выберите последнюю версию, но не предварительную.</span><span class="sxs-lookup"><span data-stu-id="d7415-152">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Диалоговое окно добавления клиентской библиотеки — выбор библиотеки](signalr/_static/libman1.png)

* <span data-ttu-id="d7415-154">Щелкните **Choose specific files** (Выбрать определенные файлы), разверните папку *dist/browser* и выберите *signalr.js* и *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="d7415-154">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="d7415-155">В поле **Целевое расположение** укажите *wwwroot/lib/signalr/* и нажмите кнопку **Установить**.</span><span class="sxs-lookup"><span data-stu-id="d7415-155">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Диалоговое окно добавления клиентской библиотеки — выбор файлов и назначения](signalr/_static/libman2.png)

  <span data-ttu-id="d7415-157">LibMan создает папку *wwwroot/lib/signalr* и копирует в нее выбранные файлы.</span><span class="sxs-lookup"><span data-stu-id="d7415-157">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d7415-158">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d7415-158">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="d7415-159">В интегрированном терминале выполните следующую команду, чтобы установить LibMan.</span><span class="sxs-lookup"><span data-stu-id="d7415-159">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="d7415-160">Выполните приведенную ниже команду, чтобы получить клиентскую библиотеку SignalR с помощью LibMan.</span><span class="sxs-lookup"><span data-stu-id="d7415-160">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="d7415-161">Возможно, придется подождать несколько секунд, прежде чем появятся выходные данные.</span><span class="sxs-lookup"><span data-stu-id="d7415-161">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="d7415-162">В параметрах указываются следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="d7415-162">The parameters specify the following options:</span></span>
  * <span data-ttu-id="d7415-163">использование поставщика unpkg;</span><span class="sxs-lookup"><span data-stu-id="d7415-163">Use the unpkg provider.</span></span>
  * <span data-ttu-id="d7415-164">копирование файлов в назначение *wwwroot/lib/signalr*;</span><span class="sxs-lookup"><span data-stu-id="d7415-164">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="d7415-165">копирование только выбранных файлов.</span><span class="sxs-lookup"><span data-stu-id="d7415-165">Copy only the specified files.</span></span>

  <span data-ttu-id="d7415-166">Выходные данные выглядят примерно так, как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="d7415-166">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d7415-167">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="d7415-167">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="d7415-168">В **терминале** выполните следующую команду, чтобы установить LibMan.</span><span class="sxs-lookup"><span data-stu-id="d7415-168">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="d7415-169">Перейдите в папку проекта (где расположен файл *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="d7415-169">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="d7415-170">Выполните приведенную ниже команду, чтобы получить клиентскую библиотеку SignalR с помощью LibMan.</span><span class="sxs-lookup"><span data-stu-id="d7415-170">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="d7415-171">В параметрах указываются следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="d7415-171">The parameters specify the following options:</span></span>
  * <span data-ttu-id="d7415-172">использование поставщика unpkg;</span><span class="sxs-lookup"><span data-stu-id="d7415-172">Use the unpkg provider.</span></span>
  * <span data-ttu-id="d7415-173">копирование файлов в назначение *wwwroot/lib/signalr*;</span><span class="sxs-lookup"><span data-stu-id="d7415-173">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="d7415-174">копирование только выбранных файлов.</span><span class="sxs-lookup"><span data-stu-id="d7415-174">Copy only the specified files.</span></span>

  <span data-ttu-id="d7415-175">Выходные данные выглядят примерно так, как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="d7415-175">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="d7415-176">Создание концентратора SignalR</span><span class="sxs-lookup"><span data-stu-id="d7415-176">Create a SignalR hub</span></span>

<span data-ttu-id="d7415-177">*hub* — это класс, который служит в качестве конвейера высокого уровня для обработки взаимодействия между клиентом и сервером.</span><span class="sxs-lookup"><span data-stu-id="d7415-177">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="d7415-178">В папке проекта SignalRChat создайте папку *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="d7415-178">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="d7415-179">В папке *Hubs* создайте файл *ChatHub.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="d7415-179">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="d7415-180">Класс `ChatHub` наследует от класса `Hub` SignalR.</span><span class="sxs-lookup"><span data-stu-id="d7415-180">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="d7415-181">Класс `Hub` управляет подключениями, группами и обменом сообщениями.</span><span class="sxs-lookup"><span data-stu-id="d7415-181">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="d7415-182">Метод `SendMessage` может вызываться любым подключенным клиентом.</span><span class="sxs-lookup"><span data-stu-id="d7415-182">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="d7415-183">Он отправляет полученное сообщение всем клиентам.</span><span class="sxs-lookup"><span data-stu-id="d7415-183">It sends the received message to all clients.</span></span> <span data-ttu-id="d7415-184">Код SignalR является асинхронным, поэтому обеспечивает максимальную масштабируемость.</span><span class="sxs-lookup"><span data-stu-id="d7415-184">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="d7415-185">Настройка SignalR</span><span class="sxs-lookup"><span data-stu-id="d7415-185">Configure SignalR</span></span>

<span data-ttu-id="d7415-186">Сервер SignalR необходимо настроить таким образом, чтобы он передавал запросы SignalR в SignalR.</span><span class="sxs-lookup"><span data-stu-id="d7415-186">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="d7415-187">Добавьте следующий выделенный код в файл *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d7415-187">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="d7415-188">В результате SignalR будет добавлен в систему внедрения зависимостей ASP.NET Core и конвейер ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="d7415-188">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="d7415-189">Добавление кода клиента SignalR</span><span class="sxs-lookup"><span data-stu-id="d7415-189">Add SignalR client code</span></span>

* <span data-ttu-id="d7415-190">Замените содержимое в файле *Pages\Index.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="d7415-190">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="d7415-191">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="d7415-191">The preceding code:</span></span>

  * <span data-ttu-id="d7415-192">Создает текстовые поля для имени и текста сообщения и кнопку отправки.</span><span class="sxs-lookup"><span data-stu-id="d7415-192">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="d7415-193">Создает список с `id="messagesList"` для отображения сообщений, полученных от концентратора SignalR.</span><span class="sxs-lookup"><span data-stu-id="d7415-193">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="d7415-194">Содержит ссылки на скрипты для SignalR и код приложения *chat.js*, который создается в следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="d7415-194">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="d7415-195">В папке *wwwroot/js* создайте файл *chat.js* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="d7415-195">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="d7415-196">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="d7415-196">The preceding code:</span></span>

  * <span data-ttu-id="d7415-197">Создает и запускает подключение.</span><span class="sxs-lookup"><span data-stu-id="d7415-197">Creates and starts a connection.</span></span>
  * <span data-ttu-id="d7415-198">Добавляет к кнопке отправки обработчик, который отправляет сообщения в концентратор.</span><span class="sxs-lookup"><span data-stu-id="d7415-198">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="d7415-199">Добавляет к объекту подключения обработчик, который получает сообщения из концентратора и добавляет их в список.</span><span class="sxs-lookup"><span data-stu-id="d7415-199">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="d7415-200">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="d7415-200">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d7415-201">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d7415-201">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d7415-202">Нажмите клавиши **CTRL+F5**, чтобы запустить приложение без отладки.</span><span class="sxs-lookup"><span data-stu-id="d7415-202">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d7415-203">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d7415-203">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="d7415-204">В интегрированном терминале выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="d7415-204">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d7415-205">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="d7415-205">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="d7415-206">В меню выберите **Запуск > Запуск без отладки**.</span><span class="sxs-lookup"><span data-stu-id="d7415-206">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="d7415-207">Скопируйте URL-адрес из адресной строки, откройте другой экземпляр или вкладку браузера и вставьте URL-адрес в адресную строку.</span><span class="sxs-lookup"><span data-stu-id="d7415-207">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="d7415-208">Выберите любой браузер, введите имя и сообщение и нажмите кнопку **Отправить сообщение**.</span><span class="sxs-lookup"><span data-stu-id="d7415-208">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="d7415-209">Имя и сообщение отображаются на обеих страницах мгновенно.</span><span class="sxs-lookup"><span data-stu-id="d7415-209">The name and message are displayed on both pages instantly.</span></span>

  ![Пример приложения SignalR](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="d7415-211">Если приложение не работает, откройте средства разработчика для браузера (F12) и перейдите в консоль.</span><span class="sxs-lookup"><span data-stu-id="d7415-211">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="d7415-212">Вы можете увидеть ошибки, связанные с вашим кодом HTML и JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d7415-212">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="d7415-213">Предположим, вы поместили *signalr.js* не в ту папку, которую указали.</span><span class="sxs-lookup"><span data-stu-id="d7415-213">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="d7415-214">В этом случае ссылка на этот файл не будет работать, и вы увидите сообщение об ошибке 404 в консоли.</span><span class="sxs-lookup"><span data-stu-id="d7415-214">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="d7415-215">![ошибка: signalr.js не найден](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="d7415-215">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="d7415-216">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="d7415-216">Next steps</span></span>

<span data-ttu-id="d7415-217">В этом руководстве вы узнали, как:</span><span class="sxs-lookup"><span data-stu-id="d7415-217">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d7415-218">создавать проект веб-приложения;</span><span class="sxs-lookup"><span data-stu-id="d7415-218">Create a web app project.</span></span>
> * <span data-ttu-id="d7415-219">добавлять клиентскую библиотеку SignalR;</span><span class="sxs-lookup"><span data-stu-id="d7415-219">Add the SignalR client library.</span></span>
> * <span data-ttu-id="d7415-220">создавать концентратор SignalR;</span><span class="sxs-lookup"><span data-stu-id="d7415-220">Create a SignalR hub.</span></span>
> * <span data-ttu-id="d7415-221">настраивать проект для использования SignalR;</span><span class="sxs-lookup"><span data-stu-id="d7415-221">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="d7415-222">добавлять код, использующий концентратор для отправки сообщений из любого клиента всем подключенным клиентам.</span><span class="sxs-lookup"><span data-stu-id="d7415-222">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="d7415-223">Дополнительные сведения о SignalR см. во введении:</span><span class="sxs-lookup"><span data-stu-id="d7415-223">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d7415-224">Введение в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="d7415-224">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
