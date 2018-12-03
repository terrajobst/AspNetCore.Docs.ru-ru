---
title: Создание веб-API с помощью ASP.NET Core и Visual Studio
author: rick-anderson
description: Создание веб-API с помощью MVC ASP.NET Core и Visual Studio в Windows
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: tutorials/first-web-api
ms.openlocfilehash: eb23d5376742e04712f714263815582fddc5d18e
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450701"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio"></a><span data-ttu-id="f3ff4-103">Создание веб-API с помощью ASP.NET Core и Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f3ff4-103">Create a Web API with ASP.NET Core and Visual Studio</span></span>

<span data-ttu-id="f3ff4-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Майк Уоссон](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="f3ff4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="f3ff4-105">Это руководство по созданию веб-API для управления списком дел.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="f3ff4-106">Пользовательский интерфейс (UI) не создаётся.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="f3ff4-107">Существуют три версии этого руководства:</span><span class="sxs-lookup"><span data-stu-id="f3ff4-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="f3ff4-108">Windows: создание веб-API с помощью Visual Studio в Windows (см. это руководство в [видеоверсии](https://www.youtube.com/watch?v=TTkhEyGBfAk))</span><span class="sxs-lookup"><span data-stu-id="f3ff4-108">Windows: Web API with Visual Studio on Windows (This tutorial, see the [video version](https://www.youtube.com/watch?v=TTkhEyGBfAk))</span></span>
* <span data-ttu-id="f3ff4-109">macOS: [создание веб-API с помощью Visual Studio для Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="f3ff4-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="f3ff4-110">macOS, Linux, Windows: [создание веб-API с помощью Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="f3ff4-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

> [!NOTE]
> <span data-ttu-id="f3ff4-111">Мы тестируем удобство использования новой предлагаемой структуры оглавления для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-111">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="f3ff4-112">Если у вас есть несколько минут, и вы хотите попробовать найти семь разных тем в существующей или планируемой структуре оглавления, [щелкните здесь, чтобы принять участие в исследовании](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span><span class="sxs-lookup"><span data-stu-id="f3ff4-112">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span></span>

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="f3ff4-113">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="f3ff4-113">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="f3ff4-114">Создание проекта</span><span class="sxs-lookup"><span data-stu-id="f3ff4-114">Create the project</span></span>

<span data-ttu-id="f3ff4-115">Выполните следующие действия в Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="f3ff4-115">Follow these steps in Visual Studio:</span></span>

* <span data-ttu-id="f3ff4-116">В меню **Файл** выберите пункт **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-116">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f3ff4-117">Выберите шаблон **Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-117">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="f3ff4-118">Назовите проект *TodoApi* и нажмите **OK**.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-118">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="f3ff4-119">В диалоговом окне **Новое веб-приложение ASP.NET Core — TodoApi** выберите версию ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-119">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="f3ff4-120">Выберите шаблон **API** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-120">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="f3ff4-121">**Не** выбирайте **Включение поддержки Docker**.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-121">Do **not** select **Enable Docker Support**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="f3ff4-122">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="f3ff4-122">Launch the app</span></span>

<span data-ttu-id="f3ff4-123">В Visual Studio нажмите клавиши CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-123">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="f3ff4-124">Visual Studio запустит браузер и перейдет к `http://localhost:<port>/api/values`, где `<port>` — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-124">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="f3ff4-125">В Chrome, Microsoft Edge и Firefox отобразится следующая информация.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-125">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="f3ff4-126">При использовании Internet Explorer будет предложено сохранить файл *values.json*.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-126">If using Internet Explorer, you'll be prompted to save a *values.json* file.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="f3ff4-127">Добавление класса модели</span><span class="sxs-lookup"><span data-stu-id="f3ff4-127">Add a model class</span></span>

<span data-ttu-id="f3ff4-128">Модель — это объект, представляющий данные в приложении.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-128">A model is an object representing the data in the app.</span></span> <span data-ttu-id="f3ff4-129">В этом случае единственной моделью является задача.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-129">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="f3ff4-130">В обозревателе решений щелкните проект правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-130">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="f3ff4-131">Выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-131">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="f3ff4-132">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-132">Name the folder *Models*.</span></span>

> [!NOTE]
> <span data-ttu-id="f3ff4-133">Классы модели могут переходить в любое место в проекте.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-133">The model classes can go anywhere in the project.</span></span> <span data-ttu-id="f3ff4-134">Папка *Модели* используется по соглашению о классах модели.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-134">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="f3ff4-135">В обозревателе решений щелкните папку *Models* правой кнопкой мыши и выберите команду **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-135">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="f3ff4-136">Назовите класс *TodoItem* и нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-136">Name the class *TodoItem* and click **Add**.</span></span>

<span data-ttu-id="f3ff4-137">Обновите класс `TodoItem` с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="f3ff4-137">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="f3ff4-138">После создания `TodoItem` база данных формирует `Id`.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-138">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="f3ff4-139">Создание контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="f3ff4-139">Create the database context</span></span>

<span data-ttu-id="f3ff4-140">*Контекст базы данных* —это основной класс, который координирует функциональные возможности Entity Framework для заданной модели данных.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-140">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="f3ff4-141">Этот класс является производным от класса `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-141">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="f3ff4-142">В обозревателе решений щелкните папку *Models* правой кнопкой мыши и выберите команду **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-142">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="f3ff4-143">Назовите класс *TodoContext* и нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-143">Name the class *TodoContext* and click **Add**.</span></span>

<span data-ttu-id="f3ff4-144">Замените класс на следующий код.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-144">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="f3ff4-145">Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="f3ff4-145">Add a controller</span></span>

<span data-ttu-id="f3ff4-146">В обозревателе решений щелкните папку *Контроллеры* правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-146">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="f3ff4-147">Выберите **Добавить** > **Новый объект**.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-147">Select **Add** > **New Item**.</span></span> <span data-ttu-id="f3ff4-148">В диалоговом окне **Добавить новый элемент** выберите шаблон **Класс контроллера API**.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-148">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span> <span data-ttu-id="f3ff4-149">Назовите класс *TodoController* и нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-149">Name the class *TodoController*, and click **Add**.</span></span>

![Диалоговое окно добавления элемента с контроллером в поле поиска и выбранным контроллером веб-API](first-web-api/_static/new_controller.png)

<span data-ttu-id="f3ff4-151">Замените класс на следующий код.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-151">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="f3ff4-152">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="f3ff4-152">Launch the app</span></span>

<span data-ttu-id="f3ff4-153">В Visual Studio нажмите клавиши CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-153">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="f3ff4-154">Visual Studio запустит браузер и перейдет к `http://localhost:<port>/api/values`, где `<port>` — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-154">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="f3ff4-155">Перейдите к контроллеру `Todo` по адресу `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="f3ff4-155">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
