---
title: Начало работы с MVC ASP.NET Core и Visual Studio для Mac
author: rick-anderson
description: Сведения о начале работы с MVC ASP.NET Core и Visual Studio
manager: wpickett
ms.author: riande
ms.date: 8/23/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: ffa620f07251c52c785672d8fbeefacac31ed4c1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="33e34-103">Начало работы с MVC ASP.NET Core и Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="33e34-103">Get started with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="33e34-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="33e34-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="33e34-105">В этом учебнике представлены основы сборки веб-приложений MVC ASP.NET Core с помощью [Visual Studio для Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span><span class="sxs-lookup"><span data-stu-id="33e34-105">This tutorial teaches you the basics of building an ASP.NET Core MVC web app using [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span></span> 

[!INCLUDE [consider RP](../../includes/razor.md)]

<span data-ttu-id="33e34-106">Существует 3 версии этого учебника:</span><span class="sxs-lookup"><span data-stu-id="33e34-106">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="33e34-107">macOS: [Создание приложения MVC ASP.NET Core с помощью Visual Studio для Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="33e34-107">macOS: [Build an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="33e34-108">Windows: [Создание приложения MVC ASP.NET Core с помощью Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="33e34-108">Windows: [Build an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="33e34-109">Linux, macOS и Windows: [Создание приложения MVC ASP.NET Core с помощью Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="33e34-109">Linux, macOS, and Windows: [Build an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="33e34-110">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="33e34-110">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="33e34-111">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="33e34-111">Create a web app</span></span>

<span data-ttu-id="33e34-112">В Visual Studio выберите **Файл > Новое решение**.</span><span class="sxs-lookup"><span data-stu-id="33e34-112">From Visual Studio, select **File > New Solution**.</span></span>

![Новое решение macOS](../first-web-api-mac/_static/sln.png)

<span data-ttu-id="33e34-114">Выберите **Приложение .NET Core > ASP.NET Core > Веб-приложение > Далее**.</span><span class="sxs-lookup"><span data-stu-id="33e34-114">Select **.NET Core App >  ASP.NET Core > Web App > Next**.</span></span>

![Диалоговое окно "Новый проект" в macOS](start-mvc/1.png)

<span data-ttu-id="33e34-116">Присвойте проекту имя **MvcMovie** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="33e34-116">Name the project **MvcMovie**, and then select **Create**.</span></span>

![Диалоговое окно "Новый проект" в macOS](start-mvc/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="33e34-118">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="33e34-118">Launch the app</span></span>

<span data-ttu-id="33e34-119">В Visual Studio выберите **Выполнить > Запуск без отладки**, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="33e34-119">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="33e34-120">Visual Studio запустит [Kestrel](xref:fundamentals/servers/index#kestrel), откроет браузер и перейдет к `http://localhost:port`, где *port* — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="33e34-120">Visual Studio starts [Kestrel](xref:fundamentals/servers/index#kestrel), launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

![Новый проект в браузере](start-mvc/b1.png)

* <span data-ttu-id="33e34-122">В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="33e34-122">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="33e34-123">Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="33e34-123">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="33e34-124">Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="33e34-124">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="33e34-125">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="33e34-125">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="33e34-126">В меню **Запуск** можно запустить приложение в режиме с отладкой или без нее.</span><span class="sxs-lookup"><span data-stu-id="33e34-126">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

<span data-ttu-id="33e34-127">Шаблон по умолчанию включает ссылки **Главная, О программе** и **Контакты**.</span><span class="sxs-lookup"><span data-stu-id="33e34-127">The default template gives you **Home, About** and **Contact** links.</span></span> <span data-ttu-id="33e34-128">На представленном выше снимке экрана эти ссылки не отображаются.</span><span class="sxs-lookup"><span data-stu-id="33e34-128">The browser image above doesn't show these links.</span></span> <span data-ttu-id="33e34-129">Если размер окна вашего браузера слишком мал, щелкните значок навигации, чтобы отобразить эти ссылки.</span><span class="sxs-lookup"><span data-stu-id="33e34-129">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Новый проект в браузере](start-mvc/b2.png)

<span data-ttu-id="33e34-131">В следующей части этого учебника мы поговорим об MVC и приступим к написанию кода.</span><span class="sxs-lookup"><span data-stu-id="33e34-131">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="33e34-132">Вперед</span><span class="sxs-lookup"><span data-stu-id="33e34-132">Next</span></span>](adding-controller.md)  
