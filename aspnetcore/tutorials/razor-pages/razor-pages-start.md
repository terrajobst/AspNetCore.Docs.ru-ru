---
title: Учебник. Начало работы с Razor Pages в ASP.NET Core
author: rick-anderson
description: В этой серии руководств объясняется, как использовать Razor Pages в ASP.NET Core. Узнайте, как создать модель, сгенерировать код для Razor Pages, использовать Entity Framework Core и SQL Server для доступа к данным, добавлять функции поиска и проверки ввода, а также использовать возможность миграции для обновления модели.
ms.author: riande
ms.date: 11/12/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 6e1d58ccd83f7d7c1083dc2bf9ce7476650812a1
ms.sourcegitcommit: da2fb2d78ce70accdba903ccbfdcfffdd0112123
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/07/2020
ms.locfileid: "75723031"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="f4baf-104">Учебник. Начало работы с Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f4baf-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="f4baf-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="f4baf-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="f4baf-106">В этом первом руководстве серии приводятся основные сведения о веб-приложении Razor Pages в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f4baf-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="f4baf-107">Пройдя всю серию, вы получите приложение, которое управляет базой данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="f4baf-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="f4baf-108">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="f4baf-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f4baf-109">Создание веб-приложения Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="f4baf-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="f4baf-110">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="f4baf-110">Run the app.</span></span>
> * <span data-ttu-id="f4baf-111">Анализ файлов проекта.</span><span class="sxs-lookup"><span data-stu-id="f4baf-111">Examine the project files.</span></span>

<span data-ttu-id="f4baf-112">Пройдя это руководство, вы получите рабочее веб-приложение Razor Pages, сборка которого описана в последующих руководствах.</span><span class="sxs-lookup"><span data-stu-id="f4baf-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="f4baf-114">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="f4baf-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f4baf-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f4baf-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f4baf-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f4baf-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f4baf-117">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="f4baf-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="f4baf-118">Создание веб-приложения Razor Pages</span><span class="sxs-lookup"><span data-stu-id="f4baf-118">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f4baf-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f4baf-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f4baf-120">В Visual Studio в меню **Файл** щелкните **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="f4baf-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f4baf-121">Создайте веб-приложение ASP.NET Core и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="f4baf-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="f4baf-122">![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="f4baf-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="f4baf-123">Назовите проект **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="f4baf-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="f4baf-124">Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.</span><span class="sxs-lookup"><span data-stu-id="f4baf-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="f4baf-125">![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="f4baf-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="f4baf-126">Выберите в раскрывающемся списке пункт **ASP.NET Core 3.1**, затем — **Веб-приложение** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="f4baf-126">Select **ASP.NET Core 3.1** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="f4baf-128">Создается следующий начальный проект:</span><span class="sxs-lookup"><span data-stu-id="f4baf-128">The following starter project is created:</span></span>

  ![обозреватель решений](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f4baf-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f4baf-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f4baf-131">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="f4baf-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="f4baf-132">Перейдите в каталог `cd`, в котором находится проект.</span><span class="sxs-lookup"><span data-stu-id="f4baf-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="f4baf-133">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="f4baf-133">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="f4baf-134">Команда `dotnet new` создает новый проект Razor Pages в папке *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="f4baf-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="f4baf-135">Команда `code` открывает папку *RazorPagesMovie* в текущем экземпляре Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="f4baf-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="f4baf-136">Когда значок строки состояния OmniSharp станет зеленым, появится диалоговое окно с предупреждением **Required assets to build and debug are missing from 'RazorPagesMovie'. (В RazorPagesMovie отсутствуют необходимые ресурсы для сборки и отладки.) Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="f4baf-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="f4baf-137">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="f4baf-137">Select **Yes**.</span></span>

  <span data-ttu-id="f4baf-138">Каталог *.vscode*, содержащий файлы *launch.json* и *tasks.json*, добавляется в корневой каталог проекта.</span><span class="sxs-lookup"><span data-stu-id="f4baf-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f4baf-139">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="f4baf-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f4baf-140">Щелкните **Файл** > **Новое решение**.</span><span class="sxs-lookup"><span data-stu-id="f4baf-140">Select **File** > **New Solution**.</span></span>

![Новое решение macOS](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="f4baf-142">Щелкните **.NET Core** > **Приложение** > **Веб-приложение** > **Далее**.</span><span class="sxs-lookup"><span data-stu-id="f4baf-142">Select **.NET Core** > **App** > **Web Application** > **Next**.</span></span>

  ![Диалоговое окно "Новый проект" в macOS](razor-pages-start/_static/webapp.png)

* <span data-ttu-id="f4baf-144">В диалоговом окне **Настройка нового веб-приложения** установите для параметра **Целевая платформа** значение **.NET Core 3.1**.</span><span class="sxs-lookup"><span data-stu-id="f4baf-144">In the **Configure your new Web Application** dialog, set the  **Target Framework** to **.NET Core 3.1**.</span></span>

  ![Выбор .NET Core 3.1 для macOS](razor-pages-start/_static/targetframework3.png)

* <span data-ttu-id="f4baf-146">Присвойте проекту имя **RazorPagesMovie** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="f4baf-146">Name the project **RazorPagesMovie**, and then select **Create**.</span></span>

  ![nameproj](razor-pages-start/_static/RazorPagesMovie.png)

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="f4baf-148">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="f4baf-148">Run the app</span></span>

  [!INCLUDE[](~/includes/run-the-app.md)]

## <a name="examine-the-project-files"></a><span data-ttu-id="f4baf-149">Анализ файлов проекта</span><span class="sxs-lookup"><span data-stu-id="f4baf-149">Examine the project files</span></span>

<span data-ttu-id="f4baf-150">Ниже приведен обзор основных папок и файлов проекта, с которыми вы будете работать в последующих учебниках.</span><span class="sxs-lookup"><span data-stu-id="f4baf-150">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="f4baf-151">Папка Pages</span><span class="sxs-lookup"><span data-stu-id="f4baf-151">Pages folder</span></span>

<span data-ttu-id="f4baf-152">Содержит страницы Razor и вспомогательные файлы.</span><span class="sxs-lookup"><span data-stu-id="f4baf-152">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="f4baf-153">Каждая страница Razor — это пара файлов.</span><span class="sxs-lookup"><span data-stu-id="f4baf-153">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="f4baf-154">Файл *.cshtml*, содержащий HTML-разметку с кодом C# и синтаксисом Razor.</span><span class="sxs-lookup"><span data-stu-id="f4baf-154">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="f4baf-155">Файл *. cshtml.cs*, содержащий код C#, который обрабатывает события страницы.</span><span class="sxs-lookup"><span data-stu-id="f4baf-155">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="f4baf-156">Имена вспомогательных файлов начинаются с символа подчеркивания.</span><span class="sxs-lookup"><span data-stu-id="f4baf-156">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="f4baf-157">Например, файл *_Layout.cshtml* настраивает элементы пользовательского интерфейса, общие для всех страниц.</span><span class="sxs-lookup"><span data-stu-id="f4baf-157">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="f4baf-158">Этот файл настраивает меню навигации в верхней части страницы и уведомление об авторских правах в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="f4baf-158">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="f4baf-159">Для получения дополнительной информации см. <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="f4baf-159">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="f4baf-160">Папка wwwroot</span><span class="sxs-lookup"><span data-stu-id="f4baf-160">wwwroot folder</span></span>

<span data-ttu-id="f4baf-161">Содержит статические файлы, такие как HTML-файлы, файлы JavaScript и CSS-файлы.</span><span class="sxs-lookup"><span data-stu-id="f4baf-161">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="f4baf-162">Для получения дополнительной информации см. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="f4baf-162">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="f4baf-163">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="f4baf-163">appSettings.json</span></span>

<span data-ttu-id="f4baf-164">Содержит данные конфигурации, например строки подключения.</span><span class="sxs-lookup"><span data-stu-id="f4baf-164">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="f4baf-165">Для получения дополнительной информации см. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="f4baf-165">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="f4baf-166">Program.cs</span><span class="sxs-lookup"><span data-stu-id="f4baf-166">Program.cs</span></span>

<span data-ttu-id="f4baf-167">Содержит точку входа для программы.</span><span class="sxs-lookup"><span data-stu-id="f4baf-167">Contains the entry point for the program.</span></span> <span data-ttu-id="f4baf-168">Для получения дополнительной информации см. <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="f4baf-168">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="f4baf-169">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="f4baf-169">Startup.cs</span></span>

<span data-ttu-id="f4baf-170">содержит код, задающий поведение приложения.</span><span class="sxs-lookup"><span data-stu-id="f4baf-170">Contains code that configures app behavior.</span></span> <span data-ttu-id="f4baf-171">Для получения дополнительной информации см. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="f4baf-171">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f4baf-172">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="f4baf-172">Next steps</span></span>

<span data-ttu-id="f4baf-173">Перейдите к следующему учебнику в серии:</span><span class="sxs-lookup"><span data-stu-id="f4baf-173">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f4baf-174">Добавление модели</span><span class="sxs-lookup"><span data-stu-id="f4baf-174">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="f4baf-175">Это первый учебник из серии.</span><span class="sxs-lookup"><span data-stu-id="f4baf-175">This is the first tutorial of a series.</span></span> <span data-ttu-id="f4baf-176">В этой [серии](xref:tutorials/razor-pages/index) приводятся основные сведения о сборке веб-приложения Razor Pages в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f4baf-176">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="f4baf-177">Пройдя всю серию, вы получите приложение, которое управляет базой данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="f4baf-177">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="f4baf-178">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="f4baf-178">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f4baf-179">Создание веб-приложения Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="f4baf-179">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="f4baf-180">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="f4baf-180">Run the app.</span></span>
> * <span data-ttu-id="f4baf-181">Анализ файлов проекта.</span><span class="sxs-lookup"><span data-stu-id="f4baf-181">Examine the project files.</span></span>

<span data-ttu-id="f4baf-182">Пройдя это руководство, вы получите рабочее веб-приложение Razor Pages, сборка которого описана в последующих руководствах.</span><span class="sxs-lookup"><span data-stu-id="f4baf-182">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="f4baf-184">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="f4baf-184">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f4baf-185">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f4baf-185">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f4baf-186">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f4baf-186">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f4baf-187">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="f4baf-187">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="f4baf-188">Создание веб-приложения Razor Pages</span><span class="sxs-lookup"><span data-stu-id="f4baf-188">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f4baf-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f4baf-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f4baf-190">В Visual Studio в меню **Файл** щелкните **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="f4baf-190">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="f4baf-191">Создайте веб-приложение ASP.NET Core и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="f4baf-191">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="f4baf-193">Назовите проект **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="f4baf-193">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="f4baf-194">Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.</span><span class="sxs-lookup"><span data-stu-id="f4baf-194">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="f4baf-196">Выберите в раскрывающемся списке пункт **ASP.NET Core 2.2**, затем — **Веб-приложение** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="f4baf-196">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="f4baf-198">Создается следующий начальный проект:</span><span class="sxs-lookup"><span data-stu-id="f4baf-198">The following starter project is created:</span></span>

  ![обозреватель решений](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f4baf-200">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f4baf-200">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f4baf-201">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="f4baf-201">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="f4baf-202">Перейдите в каталог `cd`, в котором находится проект.</span><span class="sxs-lookup"><span data-stu-id="f4baf-202">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="f4baf-203">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="f4baf-203">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="f4baf-204">Команда `dotnet new` создает новый проект Razor Pages в папке *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="f4baf-204">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="f4baf-205">Команда `code` открывает папку *RazorPagesMovie* в текущем экземпляре Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="f4baf-205">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="f4baf-206">Когда значок строки состояния OmniSharp станет зеленым, появится диалоговое окно с предупреждением **Required assets to build and debug are missing from 'RazorPagesMovie'. (В RazorPagesMovie отсутствуют необходимые ресурсы для сборки и отладки.) Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="f4baf-206">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="f4baf-207">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="f4baf-207">Select **Yes**.</span></span>

  <span data-ttu-id="f4baf-208">Каталог *.vscode*, содержащий файлы *launch.json* и *tasks.json*, добавляется в корневой каталог проекта.</span><span class="sxs-lookup"><span data-stu-id="f4baf-208">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f4baf-209">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="f4baf-209">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f4baf-210">Щелкните **Файл** > **Новое решение**.</span><span class="sxs-lookup"><span data-stu-id="f4baf-210">Select **File** > **New Solution**.</span></span>

![Новое решение macOS](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="f4baf-212">Щелкните **.NET Core** > **Приложение** > **Веб-приложение** > **Далее**.</span><span class="sxs-lookup"><span data-stu-id="f4baf-212">Select **.NET Core** > **App** > **Web Application** > **Next**.</span></span>

  ![Диалоговое окно "Новый проект" в macOS](razor-pages-start/_static/webapp.png)

* <span data-ttu-id="f4baf-214">В диалоговом окне **Настройка нового веб-API ASP.NET Core** установите для параметра **Целевая платформа** значение **.NET Core 3.1**.</span><span class="sxs-lookup"><span data-stu-id="f4baf-214">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** to **.NET Core 3.1**.</span></span>

  ![Выбор .NET Core 3.0 для macOS](razor-pages-start/_static/targetframework3.png)

* <span data-ttu-id="f4baf-216">Присвойте проекту имя **RazorPagesMovie** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="f4baf-216">Name the project **RazorPagesMovie**, and then select **Create**.</span></span>

  ![nameproj](razor-pages-start/_static/RazorPagesMovie.png)

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="f4baf-218">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="f4baf-218">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f4baf-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f4baf-219">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f4baf-220">Нажмите клавиши CTRL+F5, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="f4baf-220">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="f4baf-221">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение.</span><span class="sxs-lookup"><span data-stu-id="f4baf-221">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="f4baf-222">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="f4baf-222">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="f4baf-223">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="f4baf-223">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="f4baf-224">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="f4baf-224">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="f4baf-225">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="f4baf-225">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="f4baf-226">На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="f4baf-226">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="f4baf-227">Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.</span><span class="sxs-lookup"><span data-stu-id="f4baf-227">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="f4baf-229">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="f4baf-229">The following image shows the app after you give consent to tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f4baf-231">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f4baf-231">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="f4baf-232">Нажмите клавиши **CTRL-F5**, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="f4baf-232">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="f4baf-233">Visual Studio Code запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="f4baf-233">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="f4baf-234">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="f4baf-234">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="f4baf-235">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="f4baf-235">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="f4baf-236">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="f4baf-236">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="f4baf-237">На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="f4baf-237">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="f4baf-238">Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.</span><span class="sxs-lookup"><span data-stu-id="f4baf-238">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="f4baf-240">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="f4baf-240">The following image shows the app after you give consent to tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f4baf-242">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="f4baf-242">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="f4baf-243">Нажмите клавиши **Cmd-Opt-F5**, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="f4baf-243">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="f4baf-244">Visual Studio запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="f4baf-244">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="f4baf-245">На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="f4baf-245">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="f4baf-246">Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.</span><span class="sxs-lookup"><span data-stu-id="f4baf-246">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="f4baf-248">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="f4baf-248">The following image shows the app after you give consent to tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="f4baf-250">Анализ файлов проекта</span><span class="sxs-lookup"><span data-stu-id="f4baf-250">Examine the project files</span></span>

<span data-ttu-id="f4baf-251">Ниже приведен обзор основных папок и файлов проекта, с которыми вы будете работать в последующих учебниках.</span><span class="sxs-lookup"><span data-stu-id="f4baf-251">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="f4baf-252">Папка Pages</span><span class="sxs-lookup"><span data-stu-id="f4baf-252">Pages folder</span></span>

<span data-ttu-id="f4baf-253">Содержит страницы Razor и вспомогательные файлы.</span><span class="sxs-lookup"><span data-stu-id="f4baf-253">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="f4baf-254">Каждая страница Razor — это пара файлов.</span><span class="sxs-lookup"><span data-stu-id="f4baf-254">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="f4baf-255">Файл *.cshtml*, содержащий HTML-разметку с кодом C# и синтаксисом Razor.</span><span class="sxs-lookup"><span data-stu-id="f4baf-255">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="f4baf-256">Файл *. cshtml.cs*, содержащий код C#, который обрабатывает события страницы.</span><span class="sxs-lookup"><span data-stu-id="f4baf-256">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="f4baf-257">Имена вспомогательных файлов начинаются с символа подчеркивания.</span><span class="sxs-lookup"><span data-stu-id="f4baf-257">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="f4baf-258">Например, файл *_Layout.cshtml* настраивает элементы пользовательского интерфейса, общие для всех страниц.</span><span class="sxs-lookup"><span data-stu-id="f4baf-258">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="f4baf-259">Этот файл настраивает меню навигации в верхней части страницы и уведомление об авторских правах в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="f4baf-259">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="f4baf-260">Для получения дополнительной информации см. <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="f4baf-260">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="f4baf-261">Папка wwwroot</span><span class="sxs-lookup"><span data-stu-id="f4baf-261">wwwroot folder</span></span>

<span data-ttu-id="f4baf-262">Содержит статические файлы, такие как HTML-файлы, файлы JavaScript и CSS-файлы.</span><span class="sxs-lookup"><span data-stu-id="f4baf-262">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="f4baf-263">Для получения дополнительной информации см. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="f4baf-263">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="f4baf-264">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="f4baf-264">appSettings.json</span></span>

<span data-ttu-id="f4baf-265">Содержит данные конфигурации, например строки подключения.</span><span class="sxs-lookup"><span data-stu-id="f4baf-265">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="f4baf-266">Для получения дополнительной информации см. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="f4baf-266">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="f4baf-267">Program.cs</span><span class="sxs-lookup"><span data-stu-id="f4baf-267">Program.cs</span></span>

<span data-ttu-id="f4baf-268">Содержит точку входа для программы.</span><span class="sxs-lookup"><span data-stu-id="f4baf-268">Contains the entry point for the program.</span></span> <span data-ttu-id="f4baf-269">Для получения дополнительной информации см. <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="f4baf-269">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="f4baf-270">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="f4baf-270">Startup.cs</span></span>

<span data-ttu-id="f4baf-271">Содержит код, который настраивает поведение приложения, например, требуется ли согласие для файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="f4baf-271">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="f4baf-272">Для получения дополнительной информации см. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="f4baf-272">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f4baf-273">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="f4baf-273">Additional resources</span></span>

* [<span data-ttu-id="f4baf-274">Версия руководства на YouTube</span><span class="sxs-lookup"><span data-stu-id="f4baf-274">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="f4baf-275">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="f4baf-275">Next steps</span></span>

<span data-ttu-id="f4baf-276">Перейдите к следующему учебнику в серии:</span><span class="sxs-lookup"><span data-stu-id="f4baf-276">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f4baf-277">Добавление модели</span><span class="sxs-lookup"><span data-stu-id="f4baf-277">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end
