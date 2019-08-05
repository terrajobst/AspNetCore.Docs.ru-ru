---
title: Учебник. Начало работы с Razor Pages в ASP.NET Core
author: rick-anderson
description: В этой серии руководств объясняется, как использовать Razor Pages в ASP.NET Core. Узнайте, как создать модель, сгенерировать код для Razor Pages, использовать Entity Framework Core и SQL Server для доступа к данным, добавлять функции поиска и проверки ввода, а также использовать возможность миграции для обновления модели.
ms.author: riande
ms.date: 07/25/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 57a10895c718c539ece280afcb27cb4033c7fb45
ms.sourcegitcommit: 979dbfc5e9ce09b9470789989cddfcfb57079d94
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682795"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="5994f-104">Учебник. Начало работы с Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5994f-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="5994f-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="5994f-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="5994f-106">В этом первом руководстве серии приводятся основные сведения о веб-приложении Razor Pages в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5994f-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="5994f-107">Пройдя всю серию, вы получите приложение, которое управляет базой данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="5994f-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="5994f-108">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="5994f-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5994f-109">Создание веб-приложения Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="5994f-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="5994f-110">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="5994f-110">Run the app.</span></span>
> * <span data-ttu-id="5994f-111">Анализ файлов проекта.</span><span class="sxs-lookup"><span data-stu-id="5994f-111">Examine the project files.</span></span>

<span data-ttu-id="5994f-112">Пройдя это руководство, вы получите рабочее веб-приложение Razor Pages, сборка которого описана в последующих руководствах.</span><span class="sxs-lookup"><span data-stu-id="5994f-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="5994f-114">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="5994f-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5994f-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5994f-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5994f-116">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5994f-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5994f-117">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="5994f-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="5994f-118">Создание веб-приложения Razor Pages</span><span class="sxs-lookup"><span data-stu-id="5994f-118">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5994f-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5994f-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5994f-120">В меню **Файл** Visual Studio откройте **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="5994f-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="5994f-121">Создайте веб-приложение ASP.NET Core и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="5994f-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="5994f-122">![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="5994f-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="5994f-123">Назовите проект **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="5994f-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="5994f-124">Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.</span><span class="sxs-lookup"><span data-stu-id="5994f-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="5994f-125">![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="5994f-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="5994f-126">Выберите в раскрывающемся списке пункт **ASP.NET Core 3.0**, затем — **Веб-приложение** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="5994f-126">Select **ASP.NET Core 3.0** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="5994f-128">Создается следующий начальный проект:</span><span class="sxs-lookup"><span data-stu-id="5994f-128">The following starter project is created:</span></span>

  ![обозреватель решений](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5994f-130">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5994f-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="5994f-131">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="5994f-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="5994f-132">Перейдите в каталог `cd`, в котором находится проект.</span><span class="sxs-lookup"><span data-stu-id="5994f-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="5994f-133">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="5994f-133">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="5994f-134">Команда `dotnet new` создает новый проект Razor Pages в папке *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="5994f-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="5994f-135">Команда `code` открывает папку *RazorPagesMovie* в текущем экземпляре Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5994f-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="5994f-136">Когда значок строки состояния OmniSharp станет зеленым, появится диалоговое окно с предупреждением **Required assets to build and debug are missing from 'RazorPagesMovie'. (В RazorPagesMovie отсутствуют необходимые ресурсы для сборки и отладки.) Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="5994f-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="5994f-137">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="5994f-137">Select **Yes**.</span></span>

  <span data-ttu-id="5994f-138">Каталог *.vscode*, содержащий файлы *launch.json* и *tasks.json*, добавляется в корневой каталог проекта.</span><span class="sxs-lookup"><span data-stu-id="5994f-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5994f-139">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="5994f-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="5994f-140">В терминале выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="5994f-140">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="5994f-141">Указанные выше команды используют [интерфейс командной строки .NET Core](/dotnet/core/tools/dotnet) для создания проекта Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="5994f-141">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="5994f-142">Открытие проекта</span><span class="sxs-lookup"><span data-stu-id="5994f-142">Open the project</span></span>

<span data-ttu-id="5994f-143">В Visual Studio откройте меню **Файл > Открыть файл** и выберите файл *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="5994f-143">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="5994f-144">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="5994f-144">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5994f-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5994f-145">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5994f-146">Нажмите клавиши CTRL+F5, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="5994f-146">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="5994f-147">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение.</span><span class="sxs-lookup"><span data-stu-id="5994f-147">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="5994f-148">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="5994f-148">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="5994f-149">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="5994f-149">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="5994f-150">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="5994f-150">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="5994f-151">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="5994f-151">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
 
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5994f-152">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5994f-152">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="5994f-153">Нажмите клавиши **CTRL-F5**, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="5994f-153">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="5994f-154">Visual Studio Code запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="5994f-154">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="5994f-155">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="5994f-155">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="5994f-156">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="5994f-156">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="5994f-157">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="5994f-157">Localhost only serves web requests from the local computer.</span></span>

  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5994f-158">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="5994f-158">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="5994f-159">Чтобы выполнить запуск без отладчика, нажмите клавиши **ALT+CMD+ВВОД**.</span><span class="sxs-lookup"><span data-stu-id="5994f-159">Press **Alt-Cmd-Enter** to run without the debugger.</span></span> <span data-ttu-id="5994f-160">Вы также можете в строке меню выбрать "Запуск" > "Запуск без отладки".</span><span class="sxs-lookup"><span data-stu-id="5994f-160">Alternatively, navigate to the menu bar and go to Run>Start Without Debugging.</span></span>

  <span data-ttu-id="5994f-161">Visual Studio запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="5994f-161">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="5994f-162">Анализ файлов проекта</span><span class="sxs-lookup"><span data-stu-id="5994f-162">Examine the project files</span></span>

<span data-ttu-id="5994f-163">Ниже приведен обзор основных папок и файлов проекта, с которыми вы будете работать в последующих учебниках.</span><span class="sxs-lookup"><span data-stu-id="5994f-163">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="5994f-164">Папка Pages</span><span class="sxs-lookup"><span data-stu-id="5994f-164">Pages folder</span></span>

<span data-ttu-id="5994f-165">Содержит страницы Razor и вспомогательные файлы.</span><span class="sxs-lookup"><span data-stu-id="5994f-165">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="5994f-166">Каждая страница Razor — это пара файлов.</span><span class="sxs-lookup"><span data-stu-id="5994f-166">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="5994f-167">Файл *.cshtml*, содержащий HTML-разметку с кодом C# и синтаксисом Razor.</span><span class="sxs-lookup"><span data-stu-id="5994f-167">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="5994f-168">Файл *. cshtml.cs*, содержащий код C#, который обрабатывает события страницы.</span><span class="sxs-lookup"><span data-stu-id="5994f-168">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="5994f-169">Имена вспомогательных файлов начинаются с символа подчеркивания.</span><span class="sxs-lookup"><span data-stu-id="5994f-169">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="5994f-170">Например, файл *_Layout.cshtml* настраивает элементы пользовательского интерфейса, общие для всех страниц.</span><span class="sxs-lookup"><span data-stu-id="5994f-170">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="5994f-171">Этот файл настраивает меню навигации в верхней части страницы и уведомление об авторских правах в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="5994f-171">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="5994f-172">Дополнительные сведения можно найти по адресу: <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="5994f-172">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="5994f-173">Папка wwwroot</span><span class="sxs-lookup"><span data-stu-id="5994f-173">wwwroot folder</span></span>

<span data-ttu-id="5994f-174">Содержит статические файлы, такие как HTML-файлы, файлы JavaScript и CSS-файлы.</span><span class="sxs-lookup"><span data-stu-id="5994f-174">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="5994f-175">Дополнительные сведения можно найти по адресу: <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="5994f-175">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="5994f-176">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="5994f-176">appSettings.json</span></span>

<span data-ttu-id="5994f-177">Содержит данные конфигурации, например строки подключения.</span><span class="sxs-lookup"><span data-stu-id="5994f-177">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="5994f-178">Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="5994f-178">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="5994f-179">Program.cs</span><span class="sxs-lookup"><span data-stu-id="5994f-179">Program.cs</span></span>

<span data-ttu-id="5994f-180">Содержит точку входа для программы.</span><span class="sxs-lookup"><span data-stu-id="5994f-180">Contains the entry point for the program.</span></span> <span data-ttu-id="5994f-181">Дополнительные сведения можно найти по адресу: <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="5994f-181">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="5994f-182">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="5994f-182">Startup.cs</span></span>

<span data-ttu-id="5994f-183">содержит код, задающий поведение приложения.</span><span class="sxs-lookup"><span data-stu-id="5994f-183">Contains code that configures app behavior.</span></span> <span data-ttu-id="5994f-184">Дополнительные сведения можно найти по адресу: <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="5994f-184">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5994f-185">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="5994f-185">Next steps</span></span>

<span data-ttu-id="5994f-186">Перейдите к следующему учебнику в серии:</span><span class="sxs-lookup"><span data-stu-id="5994f-186">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="5994f-187">Добавление модели</span><span class="sxs-lookup"><span data-stu-id="5994f-187">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5994f-188">Это первый учебник из серии.</span><span class="sxs-lookup"><span data-stu-id="5994f-188">This is the first tutorial of a series.</span></span> <span data-ttu-id="5994f-189">В этой [серии](xref:tutorials/razor-pages/index) приводятся основные сведения о сборке веб-приложения Razor Pages в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5994f-189">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="5994f-190">Пройдя всю серию, вы получите приложение, которое управляет базой данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="5994f-190">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="5994f-191">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="5994f-191">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5994f-192">Создание веб-приложения Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="5994f-192">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="5994f-193">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="5994f-193">Run the app.</span></span>
> * <span data-ttu-id="5994f-194">Анализ файлов проекта.</span><span class="sxs-lookup"><span data-stu-id="5994f-194">Examine the project files.</span></span>

<span data-ttu-id="5994f-195">Пройдя это руководство, вы получите рабочее веб-приложение Razor Pages, сборка которого описана в последующих руководствах.</span><span class="sxs-lookup"><span data-stu-id="5994f-195">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="5994f-197">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="5994f-197">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5994f-198">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5994f-198">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5994f-199">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5994f-199">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5994f-200">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="5994f-200">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="5994f-201">Создание веб-приложения Razor Pages</span><span class="sxs-lookup"><span data-stu-id="5994f-201">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5994f-202">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5994f-202">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5994f-203">В меню **Файл** Visual Studio откройте **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="5994f-203">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="5994f-204">Создайте веб-приложение ASP.NET Core и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="5994f-204">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="5994f-206">Назовите проект **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="5994f-206">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="5994f-207">Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.</span><span class="sxs-lookup"><span data-stu-id="5994f-207">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="5994f-209">Выберите в раскрывающемся списке пункт **ASP.NET Core 2.2**, затем — **Веб-приложение** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="5994f-209">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="5994f-211">Создается следующий начальный проект:</span><span class="sxs-lookup"><span data-stu-id="5994f-211">The following starter project is created:</span></span>

  ![обозреватель решений](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5994f-213">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5994f-213">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="5994f-214">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="5994f-214">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="5994f-215">Перейдите в каталог `cd`, в котором находится проект.</span><span class="sxs-lookup"><span data-stu-id="5994f-215">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="5994f-216">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="5994f-216">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="5994f-217">Команда `dotnet new` создает новый проект Razor Pages в папке *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="5994f-217">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="5994f-218">Команда `code` открывает папку *RazorPagesMovie* в текущем экземпляре Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5994f-218">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="5994f-219">Когда значок строки состояния OmniSharp станет зеленым, появится диалоговое окно с предупреждением **Required assets to build and debug are missing from 'RazorPagesMovie'. (В RazorPagesMovie отсутствуют необходимые ресурсы для сборки и отладки.) Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="5994f-219">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="5994f-220">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="5994f-220">Select **Yes**.</span></span>

  <span data-ttu-id="5994f-221">Каталог *.vscode*, содержащий файлы *launch.json* и *tasks.json*, добавляется в корневой каталог проекта.</span><span class="sxs-lookup"><span data-stu-id="5994f-221">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5994f-222">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="5994f-222">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="5994f-223">В терминале выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="5994f-223">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="5994f-224">Указанные выше команды используют [интерфейс командной строки .NET Core](/dotnet/core/tools/dotnet) для создания проекта Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="5994f-224">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="5994f-225">Открытие проекта</span><span class="sxs-lookup"><span data-stu-id="5994f-225">Open the project</span></span>

<span data-ttu-id="5994f-226">В Visual Studio откройте меню **Файл > Открыть файл** и выберите файл *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="5994f-226">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="5994f-227">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="5994f-227">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5994f-228">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5994f-228">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5994f-229">Нажмите клавиши CTRL+F5, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="5994f-229">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="5994f-230">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение.</span><span class="sxs-lookup"><span data-stu-id="5994f-230">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="5994f-231">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="5994f-231">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="5994f-232">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="5994f-232">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="5994f-233">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="5994f-233">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="5994f-234">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="5994f-234">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="5994f-235">На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="5994f-235">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="5994f-236">Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.</span><span class="sxs-lookup"><span data-stu-id="5994f-236">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="5994f-238">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="5994f-238">The following image shows the app after you give consent to tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5994f-240">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5994f-240">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="5994f-241">Нажмите клавиши **CTRL-F5**, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="5994f-241">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="5994f-242">Visual Studio Code запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="5994f-242">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="5994f-243">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="5994f-243">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="5994f-244">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="5994f-244">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="5994f-245">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="5994f-245">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="5994f-246">На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="5994f-246">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="5994f-247">Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.</span><span class="sxs-lookup"><span data-stu-id="5994f-247">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="5994f-249">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="5994f-249">The following image shows the app after you give consent to tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5994f-251">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="5994f-251">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="5994f-252">Нажмите клавиши **Cmd-Opt-F5**, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="5994f-252">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="5994f-253">Visual Studio запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="5994f-253">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="5994f-254">На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="5994f-254">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="5994f-255">Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.</span><span class="sxs-lookup"><span data-stu-id="5994f-255">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="5994f-257">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="5994f-257">The following image shows the app after you give consent to tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="5994f-259">Анализ файлов проекта</span><span class="sxs-lookup"><span data-stu-id="5994f-259">Examine the project files</span></span>

<span data-ttu-id="5994f-260">Ниже приведен обзор основных папок и файлов проекта, с которыми вы будете работать в последующих учебниках.</span><span class="sxs-lookup"><span data-stu-id="5994f-260">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="5994f-261">Папка Pages</span><span class="sxs-lookup"><span data-stu-id="5994f-261">Pages folder</span></span>

<span data-ttu-id="5994f-262">Содержит страницы Razor и вспомогательные файлы.</span><span class="sxs-lookup"><span data-stu-id="5994f-262">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="5994f-263">Каждая страница Razor — это пара файлов.</span><span class="sxs-lookup"><span data-stu-id="5994f-263">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="5994f-264">Файл *.cshtml*, содержащий HTML-разметку с кодом C# и синтаксисом Razor.</span><span class="sxs-lookup"><span data-stu-id="5994f-264">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="5994f-265">Файл *. cshtml.cs*, содержащий код C#, который обрабатывает события страницы.</span><span class="sxs-lookup"><span data-stu-id="5994f-265">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="5994f-266">Имена вспомогательных файлов начинаются с символа подчеркивания.</span><span class="sxs-lookup"><span data-stu-id="5994f-266">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="5994f-267">Например, файл *_Layout.cshtml* настраивает элементы пользовательского интерфейса, общие для всех страниц.</span><span class="sxs-lookup"><span data-stu-id="5994f-267">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="5994f-268">Этот файл настраивает меню навигации в верхней части страницы и уведомление об авторских правах в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="5994f-268">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="5994f-269">Дополнительные сведения можно найти по адресу: <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="5994f-269">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="5994f-270">Папка wwwroot</span><span class="sxs-lookup"><span data-stu-id="5994f-270">wwwroot folder</span></span>

<span data-ttu-id="5994f-271">Содержит статические файлы, такие как HTML-файлы, файлы JavaScript и CSS-файлы.</span><span class="sxs-lookup"><span data-stu-id="5994f-271">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="5994f-272">Дополнительные сведения можно найти по адресу: <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="5994f-272">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="5994f-273">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="5994f-273">appSettings.json</span></span>

<span data-ttu-id="5994f-274">Содержит данные конфигурации, например строки подключения.</span><span class="sxs-lookup"><span data-stu-id="5994f-274">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="5994f-275">Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="5994f-275">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="5994f-276">Program.cs</span><span class="sxs-lookup"><span data-stu-id="5994f-276">Program.cs</span></span>

<span data-ttu-id="5994f-277">Содержит точку входа для программы.</span><span class="sxs-lookup"><span data-stu-id="5994f-277">Contains the entry point for the program.</span></span> <span data-ttu-id="5994f-278">Дополнительные сведения можно найти по адресу: <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="5994f-278">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="5994f-279">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="5994f-279">Startup.cs</span></span>

<span data-ttu-id="5994f-280">Содержит код, который настраивает поведение приложения, например, требуется ли согласие для файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="5994f-280">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="5994f-281">Дополнительные сведения можно найти по адресу: <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="5994f-281">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5994f-282">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="5994f-282">Additional resources</span></span>

* [<span data-ttu-id="5994f-283">Версия руководства на YouTube</span><span class="sxs-lookup"><span data-stu-id="5994f-283">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="5994f-284">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="5994f-284">Next steps</span></span>

<span data-ttu-id="5994f-285">Перейдите к следующему учебнику в серии:</span><span class="sxs-lookup"><span data-stu-id="5994f-285">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="5994f-286">Добавление модели</span><span class="sxs-lookup"><span data-stu-id="5994f-286">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end
