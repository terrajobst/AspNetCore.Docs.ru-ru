---
title: Начало работы с SignalR ASP.NET Core
author: tdykstra
description: В этом руководстве создается приложение чата, которое использует SignalR для ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: tutorials/signalr
ms.openlocfilehash: 7d9f3a3f8aa7a5e47169da66e6fa2d6a28de3853
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021252"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="5f862-103">Руководство. Начало работы с SignalR ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5f862-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="5f862-104">В этом руководстве описаны основы создания приложения в режиме реального времени с помощью SignalR.</span><span class="sxs-lookup"><span data-stu-id="5f862-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="5f862-105">Вы научитесь:</span><span class="sxs-lookup"><span data-stu-id="5f862-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5f862-106">Создайте веб-проект.</span><span class="sxs-lookup"><span data-stu-id="5f862-106">Create a web project.</span></span>
> * <span data-ttu-id="5f862-107">добавлять клиентскую библиотеку SignalR;</span><span class="sxs-lookup"><span data-stu-id="5f862-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="5f862-108">создавать концентратор SignalR;</span><span class="sxs-lookup"><span data-stu-id="5f862-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="5f862-109">настраивать проект для использования SignalR;</span><span class="sxs-lookup"><span data-stu-id="5f862-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="5f862-110">Добавлять код для отправки сообщений из любого клиента всем подключенным клиентам.</span><span class="sxs-lookup"><span data-stu-id="5f862-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="5f862-111">В итоге вы получите работающее приложение чата:</span><span class="sxs-lookup"><span data-stu-id="5f862-111">At the end, you'll have a working chat app:</span></span>

![Пример приложения SignalR](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="5f862-113">[Просмотреть или скачать пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="5f862-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5f862-114">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="5f862-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5f862-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5f862-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5f862-116">[Visual Studio 2017 15.8 или более поздней версии](https://www.visualstudio.com/downloads/) с рабочей нагрузкой **ASP.NET и веб-разработка**</span><span class="sxs-lookup"><span data-stu-id="5f862-116">[Visual Studio 2017 version 15.8 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="5f862-117">Пакет SDK для .NET Core 2.1.или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="5f862-117">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5f862-118">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5f862-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="5f862-119">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5f862-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="5f862-120">Пакет SDK для .NET Core 2.1.или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="5f862-120">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="5f862-121">C# для Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5f862-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5f862-122">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="5f862-122">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="5f862-123">Visual Studio для Mac 7.5.4 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="5f862-123">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="5f862-124">[Пакет SDK для .NET Core 2.1 или более поздней версии](https://www.microsoft.com/net/download/all) (входит в установку Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="5f862-124">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>

---

## <a name="create-a-web-project"></a><span data-ttu-id="5f862-125">Создайте веб-проект.</span><span class="sxs-lookup"><span data-stu-id="5f862-125">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5f862-126">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5f862-126">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="5f862-127">В меню выберите **Файл > Создать проект**.</span><span class="sxs-lookup"><span data-stu-id="5f862-127">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="5f862-128">В диалоговом окне **Новый проект** выберите **Установленные > Visual C# > Веб > Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="5f862-128">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="5f862-129">Назовите проект *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="5f862-129">Name the project *SignalRChat*.</span></span>

  ![Диалоговое окно создания проекта в Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="5f862-131">Выберите **Веб-приложение**, чтобы создать проект, который использует Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="5f862-131">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="5f862-132">Выберите целевую платформу **.NET Core**, **ASP.NET Core 2.1** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="5f862-132">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Диалоговое окно создания проекта в Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5f862-134">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5f862-134">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="5f862-135">Откройте [интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal) и перейдите в папку, в которой будет создаваться папка нового проекта.</span><span class="sxs-lookup"><span data-stu-id="5f862-135">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="5f862-136">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="5f862-136">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5f862-137">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="5f862-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="5f862-138">В меню выберите **Файл > Создать решение**.</span><span class="sxs-lookup"><span data-stu-id="5f862-138">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="5f862-139">Выберите **.NET Core > Приложение > Веб-приложение ASP.NET Core** (не выбирайте **Веб-приложение ASP.NET Core (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="5f862-139">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="5f862-140">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="5f862-140">Select **Next**.</span></span>

* <span data-ttu-id="5f862-141">Присвойте проекту имя *SignalRChat* и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="5f862-141">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="5f862-142">Добавление клиентской библиотеки SignalR</span><span class="sxs-lookup"><span data-stu-id="5f862-142">Add the SignalR client library</span></span>

<span data-ttu-id="5f862-143">Библиотека сервера SignalR входит в состав метапакета `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="5f862-143">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="5f862-144">Клиентская библиотека JavaScript не добавляется в проект автоматически.</span><span class="sxs-lookup"><span data-stu-id="5f862-144">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="5f862-145">В рамках этого руководства вы будете использовать диспетчер библиотек (LibMan), чтобы получить клиентскую библиотеку из *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="5f862-145">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="5f862-146">unpkg — это сеть доставки содержимого, которая позволяет доставить любое содержимое из npm (диспетчера пакетов Node.js).</span><span class="sxs-lookup"><span data-stu-id="5f862-146">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5f862-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5f862-147">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="5f862-148">В **обозревателе решений** щелкните проект правой кнопкой мыши и выберите **Добавить** > **Client-Side Library** (Клиентская библиотека).</span><span class="sxs-lookup"><span data-stu-id="5f862-148">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="5f862-149">В диалоговом окне **Add Client-Side Library** (Добавить клиентскую библиотеку) для параметра **Поставщик** выберите **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="5f862-149">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="5f862-150">В поле **Библиотека** введите `@aspnet/signalr@1` и выберите последнюю версию, но не предварительную.</span><span class="sxs-lookup"><span data-stu-id="5f862-150">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Диалоговое окно добавления клиентской библиотеки — выбор библиотеки](signalr/_static/libman1.png)

* <span data-ttu-id="5f862-152">Щелкните **Choose specific files** (Выбрать определенные файлы), разверните папку *dist/browser* и выберите *signalr.js* и *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="5f862-152">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="5f862-153">В поле **Целевое расположение** укажите *wwwroot/lib/signalr/* и нажмите кнопку **Установить**.</span><span class="sxs-lookup"><span data-stu-id="5f862-153">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Диалоговое окно добавления клиентской библиотеки — выбор файлов и назначения](signalr/_static/libman2.png)

  <span data-ttu-id="5f862-155">LibMan создает папку *wwwroot/lib/signalr* и копирует в нее выбранные файлы.</span><span class="sxs-lookup"><span data-stu-id="5f862-155">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5f862-156">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5f862-156">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="5f862-157">В интегрированном терминале выполните следующую команду, чтобы установить LibMan.</span><span class="sxs-lookup"><span data-stu-id="5f862-157">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="5f862-158">Выполните приведенную ниже команду, чтобы получить клиентскую библиотеку SignalR с помощью LibMan.</span><span class="sxs-lookup"><span data-stu-id="5f862-158">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="5f862-159">Возможно, придется подождать несколько секунд, прежде чем появятся выходные данные.</span><span class="sxs-lookup"><span data-stu-id="5f862-159">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="5f862-160">В параметрах указываются следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="5f862-160">The parameters specify the following options:</span></span>
  * <span data-ttu-id="5f862-161">использование поставщика unpkg;</span><span class="sxs-lookup"><span data-stu-id="5f862-161">Use the unpkg provider.</span></span>
  * <span data-ttu-id="5f862-162">копирование файлов в назначение *wwwroot/lib/signalr*;</span><span class="sxs-lookup"><span data-stu-id="5f862-162">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="5f862-163">копирование только выбранных файлов.</span><span class="sxs-lookup"><span data-stu-id="5f862-163">Copy only the specified files.</span></span>

  <span data-ttu-id="5f862-164">Выходные данные выглядят примерно так, как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="5f862-164">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5f862-165">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="5f862-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="5f862-166">В **терминале** выполните следующую команду, чтобы установить LibMan.</span><span class="sxs-lookup"><span data-stu-id="5f862-166">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="5f862-167">Перейдите в папку проекта (где расположен файл *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="5f862-167">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="5f862-168">Выполните приведенную ниже команду, чтобы получить клиентскую библиотеку SignalR с помощью LibMan.</span><span class="sxs-lookup"><span data-stu-id="5f862-168">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="5f862-169">В параметрах указываются следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="5f862-169">The parameters specify the following options:</span></span>
  * <span data-ttu-id="5f862-170">использование поставщика unpkg;</span><span class="sxs-lookup"><span data-stu-id="5f862-170">Use the unpkg provider.</span></span>
  * <span data-ttu-id="5f862-171">копирование файлов в назначение *wwwroot/lib/signalr*;</span><span class="sxs-lookup"><span data-stu-id="5f862-171">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="5f862-172">копирование только выбранных файлов.</span><span class="sxs-lookup"><span data-stu-id="5f862-172">Copy only the specified files.</span></span>

  <span data-ttu-id="5f862-173">Выходные данные выглядят примерно так, как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="5f862-173">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="5f862-174">Создание концентратора SignalR</span><span class="sxs-lookup"><span data-stu-id="5f862-174">Create a SignalR hub</span></span>

<span data-ttu-id="5f862-175">*hub* — это класс, который служит в качестве конвейера высокого уровня для обработки взаимодействия между клиентом и сервером.</span><span class="sxs-lookup"><span data-stu-id="5f862-175">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="5f862-176">В папке проекта SignalRChat создайте папку *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="5f862-176">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="5f862-177">В папке *Hubs* создайте файл *ChatHub.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="5f862-177">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="5f862-178">Класс `ChatHub` наследует от класса `Hub` SignalR.</span><span class="sxs-lookup"><span data-stu-id="5f862-178">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="5f862-179">Класс `Hub` управляет подключениями, группами и обменом сообщениями.</span><span class="sxs-lookup"><span data-stu-id="5f862-179">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="5f862-180">Метод `SendMessage` может вызываться любым подключенным клиентом.</span><span class="sxs-lookup"><span data-stu-id="5f862-180">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="5f862-181">Он отправляет полученное сообщение всем клиентам.</span><span class="sxs-lookup"><span data-stu-id="5f862-181">It sends the received message to all clients.</span></span> <span data-ttu-id="5f862-182">Код SignalR является асинхронным, поэтому обеспечивает максимальную масштабируемость.</span><span class="sxs-lookup"><span data-stu-id="5f862-182">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="5f862-183">Настройка SignalR</span><span class="sxs-lookup"><span data-stu-id="5f862-183">Configure SignalR</span></span>

<span data-ttu-id="5f862-184">Сервер SignalR необходимо настроить таким образом, чтобы он передавал запросы SignalR в SignalR.</span><span class="sxs-lookup"><span data-stu-id="5f862-184">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="5f862-185">Добавьте следующий выделенный код в файл *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="5f862-185">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="5f862-186">В результате SignalR будет добавлен в систему внедрения зависимостей ASP.NET Core и конвейер ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="5f862-186">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="5f862-187">Добавление кода клиента SignalR</span><span class="sxs-lookup"><span data-stu-id="5f862-187">Add SignalR client code</span></span>

* <span data-ttu-id="5f862-188">Замените содержимое в файле *Pages\Index.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="5f862-188">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="5f862-189">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="5f862-189">The preceding code:</span></span>

  * <span data-ttu-id="5f862-190">Создает текстовые поля для имени и текста сообщения и кнопку отправки.</span><span class="sxs-lookup"><span data-stu-id="5f862-190">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="5f862-191">Создает список с `id="messagesList"` для отображения сообщений, полученных от концентратора SignalR.</span><span class="sxs-lookup"><span data-stu-id="5f862-191">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="5f862-192">Содержит ссылки на скрипты для SignalR и код приложения *chat.js*, который создается в следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="5f862-192">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="5f862-193">В папке *wwwroot/js* создайте файл *chat.js* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="5f862-193">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="5f862-194">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="5f862-194">The preceding code:</span></span>

  * <span data-ttu-id="5f862-195">Создает и запускает подключение.</span><span class="sxs-lookup"><span data-stu-id="5f862-195">Creates and starts a connection.</span></span>
  * <span data-ttu-id="5f862-196">Добавляет к кнопке отправки обработчик, который отправляет сообщения в концентратор.</span><span class="sxs-lookup"><span data-stu-id="5f862-196">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="5f862-197">Добавляет к объекту подключения обработчик, который получает сообщения из концентратора и добавляет их в список.</span><span class="sxs-lookup"><span data-stu-id="5f862-197">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="5f862-198">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="5f862-198">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5f862-199">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5f862-199">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5f862-200">Нажмите клавиши **CTRL+F5**, чтобы запустить приложение без отладки.</span><span class="sxs-lookup"><span data-stu-id="5f862-200">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5f862-201">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5f862-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="5f862-202">В интегрированном терминале выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="5f862-202">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat
  ```
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5f862-203">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="5f862-203">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="5f862-204">В меню выберите **Запуск > Запуск без отладки**.</span><span class="sxs-lookup"><span data-stu-id="5f862-204">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="5f862-205">Скопируйте URL-адрес из адресной строки, откройте другой экземпляр или вкладку браузера и вставьте URL-адрес в адресную строку.</span><span class="sxs-lookup"><span data-stu-id="5f862-205">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="5f862-206">Выберите любой браузер, введите имя и сообщение и нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="5f862-206">Choose either browser, enter a name and message, and select the **Send** button.</span></span>

  <span data-ttu-id="5f862-207">Имя и сообщение отображаются на обеих страницах мгновенно.</span><span class="sxs-lookup"><span data-stu-id="5f862-207">The name and message are displayed on both pages instantly.</span></span>

  ![Пример приложения SignalR](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="5f862-209">Если приложение не работает, откройте средства разработчика для браузера (F12) и перейдите в консоль.</span><span class="sxs-lookup"><span data-stu-id="5f862-209">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="5f862-210">Вы можете увидеть ошибки, связанные с вашим кодом HTML и JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5f862-210">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="5f862-211">Предположим, вы поместили *signalr.js* не в ту папку, которую указали.</span><span class="sxs-lookup"><span data-stu-id="5f862-211">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="5f862-212">В этом случае ссылка на этот файл не будет работать, и вы увидите сообщение об ошибке 404 в консоли.</span><span class="sxs-lookup"><span data-stu-id="5f862-212">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="5f862-213">![ошибка: signalr.js не найден](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="5f862-213">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="5f862-214">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="5f862-214">Next steps</span></span>

<span data-ttu-id="5f862-215">В этом руководстве вы узнали, как:</span><span class="sxs-lookup"><span data-stu-id="5f862-215">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5f862-216">создавать проект веб-приложения;</span><span class="sxs-lookup"><span data-stu-id="5f862-216">Create a web app project.</span></span>
> * <span data-ttu-id="5f862-217">добавлять клиентскую библиотеку SignalR;</span><span class="sxs-lookup"><span data-stu-id="5f862-217">Add the SignalR client library.</span></span>
> * <span data-ttu-id="5f862-218">создавать концентратор SignalR;</span><span class="sxs-lookup"><span data-stu-id="5f862-218">Create a SignalR hub.</span></span>
> * <span data-ttu-id="5f862-219">настраивать проект для использования SignalR;</span><span class="sxs-lookup"><span data-stu-id="5f862-219">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="5f862-220">добавлять код, использующий концентратор для отправки сообщений из любого клиента всем подключенным клиентам.</span><span class="sxs-lookup"><span data-stu-id="5f862-220">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="5f862-221">Дополнительные сведения о SignalR см. во введении:</span><span class="sxs-lookup"><span data-stu-id="5f862-221">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="5f862-222">Введение в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="5f862-222">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
