---
title: Начало работы с MVC ASP.NET Core
author: rick-anderson
monikerRange: '>= aspnetcore-2.2'
description: Сведения о начале работы с MVC ASP.NET Core.
ms.author: riande
ms.date: 12/12/2018
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: cfce3b5792a5d0673bae5ddbba9e2d4d515a6279
ms.sourcegitcommit: 4e87712029de2aceb1cf2c52e9e3dda8195a5b8e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2018
ms.locfileid: "53381807"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="6a107-103">Начало работы с MVC ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6a107-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="6a107-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="6a107-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

https://docs.microsoft.com/en-us/visualstudio/ide/visual-studio-ide?view=vs-2017

<span data-ttu-id="6a107-105">В этом учебнике приводятся основные сведения о веб-приложении MVC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6a107-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="6a107-106">Это приложение служит для управления базой данных названий фильмов.</span><span class="sxs-lookup"><span data-stu-id="6a107-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="6a107-107">Вы научитесь:</span><span class="sxs-lookup"><span data-stu-id="6a107-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6a107-108">Создание веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="6a107-108">Create a web app.</span></span>
> * <span data-ttu-id="6a107-109">Добавление модели и формирование шаблона.</span><span class="sxs-lookup"><span data-stu-id="6a107-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="6a107-110">Работа с базой данных.</span><span class="sxs-lookup"><span data-stu-id="6a107-110">Work with a database.</span></span>
> * <span data-ttu-id="6a107-111">Добавление поиска и проверки.</span><span class="sxs-lookup"><span data-stu-id="6a107-111">Add search and validation.</span></span>

<span data-ttu-id="6a107-112">В конечном итоге вы получите приложение, позволяющее управлять данными фильмов и отображать их.</span><span class="sxs-lookup"><span data-stu-id="6a107-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

> [!NOTE]
> <span data-ttu-id="6a107-113">Мы тестируем удобство использования новой предлагаемой структуры оглавления для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6a107-113">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="6a107-114">Если у вас есть несколько минут, и вы хотите попробовать найти семь разных тем в существующей или планируемой структуре оглавления, [щелкните здесь, чтобы принять участие в исследовании](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span><span class="sxs-lookup"><span data-stu-id="6a107-114">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="6a107-115">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="6a107-115">Create a web app</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a107-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a107-116">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6a107-117">В Visual Studio выберите меню **Файл > Создать > Проект**.</span><span class="sxs-lookup"><span data-stu-id="6a107-117">From Visual Studio, select  **File > New > Project**.</span></span>

![Файл > Создать > Проект](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="6a107-119">В диалоговом окне **Создание проекта** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="6a107-119">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="6a107-120">В левой области выберите **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="6a107-120">In the left pane, select **.NET Core**</span></span>
* <span data-ttu-id="6a107-121">В центральной области выберите **Веб-приложение ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="6a107-121">In the center pane, select **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="6a107-122">Назовите проект "MvcMovie" (имя "MvcMovie" необходимо присвоить для того, чтобы при копировании кода пространства имен совпали).</span><span class="sxs-lookup"><span data-stu-id="6a107-122">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="6a107-123">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="6a107-123">select **OK**</span></span>

![<span data-ttu-id="6a107-124">Диалоговое окно нового проекта, .NET Core в левой области, веб-узел ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6a107-124">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="6a107-125">Заполните данные в диалоговом окне **Создание веб-приложения ASP.NET Core (.NET Core) — MvcMovie**.</span><span class="sxs-lookup"><span data-stu-id="6a107-125">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="6a107-126">Выберите в раскрывающемся списке выбора версии **ASP.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="6a107-126">In the version selector drop-down box select **ASP.NET Core 2.2**</span></span>
* <span data-ttu-id="6a107-127">Выберите **Веб-приложение (модель — представление — контроллер)**</span><span class="sxs-lookup"><span data-stu-id="6a107-127">Select **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="6a107-128">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="6a107-128">select **OK**.</span></span>

![<span data-ttu-id="6a107-129">Диалоговое окно нового проекта, .NET Core в левой области, веб-узел ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6a107-129">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="6a107-130">В Visual Studio используется только что созданный вами шаблон по умолчанию для проекта MVC.</span><span class="sxs-lookup"><span data-stu-id="6a107-130">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="6a107-131">Чтобы приложение стало рабочим, осталось только указать имя проекта и выбрать несколько параметров.</span><span class="sxs-lookup"><span data-stu-id="6a107-131">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="6a107-132">Это простой начальный проект и хорошая стартовая точка.</span><span class="sxs-lookup"><span data-stu-id="6a107-132">This is a basic starter project, and it's a good place to start.</span></span>

<span data-ttu-id="6a107-133">Нажмите **CTRl+F5**, чтобы запустить приложение без отладки.</span><span class="sxs-lookup"><span data-stu-id="6a107-133">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

* <span data-ttu-id="6a107-134">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем ваше приложение.</span><span class="sxs-lookup"><span data-stu-id="6a107-134">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="6a107-135">Обратите внимание на то, что в адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="6a107-135">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="6a107-136">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="6a107-136">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="6a107-137">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="6a107-137">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="6a107-138">На представленном выше снимке экрана используется порт номер 5000.</span><span class="sxs-lookup"><span data-stu-id="6a107-138">In the image above, the port number is 5000.</span></span> <span data-ttu-id="6a107-139">URL-адрес в браузере отображает `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="6a107-139">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="6a107-140">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="6a107-140">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="6a107-141">Запуск приложения с помощью клавиш **CTRL+F5** (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="6a107-141">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="6a107-142">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.</span><span class="sxs-lookup"><span data-stu-id="6a107-142">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="6a107-143">Из меню **Отладка** можно запустить приложение в режиме с отладкой или без.</span><span class="sxs-lookup"><span data-stu-id="6a107-143">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Меню отладки](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="6a107-145">Чтобы выполнить отладку приложения, нажмите кнопку **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="6a107-145">You can debug the app by selecting the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6a107-147">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6a107-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="6a107-148">Для работы с этим руководством требуется знание VS Code.</span><span class="sxs-lookup"><span data-stu-id="6a107-148">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="6a107-149">Дополнительные сведения см. в разделах [Начало работы с VS Code](https://code.visualstudio.com/docs) и [Справка по Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="6a107-149">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="6a107-150">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="6a107-150">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="6a107-151">Смените каталог (`cd`) на папку, в которой будет содержаться проект.</span><span class="sxs-lookup"><span data-stu-id="6a107-151">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="6a107-152">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="6a107-152">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="6a107-153">Появится диалоговое окно с предупреждением **В MvcMovie отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="6a107-153">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="6a107-154">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="6a107-154">Select **Yes**</span></span>

  * <span data-ttu-id="6a107-155">`dotnet new mvc -o MvcMovie`: создает новый проект MVC ASP.NET Core в папке *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="6a107-155">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="6a107-156">`code -r MvcMovie`: загружает файл проекта *MvcMovie.csproj* в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6a107-156">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="6a107-157">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="6a107-157">Launch the app</span></span>

* <span data-ttu-id="6a107-158">Нажмите клавиши **CTRL-F5**, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="6a107-158">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="6a107-159">Visual Studio Code запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="6a107-159">Visual Studio Code starts starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="6a107-160">В адресной строке указывается `localhost:port:5001`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="6a107-160">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="6a107-161">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="6a107-161">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="6a107-162">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="6a107-162">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="6a107-163">Запуск приложения с помощью клавиш **CTRL+F5** (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="6a107-163">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="6a107-164">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро обновлять страницу и просматривать изменения.</span><span class="sxs-lookup"><span data-stu-id="6a107-164">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6a107-165">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="6a107-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6a107-166">Выберите **Файл** > **Новое решение**.</span><span class="sxs-lookup"><span data-stu-id="6a107-166">Select **File** > **New Solution**.</span></span>

  ![Новое решение macOS](~/tutorials/first-web-api-mac/_static/sln.png)

* <span data-ttu-id="6a107-168">Выберите **Приложение .NET Core** > **ASP.NET Core** > **Веб-приложение ASP.NET Core (MVC)** > **Далее**.</span><span class="sxs-lookup"><span data-stu-id="6a107-168">Select **.NET Core App** > **ASP.NET Core** > **ASP.NET Core Web App (MVC)** > **Next**.</span></span>

  ![Диалоговое окно "Новый проект" в macOS](~/tutorials/first-mvc-app-mac/start-mvc/1.png)

* <span data-ttu-id="6a107-170">В диалоговом окне **Настройка нового веб-API ASP.NET Core** оставьте установленное по умолчанию значение для параметра **Целевая платформа**, то есть \**.NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="6a107-170">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="6a107-171">Присвойте проекту имя **MvcMovie** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="6a107-171">Name the project **MvcMovie**, and then select **Create**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="6a107-172">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="6a107-172">Launch the app</span></span>

<span data-ttu-id="6a107-173">Выберите **Выполнить** > **Запуск без отладки**, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="6a107-173">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="6a107-174">Visual Studio для Mac запустит сервер [Kestrel](xref:fundamentals/servers/index#kestrel), откроет браузер и перейдет к `http://localhost:port`, где *port* — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="6a107-174">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

* <span data-ttu-id="6a107-175">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="6a107-175">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="6a107-176">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="6a107-176">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="6a107-177">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="6a107-177">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="6a107-178">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="6a107-178">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="6a107-179">В меню **Запуск** можно запустить приложение в режиме с отладкой или без нее.</span><span class="sxs-lookup"><span data-stu-id="6a107-179">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

---  
<!-- End of VS tabs -->

* <span data-ttu-id="6a107-180">Нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="6a107-180">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="6a107-181">Это приложение не отслеживает персональные данные.</span><span class="sxs-lookup"><span data-stu-id="6a107-181">This app doesn't track personal information.</span></span> <span data-ttu-id="6a107-182">Созданный шаблоном код включает ресурсы для соблюдения [Общего регламента по защите данных (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="6a107-182">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/privacy.png)

  <span data-ttu-id="6a107-184">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="6a107-184">The following image shows the app after accepting tracking:</span></span>

  ![Домашняя или индексная страница](start-mvc/_static/home2.2.png)

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="6a107-186">В следующей части этого учебника мы поговорим об MVC и приступим к написанию кода.</span><span class="sxs-lookup"><span data-stu-id="6a107-186">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="6a107-187">Вперед</span><span class="sxs-lookup"><span data-stu-id="6a107-187">Next</span></span>](adding-controller.md)  
