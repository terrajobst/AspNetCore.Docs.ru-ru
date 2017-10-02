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
ms.openlocfilehash: 1d8d7805aafbf28fef044d09369a1dc76108b141
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/28/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="dc167-104">Начало работы с Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dc167-104">Getting started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="dc167-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="dc167-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="dc167-106">В этом учебнике приводятся основные сведения о веб-приложении Razor Pages в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dc167-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="dc167-107">Перед работой с этим учебником рекомендуем изучить статью [Введение в Razor Pages](xref:mvc/razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="dc167-107">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="dc167-108">Razor Pages — это рекомендуемый способ создания пользовательского интерфейса для веб-приложений в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dc167-108">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dc167-109">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="dc167-109">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="dc167-110">Создание веб-приложения Razor</span><span class="sxs-lookup"><span data-stu-id="dc167-110">Create a Razor web app</span></span>

* <span data-ttu-id="dc167-111">В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="dc167-111">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="dc167-112">Создайте новое веб-приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dc167-112">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="dc167-113">Назовите проект **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="dc167-113">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="dc167-114">Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.</span><span class="sxs-lookup"><span data-stu-id="dc167-114">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="dc167-115">![Новое веб-приложение ASP.NET Core](../../mvc/razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="dc167-115">![new ASP.NET Core Web Application](../../mvc/razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="dc167-116">Выберите в раскрывающемся списке **ASP.NET Core 2.0**, а затем **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="dc167-116">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>
  <span data-ttu-id="dc167-117">![Веб-приложение (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span><span class="sxs-lookup"><span data-stu-id="dc167-117">![Web Application (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span></span>

<span data-ttu-id="dc167-118">Шаблон Visual Studio создает начальный проект:</span><span class="sxs-lookup"><span data-stu-id="dc167-118">The Visual Studio template creates a starter project:</span></span>

![обозреватель решений](razor-pages-start/_static/se.png)

<span data-ttu-id="dc167-120">Нажмите клавишу **F5**, чтобы запустить приложение в режиме отладки, или **CTRL-F5**, чтобы запустить его без подключения отладчика.</span><span class="sxs-lookup"><span data-stu-id="dc167-120">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Домашняя или индексная страница](razor-pages-start/_static/home.png)

* <span data-ttu-id="dc167-122">Visual Studio запускает [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем ваше приложение.</span><span class="sxs-lookup"><span data-stu-id="dc167-122">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="dc167-123">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="dc167-123">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="dc167-124">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="dc167-124">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="dc167-125">Localhost обслуживает только веб-запросы с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="dc167-125">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="dc167-126">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="dc167-126">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="dc167-127">На представленном выше снимке экрана используется номер порта 5000.</span><span class="sxs-lookup"><span data-stu-id="dc167-127">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="dc167-128">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="dc167-128">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="dc167-129">Запуск приложения с помощью клавиш **CTRL+F5** (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде.</span><span class="sxs-lookup"><span data-stu-id="dc167-129">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="dc167-130">Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.</span><span class="sxs-lookup"><span data-stu-id="dc167-130">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

>[!div class="step-by-step"]
[<span data-ttu-id="dc167-131">Далее: добавление модели</span><span class="sxs-lookup"><span data-stu-id="dc167-131">Next: Adding a model</span></span>](xref:tutorials/razor-pages/modelz)
