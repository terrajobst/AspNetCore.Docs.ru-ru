---
title: Начало работы с MVC ASP.NET Core и Visual Studio
author: rick-anderson
description: Сведения о начале работы с MVC ASP.NET Core и Visual Studio.
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 738c49272c2ae2b075866001f06ad09fe73969f9
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862204"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="f910f-103">Начало работы с MVC ASP.NET Core и Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f910f-103">Get started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="f910f-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="f910f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="f910f-105">Существует 3 версии этого учебника:</span><span class="sxs-lookup"><span data-stu-id="f910f-105">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="f910f-106">macOS: [Создание приложения ASP.NET Core MVC с помощью Visual Studio для Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="f910f-106">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="f910f-107">Windows: [Создание приложения ASP.NET Core MVC с помощью Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="f910f-107">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="f910f-108">macOS, Linux и Windows: [Создание приложения MVC ASP.NET Core с помощью Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="f910f-108">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

> [!NOTE]
> <span data-ttu-id="f910f-109">Мы тестируем удобство использования новой предлагаемой структуры оглавления для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f910f-109">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="f910f-110">Если у вас есть несколько минут, и вы хотите попробовать найти семь разных тем в существующей или планируемой структуре оглавления, [щелкните здесь, чтобы принять участие в исследовании](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span><span class="sxs-lookup"><span data-stu-id="f910f-110">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="f910f-111">Установка Visual Studio и .NET Core</span><span class="sxs-lookup"><span data-stu-id="f910f-111">Install Visual Studio and .NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="f910f-112">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="f910f-112">Create a web app</span></span>

<span data-ttu-id="f910f-113">В Visual Studio выберите меню **Файл > Создать > Проект**.</span><span class="sxs-lookup"><span data-stu-id="f910f-113">From Visual Studio, select  **File > New > Project**.</span></span>

![Файл > Создать > Проект](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="f910f-115">В диалоговом окне **Создание проекта** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="f910f-115">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="f910f-116">В левой области выберите **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="f910f-116">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="f910f-117">В центральной области выберите **Веб-приложение ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="f910f-117">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="f910f-118">Назовите проект "MvcMovie" (имя "MvcMovie" необходимо присвоить для того, чтобы при копировании кода пространства имен совпали).</span><span class="sxs-lookup"><span data-stu-id="f910f-118">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="f910f-119">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="f910f-119">Tap **OK**</span></span>

![<span data-ttu-id="f910f-120">Диалоговое окно нового проекта, .NET Core в левой области, веб-узел ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f910f-120">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="f910f-121">Заполните данные в диалоговом окне **Создание веб-приложения ASP.NET Core (.NET Core) — MvcMovie**.</span><span class="sxs-lookup"><span data-stu-id="f910f-121">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="f910f-122">Выберите в раскрывающемся списке выбора версии **ASP.NET Core 2.1**</span><span class="sxs-lookup"><span data-stu-id="f910f-122">In the version selector drop-down box select **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="f910f-123">Выберите **Веб-приложение (модель — представление — контроллер)**</span><span class="sxs-lookup"><span data-stu-id="f910f-123">Select **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="f910f-124">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="f910f-124">Tap **OK**.</span></span>

![<span data-ttu-id="f910f-125">Диалоговое окно нового проекта, .NET Core в левой области, веб-узел ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f910f-125">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="f910f-126">В Visual Studio используется только что созданный вами шаблон по умолчанию для проекта MVC.</span><span class="sxs-lookup"><span data-stu-id="f910f-126">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="f910f-127">Чтобы приложение стало рабочим, осталось только указать имя проекта и выбрать несколько параметров.</span><span class="sxs-lookup"><span data-stu-id="f910f-127">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="f910f-128">Это простой начальный проект и хорошая стартовая точка.</span><span class="sxs-lookup"><span data-stu-id="f910f-128">This is a basic starter project, and it's a good place to start.</span></span>

<span data-ttu-id="f910f-129">Нажмите клавишу **F5**, чтобы запустить приложение в режиме отладки, или клавиши **CTRL-F5**, чтобы выбрать режим без отладки.</span><span class="sxs-lookup"><span data-stu-id="f910f-129">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="f910f-130"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![Запуск приложения](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="f910f-130"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="f910f-131">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем ваше приложение.</span><span class="sxs-lookup"><span data-stu-id="f910f-131">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="f910f-132">Обратите внимание на то, что в адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="f910f-132">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="f910f-133">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="f910f-133">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="f910f-134">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="f910f-134">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="f910f-135">На представленном выше снимке экрана используется порт номер 5000.</span><span class="sxs-lookup"><span data-stu-id="f910f-135">In the image above, the port number is 5000.</span></span> <span data-ttu-id="f910f-136">URL-адрес в браузере отображает `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="f910f-136">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="f910f-137">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="f910f-137">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="f910f-138">Запуск приложения с помощью клавиш **CTRL+F5** (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="f910f-138">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="f910f-139">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.</span><span class="sxs-lookup"><span data-stu-id="f910f-139">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="f910f-140">Из меню **Отладка** можно запустить приложение в режиме с отладкой или без.</span><span class="sxs-lookup"><span data-stu-id="f910f-140">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Меню отладки](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="f910f-142">Чтобы выполнить отладку приложения, нажмите кнопку **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="f910f-142">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="f910f-144">Шаблон по умолчанию включает рабочие ссылки **Домой, О программе** и **Контакты**.</span><span class="sxs-lookup"><span data-stu-id="f910f-144">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="f910f-145">На представленном выше снимке экрана эти ссылки не отображаются.</span><span class="sxs-lookup"><span data-stu-id="f910f-145">The browser image above doesn't show these links.</span></span> <span data-ttu-id="f910f-146">Если размер окна вашего браузера слишком мал, нажмите значок навигации, чтобы отобразить эти ссылки.</span><span class="sxs-lookup"><span data-stu-id="f910f-146">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Значок навигации в правом верхнем углу экрана](start-mvc/_static/2.png)

<span data-ttu-id="f910f-148">При запуске в режиме отладки нажмите клавиши **SHIFT+F5**, чтобы остановить отладку.</span><span class="sxs-lookup"><span data-stu-id="f910f-148">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="f910f-149">В следующей части этого учебника мы поговорим об MVC и приступим к написанию кода.</span><span class="sxs-lookup"><span data-stu-id="f910f-149">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="f910f-150">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="f910f-150">Create a web app</span></span>

<span data-ttu-id="f910f-151">В Visual Studio выберите меню **Файл > Создать > Проект**.</span><span class="sxs-lookup"><span data-stu-id="f910f-151">From Visual Studio, select  **File > New > Project**.</span></span>

![Файл > Создать > Проект](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="f910f-153">В диалоговом окне **Создание проекта** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="f910f-153">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="f910f-154">В левой области выберите **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="f910f-154">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="f910f-155">В центральной области выберите **Веб-приложение ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="f910f-155">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="f910f-156">Назовите проект "MvcMovie" (имя "MvcMovie" необходимо присвоить для того, чтобы при копировании кода пространства имен совпали).</span><span class="sxs-lookup"><span data-stu-id="f910f-156">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="f910f-157">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="f910f-157">Tap **OK**</span></span>

![<span data-ttu-id="f910f-158">Диалоговое окно нового проекта, .NET Core в левой области, веб-узел ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f910f-158">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)

<span data-ttu-id="f910f-159">Заполните данные в диалоговом окне **Создание веб-приложения ASP.NET Core (.NET Core) — MvcMovie**.</span><span class="sxs-lookup"><span data-stu-id="f910f-159">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="f910f-160">Выберите в раскрывающемся списке выбора версии **ASP.NET Core 2.-**.</span><span class="sxs-lookup"><span data-stu-id="f910f-160">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="f910f-161">Выберите **Веб-приложение (модель-представление-контроллер)**.</span><span class="sxs-lookup"><span data-stu-id="f910f-161">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="f910f-162">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="f910f-162">Tap **OK**.</span></span>

![<span data-ttu-id="f910f-163">Диалоговое окно нового проекта, .NET Core в левой области, веб-узел ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f910f-163">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

<span data-ttu-id="f910f-164">В Visual Studio используется только что созданный вами шаблон по умолчанию для проекта MVC.</span><span class="sxs-lookup"><span data-stu-id="f910f-164">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="f910f-165">Чтобы приложение стало рабочим, осталось только указать имя проекта и выбрать несколько параметров.</span><span class="sxs-lookup"><span data-stu-id="f910f-165">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="f910f-166">Это простой начальный проект и хорошая стартовая точка.</span><span class="sxs-lookup"><span data-stu-id="f910f-166">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="f910f-167">Нажмите клавишу **F5**, чтобы запустить приложение в режиме отладки, или клавиши **CTRL-F5**, чтобы выбрать режим без отладки.</span><span class="sxs-lookup"><span data-stu-id="f910f-167">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="f910f-168"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![Запуск приложения](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="f910f-168"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="f910f-169">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем ваше приложение.</span><span class="sxs-lookup"><span data-stu-id="f910f-169">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="f910f-170">Обратите внимание на то, что в адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="f910f-170">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="f910f-171">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="f910f-171">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="f910f-172">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="f910f-172">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="f910f-173">На представленном выше снимке экрана используется порт номер 5000.</span><span class="sxs-lookup"><span data-stu-id="f910f-173">In the image above, the port number is 5000.</span></span> <span data-ttu-id="f910f-174">URL-адрес в браузере отображает `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="f910f-174">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="f910f-175">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="f910f-175">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="f910f-176">Запуск приложения с помощью клавиш **CTRL+F5** (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="f910f-176">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="f910f-177">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.</span><span class="sxs-lookup"><span data-stu-id="f910f-177">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="f910f-178">Из меню **Отладка** можно запустить приложение в режиме с отладкой или без.</span><span class="sxs-lookup"><span data-stu-id="f910f-178">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Меню отладки](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="f910f-180">Чтобы выполнить отладку приложения, нажмите кнопку **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="f910f-180">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="f910f-182">Шаблон по умолчанию включает рабочие ссылки **Домой, О программе** и **Контакты**.</span><span class="sxs-lookup"><span data-stu-id="f910f-182">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="f910f-183">На представленном выше снимке экрана эти ссылки не отображаются.</span><span class="sxs-lookup"><span data-stu-id="f910f-183">The browser image above doesn't show these links.</span></span> <span data-ttu-id="f910f-184">Если размер окна вашего браузера слишком мал, нажмите значок навигации, чтобы отобразить эти ссылки.</span><span class="sxs-lookup"><span data-stu-id="f910f-184">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Значок навигации в правом верхнем углу экрана](start-mvc/_static/2.png)

<span data-ttu-id="f910f-186">При запуске в режиме отладки нажмите клавиши **SHIFT+F5**, чтобы остановить отладку.</span><span class="sxs-lookup"><span data-stu-id="f910f-186">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="f910f-187">В следующей части этого учебника мы поговорим об MVC и приступим к написанию кода.</span><span class="sxs-lookup"><span data-stu-id="f910f-187">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="f910f-188">Вперед</span><span class="sxs-lookup"><span data-stu-id="f910f-188">Next</span></span>](adding-controller.md)  
