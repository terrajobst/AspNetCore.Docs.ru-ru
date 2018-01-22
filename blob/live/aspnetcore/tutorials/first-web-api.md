---
title: "Создание веб-API с помощью ASP.NET Core и Visual Studio для Windows"
author: rick-anderson
description: "Сборка веб-API с помощью MVC ASP.NET Core и Visual Studio для Windows"
ms.author: riande
manager: wpickett
ms.date: 08/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-web-api
ms.openlocfilehash: 234dbf73f9751ad4f995d6e7471d94f060fb15bf
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
#<a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="c7f8a-103">Создание веб-API с помощью ASP.NET Core и Visual Studio для Windows</span><span class="sxs-lookup"><span data-stu-id="c7f8a-103">Create a web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="c7f8a-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Майк Уоссон](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="c7f8a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="c7f8a-105">В этом руководстве вы создали веб-API для управления элементами списка дел.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="c7f8a-106">Пользовательский интерфейс (UI) не создан.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-106">A user interface (UI) is not created.</span></span>

<span data-ttu-id="c7f8a-107">Существует 3 версии этого учебника:</span><span class="sxs-lookup"><span data-stu-id="c7f8a-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="c7f8a-108">Windows: создание веб-API с помощью Visual Studio для Windows (этот учебник)</span><span class="sxs-lookup"><span data-stu-id="c7f8a-108">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="c7f8a-109">macOS: [создание веб-API с помощью Visual Studio для Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="c7f8a-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="c7f8a-110">macOS, Linux, Windows: [создание веб-API с помощью Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="c7f8a-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="c7f8a-111">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="c7f8a-111">Prerequisites</span></span>

[!INCLUDE[install 2.0](../includes/install2.0.md)]

<span data-ttu-id="c7f8a-112">См. [этот PDF-файл](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) для версии ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-112">See [this PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) for the ASP.NET Core 1.1 version.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="c7f8a-113">Создание проекта</span><span class="sxs-lookup"><span data-stu-id="c7f8a-113">Create the project</span></span>

<span data-ttu-id="c7f8a-114">В Visual Studio откройте меню **Файл** > **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-114">From Visual Studio, select **File** menu, > **New** > **Project**.</span></span>

<span data-ttu-id="c7f8a-115">Выберите шаблон проекта **Веб-приложение ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-115">Select the **ASP.NET Core Web Application (.NET Core)** project template.</span></span> <span data-ttu-id="c7f8a-116">Задайте для проекта имя `TodoApi` и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-116">Name the project `TodoApi` and select **OK**.</span></span>

![Диалоговое окно создания проекта](first-web-api/_static/new-project.png)

<span data-ttu-id="c7f8a-118">В диалоговом окне **Новое веб-приложение ASP.NET Core — TodoApi** выберите шаблон **Веб-API**.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-118">In the **New ASP.NET Core Web Application - TodoApi** dialog, select the **Web API** template.</span></span> <span data-ttu-id="c7f8a-119">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-119">Select **OK**.</span></span> <span data-ttu-id="c7f8a-120">**Не** выбирайте **Включение поддержки Docker**.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-120">Do **not** select **Enable Docker Support**.</span></span>

![Диалоговое окно создания веб-приложения ASP.NET с помощью шаблона проекта веб-API, выбранное из базовых шаблонов ASP.NET](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a><span data-ttu-id="c7f8a-122">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="c7f8a-122">Launch the app</span></span>

<span data-ttu-id="c7f8a-123">В Visual Studio нажмите клавиши CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-123">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="c7f8a-124">Visual Studio запустит браузер и перейдет к `http://localhost:port/api/values`, где *порт* — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-124">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="c7f8a-125">В Chrome, Microsoft Edge и Firefox отобразится следующая информация.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-125">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```
["value1","value2"]
```

### <a name="add-a-model-class"></a><span data-ttu-id="c7f8a-126">Добавление класса модели</span><span class="sxs-lookup"><span data-stu-id="c7f8a-126">Add a model class</span></span>

<span data-ttu-id="c7f8a-127">Модель — это объект, представляющий данные в приложении.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-127">A model is an object that represents the data in the app.</span></span> <span data-ttu-id="c7f8a-128">В этом случае единственной моделью является задача.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-128">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="c7f8a-129">Добавьте папку с именем "Models".</span><span class="sxs-lookup"><span data-stu-id="c7f8a-129">Add a folder named "Models".</span></span> <span data-ttu-id="c7f8a-130">В обозревателе решений щелкните проект правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-130">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="c7f8a-131">Выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-131">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="c7f8a-132">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-132">Name the folder *Models*.</span></span>

<span data-ttu-id="c7f8a-133">Примечание. Классы модели могут переходить в любое место в проекте.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-133">Note: The model classes go anywhere in the project.</span></span> <span data-ttu-id="c7f8a-134">Папка *Модели* используется по соглашению о классах модели.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-134">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="c7f8a-135">Добавьте класс `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-135">Add a `TodoItem` class.</span></span> <span data-ttu-id="c7f8a-136">Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-136">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="c7f8a-137">Присвойте классу имя `TodoItem` и выберите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-137">Name the class `TodoItem` and select **Add**.</span></span>

<span data-ttu-id="c7f8a-138">Обновите класс `TodoItem` с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="c7f8a-138">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="c7f8a-139">После создания `TodoItem` база данных формирует `Id`.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-139">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="c7f8a-140">Создание контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="c7f8a-140">Create the database context</span></span>

<span data-ttu-id="c7f8a-141">*Контекст базы данных* —это основной класс, который координирует функциональные возможности Entity Framework для заданной модели данных.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-141">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="c7f8a-142">Этот класс является производным от класса `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-142">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="c7f8a-143">Добавьте класс `TodoContext`.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-143">Add a `TodoContext` class.</span></span> <span data-ttu-id="c7f8a-144">Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-144">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="c7f8a-145">Присвойте классу имя `TodoContext` и выберите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-145">Name the class `TodoContext` and select **Add**.</span></span>

<span data-ttu-id="c7f8a-146">Замените класс на следующий код.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-146">Replace the class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="c7f8a-147">Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="c7f8a-147">Add a controller</span></span>

<span data-ttu-id="c7f8a-148">В обозревателе решений щелкните папку *Контроллеры* правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-148">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="c7f8a-149">Выберите **Добавить** > **Новый объект**.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-149">Select **Add** > **New Item**.</span></span> <span data-ttu-id="c7f8a-150">В диалоговом окне **Add New Item** (Добавление нового элемента) выберите шаблон **Класс контроллера веб-API**.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-150">In the **Add New Item** dialog, select the **Web API Controller Class** template.</span></span> <span data-ttu-id="c7f8a-151">Присвойте классу имя `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-151">Name the class `TodoController`.</span></span>

![Диалоговое окно добавления элемента с контроллером в поле поиска и выбранным контроллером веб-API](first-web-api/_static/new_controller.png)

<span data-ttu-id="c7f8a-153">Замените класс на следующий код.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-153">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="c7f8a-154">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="c7f8a-154">Launch the app</span></span>

<span data-ttu-id="c7f8a-155">В Visual Studio нажмите клавиши CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-155">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="c7f8a-156">Visual Studio запустит браузер и перейдет к `http://localhost:port/api/values`, где *порт* — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-156">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="c7f8a-157">Перейдите к контроллеру `Todo` по адресу `http://localhost:port/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="c7f8a-157">Navigate to the `Todo` controller at `http://localhost:port/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

