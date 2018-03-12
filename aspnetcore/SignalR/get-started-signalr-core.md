---
title: "Приступая к работе с SignalR в ASP.NET Core"
author: rachelappel
ms.author: rachelap
description: "В этом учебнике создается приложение с помощью SignalR для ASP.NET Core."
manager: wpickett
ms.date: 03/06/2018
ms.topic: tutorial
ms.technology: dotnet-signalr
ms.prod: aspnet-core
ms.custom: mvc
uid: signalr/get-started-signalr-core
ms.openlocfilehash: 4afb9785fc3d0f472226a745537acbc77adefb4c
ms.sourcegitcommit: 9622bdc6326c28c3322c70000468a80ef21ad376
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2018
---
# <a name="tutorial-get-started-with-signalr-for-aspnet-core"></a><span data-ttu-id="216ad-103">Учебник: Приступая к работе с SignalR для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="216ad-103">Tutorial: Get started with SignalR for ASP.NET Core</span></span>

<span data-ttu-id="216ad-104">Автор: [Рэйчел Аппель](https://twitter.com/rachelappel) (Rachel Appel)</span><span class="sxs-lookup"><span data-stu-id="216ad-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="216ad-105">В этом учебнике показано основы построения в режиме реального времени приложения с помощью SignalR для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="216ad-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Решение](get-started-signalr-core/_static/signalr-get-started-finished.png)

<span data-ttu-id="216ad-107">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="216ad-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="216ad-108">В этом учебнике показано следующие задачи разработки SignalR:</span><span class="sxs-lookup"><span data-stu-id="216ad-108">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="216ad-109">Создайте веб-приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="216ad-109">Create an ASP.NET Core web app.</span></span>
> * <span data-ttu-id="216ad-110">Создание концентратора SignalR для передачи содержимого клиентам.</span><span class="sxs-lookup"><span data-stu-id="216ad-110">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="216ad-111">Используйте библиотеку SignalR JavaScript для отправки сообщений и отобразить обновления от концентратора.</span><span class="sxs-lookup"><span data-stu-id="216ad-111">Use the SignalR JavaScript library to send messages and display updates from the hub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="216ad-112">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="216ad-112">Prerequisites</span></span>

<span data-ttu-id="216ad-113">Установите следующее программное обеспечение:</span><span class="sxs-lookup"><span data-stu-id="216ad-113">Install the following software:</span></span>

* <span data-ttu-id="216ad-114">[.NET core 2.1.0 предварительного просмотра 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="216ad-114">[.NET Core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) or later</span></span>
* <span data-ttu-id="216ad-115">[Visual Studio 2017 г](https://www.visualstudio.com/downloads/) 15.6 или более поздней версии с рабочей нагрузкой ASP.NET и веб-разработки</span><span class="sxs-lookup"><span data-stu-id="216ad-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.6 or later with the ASP.NET and web development workload</span></span>
* [<span data-ttu-id="216ad-116">npm</span><span class="sxs-lookup"><span data-stu-id="216ad-116">npm</span></span>](https://www.npmjs.com/get-npm)

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="216ad-117">Создайте проект ASP.NET Core, на котором размещена SignalR клиента и сервера</span><span class="sxs-lookup"><span data-stu-id="216ad-117">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

1. <span data-ttu-id="216ad-118">Используйте **файл** > **новый проект** меню и выберите **веб-приложения ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="216ad-118">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="216ad-119">Задайте для проекта имя `SignalRChat`.</span><span class="sxs-lookup"><span data-stu-id="216ad-119">Name the project `SignalRChat`.</span></span>

  ![Диалоговое окно нового проекта в Visual Studio](get-started-signalr-core/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="216ad-121">Выберите **веб-приложения** для создания проекта с помощью страниц Razor.</span><span class="sxs-lookup"><span data-stu-id="216ad-121">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="216ad-122">Выберите **ОК**.</span><span class="sxs-lookup"><span data-stu-id="216ad-122">Then select **Ok**.</span></span> <span data-ttu-id="216ad-123">Убедитесь, что **ASP.NET Core 2.1** выбирается из селектора framework, то выполняется SignalR в предыдущих версиях .NET.</span><span class="sxs-lookup"><span data-stu-id="216ad-123">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

  ![Диалоговое окно нового проекта в Visual Studio](get-started-signalr-core/_static/signalr-new-project-choose-type.png)

  <span data-ttu-id="216ad-125">Библиотеки, в которых размещена SignalR серверный код, включаются в шаблоне проекта.</span><span class="sxs-lookup"><span data-stu-id="216ad-125">The libraries that host SignalR server-side code are included in the project template.</span></span> <span data-ttu-id="216ad-126">Установка клиентского JavaScript отдельно с [npm](https://www.npmjs.com/).</span><span class="sxs-lookup"><span data-stu-id="216ad-126">Install the client-side JavaScript separately with [npm](https://www.npmjs.com/).</span></span>

  ```console
   npm install @aspnet/signalr
  ```

3. <span data-ttu-id="216ad-127">Копировать *signalr.js* из *node_modules\\ @aspnet\signalr\dist\browser*  для *wwwroot\lib* в папке проекта.</span><span class="sxs-lookup"><span data-stu-id="216ad-127">Copy the *signalr.js* from *node_modules\\@aspnet\signalr\dist\browser* to the *wwwroot\lib* folder in your project.</span></span>

## <a name="create-the-signalr-hub"></a><span data-ttu-id="216ad-128">Создание концентратора SignalR</span><span class="sxs-lookup"><span data-stu-id="216ad-128">Create the SignalR Hub</span></span>

<span data-ttu-id="216ad-129">Концентратор является класс, который служит в качестве высокого уровня конвейера, который позволяет клиенту и серверу для вызова методов друг от друга.</span><span class="sxs-lookup"><span data-stu-id="216ad-129">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

1. <span data-ttu-id="216ad-130">Добавить класс в проект, выбрав **файл** > **New** > **файл** и выбрав **классов Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="216ad-130">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> 

1. <span data-ttu-id="216ad-131">Наследовать от `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="216ad-131">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="216ad-132">`Hub` Класс содержит свойства и события для управления подключения и группы, а также отправки и получения данных.</span><span class="sxs-lookup"><span data-stu-id="216ad-132">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

1. <span data-ttu-id="216ad-133">Создание `Send` метод, который отправляет сообщение всем клиентам, подключенным чат.</span><span class="sxs-lookup"><span data-stu-id="216ad-133">Create the `Send` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="216ad-134">Обратите внимание на то, она возвращает `Task`, так как SignalR является асинхронным.</span><span class="sxs-lookup"><span data-stu-id="216ad-134">Notice it returns a `Task`, because SignalR is asynchronous.</span></span> <span data-ttu-id="216ad-135">Лучше масштабируется асинхронного кода.</span><span class="sxs-lookup"><span data-stu-id="216ad-135">Asynchronous code scales better.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="216ad-136">Настроить проект для использования SignalR</span><span class="sxs-lookup"><span data-stu-id="216ad-136">Configure the project to use SignalR</span></span>

<span data-ttu-id="216ad-137">SignalR сервера необходимо настроить, чтобы он доверял передачи запросов для SignalR.</span><span class="sxs-lookup"><span data-stu-id="216ad-137">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="216ad-138">Чтобы настроить проект SignalR, измените `ConfigureServices` метод приложения `Startup` класса путем вставки вызов `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="216ad-138">To configure a SignalR project, modify the `ConfigureServices` method of the application's `Startup` class by inserting a call to `services.AddSignalR`.</span></span>

  <span data-ttu-id="216ad-139">`services.AddSignalR` Добавляет в составе SignalR [по промежуточного слоя ASP.NET Core](xref:fundamentals/middleware/index) конвейера.</span><span class="sxs-lookup"><span data-stu-id="216ad-139">`services.AddSignalR` adds SignalR as part of the [ASP.NET Core middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

1. <span data-ttu-id="216ad-140">Настроить маршруты для концентраторов с помощью `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="216ad-140">Configure routes to your hubs using `UseSignalR`.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="216ad-141">Создайте код клиента SignalR</span><span class="sxs-lookup"><span data-stu-id="216ad-141">Create the SignalR client code</span></span>

1. <span data-ttu-id="216ad-142">Замените содержимое в *Pages\Index.cshtml* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="216ad-142">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](get-started-signalr-core/sample/Pages/Index.cshtml)]

  <span data-ttu-id="216ad-143">Предыдущий HTML отображает имя и поля сообщения и кнопка отправки.</span><span class="sxs-lookup"><span data-stu-id="216ad-143">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="216ad-144">Обратите внимание, ссылки на скрипты в нижней: ссылку на SignalR и *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="216ad-144">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

1. <span data-ttu-id="216ad-145">Добавьте файл JavaScript, чтобы *wwwroot\js* папку с именем *chat.js* и добавьте в него следующий код:</span><span class="sxs-lookup"><span data-stu-id="216ad-145">Add a JavaScript file to the *wwwroot\js* folder named *chat.js* and add the following code to it:</span></span>

  [!code-javascript[Index](get-started-signalr-core/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="216ad-146">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="216ad-146">Run the app</span></span>

1. <span data-ttu-id="216ad-147">Выберите **отладки** > **Запуск без отладки** запускает браузер и загрузить веб-сайт локально.</span><span class="sxs-lookup"><span data-stu-id="216ad-147">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="216ad-148">Скопируйте URL-адрес из адресной строки.</span><span class="sxs-lookup"><span data-stu-id="216ad-148">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="216ad-149">Откройте другой экземпляр браузера (любой браузер) и вставьте URL-адрес в адресной строке.</span><span class="sxs-lookup"><span data-stu-id="216ad-149">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="216ad-150">Выберите любой браузер, введите имя и сообщение и нажмите кнопку **отправки** кнопки.</span><span class="sxs-lookup"><span data-stu-id="216ad-150">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="216ad-151">Имя и сообщения отображаются на обеих страницах мгновенно.</span><span class="sxs-lookup"><span data-stu-id="216ad-151">The name and message are displayed on both pages instantly.</span></span>

  ![Решение](get-started-signalr-core/_static/signalr-get-started-finished.png)
