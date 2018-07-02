---
title: Начало работы с MVC ASP.NET Core и Visual Studio
author: rick-anderson
description: Сведения о начале работы с MVC ASP.NET Core и Visual Studio.
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 1fb3947023843341403f4355c6ae1e61d7e4f6b1
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275556"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="41d15-103">Начало работы с MVC ASP.NET Core и Visual Studio</span><span class="sxs-lookup"><span data-stu-id="41d15-103">Get started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="41d15-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="41d15-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="41d15-105">Существует 3 версии этого учебника:</span><span class="sxs-lookup"><span data-stu-id="41d15-105">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="41d15-106">macOS: [Создание приложения ASP.NET Core MVC с помощью Visual Studio для Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="41d15-106">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="41d15-107">Windows: [Создание приложения ASP.NET Core MVC с помощью Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="41d15-107">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="41d15-108">macOS, Linux и Windows: [Создание приложения MVC ASP.NET Core с помощью Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="41d15-108">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="41d15-109">Установка Visual Studio и .NET Core</span><span class="sxs-lookup"><span data-stu-id="41d15-109">Install Visual Studio and .NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="41d15-110">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="41d15-110">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="41d15-111">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="41d15-111">Create a web app</span></span>

<span data-ttu-id="41d15-112">В Visual Studio выберите меню **Файл > Создать > Проект**.</span><span class="sxs-lookup"><span data-stu-id="41d15-112">From Visual Studio, select  **File > New > Project**.</span></span>

![Файл > Создать > Проект](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="41d15-114">В диалоговом окне **Создание проекта** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="41d15-114">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="41d15-115">В левой области выберите **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="41d15-115">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="41d15-116">В центральной области выберите **Веб-приложение ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="41d15-116">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="41d15-117">Назовите проект "MvcMovie" (имя "MvcMovie" необходимо присвоить для того, чтобы при копировании кода пространства имен совпали).</span><span class="sxs-lookup"><span data-stu-id="41d15-117">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="41d15-118">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="41d15-118">Tap **OK**</span></span>

![<span data-ttu-id="41d15-119">Диалоговое окно нового проекта, .NET Core в левой области, веб-узел ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="41d15-119">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="41d15-120">Заполните данные в диалоговом окне **Создание веб-приложения ASP.NET Core (.NET Core) — MvcMovie**.</span><span class="sxs-lookup"><span data-stu-id="41d15-120">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="41d15-121">Выберите в раскрывающемся списке выбора версии **ASP.NET Core 2.1**</span><span class="sxs-lookup"><span data-stu-id="41d15-121">In the version selector drop-down box select **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="41d15-122">Выберите **Веб-приложение (модель-представление-контроллер)**.</span><span class="sxs-lookup"><span data-stu-id="41d15-122">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="41d15-123">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="41d15-123">Tap **OK**.</span></span>

![<span data-ttu-id="41d15-124">Диалоговое окно нового проекта, .NET Core в левой области, веб-узел ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="41d15-124">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="41d15-125">В Visual Studio используется только что созданный вами шаблон по умолчанию для проекта MVC.</span><span class="sxs-lookup"><span data-stu-id="41d15-125">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="41d15-126">Чтобы приложение стало рабочим, осталось только указать имя проекта и выбрать несколько параметров.</span><span class="sxs-lookup"><span data-stu-id="41d15-126">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="41d15-127">Это простой начальный проект и хорошая стартовая точка.</span><span class="sxs-lookup"><span data-stu-id="41d15-127">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="41d15-128">Нажмите клавишу **F5**, чтобы запустить приложение в режиме отладки, или клавиши **CTRL-F5**, чтобы выбрать режим без отладки.</span><span class="sxs-lookup"><span data-stu-id="41d15-128">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="41d15-129"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![Запуск приложения](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="41d15-129"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="41d15-130">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем ваше приложение.</span><span class="sxs-lookup"><span data-stu-id="41d15-130">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="41d15-131">Обратите внимание на то, что в адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="41d15-131">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="41d15-132">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="41d15-132">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="41d15-133">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="41d15-133">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="41d15-134">На представленном выше снимке экрана используется порт номер 5000.</span><span class="sxs-lookup"><span data-stu-id="41d15-134">In the image above, the port number is 5000.</span></span> <span data-ttu-id="41d15-135">URL-адрес в браузере отображает `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="41d15-135">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="41d15-136">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="41d15-136">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="41d15-137">Запуск приложения с помощью клавиш **CTRL+F5** (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="41d15-137">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="41d15-138">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.</span><span class="sxs-lookup"><span data-stu-id="41d15-138">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="41d15-139">Из меню **Отладка** можно запустить приложение в режиме с отладкой или без.</span><span class="sxs-lookup"><span data-stu-id="41d15-139">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Меню отладки](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="41d15-141">Чтобы выполнить отладку приложения, нажмите кнопку **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="41d15-141">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="41d15-143">Шаблон по умолчанию включает рабочие ссылки **Домой, О программе** и **Контакты**.</span><span class="sxs-lookup"><span data-stu-id="41d15-143">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="41d15-144">На представленном выше снимке экрана эти ссылки не отображаются.</span><span class="sxs-lookup"><span data-stu-id="41d15-144">The browser image above doesn't show these links.</span></span> <span data-ttu-id="41d15-145">Если размер окна вашего браузера слишком мал, нажмите значок навигации, чтобы отобразить эти ссылки.</span><span class="sxs-lookup"><span data-stu-id="41d15-145">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Значок навигации в правом верхнем углу экрана](start-mvc/_static/2.png)

<span data-ttu-id="41d15-147">При запуске в режиме отладки нажмите клавиши **SHIFT+F5**, чтобы остановить отладку.</span><span class="sxs-lookup"><span data-stu-id="41d15-147">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="41d15-148">В следующей части этого учебника мы поговорим об MVC и приступим к написанию кода.</span><span class="sxs-lookup"><span data-stu-id="41d15-148">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="41d15-149">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="41d15-149">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="41d15-150">[!INCLUDE [](~/includes/net-core-prereqs.md) [](~/includes/net-core-prereqs.md)]</span><span class="sxs-lookup"><span data-stu-id="41d15-150">[!INCLUDE [](~/includes/net-core-prereqs.md) [](~/includes/net-core-prereqs.md)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="41d15-151">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="41d15-151">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="41d15-152">Установите Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="41d15-152">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="41d15-153">Выберите загрузку сообщества.</span><span class="sxs-lookup"><span data-stu-id="41d15-153">Select the Community download.</span></span> <span data-ttu-id="41d15-154">Если платформа Visual Studio 2017 уже установлена, пропустите этот шаг.</span><span class="sxs-lookup"><span data-stu-id="41d15-154">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="41d15-155">Установщик главной страницы Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="41d15-155">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="41d15-156">Запустите программу установки и выберите следующие рабочие нагрузки.</span><span class="sxs-lookup"><span data-stu-id="41d15-156">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="41d15-157">**ASP.NET и веб-разработка** в разделе **Интернет и облако**)</span><span class="sxs-lookup"><span data-stu-id="41d15-157">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="41d15-158">**Кроссплатформенная разработка .NET Core** (в разделе **Другие наборы инструментов**)</span><span class="sxs-lookup"><span data-stu-id="41d15-158">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**ASP.NET и веб-разработка** в разделе **Интернет и облако**)](start-mvc/_static/web_workload.png)

![**Кроссплатформенная разработка .NET Core** (в разделе **Другие наборы инструментов**)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="41d15-161">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="41d15-161">Create a web app</span></span>

<span data-ttu-id="41d15-162">В Visual Studio выберите меню **Файл > Создать > Проект**.</span><span class="sxs-lookup"><span data-stu-id="41d15-162">From Visual Studio, select  **File > New > Project**.</span></span>

![Файл > Создать > Проект](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="41d15-164">В диалоговом окне **Создание проекта** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="41d15-164">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="41d15-165">В левой области выберите **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="41d15-165">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="41d15-166">В центральной области выберите **Веб-приложение ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="41d15-166">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="41d15-167">Назовите проект "MvcMovie" (имя "MvcMovie" необходимо присвоить для того, чтобы при копировании кода пространства имен совпали).</span><span class="sxs-lookup"><span data-stu-id="41d15-167">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="41d15-168">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="41d15-168">Tap **OK**</span></span>

![<span data-ttu-id="41d15-169">Диалоговое окно нового проекта, .NET Core в левой области, веб-узел ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="41d15-169">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="41d15-170">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="41d15-170">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="41d15-171">Заполните данные в диалоговом окне **Создание веб-приложения ASP.NET Core (.NET Core) — MvcMovie**.</span><span class="sxs-lookup"><span data-stu-id="41d15-171">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="41d15-172">Выберите в раскрывающемся списке выбора версии **ASP.NET Core 2.-**.</span><span class="sxs-lookup"><span data-stu-id="41d15-172">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="41d15-173">Выберите **Веб-приложение (модель-представление-контроллер)**.</span><span class="sxs-lookup"><span data-stu-id="41d15-173">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="41d15-174">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="41d15-174">Tap **OK**.</span></span>

![<span data-ttu-id="41d15-175">Диалоговое окно нового проекта, .NET Core в левой области, веб-узел ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="41d15-175">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="41d15-176">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="41d15-176">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="41d15-177">Заполните данные в диалоговом окне **Создание веб-приложения ASP.NET Core (.NET Core) — MvcMovie**.</span><span class="sxs-lookup"><span data-stu-id="41d15-177">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="41d15-178">Выберите в раскрывающемся списке выбора версии **ASP.NET Core 1.1**.</span><span class="sxs-lookup"><span data-stu-id="41d15-178">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="41d15-179">Выберите **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="41d15-179">Tap **Web Application**</span></span>
* <span data-ttu-id="41d15-180">Сохраните значение по умолчанию **Без проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="41d15-180">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="41d15-181">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="41d15-181">Tap **OK**.</span></span>

![Новое веб-приложение ASP.NET Core](start-mvc/_static/p3.png)

---

<span data-ttu-id="41d15-183">В Visual Studio используется только что созданный вами шаблон по умолчанию для проекта MVC.</span><span class="sxs-lookup"><span data-stu-id="41d15-183">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="41d15-184">Чтобы приложение стало рабочим, осталось только указать имя проекта и выбрать несколько параметров.</span><span class="sxs-lookup"><span data-stu-id="41d15-184">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="41d15-185">Это простой начальный проект и хорошая стартовая точка.</span><span class="sxs-lookup"><span data-stu-id="41d15-185">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="41d15-186">Нажмите клавишу **F5**, чтобы запустить приложение в режиме отладки, или клавиши **CTRL-F5**, чтобы выбрать режим без отладки.</span><span class="sxs-lookup"><span data-stu-id="41d15-186">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="41d15-187"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![Запуск приложения](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="41d15-187"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="41d15-188">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем ваше приложение.</span><span class="sxs-lookup"><span data-stu-id="41d15-188">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="41d15-189">Обратите внимание на то, что в адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="41d15-189">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="41d15-190">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="41d15-190">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="41d15-191">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="41d15-191">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="41d15-192">На представленном выше снимке экрана используется порт номер 5000.</span><span class="sxs-lookup"><span data-stu-id="41d15-192">In the image above, the port number is 5000.</span></span> <span data-ttu-id="41d15-193">URL-адрес в браузере отображает `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="41d15-193">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="41d15-194">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="41d15-194">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="41d15-195">Запуск приложения с помощью клавиш **CTRL+F5** (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="41d15-195">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="41d15-196">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.</span><span class="sxs-lookup"><span data-stu-id="41d15-196">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="41d15-197">Из меню **Отладка** можно запустить приложение в режиме с отладкой или без.</span><span class="sxs-lookup"><span data-stu-id="41d15-197">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Меню отладки](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="41d15-199">Чтобы выполнить отладку приложения, нажмите кнопку **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="41d15-199">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="41d15-201">Шаблон по умолчанию включает рабочие ссылки **Домой, О программе** и **Контакты**.</span><span class="sxs-lookup"><span data-stu-id="41d15-201">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="41d15-202">На представленном выше снимке экрана эти ссылки не отображаются.</span><span class="sxs-lookup"><span data-stu-id="41d15-202">The browser image above doesn't show these links.</span></span> <span data-ttu-id="41d15-203">Если размер окна вашего браузера слишком мал, нажмите значок навигации, чтобы отобразить эти ссылки.</span><span class="sxs-lookup"><span data-stu-id="41d15-203">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Значок навигации в правом верхнем углу экрана](start-mvc/_static/2.png)

<span data-ttu-id="41d15-205">При запуске в режиме отладки нажмите клавиши **SHIFT+F5**, чтобы остановить отладку.</span><span class="sxs-lookup"><span data-stu-id="41d15-205">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="41d15-206">В следующей части этого учебника мы поговорим об MVC и приступим к написанию кода.</span><span class="sxs-lookup"><span data-stu-id="41d15-206">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end
> [!div class="step-by-step"]
> [<span data-ttu-id="41d15-207">Вперед</span><span class="sxs-lookup"><span data-stu-id="41d15-207">Next</span></span>](adding-controller.md)  
