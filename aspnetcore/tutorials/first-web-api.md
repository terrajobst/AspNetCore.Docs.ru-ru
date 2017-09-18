---
title: "Создание веб-API с помощью ASP.NET Core и Visual Studio для Windows"
author: rick-anderson
description: "Сборка веб-API с помощью MVC ASP.NET Core и Visual Studio для Windows"
keywords: "ASP.NET Core,вебAPI,веб-API,REST,HTTP,служба,служба HTTP"
ms.author: riande
manager: wpickett
ms.date: 8/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-web-api
ms.openlocfilehash: 4aab61c7ee4498b33a4ea8bbec6033ce9828e2af
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
#<a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="269f2-104">Создание веб-API с помощью ASP.NET Core и Visual Studio для Windows</span><span class="sxs-lookup"><span data-stu-id="269f2-104">Create a web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="269f2-105">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Майк Уоссон](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="269f2-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="269f2-106">В этом учебнике вы создадите веб-API для управления списком дел.</span><span class="sxs-lookup"><span data-stu-id="269f2-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="269f2-107">Вы не будете создавать пользовательский интерфейс.</span><span class="sxs-lookup"><span data-stu-id="269f2-107">You won’t build a UI.</span></span>

<span data-ttu-id="269f2-108">Существует 3 версии этого учебника.</span><span class="sxs-lookup"><span data-stu-id="269f2-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="269f2-109">Windows: создание веб-API с помощью Visual Studio для Windows (этот учебник)</span><span class="sxs-lookup"><span data-stu-id="269f2-109">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="269f2-110">macOS: [создание веб-API с помощью Visual Studio для Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="269f2-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="269f2-111">macOS, Linux, Windows: [создание веб-API с помощью Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="269f2-111">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="269f2-112">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="269f2-112">Prerequisites</span></span>

[!INCLUDE[install 2.0](../includes/install2.0.md)]

<span data-ttu-id="269f2-113">См. [этот PDF-файл](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) для версии ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="269f2-113">See [this PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) for the ASP.NET Core 1.1 version.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="269f2-114">Создание проекта</span><span class="sxs-lookup"><span data-stu-id="269f2-114">Create the project</span></span>

<span data-ttu-id="269f2-115">В Visual Studio откройте меню **Файл** > **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="269f2-115">From Visual Studio, select **File** menu, > **New** > **Project**.</span></span>

<span data-ttu-id="269f2-116">Выберите шаблон проекта **Веб-приложение ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="269f2-116">Select the **ASP.NET Core Web Application (.NET Core)** project template.</span></span> <span data-ttu-id="269f2-117">Задайте для проекта имя `TodoApi` и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="269f2-117">Name the project `TodoApi` and select **OK**.</span></span>

![Диалоговое окно создания проекта](first-web-api/_static/new-project.png)

<span data-ttu-id="269f2-119">В диалоговом окне **Новое веб-приложение ASP.NET Core — TodoApi** выберите шаблон **Веб-API**.</span><span class="sxs-lookup"><span data-stu-id="269f2-119">In the **New ASP.NET Core Web Application - TodoApi** dialog, select the **Web API** template.</span></span> <span data-ttu-id="269f2-120">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="269f2-120">Select **OK**.</span></span> <span data-ttu-id="269f2-121">**Не** выбирайте **Включение поддержки Docker**.</span><span class="sxs-lookup"><span data-stu-id="269f2-121">Do **not** select **Enable Docker Support**.</span></span>

![Диалоговое окно создания веб-приложения ASP.NET с помощью шаблона проекта веб-API, выбранное из базовых шаблонов ASP.NET](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a><span data-ttu-id="269f2-123">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="269f2-123">Launch the app</span></span>

<span data-ttu-id="269f2-124">В Visual Studio нажмите клавиши CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="269f2-124">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="269f2-125">Visual Studio запустит браузер и перейдет к `http://localhost:port/api/values`, где *порт* — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="269f2-125">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly-chosen port number.</span></span> <span data-ttu-id="269f2-126">В Chrome, Edge и Firefox отобразится следующая информация.</span><span class="sxs-lookup"><span data-stu-id="269f2-126">Chrome, Edge, and Firefox display the following:</span></span>

```
["value1","value2"]
``` 

### <a name="add-a-model-class"></a><span data-ttu-id="269f2-127">Добавление класса модели</span><span class="sxs-lookup"><span data-stu-id="269f2-127">Add a model class</span></span>

<span data-ttu-id="269f2-128">Модель — это объект, представляющий данные в приложении.</span><span class="sxs-lookup"><span data-stu-id="269f2-128">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="269f2-129">В этом случае единственной моделью является задача.</span><span class="sxs-lookup"><span data-stu-id="269f2-129">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="269f2-130">Добавьте папку с именем "Models".</span><span class="sxs-lookup"><span data-stu-id="269f2-130">Add a folder named "Models".</span></span> <span data-ttu-id="269f2-131">В обозревателе решений щелкните проект правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="269f2-131">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="269f2-132">Выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="269f2-132">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="269f2-133">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="269f2-133">Name the folder *Models*.</span></span>

<span data-ttu-id="269f2-134">Примечание. Классы моделей можно размещать в любом месте проекта, но обычно используется папка *Models*.</span><span class="sxs-lookup"><span data-stu-id="269f2-134">Note: The model classes go anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="269f2-135">Добавьте класс `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="269f2-135">Add a `TodoItem` class.</span></span> <span data-ttu-id="269f2-136">Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="269f2-136">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="269f2-137">Присвойте классу имя `TodoItem` и выберите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="269f2-137">Name the class `TodoItem` and select **Add**.</span></span>

<span data-ttu-id="269f2-138">Замените сгенерированный код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="269f2-138">Replace the generated code with the following:</span></span>

<span data-ttu-id="269f2-139">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span><span class="sxs-lookup"><span data-stu-id="269f2-139">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span></span>

<span data-ttu-id="269f2-140">После создания `TodoItem` база данных формирует `Id`.</span><span class="sxs-lookup"><span data-stu-id="269f2-140">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="269f2-141">Создание контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="269f2-141">Create the database context</span></span>

<span data-ttu-id="269f2-142">*Контекст базы данных* —это основной класс, который координирует функциональные возможности Entity Framework для заданной модели данных.</span><span class="sxs-lookup"><span data-stu-id="269f2-142">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="269f2-143">Этот класс является производным от класса `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="269f2-143">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="269f2-144">Добавьте класс `TodoContext`.</span><span class="sxs-lookup"><span data-stu-id="269f2-144">Add a `TodoContext` class.</span></span> <span data-ttu-id="269f2-145">Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="269f2-145">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="269f2-146">Присвойте классу имя `TodoContext` и выберите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="269f2-146">Name the class `TodoContext` and select **Add**.</span></span>

<span data-ttu-id="269f2-147">Замените сгенерированный код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="269f2-147">Replace the generated code with the following:</span></span>

<span data-ttu-id="269f2-148">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="269f2-148">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span></span>

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="269f2-149">Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="269f2-149">Add a controller</span></span>

<span data-ttu-id="269f2-150">В обозревателе решений щелкните папку *Контроллеры* правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="269f2-150">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="269f2-151">Выберите **Добавить** > **Новый объект**.</span><span class="sxs-lookup"><span data-stu-id="269f2-151">Select **Add** > **New Item**.</span></span> <span data-ttu-id="269f2-152">В диалоговом окне **Добавление нового элемента** выберите шаблон **Класс контроллера веб-API**.</span><span class="sxs-lookup"><span data-stu-id="269f2-152">In the **Add New Item** dialog, select the **Web  API Controller Class** template.</span></span> <span data-ttu-id="269f2-153">Присвойте классу имя `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="269f2-153">Name the class `TodoController`.</span></span>

![Диалоговое окно добавления элемента с контроллером в поле поиска и выбранным контроллером веб-API](first-web-api/_static/new_controller.png)

<span data-ttu-id="269f2-155">Замените сгенерированный код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="269f2-155">Replace the generated code with the following:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]
  
### <a name="launch-the-app"></a><span data-ttu-id="269f2-156">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="269f2-156">Launch the app</span></span>

<span data-ttu-id="269f2-157">В Visual Studio нажмите клавиши CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="269f2-157">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="269f2-158">Visual Studio запустит браузер и перейдет к `http://localhost:port/api/values`, где *порт* — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="269f2-158">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="269f2-159">Если вы используете Chrome, Edge или Firefox, отобразятся данные.</span><span class="sxs-lookup"><span data-stu-id="269f2-159">If you're using Chrome, Edge or Firefox, the data will be displayed.</span></span> <span data-ttu-id="269f2-160">Если вы используете IE, он предложит вам открыть или сохранить файл *values.json*.</span><span class="sxs-lookup"><span data-stu-id="269f2-160">If you're using IE, IE will prompt to you open or save the *values.json* file.</span></span> <span data-ttu-id="269f2-161">Перейдите к только что созданному контроллеру `Todo` по адресу `http://localhost:port/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="269f2-161">Navigate to the `Todo` controller we just created `http://localhost:port/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

