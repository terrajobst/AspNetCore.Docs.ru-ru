---
title: "Начало работы с Razor Pages в ASP.NET Core"
author: rick-anderson
description: "Начало работы с Razor Pages в ASP.NET Core"
manager: wpickett
ms.author: riande
ms.date: 12/22/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 54fa970d136de3903ae08b710b55f15f96f9a012
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="c8f53-103">Начало работы с Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c8f53-103">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="c8f53-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="c8f53-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c8f53-105">В этом учебнике приводятся основные сведения о веб-приложении Razor Pages в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c8f53-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="c8f53-106">Razor Pages — это рекомендуемый способ создания пользовательского интерфейса для веб-приложений в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c8f53-106">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="c8f53-107">Существуют три версии этого руководства:</span><span class="sxs-lookup"><span data-stu-id="c8f53-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="c8f53-108">Windows: это руководство.</span><span class="sxs-lookup"><span data-stu-id="c8f53-108">Windows: This tutorial</span></span>
* <span data-ttu-id="c8f53-109">MacOS: [Начало работы с Razor Pages в ASP.NET Core с использованием Visual Studio для Mac](xref:tutorials/razor-pages-mac/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="c8f53-109">MacOS: [Getting started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="c8f53-110">macOS, Linux и Windows: [Начало работы с Razor Pages в ASP.NET Core с Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="c8f53-110">macOS, Linux, and Windows: [Getting started with Razor Pages in ASP.NET Core with Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="c8f53-111">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c8f53-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c8f53-112">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="c8f53-112">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="c8f53-113">Создание веб-приложения Razor</span><span class="sxs-lookup"><span data-stu-id="c8f53-113">Create a Razor web app</span></span>

* <span data-ttu-id="c8f53-114">В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="c8f53-114">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="c8f53-115">Создайте новое веб-приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c8f53-115">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="c8f53-116">Назовите проект **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="c8f53-116">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="c8f53-117">Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.</span><span class="sxs-lookup"><span data-stu-id="c8f53-117">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="c8f53-118">![Новое веб-приложение ASP.NET Core](../../mvc/razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="c8f53-118">![new ASP.NET Core Web Application](../../mvc/razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="c8f53-119">Выберите в раскрывающемся списке **ASP.NET Core 2.0**, а затем **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="c8f53-119">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE[install 2.0](../../includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="c8f53-120">Шаблон Visual Studio создает начальный проект:</span><span class="sxs-lookup"><span data-stu-id="c8f53-120">The Visual Studio template creates a starter project:</span></span>

![обозреватель решений](razor-pages-start/_static/se.png)

<span data-ttu-id="c8f53-122">Нажмите клавишу **F5**, чтобы запустить приложение в режиме отладки, или **CTRL-F5**, чтобы запустить его без подключения отладчика.</span><span class="sxs-lookup"><span data-stu-id="c8f53-122">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/home.png)

* <span data-ttu-id="c8f53-124">Visual Studio запускает [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем ваше приложение.</span><span class="sxs-lookup"><span data-stu-id="c8f53-124">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="c8f53-125">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="c8f53-125">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="c8f53-126">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="c8f53-126">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="c8f53-127">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="c8f53-127">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="c8f53-128">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="c8f53-128">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="c8f53-129">На представленном выше снимке экрана используется номер порта 5000.</span><span class="sxs-lookup"><span data-stu-id="c8f53-129">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="c8f53-130">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="c8f53-130">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="c8f53-131">Запуск приложения с помощью клавиш **CTRL+F5** (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="c8f53-131">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="c8f53-132">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.</span><span class="sxs-lookup"><span data-stu-id="c8f53-132">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

>[!div class="step-by-step"]
[<span data-ttu-id="c8f53-133">Далее: добавление модели</span><span class="sxs-lookup"><span data-stu-id="c8f53-133">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)

>[!div class="step-by-step"]
[<span data-ttu-id="c8f53-134">Далее: добавление модели</span><span class="sxs-lookup"><span data-stu-id="c8f53-134">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
