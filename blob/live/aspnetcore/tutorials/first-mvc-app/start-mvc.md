---
title: "Начало работы с MVC ASP.NET Core и Visual Studio"
author: rick-anderson
description: "Начало работы с MVC ASP.NET Core и Visual Studio"
keywords: ASP.NET Core, MVC
ms.author: riande
manager: wpickett
ms.date: 10/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 4b86eb22e367d2305b7995421aec6f37008c5637
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="ca2ac-104">Начало работы с MVC ASP.NET Core и Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ca2ac-104">Getting started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="ca2ac-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="ca2ac-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="ca2ac-106">Существует 3 версии этого учебника:</span><span class="sxs-lookup"><span data-stu-id="ca2ac-106">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="ca2ac-107">macOS: [Создание приложения ASP.NET Core MVC с помощью Visual Studio для Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="ca2ac-107">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="ca2ac-108">Windows: [Создание приложения ASP.NET Core MVC с помощью Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="ca2ac-108">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="ca2ac-109">macOS, Linux и Windows: [Создание приложения MVC ASP.NET Core с помощью Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="ca2ac-109">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="ca2ac-110">Установка Visual Studio и .NET Core</span><span class="sxs-lookup"><span data-stu-id="ca2ac-110">Install Visual Studio and .NET Core</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ca2ac-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ca2ac-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ca2ac-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ca2ac-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ca2ac-113">Установите Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-113">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="ca2ac-114">Выберите загрузку сообщества.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-114">Select the Community download.</span></span> <span data-ttu-id="ca2ac-115">Если платформа Visual Studio 2017 уже установлена, пропустите этот шаг.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-115">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="ca2ac-116">Установщик главной страницы Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="ca2ac-116">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="ca2ac-117">Запустите программу установки и выберите следующие рабочие нагрузки.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-117">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="ca2ac-118">**ASP.NET и веб-разработка** в разделе **Интернет и облако**)</span><span class="sxs-lookup"><span data-stu-id="ca2ac-118">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="ca2ac-119">**Кроссплатформенная разработка .NET Core** (в разделе **Другие наборы инструментов**)</span><span class="sxs-lookup"><span data-stu-id="ca2ac-119">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**ASP.NET и веб-разработка** в разделе **Интернет и облако**)](start-mvc/_static/web_workload.png)

![**Кроссплатформенная разработка .NET Core** (в разделе **Другие наборы инструментов**)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="ca2ac-122">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="ca2ac-122">Create a web app</span></span>

<span data-ttu-id="ca2ac-123">В Visual Studio выберите меню **Файл > Создать > Проект**.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-123">From Visual Studio, select  **File > New > Project**.</span></span>

![Файл > Создать > Проект](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="ca2ac-125">В диалоговом окне **Создание проекта** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-125">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="ca2ac-126">В левой области выберите **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-126">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="ca2ac-127">В центральной области выберите **Веб-приложение ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-127">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="ca2ac-128">Назовите проект "MvcMovie" (имя "MvcMovie" необходимо присвоить для того, чтобы при копировании кода пространства имен совпали).</span><span class="sxs-lookup"><span data-stu-id="ca2ac-128">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="ca2ac-129">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-129">Tap **OK**</span></span>

![<span data-ttu-id="ca2ac-130">Диалоговое окно нового проекта, .NET Core в левой области, веб-узел ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ca2ac-130">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ca2ac-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ca2ac-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ca2ac-132">Заполните данные в диалоговом окне **Создание веб-приложения ASP.NET Core (.NET Core) — MvcMovie**.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-132">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="ca2ac-133">Выберите в раскрывающемся списке выбора версии **ASP.NET Core 2.-**.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-133">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="ca2ac-134">Выберите **Веб-приложение (модель-представление-контроллер)**.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-134">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="ca2ac-135">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-135">Tap **OK**.</span></span>

![<span data-ttu-id="ca2ac-136">Диалоговое окно нового проекта, .NET Core в левой области, веб-узел ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ca2ac-136">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ca2ac-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ca2ac-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ca2ac-138">Заполните данные в диалоговом окне **Создание веб-приложения ASP.NET Core (.NET Core) — MvcMovie**.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-138">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="ca2ac-139">Выберите в раскрывающемся списке выбора версии **ASP.NET Core 1.1**.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-139">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="ca2ac-140">Выберите **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-140">Tap **Web Application**</span></span>
* <span data-ttu-id="ca2ac-141">Сохраните значение по умолчанию **Без проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-141">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="ca2ac-142">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-142">Tap **OK**.</span></span>

![Новое веб-приложение ASP.NET Core](start-mvc/_static/p3.png)

---

<span data-ttu-id="ca2ac-144">В Visual Studio используется только что созданный вами шаблон по умолчанию для проекта MVC.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-144">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="ca2ac-145">Чтобы приложение стало рабочим, осталось только указать имя проекта и выбрать несколько параметров.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-145">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="ca2ac-146">Это простой начальный проект и хорошая стартовая точка.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-146">This is a simple starter project, and it's a good place to start,</span></span>

<span data-ttu-id="ca2ac-147">Нажмите клавишу **F5**, чтобы запустить приложение в режиме отладки, или клавиши **CTRL-F5**, чтобы выбрать режим без отладки.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-147">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![Запуск приложения](start-mvc/_static/1.png)

* <span data-ttu-id="ca2ac-149">Visual Studio запускает [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем ваше приложение.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-149">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="ca2ac-150">Обратите внимание на то, что в адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-150">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ca2ac-151">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-151">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="ca2ac-152">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-152">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="ca2ac-153">На представленном выше снимке экрана используется порт номер 5000.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-153">In the image above, the port number is 5000.</span></span> <span data-ttu-id="ca2ac-154">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-154">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="ca2ac-155">Запуск приложения с помощью клавиш **CTRL+F5** (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-155">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="ca2ac-156">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-156">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="ca2ac-157">Из меню **Отладка** можно запустить приложение в режиме с отладкой или без.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-157">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Меню отладки](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="ca2ac-159">Чтобы выполнить отладку приложения, нажмите кнопку **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-159">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="ca2ac-161">Шаблон по умолчанию включает рабочие ссылки **Домой, О программе** и **Контакты**.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-161">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="ca2ac-162">На представленном выше снимке экрана эти ссылки не отображаются.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-162">The browser image above doesn't show these links.</span></span> <span data-ttu-id="ca2ac-163">Если размер окна вашего браузера слишком мал, нажмите значок навигации, чтобы отобразить эти ссылки.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-163">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Значок навигации в правом верхнем углу экрана](start-mvc/_static/2.png)

<span data-ttu-id="ca2ac-165">При запуске в режиме отладки нажмите клавиши **SHIFT+F5**, чтобы остановить отладку.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-165">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="ca2ac-166">В следующей части этого учебника мы поговорим об MVC и приступим к написанию кода.</span><span class="sxs-lookup"><span data-stu-id="ca2ac-166">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="ca2ac-167">Вперед</span><span class="sxs-lookup"><span data-stu-id="ca2ac-167">Next</span></span>](adding-controller.md)  
