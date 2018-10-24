---
title: Начало работы с MVC ASP.NET Core и Visual Studio
author: rick-anderson
description: Сведения о начале работы с MVC ASP.NET Core и Visual Studio.
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: fe555e4cfcaec5d4bb8ccee00b06d1bbcaae9dcd
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391210"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="7ca21-103">Начало работы с MVC ASP.NET Core и Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7ca21-103">Get started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="7ca21-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="7ca21-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="7ca21-105">Существует 3 версии этого учебника:</span><span class="sxs-lookup"><span data-stu-id="7ca21-105">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="7ca21-106">macOS: [Создание приложения ASP.NET Core MVC с помощью Visual Studio для Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="7ca21-106">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="7ca21-107">Windows: [Создание приложения ASP.NET Core MVC с помощью Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="7ca21-107">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="7ca21-108">macOS, Linux и Windows: [Создание приложения MVC ASP.NET Core с помощью Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="7ca21-108">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="7ca21-109">Установка Visual Studio и .NET Core</span><span class="sxs-lookup"><span data-stu-id="7ca21-109">Install Visual Studio and .NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="7ca21-110">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="7ca21-110">Create a web app</span></span>

<span data-ttu-id="7ca21-111">В Visual Studio выберите меню **Файл > Создать > Проект**.</span><span class="sxs-lookup"><span data-stu-id="7ca21-111">From Visual Studio, select  **File > New > Project**.</span></span>

![Файл > Создать > Проект](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="7ca21-113">В диалоговом окне **Создание проекта** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="7ca21-113">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="7ca21-114">В левой области выберите **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="7ca21-114">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="7ca21-115">В центральной области выберите **Веб-приложение ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="7ca21-115">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="7ca21-116">Назовите проект "MvcMovie" (имя "MvcMovie" необходимо присвоить для того, чтобы при копировании кода пространства имен совпали).</span><span class="sxs-lookup"><span data-stu-id="7ca21-116">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="7ca21-117">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="7ca21-117">Tap **OK**</span></span>

![<span data-ttu-id="7ca21-118">Диалоговое окно нового проекта, .NET Core в левой области, веб-узел ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7ca21-118">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="7ca21-119">Заполните данные в диалоговом окне **Создание веб-приложения ASP.NET Core (.NET Core) — MvcMovie**.</span><span class="sxs-lookup"><span data-stu-id="7ca21-119">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="7ca21-120">Выберите в раскрывающемся списке выбора версии **ASP.NET Core 2.1**</span><span class="sxs-lookup"><span data-stu-id="7ca21-120">In the version selector drop-down box select **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="7ca21-121">Выберите **Веб-приложение (модель — представление — контроллер)**</span><span class="sxs-lookup"><span data-stu-id="7ca21-121">Select **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="7ca21-122">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="7ca21-122">Tap **OK**.</span></span>

![<span data-ttu-id="7ca21-123">Диалоговое окно нового проекта, .NET Core в левой области, веб-узел ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7ca21-123">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="7ca21-124">В Visual Studio используется только что созданный вами шаблон по умолчанию для проекта MVC.</span><span class="sxs-lookup"><span data-stu-id="7ca21-124">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="7ca21-125">Чтобы приложение стало рабочим, осталось только указать имя проекта и выбрать несколько параметров.</span><span class="sxs-lookup"><span data-stu-id="7ca21-125">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="7ca21-126">Это простой начальный проект и хорошая стартовая точка.</span><span class="sxs-lookup"><span data-stu-id="7ca21-126">This is a basic starter project, and it's a good place to start.</span></span>

<span data-ttu-id="7ca21-127">Нажмите клавишу **F5**, чтобы запустить приложение в режиме отладки, или клавиши **CTRL-F5**, чтобы выбрать режим без отладки.</span><span class="sxs-lookup"><span data-stu-id="7ca21-127">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="7ca21-128"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![Запуск приложения](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="7ca21-128"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="7ca21-129">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем ваше приложение.</span><span class="sxs-lookup"><span data-stu-id="7ca21-129">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="7ca21-130">Обратите внимание на то, что в адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="7ca21-130">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="7ca21-131">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="7ca21-131">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="7ca21-132">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="7ca21-132">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="7ca21-133">На представленном выше снимке экрана используется порт номер 5000.</span><span class="sxs-lookup"><span data-stu-id="7ca21-133">In the image above, the port number is 5000.</span></span> <span data-ttu-id="7ca21-134">URL-адрес в браузере отображает `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="7ca21-134">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="7ca21-135">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="7ca21-135">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="7ca21-136">Запуск приложения с помощью клавиш **CTRL+F5** (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="7ca21-136">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="7ca21-137">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.</span><span class="sxs-lookup"><span data-stu-id="7ca21-137">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="7ca21-138">Из меню **Отладка** можно запустить приложение в режиме с отладкой или без.</span><span class="sxs-lookup"><span data-stu-id="7ca21-138">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Меню отладки](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="7ca21-140">Чтобы выполнить отладку приложения, нажмите кнопку **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="7ca21-140">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="7ca21-142">Шаблон по умолчанию включает рабочие ссылки **Домой, О программе** и **Контакты**.</span><span class="sxs-lookup"><span data-stu-id="7ca21-142">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="7ca21-143">На представленном выше снимке экрана эти ссылки не отображаются.</span><span class="sxs-lookup"><span data-stu-id="7ca21-143">The browser image above doesn't show these links.</span></span> <span data-ttu-id="7ca21-144">Если размер окна вашего браузера слишком мал, нажмите значок навигации, чтобы отобразить эти ссылки.</span><span class="sxs-lookup"><span data-stu-id="7ca21-144">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Значок навигации в правом верхнем углу экрана](start-mvc/_static/2.png)

<span data-ttu-id="7ca21-146">При запуске в режиме отладки нажмите клавиши **SHIFT+F5**, чтобы остановить отладку.</span><span class="sxs-lookup"><span data-stu-id="7ca21-146">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="7ca21-147">В следующей части этого учебника мы поговорим об MVC и приступим к написанию кода.</span><span class="sxs-lookup"><span data-stu-id="7ca21-147">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7ca21-148">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7ca21-148">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!INCLUDE [](~/includes/net-core-prereqs.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7ca21-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7ca21-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="7ca21-150">Установите Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="7ca21-150">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="7ca21-151">Выберите загрузку сообщества.</span><span class="sxs-lookup"><span data-stu-id="7ca21-151">Select the Community download.</span></span> <span data-ttu-id="7ca21-152">Если платформа Visual Studio 2017 уже установлена, пропустите этот шаг.</span><span class="sxs-lookup"><span data-stu-id="7ca21-152">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="7ca21-153">Установщик главной страницы Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="7ca21-153">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="7ca21-154">Запустите программу установки и выберите следующие рабочие нагрузки.</span><span class="sxs-lookup"><span data-stu-id="7ca21-154">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="7ca21-155">**ASP.NET и веб-разработка** в разделе **Интернет и облако**)</span><span class="sxs-lookup"><span data-stu-id="7ca21-155">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="7ca21-156">**Кроссплатформенная разработка .NET Core** (в разделе **Другие наборы инструментов**)</span><span class="sxs-lookup"><span data-stu-id="7ca21-156">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**ASP.NET и веб-разработка** в разделе **Интернет и облако**)](start-mvc/_static/web_workload.png)

![**Кроссплатформенная разработка .NET Core** (в разделе **Другие наборы инструментов**)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="7ca21-159">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="7ca21-159">Create a web app</span></span>

<span data-ttu-id="7ca21-160">В Visual Studio выберите меню **Файл > Создать > Проект**.</span><span class="sxs-lookup"><span data-stu-id="7ca21-160">From Visual Studio, select  **File > New > Project**.</span></span>

![Файл > Создать > Проект](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="7ca21-162">В диалоговом окне **Создание проекта** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="7ca21-162">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="7ca21-163">В левой области выберите **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="7ca21-163">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="7ca21-164">В центральной области выберите **Веб-приложение ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="7ca21-164">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="7ca21-165">Назовите проект "MvcMovie" (имя "MvcMovie" необходимо присвоить для того, чтобы при копировании кода пространства имен совпали).</span><span class="sxs-lookup"><span data-stu-id="7ca21-165">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="7ca21-166">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="7ca21-166">Tap **OK**</span></span>

![<span data-ttu-id="7ca21-167">Диалоговое окно нового проекта, .NET Core в левой области, веб-узел ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7ca21-167">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7ca21-168">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7ca21-168">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7ca21-169">Заполните данные в диалоговом окне **Создание веб-приложения ASP.NET Core (.NET Core) — MvcMovie**.</span><span class="sxs-lookup"><span data-stu-id="7ca21-169">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="7ca21-170">Выберите в раскрывающемся списке выбора версии **ASP.NET Core 2.-**.</span><span class="sxs-lookup"><span data-stu-id="7ca21-170">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="7ca21-171">Выберите **Веб-приложение (модель-представление-контроллер)**.</span><span class="sxs-lookup"><span data-stu-id="7ca21-171">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="7ca21-172">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="7ca21-172">Tap **OK**.</span></span>

![<span data-ttu-id="7ca21-173">Диалоговое окно нового проекта, .NET Core в левой области, веб-узел ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7ca21-173">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7ca21-174">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7ca21-174">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7ca21-175">Заполните данные в диалоговом окне **Создание веб-приложения ASP.NET Core (.NET Core) — MvcMovie**.</span><span class="sxs-lookup"><span data-stu-id="7ca21-175">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="7ca21-176">Выберите в раскрывающемся списке выбора версии **ASP.NET Core 1.1**.</span><span class="sxs-lookup"><span data-stu-id="7ca21-176">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="7ca21-177">Выберите **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="7ca21-177">Tap **Web Application**</span></span>
* <span data-ttu-id="7ca21-178">Сохраните значение по умолчанию **Без проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="7ca21-178">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="7ca21-179">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="7ca21-179">Tap **OK**.</span></span>

![Новое веб-приложение ASP.NET Core](start-mvc/_static/p3.png)

---

<span data-ttu-id="7ca21-181">В Visual Studio используется только что созданный вами шаблон по умолчанию для проекта MVC.</span><span class="sxs-lookup"><span data-stu-id="7ca21-181">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="7ca21-182">Чтобы приложение стало рабочим, осталось только указать имя проекта и выбрать несколько параметров.</span><span class="sxs-lookup"><span data-stu-id="7ca21-182">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="7ca21-183">Это простой начальный проект и хорошая стартовая точка.</span><span class="sxs-lookup"><span data-stu-id="7ca21-183">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="7ca21-184">Нажмите клавишу **F5**, чтобы запустить приложение в режиме отладки, или клавиши **CTRL-F5**, чтобы выбрать режим без отладки.</span><span class="sxs-lookup"><span data-stu-id="7ca21-184">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="7ca21-185"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![Запуск приложения](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="7ca21-185"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="7ca21-186">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем ваше приложение.</span><span class="sxs-lookup"><span data-stu-id="7ca21-186">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="7ca21-187">Обратите внимание на то, что в адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="7ca21-187">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="7ca21-188">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="7ca21-188">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="7ca21-189">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="7ca21-189">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="7ca21-190">На представленном выше снимке экрана используется порт номер 5000.</span><span class="sxs-lookup"><span data-stu-id="7ca21-190">In the image above, the port number is 5000.</span></span> <span data-ttu-id="7ca21-191">URL-адрес в браузере отображает `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="7ca21-191">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="7ca21-192">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="7ca21-192">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="7ca21-193">Запуск приложения с помощью клавиш **CTRL+F5** (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="7ca21-193">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="7ca21-194">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.</span><span class="sxs-lookup"><span data-stu-id="7ca21-194">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="7ca21-195">Из меню **Отладка** можно запустить приложение в режиме с отладкой или без.</span><span class="sxs-lookup"><span data-stu-id="7ca21-195">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Меню отладки](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="7ca21-197">Чтобы выполнить отладку приложения, нажмите кнопку **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="7ca21-197">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="7ca21-199">Шаблон по умолчанию включает рабочие ссылки **Домой, О программе** и **Контакты**.</span><span class="sxs-lookup"><span data-stu-id="7ca21-199">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="7ca21-200">На представленном выше снимке экрана эти ссылки не отображаются.</span><span class="sxs-lookup"><span data-stu-id="7ca21-200">The browser image above doesn't show these links.</span></span> <span data-ttu-id="7ca21-201">Если размер окна вашего браузера слишком мал, нажмите значок навигации, чтобы отобразить эти ссылки.</span><span class="sxs-lookup"><span data-stu-id="7ca21-201">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Значок навигации в правом верхнем углу экрана](start-mvc/_static/2.png)

<span data-ttu-id="7ca21-203">При запуске в режиме отладки нажмите клавиши **SHIFT+F5**, чтобы остановить отладку.</span><span class="sxs-lookup"><span data-stu-id="7ca21-203">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="7ca21-204">В следующей части этого учебника мы поговорим об MVC и приступим к написанию кода.</span><span class="sxs-lookup"><span data-stu-id="7ca21-204">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="7ca21-205">Вперед</span><span class="sxs-lookup"><span data-stu-id="7ca21-205">Next</span></span>](adding-controller.md)  
