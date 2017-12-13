---
title: "Начало работы с Razor Pages в ASP.NET Core"
author: rick-anderson
description: "Начало работы с Razor Pages в ASP.NET Core"
keywords: ASP.NET Core,Razor Pages,Razor,MVC
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 5c58b5156f62572687755c9c0878db10c3c14eb1
ms.sourcegitcommit: c07fb5cb5df0a12f9fe6735fcbc90964608fa687
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/14/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="bee6c-104">Начало работы с Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bee6c-104">Getting started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="bee6c-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bee6c-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bee6c-106">В этом учебнике приводятся основные сведения о веб-приложении Razor Pages в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bee6c-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="bee6c-107">Перед работой с этим учебником рекомендуем изучить статью [Введение в Razor Pages](xref:mvc/razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="bee6c-107">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="bee6c-108">Razor Pages — это рекомендуемый способ создания пользовательского интерфейса для веб-приложений в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bee6c-108">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

<span data-ttu-id="bee6c-109">Существуют три версии этого руководства:</span><span class="sxs-lookup"><span data-stu-id="bee6c-109">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="bee6c-110">Windows: это руководство.</span><span class="sxs-lookup"><span data-stu-id="bee6c-110">Windows: This tutorial</span></span>
* <span data-ttu-id="bee6c-111">MacOS: [Начало работы с Razor Pages в ASP.NET Core с использованием Visual Studio для Mac](xref:tutorials/razor-pages-mac/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="bee6c-111">MacOS: [Getting started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="bee6c-112">macOS, Linux и Windows: [Начало работы с Razor Pages в ASP.NET Core с Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="bee6c-112">macOS, Linux, and Windows: [Getting started with Razor Pages in ASP.NET Core with Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bee6c-113">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="bee6c-113">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="bee6c-114">Создание веб-приложения Razor</span><span class="sxs-lookup"><span data-stu-id="bee6c-114">Create a Razor web app</span></span>

* <span data-ttu-id="bee6c-115">В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="bee6c-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="bee6c-116">Создайте новое веб-приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bee6c-116">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="bee6c-117">Назовите проект **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="bee6c-117">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="bee6c-118">Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.</span><span class="sxs-lookup"><span data-stu-id="bee6c-118">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="bee6c-119">![Новое веб-приложение ASP.NET Core](../../mvc/razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="bee6c-119">![new ASP.NET Core Web Application](../../mvc/razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="bee6c-120">Выберите в раскрывающемся списке **ASP.NET Core 2.0**, а затем **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="bee6c-120">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>
  <span data-ttu-id="bee6c-121">![Веб-приложение (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span><span class="sxs-lookup"><span data-stu-id="bee6c-121">![Web Application (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span></span>

<span data-ttu-id="bee6c-122">Шаблон Visual Studio создает начальный проект:</span><span class="sxs-lookup"><span data-stu-id="bee6c-122">The Visual Studio template creates a starter project:</span></span>

![обозреватель решений](razor-pages-start/_static/se.png)

<span data-ttu-id="bee6c-124">Нажмите клавишу **F5**, чтобы запустить приложение в режиме отладки, или **CTRL-F5**, чтобы запустить его без подключения отладчика.</span><span class="sxs-lookup"><span data-stu-id="bee6c-124">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/home.png)

* <span data-ttu-id="bee6c-126">Visual Studio запускает [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем ваше приложение.</span><span class="sxs-lookup"><span data-stu-id="bee6c-126">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="bee6c-127">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="bee6c-127">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="bee6c-128">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="bee6c-128">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="bee6c-129">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="bee6c-129">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="bee6c-130">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="bee6c-130">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="bee6c-131">На представленном выше снимке экрана используется номер порта 5000.</span><span class="sxs-lookup"><span data-stu-id="bee6c-131">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="bee6c-132">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="bee6c-132">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="bee6c-133">Запуск приложения с помощью клавиш **CTRL+F5** (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="bee6c-133">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="bee6c-134">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.</span><span class="sxs-lookup"><span data-stu-id="bee6c-134">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

>[!div class="step-by-step"]
[<span data-ttu-id="bee6c-135">Далее: добавление модели</span><span class="sxs-lookup"><span data-stu-id="bee6c-135">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
