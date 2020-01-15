---
title: Начало работы с MVC ASP.NET Core
author: rick-anderson
description: Сведения о начале работы с MVC ASP.NET Core.
ms.author: riande
ms.date: 10/16/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 901257efdfbc7b36249233745175f5ed253da2c7
ms.sourcegitcommit: da2fb2d78ce70accdba903ccbfdcfffdd0112123
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/07/2020
ms.locfileid: "75722898"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="d0f14-103">Начало работы с MVC ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d0f14-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="d0f14-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="d0f14-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="d0f14-105">В этом учебнике приводятся основные сведения о веб-приложении MVC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d0f14-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="d0f14-106">Это приложение служит для управления базой данных названий фильмов.</span><span class="sxs-lookup"><span data-stu-id="d0f14-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="d0f14-107">Вы научитесь:</span><span class="sxs-lookup"><span data-stu-id="d0f14-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d0f14-108">Создание веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="d0f14-108">Create a web app.</span></span>
> * <span data-ttu-id="d0f14-109">Добавление модели и формирование шаблона.</span><span class="sxs-lookup"><span data-stu-id="d0f14-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="d0f14-110">Работа с базой данных.</span><span class="sxs-lookup"><span data-stu-id="d0f14-110">Work with a database.</span></span>
> * <span data-ttu-id="d0f14-111">Добавление поиска и проверки.</span><span class="sxs-lookup"><span data-stu-id="d0f14-111">Add search and validation.</span></span>

<span data-ttu-id="d0f14-112">В конечном итоге вы получите приложение, позволяющее управлять данными фильмов и отображать их.</span><span class="sxs-lookup"><span data-stu-id="d0f14-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="d0f14-113">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="d0f14-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d0f14-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d0f14-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d0f14-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d0f14-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d0f14-116">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="d0f14-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-web-app"></a><span data-ttu-id="d0f14-117">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="d0f14-117">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d0f14-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d0f14-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d0f14-119">В Visual Studio выберите **Создать проект**.</span><span class="sxs-lookup"><span data-stu-id="d0f14-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="d0f14-120">Выберите пункт **Веб-приложение ASP.NET Core** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="d0f14-120">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Новое веб-приложение ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="d0f14-122">Присвойте проекту имя **MvcMovie** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="d0f14-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="d0f14-123">Имя **MvcMovie** необходимо присвоить для того, чтобы при копировании кода пространства имен совпали.</span><span class="sxs-lookup"><span data-stu-id="d0f14-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Новое веб-приложение ASP.NET Core](start-mvc/_static/config.png)

* <span data-ttu-id="d0f14-125">Выберите **Веб-приложение (модель-представление-контроллер)** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="d0f14-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="d0f14-126">Диалоговое окно нового проекта, .NET Core в левой области, веб-узел ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d0f14-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project30.png)

<span data-ttu-id="d0f14-127">В Visual Studio используется только что созданный вами шаблон по умолчанию для проекта MVC.</span><span class="sxs-lookup"><span data-stu-id="d0f14-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="d0f14-128">Чтобы приложение стало рабочим, осталось только указать имя проекта и выбрать несколько параметров.</span><span class="sxs-lookup"><span data-stu-id="d0f14-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="d0f14-129">Это простой начальный проект.</span><span class="sxs-lookup"><span data-stu-id="d0f14-129">This is a basic starter project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d0f14-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d0f14-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="d0f14-131">Для работы с этим руководством требуется знание VS Code.</span><span class="sxs-lookup"><span data-stu-id="d0f14-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="d0f14-132">Дополнительные сведения см. в разделах [Начало работы с VS Code](https://code.visualstudio.com/docs) и [Справка по Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="d0f14-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="d0f14-133">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="d0f14-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="d0f14-134">Смените каталог (`cd`) на папку, в которой будет содержаться проект.</span><span class="sxs-lookup"><span data-stu-id="d0f14-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="d0f14-135">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="d0f14-135">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="d0f14-136">Появится диалоговое окно с предупреждением **В MvcMovie отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="d0f14-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="d0f14-137">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="d0f14-137">Select **Yes**</span></span>

  * <span data-ttu-id="d0f14-138">`dotnet new mvc -o MvcMovie`: создает новый проект MVC ASP.NET Core в папке *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="d0f14-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="d0f14-139">`code -r MvcMovie`. загружает файл проекта *MvcMovie.csproj* в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d0f14-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d0f14-140">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="d0f14-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="d0f14-141">Щелкните **Файл** > **Новое решение**.</span><span class="sxs-lookup"><span data-stu-id="d0f14-141">Select **File** > **New Solution**.</span></span>

  ![Новое решение macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="d0f14-143">Щелкните **.NET Core** > **Приложение** > **Веб-приложение (Модель — представление — контроллер)** > **Далее**.</span><span class="sxs-lookup"><span data-stu-id="d0f14-143">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![Диалоговое окно "Новый проект" в macOS](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="d0f14-145">В диалоговом окне **Настройка нового веб-API ASP.NET Core** установите для параметра **Целевая платформа** значение **.NET Core 3.1**.</span><span class="sxs-lookup"><span data-stu-id="d0f14-145">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** of **.NET Core 3.1**.</span></span>

  ![Выбор .NET Core 3.1 для macOS](./start-mvc/_static/new_project_31_vsmac.png)

* <span data-ttu-id="d0f14-147">Присвойте проекту имя **MvcMovie** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="d0f14-147">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="d0f14-148">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="d0f14-148">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d0f14-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d0f14-149">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d0f14-150">Нажмите **CTRl+F5**, чтобы запустить приложение без отладки.</span><span class="sxs-lookup"><span data-stu-id="d0f14-150">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="d0f14-151">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение.</span><span class="sxs-lookup"><span data-stu-id="d0f14-151">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="d0f14-152">Обратите внимание на то, что в адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="d0f14-152">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="d0f14-153">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="d0f14-153">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="d0f14-154">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="d0f14-154">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="d0f14-155">Запуск приложения с помощью клавиш CTRL+F5 (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="d0f14-155">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="d0f14-156">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.</span><span class="sxs-lookup"><span data-stu-id="d0f14-156">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="d0f14-157">Из меню **Отладка** можно запустить приложение в режиме с отладкой или без.</span><span class="sxs-lookup"><span data-stu-id="d0f14-157">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Меню отладки](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="d0f14-159">Чтобы выполнить отладку приложения, нажмите кнопку **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="d0f14-159">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

  <span data-ttu-id="d0f14-161">Пример приложения приведен на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="d0f14-161">The following image shows the app:</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d0f14-163">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d0f14-163">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="d0f14-164">Нажмите клавиши CTRL+F5, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="d0f14-164">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="d0f14-165">Visual Studio Code запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d0f14-165">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="d0f14-166">В адресной строке указывается `localhost:port:5001`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="d0f14-166">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="d0f14-167">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="d0f14-167">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="d0f14-168">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="d0f14-168">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="d0f14-169">Запуск приложения с помощью клавиш CTRL+F5 (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="d0f14-169">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="d0f14-170">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро обновлять страницу и просматривать изменения.</span><span class="sxs-lookup"><span data-stu-id="d0f14-170">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d0f14-172">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="d0f14-172">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="d0f14-173">Выберите **Выполнить** > **Запуск без отладки**, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="d0f14-173">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="d0f14-174">Visual Studio для Mac запустит сервер [Kestrel](xref:fundamentals/servers/index#kestrel), откроет браузер и перейдет к `http://localhost:port`, где *port* — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="d0f14-174">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="d0f14-175">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="d0f14-175">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="d0f14-176">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="d0f14-176">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="d0f14-177">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="d0f14-177">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="d0f14-178">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="d0f14-178">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="d0f14-179">В меню **Запуск** можно запустить приложение в режиме с отладкой или без нее.</span><span class="sxs-lookup"><span data-stu-id="d0f14-179">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

  <span data-ttu-id="d0f14-180">Пример приложения приведен на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="d0f14-180">The following image shows the app:</span></span>

  ![Домашняя или индексная страница](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="d0f14-182">В следующей части этого учебника мы поговорим об MVC и приступим к написанию кода.</span><span class="sxs-lookup"><span data-stu-id="d0f14-182">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d0f14-183">Вперед</span><span class="sxs-lookup"><span data-stu-id="d0f14-183">Next</span></span>](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="d0f14-184">В этом учебнике приводятся основные сведения о веб-приложении MVC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d0f14-184">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="d0f14-185">Это приложение служит для управления базой данных названий фильмов.</span><span class="sxs-lookup"><span data-stu-id="d0f14-185">The app manages a database of movie titles.</span></span> <span data-ttu-id="d0f14-186">Вы научитесь:</span><span class="sxs-lookup"><span data-stu-id="d0f14-186">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d0f14-187">Создание веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="d0f14-187">Create a web app.</span></span>
> * <span data-ttu-id="d0f14-188">Добавление модели и формирование шаблона.</span><span class="sxs-lookup"><span data-stu-id="d0f14-188">Add and scaffold a model.</span></span>
> * <span data-ttu-id="d0f14-189">Работа с базой данных.</span><span class="sxs-lookup"><span data-stu-id="d0f14-189">Work with a database.</span></span>
> * <span data-ttu-id="d0f14-190">Добавление поиска и проверки.</span><span class="sxs-lookup"><span data-stu-id="d0f14-190">Add search and validation.</span></span>

<span data-ttu-id="d0f14-191">В конечном итоге вы получите приложение, позволяющее управлять данными фильмов и отображать их.</span><span class="sxs-lookup"><span data-stu-id="d0f14-191">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="d0f14-192">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="d0f14-192">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d0f14-193">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d0f14-193">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d0f14-194">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d0f14-194">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d0f14-195">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="d0f14-195">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="d0f14-196">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="d0f14-196">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d0f14-197">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d0f14-197">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d0f14-198">В Visual Studio выберите **Создать проект**.</span><span class="sxs-lookup"><span data-stu-id="d0f14-198">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="d0f14-199">Выберите пункт **Веб-приложение ASP.NET Core** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="d0f14-199">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Новое веб-приложение ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="d0f14-201">Присвойте проекту имя **MvcMovie** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="d0f14-201">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="d0f14-202">Имя **MvcMovie** необходимо присвоить для того, чтобы при копировании кода пространства имен совпали.</span><span class="sxs-lookup"><span data-stu-id="d0f14-202">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Новое веб-приложение ASP.NET Core](start-mvc/_static/config.png)


* <span data-ttu-id="d0f14-204">Выберите **Веб-приложение (модель-представление-контроллер)** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="d0f14-204">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="d0f14-205">Диалоговое окно нового проекта, .NET Core в левой области, веб-узел ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d0f14-205">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="d0f14-206">В Visual Studio используется только что созданный вами шаблон по умолчанию для проекта MVC.</span><span class="sxs-lookup"><span data-stu-id="d0f14-206">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="d0f14-207">Чтобы приложение стало рабочим, осталось только указать имя проекта и выбрать несколько параметров.</span><span class="sxs-lookup"><span data-stu-id="d0f14-207">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="d0f14-208">Это простой начальный проект и хорошая стартовая точка.</span><span class="sxs-lookup"><span data-stu-id="d0f14-208">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d0f14-209">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d0f14-209">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="d0f14-210">Для работы с этим руководством требуется знание VS Code.</span><span class="sxs-lookup"><span data-stu-id="d0f14-210">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="d0f14-211">Дополнительные сведения см. в разделах [Начало работы с VS Code](https://code.visualstudio.com/docs) и [Справка по Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="d0f14-211">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="d0f14-212">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="d0f14-212">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="d0f14-213">Смените каталог (`cd`) на папку, в которой будет содержаться проект.</span><span class="sxs-lookup"><span data-stu-id="d0f14-213">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="d0f14-214">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="d0f14-214">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="d0f14-215">Появится диалоговое окно с предупреждением **В MvcMovie отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="d0f14-215">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="d0f14-216">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="d0f14-216">Select **Yes**</span></span>

  * <span data-ttu-id="d0f14-217">`dotnet new mvc -o MvcMovie`: создает новый проект MVC ASP.NET Core в папке *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="d0f14-217">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="d0f14-218">`code -r MvcMovie`. загружает файл проекта *MvcMovie.csproj* в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d0f14-218">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d0f14-219">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="d0f14-219">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="d0f14-220">Щелкните **Файл** > **Новое решение**.</span><span class="sxs-lookup"><span data-stu-id="d0f14-220">Select **File** > **New Solution**.</span></span>

  ![Новое решение macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="d0f14-222">Щелкните **.NET Core** > **Приложение** > **Веб-приложение (Модель — представление — контроллер)** > **Далее**.</span><span class="sxs-lookup"><span data-stu-id="d0f14-222">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![Диалоговое окно "Новый проект" в macOS](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="d0f14-224">В диалоговом окне **Настройка нового веб-API ASP.NET Core** оставьте установленное по умолчанию значение для параметра **Целевая платформа**, то есть **.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="d0f14-224">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![Выбор .NET Core 2.2 для macOS](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="d0f14-226">Присвойте проекту имя **MvcMovie** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="d0f14-226">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="d0f14-227">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="d0f14-227">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d0f14-228">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d0f14-228">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d0f14-229">Нажмите **CTRl+F5**, чтобы запустить приложение без отладки.</span><span class="sxs-lookup"><span data-stu-id="d0f14-229">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="d0f14-230">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение.</span><span class="sxs-lookup"><span data-stu-id="d0f14-230">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="d0f14-231">Обратите внимание на то, что в адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="d0f14-231">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="d0f14-232">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="d0f14-232">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="d0f14-233">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="d0f14-233">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="d0f14-234">Запуск приложения с помощью клавиш CTRL+F5 (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="d0f14-234">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="d0f14-235">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.</span><span class="sxs-lookup"><span data-stu-id="d0f14-235">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="d0f14-236">Из меню **Отладка** можно запустить приложение в режиме с отладкой или без.</span><span class="sxs-lookup"><span data-stu-id="d0f14-236">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Меню отладки](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="d0f14-238">Чтобы выполнить отладку приложения, нажмите кнопку **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="d0f14-238">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="d0f14-240">Нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="d0f14-240">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="d0f14-241">Это приложение не отслеживает персональные данные.</span><span class="sxs-lookup"><span data-stu-id="d0f14-241">This app doesn't track personal information.</span></span> <span data-ttu-id="d0f14-242">Созданный шаблоном код включает ресурсы для соблюдения [Общего регламента по защите данных (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="d0f14-242">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/privacy.png)

  <span data-ttu-id="d0f14-244">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="d0f14-244">The following image shows the app after accepting tracking:</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d0f14-246">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d0f14-246">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="d0f14-247">Нажмите клавиши CTRL+F5, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="d0f14-247">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="d0f14-248">Visual Studio Code запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d0f14-248">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="d0f14-249">В адресной строке указывается `localhost:port:5001`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="d0f14-249">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="d0f14-250">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="d0f14-250">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="d0f14-251">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="d0f14-251">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="d0f14-252">Запуск приложения с помощью клавиш CTRL+F5 (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="d0f14-252">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="d0f14-253">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро обновлять страницу и просматривать изменения.</span><span class="sxs-lookup"><span data-stu-id="d0f14-253">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="d0f14-254">Нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="d0f14-254">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="d0f14-255">Это приложение не отслеживает персональные данные.</span><span class="sxs-lookup"><span data-stu-id="d0f14-255">This app doesn't track personal information.</span></span> <span data-ttu-id="d0f14-256">Созданный шаблоном код включает ресурсы для соблюдения [Общего регламента по защите данных (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="d0f14-256">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/privacy.png)

  <span data-ttu-id="d0f14-258">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="d0f14-258">The following image shows the app after accepting tracking:</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d0f14-260">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="d0f14-260">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="d0f14-261">Выберите **Выполнить** > **Запуск без отладки**, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="d0f14-261">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="d0f14-262">Visual Studio для Mac запустит сервер [Kestrel](xref:fundamentals/servers/index#kestrel), откроет браузер и перейдет к `http://localhost:port`, где *port* — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="d0f14-262">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="d0f14-263">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="d0f14-263">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="d0f14-264">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="d0f14-264">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="d0f14-265">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="d0f14-265">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="d0f14-266">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="d0f14-266">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="d0f14-267">В меню **Запуск** можно запустить приложение в режиме с отладкой или без нее.</span><span class="sxs-lookup"><span data-stu-id="d0f14-267">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="d0f14-268">Нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="d0f14-268">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="d0f14-269">Это приложение не отслеживает персональные данные.</span><span class="sxs-lookup"><span data-stu-id="d0f14-269">This app doesn't track personal information.</span></span> <span data-ttu-id="d0f14-270">Созданный шаблоном код включает ресурсы для соблюдения [Общего регламента по защите данных (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="d0f14-270">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="d0f14-272">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="d0f14-272">The following image shows the app after accepting tracking:</span></span>

  ![Домашняя или индексная страница](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="d0f14-274">В следующей части этого учебника мы поговорим об MVC и приступим к написанию кода.</span><span class="sxs-lookup"><span data-stu-id="d0f14-274">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d0f14-275">Вперед</span><span class="sxs-lookup"><span data-stu-id="d0f14-275">Next</span></span>](adding-controller.md)

::: moniker-end
