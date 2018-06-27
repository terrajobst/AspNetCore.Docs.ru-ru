---
title: Начало работы с Razor Pages в ASP.NET Core
author: rick-anderson
description: Основные сведения о создании веб-приложении Razor Pages в ASP.NET Core. Razor Pages рекомендуется для рабочих веб-нагрузок в ASP.NET Core.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: e317b49f2ad33c392de33bc32a87f67bb8cb72a0
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278048"
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="f72b7-104">Начало работы с Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f72b7-104">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="f72b7-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="f72b7-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="f72b7-106">Мы рекомендуем руководствоваться версией этого учебника для ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="f72b7-106">We recommend you follow the ASP.NET Core 2.1 version of this tutorial.</span></span> <span data-ttu-id="f72b7-107">Она **гораздо** проще и рассматривает больше возможностей.</span><span class="sxs-lookup"><span data-stu-id="f72b7-107">It's **much** easier to follow and covers more features.</span></span> <span data-ttu-id="f72b7-108">Выберите **ASP.NET Core 2.1** в селекторе версий.</span><span class="sxs-lookup"><span data-stu-id="f72b7-108">Select **ASP.NET Core 2.1** in the version selector.</span></span>

![Селектор версий в оглавлении](razor-pages-start/_static/v21.png)

::: moniker-end

<span data-ttu-id="f72b7-110">В этом учебнике приводятся основные сведения о веб-приложении Razor Pages в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f72b7-110">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="f72b7-111">Razor Pages — это рекомендуемый способ создания пользовательского интерфейса для веб-приложений в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f72b7-111">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="f72b7-112">Существуют три версии этого руководства:</span><span class="sxs-lookup"><span data-stu-id="f72b7-112">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="f72b7-113">Windows: это руководство.</span><span class="sxs-lookup"><span data-stu-id="f72b7-113">Windows: This tutorial</span></span>
* <span data-ttu-id="f72b7-114">MacOS: [Начало работы с Razor Pages в Visual Studio для Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="f72b7-114">MacOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="f72b7-115">macOS, Linux и Windows: [Начало работы с Razor Pages в ASP.NET Core с Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="f72b7-115">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="f72b7-116">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f72b7-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a><span data-ttu-id="f72b7-117">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="f72b7-117">Prerequisites</span></span>

<span data-ttu-id="f72b7-118">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="f72b7-118">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="f72b7-119">Создание веб-приложения Razor</span><span class="sxs-lookup"><span data-stu-id="f72b7-119">Create a Razor web app</span></span>

* <span data-ttu-id="f72b7-120">В меню **Файл** Visual Studio откройте **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="f72b7-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f72b7-121">Создайте новое веб-приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f72b7-121">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="f72b7-122">Назовите проект **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="f72b7-122">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="f72b7-123">Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.</span><span class="sxs-lookup"><span data-stu-id="f72b7-123">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="f72b7-124">![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="f72b7-124">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="f72b7-125">Выберите в раскрывающемся списке **ASP.NET Core 2.1**, а затем **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="f72b7-125">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

 ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2_2.1.png)

<span data-ttu-id="f72b7-127">Шаблон Visual Studio создает начальный проект:</span><span class="sxs-lookup"><span data-stu-id="f72b7-127">The Visual Studio template creates a starter project:</span></span>

![обозреватель решений](razor-pages-start/_static/se2.1.png)

<span data-ttu-id="f72b7-129">Нажмите клавишу **F5**, чтобы запустить приложение в режиме отладки, или **CTRL-F5**, чтобы запустить его без подключения отладчика.</span><span class="sxs-lookup"><span data-stu-id="f72b7-129">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger.</span></span> <span data-ttu-id="f72b7-130">Нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="f72b7-130">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="f72b7-131">Это приложение не отслеживает персональные данные.</span><span class="sxs-lookup"><span data-stu-id="f72b7-131">This app doesn't track personal information.</span></span> <span data-ttu-id="f72b7-132">Созданный шаблоном код включает ресурсы для соблюдения [Общего регламента по защите данных (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="f72b7-132">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR.png)

<span data-ttu-id="f72b7-134">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="f72b7-134">The following image shows the app after accepting tracking:</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/home2.1.png)

* <span data-ttu-id="f72b7-136">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение.</span><span class="sxs-lookup"><span data-stu-id="f72b7-136">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="f72b7-137">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="f72b7-137">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="f72b7-138">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="f72b7-138">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="f72b7-139">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="f72b7-139">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="f72b7-140">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="f72b7-140">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="f72b7-141">На представленном выше снимке экрана используется номер порта 5000.</span><span class="sxs-lookup"><span data-stu-id="f72b7-141">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="f72b7-142">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="f72b7-142">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="f72b7-143">Запуск приложения с помощью клавиш **CTRL+F5** (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="f72b7-143">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="f72b7-144">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.</span><span class="sxs-lookup"><span data-stu-id="f72b7-144">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a><span data-ttu-id="f72b7-145">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="f72b7-145">Prerequisites</span></span>

<span data-ttu-id="f72b7-146">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="f72b7-146">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="f72b7-147">Создание веб-приложения Razor</span><span class="sxs-lookup"><span data-stu-id="f72b7-147">Create a Razor web app</span></span>

* <span data-ttu-id="f72b7-148">В меню **Файл** Visual Studio откройте **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="f72b7-148">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f72b7-149">Создайте новое веб-приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f72b7-149">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="f72b7-150">Назовите проект **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="f72b7-150">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="f72b7-151">Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.</span><span class="sxs-lookup"><span data-stu-id="f72b7-151">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="f72b7-152">![Новое веб-приложение ASP.NET Core](../../razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="f72b7-152">![new ASP.NET Core Web Application](../../razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="f72b7-153">Выберите в раскрывающемся списке **ASP.NET Core 2.0**, а затем **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="f72b7-153">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="f72b7-154">Шаблон Visual Studio создает начальный проект:</span><span class="sxs-lookup"><span data-stu-id="f72b7-154">The Visual Studio template creates a starter project:</span></span>

![обозреватель решений](razor-pages-start/_static/se.png)

<span data-ttu-id="f72b7-156">Нажмите клавишу **F5**, чтобы запустить приложение в режиме отладки, или **CTRL-F5**, чтобы запустить его без подключения отладчика.</span><span class="sxs-lookup"><span data-stu-id="f72b7-156">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/home.png)

* <span data-ttu-id="f72b7-158">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем ваше приложение.</span><span class="sxs-lookup"><span data-stu-id="f72b7-158">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="f72b7-159">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="f72b7-159">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="f72b7-160">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="f72b7-160">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="f72b7-161">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="f72b7-161">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="f72b7-162">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="f72b7-162">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="f72b7-163">На представленном выше снимке экрана используется номер порта 5000.</span><span class="sxs-lookup"><span data-stu-id="f72b7-163">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="f72b7-164">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="f72b7-164">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="f72b7-165">Запуск приложения с помощью клавиш **CTRL+F5** (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="f72b7-165">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="f72b7-166">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.</span><span class="sxs-lookup"><span data-stu-id="f72b7-166">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="f72b7-167">Далее: добавление модели</span><span class="sxs-lookup"><span data-stu-id="f72b7-167">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
