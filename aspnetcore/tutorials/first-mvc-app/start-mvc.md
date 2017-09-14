---
title: "Начало работы с MVC ASP.NET Core и Visual Studio"
author: rick-anderson
description: "Начало работы с MVC ASP.NET Core и Visual Studio"
keywords: ASP.NET Core, MVC
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 9636e0e51e506d294ffb50a21165195c9d04fe20
ms.sourcegitcommit: bd05f7ea8f87ad076ef6e8b704698ebcba5ca80c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/25/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="21ae5-104">Начало работы с MVC ASP.NET Core и Visual Studio</span><span class="sxs-lookup"><span data-stu-id="21ae5-104">Getting started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="21ae5-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="21ae5-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="21ae5-106">В этом учебнике представлены основы сборки веб-приложений MVC ASP.NET Core с помощью [Visual Studio 2017](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="21ae5-106">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio 2017](https://www.visualstudio.com/).</span></span> [!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="21ae5-107">Существует 3 версии этого учебника.</span><span class="sxs-lookup"><span data-stu-id="21ae5-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="21ae5-108">macOS: [Создание приложения ASP.NET Core MVC с помощью Visual Studio для Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="21ae5-108">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="21ae5-109">Windows: [Создание приложения ASP.NET Core MVC с помощью Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="21ae5-109">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="21ae5-110">macOS, Linux и Windows: [Создание приложения MVC ASP.NET Core с помощью Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="21ae5-110">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

<span data-ttu-id="21ae5-111">Версию этого учебника для Visual Studio 2015 см. в статье [документации по ASP.NET Core для VS 2015 в формате PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf).</span><span class="sxs-lookup"><span data-stu-id="21ae5-111">For the Visual Studio 2015 version of this tutorial, see the [VS 2015 version of ASP.NET Core documentation in PDF format](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf).</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="21ae5-112">Установка Visual Studio и .NET Core</span><span class="sxs-lookup"><span data-stu-id="21ae5-112">Install Visual Studio and .NET Core</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="21ae5-113">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="21ae5-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="21ae5-114">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="21ae5-114">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="21ae5-115">Установите Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="21ae5-115">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="21ae5-116">Выберите загрузку сообщества.</span><span class="sxs-lookup"><span data-stu-id="21ae5-116">Select the Community download.</span></span> <span data-ttu-id="21ae5-117">Если платформа Visual Studio 2017 уже установлена, пропустите этот шаг.</span><span class="sxs-lookup"><span data-stu-id="21ae5-117">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="21ae5-118">Установщик главной страницы Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="21ae5-118">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/visual-studio-homepage-vs.aspx)

<span data-ttu-id="21ae5-119">Запустите программу установки и выберите следующие рабочие нагрузки.</span><span class="sxs-lookup"><span data-stu-id="21ae5-119">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="21ae5-120">**ASP.NET и веб-разработка** в разделе **Интернет и облако**)</span><span class="sxs-lookup"><span data-stu-id="21ae5-120">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="21ae5-121">**Кроссплатформенная разработка .NET Core** (в разделе **Другие наборы инструментов**)</span><span class="sxs-lookup"><span data-stu-id="21ae5-121">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**ASP.NET и веб-разработка** в разделе **Интернет и облако**)](start-mvc/_static/web_workload.png)

![**Кроссплатформенная разработка .NET Core** (в разделе **Другие наборы инструментов**)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="21ae5-124">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="21ae5-124">Create a web app</span></span>

<span data-ttu-id="21ae5-125">В Visual Studio выберите меню **Файл > Создать > Проект**.</span><span class="sxs-lookup"><span data-stu-id="21ae5-125">From Visual Studio, select  **File > New > Project**.</span></span>

![Файл > Создать > Проект](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="21ae5-127">В диалоговом окне **Создание проекта** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="21ae5-127">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="21ae5-128">В левой области выберите **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="21ae5-128">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="21ae5-129">В центральной области выберите **Веб-приложение ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="21ae5-129">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="21ae5-130">Назовите проект "MvcMovie" (имя "MvcMovie" необходимо присвоить для того, чтобы при копировании кода пространства имен совпали).</span><span class="sxs-lookup"><span data-stu-id="21ae5-130">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="21ae5-131">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="21ae5-131">Tap **OK**</span></span>

![<span data-ttu-id="21ae5-132">Диалоговое окно нового проекта, .NET Core в левой области, веб-узел ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="21ae5-132">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="21ae5-133">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="21ae5-133">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="21ae5-134">Заполните данные в диалоговом окне **Создание веб-приложения ASP.NET Core (.NET Core) — MvcMovie**.</span><span class="sxs-lookup"><span data-stu-id="21ae5-134">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="21ae5-135">Выберите в раскрывающемся списке выбора версии **ASP.NET Core 2.-**.</span><span class="sxs-lookup"><span data-stu-id="21ae5-135">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="21ae5-136">Выберите **Веб-приложение (модель-представление-контроллер)**.</span><span class="sxs-lookup"><span data-stu-id="21ae5-136">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="21ae5-137">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="21ae5-137">Tap **OK**.</span></span>

![<span data-ttu-id="21ae5-138">Диалоговое окно нового проекта, .NET Core в левой области, веб-узел ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="21ae5-138">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="21ae5-139">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="21ae5-139">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="21ae5-140">Заполните данные в диалоговом окне **Создание веб-приложения ASP.NET Core (.NET Core) — MvcMovie**.</span><span class="sxs-lookup"><span data-stu-id="21ae5-140">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="21ae5-141">Выберите в раскрывающемся списке выбора версии **ASP.NET Core 1.1**.</span><span class="sxs-lookup"><span data-stu-id="21ae5-141">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="21ae5-142">Выберите **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="21ae5-142">Tap **Web Application**</span></span>
* <span data-ttu-id="21ae5-143">Сохраните значение по умолчанию **Без проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="21ae5-143">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="21ae5-144">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="21ae5-144">Tap **OK**.</span></span>

![Новое веб-приложение ASP.NET Core](start-mvc/_static/p3.png)

---

<span data-ttu-id="21ae5-146">В Visual Studio используется только что созданный вами шаблон по умолчанию для проекта MVC.</span><span class="sxs-lookup"><span data-stu-id="21ae5-146">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="21ae5-147">Чтобы приложение стало рабочим, осталось только указать имя проекта и выбрать несколько параметров.</span><span class="sxs-lookup"><span data-stu-id="21ae5-147">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="21ae5-148">Это простой начальный проект и хорошая стартовая точка.</span><span class="sxs-lookup"><span data-stu-id="21ae5-148">This is a simple starter project, and it's a good place to start,</span></span>

<span data-ttu-id="21ae5-149">Нажмите клавишу **F5**, чтобы запустить приложение в режиме отладки, или клавиши **CTRL-F5**, чтобы выбрать режим без отладки.</span><span class="sxs-lookup"><span data-stu-id="21ae5-149">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![Запуск приложения](start-mvc/_static/1.png)

* <span data-ttu-id="21ae5-151">Visual Studio запускает [IIS Express](http://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview), а затем ваше приложение.</span><span class="sxs-lookup"><span data-stu-id="21ae5-151">Visual Studio starts [IIS Express](http://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="21ae5-152">Обратите внимание на то, что в адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="21ae5-152">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="21ae5-153">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="21ae5-153">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="21ae5-154">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="21ae5-154">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="21ae5-155">На представленном выше снимке экрана используется порт номер 5000.</span><span class="sxs-lookup"><span data-stu-id="21ae5-155">In the image above, the port number is 5000.</span></span> <span data-ttu-id="21ae5-156">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="21ae5-156">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="21ae5-157">Запуск приложения с помощью клавиш **CTRL+F5** (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="21ae5-157">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="21ae5-158">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.</span><span class="sxs-lookup"><span data-stu-id="21ae5-158">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="21ae5-159">Из меню **Отладка** можно запустить приложение в режиме с отладкой или без.</span><span class="sxs-lookup"><span data-stu-id="21ae5-159">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Меню отладки](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="21ae5-161">Чтобы выполнить отладку приложения, нажмите кнопку **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="21ae5-161">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="21ae5-163">Шаблон по умолчанию включает рабочие ссылки **Домой, О программе** и **Контакты**.</span><span class="sxs-lookup"><span data-stu-id="21ae5-163">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="21ae5-164">На представленном выше снимке экрана эти ссылки не отображаются.</span><span class="sxs-lookup"><span data-stu-id="21ae5-164">The browser image above doesn't show these links.</span></span> <span data-ttu-id="21ae5-165">Если размер окна вашего браузера слишком мал, нажмите значок навигации, чтобы отобразить эти ссылки.</span><span class="sxs-lookup"><span data-stu-id="21ae5-165">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Значок навигации в правом верхнем углу экрана](start-mvc/_static/2.png)

<span data-ttu-id="21ae5-167">При запуске в режиме отладки нажмите клавиши **SHIFT+F5**, чтобы остановить отладку.</span><span class="sxs-lookup"><span data-stu-id="21ae5-167">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="21ae5-168">В следующей части этого учебника мы поговорим об MVC и приступим к написанию кода.</span><span class="sxs-lookup"><span data-stu-id="21ae5-168">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="21ae5-169">Вперед</span><span class="sxs-lookup"><span data-stu-id="21ae5-169">Next</span></span>](adding-controller.md)  
