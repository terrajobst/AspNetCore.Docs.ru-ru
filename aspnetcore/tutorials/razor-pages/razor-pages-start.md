---
title: Учебник. Начало работы с Razor Pages в ASP.NET Core
author: rick-anderson
description: В этой серии руководств объясняется, как использовать Razor Pages в ASP.NET Core. Узнайте, как создать модель, сгенерировать код для Razor Pages, использовать Entity Framework Core и SQL Server для доступа к данным, добавлять функции поиска и проверки ввода, а также использовать возможность миграции для обновления модели.
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1d264ca4a605d8291e273a8f054c92e7eefa5548
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64891159"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="53498-104">Учебник. Начало работы с Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="53498-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="53498-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="53498-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="53498-106">Это первый учебник из серии.</span><span class="sxs-lookup"><span data-stu-id="53498-106">This is the first tutorial of a series.</span></span> <span data-ttu-id="53498-107">В этой [серии](xref:tutorials/razor-pages/index) приводятся основные сведения о сборке веб-приложения Razor Pages в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="53498-107">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="53498-108">В конце серии вы получите приложение, которое управляет базой данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="53498-108">At the end of the series you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="53498-109">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="53498-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="53498-110">Создание веб-приложения Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="53498-110">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="53498-111">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="53498-111">Run the app.</span></span>
> * <span data-ttu-id="53498-112">Анализ файлов проекта.</span><span class="sxs-lookup"><span data-stu-id="53498-112">Examine the project files.</span></span>

<span data-ttu-id="53498-113">В конце этого учебника вы получите рабочее веб-приложения Razor Pages, сборку которого вы будете выполнять в последующих учебниках.</span><span class="sxs-lookup"><span data-stu-id="53498-113">At the end of this tutorial you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="53498-115">Создание веб-приложения Razor Pages</span><span class="sxs-lookup"><span data-stu-id="53498-115">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="53498-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="53498-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="53498-117">В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="53498-117">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="53498-118">Создайте новое веб-приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="53498-118">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="53498-119">Назовите проект **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="53498-119">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="53498-120">Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.</span><span class="sxs-lookup"><span data-stu-id="53498-120">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="53498-122">Выберите в раскрывающемся списке **ASP.NET Core 2.2**, а затем **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="53498-122">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>

  ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="53498-124">Создается следующий начальный проект:</span><span class="sxs-lookup"><span data-stu-id="53498-124">The following starter project is created:</span></span>

  ![обозреватель решений](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="53498-126">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="53498-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="53498-127">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="53498-127">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="53498-128">Смените каталог (`cd`) на папку, в которой будет содержаться проект.</span><span class="sxs-lookup"><span data-stu-id="53498-128">Change directories (`cd`) to a folder which will contain the project.</span></span>

* <span data-ttu-id="53498-129">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="53498-129">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="53498-130">Команда `dotnet new` создает новый проект Razor Pages в папке *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="53498-130">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="53498-131">Команда `code` открывает папку *RazorPagesMovie* в новом экземпляре Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="53498-131">The `code` command opens the *RazorPagesMovie* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="53498-132">Появится диалоговое окно с предупреждением **В RazorPagesMovie отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="53498-132">A dialog box appears with **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span>

* <span data-ttu-id="53498-133">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="53498-133">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="53498-134">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="53498-134">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="53498-135">В терминале выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="53498-135">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="53498-136">Указанные выше команды используют [интерфейс командной строки .NET Core](/dotnet/core/tools/dotnet) для создания проекта Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="53498-136">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="53498-137">Открытие проекта</span><span class="sxs-lookup"><span data-stu-id="53498-137">Open the project</span></span>

<span data-ttu-id="53498-138">В Visual Studio откройте меню **Файл > Открыть файл** и выберите файл *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="53498-138">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="53498-139">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="53498-139">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="53498-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="53498-140">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="53498-141">Нажмите клавиши CTRL+F5, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="53498-141">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="53498-142">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение.</span><span class="sxs-lookup"><span data-stu-id="53498-142">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="53498-143">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="53498-143">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="53498-144">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="53498-144">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="53498-145">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="53498-145">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="53498-146">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="53498-146">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="53498-147">На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="53498-147">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="53498-148">Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.</span><span class="sxs-lookup"><span data-stu-id="53498-148">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="53498-150">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="53498-150">The following image shows the app after you give consent to tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="53498-152">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="53498-152">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="53498-153">Нажмите клавиши **CTRL-F5**, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="53498-153">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="53498-154">Visual Studio Code запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="53498-154">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="53498-155">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="53498-155">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="53498-156">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="53498-156">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="53498-157">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="53498-157">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="53498-158">На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="53498-158">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="53498-159">Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.</span><span class="sxs-lookup"><span data-stu-id="53498-159">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="53498-161">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="53498-161">The following image shows the app after you give consent to tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="53498-163">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="53498-163">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="53498-164">Нажмите клавиши **Cmd-Opt-F5**, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="53498-164">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="53498-165">Visual Studio запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="53498-165">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="53498-166">На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="53498-166">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="53498-167">Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.</span><span class="sxs-lookup"><span data-stu-id="53498-167">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="53498-169">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="53498-169">The following image shows the app after you give consent to tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="53498-171">Анализ файлов проекта</span><span class="sxs-lookup"><span data-stu-id="53498-171">Examine the project files</span></span>

<span data-ttu-id="53498-172">Ниже приведен обзор основных папок и файлов проекта, с которыми вы будете работать в последующих учебниках.</span><span class="sxs-lookup"><span data-stu-id="53498-172">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="53498-173">Папка Pages</span><span class="sxs-lookup"><span data-stu-id="53498-173">Pages folder</span></span>

<span data-ttu-id="53498-174">Содержит страницы Razor и вспомогательные файлы.</span><span class="sxs-lookup"><span data-stu-id="53498-174">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="53498-175">Каждая страница Razor — это пара файлов.</span><span class="sxs-lookup"><span data-stu-id="53498-175">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="53498-176">Файл *.cshtml*, содержащий HTML-разметку с кодом C# и синтаксисом Razor.</span><span class="sxs-lookup"><span data-stu-id="53498-176">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="53498-177">Файл *. cshtml.cs*, содержащий код C#, который обрабатывает события страницы.</span><span class="sxs-lookup"><span data-stu-id="53498-177">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="53498-178">Имена вспомогательных файлов начинаются с символа подчеркивания.</span><span class="sxs-lookup"><span data-stu-id="53498-178">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="53498-179">Например, файл *_Layout.cshtml* настраивает элементы пользовательского интерфейса, общие для всех страниц.</span><span class="sxs-lookup"><span data-stu-id="53498-179">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="53498-180">Этот файл настраивает меню навигации в верхней части страницы и уведомление об авторских правах в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="53498-180">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="53498-181">Для получения дополнительной информации см. <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="53498-181">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="53498-182">Папка wwwroot</span><span class="sxs-lookup"><span data-stu-id="53498-182">wwwroot folder</span></span>

<span data-ttu-id="53498-183">Содержит статические файлы, такие как HTML-файлы, файлы JavaScript и CSS-файлы.</span><span class="sxs-lookup"><span data-stu-id="53498-183">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="53498-184">Для получения дополнительной информации см. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="53498-184">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="53498-185">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="53498-185">appSettings.json</span></span>

<span data-ttu-id="53498-186">Содержит данные конфигурации, например строки подключения.</span><span class="sxs-lookup"><span data-stu-id="53498-186">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="53498-187">Для получения дополнительной информации см. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="53498-187">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="53498-188">Program.cs</span><span class="sxs-lookup"><span data-stu-id="53498-188">Program.cs</span></span>

<span data-ttu-id="53498-189">Содержит точку входа для программы.</span><span class="sxs-lookup"><span data-stu-id="53498-189">Contains the entry point for the program.</span></span> <span data-ttu-id="53498-190">Для получения дополнительной информации см. <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="53498-190">For more information, see <xref:fundamentals/host/web-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="53498-191">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="53498-191">Startup.cs</span></span>

<span data-ttu-id="53498-192">Содержит код, который настраивает поведение приложения, например, требуется ли согласие для файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="53498-192">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="53498-193">Для получения дополнительной информации см. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="53498-193">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="53498-194">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="53498-194">Additional resources</span></span>

* [<span data-ttu-id="53498-195">Версия руководства на YouTube</span><span class="sxs-lookup"><span data-stu-id="53498-195">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="53498-196">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="53498-196">Next steps</span></span>

<span data-ttu-id="53498-197">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="53498-197">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="53498-198">Создание веб-приложения Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="53498-198">Created a Razor Pages web app.</span></span>
> * <span data-ttu-id="53498-199">Запуск приложения.</span><span class="sxs-lookup"><span data-stu-id="53498-199">Ran the app.</span></span>
> * <span data-ttu-id="53498-200">Анализ файлов проекта.</span><span class="sxs-lookup"><span data-stu-id="53498-200">Examined the project files.</span></span>

<span data-ttu-id="53498-201">Перейдите к следующему учебнику в серии:</span><span class="sxs-lookup"><span data-stu-id="53498-201">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="53498-202">Добавление модели</span><span class="sxs-lookup"><span data-stu-id="53498-202">Add a model</span></span>](xref:tutorials/razor-pages/model)
