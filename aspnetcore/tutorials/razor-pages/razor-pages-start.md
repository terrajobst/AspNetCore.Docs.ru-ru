---
title: Учебник. Начало работы с Razor Pages в ASP.NET Core
author: rick-anderson
description: В этой серии руководств объясняется, как использовать Razor Pages в ASP.NET Core. Узнайте, как создать модель, сгенерировать код для Razor Pages, использовать Entity Framework Core и SQL Server для доступа к данным, добавлять функции поиска и проверки ввода, а также использовать возможность миграции для обновления модели.
ms.author: riande
ms.date: 07/25/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1605197188d97f27a884739a72400da2d5818b1a
ms.sourcegitcommit: 849af69ee3c94cdb9fd8fa1f1bb8f5a5dda7b9eb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2019
ms.locfileid: "68371989"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="9c493-104">Учебник. Начало работы с Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9c493-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="9c493-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="9c493-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="9c493-106">В этом первом руководстве серии приводятся основные сведения о веб-приложении Razor Pages в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9c493-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="9c493-107">Пройдя всю серию, вы получите приложение, которое управляет базой данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="9c493-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="9c493-108">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="9c493-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9c493-109">Создание веб-приложения Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="9c493-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="9c493-110">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="9c493-110">Run the app.</span></span>
> * <span data-ttu-id="9c493-111">Анализ файлов проекта.</span><span class="sxs-lookup"><span data-stu-id="9c493-111">Examine the project files.</span></span>

<span data-ttu-id="9c493-112">Пройдя это руководство, вы получите рабочее веб-приложение Razor Pages, сборка которого описана в последующих руководствах.</span><span class="sxs-lookup"><span data-stu-id="9c493-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="9c493-114">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="9c493-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9c493-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9c493-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9c493-116">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="9c493-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9c493-117">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="9c493-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="9c493-118">Создание веб-приложения Razor Pages</span><span class="sxs-lookup"><span data-stu-id="9c493-118">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9c493-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9c493-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9c493-120">В меню **Файл** Visual Studio откройте **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="9c493-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="9c493-121">Создайте веб-приложение ASP.NET Core и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="9c493-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="9c493-122">![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="9c493-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="9c493-123">Назовите проект **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="9c493-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="9c493-124">Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.</span><span class="sxs-lookup"><span data-stu-id="9c493-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="9c493-125">![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="9c493-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="9c493-126">Выберите в раскрывающемся списке пункт **ASP.NET Core 3.0**, затем — **Веб-приложение** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="9c493-126">Select **ASP.NET Core 3.0** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="9c493-128">Создается следующий начальный проект:</span><span class="sxs-lookup"><span data-stu-id="9c493-128">The following starter project is created:</span></span>

  ![обозреватель решений](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9c493-130">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="9c493-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="9c493-131">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="9c493-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="9c493-132">Перейдите в каталог `cd`, в котором находится проект.</span><span class="sxs-lookup"><span data-stu-id="9c493-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="9c493-133">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="9c493-133">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="9c493-134">Команда `dotnet new` создает новый проект Razor Pages в папке *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="9c493-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="9c493-135">Команда `code` открывает папку *RazorPagesMovie* в текущем экземпляре Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="9c493-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="9c493-136">Когда значок строки состояния OmniSharp станет зеленым, появится диалоговое окно с предупреждением **Required assets to build and debug are missing from 'RazorPagesMovie'. (В RazorPagesMovie отсутствуют необходимые ресурсы для сборки и отладки.) Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="9c493-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="9c493-137">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="9c493-137">Select **Yes**.</span></span>

  <span data-ttu-id="9c493-138">Каталог *.vscode*, содержащий файлы *launch.json* и *tasks.json*, добавляется в корневой каталог проекта.</span><span class="sxs-lookup"><span data-stu-id="9c493-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9c493-139">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="9c493-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="9c493-140">В терминале выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="9c493-140">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="9c493-141">Указанные выше команды используют [интерфейс командной строки .NET Core](/dotnet/core/tools/dotnet) для создания проекта Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="9c493-141">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="9c493-142">Открытие проекта</span><span class="sxs-lookup"><span data-stu-id="9c493-142">Open the project</span></span>

<span data-ttu-id="9c493-143">В Visual Studio откройте меню **Файл > Открыть файл** и выберите файл *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="9c493-143">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="9c493-144">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="9c493-144">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9c493-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9c493-145">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9c493-146">Нажмите клавиши CTRL+F5, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="9c493-146">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="9c493-147">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение.</span><span class="sxs-lookup"><span data-stu-id="9c493-147">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="9c493-148">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="9c493-148">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="9c493-149">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="9c493-149">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="9c493-150">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="9c493-150">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="9c493-151">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="9c493-151">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
 
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9c493-152">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="9c493-152">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="9c493-153">Нажмите клавиши **CTRL-F5**, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="9c493-153">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="9c493-154">Visual Studio Code запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="9c493-154">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="9c493-155">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="9c493-155">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="9c493-156">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="9c493-156">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="9c493-157">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="9c493-157">Localhost only serves web requests from the local computer.</span></span>

  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9c493-158">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="9c493-158">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="9c493-159">Нажмите клавиши **Cmd-Opt-F5**, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="9c493-159">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="9c493-160">Visual Studio запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="9c493-160">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="9c493-161">Анализ файлов проекта</span><span class="sxs-lookup"><span data-stu-id="9c493-161">Examine the project files</span></span>

<span data-ttu-id="9c493-162">Ниже приведен обзор основных папок и файлов проекта, с которыми вы будете работать в последующих учебниках.</span><span class="sxs-lookup"><span data-stu-id="9c493-162">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="9c493-163">Папка Pages</span><span class="sxs-lookup"><span data-stu-id="9c493-163">Pages folder</span></span>

<span data-ttu-id="9c493-164">Содержит страницы Razor и вспомогательные файлы.</span><span class="sxs-lookup"><span data-stu-id="9c493-164">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="9c493-165">Каждая страница Razor — это пара файлов.</span><span class="sxs-lookup"><span data-stu-id="9c493-165">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="9c493-166">Файл *.cshtml*, содержащий HTML-разметку с кодом C# и синтаксисом Razor.</span><span class="sxs-lookup"><span data-stu-id="9c493-166">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="9c493-167">Файл *. cshtml.cs*, содержащий код C#, который обрабатывает события страницы.</span><span class="sxs-lookup"><span data-stu-id="9c493-167">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="9c493-168">Имена вспомогательных файлов начинаются с символа подчеркивания.</span><span class="sxs-lookup"><span data-stu-id="9c493-168">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="9c493-169">Например, файл *_Layout.cshtml* настраивает элементы пользовательского интерфейса, общие для всех страниц.</span><span class="sxs-lookup"><span data-stu-id="9c493-169">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="9c493-170">Этот файл настраивает меню навигации в верхней части страницы и уведомление об авторских правах в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="9c493-170">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="9c493-171">Дополнительные сведения можно найти по адресу: <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="9c493-171">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="9c493-172">Папка wwwroot</span><span class="sxs-lookup"><span data-stu-id="9c493-172">wwwroot folder</span></span>

<span data-ttu-id="9c493-173">Содержит статические файлы, такие как HTML-файлы, файлы JavaScript и CSS-файлы.</span><span class="sxs-lookup"><span data-stu-id="9c493-173">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="9c493-174">Дополнительные сведения можно найти по адресу: <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="9c493-174">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="9c493-175">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="9c493-175">appSettings.json</span></span>

<span data-ttu-id="9c493-176">Содержит данные конфигурации, например строки подключения.</span><span class="sxs-lookup"><span data-stu-id="9c493-176">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="9c493-177">Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="9c493-177">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="9c493-178">Program.cs</span><span class="sxs-lookup"><span data-stu-id="9c493-178">Program.cs</span></span>

<span data-ttu-id="9c493-179">Содержит точку входа для программы.</span><span class="sxs-lookup"><span data-stu-id="9c493-179">Contains the entry point for the program.</span></span> <span data-ttu-id="9c493-180">Дополнительные сведения можно найти по адресу: <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="9c493-180">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="9c493-181">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="9c493-181">Startup.cs</span></span>

<span data-ttu-id="9c493-182">содержит код, задающий поведение приложения.</span><span class="sxs-lookup"><span data-stu-id="9c493-182">Contains code that configures app behavior.</span></span> <span data-ttu-id="9c493-183">Дополнительные сведения можно найти по адресу: <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="9c493-183">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9c493-184">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="9c493-184">Next steps</span></span>

<span data-ttu-id="9c493-185">Перейдите к следующему учебнику в серии:</span><span class="sxs-lookup"><span data-stu-id="9c493-185">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9c493-186">Добавление модели</span><span class="sxs-lookup"><span data-stu-id="9c493-186">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9c493-187">Это первый учебник из серии.</span><span class="sxs-lookup"><span data-stu-id="9c493-187">This is the first tutorial of a series.</span></span> <span data-ttu-id="9c493-188">В этой [серии](xref:tutorials/razor-pages/index) приводятся основные сведения о сборке веб-приложения Razor Pages в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9c493-188">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="9c493-189">Пройдя всю серию, вы получите приложение, которое управляет базой данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="9c493-189">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="9c493-190">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="9c493-190">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9c493-191">Создание веб-приложения Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="9c493-191">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="9c493-192">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="9c493-192">Run the app.</span></span>
> * <span data-ttu-id="9c493-193">Анализ файлов проекта.</span><span class="sxs-lookup"><span data-stu-id="9c493-193">Examine the project files.</span></span>

<span data-ttu-id="9c493-194">Пройдя это руководство, вы получите рабочее веб-приложение Razor Pages, сборка которого описана в последующих руководствах.</span><span class="sxs-lookup"><span data-stu-id="9c493-194">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="9c493-196">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="9c493-196">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9c493-197">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9c493-197">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9c493-198">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="9c493-198">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9c493-199">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="9c493-199">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="9c493-200">Создание веб-приложения Razor Pages</span><span class="sxs-lookup"><span data-stu-id="9c493-200">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9c493-201">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9c493-201">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9c493-202">В меню **Файл** Visual Studio откройте **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="9c493-202">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="9c493-203">Создайте веб-приложение ASP.NET Core и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="9c493-203">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="9c493-205">Назовите проект **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="9c493-205">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="9c493-206">Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.</span><span class="sxs-lookup"><span data-stu-id="9c493-206">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="9c493-208">Выберите в раскрывающемся списке пункт **ASP.NET Core 2.2**, затем — **Веб-приложение** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="9c493-208">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="9c493-210">Создается следующий начальный проект:</span><span class="sxs-lookup"><span data-stu-id="9c493-210">The following starter project is created:</span></span>

  ![обозреватель решений](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9c493-212">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="9c493-212">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="9c493-213">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="9c493-213">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="9c493-214">Перейдите в каталог `cd`, в котором находится проект.</span><span class="sxs-lookup"><span data-stu-id="9c493-214">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="9c493-215">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="9c493-215">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="9c493-216">Команда `dotnet new` создает новый проект Razor Pages в папке *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="9c493-216">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="9c493-217">Команда `code` открывает папку *RazorPagesMovie* в текущем экземпляре Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="9c493-217">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="9c493-218">Когда значок строки состояния OmniSharp станет зеленым, появится диалоговое окно с предупреждением **Required assets to build and debug are missing from 'RazorPagesMovie'. (В RazorPagesMovie отсутствуют необходимые ресурсы для сборки и отладки.) Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="9c493-218">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="9c493-219">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="9c493-219">Select **Yes**.</span></span>

  <span data-ttu-id="9c493-220">Каталог *.vscode*, содержащий файлы *launch.json* и *tasks.json*, добавляется в корневой каталог проекта.</span><span class="sxs-lookup"><span data-stu-id="9c493-220">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9c493-221">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="9c493-221">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="9c493-222">В терминале выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="9c493-222">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="9c493-223">Указанные выше команды используют [интерфейс командной строки .NET Core](/dotnet/core/tools/dotnet) для создания проекта Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="9c493-223">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="9c493-224">Открытие проекта</span><span class="sxs-lookup"><span data-stu-id="9c493-224">Open the project</span></span>

<span data-ttu-id="9c493-225">В Visual Studio откройте меню **Файл > Открыть файл** и выберите файл *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="9c493-225">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="9c493-226">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="9c493-226">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9c493-227">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9c493-227">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9c493-228">Нажмите клавиши CTRL+F5, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="9c493-228">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="9c493-229">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение.</span><span class="sxs-lookup"><span data-stu-id="9c493-229">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="9c493-230">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="9c493-230">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="9c493-231">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="9c493-231">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="9c493-232">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="9c493-232">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="9c493-233">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="9c493-233">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="9c493-234">На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="9c493-234">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="9c493-235">Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.</span><span class="sxs-lookup"><span data-stu-id="9c493-235">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="9c493-237">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="9c493-237">The following image shows the app after you give consent to tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9c493-239">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="9c493-239">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="9c493-240">Нажмите клавиши **CTRL-F5**, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="9c493-240">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="9c493-241">Visual Studio Code запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="9c493-241">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="9c493-242">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="9c493-242">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="9c493-243">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="9c493-243">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="9c493-244">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="9c493-244">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="9c493-245">На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="9c493-245">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="9c493-246">Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.</span><span class="sxs-lookup"><span data-stu-id="9c493-246">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="9c493-248">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="9c493-248">The following image shows the app after you give consent to tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9c493-250">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="9c493-250">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="9c493-251">Нажмите клавиши **Cmd-Opt-F5**, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="9c493-251">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="9c493-252">Visual Studio запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="9c493-252">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="9c493-253">На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="9c493-253">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="9c493-254">Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.</span><span class="sxs-lookup"><span data-stu-id="9c493-254">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="9c493-256">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="9c493-256">The following image shows the app after you give consent to tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="9c493-258">Анализ файлов проекта</span><span class="sxs-lookup"><span data-stu-id="9c493-258">Examine the project files</span></span>

<span data-ttu-id="9c493-259">Ниже приведен обзор основных папок и файлов проекта, с которыми вы будете работать в последующих учебниках.</span><span class="sxs-lookup"><span data-stu-id="9c493-259">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="9c493-260">Папка Pages</span><span class="sxs-lookup"><span data-stu-id="9c493-260">Pages folder</span></span>

<span data-ttu-id="9c493-261">Содержит страницы Razor и вспомогательные файлы.</span><span class="sxs-lookup"><span data-stu-id="9c493-261">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="9c493-262">Каждая страница Razor — это пара файлов.</span><span class="sxs-lookup"><span data-stu-id="9c493-262">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="9c493-263">Файл *.cshtml*, содержащий HTML-разметку с кодом C# и синтаксисом Razor.</span><span class="sxs-lookup"><span data-stu-id="9c493-263">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="9c493-264">Файл *. cshtml.cs*, содержащий код C#, который обрабатывает события страницы.</span><span class="sxs-lookup"><span data-stu-id="9c493-264">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="9c493-265">Имена вспомогательных файлов начинаются с символа подчеркивания.</span><span class="sxs-lookup"><span data-stu-id="9c493-265">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="9c493-266">Например, файл *_Layout.cshtml* настраивает элементы пользовательского интерфейса, общие для всех страниц.</span><span class="sxs-lookup"><span data-stu-id="9c493-266">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="9c493-267">Этот файл настраивает меню навигации в верхней части страницы и уведомление об авторских правах в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="9c493-267">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="9c493-268">Дополнительные сведения можно найти по адресу: <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="9c493-268">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="9c493-269">Папка wwwroot</span><span class="sxs-lookup"><span data-stu-id="9c493-269">wwwroot folder</span></span>

<span data-ttu-id="9c493-270">Содержит статические файлы, такие как HTML-файлы, файлы JavaScript и CSS-файлы.</span><span class="sxs-lookup"><span data-stu-id="9c493-270">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="9c493-271">Дополнительные сведения можно найти по адресу: <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="9c493-271">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="9c493-272">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="9c493-272">appSettings.json</span></span>

<span data-ttu-id="9c493-273">Содержит данные конфигурации, например строки подключения.</span><span class="sxs-lookup"><span data-stu-id="9c493-273">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="9c493-274">Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="9c493-274">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="9c493-275">Program.cs</span><span class="sxs-lookup"><span data-stu-id="9c493-275">Program.cs</span></span>

<span data-ttu-id="9c493-276">Содержит точку входа для программы.</span><span class="sxs-lookup"><span data-stu-id="9c493-276">Contains the entry point for the program.</span></span> <span data-ttu-id="9c493-277">Дополнительные сведения можно найти по адресу: <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="9c493-277">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="9c493-278">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="9c493-278">Startup.cs</span></span>

<span data-ttu-id="9c493-279">Содержит код, который настраивает поведение приложения, например, требуется ли согласие для файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="9c493-279">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="9c493-280">Дополнительные сведения можно найти по адресу: <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="9c493-280">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9c493-281">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="9c493-281">Additional resources</span></span>

* [<span data-ttu-id="9c493-282">Версия руководства на YouTube</span><span class="sxs-lookup"><span data-stu-id="9c493-282">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="9c493-283">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="9c493-283">Next steps</span></span>

<span data-ttu-id="9c493-284">Перейдите к следующему учебнику в серии:</span><span class="sxs-lookup"><span data-stu-id="9c493-284">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9c493-285">Добавление модели</span><span class="sxs-lookup"><span data-stu-id="9c493-285">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end