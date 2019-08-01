---
title: Начало работы с MVC ASP.NET Core
author: rick-anderson
description: Сведения о начале работы с MVC ASP.NET Core.
ms.author: riande
ms.date: 04/24/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: b3544769b0e40fc27f5b6c939c6d7b5f9082f854
ms.sourcegitcommit: 979dbfc5e9ce09b9470789989cddfcfb57079d94
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682813"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="ee0b5-103">Начало работы с MVC ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ee0b5-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="ee0b5-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="ee0b5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="ee0b5-105">В этом учебнике приводятся основные сведения о веб-приложении MVC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="ee0b5-106">Это приложение служит для управления базой данных названий фильмов.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="ee0b5-107">Вы научитесь:</span><span class="sxs-lookup"><span data-stu-id="ee0b5-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ee0b5-108">Создание веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-108">Create a web app.</span></span>
> * <span data-ttu-id="ee0b5-109">Добавление модели и формирование шаблона.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="ee0b5-110">Работа с базой данных.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-110">Work with a database.</span></span>
> * <span data-ttu-id="ee0b5-111">Добавление поиска и проверки.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-111">Add search and validation.</span></span>

<span data-ttu-id="ee0b5-112">В конечном итоге вы получите приложение, позволяющее управлять данными фильмов и отображать их.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="ee0b5-113">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="ee0b5-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ee0b5-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ee0b5-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ee0b5-115">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ee0b5-116">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="ee0b5-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app"></a><span data-ttu-id="ee0b5-117">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="ee0b5-117">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ee0b5-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ee0b5-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ee0b5-119">В Visual Studio выберите **Создать проект**.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="ee0b5-120">Выберите пункт **Веб-приложение ASP.NET Core** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-120">Selecct **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Новое веб-приложение ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="ee0b5-122">Присвойте проекту имя **MvcMovie** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="ee0b5-123">Имя **MvcMovie** необходимо присвоить для того, чтобы при копировании кода пространства имен совпали.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Новое веб-приложение ASP.NET Core](start-mvc/_static/config.png)

* <span data-ttu-id="ee0b5-125">Выберите **Веб-приложение (модель-представление-контроллер)** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="ee0b5-126">Диалоговое окно нового проекта, .NET Core в левой области, веб-узел ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ee0b5-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="ee0b5-127">В Visual Studio используется только что созданный вами шаблон по умолчанию для проекта MVC.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="ee0b5-128">Чтобы приложение стало рабочим, осталось только указать имя проекта и выбрать несколько параметров.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="ee0b5-129">Это простой начальный проект.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-129">This is a basic starter project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ee0b5-130">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="ee0b5-131">Для работы с этим руководством требуется знание VS Code.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="ee0b5-132">Дополнительные сведения см. в разделах [Начало работы с VS Code](https://code.visualstudio.com/docs) и [Справка по Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="ee0b5-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="ee0b5-133">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="ee0b5-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="ee0b5-134">Смените каталог (`cd`) на папку, в которой будет содержаться проект.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="ee0b5-135">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="ee0b5-135">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="ee0b5-136">Появится диалоговое окно с предупреждением **В MvcMovie отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="ee0b5-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="ee0b5-137">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-137">Select **Yes**</span></span>

  * <span data-ttu-id="ee0b5-138">`dotnet new mvc -o MvcMovie`: создает новый проект MVC ASP.NET Core в папке *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="ee0b5-139">`code -r MvcMovie`: загружает файл проекта *MvcMovie.csproj* в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ee0b5-140">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="ee0b5-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="ee0b5-141">Выберите **Файл** > **Новое решение**.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-141">Select **File** > **New Solution**.</span></span>

  ![Новое решение macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="ee0b5-143">Выберите **.NET Core** > **Приложение** > **Веб-приложение (модель — представление — контроллер)** > **Далее**.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-143">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![Диалоговое окно "Новый проект" в macOS](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="ee0b5-145">В диалоговом окне **Настройка нового веб-API ASP.NET Core** установите для параметра **Целевая платформа** значение **.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-145">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** of **.NET Core 3.0**.</span></span>

<!-- 
  ![macOS .NET Core 2.2 selection](./start-mvc/_static/new_project_22_vsmac.png)
-->

* <span data-ttu-id="ee0b5-146">Присвойте проекту имя **MvcMovie** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-146">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="ee0b5-147">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="ee0b5-147">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ee0b5-148">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ee0b5-148">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ee0b5-149">Нажмите **CTRl+F5**, чтобы запустить приложение без отладки.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-149">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="ee0b5-150">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-150">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="ee0b5-151">Обратите внимание на то, что в адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-151">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ee0b5-152">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-152">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="ee0b5-153">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-153">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="ee0b5-154">Запуск приложения с помощью клавиш CTRL+F5 (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-154">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="ee0b5-155">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-155">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="ee0b5-156">Из меню **Отладка** можно запустить приложение в режиме с отладкой или без.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-156">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Меню отладки](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="ee0b5-158">Чтобы выполнить отладку приложения, нажмите кнопку **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-158">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)


  <span data-ttu-id="ee0b5-160">Пример приложения приведен на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="ee0b5-160">The following image shows the app:</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ee0b5-162">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-162">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="ee0b5-163">Нажмите клавиши CTRL+F5, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-163">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="ee0b5-164">Visual Studio Code запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-164">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="ee0b5-165">В адресной строке указывается `localhost:port:5001`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-165">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="ee0b5-166">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-166">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="ee0b5-167">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-167">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="ee0b5-168">Запуск приложения с помощью клавиш CTRL+F5 (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-168">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="ee0b5-169">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро обновлять страницу и просматривать изменения.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-169">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="ee0b5-170">Нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-170">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="ee0b5-171">Это приложение не отслеживает персональные данные.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-171">This app doesn't track personal information.</span></span> <span data-ttu-id="ee0b5-172">Созданный шаблоном код включает ресурсы для соблюдения [Общего регламента по защите данных (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="ee0b5-172">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/privacy.png)

  <span data-ttu-id="ee0b5-174">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="ee0b5-174">The following image shows the app after accepting tracking:</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ee0b5-176">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="ee0b5-176">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="ee0b5-177">Выберите **Выполнить** > **Запуск без отладки**, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-177">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="ee0b5-178">Visual Studio для Mac запустит сервер [Kestrel](xref:fundamentals/servers/index#kestrel), откроет браузер и перейдет к `http://localhost:port`, где *port* — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-178">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="ee0b5-179">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-179">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ee0b5-180">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-180">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="ee0b5-181">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-181">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="ee0b5-182">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-182">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="ee0b5-183">В меню **Запуск** можно запустить приложение в режиме с отладкой или без нее.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-183">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

  <span data-ttu-id="ee0b5-184">Пример приложения приведен на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="ee0b5-184">The following image shows the app:</span></span>

  ![Домашняя или индексная страница](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="ee0b5-186">В следующей части этого учебника мы поговорим об MVC и приступим к написанию кода.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-186">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ee0b5-187">Вперед</span><span class="sxs-lookup"><span data-stu-id="ee0b5-187">Next</span></span>](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="ee0b5-188">В этом учебнике приводятся основные сведения о веб-приложении MVC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-188">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="ee0b5-189">Это приложение служит для управления базой данных названий фильмов.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-189">The app manages a database of movie titles.</span></span> <span data-ttu-id="ee0b5-190">Вы научитесь:</span><span class="sxs-lookup"><span data-stu-id="ee0b5-190">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ee0b5-191">Создание веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-191">Create a web app.</span></span>
> * <span data-ttu-id="ee0b5-192">Добавление модели и формирование шаблона.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-192">Add and scaffold a model.</span></span>
> * <span data-ttu-id="ee0b5-193">Работа с базой данных.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-193">Work with a database.</span></span>
> * <span data-ttu-id="ee0b5-194">Добавление поиска и проверки.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-194">Add search and validation.</span></span>

<span data-ttu-id="ee0b5-195">В конечном итоге вы получите приложение, позволяющее управлять данными фильмов и отображать их.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-195">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="ee0b5-196">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="ee0b5-196">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ee0b5-197">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ee0b5-197">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ee0b5-198">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-198">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ee0b5-199">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="ee0b5-199">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="ee0b5-200">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="ee0b5-200">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ee0b5-201">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ee0b5-201">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ee0b5-202">В Visual Studio выберите **Создать проект**.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-202">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="ee0b5-203">Выберите пункт **Веб-приложение ASP.NET Core** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-203">Selecct **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Новое веб-приложение ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="ee0b5-205">Присвойте проекту имя **MvcMovie** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-205">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="ee0b5-206">Имя **MvcMovie** необходимо присвоить для того, чтобы при копировании кода пространства имен совпали.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-206">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Новое веб-приложение ASP.NET Core](start-mvc/_static/config.png)


* <span data-ttu-id="ee0b5-208">Выберите **Веб-приложение (модель-представление-контроллер)** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-208">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="ee0b5-209">Диалоговое окно нового проекта, .NET Core в левой области, веб-узел ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ee0b5-209">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="ee0b5-210">В Visual Studio используется только что созданный вами шаблон по умолчанию для проекта MVC.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-210">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="ee0b5-211">Чтобы приложение стало рабочим, осталось только указать имя проекта и выбрать несколько параметров.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-211">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="ee0b5-212">Это простой начальный проект и хорошая стартовая точка.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-212">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ee0b5-213">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-213">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="ee0b5-214">Для работы с этим руководством требуется знание VS Code.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-214">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="ee0b5-215">Дополнительные сведения см. в разделах [Начало работы с VS Code](https://code.visualstudio.com/docs) и [Справка по Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="ee0b5-215">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="ee0b5-216">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="ee0b5-216">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="ee0b5-217">Смените каталог (`cd`) на папку, в которой будет содержаться проект.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-217">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="ee0b5-218">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="ee0b5-218">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="ee0b5-219">Появится диалоговое окно с предупреждением **В MvcMovie отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="ee0b5-219">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="ee0b5-220">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-220">Select **Yes**</span></span>

  * <span data-ttu-id="ee0b5-221">`dotnet new mvc -o MvcMovie`: создает новый проект MVC ASP.NET Core в папке *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-221">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="ee0b5-222">`code -r MvcMovie`: загружает файл проекта *MvcMovie.csproj* в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-222">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ee0b5-223">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="ee0b5-223">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="ee0b5-224">Выберите **Файл** > **Новое решение**.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-224">Select **File** > **New Solution**.</span></span>

  ![Новое решение macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="ee0b5-226">Выберите **.NET Core** > **Приложение** > **Веб-приложение (модель — представление — контроллер)** > **Далее**.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-226">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![Диалоговое окно "Новый проект" в macOS](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="ee0b5-228">В диалоговом окне **Настройка нового веб-API ASP.NET Core** оставьте установленное по умолчанию значение для параметра **Целевая платформа**, то есть **.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-228">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![Выбор .NET Core 2.2 для macOS](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="ee0b5-230">Присвойте проекту имя **MvcMovie** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-230">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="ee0b5-231">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="ee0b5-231">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ee0b5-232">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ee0b5-232">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ee0b5-233">Нажмите **CTRl+F5**, чтобы запустить приложение без отладки.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-233">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="ee0b5-234">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-234">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="ee0b5-235">Обратите внимание на то, что в адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-235">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ee0b5-236">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-236">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="ee0b5-237">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-237">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="ee0b5-238">Запуск приложения с помощью клавиш CTRL+F5 (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-238">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="ee0b5-239">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-239">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="ee0b5-240">Из меню **Отладка** можно запустить приложение в режиме с отладкой или без.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-240">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Меню отладки](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="ee0b5-242">Чтобы выполнить отладку приложения, нажмите кнопку **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-242">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="ee0b5-244">Нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-244">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="ee0b5-245">Это приложение не отслеживает персональные данные.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-245">This app doesn't track personal information.</span></span> <span data-ttu-id="ee0b5-246">Созданный шаблоном код включает ресурсы для соблюдения [Общего регламента по защите данных (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="ee0b5-246">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/privacy.png)

  <span data-ttu-id="ee0b5-248">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="ee0b5-248">The following image shows the app after accepting tracking:</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ee0b5-250">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-250">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="ee0b5-251">Нажмите клавиши CTRL+F5, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-251">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="ee0b5-252">Visual Studio Code запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-252">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="ee0b5-253">В адресной строке указывается `localhost:port:5001`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-253">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="ee0b5-254">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-254">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="ee0b5-255">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-255">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="ee0b5-256">Запуск приложения с помощью клавиш CTRL+F5 (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-256">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="ee0b5-257">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро обновлять страницу и просматривать изменения.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-257">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="ee0b5-258">Нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-258">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="ee0b5-259">Это приложение не отслеживает персональные данные.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-259">This app doesn't track personal information.</span></span> <span data-ttu-id="ee0b5-260">Созданный шаблоном код включает ресурсы для соблюдения [Общего регламента по защите данных (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="ee0b5-260">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/privacy.png)

  <span data-ttu-id="ee0b5-262">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="ee0b5-262">The following image shows the app after accepting tracking:</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ee0b5-264">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="ee0b5-264">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="ee0b5-265">Выберите **Выполнить** > **Запуск без отладки**, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-265">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="ee0b5-266">Visual Studio для Mac запустит сервер [Kestrel](xref:fundamentals/servers/index#kestrel), откроет браузер и перейдет к `http://localhost:port`, где *port* — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-266">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="ee0b5-267">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-267">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ee0b5-268">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-268">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="ee0b5-269">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-269">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="ee0b5-270">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-270">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="ee0b5-271">В меню **Запуск** можно запустить приложение в режиме с отладкой или без нее.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-271">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="ee0b5-272">Нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-272">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="ee0b5-273">Это приложение не отслеживает персональные данные.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-273">This app doesn't track personal information.</span></span> <span data-ttu-id="ee0b5-274">Созданный шаблоном код включает ресурсы для соблюдения [Общего регламента по защите данных (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="ee0b5-274">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="ee0b5-276">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="ee0b5-276">The following image shows the app after accepting tracking:</span></span>

  ![Домашняя или индексная страница](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="ee0b5-278">В следующей части этого учебника мы поговорим об MVC и приступим к написанию кода.</span><span class="sxs-lookup"><span data-stu-id="ee0b5-278">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ee0b5-279">Вперед</span><span class="sxs-lookup"><span data-stu-id="ee0b5-279">Next</span></span>](adding-controller.md)

::: moniker-end
