---
title: Учебник. Начало работы с Razor Pages в ASP.NET Core
author: rick-anderson
description: В этой серии руководств объясняется, как использовать Razor Pages в ASP.NET Core. Узнайте, как создать модель, сгенерировать код для Razor Pages, использовать Entity Framework Core и SQL Server для доступа к данным, добавлять функции поиска и проверки ввода, а также использовать возможность миграции для обновления модели.
ms.author: riande
ms.date: 07/25/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 67a5fcee0a37861fd39a018443edbc0b9e513213
ms.sourcegitcommit: 7a46973998623aead757ad386fe33602b1658793
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/15/2019
ms.locfileid: "69487668"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="0af56-104">Учебник. Начало работы с Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0af56-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="0af56-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="0af56-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="0af56-106">В этом первом руководстве серии приводятся основные сведения о веб-приложении Razor Pages в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0af56-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="0af56-107">Пройдя всю серию, вы получите приложение, которое управляет базой данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="0af56-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="0af56-108">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="0af56-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0af56-109">Создание веб-приложения Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0af56-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="0af56-110">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="0af56-110">Run the app.</span></span>
> * <span data-ttu-id="0af56-111">Анализ файлов проекта.</span><span class="sxs-lookup"><span data-stu-id="0af56-111">Examine the project files.</span></span>

<span data-ttu-id="0af56-112">Пройдя это руководство, вы получите рабочее веб-приложение Razor Pages, сборка которого описана в последующих руководствах.</span><span class="sxs-lookup"><span data-stu-id="0af56-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="0af56-114">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="0af56-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0af56-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0af56-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0af56-116">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="0af56-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0af56-117">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="0af56-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="0af56-118">Создание веб-приложения Razor Pages</span><span class="sxs-lookup"><span data-stu-id="0af56-118">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0af56-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0af56-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0af56-120">В меню **Файл** Visual Studio откройте **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="0af56-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="0af56-121">Создайте веб-приложение ASP.NET Core и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="0af56-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="0af56-122">![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="0af56-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="0af56-123">Назовите проект **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="0af56-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="0af56-124">Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.</span><span class="sxs-lookup"><span data-stu-id="0af56-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="0af56-125">![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="0af56-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="0af56-126">Выберите в раскрывающемся списке пункт **ASP.NET Core 3.0**, затем — **Веб-приложение** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="0af56-126">Select **ASP.NET Core 3.0** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="0af56-128">Создается следующий начальный проект:</span><span class="sxs-lookup"><span data-stu-id="0af56-128">The following starter project is created:</span></span>

  ![обозреватель решений](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0af56-130">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="0af56-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="0af56-131">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="0af56-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="0af56-132">Перейдите в каталог `cd`, в котором находится проект.</span><span class="sxs-lookup"><span data-stu-id="0af56-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="0af56-133">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="0af56-133">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="0af56-134">Команда `dotnet new` создает новый проект Razor Pages в папке *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="0af56-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="0af56-135">Команда `code` открывает папку *RazorPagesMovie* в текущем экземпляре Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="0af56-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="0af56-136">Когда значок строки состояния OmniSharp станет зеленым, появится диалоговое окно с предупреждением **Required assets to build and debug are missing from 'RazorPagesMovie'. (В RazorPagesMovie отсутствуют необходимые ресурсы для сборки и отладки.) Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="0af56-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="0af56-137">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="0af56-137">Select **Yes**.</span></span>

  <span data-ttu-id="0af56-138">Каталог *.vscode*, содержащий файлы *launch.json* и *tasks.json*, добавляется в корневой каталог проекта.</span><span class="sxs-lookup"><span data-stu-id="0af56-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0af56-139">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="0af56-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0af56-140">Выберите **Файл** > **Новое решение**.</span><span class="sxs-lookup"><span data-stu-id="0af56-140">Select **File** > **New Solution**.</span></span>

![Новое решение macOS](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="0af56-142">Выберите **.NET Core** > **Приложение** > **Веб-приложение** > **Далее**.</span><span class="sxs-lookup"><span data-stu-id="0af56-142">Select **.NET Core** > **App** > **Web Application** > **Next**.</span></span>

  ![Диалоговое окно "Новый проект" в macOS](razor-pages-start/_static/webapp.png)

* <span data-ttu-id="0af56-144">В диалоговом окне **Настройка нового веб-API ASP.NET Core** установите для параметра **Целевая платформа** значение **.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="0af56-144">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** to **.NET Core 3.0**.</span></span>

  ![Выбор .NET Core 3.0 для macOS](razor-pages-start/_static/targetframework3.png)

* <span data-ttu-id="0af56-146">Присвойте проекту имя **RazorPagesMovie** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="0af56-146">Name the project **RazorPagesMovie**, and then select **Create**.</span></span>

  ![nameproj](razor-pages-start/_static/RazorPagesMovie.png)


## <a name="open-the-project"></a><span data-ttu-id="0af56-148">Открытие проекта</span><span class="sxs-lookup"><span data-stu-id="0af56-148">Open the project</span></span>

<span data-ttu-id="0af56-149">В Visual Studio откройте меню **Файл > Открыть файл** и выберите файл *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="0af56-149">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="0af56-150">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="0af56-150">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0af56-151">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0af56-151">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0af56-152">Нажмите клавиши CTRL+F5, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="0af56-152">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="0af56-153">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение.</span><span class="sxs-lookup"><span data-stu-id="0af56-153">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="0af56-154">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="0af56-154">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="0af56-155">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="0af56-155">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="0af56-156">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="0af56-156">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="0af56-157">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="0af56-157">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
 
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0af56-158">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="0af56-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="0af56-159">Нажмите клавиши **CTRL-F5**, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="0af56-159">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="0af56-160">Visual Studio Code запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="0af56-160">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="0af56-161">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="0af56-161">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="0af56-162">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="0af56-162">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="0af56-163">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="0af56-163">Localhost only serves web requests from the local computer.</span></span>

  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0af56-164">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="0af56-164">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="0af56-165">Чтобы выполнить запуск без отладчика, нажмите клавиши **ALT+CMD+ВВОД**.</span><span class="sxs-lookup"><span data-stu-id="0af56-165">Press **Alt-Cmd-Enter** to run without the debugger.</span></span> <span data-ttu-id="0af56-166">Вы также можете в строке меню выбрать "Запуск" > "Запуск без отладки".</span><span class="sxs-lookup"><span data-stu-id="0af56-166">Alternatively, navigate to the menu bar and go to Run>Start Without Debugging.</span></span>

  <span data-ttu-id="0af56-167">Visual Studio запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="0af56-167">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="0af56-168">Анализ файлов проекта</span><span class="sxs-lookup"><span data-stu-id="0af56-168">Examine the project files</span></span>

<span data-ttu-id="0af56-169">Ниже приведен обзор основных папок и файлов проекта, с которыми вы будете работать в последующих учебниках.</span><span class="sxs-lookup"><span data-stu-id="0af56-169">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="0af56-170">Папка Pages</span><span class="sxs-lookup"><span data-stu-id="0af56-170">Pages folder</span></span>

<span data-ttu-id="0af56-171">Содержит страницы Razor и вспомогательные файлы.</span><span class="sxs-lookup"><span data-stu-id="0af56-171">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="0af56-172">Каждая страница Razor — это пара файлов.</span><span class="sxs-lookup"><span data-stu-id="0af56-172">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="0af56-173">Файл *.cshtml*, содержащий HTML-разметку с кодом C# и синтаксисом Razor.</span><span class="sxs-lookup"><span data-stu-id="0af56-173">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="0af56-174">Файл *. cshtml.cs*, содержащий код C#, который обрабатывает события страницы.</span><span class="sxs-lookup"><span data-stu-id="0af56-174">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="0af56-175">Имена вспомогательных файлов начинаются с символа подчеркивания.</span><span class="sxs-lookup"><span data-stu-id="0af56-175">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="0af56-176">Например, файл *_Layout.cshtml* настраивает элементы пользовательского интерфейса, общие для всех страниц.</span><span class="sxs-lookup"><span data-stu-id="0af56-176">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="0af56-177">Этот файл настраивает меню навигации в верхней части страницы и уведомление об авторских правах в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="0af56-177">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="0af56-178">Дополнительные сведения можно найти по адресу: <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="0af56-178">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="0af56-179">Папка wwwroot</span><span class="sxs-lookup"><span data-stu-id="0af56-179">wwwroot folder</span></span>

<span data-ttu-id="0af56-180">Содержит статические файлы, такие как HTML-файлы, файлы JavaScript и CSS-файлы.</span><span class="sxs-lookup"><span data-stu-id="0af56-180">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="0af56-181">Дополнительные сведения можно найти по адресу: <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="0af56-181">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="0af56-182">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="0af56-182">appSettings.json</span></span>

<span data-ttu-id="0af56-183">Содержит данные конфигурации, например строки подключения.</span><span class="sxs-lookup"><span data-stu-id="0af56-183">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="0af56-184">Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="0af56-184">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="0af56-185">Program.cs</span><span class="sxs-lookup"><span data-stu-id="0af56-185">Program.cs</span></span>

<span data-ttu-id="0af56-186">Содержит точку входа для программы.</span><span class="sxs-lookup"><span data-stu-id="0af56-186">Contains the entry point for the program.</span></span> <span data-ttu-id="0af56-187">Дополнительные сведения можно найти по адресу: <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="0af56-187">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="0af56-188">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="0af56-188">Startup.cs</span></span>

<span data-ttu-id="0af56-189">содержит код, задающий поведение приложения.</span><span class="sxs-lookup"><span data-stu-id="0af56-189">Contains code that configures app behavior.</span></span> <span data-ttu-id="0af56-190">Дополнительные сведения можно найти по адресу: <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="0af56-190">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0af56-191">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="0af56-191">Next steps</span></span>

<span data-ttu-id="0af56-192">Перейдите к следующему учебнику в серии:</span><span class="sxs-lookup"><span data-stu-id="0af56-192">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0af56-193">Добавление модели</span><span class="sxs-lookup"><span data-stu-id="0af56-193">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="0af56-194">Это первый учебник из серии.</span><span class="sxs-lookup"><span data-stu-id="0af56-194">This is the first tutorial of a series.</span></span> <span data-ttu-id="0af56-195">В этой [серии](xref:tutorials/razor-pages/index) приводятся основные сведения о сборке веб-приложения Razor Pages в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0af56-195">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="0af56-196">Пройдя всю серию, вы получите приложение, которое управляет базой данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="0af56-196">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="0af56-197">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="0af56-197">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0af56-198">Создание веб-приложения Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0af56-198">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="0af56-199">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="0af56-199">Run the app.</span></span>
> * <span data-ttu-id="0af56-200">Анализ файлов проекта.</span><span class="sxs-lookup"><span data-stu-id="0af56-200">Examine the project files.</span></span>

<span data-ttu-id="0af56-201">Пройдя это руководство, вы получите рабочее веб-приложение Razor Pages, сборка которого описана в последующих руководствах.</span><span class="sxs-lookup"><span data-stu-id="0af56-201">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="0af56-203">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="0af56-203">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0af56-204">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0af56-204">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0af56-205">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="0af56-205">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0af56-206">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="0af56-206">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="0af56-207">Создание веб-приложения Razor Pages</span><span class="sxs-lookup"><span data-stu-id="0af56-207">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0af56-208">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0af56-208">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0af56-209">В меню **Файл** Visual Studio откройте **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="0af56-209">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="0af56-210">Создайте веб-приложение ASP.NET Core и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="0af56-210">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="0af56-212">Назовите проект **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="0af56-212">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="0af56-213">Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.</span><span class="sxs-lookup"><span data-stu-id="0af56-213">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="0af56-215">Выберите в раскрывающемся списке пункт **ASP.NET Core 2.2**, затем — **Веб-приложение** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="0af56-215">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="0af56-217">Создается следующий начальный проект:</span><span class="sxs-lookup"><span data-stu-id="0af56-217">The following starter project is created:</span></span>

  ![обозреватель решений](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0af56-219">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="0af56-219">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="0af56-220">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="0af56-220">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="0af56-221">Перейдите в каталог `cd`, в котором находится проект.</span><span class="sxs-lookup"><span data-stu-id="0af56-221">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="0af56-222">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="0af56-222">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="0af56-223">Команда `dotnet new` создает новый проект Razor Pages в папке *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="0af56-223">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="0af56-224">Команда `code` открывает папку *RazorPagesMovie* в текущем экземпляре Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="0af56-224">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="0af56-225">Когда значок строки состояния OmniSharp станет зеленым, появится диалоговое окно с предупреждением **Required assets to build and debug are missing from 'RazorPagesMovie'. (В RazorPagesMovie отсутствуют необходимые ресурсы для сборки и отладки.) Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="0af56-225">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="0af56-226">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="0af56-226">Select **Yes**.</span></span>

  <span data-ttu-id="0af56-227">Каталог *.vscode*, содержащий файлы *launch.json* и *tasks.json*, добавляется в корневой каталог проекта.</span><span class="sxs-lookup"><span data-stu-id="0af56-227">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0af56-228">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="0af56-228">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="0af56-229">В терминале выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="0af56-229">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="0af56-230">Указанные выше команды используют [интерфейс командной строки .NET Core](/dotnet/core/tools/dotnet) для создания проекта Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0af56-230">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="0af56-231">Открытие проекта</span><span class="sxs-lookup"><span data-stu-id="0af56-231">Open the project</span></span>

<span data-ttu-id="0af56-232">В Visual Studio откройте меню **Файл > Открыть файл** и выберите файл *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="0af56-232">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="0af56-233">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="0af56-233">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0af56-234">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0af56-234">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0af56-235">Нажмите клавиши CTRL+F5, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="0af56-235">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="0af56-236">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение.</span><span class="sxs-lookup"><span data-stu-id="0af56-236">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="0af56-237">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="0af56-237">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="0af56-238">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="0af56-238">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="0af56-239">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="0af56-239">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="0af56-240">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="0af56-240">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="0af56-241">На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="0af56-241">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="0af56-242">Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.</span><span class="sxs-lookup"><span data-stu-id="0af56-242">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="0af56-244">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="0af56-244">The following image shows the app after you give consent to tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0af56-246">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="0af56-246">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="0af56-247">Нажмите клавиши **CTRL-F5**, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="0af56-247">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="0af56-248">Visual Studio Code запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="0af56-248">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="0af56-249">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="0af56-249">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="0af56-250">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="0af56-250">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="0af56-251">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="0af56-251">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="0af56-252">На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="0af56-252">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="0af56-253">Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.</span><span class="sxs-lookup"><span data-stu-id="0af56-253">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="0af56-255">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="0af56-255">The following image shows the app after you give consent to tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0af56-257">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="0af56-257">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="0af56-258">Нажмите клавиши **Cmd-Opt-F5**, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="0af56-258">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="0af56-259">Visual Studio запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="0af56-259">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="0af56-260">На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="0af56-260">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="0af56-261">Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.</span><span class="sxs-lookup"><span data-stu-id="0af56-261">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="0af56-263">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="0af56-263">The following image shows the app after you give consent to tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="0af56-265">Анализ файлов проекта</span><span class="sxs-lookup"><span data-stu-id="0af56-265">Examine the project files</span></span>

<span data-ttu-id="0af56-266">Ниже приведен обзор основных папок и файлов проекта, с которыми вы будете работать в последующих учебниках.</span><span class="sxs-lookup"><span data-stu-id="0af56-266">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="0af56-267">Папка Pages</span><span class="sxs-lookup"><span data-stu-id="0af56-267">Pages folder</span></span>

<span data-ttu-id="0af56-268">Содержит страницы Razor и вспомогательные файлы.</span><span class="sxs-lookup"><span data-stu-id="0af56-268">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="0af56-269">Каждая страница Razor — это пара файлов.</span><span class="sxs-lookup"><span data-stu-id="0af56-269">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="0af56-270">Файл *.cshtml*, содержащий HTML-разметку с кодом C# и синтаксисом Razor.</span><span class="sxs-lookup"><span data-stu-id="0af56-270">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="0af56-271">Файл *. cshtml.cs*, содержащий код C#, который обрабатывает события страницы.</span><span class="sxs-lookup"><span data-stu-id="0af56-271">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="0af56-272">Имена вспомогательных файлов начинаются с символа подчеркивания.</span><span class="sxs-lookup"><span data-stu-id="0af56-272">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="0af56-273">Например, файл *_Layout.cshtml* настраивает элементы пользовательского интерфейса, общие для всех страниц.</span><span class="sxs-lookup"><span data-stu-id="0af56-273">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="0af56-274">Этот файл настраивает меню навигации в верхней части страницы и уведомление об авторских правах в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="0af56-274">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="0af56-275">Дополнительные сведения можно найти по адресу: <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="0af56-275">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="0af56-276">Папка wwwroot</span><span class="sxs-lookup"><span data-stu-id="0af56-276">wwwroot folder</span></span>

<span data-ttu-id="0af56-277">Содержит статические файлы, такие как HTML-файлы, файлы JavaScript и CSS-файлы.</span><span class="sxs-lookup"><span data-stu-id="0af56-277">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="0af56-278">Дополнительные сведения можно найти по адресу: <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="0af56-278">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="0af56-279">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="0af56-279">appSettings.json</span></span>

<span data-ttu-id="0af56-280">Содержит данные конфигурации, например строки подключения.</span><span class="sxs-lookup"><span data-stu-id="0af56-280">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="0af56-281">Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="0af56-281">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="0af56-282">Program.cs</span><span class="sxs-lookup"><span data-stu-id="0af56-282">Program.cs</span></span>

<span data-ttu-id="0af56-283">Содержит точку входа для программы.</span><span class="sxs-lookup"><span data-stu-id="0af56-283">Contains the entry point for the program.</span></span> <span data-ttu-id="0af56-284">Дополнительные сведения можно найти по адресу: <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="0af56-284">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="0af56-285">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="0af56-285">Startup.cs</span></span>

<span data-ttu-id="0af56-286">Содержит код, который настраивает поведение приложения, например, требуется ли согласие для файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="0af56-286">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="0af56-287">Дополнительные сведения можно найти по адресу: <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="0af56-287">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0af56-288">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="0af56-288">Additional resources</span></span>

* [<span data-ttu-id="0af56-289">Версия руководства на YouTube</span><span class="sxs-lookup"><span data-stu-id="0af56-289">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="0af56-290">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="0af56-290">Next steps</span></span>

<span data-ttu-id="0af56-291">Перейдите к следующему учебнику в серии:</span><span class="sxs-lookup"><span data-stu-id="0af56-291">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0af56-292">Добавление модели</span><span class="sxs-lookup"><span data-stu-id="0af56-292">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end
