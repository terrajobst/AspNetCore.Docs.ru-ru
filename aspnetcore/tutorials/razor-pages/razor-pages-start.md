---
title: Начало работы с Razor Pages в ASP.NET Core
author: rick-anderson
description: Основные сведения о создании веб-приложении Razor Pages в ASP.NET Core. Razor Pages рекомендуется для рабочих веб-нагрузок в ASP.NET Core.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 7148f2d944bd1978b1a83278dfed9051f192e4dd
ms.sourcegitcommit: 08f1a9baa97060da5168840b332c9c0805b5f901
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2018
ms.locfileid: "37144941"
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="ade7c-104">Начало работы с Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ade7c-104">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="ade7c-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="ade7c-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="ade7c-106">Мы рекомендуем руководствоваться версией этого учебника для ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="ade7c-106">We recommend you follow the ASP.NET Core 2.1 version of this tutorial.</span></span> <span data-ttu-id="ade7c-107">Она **гораздо** проще и рассматривает больше возможностей.</span><span class="sxs-lookup"><span data-stu-id="ade7c-107">It's **much** easier to follow and covers more features.</span></span> <span data-ttu-id="ade7c-108">Выберите **ASP.NET Core 2.1** в селекторе версий.</span><span class="sxs-lookup"><span data-stu-id="ade7c-108">Select **ASP.NET Core 2.1** in the version selector.</span></span>

![Селектор версий в оглавлении](razor-pages-start/_static/v21.png)

::: moniker-end

<span data-ttu-id="ade7c-110">В этом учебнике приводятся основные сведения о веб-приложении Razor Pages в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ade7c-110">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="ade7c-111">Razor Pages — это рекомендуемый способ создания пользовательского интерфейса для веб-приложений в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ade7c-111">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="ade7c-112">Существуют три версии этого руководства:</span><span class="sxs-lookup"><span data-stu-id="ade7c-112">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="ade7c-113">Windows: это руководство.</span><span class="sxs-lookup"><span data-stu-id="ade7c-113">Windows: This tutorial</span></span>
* <span data-ttu-id="ade7c-114">MacOS: [Начало работы с Razor Pages в Visual Studio для Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="ade7c-114">MacOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="ade7c-115">macOS, Linux и Windows: [Начало работы с Razor Pages в ASP.NET Core с Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="ade7c-115">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="ade7c-116">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ade7c-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a><span data-ttu-id="ade7c-117">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="ade7c-117">Prerequisites</span></span>

<span data-ttu-id="ade7c-118">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="ade7c-118">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="ade7c-119">Создание веб-приложения Razor</span><span class="sxs-lookup"><span data-stu-id="ade7c-119">Create a Razor web app</span></span>

* <span data-ttu-id="ade7c-120">В меню **Файл** Visual Studio откройте **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="ade7c-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="ade7c-121">Создайте новое веб-приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ade7c-121">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="ade7c-122">Назовите проект **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="ade7c-122">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="ade7c-123">Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.</span><span class="sxs-lookup"><span data-stu-id="ade7c-123">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="ade7c-124">![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="ade7c-124">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="ade7c-125">Выберите в раскрывающемся списке **ASP.NET Core 2.1**, а затем **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="ade7c-125">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

 ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2_2.1.png)

<span data-ttu-id="ade7c-127">Шаблон Visual Studio создает начальный проект:</span><span class="sxs-lookup"><span data-stu-id="ade7c-127">The Visual Studio template creates a starter project:</span></span>

![обозреватель решений](razor-pages-start/_static/se2.1.png)

<span data-ttu-id="ade7c-129">Нажмите клавишу **F5**, чтобы запустить приложение в режиме отладки, или **CTRL-F5**, чтобы запустить его без подключения отладчика.</span><span class="sxs-lookup"><span data-stu-id="ade7c-129">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger.</span></span> <span data-ttu-id="ade7c-130">Нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="ade7c-130">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="ade7c-131">Это приложение не отслеживает персональные данные.</span><span class="sxs-lookup"><span data-stu-id="ade7c-131">This app doesn't track personal information.</span></span> <span data-ttu-id="ade7c-132">Созданный шаблоном код включает ресурсы для соблюдения [Общего регламента по защите данных (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="ade7c-132">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR.png)

<span data-ttu-id="ade7c-134">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="ade7c-134">The following image shows the app after accepting tracking:</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/home2.1.png)

* <span data-ttu-id="ade7c-136">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение.</span><span class="sxs-lookup"><span data-stu-id="ade7c-136">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="ade7c-137">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="ade7c-137">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ade7c-138">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="ade7c-138">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="ade7c-139">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="ade7c-139">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="ade7c-140">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="ade7c-140">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="ade7c-141">На представленном выше снимке экрана используется номер порта 5000.</span><span class="sxs-lookup"><span data-stu-id="ade7c-141">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="ade7c-142">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="ade7c-142">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="ade7c-143">Запуск приложения с помощью клавиш **CTRL+F5** (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="ade7c-143">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="ade7c-144">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.</span><span class="sxs-lookup"><span data-stu-id="ade7c-144">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a><span data-ttu-id="ade7c-145">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="ade7c-145">Prerequisites</span></span>

<span data-ttu-id="ade7c-146">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="ade7c-146">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="ade7c-147">Создание веб-приложения Razor</span><span class="sxs-lookup"><span data-stu-id="ade7c-147">Create a Razor web app</span></span>

* <span data-ttu-id="ade7c-148">В меню **Файл** Visual Studio откройте **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="ade7c-148">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="ade7c-149">Создайте новое веб-приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ade7c-149">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="ade7c-150">Назовите проект **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="ade7c-150">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="ade7c-151">Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.</span><span class="sxs-lookup"><span data-stu-id="ade7c-151">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="ade7c-152">![Новое веб-приложение ASP.NET Core](../../razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="ade7c-152">![new ASP.NET Core Web Application](../../razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="ade7c-153">Выберите в раскрывающемся списке **ASP.NET Core 2.0**, а затем **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="ade7c-153">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="ade7c-154">Шаблон Visual Studio создает начальный проект:</span><span class="sxs-lookup"><span data-stu-id="ade7c-154">The Visual Studio template creates a starter project:</span></span>

![обозреватель решений](razor-pages-start/_static/se.png)

<span data-ttu-id="ade7c-156">Нажмите клавишу **F5**, чтобы запустить приложение в режиме отладки, или **CTRL-F5**, чтобы запустить его без подключения отладчика.</span><span class="sxs-lookup"><span data-stu-id="ade7c-156">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/home.png)

* <span data-ttu-id="ade7c-158">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем ваше приложение.</span><span class="sxs-lookup"><span data-stu-id="ade7c-158">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="ade7c-159">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="ade7c-159">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ade7c-160">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="ade7c-160">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="ade7c-161">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="ade7c-161">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="ade7c-162">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="ade7c-162">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="ade7c-163">На представленном выше снимке экрана используется номер порта 5000.</span><span class="sxs-lookup"><span data-stu-id="ade7c-163">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="ade7c-164">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="ade7c-164">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="ade7c-165">Запуск приложения с помощью клавиш **CTRL+F5** (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="ade7c-165">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="ade7c-166">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.</span><span class="sxs-lookup"><span data-stu-id="ade7c-166">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="ade7c-167">Далее: добавление модели</span><span class="sxs-lookup"><span data-stu-id="ade7c-167">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
