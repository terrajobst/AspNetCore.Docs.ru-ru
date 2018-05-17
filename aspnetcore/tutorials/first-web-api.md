---
title: Создание веб-API с помощью ASP.NET Core и Visual Studio для Windows
author: rick-anderson
description: Сборка веб-API с помощью MVC ASP.NET Core и Visual Studio для Windows
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api
ms.openlocfilehash: cb46f8b4013488dbe2bb5ca3d08a8c6e452141dd
ms.sourcegitcommit: c867d7427bd4a88a78b2322e156367733b532730
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/09/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="4c3a3-103">Создание веб-API с помощью ASP.NET Core и Visual Studio для Windows</span><span class="sxs-lookup"><span data-stu-id="4c3a3-103">Create a Web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="4c3a3-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Майк Уоссон](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="4c3a3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="4c3a3-105">В этом руководстве вы создали веб-API для управления элементами списка дел.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="4c3a3-106">Пользовательский интерфейс не создан.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="4c3a3-107">Существуют три версии этого руководства:</span><span class="sxs-lookup"><span data-stu-id="4c3a3-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="4c3a3-108">Windows: создание веб-API с помощью Visual Studio для Windows (этот учебник)</span><span class="sxs-lookup"><span data-stu-id="4c3a3-108">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="4c3a3-109">macOS: [создание веб-API с помощью Visual Studio для Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="4c3a3-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="4c3a3-110">macOS, Linux, Windows: [создание веб-API с помощью Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="4c3a3-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="4c3a3-111">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="4c3a3-111">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="4c3a3-112">Создание проекта</span><span class="sxs-lookup"><span data-stu-id="4c3a3-112">Create the project</span></span>

<span data-ttu-id="4c3a3-113">Выполните следующие действия в Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="4c3a3-113">Follow these steps in Visual Studio:</span></span>

* <span data-ttu-id="4c3a3-114">В меню **Файл** выберите пункт **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-114">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="4c3a3-115">Выберите шаблон **Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-115">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="4c3a3-116">Назовите проект *TodoApi* и нажмите **OK**.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-116">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="4c3a3-117">В диалоговом окне **Новое веб-приложение ASP.NET Core — TodoApi** выберите версию ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-117">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="4c3a3-118">Выберите шаблон **API** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-118">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="4c3a3-119">**Не** выбирайте **Включение поддержки Docker**.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-119">Do **not** select **Enable Docker Support**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="4c3a3-120">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="4c3a3-120">Launch the app</span></span>

<span data-ttu-id="4c3a3-121">В Visual Studio нажмите клавиши CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-121">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="4c3a3-122">Visual Studio запустит браузер и перейдет к `http://localhost:<port>/api/values`, где `<port>` — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-122">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="4c3a3-123">В Chrome, Microsoft Edge и Firefox отобразится следующая информация.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-123">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```json
["value1","value2"]
```

### <a name="add-a-model-class"></a><span data-ttu-id="4c3a3-124">Добавление класса модели</span><span class="sxs-lookup"><span data-stu-id="4c3a3-124">Add a model class</span></span>

<span data-ttu-id="4c3a3-125">Модель — это объект, представляющий данные в приложении.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-125">A model is an object representing the data in the app.</span></span> <span data-ttu-id="4c3a3-126">В этом случае единственной моделью является задача.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-126">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="4c3a3-127">В обозревателе решений щелкните проект правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-127">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="4c3a3-128">Выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-128">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="4c3a3-129">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-129">Name the folder *Models*.</span></span>

> [!NOTE]
> <span data-ttu-id="4c3a3-130">Классы модели могут переходить в любое место в проекте.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-130">The model classes can go anywhere in the project.</span></span> <span data-ttu-id="4c3a3-131">Папка *Модели* используется по соглашению о классах модели.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-131">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="4c3a3-132">В обозревателе решений щелкните папку *Models* правой кнопкой мыши и выберите команду **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-132">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="4c3a3-133">Назовите класс *TodoItem* и нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-133">Name the class *TodoItem* and click **Add**.</span></span>

<span data-ttu-id="4c3a3-134">Обновите класс `TodoItem` с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="4c3a3-134">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="4c3a3-135">После создания `TodoItem` база данных формирует `Id`.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-135">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="4c3a3-136">Создание контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="4c3a3-136">Create the database context</span></span>

<span data-ttu-id="4c3a3-137">*Контекст базы данных* —это основной класс, который координирует функциональные возможности Entity Framework для заданной модели данных.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-137">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="4c3a3-138">Этот класс является производным от класса `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-138">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="4c3a3-139">В обозревателе решений щелкните папку *Models* правой кнопкой мыши и выберите команду **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-139">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="4c3a3-140">Назовите класс *TodoContext* и нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-140">Name the class *TodoContext* and click **Add**.</span></span>

<span data-ttu-id="4c3a3-141">Замените класс на следующий код.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-141">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="4c3a3-142">Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="4c3a3-142">Add a controller</span></span>

<span data-ttu-id="4c3a3-143">В обозревателе решений щелкните папку *Контроллеры* правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-143">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="4c3a3-144">Выберите **Добавить** > **Новый объект**.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-144">Select **Add** > **New Item**.</span></span> <span data-ttu-id="4c3a3-145">В диалоговом окне **Добавить новый элемент** выберите шаблон **Класс контроллера API**.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-145">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span> <span data-ttu-id="4c3a3-146">Назовите класс *TodoController* и нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-146">Name the class *TodoController*, and click **Add**.</span></span>

![Диалоговое окно добавления элемента с контроллером в поле поиска и выбранным контроллером веб-API](first-web-api/_static/new_controller.png)

<span data-ttu-id="4c3a3-148">Замените класс на следующий код.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-148">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="4c3a3-149">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="4c3a3-149">Launch the app</span></span>

<span data-ttu-id="4c3a3-150">В Visual Studio нажмите клавиши CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-150">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="4c3a3-151">Visual Studio запустит браузер и перейдет к `http://localhost:<port>/api/values`, где `<port>` — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-151">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="4c3a3-152">Перейдите к контроллеру `Todo` по адресу `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-152">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
