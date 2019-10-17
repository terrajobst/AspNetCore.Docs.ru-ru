---
title: Начало работы с MVC ASP.NET Core
author: rick-anderson
description: Сведения о начале работы с MVC ASP.NET Core.
ms.author: riande
ms.date: 10/16/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: f07afb15c0a31110c20d96a5db5c02030cefe5d5
ms.sourcegitcommit: e71b6a85b0e94a600af607107e298f932924c849
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/17/2019
ms.locfileid: "72519108"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="774f8-103">Начало работы с MVC ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="774f8-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="774f8-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="774f8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="774f8-105">В этом учебнике приводятся основные сведения о веб-приложении MVC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="774f8-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="774f8-106">Это приложение служит для управления базой данных названий фильмов.</span><span class="sxs-lookup"><span data-stu-id="774f8-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="774f8-107">Вы научитесь:</span><span class="sxs-lookup"><span data-stu-id="774f8-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="774f8-108">Создание веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="774f8-108">Create a web app.</span></span>
> * <span data-ttu-id="774f8-109">Добавление модели и формирование шаблона.</span><span class="sxs-lookup"><span data-stu-id="774f8-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="774f8-110">Работа с базой данных.</span><span class="sxs-lookup"><span data-stu-id="774f8-110">Work with a database.</span></span>
> * <span data-ttu-id="774f8-111">Добавление поиска и проверки.</span><span class="sxs-lookup"><span data-stu-id="774f8-111">Add search and validation.</span></span>

<span data-ttu-id="774f8-112">В конечном итоге вы получите приложение, позволяющее управлять данными фильмов и отображать их.</span><span class="sxs-lookup"><span data-stu-id="774f8-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="774f8-113">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="774f8-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="774f8-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="774f8-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="774f8-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="774f8-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="774f8-116">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="774f8-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app"></a><span data-ttu-id="774f8-117">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="774f8-117">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="774f8-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="774f8-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="774f8-119">В Visual Studio выберите **Создать проект**.</span><span class="sxs-lookup"><span data-stu-id="774f8-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="774f8-120">Выберите пункт **Веб-приложение ASP.NET Core** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="774f8-120">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Новое веб-приложение ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="774f8-122">Присвойте проекту имя **MvcMovie** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="774f8-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="774f8-123">Имя **MvcMovie** необходимо присвоить для того, чтобы при копировании кода пространства имен совпали.</span><span class="sxs-lookup"><span data-stu-id="774f8-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Новое веб-приложение ASP.NET Core](start-mvc/_static/config.png)

* <span data-ttu-id="774f8-125">Выберите **Веб-приложение (модель-представление-контроллер)** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="774f8-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="774f8-126">Диалоговое окно нового проекта, .NET Core в левой области, веб-узел ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="774f8-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project30.png)

<span data-ttu-id="774f8-127">В Visual Studio используется только что созданный вами шаблон по умолчанию для проекта MVC.</span><span class="sxs-lookup"><span data-stu-id="774f8-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="774f8-128">Чтобы приложение стало рабочим, осталось только указать имя проекта и выбрать несколько параметров.</span><span class="sxs-lookup"><span data-stu-id="774f8-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="774f8-129">Это простой начальный проект.</span><span class="sxs-lookup"><span data-stu-id="774f8-129">This is a basic starter project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="774f8-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="774f8-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="774f8-131">Для работы с этим руководством требуется знание VS Code.</span><span class="sxs-lookup"><span data-stu-id="774f8-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="774f8-132">Дополнительные сведения см. в разделах [Начало работы с VS Code](https://code.visualstudio.com/docs) и [Справка по Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="774f8-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="774f8-133">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="774f8-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="774f8-134">Смените каталог (`cd`) на папку, в которой будет содержаться проект.</span><span class="sxs-lookup"><span data-stu-id="774f8-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="774f8-135">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="774f8-135">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="774f8-136">Появится диалоговое окно с предупреждением **В MvcMovie отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="774f8-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="774f8-137">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="774f8-137">Select **Yes**</span></span>

  * <span data-ttu-id="774f8-138">`dotnet new mvc -o MvcMovie`: создает новый проект MVC ASP.NET Core в папке *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="774f8-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="774f8-139">`code -r MvcMovie`. загружает файл проекта *MvcMovie.csproj* в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="774f8-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="774f8-140">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="774f8-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="774f8-141">Выберите **Файл** > **Новое решение**.</span><span class="sxs-lookup"><span data-stu-id="774f8-141">Select **File** > **New Solution**.</span></span>

  ![Новое решение macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="774f8-143">Выберите **.NET Core** > **Приложение** > **Веб-приложение (модель — представление — контроллер)** > **Далее**.</span><span class="sxs-lookup"><span data-stu-id="774f8-143">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![Диалоговое окно "Новый проект" в macOS](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="774f8-145">В диалоговом окне **Настройка нового веб-API ASP.NET Core** установите для параметра **Целевая платформа** значение **.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="774f8-145">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** of **.NET Core 3.0**.</span></span>

<!-- 
  ![macOS .NET Core 2.2 selection](./start-mvc/_static/new_project_22_vsmac.png)
-->

* <span data-ttu-id="774f8-146">Присвойте проекту имя **MvcMovie** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="774f8-146">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="774f8-147">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="774f8-147">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="774f8-148">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="774f8-148">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="774f8-149">Нажмите **CTRl+F5**, чтобы запустить приложение без отладки.</span><span class="sxs-lookup"><span data-stu-id="774f8-149">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="774f8-150">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение.</span><span class="sxs-lookup"><span data-stu-id="774f8-150">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="774f8-151">Обратите внимание на то, что в адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="774f8-151">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="774f8-152">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="774f8-152">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="774f8-153">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="774f8-153">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="774f8-154">Запуск приложения с помощью клавиш CTRL+F5 (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="774f8-154">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="774f8-155">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.</span><span class="sxs-lookup"><span data-stu-id="774f8-155">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="774f8-156">Из меню **Отладка** можно запустить приложение в режиме с отладкой или без.</span><span class="sxs-lookup"><span data-stu-id="774f8-156">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Меню отладки](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="774f8-158">Чтобы выполнить отладку приложения, нажмите кнопку **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="774f8-158">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

  <span data-ttu-id="774f8-160">Пример приложения приведен на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="774f8-160">The following image shows the app:</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="774f8-162">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="774f8-162">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="774f8-163">Нажмите клавиши CTRL+F5, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="774f8-163">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="774f8-164">Visual Studio Code запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="774f8-164">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="774f8-165">В адресной строке указывается `localhost:port:5001`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="774f8-165">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="774f8-166">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="774f8-166">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="774f8-167">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="774f8-167">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="774f8-168">Запуск приложения с помощью клавиш CTRL+F5 (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="774f8-168">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="774f8-169">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро обновлять страницу и просматривать изменения.</span><span class="sxs-lookup"><span data-stu-id="774f8-169">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="774f8-171">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="774f8-171">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="774f8-172">Выберите **Выполнить** > **Запуск без отладки**, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="774f8-172">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="774f8-173">Visual Studio для Mac запустит сервер [Kestrel](xref:fundamentals/servers/index#kestrel), откроет браузер и перейдет к `http://localhost:port`, где *port* — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="774f8-173">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="774f8-174">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="774f8-174">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="774f8-175">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="774f8-175">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="774f8-176">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="774f8-176">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="774f8-177">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="774f8-177">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="774f8-178">В меню **Запуск** можно запустить приложение в режиме с отладкой или без нее.</span><span class="sxs-lookup"><span data-stu-id="774f8-178">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

  <span data-ttu-id="774f8-179">Пример приложения приведен на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="774f8-179">The following image shows the app:</span></span>

  ![Домашняя или индексная страница](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="774f8-181">В следующей части этого учебника мы поговорим об MVC и приступим к написанию кода.</span><span class="sxs-lookup"><span data-stu-id="774f8-181">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="774f8-182">Вперед</span><span class="sxs-lookup"><span data-stu-id="774f8-182">Next</span></span>](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="774f8-183">В этом учебнике приводятся основные сведения о веб-приложении MVC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="774f8-183">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="774f8-184">Это приложение служит для управления базой данных названий фильмов.</span><span class="sxs-lookup"><span data-stu-id="774f8-184">The app manages a database of movie titles.</span></span> <span data-ttu-id="774f8-185">Вы научитесь:</span><span class="sxs-lookup"><span data-stu-id="774f8-185">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="774f8-186">Создание веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="774f8-186">Create a web app.</span></span>
> * <span data-ttu-id="774f8-187">Добавление модели и формирование шаблона.</span><span class="sxs-lookup"><span data-stu-id="774f8-187">Add and scaffold a model.</span></span>
> * <span data-ttu-id="774f8-188">Работа с базой данных.</span><span class="sxs-lookup"><span data-stu-id="774f8-188">Work with a database.</span></span>
> * <span data-ttu-id="774f8-189">Добавление поиска и проверки.</span><span class="sxs-lookup"><span data-stu-id="774f8-189">Add search and validation.</span></span>

<span data-ttu-id="774f8-190">В конечном итоге вы получите приложение, позволяющее управлять данными фильмов и отображать их.</span><span class="sxs-lookup"><span data-stu-id="774f8-190">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="774f8-191">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="774f8-191">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="774f8-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="774f8-192">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="774f8-193">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="774f8-193">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="774f8-194">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="774f8-194">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="774f8-195">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="774f8-195">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="774f8-196">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="774f8-196">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="774f8-197">В Visual Studio выберите **Создать проект**.</span><span class="sxs-lookup"><span data-stu-id="774f8-197">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="774f8-198">Выберите пункт **Веб-приложение ASP.NET Core** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="774f8-198">Selecct **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Новое веб-приложение ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="774f8-200">Присвойте проекту имя **MvcMovie** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="774f8-200">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="774f8-201">Имя **MvcMovie** необходимо присвоить для того, чтобы при копировании кода пространства имен совпали.</span><span class="sxs-lookup"><span data-stu-id="774f8-201">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Новое веб-приложение ASP.NET Core](start-mvc/_static/config.png)


* <span data-ttu-id="774f8-203">Выберите **Веб-приложение (модель-представление-контроллер)** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="774f8-203">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="774f8-204">Диалоговое окно нового проекта, .NET Core в левой области, веб-узел ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="774f8-204">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="774f8-205">В Visual Studio используется только что созданный вами шаблон по умолчанию для проекта MVC.</span><span class="sxs-lookup"><span data-stu-id="774f8-205">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="774f8-206">Чтобы приложение стало рабочим, осталось только указать имя проекта и выбрать несколько параметров.</span><span class="sxs-lookup"><span data-stu-id="774f8-206">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="774f8-207">Это простой начальный проект и хорошая стартовая точка.</span><span class="sxs-lookup"><span data-stu-id="774f8-207">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="774f8-208">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="774f8-208">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="774f8-209">Для работы с этим руководством требуется знание VS Code.</span><span class="sxs-lookup"><span data-stu-id="774f8-209">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="774f8-210">Дополнительные сведения см. в разделах [Начало работы с VS Code](https://code.visualstudio.com/docs) и [Справка по Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="774f8-210">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="774f8-211">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="774f8-211">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="774f8-212">Смените каталог (`cd`) на папку, в которой будет содержаться проект.</span><span class="sxs-lookup"><span data-stu-id="774f8-212">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="774f8-213">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="774f8-213">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="774f8-214">Появится диалоговое окно с предупреждением **В MvcMovie отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="774f8-214">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="774f8-215">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="774f8-215">Select **Yes**</span></span>

  * <span data-ttu-id="774f8-216">`dotnet new mvc -o MvcMovie`: создает новый проект MVC ASP.NET Core в папке *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="774f8-216">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="774f8-217">`code -r MvcMovie`. загружает файл проекта *MvcMovie.csproj* в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="774f8-217">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="774f8-218">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="774f8-218">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="774f8-219">Выберите **Файл** > **Новое решение**.</span><span class="sxs-lookup"><span data-stu-id="774f8-219">Select **File** > **New Solution**.</span></span>

  ![Новое решение macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="774f8-221">Выберите **.NET Core** > **Приложение** > **Веб-приложение (модель — представление — контроллер)** > **Далее**.</span><span class="sxs-lookup"><span data-stu-id="774f8-221">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![Диалоговое окно "Новый проект" в macOS](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="774f8-223">В диалоговом окне **Настройка нового веб-API ASP.NET Core** оставьте установленное по умолчанию значение для параметра **Целевая платформа**, то есть **.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="774f8-223">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![Выбор .NET Core 2.2 для macOS](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="774f8-225">Присвойте проекту имя **MvcMovie** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="774f8-225">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="774f8-226">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="774f8-226">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="774f8-227">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="774f8-227">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="774f8-228">Нажмите **CTRl+F5**, чтобы запустить приложение без отладки.</span><span class="sxs-lookup"><span data-stu-id="774f8-228">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="774f8-229">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение.</span><span class="sxs-lookup"><span data-stu-id="774f8-229">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="774f8-230">Обратите внимание на то, что в адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="774f8-230">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="774f8-231">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="774f8-231">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="774f8-232">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="774f8-232">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="774f8-233">Запуск приложения с помощью клавиш CTRL+F5 (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="774f8-233">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="774f8-234">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.</span><span class="sxs-lookup"><span data-stu-id="774f8-234">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="774f8-235">Из меню **Отладка** можно запустить приложение в режиме с отладкой или без.</span><span class="sxs-lookup"><span data-stu-id="774f8-235">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Меню отладки](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="774f8-237">Чтобы выполнить отладку приложения, нажмите кнопку **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="774f8-237">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="774f8-239">Нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="774f8-239">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="774f8-240">Это приложение не отслеживает персональные данные.</span><span class="sxs-lookup"><span data-stu-id="774f8-240">This app doesn't track personal information.</span></span> <span data-ttu-id="774f8-241">Созданный шаблоном код включает ресурсы для соблюдения [Общего регламента по защите данных (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="774f8-241">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/privacy.png)

  <span data-ttu-id="774f8-243">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="774f8-243">The following image shows the app after accepting tracking:</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="774f8-245">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="774f8-245">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="774f8-246">Нажмите клавиши CTRL+F5, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="774f8-246">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="774f8-247">Visual Studio Code запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="774f8-247">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="774f8-248">В адресной строке указывается `localhost:port:5001`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="774f8-248">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="774f8-249">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="774f8-249">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="774f8-250">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="774f8-250">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="774f8-251">Запуск приложения с помощью клавиш CTRL+F5 (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="774f8-251">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="774f8-252">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро обновлять страницу и просматривать изменения.</span><span class="sxs-lookup"><span data-stu-id="774f8-252">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="774f8-253">Нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="774f8-253">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="774f8-254">Это приложение не отслеживает персональные данные.</span><span class="sxs-lookup"><span data-stu-id="774f8-254">This app doesn't track personal information.</span></span> <span data-ttu-id="774f8-255">Созданный шаблоном код включает ресурсы для соблюдения [Общего регламента по защите данных (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="774f8-255">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/privacy.png)

  <span data-ttu-id="774f8-257">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="774f8-257">The following image shows the app after accepting tracking:</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="774f8-259">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="774f8-259">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="774f8-260">Выберите **Выполнить** > **Запуск без отладки**, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="774f8-260">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="774f8-261">Visual Studio для Mac запустит сервер [Kestrel](xref:fundamentals/servers/index#kestrel), откроет браузер и перейдет к `http://localhost:port`, где *port* — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="774f8-261">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="774f8-262">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="774f8-262">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="774f8-263">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="774f8-263">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="774f8-264">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="774f8-264">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="774f8-265">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="774f8-265">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="774f8-266">В меню **Запуск** можно запустить приложение в режиме с отладкой или без нее.</span><span class="sxs-lookup"><span data-stu-id="774f8-266">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="774f8-267">Нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="774f8-267">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="774f8-268">Это приложение не отслеживает персональные данные.</span><span class="sxs-lookup"><span data-stu-id="774f8-268">This app doesn't track personal information.</span></span> <span data-ttu-id="774f8-269">Созданный шаблоном код включает ресурсы для соблюдения [Общего регламента по защите данных (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="774f8-269">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="774f8-271">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="774f8-271">The following image shows the app after accepting tracking:</span></span>

  ![Домашняя или индексная страница](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="774f8-273">В следующей части этого учебника мы поговорим об MVC и приступим к написанию кода.</span><span class="sxs-lookup"><span data-stu-id="774f8-273">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="774f8-274">Вперед</span><span class="sxs-lookup"><span data-stu-id="774f8-274">Next</span></span>](adding-controller.md)

::: moniker-end
