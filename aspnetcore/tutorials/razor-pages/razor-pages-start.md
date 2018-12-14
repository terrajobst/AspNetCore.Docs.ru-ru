---
title: Начало работы с Razor Pages в ASP.NET Core
author: rick-anderson
monikerRange: '>= aspnetcore-2.2'
description: В этой серии руководств объясняется, как использовать Razor Pages в ASP.NET Core. Узнайте, как создать модель, сгенерировать код для Razor Pages, использовать Entity Framework Core и SQL Server для доступа к данным, добавлять функции поиска и проверки ввода, а также использовать возможность миграции для обновления модели.
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1152ebfcee48a46ecd28c941fce32d3fc1e05c41
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861632"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="5075b-104">Руководство. Начало работы с Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5075b-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="5075b-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="5075b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5075b-106">В этом учебнике приводятся основные сведения о веб-приложении Razor Pages в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5075b-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

<span data-ttu-id="5075b-107">Это приложение служит для управления базой данных названий фильмов.</span><span class="sxs-lookup"><span data-stu-id="5075b-107">The app manages a database of movie titles.</span></span> <span data-ttu-id="5075b-108">Вы научитесь:</span><span class="sxs-lookup"><span data-stu-id="5075b-108">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5075b-109">Создание веб-приложения Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="5075b-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="5075b-110">Добавление модели и формирование шаблона.</span><span class="sxs-lookup"><span data-stu-id="5075b-110">Add and scaffold a model.</span></span>
> * <span data-ttu-id="5075b-111">Работа с базой данных.</span><span class="sxs-lookup"><span data-stu-id="5075b-111">Work with a database.</span></span>
> * <span data-ttu-id="5075b-112">Добавление поиска и проверки.</span><span class="sxs-lookup"><span data-stu-id="5075b-112">Add search and validation.</span></span>

<span data-ttu-id="5075b-113">В конечном итоге вы получите приложение, позволяющее управлять названиями фильмов и отображать их.</span><span class="sxs-lookup"><span data-stu-id="5075b-113">At the end, you have an app that can manage and display movie titles items.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="5075b-114">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="5075b-114">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="5075b-115">Создание веб-приложения Razor</span><span class="sxs-lookup"><span data-stu-id="5075b-115">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5075b-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5075b-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5075b-117">В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="5075b-117">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="5075b-118">Создайте новое веб-приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5075b-118">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="5075b-119">Назовите проект **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="5075b-119">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="5075b-120">Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.</span><span class="sxs-lookup"><span data-stu-id="5075b-120">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="5075b-121">![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="5075b-121">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>

* <span data-ttu-id="5075b-122">Выберите в раскрывающемся списке **ASP.NET Core 2.2**, а затем **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="5075b-122">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>

  ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="5075b-124">Создается следующий начальный проект:</span><span class="sxs-lookup"><span data-stu-id="5075b-124">The following starter project is created:</span></span>

  ![обозреватель решений](razor-pages-start/_static/se2.2.png)

* <span data-ttu-id="5075b-126">Нажмите клавиши **CTRL-F5**, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="5075b-126">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="5075b-127">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение.</span><span class="sxs-lookup"><span data-stu-id="5075b-127">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="5075b-128">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="5075b-128">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="5075b-129">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="5075b-129">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="5075b-130">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="5075b-130">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="5075b-131">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="5075b-131">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="5075b-132">На представленном выше снимке экрана используется номер порта 5001.</span><span class="sxs-lookup"><span data-stu-id="5075b-132">In the preceding image, the port number is 5001.</span></span> <span data-ttu-id="5075b-133">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="5075b-133">When you run the app, you'll see a different port number.</span></span>

  <span data-ttu-id="5075b-134">Запуск приложения с помощью клавиш **CTRL+F5** (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="5075b-134">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="5075b-135">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро обновлять страницу и просматривать изменения.</span><span class="sxs-lookup"><span data-stu-id="5075b-135">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5075b-136">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5075b-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="5075b-137">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="5075b-137">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="5075b-138">Смените каталог (`cd`) на папку, в которой будет содержаться проект.</span><span class="sxs-lookup"><span data-stu-id="5075b-138">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="5075b-139">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="5075b-139">Run the following command:</span></span>

   ```console
   dotnet new webapp -o RazorPagesMovie
   code -r RazorPagesMovie
   ```

  * <span data-ttu-id="5075b-140">Появится диалоговое окно с предупреждением **В RazorPagesMovie отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="5075b-140">A dialog box appears with **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span>  <span data-ttu-id="5075b-141">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="5075b-141">Select **Yes**</span></span>

  * <span data-ttu-id="5075b-142">`dotnet new webapp -o RazorPagesMovie`. Создает новый проект Razor Pages в папке *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="5075b-142">`dotnet new webapp -o RazorPagesMovie`: creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="5075b-143">`code -r RazorPagesMovie`. Загружает файл проекта *RazorPagesMovie.csproj* в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5075b-143">`code -r RazorPagesMovie`: Loads the *RazorPagesMovie.csproj* project file in Visual Studio Code.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="5075b-144">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="5075b-144">Launch the app</span></span>

* <span data-ttu-id="5075b-145">Нажмите клавиши **CTRL-F5**, чтобы выполнить запуск без отладчика.</span><span class="sxs-lookup"><span data-stu-id="5075b-145">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="5075b-146">Visual Studio Code запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="5075b-146">Visual Studio Code starts starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="5075b-147">В адресной строке указывается `localhost:port:5001`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="5075b-147">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="5075b-148">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="5075b-148">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="5075b-149">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="5075b-149">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="5075b-150">Запуск приложения с помощью клавиш **CTRL+F5** (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="5075b-150">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="5075b-151">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро обновлять страницу и просматривать изменения.</span><span class="sxs-lookup"><span data-stu-id="5075b-151">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5075b-152">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="5075b-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="5075b-153">Из терминала выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="5075b-153">From a terminal, run the following commands:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="5075b-154">Указанные выше команды используют [интерфейс командной строки .NET Core](/dotnet/core/tools/dotnet) для создания и запуска проекта Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="5075b-154">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="5075b-155">Откройте в браузере страницу http://localhost:5000 для просмотра приложения.</span><span class="sxs-lookup"><span data-stu-id="5075b-155">Open a browser to http://localhost:5000 to view the application.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="5075b-156">Открытие проекта</span><span class="sxs-lookup"><span data-stu-id="5075b-156">Open the project</span></span>

<span data-ttu-id="5075b-157">Нажмите клавиши CTRL+C, чтобы завершить работу приложения.</span><span class="sxs-lookup"><span data-stu-id="5075b-157">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="5075b-158">В Visual Studio откройте меню **Файл > Открыть файл** и выберите файл *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="5075b-158">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="5075b-159">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="5075b-159">Launch the app</span></span>

<span data-ttu-id="5075b-160">Выберите **Выполнить > Запуск без отладки**, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="5075b-160">Select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="5075b-161">Visual Studio запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="5075b-161">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

* <span data-ttu-id="5075b-162">Нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="5075b-162">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="5075b-163">Это приложение не отслеживает персональные данные.</span><span class="sxs-lookup"><span data-stu-id="5075b-163">This app doesn't track personal information.</span></span> <span data-ttu-id="5075b-164">Созданный шаблоном код включает ресурсы для соблюдения [Общего регламента по защите данных (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="5075b-164">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="5075b-166">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="5075b-166">The following image shows the app after accepting tracking:</span></span>

  ![Домашняя или индексная страница](razor-pages-start/_static/home2.2.png)

## <a name="project-files-and-folders"></a><span data-ttu-id="5075b-168">Защита файлов и папок</span><span class="sxs-lookup"><span data-stu-id="5075b-168">Project files and folders</span></span>

<span data-ttu-id="5075b-169">В следующей таблице перечислены файлы и папки в проекте.</span><span class="sxs-lookup"><span data-stu-id="5075b-169">The following table lists the files and folders in the project.</span></span> <span data-ttu-id="5075b-170">На этой стадии важнее всего разобраться с файлом *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="5075b-170">At this point in the tutorial, the *Startup.cs* file is the most important to understand.</span></span> <span data-ttu-id="5075b-171">Каждую из указанных ниже ссылок просматривать не требуется.</span><span class="sxs-lookup"><span data-stu-id="5075b-171">You don't need to review each link provided below.</span></span> <span data-ttu-id="5075b-172">Ссылки приводятся для справки и позволяют получить дополнительную информацию о каком-либо файле или папке в проекте.</span><span class="sxs-lookup"><span data-stu-id="5075b-172">The links are provided as a reference when you need more information on a file or folder in the project.</span></span>

| <span data-ttu-id="5075b-173">Файл или папка</span><span class="sxs-lookup"><span data-stu-id="5075b-173">File or folder</span></span>              | <span data-ttu-id="5075b-174">Цель</span><span class="sxs-lookup"><span data-stu-id="5075b-174">Purpose</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="5075b-175">*wwwroot*</span><span class="sxs-lookup"><span data-stu-id="5075b-175">*wwwroot*</span></span> | <span data-ttu-id="5075b-176">Содержит статические файлы.</span><span class="sxs-lookup"><span data-stu-id="5075b-176">Contains static files.</span></span> <span data-ttu-id="5075b-177">См. [Статические файлы](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="5075b-177">See [Static files](xref:fundamentals/static-files).</span></span> |
| <span data-ttu-id="5075b-178">*Страницы*</span><span class="sxs-lookup"><span data-stu-id="5075b-178">*Pages*</span></span> | <span data-ttu-id="5075b-179">Папка для [Razor Pages](xref:razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="5075b-179">Folder for [Razor Pages](xref:razor-pages/index).</span></span> |
| <span data-ttu-id="5075b-180">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="5075b-180">*appsettings.json*</span></span> | [<span data-ttu-id="5075b-181">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="5075b-181">Configuration</span></span>](xref:fundamentals/configuration/index) |
| <span data-ttu-id="5075b-182">*Program.cs*</span><span class="sxs-lookup"><span data-stu-id="5075b-182">*Program.cs*</span></span> | <span data-ttu-id="5075b-183">[Содержит](xref:fundamentals/host/index) приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5075b-183">[Hosts](xref:fundamentals/host/index) the ASP.NET Core app.</span></span>|
| <span data-ttu-id="5075b-184">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="5075b-184">*Startup.cs*</span></span> | <span data-ttu-id="5075b-185">Настраивает службы и конвейер обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="5075b-185">Configures services and the request pipeline.</span></span> <span data-ttu-id="5075b-186">См. раздел [Запуск](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="5075b-186">See [Startup](xref:fundamentals/startup).</span></span>|

### <a name="the-pages-folder"></a><span data-ttu-id="5075b-187">Папка "Pages"</span><span class="sxs-lookup"><span data-stu-id="5075b-187">The Pages folder</span></span>

<span data-ttu-id="5075b-188">Файл *_Layout.cshtml* содержит общие элементы HTML (скрипты и таблицу стилей) и определяет макет для приложения.</span><span class="sxs-lookup"><span data-stu-id="5075b-188">The *_Layout.cshtml* file contains common HTML elements (scripts and stylesheets) and sets the layout for the application.</span></span> <span data-ttu-id="5075b-189">Например, при выборе ссылки **RazorPagesMovie**, **Главная** или **Конфиденциальность** отображаются одни и те же элементы.</span><span class="sxs-lookup"><span data-stu-id="5075b-189">For example, when you click on **RazorPagesMovie**, **Home**, or **Privacy**, you see the same elements.</span></span> <span data-ttu-id="5075b-190">В число общих элементов входят меню навигации наверху экрана и заголовок внизу окна.</span><span class="sxs-lookup"><span data-stu-id="5075b-190">The common elements include the navigation menu on the top and the header on the bottom of the window.</span></span> <span data-ttu-id="5075b-191">Дополнительные сведения см. в статье о [макете](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="5075b-191">See [Layout](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="5075b-192">Файл *_ViewImports.cshtml* содержит директивы Razor, импортированные в каждую страницу Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="5075b-192">The *_ViewImports.cshtml* file contains Razor directives that are imported into each Razor Page.</span></span> <span data-ttu-id="5075b-193">Дополнительные сведения см. в разделе [Импорт общих директив](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="5075b-193">See [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives) for more information.</span></span>

<span data-ttu-id="5075b-194">Файл *_ViewStart.cshtml* определяет свойство Razor Pages `Layout`, необходимое для использования файла *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5075b-194">The *_ViewStart.cshtml* sets the Razor Pages `Layout` property to use the *_Layout.cshtml* file.</span></span> <span data-ttu-id="5075b-195">Дополнительные сведения см. в статье о [макете](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="5075b-195">See [Layout](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="5075b-196">Файл *_ValidationScriptsPartial.cshtml* содержит ссылку на сценарии проверки [jQuery](https://jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="5075b-196">The *_ValidationScriptsPartial.cshtml* file provides a reference to [jQuery](https://jquery.com/) validation scripts.</span></span> <span data-ttu-id="5075b-197">Когда далее в этом учебнике мы добавим страницы `Create` и `Edit`, будет использоваться файл *_ValidationScriptsPartial.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5075b-197">When the `Create` and `Edit` pages are added later in the tutorial, the *_ValidationScriptsPartial.cshtml* file will be used.</span></span>

<span data-ttu-id="5075b-198">Страницы `Index`, `Error` и `Privacy` имеют следующее назначение:</span><span class="sxs-lookup"><span data-stu-id="5075b-198">`Index`, `Error`, and `Privacy` pages are provided to:</span></span>

* <span data-ttu-id="5075b-199">`Index`. Запуск приложения.</span><span class="sxs-lookup"><span data-stu-id="5075b-199">`Index`: Start an app.</span></span>
* <span data-ttu-id="5075b-200">`Error`. Вывод сведений об ошибке.</span><span class="sxs-lookup"><span data-stu-id="5075b-200">`Error`: Display error information.</span></span>
* <span data-ttu-id="5075b-201">`Privacy`. Вывод сведений о политике конфиденциальности сайта.</span><span class="sxs-lookup"><span data-stu-id="5075b-201">`Privacy`: Specify details about the site's privacy policy.</span></span>

<span data-ttu-id="5075b-202">В рамках этого учебника предыдущие страницы не используются.</span><span class="sxs-lookup"><span data-stu-id="5075b-202">For this tutorial, the preceding pages are not used.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5075b-203">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5075b-203">Visual Studio</span></span>](#tab/visual-studio)

<a name="f7"></a>
### <a name="use-f7-to-toggle-between-a-razor-page-and-the-pagemodel"></a><span data-ttu-id="5075b-204">Переключения между Razor Pages и PageModel с помощью клавиши F7</span><span class="sxs-lookup"><span data-stu-id="5075b-204">Use F7 to toggle between a Razor Page and the PageModel</span></span>

<span data-ttu-id="5075b-205">Клавиша F7 позволяет включить переключение между Razor Pages (файл *\*.cshtml*) и файлом C# (*\*.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="5075b-205">F7 toggles between a Razor Page (*\*.cshtml* file) and the C# file (*\*.cshtml.cs*).</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5075b-206">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5075b-206">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- TODO review  Need something in these tabs -->

<span data-ttu-id="5075b-207">По соглашению Razor Page (файл *\*.cshtml*) и связанная `PageModel` имеют одинаковое имя корневого файла.</span><span class="sxs-lookup"><span data-stu-id="5075b-207">By convention, the Razor Page (*\*.cshtml* file) and the associated `PageModel` have the same root file name.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5075b-208">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="5075b-208">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="5075b-209">По соглашению Razor Page (файл *\*.cshtml*) и связанная `PageModel` имеют одинаковое имя корневого файла.</span><span class="sxs-lookup"><span data-stu-id="5075b-209">By convention, the Razor Page (*\*.cshtml* file) and the associated `PageModel` have the same root file name.</span></span>

---

> [!div class="step-by-step"]
> [<span data-ttu-id="5075b-210">Далее: добавление модели</span><span class="sxs-lookup"><span data-stu-id="5075b-210">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)