---
title: Создание веб-API с помощью ASP.NET Core и Visual Studio для Windows
author: rick-anderson
description: Сборка веб-API с помощью MVC ASP.NET Core и Visual Studio для Windows
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api
ms.openlocfilehash: 1680d5e0be0f4844c904d923af30634c53bd1b81
ms.sourcegitcommit: 40b102ecf88e53d9d872603ce6f3f7044bca95ce
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/15/2018
ms.locfileid: "34306677"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="65357-103">Создание веб-API с помощью ASP.NET Core и Visual Studio для Windows</span><span class="sxs-lookup"><span data-stu-id="65357-103">Create a Web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="65357-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Майк Уоссон](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="65357-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="65357-105">В этом руководстве вы создали веб-API для управления элементами списка дел.</span><span class="sxs-lookup"><span data-stu-id="65357-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="65357-106">Пользовательский интерфейс не создан.</span><span class="sxs-lookup"><span data-stu-id="65357-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="65357-107">Существуют три версии этого руководства:</span><span class="sxs-lookup"><span data-stu-id="65357-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="65357-108">Windows: создание веб-API с помощью Visual Studio для Windows (этот учебник)</span><span class="sxs-lookup"><span data-stu-id="65357-108">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="65357-109">macOS: [создание веб-API с помощью Visual Studio для Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="65357-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="65357-110">macOS, Linux, Windows: [создание веб-API с помощью Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="65357-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="65357-111">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="65357-111">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="65357-112">Создание проекта</span><span class="sxs-lookup"><span data-stu-id="65357-112">Create the project</span></span>

<span data-ttu-id="65357-113">Выполните следующие действия в Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="65357-113">Follow these steps in Visual Studio:</span></span>

* <span data-ttu-id="65357-114">В меню **Файл** выберите пункт **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="65357-114">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="65357-115">Выберите шаблон **Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="65357-115">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="65357-116">Назовите проект *TodoApi* и нажмите **OK**.</span><span class="sxs-lookup"><span data-stu-id="65357-116">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="65357-117">В диалоговом окне **Новое веб-приложение ASP.NET Core — TodoApi** выберите версию ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="65357-117">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="65357-118">Выберите шаблон **API** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="65357-118">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="65357-119">**Не** выбирайте **Включение поддержки Docker**.</span><span class="sxs-lookup"><span data-stu-id="65357-119">Do **not** select **Enable Docker Support**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="65357-120">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="65357-120">Launch the app</span></span>

<span data-ttu-id="65357-121">В Visual Studio нажмите клавиши CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="65357-121">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="65357-122">Visual Studio запустит браузер и перейдет к `http://localhost:<port>/api/values`, где `<port>` — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="65357-122">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="65357-123">В Chrome, Microsoft Edge и Firefox отобразится следующая информация.</span><span class="sxs-lookup"><span data-stu-id="65357-123">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="65357-124">При использовании Internet Explorer будет предложено сохранить файл *values.json*.</span><span class="sxs-lookup"><span data-stu-id="65357-124">If using Internet Explorer, you'll be prompted to save a *values.json* file.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="65357-125">Добавление класса модели</span><span class="sxs-lookup"><span data-stu-id="65357-125">Add a model class</span></span>

<span data-ttu-id="65357-126">Модель — это объект, представляющий данные в приложении.</span><span class="sxs-lookup"><span data-stu-id="65357-126">A model is an object representing the data in the app.</span></span> <span data-ttu-id="65357-127">В этом случае единственной моделью является задача.</span><span class="sxs-lookup"><span data-stu-id="65357-127">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="65357-128">В обозревателе решений щелкните проект правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="65357-128">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="65357-129">Выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="65357-129">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="65357-130">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="65357-130">Name the folder *Models*.</span></span>

> [!NOTE]
> <span data-ttu-id="65357-131">Классы модели могут переходить в любое место в проекте.</span><span class="sxs-lookup"><span data-stu-id="65357-131">The model classes can go anywhere in the project.</span></span> <span data-ttu-id="65357-132">Папка *Модели* используется по соглашению о классах модели.</span><span class="sxs-lookup"><span data-stu-id="65357-132">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="65357-133">В обозревателе решений щелкните папку *Models* правой кнопкой мыши и выберите команду **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="65357-133">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="65357-134">Назовите класс *TodoItem* и нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="65357-134">Name the class *TodoItem* and click **Add**.</span></span>

<span data-ttu-id="65357-135">Обновите класс `TodoItem` с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="65357-135">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="65357-136">После создания `TodoItem` база данных формирует `Id`.</span><span class="sxs-lookup"><span data-stu-id="65357-136">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="65357-137">Создание контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="65357-137">Create the database context</span></span>

<span data-ttu-id="65357-138">*Контекст базы данных* —это основной класс, который координирует функциональные возможности Entity Framework для заданной модели данных.</span><span class="sxs-lookup"><span data-stu-id="65357-138">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="65357-139">Этот класс является производным от класса `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="65357-139">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="65357-140">В обозревателе решений щелкните папку *Models* правой кнопкой мыши и выберите команду **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="65357-140">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="65357-141">Назовите класс *TodoContext* и нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="65357-141">Name the class *TodoContext* and click **Add**.</span></span>

<span data-ttu-id="65357-142">Замените класс на следующий код.</span><span class="sxs-lookup"><span data-stu-id="65357-142">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="65357-143">Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="65357-143">Add a controller</span></span>

<span data-ttu-id="65357-144">В обозревателе решений щелкните папку *Контроллеры* правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="65357-144">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="65357-145">Выберите **Добавить** > **Новый объект**.</span><span class="sxs-lookup"><span data-stu-id="65357-145">Select **Add** > **New Item**.</span></span> <span data-ttu-id="65357-146">В диалоговом окне **Добавить новый элемент** выберите шаблон **Класс контроллера API**.</span><span class="sxs-lookup"><span data-stu-id="65357-146">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span> <span data-ttu-id="65357-147">Назовите класс *TodoController* и нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="65357-147">Name the class *TodoController*, and click **Add**.</span></span>

![Диалоговое окно добавления элемента с контроллером в поле поиска и выбранным контроллером веб-API](first-web-api/_static/new_controller.png)

<span data-ttu-id="65357-149">Замените класс на следующий код.</span><span class="sxs-lookup"><span data-stu-id="65357-149">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="65357-150">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="65357-150">Launch the app</span></span>

<span data-ttu-id="65357-151">В Visual Studio нажмите клавиши CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="65357-151">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="65357-152">Visual Studio запустит браузер и перейдет к `http://localhost:<port>/api/values`, где `<port>` — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="65357-152">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="65357-153">Перейдите к контроллеру `Todo` по адресу `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="65357-153">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
