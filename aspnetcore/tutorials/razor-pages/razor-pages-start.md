---
title: Начало работы с Razor Pages в ASP.NET Core
author: rick-anderson
description: В этой серии руководств объясняется, как использовать Razor Pages в ASP.NET Core. Узнайте, как создать модель, сгенерировать код для Razor Pages, использовать Entity Framework Core и SQL Server для доступа к данным, добавлять функции поиска и проверки ввода, а также использовать возможность миграции для обновления модели.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: ca5b134aaa0a9218bc3b0466c3448bd41e8cef8b
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450740"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="10004-104">Руководство. Начало работы с Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="10004-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="10004-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="10004-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="10004-106">Мы рекомендуем руководствоваться версией этого учебника для ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="10004-106">We recommend you follow the ASP.NET Core 2.1 version of this tutorial.</span></span> <span data-ttu-id="10004-107">Она **гораздо** проще и рассматривает больше возможностей.</span><span class="sxs-lookup"><span data-stu-id="10004-107">It's **much** easier to follow and covers more features.</span></span> <span data-ttu-id="10004-108">Выберите **ASP.NET Core 2.1** в селекторе версий.</span><span class="sxs-lookup"><span data-stu-id="10004-108">Select **ASP.NET Core 2.1** in the version selector.</span></span>

![Селектор версий в оглавлении](razor-pages-start/_static/v21.png)

::: moniker-end

<span data-ttu-id="10004-110">В этом учебнике приводятся основные сведения о веб-приложении Razor Pages в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="10004-110">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="10004-111">Razor Pages — это рекомендуемый способ создания пользовательского интерфейса для веб-приложений в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="10004-111">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="10004-112">Существуют три версии этого руководства:</span><span class="sxs-lookup"><span data-stu-id="10004-112">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="10004-113">Windows: это руководство.</span><span class="sxs-lookup"><span data-stu-id="10004-113">Windows: This tutorial</span></span>
* <span data-ttu-id="10004-114">MacOS: [Начало работы с Razor Pages в Visual Studio для Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="10004-114">macOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="10004-115">macOS, Linux и Windows: [Начало работы с Razor Pages в ASP.NET Core с Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="10004-115">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="10004-116">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="10004-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

> [!NOTE]
> <span data-ttu-id="10004-117">Мы тестируем удобство использования новой предлагаемой структуры оглавления для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="10004-117">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="10004-118">Если у вас есть несколько минут, и вы хотите попробовать найти семь разных тем в существующей или планируемой структуре оглавления, [щелкните здесь, чтобы принять участие в исследовании](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span><span class="sxs-lookup"><span data-stu-id="10004-118">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a><span data-ttu-id="10004-119">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="10004-119">Prerequisites</span></span>

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="10004-120">Создание веб-приложения Razor</span><span class="sxs-lookup"><span data-stu-id="10004-120">Create a Razor web app</span></span>

* <span data-ttu-id="10004-121">В меню **Файл** Visual Studio откройте **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="10004-121">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="10004-122">Создайте новое веб-приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="10004-122">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="10004-123">Назовите проект **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="10004-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="10004-124">Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.</span><span class="sxs-lookup"><span data-stu-id="10004-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="10004-125">![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="10004-125">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="10004-126">Выберите в раскрывающемся списке **ASP.NET Core 2.1**, а затем **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="10004-126">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

 ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2_2.1.png)

<span data-ttu-id="10004-128">Шаблон Visual Studio создает начальный проект:</span><span class="sxs-lookup"><span data-stu-id="10004-128">The Visual Studio template creates a starter project:</span></span>

![обозреватель решений](razor-pages-start/_static/se2.1.png)

<span data-ttu-id="10004-130">Нажмите клавишу **F5**, чтобы запустить приложение в режиме отладки, или **CTRL-F5**, чтобы запустить его без подключения отладчика.</span><span class="sxs-lookup"><span data-stu-id="10004-130">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger.</span></span> <span data-ttu-id="10004-131">Нажмите **Принять**, чтобы согласиться на отслеживание.</span><span class="sxs-lookup"><span data-stu-id="10004-131">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="10004-132">Это приложение не отслеживает персональные данные.</span><span class="sxs-lookup"><span data-stu-id="10004-132">This app doesn't track personal information.</span></span> <span data-ttu-id="10004-133">Созданный шаблоном код включает ресурсы для соблюдения [Общего регламента по защите данных (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="10004-133">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR.png)

<span data-ttu-id="10004-135">На следующем рисунке показано приложение после принятия отслеживания:</span><span class="sxs-lookup"><span data-stu-id="10004-135">The following image shows the app after accepting tracking:</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/home2.1.png)

* <span data-ttu-id="10004-137">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение.</span><span class="sxs-lookup"><span data-stu-id="10004-137">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="10004-138">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="10004-138">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="10004-139">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="10004-139">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="10004-140">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="10004-140">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="10004-141">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="10004-141">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="10004-142">На представленном выше снимке экрана используется номер порта 5001.</span><span class="sxs-lookup"><span data-stu-id="10004-142">In the preceding image, the port number is 5001.</span></span> <span data-ttu-id="10004-143">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="10004-143">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="10004-144">Запуск приложения с помощью клавиш **CTRL+F5** (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="10004-144">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="10004-145">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.</span><span class="sxs-lookup"><span data-stu-id="10004-145">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

[!INCLUDE [F7](~/includes/RP/F7.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a><span data-ttu-id="10004-146">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="10004-146">Prerequisites</span></span>

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="10004-147">Создание веб-приложения Razor</span><span class="sxs-lookup"><span data-stu-id="10004-147">Create a Razor web app</span></span>

* <span data-ttu-id="10004-148">В меню **Файл** Visual Studio откройте **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="10004-148">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="10004-149">Создайте новое веб-приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="10004-149">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="10004-150">Назовите проект **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="10004-150">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="10004-151">Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.</span><span class="sxs-lookup"><span data-stu-id="10004-151">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="10004-152">![Новое веб-приложение ASP.NET Core](../../razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="10004-152">![new ASP.NET Core Web Application](../../razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="10004-153">Выберите в раскрывающемся списке **ASP.NET Core 2.0**, а затем **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="10004-153">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="10004-154">Шаблон Visual Studio создает начальный проект:</span><span class="sxs-lookup"><span data-stu-id="10004-154">The Visual Studio template creates a starter project:</span></span>

![обозреватель решений](razor-pages-start/_static/se.png)

<span data-ttu-id="10004-156">Нажмите клавишу **F5**, чтобы запустить приложение в режиме отладки, или **CTRL-F5**, чтобы запустить его без подключения отладчика.</span><span class="sxs-lookup"><span data-stu-id="10004-156">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/home.png)

* <span data-ttu-id="10004-158">Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем ваше приложение.</span><span class="sxs-lookup"><span data-stu-id="10004-158">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="10004-159">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="10004-159">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="10004-160">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="10004-160">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="10004-161">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="10004-161">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="10004-162">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="10004-162">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="10004-163">На представленном выше снимке экрана используется номер порта 5000.</span><span class="sxs-lookup"><span data-stu-id="10004-163">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="10004-164">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="10004-164">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="10004-165">Запуск приложения с помощью клавиш **CTRL+F5** (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="10004-165">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="10004-166">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.</span><span class="sxs-lookup"><span data-stu-id="10004-166">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="10004-167">Далее: добавление модели</span><span class="sxs-lookup"><span data-stu-id="10004-167">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
