---
title: Учебник. Начало работы с Razor Pages в ASP.NET Core
author: rick-anderson
description: В этой серии руководств объясняется, как использовать Razor Pages в ASP.NET Core. Узнайте, как создать модель, сгенерировать код для Razor Pages, использовать Entity Framework Core и SQL Server для доступа к данным, добавлять функции поиска и проверки ввода, а также использовать возможность миграции для обновления модели.
ms.author: riande
ms.date: 11/12/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: a8381dee05f267077a29999f3d8bbe6327c2b863
ms.sourcegitcommit: 231780c8d7848943e5e9fd55e93f437f7e5a371d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2019
ms.locfileid: "74116148"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="4cd31-104">Учебник. Начало работы с Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4cd31-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="4cd31-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="4cd31-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="4cd31-106">В этом первом руководстве серии приводятся основные сведения о веб-приложении Razor Pages в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4cd31-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="4cd31-107">Пройдя всю серию, вы получите приложение, которое управляет базой данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="4cd31-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="4cd31-108">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="4cd31-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4cd31-109">Создание веб-приложения Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="4cd31-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="4cd31-110">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="4cd31-110">Run the app.</span></span>
> * <span data-ttu-id="4cd31-111">Анализ файлов проекта.</span><span class="sxs-lookup"><span data-stu-id="4cd31-111">Examine the project files.</span></span>

<span data-ttu-id="4cd31-112">Пройдя это руководство, вы получите рабочее веб-приложение Razor Pages, сборка которого описана в последующих руководствах.</span><span class="sxs-lookup"><span data-stu-id="4cd31-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="4cd31-114">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="4cd31-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4cd31-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd31-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4cd31-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4cd31-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4cd31-117">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="4cd31-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="4cd31-118">Создание веб-приложения Razor Pages</span><span class="sxs-lookup"><span data-stu-id="4cd31-118">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4cd31-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd31-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4cd31-120">В меню **Файл** Visual Studio откройте **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="4cd31-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="4cd31-121">Создайте веб-приложение ASP.NET Core и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="4cd31-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="4cd31-122">![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="4cd31-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="4cd31-123">Назовите проект **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="4cd31-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="4cd31-124">Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.</span><span class="sxs-lookup"><span data-stu-id="4cd31-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="4cd31-125">![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="4cd31-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="4cd31-126">Выберите в раскрывающемся списке пункт **ASP.NET Core 3.0**, затем — **Веб-приложение** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="4cd31-126">Select **ASP.NET Core 3.0** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="4cd31-128">Создается следующий начальный проект:</span><span class="sxs-lookup"><span data-stu-id="4cd31-128">The following starter project is created:</span></span>

  ![обозреватель решений](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4cd31-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4cd31-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="4cd31-131">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="4cd31-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="4cd31-132">Перейдите в каталог `cd`, в котором находится проект.</span><span class="sxs-lookup"><span data-stu-id="4cd31-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="4cd31-133">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="4cd31-133">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="4cd31-134">Команда `dotnet new` создает новый проект Razor Pages в папке *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="4cd31-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="4cd31-135">Команда `code` открывает папку *RazorPagesMovie* в текущем экземпляре Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4cd31-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="4cd31-136">Когда значок строки состояния OmniSharp станет зеленым, появится диалоговое окно с предупреждением **Required assets to build and debug are missing from 'RazorPagesMovie'. (В RazorPagesMovie отсутствуют необходимые ресурсы для сборки и отладки.) Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="4cd31-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="4cd31-137">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="4cd31-137">Select **Yes**.</span></span>

  <span data-ttu-id="4cd31-138">Каталог *.vscode*, содержащий файлы *launch.json* и *tasks.json*, добавляется в корневой каталог проекта.</span><span class="sxs-lookup"><span data-stu-id="4cd31-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4cd31-139">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="4cd31-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="4cd31-140">Выберите **Файл** > **Новое решение**.</span><span class="sxs-lookup"><span data-stu-id="4cd31-140">Select **File** > **New Solution**.</span></span>

![Новое решение macOS](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="4cd31-142">Выберите **.NET Core** > **Приложение** > **Веб-приложение** > **Далее**.</span><span class="sxs-lookup"><span data-stu-id="4cd31-142">Select **.NET Core** > **App** > **Web Application** > **Next**.</span></span>

  ![Диалоговое окно "Новый проект" в macOS](razor-pages-start/_static/webapp.png)

* <span data-ttu-id="4cd31-144">В диалоговом окне **Настройка нового веб-API ASP.NET Core** установите для параметра **Целевая платформа** значение **.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="4cd31-144">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** to **.NET Core 3.0**.</span></span>

  ![Выбор .NET Core 3.0 для macOS](razor-pages-start/_static/targetframework3.png)

* <span data-ttu-id="4cd31-146">Присвойте проекту имя **RazorPagesMovie** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="4cd31-146">Name the project **RazorPagesMovie**, and then select **Create**.</span></span>

  ![nameproj](razor-pages-start/_static/RazorPagesMovie.png)


## <a name="open-the-project"></a><span data-ttu-id="4cd31-148">Открытие проекта</span><span class="sxs-lookup"><span data-stu-id="4cd31-148">Open the project</span></span>

<span data-ttu-id="4cd31-149">В Visual Studio откройте меню **Файл > Открыть файл** и выберите файл *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="4cd31-149">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="4cd31-150">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="4cd31-150">Run the app</span></span>

  [!INCLUDE[](~/includes/run-the-app.md)]

## <a name="examine-the-project-files"></a><span data-ttu-id="4cd31-151">Анализ файлов проекта</span><span class="sxs-lookup"><span data-stu-id="4cd31-151">Examine the project files</span></span>

<span data-ttu-id="4cd31-152">Ниже приведен обзор основных папок и файлов проекта, с которыми вы будете работать в последующих учебниках.</span><span class="sxs-lookup"><span data-stu-id="4cd31-152">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="4cd31-153">Папка Pages</span><span class="sxs-lookup"><span data-stu-id="4cd31-153">Pages folder</span></span>

<span data-ttu-id="4cd31-154">Содержит страницы Razor и вспомогательные файлы.</span><span class="sxs-lookup"><span data-stu-id="4cd31-154">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="4cd31-155">Каждая страница Razor — это пара файлов.</span><span class="sxs-lookup"><span data-stu-id="4cd31-155">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="4cd31-156">Файл *.cshtml*, содержащий HTML-разметку с кодом C# и синтаксисом Razor.</span><span class="sxs-lookup"><span data-stu-id="4cd31-156">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="4cd31-157">Файл *. cshtml.cs*, содержащий код C#, который обрабатывает события страницы.</span><span class="sxs-lookup"><span data-stu-id="4cd31-157">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="4cd31-158">Имена вспомогательных файлов начинаются с символа подчеркивания.</span><span class="sxs-lookup"><span data-stu-id="4cd31-158">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="4cd31-159">Например, файл *_Layout.cshtml* настраивает элементы пользовательского интерфейса, общие для всех страниц.</span><span class="sxs-lookup"><span data-stu-id="4cd31-159">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="4cd31-160">Этот файл настраивает меню навигации в верхней части страницы и уведомление об авторских правах в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="4cd31-160">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="4cd31-161">Дополнительные сведения можно найти по адресу: <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="4cd31-161">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="4cd31-162">Папка wwwroot</span><span class="sxs-lookup"><span data-stu-id="4cd31-162">wwwroot folder</span></span>

<span data-ttu-id="4cd31-163">Содержит статические файлы, такие как HTML-файлы, файлы JavaScript и CSS-файлы.</span><span class="sxs-lookup"><span data-stu-id="4cd31-163">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="4cd31-164">Дополнительные сведения можно найти по адресу: <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="4cd31-164">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="4cd31-165">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="4cd31-165">appSettings.json</span></span>

<span data-ttu-id="4cd31-166">Содержит данные конфигурации, например строки подключения.</span><span class="sxs-lookup"><span data-stu-id="4cd31-166">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="4cd31-167">Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="4cd31-167">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="4cd31-168">Program.cs</span><span class="sxs-lookup"><span data-stu-id="4cd31-168">Program.cs</span></span>

<span data-ttu-id="4cd31-169">Содержит точку входа для программы.</span><span class="sxs-lookup"><span data-stu-id="4cd31-169">Contains the entry point for the program.</span></span> <span data-ttu-id="4cd31-170">Дополнительные сведения можно найти по адресу: <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="4cd31-170">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="4cd31-171">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="4cd31-171">Startup.cs</span></span>

<span data-ttu-id="4cd31-172">содержит код, задающий поведение приложения.</span><span class="sxs-lookup"><span data-stu-id="4cd31-172">Contains code that configures app behavior.</span></span> <span data-ttu-id="4cd31-173">Дополнительные сведения можно найти по адресу: <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="4cd31-173">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4cd31-174">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="4cd31-174">Next steps</span></span>

<span data-ttu-id="4cd31-175">Перейдите к следующему учебнику в серии:</span><span class="sxs-lookup"><span data-stu-id="4cd31-175">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4cd31-176">Добавление модели</span><span class="sxs-lookup"><span data-stu-id="4cd31-176">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="4cd31-177">Это первый учебник из серии.</span><span class="sxs-lookup"><span data-stu-id="4cd31-177">This is the first tutorial of a series.</span></span> <span data-ttu-id="4cd31-178">В этой [серии](xref:tutorials/razor-pages/index) приводятся основные сведения о сборке веб-приложения Razor Pages в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4cd31-178">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="4cd31-179">Пройдя всю серию, вы получите приложение, которое управляет базой данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="4cd31-179">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="4cd31-180">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="4cd31-180">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4cd31-181">Создание веб-приложения Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="4cd31-181">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="4cd31-182">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="4cd31-182">Run the app.</span></span>
> * <span data-ttu-id="4cd31-183">Анализ файлов проекта.</span><span class="sxs-lookup"><span data-stu-id="4cd31-183">Examine the project files.</span></span>

<span data-ttu-id="4cd31-184">Пройдя это руководство, вы получите рабочее веб-приложение Razor Pages, сборка которого описана в последующих руководствах.</span><span class="sxs-lookup"><span data-stu-id="4cd31-184">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="4cd31-186">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="4cd31-186">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4cd31-187">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd31-187">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4cd31-188">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4cd31-188">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4cd31-189">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="4cd31-189">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="4cd31-190">Создание веб-приложения Razor Pages</span><span class="sxs-lookup"><span data-stu-id="4cd31-190">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4cd31-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd31-191">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4cd31-192">В меню **Файл** Visual Studio откройте **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="4cd31-192">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="4cd31-193">Создайте веб-приложение ASP.NET Core и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="4cd31-193">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="4cd31-195">Назовите проект **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="4cd31-195">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="4cd31-196">Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.</span><span class="sxs-lookup"><span data-stu-id="4cd31-196">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="4cd31-198">Выберите в раскрывающемся списке пункт **ASP.NET Core 2.2**, затем — **Веб-приложение** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="4cd31-198">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="4cd31-200">Создается следующий начальный проект:</span><span class="sxs-lookup"><span data-stu-id="4cd31-200">The following starter project is created:</span></span>

  ![обозреватель решений](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4cd31-202">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4cd31-202">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="4cd31-203">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="4cd31-203">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="4cd31-204">Перейдите в каталог `cd`, в котором находится проект.</span><span class="sxs-lookup"><span data-stu-id="4cd31-204">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="4cd31-205">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="4cd31-205">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="4cd31-206">Команда `dotnet new` создает новый проект Razor Pages в папке *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="4cd31-206">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="4cd31-207">Команда `code` открывает папку *RazorPagesMovie* в текущем экземпляре Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4cd31-207">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="4cd31-208">Когда значок строки состояния OmniSharp станет зеленым, появится диалоговое окно с предупреждением **Required assets to build and debug are missing from 'RazorPagesMovie'. (В RazorPagesMovie отсутствуют необходимые ресурсы для сборки и отладки.) Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="4cd31-208">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="4cd31-209">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="4cd31-209">Select **Yes**.</span></span>

  <span data-ttu-id="4cd31-210">Каталог *.vscode*, содержащий файлы *launch.json* и *tasks.json*, добавляется в корневой каталог проекта.</span><span class="sxs-lookup"><span data-stu-id="4cd31-210">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4cd31-211">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="4cd31-211">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="4cd31-212">В терминале выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="4cd31-212">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```dotnetcli
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="4cd31-213">Указанные выше команды используют [интерфейс командной строки .NET Core](/dotnet/core/tools/dotnet) для создания проекта Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="4cd31-213">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="4cd31-214">Открытие проекта</span><span class="sxs-lookup"><span data-stu-id="4cd31-214">Open the project</span></span>

<span data-ttu-id="4cd31-215">В Visual Studio откройте меню **Файл > Открыть файл** и выберите файл *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="4cd31-215">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="4cd31-216">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="4cd31-216">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4cd31-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd31-217">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4cd31-218">Нажмите клавиши CTRL+F5, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="4cd31-218">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="4cd31-219">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение.</span><span class="sxs-lookup"><span data-stu-id="4cd31-219">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="4cd31-220">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="4cd31-220">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="4cd31-221">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="4cd31-221">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="4cd31-222">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="4cd31-222">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="4cd31-223">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="4cd31-223">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="4cd31-224">На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="4cd31-224">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="4cd31-225">Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.</span><span class="sxs-lookup"><span data-stu-id="4cd31-225">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="4cd31-227">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="4cd31-227">The following image shows the app after you give consent to tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4cd31-229">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4cd31-229">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="4cd31-230">Нажмите клавиши **CTRL-F5**, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="4cd31-230">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="4cd31-231">Visual Studio Code запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="4cd31-231">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="4cd31-232">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="4cd31-232">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="4cd31-233">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="4cd31-233">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="4cd31-234">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="4cd31-234">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="4cd31-235">На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="4cd31-235">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="4cd31-236">Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.</span><span class="sxs-lookup"><span data-stu-id="4cd31-236">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="4cd31-238">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="4cd31-238">The following image shows the app after you give consent to tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4cd31-240">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="4cd31-240">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="4cd31-241">Нажмите клавиши **Cmd-Opt-F5**, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="4cd31-241">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="4cd31-242">Visual Studio запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="4cd31-242">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="4cd31-243">На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="4cd31-243">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="4cd31-244">Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.</span><span class="sxs-lookup"><span data-stu-id="4cd31-244">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="4cd31-246">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="4cd31-246">The following image shows the app after you give consent to tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="4cd31-248">Анализ файлов проекта</span><span class="sxs-lookup"><span data-stu-id="4cd31-248">Examine the project files</span></span>

<span data-ttu-id="4cd31-249">Ниже приведен обзор основных папок и файлов проекта, с которыми вы будете работать в последующих учебниках.</span><span class="sxs-lookup"><span data-stu-id="4cd31-249">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="4cd31-250">Папка Pages</span><span class="sxs-lookup"><span data-stu-id="4cd31-250">Pages folder</span></span>

<span data-ttu-id="4cd31-251">Содержит страницы Razor и вспомогательные файлы.</span><span class="sxs-lookup"><span data-stu-id="4cd31-251">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="4cd31-252">Каждая страница Razor — это пара файлов.</span><span class="sxs-lookup"><span data-stu-id="4cd31-252">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="4cd31-253">Файл *.cshtml*, содержащий HTML-разметку с кодом C# и синтаксисом Razor.</span><span class="sxs-lookup"><span data-stu-id="4cd31-253">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="4cd31-254">Файл *. cshtml.cs*, содержащий код C#, который обрабатывает события страницы.</span><span class="sxs-lookup"><span data-stu-id="4cd31-254">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="4cd31-255">Имена вспомогательных файлов начинаются с символа подчеркивания.</span><span class="sxs-lookup"><span data-stu-id="4cd31-255">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="4cd31-256">Например, файл *_Layout.cshtml* настраивает элементы пользовательского интерфейса, общие для всех страниц.</span><span class="sxs-lookup"><span data-stu-id="4cd31-256">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="4cd31-257">Этот файл настраивает меню навигации в верхней части страницы и уведомление об авторских правах в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="4cd31-257">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="4cd31-258">Дополнительные сведения можно найти по адресу: <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="4cd31-258">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="4cd31-259">Папка wwwroot</span><span class="sxs-lookup"><span data-stu-id="4cd31-259">wwwroot folder</span></span>

<span data-ttu-id="4cd31-260">Содержит статические файлы, такие как HTML-файлы, файлы JavaScript и CSS-файлы.</span><span class="sxs-lookup"><span data-stu-id="4cd31-260">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="4cd31-261">Дополнительные сведения можно найти по адресу: <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="4cd31-261">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="4cd31-262">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="4cd31-262">appSettings.json</span></span>

<span data-ttu-id="4cd31-263">Содержит данные конфигурации, например строки подключения.</span><span class="sxs-lookup"><span data-stu-id="4cd31-263">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="4cd31-264">Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="4cd31-264">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="4cd31-265">Program.cs</span><span class="sxs-lookup"><span data-stu-id="4cd31-265">Program.cs</span></span>

<span data-ttu-id="4cd31-266">Содержит точку входа для программы.</span><span class="sxs-lookup"><span data-stu-id="4cd31-266">Contains the entry point for the program.</span></span> <span data-ttu-id="4cd31-267">Дополнительные сведения можно найти по адресу: <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="4cd31-267">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="4cd31-268">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="4cd31-268">Startup.cs</span></span>

<span data-ttu-id="4cd31-269">Содержит код, который настраивает поведение приложения, например, требуется ли согласие для файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="4cd31-269">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="4cd31-270">Дополнительные сведения можно найти по адресу: <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="4cd31-270">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4cd31-271">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="4cd31-271">Additional resources</span></span>

* [<span data-ttu-id="4cd31-272">Версия руководства на YouTube</span><span class="sxs-lookup"><span data-stu-id="4cd31-272">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="4cd31-273">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="4cd31-273">Next steps</span></span>

<span data-ttu-id="4cd31-274">Перейдите к следующему учебнику в серии:</span><span class="sxs-lookup"><span data-stu-id="4cd31-274">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4cd31-275">Добавление модели</span><span class="sxs-lookup"><span data-stu-id="4cd31-275">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end
