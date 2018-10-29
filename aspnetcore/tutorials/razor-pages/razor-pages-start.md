---
title: Начало работы с Razor Pages в ASP.NET Core
author: rick-anderson
description: В этой серии руководств объясняется, как использовать Razor Pages в ASP.NET Core. Узнайте, как создать модель, сгенерировать код для Razor Pages, использовать Entity Framework Core и SQL Server для доступа к данным, добавлять функции поиска и проверки ввода, а также использовать возможность миграции для обновления модели.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 4ee9a014114e2536f7584b2a1ff9d699fb971aa8
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206982"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="0c2fc-104">Руководство. Начало работы с Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0c2fc-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="0c2fc-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="0c2fc-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="0c2fc-106">Мы рекомендуем руководствоваться версией этого учебника для ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-106">We recommend you follow the ASP.NET Core 2.1 version of this tutorial.</span></span> <span data-ttu-id="0c2fc-107">Она **гораздо** проще и рассматривает больше возможностей.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-107">It's **much** easier to follow and covers more features.</span></span> <span data-ttu-id="0c2fc-108">Выберите **ASP.NET Core 2.1** в селекторе версий.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-108">Select **ASP.NET Core 2.1** in the version selector.</span></span>

![Селектор версий в оглавлении](razor-pages-start/_static/v21.png)

::: moniker-end

<span data-ttu-id="0c2fc-110">В этом учебнике приводятся основные сведения о веб-приложении Razor Pages в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-110">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="0c2fc-111">Razor Pages — это рекомендуемый способ создания пользовательского интерфейса для веб-приложений в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-111">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="0c2fc-112">Существуют три версии этого руководства:</span><span class="sxs-lookup"><span data-stu-id="0c2fc-112">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="0c2fc-113">Windows: это руководство.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-113">Windows: This tutorial</span></span>
* <span data-ttu-id="0c2fc-114">MacOS: [Начало работы с Razor Pages в Visual Studio для Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="0c2fc-114">MacOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="0c2fc-115">macOS, Linux и Windows: [Начало работы с Razor Pages в ASP.NET Core с Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="0c2fc-115">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="0c2fc-116">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0c2fc-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a><span data-ttu-id="0c2fc-117">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="0c2fc-117">Prerequisites</span></span>

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="0c2fc-118">Создание веб-приложения Razor</span><span class="sxs-lookup"><span data-stu-id="0c2fc-118">Create a Razor web app</span></span>

* <span data-ttu-id="0c2fc-119">В меню **Файл** Visual Studio откройте **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-119">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="0c2fc-120">Создайте новое веб-приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-120">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="0c2fc-121">Назовите проект **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-121">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="0c2fc-122">Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-122">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="0c2fc-123">![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="0c2fc-123">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="0c2fc-124">Выберите в раскрывающемся списке **ASP.NET Core 2.1**, а затем **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-124">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

 ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2_2.1.png)

<span data-ttu-id="0c2fc-126">Шаблон Visual Studio создает начальный проект:</span><span class="sxs-lookup"><span data-stu-id="0c2fc-126">The Visual Studio template creates a starter project:</span></span>

![обозреватель решений](razor-pages-start/_static/se2.1.png)

<span data-ttu-id="0c2fc-128">Нажмите клавишу **F5**, чтобы запустить приложение в режиме отладки, или **CTRL-F5**, чтобы запустить его без подключения отладчика.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-128">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger.</span></span> <span data-ttu-id="0c2fc-129">Нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-129">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="0c2fc-130">Это приложение не отслеживает персональные данные.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-130">This app doesn't track personal information.</span></span> <span data-ttu-id="0c2fc-131">Созданный шаблоном код включает ресурсы для соблюдения [Общего регламента по защите данных (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="0c2fc-131">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR.png)

<span data-ttu-id="0c2fc-133">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="0c2fc-133">The following image shows the app after accepting tracking:</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/home2.1.png)

* <span data-ttu-id="0c2fc-135">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-135">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="0c2fc-136">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-136">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="0c2fc-137">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-137">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="0c2fc-138">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-138">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="0c2fc-139">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-139">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="0c2fc-140">На представленном выше снимке экрана используется номер порта 5001.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-140">In the preceding image, the port number is 5001.</span></span> <span data-ttu-id="0c2fc-141">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-141">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="0c2fc-142">Запуск приложения с помощью клавиш **CTRL+F5** (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-142">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="0c2fc-143">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-143">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

[!INCLUDE [F7](~/includes/RP/F7.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a><span data-ttu-id="0c2fc-144">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="0c2fc-144">Prerequisites</span></span>

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="0c2fc-145">Создание веб-приложения Razor</span><span class="sxs-lookup"><span data-stu-id="0c2fc-145">Create a Razor web app</span></span>

* <span data-ttu-id="0c2fc-146">В меню **Файл** Visual Studio откройте **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-146">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="0c2fc-147">Создайте новое веб-приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-147">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="0c2fc-148">Назовите проект **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-148">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="0c2fc-149">Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-149">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="0c2fc-150">![Новое веб-приложение ASP.NET Core](../../razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="0c2fc-150">![new ASP.NET Core Web Application](../../razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="0c2fc-151">Выберите в раскрывающемся списке **ASP.NET Core 2.0**, а затем **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-151">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="0c2fc-152">Шаблон Visual Studio создает начальный проект:</span><span class="sxs-lookup"><span data-stu-id="0c2fc-152">The Visual Studio template creates a starter project:</span></span>

![обозреватель решений](razor-pages-start/_static/se.png)

<span data-ttu-id="0c2fc-154">Нажмите клавишу **F5**, чтобы запустить приложение в режиме отладки, или **CTRL-F5**, чтобы запустить его без подключения отладчика.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-154">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/home.png)

* <span data-ttu-id="0c2fc-156">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем ваше приложение.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-156">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="0c2fc-157">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-157">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="0c2fc-158">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-158">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="0c2fc-159">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-159">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="0c2fc-160">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-160">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="0c2fc-161">На представленном выше снимке экрана используется номер порта 5000.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-161">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="0c2fc-162">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-162">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="0c2fc-163">Запуск приложения с помощью клавиш **CTRL+F5** (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-163">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="0c2fc-164">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.</span><span class="sxs-lookup"><span data-stu-id="0c2fc-164">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="0c2fc-165">Далее: добавление модели</span><span class="sxs-lookup"><span data-stu-id="0c2fc-165">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
