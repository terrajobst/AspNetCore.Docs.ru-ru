---
title: Учебник. Начало работы с Razor Pages в ASP.NET Core
author: rick-anderson
description: В этой серии руководств объясняется, как использовать Razor Pages в ASP.NET Core. Узнайте, как создать модель, сгенерировать код для Razor Pages, использовать Entity Framework Core и SQL Server для доступа к данным, добавлять функции поиска и проверки ввода, а также использовать возможность миграции для обновления модели.
ms.author: riande
ms.date: 6/3/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: d843e47ccb5180fab34b4c4c4a4b5cbda21289bf
ms.sourcegitcommit: a1364109d11d414121a6337b611bee61d6e489e9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/04/2019
ms.locfileid: "66491216"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="a6f08-104">Учебник. Начало работы с Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a6f08-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="a6f08-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="a6f08-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a6f08-106">Это первый учебник из серии.</span><span class="sxs-lookup"><span data-stu-id="a6f08-106">This is the first tutorial of a series.</span></span> <span data-ttu-id="a6f08-107">В этой [серии](xref:tutorials/razor-pages/index) приводятся основные сведения о сборке веб-приложения Razor Pages в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a6f08-107">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="a6f08-108">Пройдя всю серию, вы получите приложение, которое управляет базой данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="a6f08-108">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="a6f08-109">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="a6f08-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a6f08-110">Создание веб-приложения Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="a6f08-110">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="a6f08-111">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="a6f08-111">Run the app.</span></span>
> * <span data-ttu-id="a6f08-112">Анализ файлов проекта.</span><span class="sxs-lookup"><span data-stu-id="a6f08-112">Examine the project files.</span></span>

<span data-ttu-id="a6f08-113">Пройдя это руководство, вы получите рабочее веб-приложение Razor Pages, сборка которого описана в последующих руководствах.</span><span class="sxs-lookup"><span data-stu-id="a6f08-113">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="a6f08-115">Создание веб-приложения Razor Pages</span><span class="sxs-lookup"><span data-stu-id="a6f08-115">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a6f08-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a6f08-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a6f08-117">В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="a6f08-117">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="a6f08-118">Создайте веб-приложение ASP.NET Core и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="a6f08-118">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="a6f08-120">Назовите проект **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="a6f08-120">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="a6f08-121">Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.</span><span class="sxs-lookup"><span data-stu-id="a6f08-121">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code code.</span></span>

  ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="a6f08-123">Выберите в раскрывающемся списке пункт **ASP.NET Core 2.2**, затем — **Веб-приложение** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="a6f08-123">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="a6f08-125">Создается следующий начальный проект:</span><span class="sxs-lookup"><span data-stu-id="a6f08-125">The following starter project is created:</span></span>

  ![обозреватель решений](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a6f08-127">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a6f08-127">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a6f08-128">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="a6f08-128">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="a6f08-129">Перейдите в каталог `cd`, в котором находится проект.</span><span class="sxs-lookup"><span data-stu-id="a6f08-129">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="a6f08-130">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="a6f08-130">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="a6f08-131">Команда `dotnet new` создает новый проект Razor Pages в папке *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="a6f08-131">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="a6f08-132">Команда `code` открывает папку *RazorPagesMovie* в текущем экземпляре Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a6f08-132">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="a6f08-133">Когда значок строки состояния OmniSharp станет зеленым, появится диалоговое окно с предупреждением **Required assets to build and debug are missing from 'RazorPagesMovie'. (В RazorPagesMovie отсутствуют необходимые ресурсы для сборки и отладки.) Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="a6f08-133">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="a6f08-134">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="a6f08-134">Select **Yes**.</span></span>

  <span data-ttu-id="a6f08-135">Каталог *.vscode*, содержащий файлы *launch.json* и *tasks.json*, добавляется в корневой каталог проекта.</span><span class="sxs-lookup"><span data-stu-id="a6f08-135">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a6f08-136">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="a6f08-136">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="a6f08-137">В терминале выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="a6f08-137">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="a6f08-138">Указанные выше команды используют [интерфейс командной строки .NET Core](/dotnet/core/tools/dotnet) для создания проекта Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="a6f08-138">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="a6f08-139">Открытие проекта</span><span class="sxs-lookup"><span data-stu-id="a6f08-139">Open the project</span></span>

<span data-ttu-id="a6f08-140">В Visual Studio откройте меню **Файл > Открыть файл** и выберите файл *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="a6f08-140">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="a6f08-141">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="a6f08-141">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a6f08-142">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a6f08-142">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a6f08-143">Нажмите клавиши CTRL+F5, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="a6f08-143">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="a6f08-144">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение.</span><span class="sxs-lookup"><span data-stu-id="a6f08-144">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="a6f08-145">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="a6f08-145">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="a6f08-146">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="a6f08-146">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="a6f08-147">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="a6f08-147">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="a6f08-148">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="a6f08-148">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="a6f08-149">На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="a6f08-149">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="a6f08-150">Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.</span><span class="sxs-lookup"><span data-stu-id="a6f08-150">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="a6f08-152">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="a6f08-152">The following image shows the app after you give consent to tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a6f08-154">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a6f08-154">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="a6f08-155">Нажмите клавиши **CTRL-F5**, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="a6f08-155">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="a6f08-156">Visual Studio Code запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="a6f08-156">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="a6f08-157">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="a6f08-157">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="a6f08-158">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="a6f08-158">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="a6f08-159">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="a6f08-159">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="a6f08-160">На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="a6f08-160">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="a6f08-161">Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.</span><span class="sxs-lookup"><span data-stu-id="a6f08-161">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="a6f08-163">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="a6f08-163">The following image shows the app after you give consent to tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a6f08-165">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="a6f08-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="a6f08-166">Нажмите клавиши **Cmd-Opt-F5**, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="a6f08-166">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="a6f08-167">Visual Studio запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="a6f08-167">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="a6f08-168">На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="a6f08-168">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="a6f08-169">Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.</span><span class="sxs-lookup"><span data-stu-id="a6f08-169">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="a6f08-171">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="a6f08-171">The following image shows the app after you give consent to tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="a6f08-173">Анализ файлов проекта</span><span class="sxs-lookup"><span data-stu-id="a6f08-173">Examine the project files</span></span>

<span data-ttu-id="a6f08-174">Ниже приведен обзор основных папок и файлов проекта, с которыми вы будете работать в последующих учебниках.</span><span class="sxs-lookup"><span data-stu-id="a6f08-174">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="a6f08-175">Папка Pages</span><span class="sxs-lookup"><span data-stu-id="a6f08-175">Pages folder</span></span>

<span data-ttu-id="a6f08-176">Содержит страницы Razor и вспомогательные файлы.</span><span class="sxs-lookup"><span data-stu-id="a6f08-176">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="a6f08-177">Каждая страница Razor — это пара файлов.</span><span class="sxs-lookup"><span data-stu-id="a6f08-177">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="a6f08-178">Файл *.cshtml*, содержащий HTML-разметку с кодом C# и синтаксисом Razor.</span><span class="sxs-lookup"><span data-stu-id="a6f08-178">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="a6f08-179">Файл *. cshtml.cs*, содержащий код C#, который обрабатывает события страницы.</span><span class="sxs-lookup"><span data-stu-id="a6f08-179">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="a6f08-180">Имена вспомогательных файлов начинаются с символа подчеркивания.</span><span class="sxs-lookup"><span data-stu-id="a6f08-180">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="a6f08-181">Например, файл *_Layout.cshtml* настраивает элементы пользовательского интерфейса, общие для всех страниц.</span><span class="sxs-lookup"><span data-stu-id="a6f08-181">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="a6f08-182">Этот файл настраивает меню навигации в верхней части страницы и уведомление об авторских правах в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="a6f08-182">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="a6f08-183">Для получения дополнительной информации см. <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="a6f08-183">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="a6f08-184">Папка wwwroot</span><span class="sxs-lookup"><span data-stu-id="a6f08-184">wwwroot folder</span></span>

<span data-ttu-id="a6f08-185">Содержит статические файлы, такие как HTML-файлы, файлы JavaScript и CSS-файлы.</span><span class="sxs-lookup"><span data-stu-id="a6f08-185">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="a6f08-186">Для получения дополнительной информации см. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="a6f08-186">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="a6f08-187">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="a6f08-187">appSettings.json</span></span>

<span data-ttu-id="a6f08-188">Содержит данные конфигурации, например строки подключения.</span><span class="sxs-lookup"><span data-stu-id="a6f08-188">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="a6f08-189">Для получения дополнительной информации см. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="a6f08-189">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="a6f08-190">Program.cs</span><span class="sxs-lookup"><span data-stu-id="a6f08-190">Program.cs</span></span>

<span data-ttu-id="a6f08-191">Содержит точку входа для программы.</span><span class="sxs-lookup"><span data-stu-id="a6f08-191">Contains the entry point for the program.</span></span> <span data-ttu-id="a6f08-192">Для получения дополнительной информации см. <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="a6f08-192">For more information, see <xref:fundamentals/host/web-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="a6f08-193">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="a6f08-193">Startup.cs</span></span>

<span data-ttu-id="a6f08-194">Содержит код, который настраивает поведение приложения, например, требуется ли согласие для файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="a6f08-194">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="a6f08-195">Для получения дополнительной информации см. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="a6f08-195">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a6f08-196">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a6f08-196">Additional resources</span></span>

* [<span data-ttu-id="a6f08-197">Версия руководства на YouTube</span><span class="sxs-lookup"><span data-stu-id="a6f08-197">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="a6f08-198">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="a6f08-198">Next steps</span></span>

<span data-ttu-id="a6f08-199">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="a6f08-199">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a6f08-200">Создание веб-приложения Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="a6f08-200">Created a Razor Pages web app.</span></span>
> * <span data-ttu-id="a6f08-201">Запуск приложения.</span><span class="sxs-lookup"><span data-stu-id="a6f08-201">Ran the app.</span></span>
> * <span data-ttu-id="a6f08-202">Анализ файлов проекта.</span><span class="sxs-lookup"><span data-stu-id="a6f08-202">Examined the project files.</span></span>

<span data-ttu-id="a6f08-203">Перейдите к следующему учебнику в серии:</span><span class="sxs-lookup"><span data-stu-id="a6f08-203">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a6f08-204">Добавление модели</span><span class="sxs-lookup"><span data-stu-id="a6f08-204">Add a model</span></span>](xref:tutorials/razor-pages/model)
