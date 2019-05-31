---
title: Учебник. Начало работы с Razor Pages в ASP.NET Core
author: rick-anderson
description: В этой серии руководств объясняется, как использовать Razor Pages в ASP.NET Core. Узнайте, как создать модель, сгенерировать код для Razor Pages, использовать Entity Framework Core и SQL Server для доступа к данным, добавлять функции поиска и проверки ввода, а также использовать возможность миграции для обновления модели.
ms.author: riande
ms.date: 05/30/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: e9f11f68aa138ab74a0ffbbd0e32067bc984606d
ms.sourcegitcommit: 9ae1fd11f39b0a72b2ae42f0b450345e6e306bc0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/30/2019
ms.locfileid: "66415662"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="ba6b1-104">Учебник. Начало работы с Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ba6b1-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="ba6b1-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="ba6b1-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ba6b1-106">Это первый учебник из серии.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-106">This is the first tutorial of a series.</span></span> <span data-ttu-id="ba6b1-107">В этой [серии](xref:tutorials/razor-pages/index) приводятся основные сведения о сборке веб-приложения Razor Pages в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-107">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="ba6b1-108">Пройдя всю серию, вы получите приложение, которое управляет базой данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-108">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="ba6b1-109">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ba6b1-110">Создание веб-приложения Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-110">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="ba6b1-111">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-111">Run the app.</span></span>
> * <span data-ttu-id="ba6b1-112">Анализ файлов проекта.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-112">Examine the project files.</span></span>

<span data-ttu-id="ba6b1-113">Пройдя это руководство, вы получите рабочее веб-приложение Razor Pages, сборка которого описана в последующих руководствах.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-113">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="ba6b1-115">Создание веб-приложения Razor Pages</span><span class="sxs-lookup"><span data-stu-id="ba6b1-115">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ba6b1-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ba6b1-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ba6b1-117">В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-117">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="ba6b1-118">Создайте новое веб-приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-118">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="ba6b1-119">Назовите проект **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-119">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="ba6b1-120">Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-120">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="ba6b1-122">Выберите в раскрывающемся списке **ASP.NET Core 2.2**, а затем **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-122">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>

  ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="ba6b1-124">Создается следующий начальный проект:</span><span class="sxs-lookup"><span data-stu-id="ba6b1-124">The following starter project is created:</span></span>

  ![обозреватель решений](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ba6b1-126">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="ba6b1-127">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="ba6b1-127">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="ba6b1-128">Перейдите в каталог `cd`, в котором находится проект.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-128">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="ba6b1-129">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="ba6b1-129">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="ba6b1-130">Команда `dotnet new` создает новый проект Razor Pages в папке *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-130">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="ba6b1-131">Команда `code` открывает папку *RazorPagesMovie* в текущем экземпляре Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-131">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="ba6b1-132">Когда значок строки состояния OmniSharp станет зеленым, появится диалоговое окно с предупреждением **Required assets to build and debug are missing from 'RazorPagesMovie'. (В RazorPagesMovie отсутствуют необходимые ресурсы для сборки и отладки.) Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="ba6b1-132">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="ba6b1-133">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-133">Select **Yes**.</span></span>

  <span data-ttu-id="ba6b1-134">Каталог *.vscode*, содержащий файлы *launch.json* и *tasks.json*, добавляется в корневой каталог проекта.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-134">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ba6b1-135">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="ba6b1-135">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="ba6b1-136">В терминале выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="ba6b1-136">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="ba6b1-137">Указанные выше команды используют [интерфейс командной строки .NET Core](/dotnet/core/tools/dotnet) для создания проекта Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-137">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="ba6b1-138">Открытие проекта</span><span class="sxs-lookup"><span data-stu-id="ba6b1-138">Open the project</span></span>

<span data-ttu-id="ba6b1-139">В Visual Studio откройте меню **Файл > Открыть файл** и выберите файл *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-139">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="ba6b1-140">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="ba6b1-140">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ba6b1-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ba6b1-141">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ba6b1-142">Нажмите клавиши CTRL+F5, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-142">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="ba6b1-143">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-143">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="ba6b1-144">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-144">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ba6b1-145">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-145">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="ba6b1-146">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-146">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="ba6b1-147">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-147">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="ba6b1-148">На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-148">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="ba6b1-149">Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-149">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="ba6b1-151">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="ba6b1-151">The following image shows the app after you give consent to tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ba6b1-153">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-153">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="ba6b1-154">Нажмите клавиши **CTRL-F5**, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-154">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="ba6b1-155">Visual Studio Code запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-155">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="ba6b1-156">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-156">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ba6b1-157">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-157">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="ba6b1-158">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-158">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="ba6b1-159">На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-159">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="ba6b1-160">Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-160">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="ba6b1-162">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="ba6b1-162">The following image shows the app after you give consent to tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ba6b1-164">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="ba6b1-164">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="ba6b1-165">Нажмите клавиши **Cmd-Opt-F5**, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-165">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="ba6b1-166">Visual Studio запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-166">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="ba6b1-167">На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-167">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="ba6b1-168">Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-168">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="ba6b1-170">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="ba6b1-170">The following image shows the app after you give consent to tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="ba6b1-172">Анализ файлов проекта</span><span class="sxs-lookup"><span data-stu-id="ba6b1-172">Examine the project files</span></span>

<span data-ttu-id="ba6b1-173">Ниже приведен обзор основных папок и файлов проекта, с которыми вы будете работать в последующих учебниках.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-173">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="ba6b1-174">Папка Pages</span><span class="sxs-lookup"><span data-stu-id="ba6b1-174">Pages folder</span></span>

<span data-ttu-id="ba6b1-175">Содержит страницы Razor и вспомогательные файлы.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-175">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="ba6b1-176">Каждая страница Razor — это пара файлов.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-176">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="ba6b1-177">Файл *.cshtml*, содержащий HTML-разметку с кодом C# и синтаксисом Razor.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-177">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="ba6b1-178">Файл *. cshtml.cs*, содержащий код C#, который обрабатывает события страницы.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-178">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="ba6b1-179">Имена вспомогательных файлов начинаются с символа подчеркивания.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-179">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="ba6b1-180">Например, файл *_Layout.cshtml* настраивает элементы пользовательского интерфейса, общие для всех страниц.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-180">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="ba6b1-181">Этот файл настраивает меню навигации в верхней части страницы и уведомление об авторских правах в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-181">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="ba6b1-182">Для получения дополнительной информации см. <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-182">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="ba6b1-183">Папка wwwroot</span><span class="sxs-lookup"><span data-stu-id="ba6b1-183">wwwroot folder</span></span>

<span data-ttu-id="ba6b1-184">Содержит статические файлы, такие как HTML-файлы, файлы JavaScript и CSS-файлы.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-184">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="ba6b1-185">Для получения дополнительной информации см. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-185">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="ba6b1-186">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="ba6b1-186">appSettings.json</span></span>

<span data-ttu-id="ba6b1-187">Содержит данные конфигурации, например строки подключения.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-187">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="ba6b1-188">Для получения дополнительной информации см. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-188">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="ba6b1-189">Program.cs</span><span class="sxs-lookup"><span data-stu-id="ba6b1-189">Program.cs</span></span>

<span data-ttu-id="ba6b1-190">Содержит точку входа для программы.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-190">Contains the entry point for the program.</span></span> <span data-ttu-id="ba6b1-191">Для получения дополнительной информации см. <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-191">For more information, see <xref:fundamentals/host/web-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="ba6b1-192">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="ba6b1-192">Startup.cs</span></span>

<span data-ttu-id="ba6b1-193">Содержит код, который настраивает поведение приложения, например, требуется ли согласие для файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-193">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="ba6b1-194">Для получения дополнительной информации см. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-194">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ba6b1-195">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="ba6b1-195">Additional resources</span></span>

* [<span data-ttu-id="ba6b1-196">Версия руководства на YouTube</span><span class="sxs-lookup"><span data-stu-id="ba6b1-196">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="ba6b1-197">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="ba6b1-197">Next steps</span></span>

<span data-ttu-id="ba6b1-198">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-198">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ba6b1-199">Создание веб-приложения Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-199">Created a Razor Pages web app.</span></span>
> * <span data-ttu-id="ba6b1-200">Запуск приложения.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-200">Ran the app.</span></span>
> * <span data-ttu-id="ba6b1-201">Анализ файлов проекта.</span><span class="sxs-lookup"><span data-stu-id="ba6b1-201">Examined the project files.</span></span>

<span data-ttu-id="ba6b1-202">Перейдите к следующему учебнику в серии:</span><span class="sxs-lookup"><span data-stu-id="ba6b1-202">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ba6b1-203">Добавление модели</span><span class="sxs-lookup"><span data-stu-id="ba6b1-203">Add a model</span></span>](xref:tutorials/razor-pages/model)
