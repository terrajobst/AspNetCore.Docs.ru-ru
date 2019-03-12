---
title: Учебник. Начало работы с Razor Pages в ASP.NET Core
author: rick-anderson
description: В этой серии руководств объясняется, как использовать Razor Pages в ASP.NET Core. Узнайте, как создать модель, сгенерировать код для Razor Pages, использовать Entity Framework Core и SQL Server для доступа к данным, добавлять функции поиска и проверки ввода, а также использовать возможность миграции для обновления модели.
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 29d9369cfa6a4c76f015b5a819a27dfa280d4075
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346415"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="8e0cb-104">Учебник. Начало работы с Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8e0cb-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="8e0cb-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="8e0cb-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8e0cb-106">Это первый учебник из серии.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-106">This is the first tutorial of a series.</span></span> <span data-ttu-id="8e0cb-107">В этой [серии](xref:tutorials/razor-pages/index) приводятся основные сведения о сборке веб-приложения Razor Pages в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-107">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="8e0cb-108">В конце серии вы получите приложение, которое управляет базой данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-108">At the end of the series you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="8e0cb-109">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8e0cb-110">Создание веб-приложения Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-110">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="8e0cb-111">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-111">Run the app.</span></span>
> * <span data-ttu-id="8e0cb-112">Анализ файлов проекта.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-112">Examine the project files.</span></span>

<span data-ttu-id="8e0cb-113">В конце этого учебника вы получите рабочее веб-приложения Razor Pages, сборку которого вы будете выполнять в последующих учебниках.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-113">At the end of this tutorial you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="8e0cb-115">Создание веб-приложения Razor Pages</span><span class="sxs-lookup"><span data-stu-id="8e0cb-115">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8e0cb-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8e0cb-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8e0cb-117">В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-117">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="8e0cb-118">Создайте новое веб-приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-118">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="8e0cb-119">Назовите проект **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-119">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="8e0cb-120">Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-120">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="8e0cb-122">Выберите в раскрывающемся списке **ASP.NET Core 2.2**, а затем **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-122">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>

  ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="8e0cb-124">Создается следующий начальный проект:</span><span class="sxs-lookup"><span data-stu-id="8e0cb-124">The following starter project is created:</span></span>

  ![обозреватель решений](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8e0cb-126">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="8e0cb-127">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="8e0cb-127">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="8e0cb-128">Смените каталог (`cd`) на папку, в которой будет содержаться проект.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-128">Change directories (`cd`) to a folder which will contain the project.</span></span>

* <span data-ttu-id="8e0cb-129">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="8e0cb-129">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="8e0cb-130">Команда `dotnet new` создает новый проект Razor Pages в папке *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-130">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="8e0cb-131">Команда `code` открывает папку *RazorPagesMovie* в новом экземпляре Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-131">The `code` command opens the *RazorPagesMovie* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="8e0cb-132">Появится диалоговое окно с предупреждением **В RazorPagesMovie отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="8e0cb-132">A dialog box appears with **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span>

* <span data-ttu-id="8e0cb-133">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-133">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8e0cb-134">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="8e0cb-134">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="8e0cb-135">Из терминала выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="8e0cb-135">From a terminal, run the following commands:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
```

<span data-ttu-id="8e0cb-136">Указанные выше команды используют [интерфейс командной строки .NET Core](/dotnet/core/tools/dotnet) для создания проекта Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-136">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="8e0cb-137">Открытие проекта</span><span class="sxs-lookup"><span data-stu-id="8e0cb-137">Open the project</span></span>

<span data-ttu-id="8e0cb-138">В Visual Studio откройте меню **Файл > Открыть файл** и выберите файл *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-138">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="8e0cb-139">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="8e0cb-139">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8e0cb-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8e0cb-140">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8e0cb-141">Нажмите клавиши CTRL+F5, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-141">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="8e0cb-142">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-142">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="8e0cb-143">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-143">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="8e0cb-144">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-144">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="8e0cb-145">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-145">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="8e0cb-146">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-146">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8e0cb-147">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="8e0cb-148">Нажмите клавиши **CTRL-F5**, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-148">Press **Ctrl-F5** to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="8e0cb-149">Visual Studio Code запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-149">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="8e0cb-150">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-150">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="8e0cb-151">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-151">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="8e0cb-152">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-152">Localhost only serves web requests from the local computer.</span></span>
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8e0cb-153">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="8e0cb-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="8e0cb-154">Выберите **Выполнить > Запуск без отладки**, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-154">Select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="8e0cb-155">Visual Studio запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-155">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

<!-- End of VS tabs -->

---

* <span data-ttu-id="8e0cb-156">На домашней странице нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-156">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="8e0cb-157">Это приложение не отслеживает персональные данные, но в шаблон проекта входит компонент согласия на случай, если приложение должно соответствовать [общему регламенту по защите данных (GDPR)](xref:security/gdpr) Европейского Союза.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-157">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="8e0cb-159">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="8e0cb-159">The following image shows the app after you give consent to tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)

## <a name="examine-the-project-files"></a><span data-ttu-id="8e0cb-161">Анализ файлов проекта</span><span class="sxs-lookup"><span data-stu-id="8e0cb-161">Examine the project files</span></span>

<span data-ttu-id="8e0cb-162">Ниже приведен обзор основных папок и файлов проекта, с которыми вы будете работать в последующих учебниках.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-162">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="8e0cb-163">Папка Pages</span><span class="sxs-lookup"><span data-stu-id="8e0cb-163">Pages folder</span></span>

<span data-ttu-id="8e0cb-164">Содержит страницы Razor и вспомогательные файлы.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-164">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="8e0cb-165">Каждая страница Razor — это пара файлов.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-165">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="8e0cb-166">Файл *.cshtml*, содержащий HTML-разметку с кодом C# и синтаксисом Razor.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-166">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="8e0cb-167">Файл *. cshtml.cs*, содержащий код C#, который обрабатывает события страницы.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-167">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="8e0cb-168">Имена вспомогательных файлов начинаются с символа подчеркивания.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-168">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="8e0cb-169">Например, файл *_Layout.cshtml* настраивает элементы пользовательского интерфейса, общие для всех страниц.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-169">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="8e0cb-170">Этот файл настраивает меню навигации в верхней части страницы и уведомление об авторских правах в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-170">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="8e0cb-171">Для получения дополнительной информации см. <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-171">For more information, see <xref:mvc/views/layout>.</span></span>


### <a name="wwwroot-folder"></a><span data-ttu-id="8e0cb-172">Папка wwwroot</span><span class="sxs-lookup"><span data-stu-id="8e0cb-172">wwwroot folder</span></span>

<span data-ttu-id="8e0cb-173">Содержит статические файлы, такие как HTML-файлы, файлы JavaScript и CSS-файлы.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-173">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="8e0cb-174">Для получения дополнительной информации см. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-174">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="8e0cb-175">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="8e0cb-175">appSettings.json</span></span>

<span data-ttu-id="8e0cb-176">Содержит данные конфигурации, например строки подключения.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-176">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="8e0cb-177">Для получения дополнительной информации см. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-177">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="8e0cb-178">Program.cs</span><span class="sxs-lookup"><span data-stu-id="8e0cb-178">Program.cs</span></span>

<span data-ttu-id="8e0cb-179">Содержит точку входа для программы.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-179">Contains the entry point for the program.</span></span> <span data-ttu-id="8e0cb-180">Для получения дополнительной информации см. <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-180">For more information, see <xref:fundamentals/host/web-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="8e0cb-181">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="8e0cb-181">Startup.cs</span></span>

<span data-ttu-id="8e0cb-182">Содержит код, который настраивает поведение приложения, например, требуется ли согласие для файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-182">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="8e0cb-183">Для получения дополнительной информации см. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-183">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8e0cb-184">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="8e0cb-184">Additional resources</span></span>

* [<span data-ttu-id="8e0cb-185">Версия руководства на YouTube</span><span class="sxs-lookup"><span data-stu-id="8e0cb-185">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="8e0cb-186">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="8e0cb-186">Next steps</span></span>

<span data-ttu-id="8e0cb-187">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-187">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8e0cb-188">Создание веб-приложения Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-188">Created a Razor Pages web app.</span></span>
> * <span data-ttu-id="8e0cb-189">Запуск приложения.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-189">Ran the app.</span></span>
> * <span data-ttu-id="8e0cb-190">Анализ файлов проекта.</span><span class="sxs-lookup"><span data-stu-id="8e0cb-190">Examined the project files.</span></span>

<span data-ttu-id="8e0cb-191">Перейдите к следующему учебнику в серии:</span><span class="sxs-lookup"><span data-stu-id="8e0cb-191">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="8e0cb-192">Добавление модели</span><span class="sxs-lookup"><span data-stu-id="8e0cb-192">Add a model</span></span>](xref:tutorials/razor-pages/model)
