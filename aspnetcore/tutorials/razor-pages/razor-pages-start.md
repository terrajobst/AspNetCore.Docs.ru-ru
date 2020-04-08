---
title: Учебник. Начало работы с Razor Pages в ASP.NET Core
author: rick-anderson
description: В этой серии руководств объясняется, как использовать Razor Pages в ASP.NET Core. Узнайте, как создать модель, сгенерировать код для Razor Pages, использовать Entity Framework Core и SQL Server для доступа к данным, добавлять функции поиска и проверки ввода, а также использовать возможность миграции для обновления модели.
ms.author: riande
ms.date: 11/12/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 6e1d58ccd83f7d7c1083dc2bf9ce7476650812a1
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2020
ms.locfileid: "78646906"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="098b4-104">Учебник. Начало работы с Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="098b4-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="098b4-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="098b4-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="098b4-106">В этом первом руководстве серии приводятся основные сведения о веб-приложении Razor Pages в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="098b4-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="098b4-107">Пройдя всю серию, вы получите приложение, которое управляет базой данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="098b4-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="098b4-108">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="098b4-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="098b4-109">Создание веб-приложения Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="098b4-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="098b4-110">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="098b4-110">Run the app.</span></span>
> * <span data-ttu-id="098b4-111">Анализ файлов проекта.</span><span class="sxs-lookup"><span data-stu-id="098b4-111">Examine the project files.</span></span>

<span data-ttu-id="098b4-112">Пройдя это руководство, вы получите рабочее веб-приложение Razor Pages, сборка которого описана в последующих руководствах.</span><span class="sxs-lookup"><span data-stu-id="098b4-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="098b4-114">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="098b4-114">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="098b4-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="098b4-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="098b4-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="098b4-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="098b4-117">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="098b4-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="098b4-118">Создание веб-приложения Razor Pages</span><span class="sxs-lookup"><span data-stu-id="098b4-118">Create a Razor Pages web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="098b4-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="098b4-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="098b4-120">В Visual Studio в меню **Файл** щелкните **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="098b4-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="098b4-121">Создайте веб-приложение ASP.NET Core и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="098b4-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="098b4-122">![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="098b4-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="098b4-123">Назовите проект **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="098b4-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="098b4-124">Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.</span><span class="sxs-lookup"><span data-stu-id="098b4-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="098b4-125">![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="098b4-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="098b4-126">Выберите в раскрывающемся списке пункт **ASP.NET Core 3.1**, затем — **Веб-приложение** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="098b4-126">Select **ASP.NET Core 3.1** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="098b4-128">Создается следующий начальный проект:</span><span class="sxs-lookup"><span data-stu-id="098b4-128">The following starter project is created:</span></span>

  ![обозреватель решений](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="098b4-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="098b4-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="098b4-131">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="098b4-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="098b4-132">Перейдите в каталог `cd`, в котором находится проект.</span><span class="sxs-lookup"><span data-stu-id="098b4-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="098b4-133">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="098b4-133">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="098b4-134">Команда `dotnet new` создает новый проект Razor Pages в папке *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="098b4-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="098b4-135">Команда `code` открывает папку *RazorPagesMovie* в текущем экземпляре Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="098b4-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="098b4-136">Когда значок строки состояния OmniSharp станет зеленым, появится диалоговое окно с предупреждением **Required assets to build and debug are missing from 'RazorPagesMovie'. (В RazorPagesMovie отсутствуют необходимые ресурсы для сборки и отладки.) Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="098b4-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="098b4-137">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="098b4-137">Select **Yes**.</span></span>

  <span data-ttu-id="098b4-138">Каталог *.vscode*, содержащий файлы *launch.json* и *tasks.json*, добавляется в корневой каталог проекта.</span><span class="sxs-lookup"><span data-stu-id="098b4-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="098b4-139">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="098b4-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="098b4-140">Щелкните **Файл** > **Новое решение**.</span><span class="sxs-lookup"><span data-stu-id="098b4-140">Select **File** > **New Solution**.</span></span>

![Новое решение macOS](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="098b4-142">Щелкните **.NET Core** > **Приложение** > **Веб-приложение** > **Далее**.</span><span class="sxs-lookup"><span data-stu-id="098b4-142">Select **.NET Core** > **App** > **Web Application** > **Next**.</span></span>

  ![Диалоговое окно "Новый проект" в macOS](razor-pages-start/_static/webapp.png)

* <span data-ttu-id="098b4-144">В диалоговом окне **Настройка нового веб-приложения** установите для параметра **Целевая платформа** значение **.NET Core 3.1**.</span><span class="sxs-lookup"><span data-stu-id="098b4-144">In the **Configure your new Web Application** dialog, set the  **Target Framework** to **.NET Core 3.1**.</span></span>

  ![Выбор .NET Core 3.1 для macOS](razor-pages-start/_static/targetframework3.png)

* <span data-ttu-id="098b4-146">Присвойте проекту имя **RazorPagesMovie** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="098b4-146">Name the project **RazorPagesMovie**, and then select **Create**.</span></span>

  ![nameproj](razor-pages-start/_static/RazorPagesMovie.png)

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="098b4-148">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="098b4-148">Run the app</span></span>

  [!INCLUDE[](~/includes/run-the-app.md)]

## <a name="examine-the-project-files"></a><span data-ttu-id="098b4-149">Анализ файлов проекта</span><span class="sxs-lookup"><span data-stu-id="098b4-149">Examine the project files</span></span>

<span data-ttu-id="098b4-150">Ниже приведен обзор основных папок и файлов проекта, с которыми вы будете работать в последующих учебниках.</span><span class="sxs-lookup"><span data-stu-id="098b4-150">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="098b4-151">Папка Pages</span><span class="sxs-lookup"><span data-stu-id="098b4-151">Pages folder</span></span>

<span data-ttu-id="098b4-152">Содержит страницы Razor и вспомогательные файлы.</span><span class="sxs-lookup"><span data-stu-id="098b4-152">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="098b4-153">Каждая страница Razor — это пара файлов.</span><span class="sxs-lookup"><span data-stu-id="098b4-153">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="098b4-154">Файл *.cshtml*, содержащий HTML-разметку с кодом C# и синтаксисом Razor.</span><span class="sxs-lookup"><span data-stu-id="098b4-154">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="098b4-155">Файл *. cshtml.cs*, содержащий код C#, который обрабатывает события страницы.</span><span class="sxs-lookup"><span data-stu-id="098b4-155">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="098b4-156">Имена вспомогательных файлов начинаются с символа подчеркивания.</span><span class="sxs-lookup"><span data-stu-id="098b4-156">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="098b4-157">Например, файл *_Layout.cshtml* настраивает элементы пользовательского интерфейса, общие для всех страниц.</span><span class="sxs-lookup"><span data-stu-id="098b4-157">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="098b4-158">Этот файл настраивает меню навигации в верхней части страницы и уведомление об авторских правах в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="098b4-158">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="098b4-159">Для получения дополнительной информации см. <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="098b4-159">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="098b4-160">Папка wwwroot</span><span class="sxs-lookup"><span data-stu-id="098b4-160">wwwroot folder</span></span>

<span data-ttu-id="098b4-161">Содержит статические файлы, такие как HTML-файлы, файлы JavaScript и CSS-файлы.</span><span class="sxs-lookup"><span data-stu-id="098b4-161">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="098b4-162">Для получения дополнительной информации см. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="098b4-162">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="098b4-163">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="098b4-163">appSettings.json</span></span>

<span data-ttu-id="098b4-164">Содержит данные конфигурации, например строки подключения.</span><span class="sxs-lookup"><span data-stu-id="098b4-164">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="098b4-165">Для получения дополнительной информации см. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="098b4-165">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="098b4-166">Program.cs</span><span class="sxs-lookup"><span data-stu-id="098b4-166">Program.cs</span></span>

<span data-ttu-id="098b4-167">Содержит точку входа для программы.</span><span class="sxs-lookup"><span data-stu-id="098b4-167">Contains the entry point for the program.</span></span> <span data-ttu-id="098b4-168">Для получения дополнительной информации см. <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="098b4-168">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="098b4-169">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="098b4-169">Startup.cs</span></span>

<span data-ttu-id="098b4-170">содержит код, задающий поведение приложения.</span><span class="sxs-lookup"><span data-stu-id="098b4-170">Contains code that configures app behavior.</span></span> <span data-ttu-id="098b4-171">Для получения дополнительной информации см. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="098b4-171">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="098b4-172">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="098b4-172">Next steps</span></span>

<span data-ttu-id="098b4-173">Перейдите к следующему учебнику в серии:</span><span class="sxs-lookup"><span data-stu-id="098b4-173">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="098b4-174">Добавление модели</span><span class="sxs-lookup"><span data-stu-id="098b4-174">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="098b4-175">Это первый учебник из серии.</span><span class="sxs-lookup"><span data-stu-id="098b4-175">This is the first tutorial of a series.</span></span> <span data-ttu-id="098b4-176">В этой [серии](xref:tutorials/razor-pages/index) приводятся основные сведения о сборке веб-приложения Razor Pages в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="098b4-176">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="098b4-177">Пройдя всю серию, вы получите приложение, которое управляет базой данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="098b4-177">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="098b4-178">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="098b4-178">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="098b4-179">Создание веб-приложения Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="098b4-179">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="098b4-180">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="098b4-180">Run the app.</span></span>
> * <span data-ttu-id="098b4-181">Анализ файлов проекта.</span><span class="sxs-lookup"><span data-stu-id="098b4-181">Examine the project files.</span></span>

<span data-ttu-id="098b4-182">Пройдя это руководство, вы получите рабочее веб-приложение Razor Pages, сборка которого описана в последующих руководствах.</span><span class="sxs-lookup"><span data-stu-id="098b4-182">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="098b4-184">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="098b4-184">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="098b4-185">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="098b4-185">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="098b4-186">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="098b4-186">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="098b4-187">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="098b4-187">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="098b4-188">Создание веб-приложения Razor Pages</span><span class="sxs-lookup"><span data-stu-id="098b4-188">Create a Razor Pages web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="098b4-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="098b4-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="098b4-190">В Visual Studio в меню **Файл** щелкните **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="098b4-190">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="098b4-191">Создайте веб-приложение ASP.NET Core и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="098b4-191">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="098b4-193">Назовите проект **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="098b4-193">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="098b4-194">Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.</span><span class="sxs-lookup"><span data-stu-id="098b4-194">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="098b4-196">Выберите в раскрывающемся списке пункт **ASP.NET Core 2.2**, затем — **Веб-приложение** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="098b4-196">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="098b4-198">Создается следующий начальный проект:</span><span class="sxs-lookup"><span data-stu-id="098b4-198">The following starter project is created:</span></span>

  ![обозреватель решений](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="098b4-200">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="098b4-200">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="098b4-201">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="098b4-201">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="098b4-202">Перейдите в каталог `cd`, в котором находится проект.</span><span class="sxs-lookup"><span data-stu-id="098b4-202">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="098b4-203">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="098b4-203">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="098b4-204">Команда `dotnet new` создает новый проект Razor Pages в папке *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="098b4-204">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="098b4-205">Команда `code` открывает папку *RazorPagesMovie* в текущем экземпляре Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="098b4-205">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="098b4-206">Когда значок строки состояния OmniSharp станет зеленым, появится диалоговое окно с предупреждением **Required assets to build and debug are missing from 'RazorPagesMovie'. (В RazorPagesMovie отсутствуют необходимые ресурсы для сборки и отладки.) Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="098b4-206">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="098b4-207">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="098b4-207">Select **Yes**.</span></span>

  <span data-ttu-id="098b4-208">Каталог *.vscode*, содержащий файлы *launch.json* и *tasks.json*, добавляется в корневой каталог проекта.</span><span class="sxs-lookup"><span data-stu-id="098b4-208">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="098b4-209">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="098b4-209">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="098b4-210">Щелкните **Файл** > **Новое решение**.</span><span class="sxs-lookup"><span data-stu-id="098b4-210">Select **File** > **New Solution**.</span></span>

![Новое решение macOS](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="098b4-212">Щелкните **.NET Core** > **Приложение** > **Веб-приложение** > **Далее**.</span><span class="sxs-lookup"><span data-stu-id="098b4-212">Select **.NET Core** > **App** > **Web Application** > **Next**.</span></span>

  ![Диалоговое окно "Новый проект" в macOS](razor-pages-start/_static/webapp.png)

* <span data-ttu-id="098b4-214">В диалоговом окне **Настройка нового веб-API ASP.NET Core** установите для параметра **Целевая платформа** значение **.NET Core 3.1**.</span><span class="sxs-lookup"><span data-stu-id="098b4-214">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** to **.NET Core 3.1**.</span></span>

  ![Выбор .NET Core 3.0 для macOS](razor-pages-start/_static/targetframework3.png)

* <span data-ttu-id="098b4-216">Присвойте проекту имя **RazorPagesMovie** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="098b4-216">Name the project **RazorPagesMovie**, and then select **Create**.</span></span>

  ![nameproj](razor-pages-start/_static/RazorPagesMovie.png)

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="098b4-218">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="098b4-218">Run the app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="098b4-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="098b4-219">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="098b4-220">Нажмите клавиши CTRL+F5, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="098b4-220">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="098b4-221">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение.</span><span class="sxs-lookup"><span data-stu-id="098b4-221">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="098b4-222">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="098b4-222">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="098b4-223">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="098b4-223">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="098b4-224">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="098b4-224">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="098b4-225">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="098b4-225">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="098b4-226">На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="098b4-226">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="098b4-227">Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.</span><span class="sxs-lookup"><span data-stu-id="098b4-227">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="098b4-229">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="098b4-229">The following image shows the app after you give consent to tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-code"></a>[<span data-ttu-id="098b4-231">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="098b4-231">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="098b4-232">Нажмите клавиши **CTRL-F5**, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="098b4-232">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="098b4-233">Visual Studio Code запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="098b4-233">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="098b4-234">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="098b4-234">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="098b4-235">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="098b4-235">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="098b4-236">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="098b4-236">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="098b4-237">На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="098b4-237">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="098b4-238">Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.</span><span class="sxs-lookup"><span data-stu-id="098b4-238">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="098b4-240">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="098b4-240">The following image shows the app after you give consent to tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="098b4-242">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="098b4-242">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="098b4-243">Нажмите клавиши **Cmd-Opt-F5**, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="098b4-243">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="098b4-244">Visual Studio запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="098b4-244">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="098b4-245">На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="098b4-245">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="098b4-246">Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.</span><span class="sxs-lookup"><span data-stu-id="098b4-246">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="098b4-248">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="098b4-248">The following image shows the app after you give consent to tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="098b4-250">Анализ файлов проекта</span><span class="sxs-lookup"><span data-stu-id="098b4-250">Examine the project files</span></span>

<span data-ttu-id="098b4-251">Ниже приведен обзор основных папок и файлов проекта, с которыми вы будете работать в последующих учебниках.</span><span class="sxs-lookup"><span data-stu-id="098b4-251">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="098b4-252">Папка Pages</span><span class="sxs-lookup"><span data-stu-id="098b4-252">Pages folder</span></span>

<span data-ttu-id="098b4-253">Содержит страницы Razor и вспомогательные файлы.</span><span class="sxs-lookup"><span data-stu-id="098b4-253">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="098b4-254">Каждая страница Razor — это пара файлов.</span><span class="sxs-lookup"><span data-stu-id="098b4-254">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="098b4-255">Файл *.cshtml*, содержащий HTML-разметку с кодом C# и синтаксисом Razor.</span><span class="sxs-lookup"><span data-stu-id="098b4-255">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="098b4-256">Файл *. cshtml.cs*, содержащий код C#, который обрабатывает события страницы.</span><span class="sxs-lookup"><span data-stu-id="098b4-256">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="098b4-257">Имена вспомогательных файлов начинаются с символа подчеркивания.</span><span class="sxs-lookup"><span data-stu-id="098b4-257">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="098b4-258">Например, файл *_Layout.cshtml* настраивает элементы пользовательского интерфейса, общие для всех страниц.</span><span class="sxs-lookup"><span data-stu-id="098b4-258">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="098b4-259">Этот файл настраивает меню навигации в верхней части страницы и уведомление об авторских правах в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="098b4-259">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="098b4-260">Для получения дополнительной информации см. <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="098b4-260">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="098b4-261">Папка wwwroot</span><span class="sxs-lookup"><span data-stu-id="098b4-261">wwwroot folder</span></span>

<span data-ttu-id="098b4-262">Содержит статические файлы, такие как HTML-файлы, файлы JavaScript и CSS-файлы.</span><span class="sxs-lookup"><span data-stu-id="098b4-262">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="098b4-263">Для получения дополнительной информации см. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="098b4-263">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="098b4-264">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="098b4-264">appSettings.json</span></span>

<span data-ttu-id="098b4-265">Содержит данные конфигурации, например строки подключения.</span><span class="sxs-lookup"><span data-stu-id="098b4-265">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="098b4-266">Для получения дополнительной информации см. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="098b4-266">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="098b4-267">Program.cs</span><span class="sxs-lookup"><span data-stu-id="098b4-267">Program.cs</span></span>

<span data-ttu-id="098b4-268">Содержит точку входа для программы.</span><span class="sxs-lookup"><span data-stu-id="098b4-268">Contains the entry point for the program.</span></span> <span data-ttu-id="098b4-269">Для получения дополнительной информации см. <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="098b4-269">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="098b4-270">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="098b4-270">Startup.cs</span></span>

<span data-ttu-id="098b4-271">Содержит код, который настраивает поведение приложения, например, требуется ли согласие для файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="098b4-271">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="098b4-272">Для получения дополнительной информации см. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="098b4-272">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="098b4-273">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="098b4-273">Additional resources</span></span>

* [<span data-ttu-id="098b4-274">Версия руководства на YouTube</span><span class="sxs-lookup"><span data-stu-id="098b4-274">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="098b4-275">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="098b4-275">Next steps</span></span>

<span data-ttu-id="098b4-276">Перейдите к следующему учебнику в серии:</span><span class="sxs-lookup"><span data-stu-id="098b4-276">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="098b4-277">Добавление модели</span><span class="sxs-lookup"><span data-stu-id="098b4-277">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end
