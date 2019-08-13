---
title: Начало работы с MVC ASP.NET Core
author: rick-anderson
description: Сведения о начале работы с MVC ASP.NET Core.
ms.author: riande
ms.date: 08/05/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: f6a92423546ebd9d4c8e1a92fb81b6b72f847f61
ms.sourcegitcommit: 2eb605f4f20ac4dd9de6c3b3e3453e108a357a21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/06/2019
ms.locfileid: "68820100"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="61aa6-103">Начало работы с MVC ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="61aa6-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="61aa6-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="61aa6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="61aa6-105">В этом учебнике приводятся основные сведения о веб-приложении MVC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="61aa6-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="61aa6-106">Это приложение служит для управления базой данных названий фильмов.</span><span class="sxs-lookup"><span data-stu-id="61aa6-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="61aa6-107">Вы научитесь:</span><span class="sxs-lookup"><span data-stu-id="61aa6-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="61aa6-108">Создание веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="61aa6-108">Create a web app.</span></span>
> * <span data-ttu-id="61aa6-109">Добавление модели и формирование шаблона.</span><span class="sxs-lookup"><span data-stu-id="61aa6-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="61aa6-110">Работа с базой данных.</span><span class="sxs-lookup"><span data-stu-id="61aa6-110">Work with a database.</span></span>
> * <span data-ttu-id="61aa6-111">Добавление поиска и проверки.</span><span class="sxs-lookup"><span data-stu-id="61aa6-111">Add search and validation.</span></span>

<span data-ttu-id="61aa6-112">В конечном итоге вы получите приложение, позволяющее управлять данными фильмов и отображать их.</span><span class="sxs-lookup"><span data-stu-id="61aa6-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="61aa6-113">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="61aa6-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="61aa6-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61aa6-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="61aa6-115">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="61aa6-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="61aa6-116">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="61aa6-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app"></a><span data-ttu-id="61aa6-117">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="61aa6-117">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="61aa6-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61aa6-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="61aa6-119">В Visual Studio выберите **Создать проект**.</span><span class="sxs-lookup"><span data-stu-id="61aa6-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="61aa6-120">Выберите пункт **Веб-приложение ASP.NET Core** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="61aa6-120">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Новое веб-приложение ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="61aa6-122">Присвойте проекту имя **MvcMovie** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="61aa6-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="61aa6-123">Имя **MvcMovie** необходимо присвоить для того, чтобы при копировании кода пространства имен совпали.</span><span class="sxs-lookup"><span data-stu-id="61aa6-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Новое веб-приложение ASP.NET Core](start-mvc/_static/config.png)

* <span data-ttu-id="61aa6-125">Выберите **Веб-приложение (модель-представление-контроллер)** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="61aa6-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="61aa6-126">Диалоговое окно нового проекта, .NET Core в левой области, веб-узел ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="61aa6-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="61aa6-127">В Visual Studio используется только что созданный вами шаблон по умолчанию для проекта MVC.</span><span class="sxs-lookup"><span data-stu-id="61aa6-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="61aa6-128">Чтобы приложение стало рабочим, осталось только указать имя проекта и выбрать несколько параметров.</span><span class="sxs-lookup"><span data-stu-id="61aa6-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="61aa6-129">Это простой начальный проект.</span><span class="sxs-lookup"><span data-stu-id="61aa6-129">This is a basic starter project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="61aa6-130">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="61aa6-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="61aa6-131">Для работы с этим руководством требуется знание VS Code.</span><span class="sxs-lookup"><span data-stu-id="61aa6-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="61aa6-132">Дополнительные сведения см. в разделах [Начало работы с VS Code](https://code.visualstudio.com/docs) и [Справка по Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="61aa6-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="61aa6-133">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="61aa6-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="61aa6-134">Смените каталог (`cd`) на папку, в которой будет содержаться проект.</span><span class="sxs-lookup"><span data-stu-id="61aa6-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="61aa6-135">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="61aa6-135">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="61aa6-136">Появится диалоговое окно с предупреждением **В MvcMovie отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="61aa6-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="61aa6-137">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="61aa6-137">Select **Yes**</span></span>

  * <span data-ttu-id="61aa6-138">`dotnet new mvc -o MvcMovie`: создает новый проект MVC ASP.NET Core в папке *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="61aa6-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="61aa6-139">`code -r MvcMovie`: загружает файл проекта *MvcMovie.csproj* в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="61aa6-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="61aa6-140">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="61aa6-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="61aa6-141">Выберите **Файл** > **Новое решение**.</span><span class="sxs-lookup"><span data-stu-id="61aa6-141">Select **File** > **New Solution**.</span></span>

  ![Новое решение macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="61aa6-143">Выберите **.NET Core** > **Приложение** > **Веб-приложение (модель — представление — контроллер)** > **Далее**.</span><span class="sxs-lookup"><span data-stu-id="61aa6-143">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![Диалоговое окно "Новый проект" в macOS](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="61aa6-145">В диалоговом окне **Настройка нового веб-API ASP.NET Core** установите для параметра **Целевая платформа** значение **.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="61aa6-145">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** of **.NET Core 3.0**.</span></span>

<!-- 
  ![macOS .NET Core 2.2 selection](./start-mvc/_static/new_project_22_vsmac.png)
-->

* <span data-ttu-id="61aa6-146">Присвойте проекту имя **MvcMovie** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="61aa6-146">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="61aa6-147">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="61aa6-147">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="61aa6-148">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61aa6-148">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="61aa6-149">Нажмите **CTRl+F5**, чтобы запустить приложение без отладки.</span><span class="sxs-lookup"><span data-stu-id="61aa6-149">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="61aa6-150">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение.</span><span class="sxs-lookup"><span data-stu-id="61aa6-150">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="61aa6-151">Обратите внимание на то, что в адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="61aa6-151">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="61aa6-152">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="61aa6-152">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="61aa6-153">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="61aa6-153">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="61aa6-154">Запуск приложения с помощью клавиш CTRL+F5 (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="61aa6-154">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="61aa6-155">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.</span><span class="sxs-lookup"><span data-stu-id="61aa6-155">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="61aa6-156">Из меню **Отладка** можно запустить приложение в режиме с отладкой или без.</span><span class="sxs-lookup"><span data-stu-id="61aa6-156">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Меню отладки](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="61aa6-158">Чтобы выполнить отладку приложения, нажмите кнопку **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="61aa6-158">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)


  <span data-ttu-id="61aa6-160">Пример приложения приведен на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="61aa6-160">The following image shows the app:</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="61aa6-162">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="61aa6-162">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="61aa6-163">Нажмите клавиши CTRL+F5, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="61aa6-163">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="61aa6-164">Visual Studio Code запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="61aa6-164">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="61aa6-165">В адресной строке указывается `localhost:port:5001`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="61aa6-165">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="61aa6-166">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="61aa6-166">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="61aa6-167">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="61aa6-167">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="61aa6-168">Запуск приложения с помощью клавиш CTRL+F5 (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="61aa6-168">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="61aa6-169">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро обновлять страницу и просматривать изменения.</span><span class="sxs-lookup"><span data-stu-id="61aa6-169">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="61aa6-170">Нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="61aa6-170">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="61aa6-171">Это приложение не отслеживает персональные данные.</span><span class="sxs-lookup"><span data-stu-id="61aa6-171">This app doesn't track personal information.</span></span> <span data-ttu-id="61aa6-172">Созданный шаблоном код включает ресурсы для соблюдения [Общего регламента по защите данных (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="61aa6-172">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/privacy.png)

  <span data-ttu-id="61aa6-174">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="61aa6-174">The following image shows the app after accepting tracking:</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="61aa6-176">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="61aa6-176">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="61aa6-177">Выберите **Выполнить** > **Запуск без отладки**, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="61aa6-177">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="61aa6-178">Visual Studio для Mac запустит сервер [Kestrel](xref:fundamentals/servers/index#kestrel), откроет браузер и перейдет к `http://localhost:port`, где *port* — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="61aa6-178">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="61aa6-179">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="61aa6-179">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="61aa6-180">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="61aa6-180">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="61aa6-181">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="61aa6-181">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="61aa6-182">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="61aa6-182">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="61aa6-183">В меню **Запуск** можно запустить приложение в режиме с отладкой или без нее.</span><span class="sxs-lookup"><span data-stu-id="61aa6-183">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

  <span data-ttu-id="61aa6-184">Пример приложения приведен на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="61aa6-184">The following image shows the app:</span></span>

  ![Домашняя или индексная страница](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="61aa6-186">В следующей части этого учебника мы поговорим об MVC и приступим к написанию кода.</span><span class="sxs-lookup"><span data-stu-id="61aa6-186">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="61aa6-187">Вперед</span><span class="sxs-lookup"><span data-stu-id="61aa6-187">Next</span></span>](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="61aa6-188">В этом учебнике приводятся основные сведения о веб-приложении MVC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="61aa6-188">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="61aa6-189">Это приложение служит для управления базой данных названий фильмов.</span><span class="sxs-lookup"><span data-stu-id="61aa6-189">The app manages a database of movie titles.</span></span> <span data-ttu-id="61aa6-190">Вы научитесь:</span><span class="sxs-lookup"><span data-stu-id="61aa6-190">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="61aa6-191">Создание веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="61aa6-191">Create a web app.</span></span>
> * <span data-ttu-id="61aa6-192">Добавление модели и формирование шаблона.</span><span class="sxs-lookup"><span data-stu-id="61aa6-192">Add and scaffold a model.</span></span>
> * <span data-ttu-id="61aa6-193">Работа с базой данных.</span><span class="sxs-lookup"><span data-stu-id="61aa6-193">Work with a database.</span></span>
> * <span data-ttu-id="61aa6-194">Добавление поиска и проверки.</span><span class="sxs-lookup"><span data-stu-id="61aa6-194">Add search and validation.</span></span>

<span data-ttu-id="61aa6-195">В конечном итоге вы получите приложение, позволяющее управлять данными фильмов и отображать их.</span><span class="sxs-lookup"><span data-stu-id="61aa6-195">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="61aa6-196">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="61aa6-196">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="61aa6-197">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61aa6-197">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="61aa6-198">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="61aa6-198">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="61aa6-199">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="61aa6-199">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="61aa6-200">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="61aa6-200">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="61aa6-201">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61aa6-201">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="61aa6-202">В Visual Studio выберите **Создать проект**.</span><span class="sxs-lookup"><span data-stu-id="61aa6-202">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="61aa6-203">Выберите пункт **Веб-приложение ASP.NET Core** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="61aa6-203">Selecct **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Новое веб-приложение ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="61aa6-205">Присвойте проекту имя **MvcMovie** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="61aa6-205">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="61aa6-206">Имя **MvcMovie** необходимо присвоить для того, чтобы при копировании кода пространства имен совпали.</span><span class="sxs-lookup"><span data-stu-id="61aa6-206">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Новое веб-приложение ASP.NET Core](start-mvc/_static/config.png)


* <span data-ttu-id="61aa6-208">Выберите **Веб-приложение (модель-представление-контроллер)** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="61aa6-208">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="61aa6-209">Диалоговое окно нового проекта, .NET Core в левой области, веб-узел ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="61aa6-209">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="61aa6-210">В Visual Studio используется только что созданный вами шаблон по умолчанию для проекта MVC.</span><span class="sxs-lookup"><span data-stu-id="61aa6-210">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="61aa6-211">Чтобы приложение стало рабочим, осталось только указать имя проекта и выбрать несколько параметров.</span><span class="sxs-lookup"><span data-stu-id="61aa6-211">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="61aa6-212">Это простой начальный проект и хорошая стартовая точка.</span><span class="sxs-lookup"><span data-stu-id="61aa6-212">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="61aa6-213">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="61aa6-213">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="61aa6-214">Для работы с этим руководством требуется знание VS Code.</span><span class="sxs-lookup"><span data-stu-id="61aa6-214">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="61aa6-215">Дополнительные сведения см. в разделах [Начало работы с VS Code](https://code.visualstudio.com/docs) и [Справка по Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="61aa6-215">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="61aa6-216">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="61aa6-216">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="61aa6-217">Смените каталог (`cd`) на папку, в которой будет содержаться проект.</span><span class="sxs-lookup"><span data-stu-id="61aa6-217">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="61aa6-218">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="61aa6-218">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="61aa6-219">Появится диалоговое окно с предупреждением **В MvcMovie отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="61aa6-219">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="61aa6-220">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="61aa6-220">Select **Yes**</span></span>

  * <span data-ttu-id="61aa6-221">`dotnet new mvc -o MvcMovie`: создает новый проект MVC ASP.NET Core в папке *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="61aa6-221">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="61aa6-222">`code -r MvcMovie`: загружает файл проекта *MvcMovie.csproj* в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="61aa6-222">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="61aa6-223">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="61aa6-223">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="61aa6-224">Выберите **Файл** > **Новое решение**.</span><span class="sxs-lookup"><span data-stu-id="61aa6-224">Select **File** > **New Solution**.</span></span>

  ![Новое решение macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="61aa6-226">Выберите **.NET Core** > **Приложение** > **Веб-приложение (модель — представление — контроллер)** > **Далее**.</span><span class="sxs-lookup"><span data-stu-id="61aa6-226">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![Диалоговое окно "Новый проект" в macOS](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="61aa6-228">В диалоговом окне **Настройка нового веб-API ASP.NET Core** оставьте установленное по умолчанию значение для параметра **Целевая платформа**, то есть **.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="61aa6-228">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![Выбор .NET Core 2.2 для macOS](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="61aa6-230">Присвойте проекту имя **MvcMovie** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="61aa6-230">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="61aa6-231">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="61aa6-231">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="61aa6-232">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61aa6-232">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="61aa6-233">Нажмите **CTRl+F5**, чтобы запустить приложение без отладки.</span><span class="sxs-lookup"><span data-stu-id="61aa6-233">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="61aa6-234">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение.</span><span class="sxs-lookup"><span data-stu-id="61aa6-234">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="61aa6-235">Обратите внимание на то, что в адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="61aa6-235">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="61aa6-236">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="61aa6-236">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="61aa6-237">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="61aa6-237">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="61aa6-238">Запуск приложения с помощью клавиш CTRL+F5 (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="61aa6-238">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="61aa6-239">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.</span><span class="sxs-lookup"><span data-stu-id="61aa6-239">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="61aa6-240">Из меню **Отладка** можно запустить приложение в режиме с отладкой или без.</span><span class="sxs-lookup"><span data-stu-id="61aa6-240">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Меню отладки](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="61aa6-242">Чтобы выполнить отладку приложения, нажмите кнопку **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="61aa6-242">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="61aa6-244">Нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="61aa6-244">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="61aa6-245">Это приложение не отслеживает персональные данные.</span><span class="sxs-lookup"><span data-stu-id="61aa6-245">This app doesn't track personal information.</span></span> <span data-ttu-id="61aa6-246">Созданный шаблоном код включает ресурсы для соблюдения [Общего регламента по защите данных (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="61aa6-246">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/privacy.png)

  <span data-ttu-id="61aa6-248">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="61aa6-248">The following image shows the app after accepting tracking:</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="61aa6-250">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="61aa6-250">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="61aa6-251">Нажмите клавиши CTRL+F5, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="61aa6-251">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="61aa6-252">Visual Studio Code запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="61aa6-252">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="61aa6-253">В адресной строке указывается `localhost:port:5001`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="61aa6-253">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="61aa6-254">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="61aa6-254">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="61aa6-255">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="61aa6-255">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="61aa6-256">Запуск приложения с помощью клавиш CTRL+F5 (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="61aa6-256">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="61aa6-257">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро обновлять страницу и просматривать изменения.</span><span class="sxs-lookup"><span data-stu-id="61aa6-257">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="61aa6-258">Нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="61aa6-258">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="61aa6-259">Это приложение не отслеживает персональные данные.</span><span class="sxs-lookup"><span data-stu-id="61aa6-259">This app doesn't track personal information.</span></span> <span data-ttu-id="61aa6-260">Созданный шаблоном код включает ресурсы для соблюдения [Общего регламента по защите данных (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="61aa6-260">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/privacy.png)

  <span data-ttu-id="61aa6-262">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="61aa6-262">The following image shows the app after accepting tracking:</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="61aa6-264">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="61aa6-264">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="61aa6-265">Выберите **Выполнить** > **Запуск без отладки**, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="61aa6-265">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="61aa6-266">Visual Studio для Mac запустит сервер [Kestrel](xref:fundamentals/servers/index#kestrel), откроет браузер и перейдет к `http://localhost:port`, где *port* — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="61aa6-266">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="61aa6-267">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="61aa6-267">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="61aa6-268">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="61aa6-268">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="61aa6-269">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="61aa6-269">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="61aa6-270">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="61aa6-270">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="61aa6-271">В меню **Запуск** можно запустить приложение в режиме с отладкой или без нее.</span><span class="sxs-lookup"><span data-stu-id="61aa6-271">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="61aa6-272">Нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="61aa6-272">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="61aa6-273">Это приложение не отслеживает персональные данные.</span><span class="sxs-lookup"><span data-stu-id="61aa6-273">This app doesn't track personal information.</span></span> <span data-ttu-id="61aa6-274">Созданный шаблоном код включает ресурсы для соблюдения [Общего регламента по защите данных (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="61aa6-274">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="61aa6-276">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="61aa6-276">The following image shows the app after accepting tracking:</span></span>

  ![Домашняя или индексная страница](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="61aa6-278">В следующей части этого учебника мы поговорим об MVC и приступим к написанию кода.</span><span class="sxs-lookup"><span data-stu-id="61aa6-278">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="61aa6-279">Вперед</span><span class="sxs-lookup"><span data-stu-id="61aa6-279">Next</span></span>](adding-controller.md)

::: moniker-end
