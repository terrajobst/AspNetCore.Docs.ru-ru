---
title: "Построение проектов с Yeoman в ASP.NET Core"
author: spboyer
description: "В этой статье описывается создание веб-приложения, с помощью Yeoman ASP.NET Core генератора на macOS."
keywords: "ASP.NET Core, Yeoman, кроссплатформенный, ё aspnet"
ms.author: spboyer
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.assetid: fda0c2a8-1743-4505-be1a-7f8ceeef8647
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/yeoman
ms.openlocfilehash: 3a7cd83becc570d2f73014b356977fedb16f29bb
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-building-projects-with-yeoman-in-aspnet-core"></a><span data-ttu-id="5bb24-104">Введение в построение проектов с Yeoman в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5bb24-104">Introduction to building projects with Yeoman in ASP.NET Core</span></span>

<span data-ttu-id="5bb24-105">[Yeoman](http://yeoman.io/) — это система формирования шаблонов проекта для создания различных типов приложений.</span><span class="sxs-lookup"><span data-stu-id="5bb24-105">[Yeoman](http://yeoman.io/) is a project scaffolding system for creating many kinds of applications.</span></span> <span data-ttu-id="5bb24-106">Yeoman генератор для ASP.NET Core содержит множество шаблонов проекта для запуска нового веб-узла, MVC или консольное приложение.</span><span class="sxs-lookup"><span data-stu-id="5bb24-106">The Yeoman generator for ASP.NET Core contains a variety of project templates for starting a new web, MVC, or console application.</span></span>

## <a name="install-nodejs-npm-and-yeoman"></a><span data-ttu-id="5bb24-107">Установка Node.js и npm, Yeoman</span><span class="sxs-lookup"><span data-stu-id="5bb24-107">Install Node.js, npm, and Yeoman</span></span>

### <a name="prerequisites"></a><span data-ttu-id="5bb24-108">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="5bb24-108">Prerequisites</span></span>

<span data-ttu-id="5bb24-109">Node.js и npm являются обязательными для Yeoman.</span><span class="sxs-lookup"><span data-stu-id="5bb24-109">Node.js and npm are required for Yeoman.</span></span> <span data-ttu-id="5bb24-110">Загрузить [Node.js](https://nodejs.org/en/).</span><span class="sxs-lookup"><span data-stu-id="5bb24-110">Download from [Node.js](https://nodejs.org/en/).</span></span> <span data-ttu-id="5bb24-111">Программа установки включает [Node.js](https://nodejs.org/en/) и [npm](https://www.npmjs.com/).</span><span class="sxs-lookup"><span data-stu-id="5bb24-111">The installer includes [Node.js](https://nodejs.org/en/) and [npm](https://www.npmjs.com/).</span></span> <span data-ttu-id="5bb24-112">Bower также является обязательным для установки платформы пользовательского интерфейса, такие как начальной загрузки.</span><span class="sxs-lookup"><span data-stu-id="5bb24-112">Bower is also required for installing UI frameworks like Bootstrap.</span></span>

<span data-ttu-id="5bb24-113">Чтобы установить Yeoman и Bower, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="5bb24-113">To install Yeoman and Bower, run the following command:</span></span>

```console
npm install -g yo bower
```

>[!Note]
><span data-ttu-id="5bb24-114">Если появляется ошибка `npm ERR! Please try running this command again as root/Administrator.` на macOS, выполните следующие команды, используя [sudo](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/sudo.8.html):`sudo npm install -g yo bower`</span><span class="sxs-lookup"><span data-stu-id="5bb24-114">If you get the error `npm ERR! Please try running this command again as root/Administrator.` on macOS, run the following command using [sudo](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/sudo.8.html): `sudo npm install -g yo bower`</span></span>

<span data-ttu-id="5bb24-115">Из командной строки установите генератор ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="5bb24-115">From a command prompt, install the ASP.NET generator:</span></span>

```console
npm install -g generator-aspnet
```

> [!NOTE]
> <span data-ttu-id="5bb24-116">Если появляется ошибка разрешение, выполните команду под `sudo` как описано выше.</span><span class="sxs-lookup"><span data-stu-id="5bb24-116">If you get a permission error, run the command under `sudo` as described above.</span></span>

<span data-ttu-id="5bb24-117">`–g` Флаг глобально, устанавливает генератор, чтобы его можно использовать из любого пути.</span><span class="sxs-lookup"><span data-stu-id="5bb24-117">The `–g` flag installs the generator globally, so that it can be used from any path.</span></span>

## <a name="create-an-aspnet-app"></a><span data-ttu-id="5bb24-118">Создание приложения ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5bb24-118">Create an ASP.NET app</span></span>

<span data-ttu-id="5bb24-119">Выполните генератор Yeoman под управлением ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5bb24-119">Run the Yeoman-based ASP.NET generator:</span></span>

```console
yo aspnet
```

<span data-ttu-id="5bb24-120">Генератор отображает меню.</span><span class="sxs-lookup"><span data-stu-id="5bb24-120">The generator displays a menu.</span></span> <span data-ttu-id="5bb24-121">Стрелка вниз до **Web приложения, основные [без членства и авторизации]** проекта, а затем коснитесь **ввод**:</span><span class="sxs-lookup"><span data-stu-id="5bb24-121">Arrow down to the **Web Application Basic [without Membership and Authorization]** project and tap **Enter**:</span></span>

![Окно командной строки: тип приложений вы хотите создать?](yeoman/_static/yeoman-yo-aspnet.png)

<span data-ttu-id="5bb24-124">Выберите программу начальной загрузки как платформа пользовательского интерфейса и коснитесь **ввод**.</span><span class="sxs-lookup"><span data-stu-id="5bb24-124">Select Bootstrap as the UI Framework and tap **Enter**.</span></span>

<span data-ttu-id="5bb24-125">Используйте "**MyWebApp**" для имени приложения, а затем коснитесь **ввод**.</span><span class="sxs-lookup"><span data-stu-id="5bb24-125">Use "**MyWebApp**" for the app name and then tap **Enter**.</span></span>

<span data-ttu-id="5bb24-126">Yeoman будет формировать проекта и его вспомогательные файлы.</span><span class="sxs-lookup"><span data-stu-id="5bb24-126">Yeoman will scaffold the project and its supporting files.</span></span> <span data-ttu-id="5bb24-127">Предлагаемые дальнейшие действия также предоставляются в виде команды.</span><span class="sxs-lookup"><span data-stu-id="5bb24-127">Suggested next steps are also provided in the form of commands.</span></span>

![Окно командной строки: имя приложения ASP.NET?](yeoman/_static/yeoman-yo-aspnet-created.png)

<span data-ttu-id="5bb24-130">[Генератор](https://www.npmjs.com/package/generator-aspnet) создает ASP.NET Core проекты, которые могут быть загружены в коде Visual Studio, Visual Studio или запустите из командной строки.</span><span class="sxs-lookup"><span data-stu-id="5bb24-130">The [ASP.NET generator](https://www.npmjs.com/package/generator-aspnet) creates ASP.NET Core projects that can be loaded into Visual Studio Code, Visual Studio, or run from the command line.</span></span>

## <a name="restore-build-and-run"></a><span data-ttu-id="5bb24-131">Восстановление, сборка и выполнение</span><span class="sxs-lookup"><span data-stu-id="5bb24-131">Restore, build, and run</span></span>

<span data-ttu-id="5bb24-132">Выполните предлагаемые команд, изменив каталоги для `MyWebApp` каталога.</span><span class="sxs-lookup"><span data-stu-id="5bb24-132">Follow the suggested commands by changing directories to the `MyWebApp` directory.</span></span> <span data-ttu-id="5bb24-133">Затем запустите `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="5bb24-133">Then run `dotnet restore`.</span></span>

![командное окно](yeoman/_static/dotnet-restore.png)

<span data-ttu-id="5bb24-135">Построение и запуск приложения с использованием `dotnet build` и `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="5bb24-135">Build and run the app using `dotnet build` and `dotnet run`:</span></span>

![командное окно](yeoman/_static/dotnet-build-run.png)

<span data-ttu-id="5bb24-137">На этом этапе можно перейти к показано тестирование приложения ASP.NET Core только что созданный URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="5bb24-137">At this point, you can navigate to the URL shown to test the newly-created ASP.NET Core app.</span></span>

## <a name="client-side-packages"></a><span data-ttu-id="5bb24-138">Пакеты на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="5bb24-138">Client-side packages</span></span>

<span data-ttu-id="5bb24-139">Интерфейсный ресурсы предоставляются с помощью шаблонов из Yeoman с помощью генератора [Bower](xref:client-side/bower) диспетчер клиентского пакета, добавление *bower.json* и *.bowerrc* файлы для восстановления клиентских пакетов, с помощью Bower.</span><span class="sxs-lookup"><span data-stu-id="5bb24-139">The front-end resources are provided by the templates from the Yeoman generator using the [Bower](xref:client-side/bower) client-side package manager, adding *bower.json* and *.bowerrc* files to restore client-side packages using Bower.</span></span>

<span data-ttu-id="5bb24-140">[BundlerMinifier](xref:client-side/bundling-and-minification) компонент также включается по умолчанию для облегчения объединения (объединение) и минификацию CSS, JavaScript и HTML.</span><span class="sxs-lookup"><span data-stu-id="5bb24-140">The [BundlerMinifier](xref:client-side/bundling-and-minification) component is also included by default for ease of concatenation (bundling) and minification of CSS, JavaScript, and HTML.</span></span>

## <a name="building-and-running-from-visual-studio"></a><span data-ttu-id="5bb24-141">Построение и запуск из Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5bb24-141">Building and running from Visual Studio</span></span>

<span data-ttu-id="5bb24-142">Можно загрузить созданный веб-проекта ASP.NET Core непосредственно в Visual Studio, построения и запуска проекта из него.</span><span class="sxs-lookup"><span data-stu-id="5bb24-142">You can load your generated ASP.NET Core web project directly into Visual Studio, then build and run your project from there.</span></span> <span data-ttu-id="5bb24-143">Следуйте приведенным выше инструкциям для формировать с помощью Yeoman нового приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5bb24-143">Follow the instructions above to scaffold a new ASP.NET Core app using Yeoman.</span></span> <span data-ttu-id="5bb24-144">На этот раз выберите **веб-приложение** из меню и имя приложения `MyWebApp`.</span><span class="sxs-lookup"><span data-stu-id="5bb24-144">This time, choose **Web Application** from the menu and name the app `MyWebApp`.</span></span>

<span data-ttu-id="5bb24-145">Запустите Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5bb24-145">Open Visual Studio.</span></span> <span data-ttu-id="5bb24-146">В меню "файл" выберите ‣ Открыть решение или проект.</span><span class="sxs-lookup"><span data-stu-id="5bb24-146">From the File menu, select Open ‣ Project/Solution.</span></span>

<span data-ttu-id="5bb24-147">В диалоговом окне Открытие проекта перейдите на вкладку *.csproj* файла, выберите его и нажмите кнопку **откройте** кнопки.</span><span class="sxs-lookup"><span data-stu-id="5bb24-147">In the Open Project dialog, navigate to the *.csproj* file, select it, and click the **Open** button.</span></span> <span data-ttu-id="5bb24-148">В обозревателе решений щелкните проект должно быть похоже на снимке экрана ниже.</span><span class="sxs-lookup"><span data-stu-id="5bb24-148">In the Solution Explorer, the project should look something like the screenshot below.</span></span>

![Файлы и папки из нового проекта в обозревателе решений](yeoman/_static/yeoman-solution.png)

<span data-ttu-id="5bb24-150">Yeoman scaffolds веб-приложение MVC, дополненный и стороне сервера и клиента поддержка построения.</span><span class="sxs-lookup"><span data-stu-id="5bb24-150">Yeoman scaffolds a MVC web application, complete with both server- and client-side build support.</span></span> <span data-ttu-id="5bb24-151">Серверные зависимости перечислены в разделе **зависимости/NuGet** узла и зависимости на стороне клиента в **зависимости и Bower** обозревателя решений.</span><span class="sxs-lookup"><span data-stu-id="5bb24-151">Server-side dependencies are listed under the **Dependencies/NuGet** node, and client-side dependencies in the **Dependencies/Bower** node of Solution Explorer.</span></span> <span data-ttu-id="5bb24-152">Зависимости, автоматически восстанавливаются при загрузке проекта.</span><span class="sxs-lookup"><span data-stu-id="5bb24-152">Dependencies are restored automatically when the project is loaded.</span></span>

![В дереве обозревателя решений в узле зависимости в папку Bower открыт список его зависимостей.](yeoman/_static/yeoman-loading-dependencies.png)

<span data-ttu-id="5bb24-154">После восстановления всех зависимостей нажмите **F5** для запуска проекта.</span><span class="sxs-lookup"><span data-stu-id="5bb24-154">When all the dependencies are restored, press **F5** to run the project.</span></span> <span data-ttu-id="5bb24-155">Домашняя страница по умолчанию отображается в браузере.</span><span class="sxs-lookup"><span data-stu-id="5bb24-155">The default home page displays in the browser.</span></span>

![Открыть в Microsoft Edge веб-приложения](yeoman/_static/yeoman-home-page.png)

## <a name="restoring-building-and-hosting-from-a-command-line"></a><span data-ttu-id="5bb24-157">Восстановление, построение и размещение из командной строки</span><span class="sxs-lookup"><span data-stu-id="5bb24-157">Restoring, building, and hosting from a command line</span></span>

<span data-ttu-id="5bb24-158">Можно подготовить и размещения веб-приложения с помощью .NET Core CLI.</span><span class="sxs-lookup"><span data-stu-id="5bb24-158">You can prepare and host your web application using the .NET Core CLI.</span></span>

<span data-ttu-id="5bb24-159">В командной строке измените текущий каталог на папку, содержащую проект (то есть к папке, содержащей *.csproj* файла):</span><span class="sxs-lookup"><span data-stu-id="5bb24-159">At a command prompt, change the current directory to the folder containing the project (that is, the folder containing the *.csproj* file):</span></span>

```console
cd src\MyWebApp
```

<span data-ttu-id="5bb24-160">Восстановление зависимости пакета NuGet для проекта.</span><span class="sxs-lookup"><span data-stu-id="5bb24-160">Restore the project's NuGet package dependencies:</span></span>

```console
dotnet restore
```

<span data-ttu-id="5bb24-161">Запуск приложения:</span><span class="sxs-lookup"><span data-stu-id="5bb24-161">Run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="5bb24-162">Кросс платформенных [Kestrel](xref:fundamentals/servers/kestrel) начинается веб-сервер прослушивает порт 5000.</span><span class="sxs-lookup"><span data-stu-id="5bb24-162">The cross-platform [Kestrel](xref:fundamentals/servers/kestrel) web server will begin listening on port 5000.</span></span>

<span data-ttu-id="5bb24-163">Откройте веб-браузер и перейдите к `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="5bb24-163">Open a web browser, and navigate to `http://localhost:5000`.</span></span>

![Открыть в Microsoft Edge веб-приложения](yeoman/_static/yeoman-home-page_5000.png)

## <a name="adding-to-your-project-with-sub-generators"></a><span data-ttu-id="5bb24-165">Добавление в проект с генераторами sub</span><span class="sxs-lookup"><span data-stu-id="5bb24-165">Adding to your project with sub generators</span></span>

<span data-ttu-id="5bb24-166">С помощью Yeoman [sub генераторы](https://www.github.com/omnisharp/generator-aspnet#sub-generators), можно добавить любую `nuget.config` или `web.config` после создания проекта.</span><span class="sxs-lookup"><span data-stu-id="5bb24-166">Using Yeoman [sub generators](https://www.github.com/omnisharp/generator-aspnet#sub-generators), you can add either a `nuget.config` or a `web.config` after the project is created.</span></span> <span data-ttu-id="5bb24-167">Например выполните следующую команду из каталога, в котором следует создать файл:</span><span class="sxs-lookup"><span data-stu-id="5bb24-167">For example, execute the following command from the directory in which the file should be created:</span></span>

```console
yo aspnet:nugetconfig
```

<span data-ttu-id="5bb24-168">Результатом является файл конфигурации NuGet с именем `nuget.config` со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="5bb24-168">The result is a NuGet configuration file named `nuget.config` with the following content:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
 <packageSources>
    <!--To inherit the global NuGet package sources remove the <clear/> line below -->
    <clear />
 </packageSources>
</configuration>
```

## <a name="additional-resources"></a><span data-ttu-id="5bb24-169">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="5bb24-169">Additional resources</span></span>

* [<span data-ttu-id="5bb24-170">Серверы (Kestrel и WebListener)</span><span class="sxs-lookup"><span data-stu-id="5bb24-170">Servers (Kestrel and WebListener)</span></span>](xref:fundamentals/servers/index)
* [<span data-ttu-id="5bb24-171">Принципы работы</span><span class="sxs-lookup"><span data-stu-id="5bb24-171">Fundamentals</span></span>](xref:fundamentals/index)
