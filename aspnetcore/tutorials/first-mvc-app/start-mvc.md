---
title: Начало работы с MVC ASP.NET Core
author: rick-anderson
description: Сведения о начале работы с MVC ASP.NET Core.
ms.author: riande
ms.date: 04/24/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: dc3499c89860190b76d6be7b8abeeaef827880d6
ms.sourcegitcommit: a1364109d11d414121a6337b611bee61d6e489e9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/04/2019
ms.locfileid: "66491252"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="fab59-103">Начало работы с MVC ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fab59-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="fab59-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="fab59-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="fab59-105">В этом учебнике приводятся основные сведения о веб-приложении MVC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fab59-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="fab59-106">Это приложение служит для управления базой данных названий фильмов.</span><span class="sxs-lookup"><span data-stu-id="fab59-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="fab59-107">Вы научитесь:</span><span class="sxs-lookup"><span data-stu-id="fab59-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fab59-108">Создание веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="fab59-108">Create a web app.</span></span>
> * <span data-ttu-id="fab59-109">Добавление модели и формирование шаблона.</span><span class="sxs-lookup"><span data-stu-id="fab59-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="fab59-110">Работа с базой данных.</span><span class="sxs-lookup"><span data-stu-id="fab59-110">Work with a database.</span></span>
> * <span data-ttu-id="fab59-111">Добавление поиска и проверки.</span><span class="sxs-lookup"><span data-stu-id="fab59-111">Add search and validation.</span></span>

<span data-ttu-id="fab59-112">В конечном итоге вы получите приложение, позволяющее управлять данными фильмов и отображать их.</span><span class="sxs-lookup"><span data-stu-id="fab59-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="fab59-113">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="fab59-113">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fab59-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fab59-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="fab59-115">В Visual Studio выберите **Создать проект**.</span><span class="sxs-lookup"><span data-stu-id="fab59-115">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="fab59-116">Выберите пункт **Веб-приложение ASP.NET Core** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="fab59-116">Selecct **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Новое веб-приложение ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="fab59-118">Присвойте проекту имя **MvcMovie** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="fab59-118">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="fab59-119">Имя **MvcMovie** необходимо присвоить для того, чтобы при копировании кода пространства имен совпали.</span><span class="sxs-lookup"><span data-stu-id="fab59-119">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Новое веб-приложение ASP.NET Core](start-mvc/_static/config.png)


* <span data-ttu-id="fab59-121">Выберите **Веб-приложение (модель-представление-контроллер)** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="fab59-121">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="fab59-122">Диалоговое окно нового проекта, .NET Core в левой области, веб-узел ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fab59-122">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="fab59-123">В Visual Studio используется только что созданный вами шаблон по умолчанию для проекта MVC.</span><span class="sxs-lookup"><span data-stu-id="fab59-123">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="fab59-124">Чтобы приложение стало рабочим, осталось только указать имя проекта и выбрать несколько параметров.</span><span class="sxs-lookup"><span data-stu-id="fab59-124">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="fab59-125">Это простой начальный проект и хорошая стартовая точка.</span><span class="sxs-lookup"><span data-stu-id="fab59-125">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fab59-126">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="fab59-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="fab59-127">Для работы с этим руководством требуется знание VS Code.</span><span class="sxs-lookup"><span data-stu-id="fab59-127">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="fab59-128">Дополнительные сведения см. в разделах [Начало работы с VS Code](https://code.visualstudio.com/docs) и [Справка по Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="fab59-128">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="fab59-129">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="fab59-129">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="fab59-130">Смените каталог (`cd`) на папку, в которой будет содержаться проект.</span><span class="sxs-lookup"><span data-stu-id="fab59-130">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="fab59-131">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="fab59-131">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="fab59-132">Появится диалоговое окно с предупреждением **В MvcMovie отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="fab59-132">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="fab59-133">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="fab59-133">Select **Yes**</span></span>

  * <span data-ttu-id="fab59-134">`dotnet new mvc -o MvcMovie`: создает новый проект MVC ASP.NET Core в папке *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="fab59-134">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="fab59-135">`code -r MvcMovie`: загружает файл проекта *MvcMovie.csproj* в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="fab59-135">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fab59-136">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="fab59-136">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="fab59-137">Выберите **Файл** > **Новое решение**.</span><span class="sxs-lookup"><span data-stu-id="fab59-137">Select **File** > **New Solution**.</span></span>

  ![Новое решение macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="fab59-139">Выберите **.NET Core** > **Приложение** > **Веб-приложение (модель — представление — контроллер)**  > **Далее**.</span><span class="sxs-lookup"><span data-stu-id="fab59-139">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![Диалоговое окно "Новый проект" в macOS](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="fab59-141">В диалоговом окне **Настройка нового веб-API ASP.NET Core** оставьте установленное по умолчанию значение для параметра **Целевая платформа**, то есть **.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="fab59-141">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![Выбор .NET Core 2.2 для macOS](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="fab59-143">Присвойте проекту имя **MvcMovie** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="fab59-143">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="fab59-144">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="fab59-144">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fab59-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fab59-145">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fab59-146">Нажмите **CTRl+F5**, чтобы запустить приложение без отладки.</span><span class="sxs-lookup"><span data-stu-id="fab59-146">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="fab59-147">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение.</span><span class="sxs-lookup"><span data-stu-id="fab59-147">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="fab59-148">Обратите внимание на то, что в адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="fab59-148">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="fab59-149">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="fab59-149">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="fab59-150">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="fab59-150">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="fab59-151">Запуск приложения с помощью клавиш CTRL+F5 (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="fab59-151">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="fab59-152">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.</span><span class="sxs-lookup"><span data-stu-id="fab59-152">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="fab59-153">Из меню **Отладка** можно запустить приложение в режиме с отладкой или без.</span><span class="sxs-lookup"><span data-stu-id="fab59-153">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Меню отладки](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="fab59-155">Чтобы выполнить отладку приложения, нажмите кнопку **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="fab59-155">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="fab59-157">Нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="fab59-157">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="fab59-158">Это приложение не отслеживает персональные данные.</span><span class="sxs-lookup"><span data-stu-id="fab59-158">This app doesn't track personal information.</span></span> <span data-ttu-id="fab59-159">Созданный шаблоном код включает ресурсы для соблюдения [Общего регламента по защите данных (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="fab59-159">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/privacy.png)

  <span data-ttu-id="fab59-161">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="fab59-161">The following image shows the app after accepting tracking:</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fab59-163">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="fab59-163">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="fab59-164">Нажмите клавиши CTRL+F5, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="fab59-164">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="fab59-165">Visual Studio Code запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="fab59-165">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="fab59-166">В адресной строке указывается `localhost:port:5001`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="fab59-166">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="fab59-167">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="fab59-167">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="fab59-168">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="fab59-168">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="fab59-169">Запуск приложения с помощью клавиш CTRL+F5 (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="fab59-169">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="fab59-170">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро обновлять страницу и просматривать изменения.</span><span class="sxs-lookup"><span data-stu-id="fab59-170">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="fab59-171">Нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="fab59-171">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="fab59-172">Это приложение не отслеживает персональные данные.</span><span class="sxs-lookup"><span data-stu-id="fab59-172">This app doesn't track personal information.</span></span> <span data-ttu-id="fab59-173">Созданный шаблоном код включает ресурсы для соблюдения [Общего регламента по защите данных (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="fab59-173">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/privacy.png)

  <span data-ttu-id="fab59-175">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="fab59-175">The following image shows the app after accepting tracking:</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fab59-177">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="fab59-177">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="fab59-178">Выберите **Выполнить** > **Запуск без отладки**, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="fab59-178">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="fab59-179">Visual Studio для Mac запустит сервер [Kestrel](xref:fundamentals/servers/index#kestrel), откроет браузер и перейдет к `http://localhost:port`, где *port* — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="fab59-179">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="fab59-180">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="fab59-180">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="fab59-181">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="fab59-181">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="fab59-182">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="fab59-182">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="fab59-183">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="fab59-183">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="fab59-184">В меню **Запуск** можно запустить приложение в режиме с отладкой или без нее.</span><span class="sxs-lookup"><span data-stu-id="fab59-184">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="fab59-185">Нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="fab59-185">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="fab59-186">Это приложение не отслеживает персональные данные.</span><span class="sxs-lookup"><span data-stu-id="fab59-186">This app doesn't track personal information.</span></span> <span data-ttu-id="fab59-187">Созданный шаблоном код включает ресурсы для соблюдения [Общего регламента по защите данных (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="fab59-187">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="fab59-189">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="fab59-189">The following image shows the app after accepting tracking:</span></span>

  ![Домашняя или индексная страница](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="fab59-191">В следующей части этого учебника мы поговорим об MVC и приступим к написанию кода.</span><span class="sxs-lookup"><span data-stu-id="fab59-191">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="fab59-192">Вперед</span><span class="sxs-lookup"><span data-stu-id="fab59-192">Next</span></span>](adding-controller.md)
