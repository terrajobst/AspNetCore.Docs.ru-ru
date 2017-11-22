---
title: "Начало работы с MVC ASP.NET Core и Visual Studio для Mac"
author: rick-anderson
description: "Начало работы с MVC ASP.NET Core и Visual Studio"
keywords: "ASP.NET Core, MVC, Visual Studio для Mac, Entity Framework"
ms.author: riande
manager: wpickett
ms.date: 8/23/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: 387d7a91ae7d58cbc293c04039017df1dd208c82
ms.sourcegitcommit: f017f940a164dbaf84307410c78eb14e0f3ac811
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="e0d51-104">Начало работы с MVC ASP.NET Core и Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="e0d51-104">Getting started with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="e0d51-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="e0d51-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e0d51-106">В этом учебнике представлены основы сборки веб-приложений MVC ASP.NET Core с помощью [Visual Studio для Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span><span class="sxs-lookup"><span data-stu-id="e0d51-106">This tutorial teaches you the basics of building an ASP.NET Core MVC web app using [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span></span> 

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="e0d51-107">Существует 3 версии этого учебника:</span><span class="sxs-lookup"><span data-stu-id="e0d51-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="e0d51-108">macOS: [Создание приложения MVC ASP.NET Core с помощью Visual Studio для Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="e0d51-108">macOS: [Build an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="e0d51-109">Windows: [Создание приложения MVC ASP.NET Core с помощью Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="e0d51-109">Windows: [Build an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="e0d51-110">Linux, macOS и Windows: [Создание приложения MVC ASP.NET Core с помощью Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="e0d51-110">Linux, macOS, and Windows: [Build an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e0d51-111">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="e0d51-111">Prerequisites</span></span>

<span data-ttu-id="e0d51-112">Для этого учебника требуется [пакет SDK для .NET Core 2.0.0](https://www.microsoft.com/net/core) или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="e0d51-112">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span> <span data-ttu-id="e0d51-113">См. [этот PDF-файл](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) для версии ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="e0d51-113">See [the pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) for the ASP.NET Core 1.1 version.</span></span>

<span data-ttu-id="e0d51-114">Установите следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="e0d51-114">Install the following:</span></span>

- <span data-ttu-id="e0d51-115">[пакет SDK для .NET Core 2.0.0](https://www.microsoft.com/net/core) или более поздней версии;</span><span class="sxs-lookup"><span data-stu-id="e0d51-115">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
- [<span data-ttu-id="e0d51-116">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="e0d51-116">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-web-app"></a><span data-ttu-id="e0d51-117">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="e0d51-117">Create a web app</span></span>

<span data-ttu-id="e0d51-118">В Visual Studio выберите **Файл > Новое решение**.</span><span class="sxs-lookup"><span data-stu-id="e0d51-118">From Visual Studio, select **File > New Solution**.</span></span>

![Новое решение macOS](../first-web-api-mac/_static/sln.png)

<span data-ttu-id="e0d51-120">Выберите **Приложение .NET Core > ASP.NET Core > Веб-приложение > Далее**.</span><span class="sxs-lookup"><span data-stu-id="e0d51-120">Select **.NET Core App >  ASP.NET Core > Web App > Next**.</span></span>

![Диалоговое окно "Новый проект" в macOS](start-mvc/1.png)

<span data-ttu-id="e0d51-122">Присвойте проекту имя **MvcMovie** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="e0d51-122">Name the project **MvcMovie**, and then select **Create**.</span></span>

![Диалоговое окно "Новый проект" в macOS](start-mvc/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="e0d51-124">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="e0d51-124">Launch the app</span></span>

<span data-ttu-id="e0d51-125">В Visual Studio выберите **Выполнить > Запуск без отладки**, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="e0d51-125">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="e0d51-126">Visual Studio запустит [Kestrel](xref:fundamentals/servers/index#Kestrel), откроет браузер и перейдет к `http://localhost:port`, где *port* — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="e0d51-126">Visual Studio starts [Kestrel](xref:fundamentals/servers/index#Kestrel), launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

![Новый проект в браузере](start-mvc/b1.png)

* <span data-ttu-id="e0d51-128">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="e0d51-128">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e0d51-129">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="e0d51-129">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="e0d51-130">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="e0d51-130">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="e0d51-131">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="e0d51-131">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="e0d51-132">В меню **Запуск** можно запустить приложение в режиме с отладкой или без нее.</span><span class="sxs-lookup"><span data-stu-id="e0d51-132">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

<span data-ttu-id="e0d51-133">Шаблон по умолчанию включает ссылки **Главная, О программе** и **Контакты**.</span><span class="sxs-lookup"><span data-stu-id="e0d51-133">The default template gives you **Home, About** and **Contact** links.</span></span> <span data-ttu-id="e0d51-134">На представленном выше снимке экрана эти ссылки не отображаются.</span><span class="sxs-lookup"><span data-stu-id="e0d51-134">The browser image above doesn't show these links.</span></span> <span data-ttu-id="e0d51-135">Если размер окна вашего браузера слишком мал, щелкните значок навигации, чтобы отобразить эти ссылки.</span><span class="sxs-lookup"><span data-stu-id="e0d51-135">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Новый проект в браузере](start-mvc/b2.png)

<span data-ttu-id="e0d51-137">В следующей части этого учебника мы поговорим об MVC и приступим к написанию кода.</span><span class="sxs-lookup"><span data-stu-id="e0d51-137">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="e0d51-138">Вперед</span><span class="sxs-lookup"><span data-stu-id="e0d51-138">Next</span></span>](adding-controller.md)  
