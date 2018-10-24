---
title: Начало работы с SignalR ASP.NET Core
author: tdykstra
description: В этом руководстве создается приложение чата, которое использует SignalR для ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: tutorials/signalr
ms.openlocfilehash: 55fb6b1c13549129a00541c1228956a93854ad78
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578033"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="5d9c7-103">Руководство. Начало работы с SignalR ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5d9c7-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="5d9c7-104">В этом руководстве описаны основы создания приложения в режиме реального времени с помощью SignalR.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="5d9c7-105">Вы научитесь:</span><span class="sxs-lookup"><span data-stu-id="5d9c7-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5d9c7-106">создавать проект веб-приложения;</span><span class="sxs-lookup"><span data-stu-id="5d9c7-106">Create a web app project.</span></span>
> * <span data-ttu-id="5d9c7-107">добавлять клиентскую библиотеку SignalR;</span><span class="sxs-lookup"><span data-stu-id="5d9c7-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="5d9c7-108">создавать концентратор SignalR;</span><span class="sxs-lookup"><span data-stu-id="5d9c7-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="5d9c7-109">настраивать проект для использования SignalR;</span><span class="sxs-lookup"><span data-stu-id="5d9c7-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="5d9c7-110">добавлять код, использующий концентратор для отправки сообщений из любого клиента всем подключенным клиентам.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-110">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="5d9c7-111">В итоге вы получите работающее приложение чата:</span><span class="sxs-lookup"><span data-stu-id="5d9c7-111">At the end, you'll have a working chat app:</span></span>

![Пример приложения SignalR](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="5d9c7-113">[Просмотреть или скачать пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([описание скачивания](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="5d9c7-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5d9c7-114">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="5d9c7-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5d9c7-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5d9c7-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5d9c7-116">[Visual Studio 2017 15.8 или более поздней версии](https://www.visualstudio.com/downloads/) с рабочей нагрузкой **ASP.NET и веб-разработка**</span><span class="sxs-lookup"><span data-stu-id="5d9c7-116">[Visual Studio 2017 version 15.8 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="5d9c7-117">Пакет SDK для .NET Core 2.1.или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="5d9c7-117">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5d9c7-118">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="5d9c7-119">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="5d9c7-120">Пакет SDK для .NET Core 2.1.или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="5d9c7-120">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="5d9c7-121">C# для Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5d9c7-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5d9c7-122">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="5d9c7-122">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="5d9c7-123">Visual Studio для Mac 7.5.4 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="5d9c7-123">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="5d9c7-124">[Пакет SDK для .NET Core 2.1 или более поздней версии](https://www.microsoft.com/net/download/all) (входит в установку Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="5d9c7-124">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>

---

## <a name="create-a-web-project"></a><span data-ttu-id="5d9c7-125">Создайте веб-проект.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-125">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5d9c7-126">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5d9c7-126">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="5d9c7-127">В меню выберите **Файл > Создать проект**.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-127">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="5d9c7-128">В диалоговом окне **Новый проект** выберите **Установленные > Visual C# > Веб > Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-128">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="5d9c7-129">Назовите проект *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-129">Name the project *SignalRChat*.</span></span>

  ![Диалоговое окно создания проекта в Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="5d9c7-131">Выберите **Веб-приложение**, чтобы создать проект, который использует Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-131">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="5d9c7-132">Выберите целевую платформу **.NET Core**, **ASP.NET Core 2.1** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-132">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Диалоговое окно создания проекта в Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5d9c7-134">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-134">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="5d9c7-135">Откройте папку, которую можно использовать для нового проекта.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-135">Open a folder that you can use for a new project.</span></span>

* <span data-ttu-id="5d9c7-136">Во **встроенном терминале** выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="5d9c7-136">In the **Integrated Terminal**, run the following command:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5d9c7-137">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="5d9c7-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="5d9c7-138">В меню выберите **Файл > Создать решение**.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-138">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="5d9c7-139">Выберите **.NET Core > Приложение > Веб-приложение ASP.NET Core** (не выбирайте **Веб-приложение ASP.NET Core (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="5d9c7-139">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="5d9c7-140">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-140">Select **Next**.</span></span>

* <span data-ttu-id="5d9c7-141">Присвойте проекту имя *SignalRChat* и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-141">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="5d9c7-142">Добавление клиентской библиотеки SignalR</span><span class="sxs-lookup"><span data-stu-id="5d9c7-142">Add the SignalR client library</span></span>

<span data-ttu-id="5d9c7-143">Библиотека сервера SignalR входит в состав метапакета `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-143">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="5d9c7-144">Клиентская библиотека JavaScript не добавляется в проект автоматически.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-144">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="5d9c7-145">В рамках этого руководства вы будете использовать диспетчер библиотек (LibMan), чтобы получить клиентскую библиотеку из *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-145">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="5d9c7-146">unpkg — это сеть доставки содержимого, которая позволяет доставить любое содержимое из npm (диспетчера пакетов Node.js).</span><span class="sxs-lookup"><span data-stu-id="5d9c7-146">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5d9c7-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5d9c7-147">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="5d9c7-148">В **обозревателе решений** щелкните проект правой кнопкой мыши и выберите **Добавить** > **Client-Side Library** (Клиентская библиотека).</span><span class="sxs-lookup"><span data-stu-id="5d9c7-148">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="5d9c7-149">В диалоговом окне **Add Client-Side Library** (Добавить клиентскую библиотеку) для параметра **Поставщик** выберите **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-149">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="5d9c7-150">В поле **Библиотека** введите `@aspnet/signalr@1` и выберите последнюю версию, но не предварительную.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-150">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Диалоговое окно добавления клиентской библиотеки — выбор библиотеки](signalr/_static/libman1.png)

* <span data-ttu-id="5d9c7-152">Щелкните **Choose specific files** (Выбрать определенные файлы), разверните папку *dist/browser* и выберите *signalr.js* и *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-152">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="5d9c7-153">В поле **Целевое расположение** укажите *wwwroot/lib/signalr/* и нажмите кнопку **Установить**.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-153">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Диалоговое окно добавления клиентской библиотеки — выбор файлов и назначения](signalr/_static/libman2.png)

  <span data-ttu-id="5d9c7-155">LibMan создает папку *wwwroot/lib/signalr* и копирует в нее выбранные файлы.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-155">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5d9c7-156">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-156">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="5d9c7-157">Во **встроенном терминале** выполните следующую команду, чтобы установить LibMan.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-157">In the **Integrated Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="5d9c7-158">Перейдите в папку проекта (где расположен файл *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="5d9c7-158">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="5d9c7-159">Выполните приведенную ниже команду, чтобы получить клиентскую библиотеку SignalR с помощью LibMan.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-159">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="5d9c7-160">Возможно, придется подождать несколько секунд, прежде чем появятся выходные данные.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-160">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="5d9c7-161">В параметрах указываются следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="5d9c7-161">The parameters specify the following options:</span></span>
  * <span data-ttu-id="5d9c7-162">использование поставщика unpkg;</span><span class="sxs-lookup"><span data-stu-id="5d9c7-162">Use the unpkg provider.</span></span>
  * <span data-ttu-id="5d9c7-163">копирование файлов в назначение *wwwroot/lib/signalr*;</span><span class="sxs-lookup"><span data-stu-id="5d9c7-163">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="5d9c7-164">копирование только выбранных файлов.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-164">Copy only the specified files.</span></span>

  <span data-ttu-id="5d9c7-165">Выходные данные выглядят примерно так, как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="5d9c7-165">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5d9c7-166">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="5d9c7-166">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="5d9c7-167">В **терминале** выполните следующую команду, чтобы установить LibMan.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-167">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="5d9c7-168">Перейдите в папку проекта (где расположен файл *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="5d9c7-168">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="5d9c7-169">Выполните приведенную ниже команду, чтобы получить клиентскую библиотеку SignalR с помощью LibMan.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-169">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="5d9c7-170">В параметрах указываются следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="5d9c7-170">The parameters specify the following options:</span></span>
  * <span data-ttu-id="5d9c7-171">использование поставщика unpkg;</span><span class="sxs-lookup"><span data-stu-id="5d9c7-171">Use the unpkg provider.</span></span>
  * <span data-ttu-id="5d9c7-172">копирование файлов в назначение *wwwroot/lib/signalr*;</span><span class="sxs-lookup"><span data-stu-id="5d9c7-172">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="5d9c7-173">копирование только выбранных файлов.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-173">Copy only the specified files.</span></span>

  <span data-ttu-id="5d9c7-174">Выходные данные выглядят примерно так, как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="5d9c7-174">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="5d9c7-175">Создание концентратора SignalR</span><span class="sxs-lookup"><span data-stu-id="5d9c7-175">Create a SignalR hub</span></span>

<span data-ttu-id="5d9c7-176">*hub* — это класс, который служит в качестве конвейера высокого уровня для обработки взаимодействия между клиентом и сервером.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-176">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="5d9c7-177">В папке проекта SignalRChat создайте папку *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-177">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="5d9c7-178">В папке *Hubs* создайте файл *ChatHub.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="5d9c7-178">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="5d9c7-179">Класс `ChatHub` наследует от класса `Hub` SignalR.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-179">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="5d9c7-180">Класс `Hub` управляет подключениями, группами и обменом сообщениями.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-180">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="5d9c7-181">Метод `SendMessage` может вызываться любым подключенным клиентом.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-181">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="5d9c7-182">Он отправляет полученное сообщение всем клиентам.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-182">It sends the received message to all clients.</span></span> <span data-ttu-id="5d9c7-183">Код SignalR является асинхронным, поэтому обеспечивает максимальную масштабируемость.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-183">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="5d9c7-184">Настройка SignalR</span><span class="sxs-lookup"><span data-stu-id="5d9c7-184">Configure SignalR</span></span>

<span data-ttu-id="5d9c7-185">Сервер SignalR необходимо настроить таким образом, чтобы он передавал запросы SignalR в SignalR.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-185">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="5d9c7-186">Добавьте следующий выделенный код в файл *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-186">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="5d9c7-187">В результате SignalR будет добавлен в систему внедрения зависимостей ASP.NET Core и конвейер ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-187">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="5d9c7-188">Добавление кода клиента SignalR</span><span class="sxs-lookup"><span data-stu-id="5d9c7-188">Add SignalR client code</span></span>

* <span data-ttu-id="5d9c7-189">Замените содержимое в файле *Pages\Index.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="5d9c7-189">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="5d9c7-190">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="5d9c7-190">The preceding code:</span></span>

  * <span data-ttu-id="5d9c7-191">Создает текстовые поля для имени и текста сообщения и кнопку отправки.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-191">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="5d9c7-192">Создает список с `id="messagesList"` для отображения сообщений, полученных от концентратора SignalR.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-192">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="5d9c7-193">Содержит ссылки на скрипты для SignalR и код приложения *chat.js*, который создается в следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-193">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="5d9c7-194">В папке *wwwroot/js* создайте файл *chat.js* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="5d9c7-194">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="5d9c7-195">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="5d9c7-195">The preceding code:</span></span>

  * <span data-ttu-id="5d9c7-196">Создает и запускает подключение.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-196">Creates and starts a connection.</span></span>
  * <span data-ttu-id="5d9c7-197">Добавляет к кнопке отправки обработчик, который отправляет сообщения в концентратор.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-197">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="5d9c7-198">Добавляет к объекту подключения обработчик, который получает сообщения из концентратора и добавляет их в список.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-198">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="5d9c7-199">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="5d9c7-199">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5d9c7-200">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5d9c7-200">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5d9c7-201">Нажмите клавиши **CTRL+F5**, чтобы запустить приложение без отладки.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-201">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5d9c7-202">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-202">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="5d9c7-203">Нажмите клавиши **CTRL+F5**, чтобы запустить приложение без отладки.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-203">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5d9c7-204">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="5d9c7-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="5d9c7-205">В меню выберите **Запуск > Запуск без отладки**.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-205">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="5d9c7-206">Скопируйте URL-адрес из адресной строки, откройте другой экземпляр или вкладку браузера и вставьте URL-адрес в адресную строку.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-206">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="5d9c7-207">Выберите любой браузер, введите имя и сообщение и нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-207">Choose either browser, enter a name and message, and select the **Send** button.</span></span>

  <span data-ttu-id="5d9c7-208">Имя и сообщение отображаются на обеих страницах мгновенно.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-208">The name and message are displayed on both pages instantly.</span></span>

  ![Пример приложения SignalR](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="5d9c7-210">Если приложение не работает, откройте средства разработчика для браузера (F12) и перейдите в консоль.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-210">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="5d9c7-211">Вы можете увидеть ошибки, связанные с вашим кодом HTML и JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-211">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="5d9c7-212">Предположим, вы поместили *signalr.js* не в ту папку, которую указали.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-212">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="5d9c7-213">В этом случае ссылка на этот файл не будет работать, и вы увидите сообщение об ошибке 404 в консоли.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-213">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="5d9c7-214">![ошибка: signalr.js не найден](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="5d9c7-214">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="5d9c7-215">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="5d9c7-215">Next steps</span></span>

<span data-ttu-id="5d9c7-216">В этом руководстве вы узнали, как:</span><span class="sxs-lookup"><span data-stu-id="5d9c7-216">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5d9c7-217">создавать проект веб-приложения;</span><span class="sxs-lookup"><span data-stu-id="5d9c7-217">Create a web app project.</span></span>
> * <span data-ttu-id="5d9c7-218">добавлять клиентскую библиотеку SignalR;</span><span class="sxs-lookup"><span data-stu-id="5d9c7-218">Add the SignalR client library.</span></span>
> * <span data-ttu-id="5d9c7-219">создавать концентратор SignalR;</span><span class="sxs-lookup"><span data-stu-id="5d9c7-219">Create a SignalR hub.</span></span>
> * <span data-ttu-id="5d9c7-220">настраивать проект для использования SignalR;</span><span class="sxs-lookup"><span data-stu-id="5d9c7-220">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="5d9c7-221">добавлять код, использующий концентратор для отправки сообщений из любого клиента всем подключенным клиентам.</span><span class="sxs-lookup"><span data-stu-id="5d9c7-221">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="5d9c7-222">Дополнительные сведения о SignalR см. во введении:</span><span class="sxs-lookup"><span data-stu-id="5d9c7-222">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="5d9c7-223">Введение в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="5d9c7-223">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
