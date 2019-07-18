---
title: Учебник. Начало работы с Razor Pages в ASP.NET Core
author: rick-anderson
description: В этой серии руководств объясняется, как использовать Razor Pages в ASP.NET Core. Узнайте, как создать модель, сгенерировать код для Razor Pages, использовать Entity Framework Core и SQL Server для доступа к данным, добавлять функции поиска и проверки ввода, а также использовать возможность миграции для обновления модели.
ms.author: riande
ms.date: 06/03/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 7e228c99b4d55c14cea9c915cf06a7fbbbd5af44
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/12/2019
ms.locfileid: "67855739"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="81237-104">Учебник. Начало работы с Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="81237-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="81237-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="81237-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="81237-106">Это первый учебник из серии.</span><span class="sxs-lookup"><span data-stu-id="81237-106">This is the first tutorial of a series.</span></span> <span data-ttu-id="81237-107">В этой [серии](xref:tutorials/razor-pages/index) приводятся основные сведения о сборке веб-приложения Razor Pages в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="81237-107">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="81237-108">Пройдя всю серию, вы получите приложение, которое управляет базой данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="81237-108">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="81237-109">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="81237-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="81237-110">Создание веб-приложения Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="81237-110">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="81237-111">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="81237-111">Run the app.</span></span>
> * <span data-ttu-id="81237-112">Анализ файлов проекта.</span><span class="sxs-lookup"><span data-stu-id="81237-112">Examine the project files.</span></span>

<span data-ttu-id="81237-113">Пройдя это руководство, вы получите рабочее веб-приложение Razor Pages, сборка которого описана в последующих руководствах.</span><span class="sxs-lookup"><span data-stu-id="81237-113">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="81237-115">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="81237-115">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="81237-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="81237-116">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="81237-117">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="81237-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="81237-118">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="81237-118">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="81237-119">Создание веб-приложения Razor Pages</span><span class="sxs-lookup"><span data-stu-id="81237-119">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="81237-120">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="81237-120">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="81237-121">В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="81237-121">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="81237-122">Создайте веб-приложение ASP.NET Core и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="81237-122">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="81237-124">Назовите проект **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="81237-124">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="81237-125">Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.</span><span class="sxs-lookup"><span data-stu-id="81237-125">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="81237-127">Выберите в раскрывающемся списке пункт **ASP.NET Core 2.2**, затем — **Веб-приложение** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="81237-127">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="81237-129">Создается следующий начальный проект:</span><span class="sxs-lookup"><span data-stu-id="81237-129">The following starter project is created:</span></span>

  ![обозреватель решений](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="81237-131">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="81237-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="81237-132">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="81237-132">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="81237-133">Перейдите в каталог `cd`, в котором находится проект.</span><span class="sxs-lookup"><span data-stu-id="81237-133">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="81237-134">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="81237-134">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="81237-135">Команда `dotnet new` создает новый проект Razor Pages в папке *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="81237-135">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="81237-136">Команда `code` открывает папку *RazorPagesMovie* в текущем экземпляре Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="81237-136">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="81237-137">Когда значок строки состояния OmniSharp станет зеленым, появится диалоговое окно с предупреждением **Required assets to build and debug are missing from 'RazorPagesMovie'. (В RazorPagesMovie отсутствуют необходимые ресурсы для сборки и отладки.) Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="81237-137">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="81237-138">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="81237-138">Select **Yes**.</span></span>

  <span data-ttu-id="81237-139">Каталог *.vscode*, содержащий файлы *launch.json* и *tasks.json*, добавляется в корневой каталог проекта.</span><span class="sxs-lookup"><span data-stu-id="81237-139">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="81237-140">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="81237-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="81237-141">В терминале выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="81237-141">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="81237-142">Указанные выше команды используют [интерфейс командной строки .NET Core](/dotnet/core/tools/dotnet) для создания проекта Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="81237-142">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="81237-143">Открытие проекта</span><span class="sxs-lookup"><span data-stu-id="81237-143">Open the project</span></span>

<span data-ttu-id="81237-144">В Visual Studio откройте меню **Файл > Открыть файл** и выберите файл *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="81237-144">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="81237-145">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="81237-145">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="81237-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="81237-146">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="81237-147">Нажмите клавиши CTRL+F5, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="81237-147">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="81237-148">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение.</span><span class="sxs-lookup"><span data-stu-id="81237-148">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="81237-149">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="81237-149">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="81237-150">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="81237-150">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="81237-151">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="81237-151">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="81237-152">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="81237-152">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="81237-153">На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="81237-153">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="81237-154">Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.</span><span class="sxs-lookup"><span data-stu-id="81237-154">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="81237-156">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="81237-156">The following image shows the app after you give consent to tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="81237-158">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="81237-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="81237-159">Нажмите клавиши **CTRL-F5**, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="81237-159">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="81237-160">Visual Studio Code запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="81237-160">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="81237-161">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="81237-161">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="81237-162">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="81237-162">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="81237-163">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="81237-163">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="81237-164">На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="81237-164">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="81237-165">Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.</span><span class="sxs-lookup"><span data-stu-id="81237-165">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="81237-167">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="81237-167">The following image shows the app after you give consent to tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="81237-169">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="81237-169">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="81237-170">Нажмите клавиши **Cmd-Opt-F5**, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="81237-170">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="81237-171">Visual Studio запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="81237-171">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="81237-172">На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="81237-172">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="81237-173">Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.</span><span class="sxs-lookup"><span data-stu-id="81237-173">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="81237-175">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="81237-175">The following image shows the app after you give consent to tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="81237-177">Анализ файлов проекта</span><span class="sxs-lookup"><span data-stu-id="81237-177">Examine the project files</span></span>

<span data-ttu-id="81237-178">Ниже приведен обзор основных папок и файлов проекта, с которыми вы будете работать в последующих учебниках.</span><span class="sxs-lookup"><span data-stu-id="81237-178">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="81237-179">Папка Pages</span><span class="sxs-lookup"><span data-stu-id="81237-179">Pages folder</span></span>

<span data-ttu-id="81237-180">Содержит страницы Razor и вспомогательные файлы.</span><span class="sxs-lookup"><span data-stu-id="81237-180">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="81237-181">Каждая страница Razor — это пара файлов.</span><span class="sxs-lookup"><span data-stu-id="81237-181">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="81237-182">Файл *.cshtml*, содержащий HTML-разметку с кодом C# и синтаксисом Razor.</span><span class="sxs-lookup"><span data-stu-id="81237-182">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="81237-183">Файл *. cshtml.cs*, содержащий код C#, который обрабатывает события страницы.</span><span class="sxs-lookup"><span data-stu-id="81237-183">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="81237-184">Имена вспомогательных файлов начинаются с символа подчеркивания.</span><span class="sxs-lookup"><span data-stu-id="81237-184">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="81237-185">Например, файл *_Layout.cshtml* настраивает элементы пользовательского интерфейса, общие для всех страниц.</span><span class="sxs-lookup"><span data-stu-id="81237-185">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="81237-186">Этот файл настраивает меню навигации в верхней части страницы и уведомление об авторских правах в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="81237-186">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="81237-187">Дополнительные сведения можно найти по адресу: <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="81237-187">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="81237-188">Папка wwwroot</span><span class="sxs-lookup"><span data-stu-id="81237-188">wwwroot folder</span></span>

<span data-ttu-id="81237-189">Содержит статические файлы, такие как HTML-файлы, файлы JavaScript и CSS-файлы.</span><span class="sxs-lookup"><span data-stu-id="81237-189">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="81237-190">Дополнительные сведения можно найти по адресу: <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="81237-190">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="81237-191">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="81237-191">appSettings.json</span></span>

<span data-ttu-id="81237-192">Содержит данные конфигурации, например строки подключения.</span><span class="sxs-lookup"><span data-stu-id="81237-192">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="81237-193">Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="81237-193">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="81237-194">Program.cs</span><span class="sxs-lookup"><span data-stu-id="81237-194">Program.cs</span></span>

<span data-ttu-id="81237-195">Содержит точку входа для программы.</span><span class="sxs-lookup"><span data-stu-id="81237-195">Contains the entry point for the program.</span></span> <span data-ttu-id="81237-196">Дополнительные сведения можно найти по адресу: <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="81237-196">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="81237-197">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="81237-197">Startup.cs</span></span>

<span data-ttu-id="81237-198">Содержит код, который настраивает поведение приложения, например, требуется ли согласие для файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="81237-198">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="81237-199">Дополнительные сведения можно найти по адресу: <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="81237-199">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="81237-200">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="81237-200">Additional resources</span></span>

* [<span data-ttu-id="81237-201">Версия руководства на YouTube</span><span class="sxs-lookup"><span data-stu-id="81237-201">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="81237-202">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="81237-202">Next steps</span></span>

<span data-ttu-id="81237-203">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="81237-203">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="81237-204">Создание веб-приложения Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="81237-204">Created a Razor Pages web app.</span></span>
> * <span data-ttu-id="81237-205">Запуск приложения.</span><span class="sxs-lookup"><span data-stu-id="81237-205">Ran the app.</span></span>
> * <span data-ttu-id="81237-206">Анализ файлов проекта.</span><span class="sxs-lookup"><span data-stu-id="81237-206">Examined the project files.</span></span>

<span data-ttu-id="81237-207">Перейдите к следующему учебнику в серии:</span><span class="sxs-lookup"><span data-stu-id="81237-207">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="81237-208">Добавление модели</span><span class="sxs-lookup"><span data-stu-id="81237-208">Add a model</span></span>](xref:tutorials/razor-pages/model)
