---
title: Учебник. Создание веб-API с помощью ASP.NET Core
author: rick-anderson
description: Узнайте, как создать веб-API с помощью ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 08/27/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 25bfccb136d875b454034bd011828c9f3b6cd3d8
ms.sourcegitcommit: de17150e5ec7507d7114dde0e5dbc2e45a66ef53
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/28/2019
ms.locfileid: "70113287"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="a28e0-103">Учебник. Создание веб-API с помощью ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a28e0-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="a28e0-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Майк Уоссон](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="a28e0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="a28e0-105">В этом учебнике приводятся основные сведения о создании веб-API с помощью ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a28e0-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a28e0-106">В этом руководстве вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="a28e0-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a28e0-107">Создание проекта веб-API.</span><span class="sxs-lookup"><span data-stu-id="a28e0-107">Create a web API project.</span></span>
> * <span data-ttu-id="a28e0-108">Добавление класса модели и контекста базы данных.</span><span class="sxs-lookup"><span data-stu-id="a28e0-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="a28e0-109">Формирование шаблонов контроллера с использованием методов CRUD.</span><span class="sxs-lookup"><span data-stu-id="a28e0-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="a28e0-110">Настройка маршрутизации, URL-пути и возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="a28e0-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="a28e0-111">Вызов веб-API с помощью Postman.</span><span class="sxs-lookup"><span data-stu-id="a28e0-111">Call the web API with Postman.</span></span>

<span data-ttu-id="a28e0-112">В итоге вы получите веб-API, позволяющий работать с элементами списка дел, хранимыми в базе данных.</span><span class="sxs-lookup"><span data-stu-id="a28e0-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="a28e0-113">Обзор</span><span class="sxs-lookup"><span data-stu-id="a28e0-113">Overview</span></span>

<span data-ttu-id="a28e0-114">В этом руководстве создается следующий API-интерфейс:</span><span class="sxs-lookup"><span data-stu-id="a28e0-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="a28e0-115">API</span><span class="sxs-lookup"><span data-stu-id="a28e0-115">API</span></span> | <span data-ttu-id="a28e0-116">ОПИСАНИЕ</span><span class="sxs-lookup"><span data-stu-id="a28e0-116">Description</span></span> | <span data-ttu-id="a28e0-117">Текст запроса</span><span class="sxs-lookup"><span data-stu-id="a28e0-117">Request body</span></span> | <span data-ttu-id="a28e0-118">Текст ответа</span><span class="sxs-lookup"><span data-stu-id="a28e0-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="a28e0-119">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="a28e0-119">GET /api/TodoItems</span></span> | <span data-ttu-id="a28e0-120">Получение всех элементов задач</span><span class="sxs-lookup"><span data-stu-id="a28e0-120">Get all to-do items</span></span> | <span data-ttu-id="a28e0-121">Нет</span><span class="sxs-lookup"><span data-stu-id="a28e0-121">None</span></span> | <span data-ttu-id="a28e0-122">Массив элементов задач</span><span class="sxs-lookup"><span data-stu-id="a28e0-122">Array of to-do items</span></span>|
|<span data-ttu-id="a28e0-123">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="a28e0-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="a28e0-124">Получение объекта по идентификатору</span><span class="sxs-lookup"><span data-stu-id="a28e0-124">Get an item by ID</span></span> | <span data-ttu-id="a28e0-125">Нет</span><span class="sxs-lookup"><span data-stu-id="a28e0-125">None</span></span> | <span data-ttu-id="a28e0-126">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="a28e0-126">To-do item</span></span>|
|<span data-ttu-id="a28e0-127">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="a28e0-127">POST /api/TodoItems</span></span> | <span data-ttu-id="a28e0-128">Добавление нового элемента</span><span class="sxs-lookup"><span data-stu-id="a28e0-128">Add a new item</span></span> | <span data-ttu-id="a28e0-129">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="a28e0-129">To-do item</span></span> | <span data-ttu-id="a28e0-130">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="a28e0-130">To-do item</span></span> |
|<span data-ttu-id="a28e0-131">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="a28e0-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="a28e0-132">Обновление существующего элемента &nbsp;</span><span class="sxs-lookup"><span data-stu-id="a28e0-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="a28e0-133">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="a28e0-133">To-do item</span></span> | <span data-ttu-id="a28e0-134">Нет</span><span class="sxs-lookup"><span data-stu-id="a28e0-134">None</span></span> |
|<span data-ttu-id="a28e0-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="a28e0-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="a28e0-136">Удаление элемента &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="a28e0-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="a28e0-137">Нет</span><span class="sxs-lookup"><span data-stu-id="a28e0-137">None</span></span> | <span data-ttu-id="a28e0-138">Нет</span><span class="sxs-lookup"><span data-stu-id="a28e0-138">None</span></span>|

<span data-ttu-id="a28e0-139">На следующем рисунке показана структура приложения.</span><span class="sxs-lookup"><span data-stu-id="a28e0-139">The following diagram shows the design of the app.</span></span>

![Клиент представлен прямоугольником слева.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="a28e0-145">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="a28e0-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a28e0-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a28e0-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a28e0-147">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a28e0-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a28e0-148">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="a28e0-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="a28e0-149">Создайте веб-проект.</span><span class="sxs-lookup"><span data-stu-id="a28e0-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a28e0-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a28e0-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a28e0-151">В меню **Файл** выберите **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="a28e0-152">Выберите шаблон **Веб-приложение ASP.NET Core** и нажмите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="a28e0-153">Назовите проект *TodoApi* и нажмите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="a28e0-154">В диалоговом окне **Создание веб-приложения ASP.NET Core** убедитесь в том, что выбраны платформы **.NET Core** и **ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span> <span data-ttu-id="a28e0-155">Выберите шаблон **API** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-155">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="a28e0-156">**Не** выбирайте **Включение поддержки Docker**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-156">**Don't** select **Enable Docker Support**.</span></span>

![Диалоговое окно создания проекта VS](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a28e0-158">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a28e0-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a28e0-159">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="a28e0-159">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="a28e0-160">Смените каталог (`cd`) на папку, в которой будет содержаться папка проекта.</span><span class="sxs-lookup"><span data-stu-id="a28e0-160">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="a28e0-161">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="a28e0-161">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   cd TodoAPI
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
   dotnet add package Microsoft.EntityFrameworkCore.InMemory --version 3.0.0-*
   code -r ../TodoApi
   ```

* <span data-ttu-id="a28e0-162">При появлении диалогового окна с запросом на добавление в проект необходимых ресурсов выберите **Да**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-162">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="a28e0-163">Предыдущие команды:</span><span class="sxs-lookup"><span data-stu-id="a28e0-163">The preceding commands:</span></span>

  * <span data-ttu-id="a28e0-164">Создает новый проект веб-API и открывает его в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a28e0-164">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="a28e0-165">Добавляет пакеты NuGet, которые понадобятся в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="a28e0-165">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a28e0-166">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="a28e0-166">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a28e0-167">Выберите **Файл** > **Новое решение**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-167">Select **File** > **New Solution**.</span></span>

  ![Новое решение macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="a28e0-169">Выберите **.NET Core** > **Приложение** > **API** > **Далее**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-169">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![Диалоговое окно "Новый проект" в macOS](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="a28e0-171">В диалоговом окне **Настройка нового веб-API ASP.NET Core** в качестве **Целевой платформы** выберите \* *.NET Core 3.0*.</span><span class="sxs-lookup"><span data-stu-id="a28e0-171">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.0*.</span></span>

* <span data-ttu-id="a28e0-172">Введите *TodoApi* в поле **Имя проекта** и выберите команду **Создать**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-172">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Диалоговое окно конфигурации](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="a28e0-174">Откройте командный терминал в папке проекта и выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="a28e0-174">Open a command terminal in the project folder and run the following commands:</span></span>

   ```console
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
   dotnet add package Microsoft.EntityFrameworkCore.InMemory --version 3.0.0-*
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="a28e0-175">Тестирование API</span><span class="sxs-lookup"><span data-stu-id="a28e0-175">Test the API</span></span>

<span data-ttu-id="a28e0-176">Шаблон проекта создает API `WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="a28e0-176">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="a28e0-177">Вызовите метод `Get` из браузера для тестирования приложения.</span><span class="sxs-lookup"><span data-stu-id="a28e0-177">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a28e0-178">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a28e0-178">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a28e0-179">Нажмите клавиши CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="a28e0-179">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="a28e0-180">Visual Studio запустит браузер и перейдет к `https://localhost:<port>/WeatherForecast`, где `<port>` — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="a28e0-180">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="a28e0-181">Если появится диалоговое окно с запросом о необходимости доверять сертификату IIS Express, выберите **Да**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-181">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="a28e0-182">В появляющемся следом диалоговом окне **Предупреждение системы безопасности** выберите **Да**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-182">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a28e0-183">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a28e0-183">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a28e0-184">Нажмите клавиши CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="a28e0-184">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="a28e0-185">В браузере перейдите по следующему URL-адресу: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="a28e0-185">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a28e0-186">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="a28e0-186">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="a28e0-187">Выберите **Выполнить** > **Начать отладку**, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="a28e0-187">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="a28e0-188">Visual Studio для Mac запустит браузер и перейдет к `https://localhost:<port>`, где `<port>` — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="a28e0-188">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="a28e0-189">Появится ошибка HTTP 404 (Не найдено).</span><span class="sxs-lookup"><span data-stu-id="a28e0-189">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="a28e0-190">Добавьте `/WeatherForecast` к URL-адресу (измените URL-адрес на `https://localhost:<port>/WeatherForecast`).</span><span class="sxs-lookup"><span data-stu-id="a28e0-190">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="a28e0-191">Возвращаемые данные JSON будут выглядеть примерно так:</span><span class="sxs-lookup"><span data-stu-id="a28e0-191">JSON similar to the following is returned:</span></span>

```json
[
    {
        "date": "2019-07-16T19:04:05.7257911-06:00",
        "temperatureC": 52,
        "temperatureF": 125,
        "summary": "Mild"
    },
    {
        "date": "2019-07-17T19:04:05.7258461-06:00",
        "temperatureC": 36,
        "temperatureF": 96,
        "summary": "Warm"
    },
    {
        "date": "2019-07-18T19:04:05.7258467-06:00",
        "temperatureC": 39,
        "temperatureF": 102,
        "summary": "Cool"
    },
    {
        "date": "2019-07-19T19:04:05.7258471-06:00",
        "temperatureC": 10,
        "temperatureF": 49,
        "summary": "Bracing"
    },
    {
        "date": "2019-07-20T19:04:05.7258474-06:00",
        "temperatureC": -1,
        "temperatureF": 31,
        "summary": "Chilly"
    }
]
```

## <a name="add-a-model-class"></a><span data-ttu-id="a28e0-192">Добавление класса модели</span><span class="sxs-lookup"><span data-stu-id="a28e0-192">Add a model class</span></span>

<span data-ttu-id="a28e0-193">*Модель* — это набор классов, представляющих данные, которыми управляет приложение.</span><span class="sxs-lookup"><span data-stu-id="a28e0-193">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="a28e0-194">Модель этого приложения содержит единственный класс `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="a28e0-194">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a28e0-195">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a28e0-195">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a28e0-196">В **обозревателе решений** щелкните проект правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="a28e0-196">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="a28e0-197">Выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-197">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="a28e0-198">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="a28e0-198">Name the folder *Models*.</span></span>

* <span data-ttu-id="a28e0-199">Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-199">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="a28e0-200">Присвойте классу имя *TodoItem* и выберите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-200">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="a28e0-201">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="a28e0-201">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a28e0-202">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a28e0-202">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a28e0-203">Добавьте папку с именем *Models*.</span><span class="sxs-lookup"><span data-stu-id="a28e0-203">Add a folder named *Models*.</span></span>

* <span data-ttu-id="a28e0-204">Добавьте класс `TodoItem` в папку *Models*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="a28e0-204">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a28e0-205">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="a28e0-205">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a28e0-206">Щелкните проект правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="a28e0-206">Right-click the project.</span></span> <span data-ttu-id="a28e0-207">Выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-207">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="a28e0-208">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="a28e0-208">Name the folder *Models*.</span></span>

  ![Новая папка](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="a28e0-210">Правой кнопкой мыши щелкните папку *Models* и выберите **Добавить** > **Новый файл** > **Общие** > **Пустой класс**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-210">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="a28e0-211">Назовите класс *TodoItem* и нажмите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-211">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="a28e0-212">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="a28e0-212">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="a28e0-213">Свойство `Id` выступает в качестве уникального ключа реляционной базы данных.</span><span class="sxs-lookup"><span data-stu-id="a28e0-213">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="a28e0-214">Классы моделей можно размещать в любом месте проекта, но обычно для этого используется папка *Models*.</span><span class="sxs-lookup"><span data-stu-id="a28e0-214">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="a28e0-215">Добавление контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="a28e0-215">Add a database context</span></span>

<span data-ttu-id="a28e0-216">*Контекст базы данных* —это основной класс, который координирует функциональные возможности Entity Framework для модели данных.</span><span class="sxs-lookup"><span data-stu-id="a28e0-216">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="a28e0-217">Этот класс является производным от класса `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="a28e0-217">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a28e0-218">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a28e0-218">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="a28e0-219">Добавление Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="a28e0-219">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="a28e0-220">В меню **Сервис** выберите **Диспетчер пакетов NuGet > Управление пакетами NuGet для решения**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-220">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="a28e0-221">Установите флажок **Включить предварительные выпуски**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-221">Select the **Include prerelease** checkbox.</span></span>
* <span data-ttu-id="a28e0-222">Перейдите на вкладку **Обзор** и введите **Microsoft.EntityFrameworkCore.SqlServer** в поле поиска.</span><span class="sxs-lookup"><span data-stu-id="a28e0-222">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="a28e0-223">В панели слева выберите **Microsoft.EntityFrameworkCore.SqlServer V3.0.0-preview**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-223">Select  **Microsoft.EntityFrameworkCore.SqlServer V3.0.0-preview** in the left pane.</span></span>
* <span data-ttu-id="a28e0-224">Установите флажок **Проект** на правой панели и выберите **Установить**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-224">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="a28e0-225">Добавьте пакет NuGet `Microsoft.EntityFrameworkCore.InMemory` согласно инструкциям выше.</span><span class="sxs-lookup"><span data-stu-id="a28e0-225">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![Диспетчер пакетов NuGet](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="a28e0-227">Добавление контекста базы данных TodoContext</span><span class="sxs-lookup"><span data-stu-id="a28e0-227">Add the TodoContext database context</span></span>

* <span data-ttu-id="a28e0-228">Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-228">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="a28e0-229">Назовите класс *TodoContext* и нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-229">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="a28e0-230">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="a28e0-230">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="a28e0-231">Добавьте класс `TodoContext` в папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="a28e0-231">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="a28e0-232">Введите следующий код:</span><span class="sxs-lookup"><span data-stu-id="a28e0-232">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="a28e0-233">Регистрация контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="a28e0-233">Register the database context</span></span>

<span data-ttu-id="a28e0-234">В ASP.NET Core службы (такие как контекст базы данных) должны быть зарегистрированы с помощью контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a28e0-234">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="a28e0-235">Контейнер предоставляет службу контроллерам.</span><span class="sxs-lookup"><span data-stu-id="a28e0-235">The container provides the service to controllers.</span></span>

<span data-ttu-id="a28e0-236">Обновите файл *Startup.cs*, используя следующий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="a28e0-236">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="a28e0-237">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="a28e0-237">The preceding code:</span></span>

* <span data-ttu-id="a28e0-238">Удалите неиспользуемые объявления `using`.</span><span class="sxs-lookup"><span data-stu-id="a28e0-238">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="a28e0-239">Добавляет контекст базы данных в контейнер внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="a28e0-239">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="a28e0-240">Указывает, что контекст базы данных будет использовать базу данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="a28e0-240">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="a28e0-241">Формирование шаблонов контроллера</span><span class="sxs-lookup"><span data-stu-id="a28e0-241">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a28e0-242">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a28e0-242">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a28e0-243">Щелкните папку *Controllers* правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="a28e0-243">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="a28e0-244">Выберите **Добавить** > **Создать шаблонный элемент**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-244">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="a28e0-245">Выберите **Контроллер API с действиями, использующий Entity Framework**, а затем выберите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-245">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="a28e0-246">В диалоговом окне **Контроллер API с действиями, использующий Entity Framework** сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="a28e0-246">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="a28e0-247">Выберите **TodoItem (TodoAPI.Models)** в поле **Класс модели**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-247">Select **TodoItem (TodoAPI.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="a28e0-248">Выберите **TodoContext (TodoAPI.Models)** в поле **Класс контекста данных**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-248">Select **TodoContext (TodoAPI.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="a28e0-249">Нажмите **Добавить**</span><span class="sxs-lookup"><span data-stu-id="a28e0-249">Select **Add**</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="a28e0-250">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="a28e0-250">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="a28e0-251">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="a28e0-251">Run the following commands:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext  -outDir Controllers
```

<span data-ttu-id="a28e0-252">Предыдущие команды:</span><span class="sxs-lookup"><span data-stu-id="a28e0-252">The preceding commands:</span></span>

* <span data-ttu-id="a28e0-253">добавляют пакеты NuGet, необходимые для формирования шаблонов;</span><span class="sxs-lookup"><span data-stu-id="a28e0-253">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="a28e0-254">устанавливают подсистему формирования шаблонов (`dotnet-aspnet-codegenerator`);</span><span class="sxs-lookup"><span data-stu-id="a28e0-254">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="a28e0-255">формируют шаблоны для `TodoItemsController`.</span><span class="sxs-lookup"><span data-stu-id="a28e0-255">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="a28e0-256">Сформированный код:</span><span class="sxs-lookup"><span data-stu-id="a28e0-256">The generated code:</span></span>

* <span data-ttu-id="a28e0-257">Определяет класс контроллера API без методов.</span><span class="sxs-lookup"><span data-stu-id="a28e0-257">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="a28e0-258">Добавляет в класс атрибут [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="a28e0-258">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="a28e0-259">Этот атрибут указывает, что контроллер отвечает на запросы веб-API.</span><span class="sxs-lookup"><span data-stu-id="a28e0-259">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="a28e0-260">Дополнительные сведения о поведении, которое реализует этот атрибут, см. в <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="a28e0-260">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="a28e0-261">Использует внедрение зависимостей для внедрения контекста базы данных (`TodoContext`) в контроллер.</span><span class="sxs-lookup"><span data-stu-id="a28e0-261">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="a28e0-262">Контекст базы данных используется в каждом методе [создания, чтения, обновления и удаления](https://wikipedia.org/wiki/Create,_read,_update_and_delete) в контроллере.</span><span class="sxs-lookup"><span data-stu-id="a28e0-262">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="a28e0-263">Знакомство с методом создания PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="a28e0-263">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="a28e0-264">Измените инструкцию возврата в `PostTodoItem` и используйте оператор [nameof](/dotnet/csharp/language-reference/operators/nameof):</span><span class="sxs-lookup"><span data-stu-id="a28e0-264">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="a28e0-265">Предыдущий код является методом HTTP POST, как указывает атрибут [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="a28e0-265">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="a28e0-266">Этот метод получает значение элемента списка дел из текста HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="a28e0-266">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="a28e0-267">Метод <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*>:</span><span class="sxs-lookup"><span data-stu-id="a28e0-267">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="a28e0-268">В случае успеха возвращает код состояния HTTP 201.</span><span class="sxs-lookup"><span data-stu-id="a28e0-268">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="a28e0-269">HTTP 201 представляет собой стандартный ответ для метода HTTP POST, создающий ресурс на сервере.</span><span class="sxs-lookup"><span data-stu-id="a28e0-269">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="a28e0-270">Добавляет в ответ заголовок [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location).</span><span class="sxs-lookup"><span data-stu-id="a28e0-270">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="a28e0-271">Заголовок `Location` указывает [URI](https://developer.mozilla.org/docs/Glossary/URI) новой созданной задачи.</span><span class="sxs-lookup"><span data-stu-id="a28e0-271">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="a28e0-272">Дополнительные сведения см. в статье [10.2.2 201 "Создан ресурс"](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="a28e0-272">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="a28e0-273">Указывает действие `GetTodoItem` для создания URI заголовка `Location`.</span><span class="sxs-lookup"><span data-stu-id="a28e0-273">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="a28e0-274">Ключевое слово `nameof` C# используется для предотвращения жесткого программирования имени действия в вызове `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="a28e0-274">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="a28e0-275">Установка Postman</span><span class="sxs-lookup"><span data-stu-id="a28e0-275">Install Postman</span></span>

<span data-ttu-id="a28e0-276">В этом учебнике для тестирования веб-API используется Postman.</span><span class="sxs-lookup"><span data-stu-id="a28e0-276">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="a28e0-277">Установка [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="a28e0-277">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="a28e0-278">Запустите веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="a28e0-278">Start the web app.</span></span>
* <span data-ttu-id="a28e0-279">Запустите Postman.</span><span class="sxs-lookup"><span data-stu-id="a28e0-279">Start Postman.</span></span>
* <span data-ttu-id="a28e0-280">Отключение параметра **Проверка SSL-сертификата**</span><span class="sxs-lookup"><span data-stu-id="a28e0-280">Disable **SSL certificate verification**</span></span>
* <span data-ttu-id="a28e0-281">В меню **Файл > Параметры** (вкладка \**Общие*) отключите параметр **Проверка SSL-сертификата**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-281">From  **File > Settings** (\**General* tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="a28e0-282">После тестирования контроллера снова включите проверку SSL-сертификата.</span><span class="sxs-lookup"><span data-stu-id="a28e0-282">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="a28e0-283">Тестирование PostTodoItem с использованием Postman</span><span class="sxs-lookup"><span data-stu-id="a28e0-283">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="a28e0-284">Создайте новый запрос.</span><span class="sxs-lookup"><span data-stu-id="a28e0-284">Create a new request.</span></span>
* <span data-ttu-id="a28e0-285">Установите HTTP-метод `POST`.</span><span class="sxs-lookup"><span data-stu-id="a28e0-285">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="a28e0-286">Откройте вкладку **Тело**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-286">Select the **Body** tab.</span></span>
* <span data-ttu-id="a28e0-287">Установите переключатель **без обработки**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-287">Select the **raw** radio button.</span></span>
* <span data-ttu-id="a28e0-288">Задайте тип **JSON (приложение/json)** .</span><span class="sxs-lookup"><span data-stu-id="a28e0-288">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="a28e0-289">В теле запроса введите код JSON для элемента списка дел:</span><span class="sxs-lookup"><span data-stu-id="a28e0-289">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="a28e0-290">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-290">Select **Send**.</span></span>

  ![Postman с запросом Create](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="a28e0-292">Тестирование URI заголовка расположения</span><span class="sxs-lookup"><span data-stu-id="a28e0-292">Test the location header URI</span></span>

* <span data-ttu-id="a28e0-293">Перейдите на вкладку **Заголовки** в области **Ответ**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-293">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="a28e0-294">Скопируйте значение заголовка **Расположение**:</span><span class="sxs-lookup"><span data-stu-id="a28e0-294">Copy the **Location** header value:</span></span>

  ![Вкладка "Заголовки" в консоли Postman](first-web-api/_static/3/create.png)

* <span data-ttu-id="a28e0-296">Укажите метод GET.</span><span class="sxs-lookup"><span data-stu-id="a28e0-296">Set the method to GET.</span></span>
* <span data-ttu-id="a28e0-297">Вставьте URI (например, `https://localhost:5001/api/TodoItems/1`)</span><span class="sxs-lookup"><span data-stu-id="a28e0-297">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`)</span></span>
* <span data-ttu-id="a28e0-298">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-298">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="a28e0-299">Знакомство с методами GET</span><span class="sxs-lookup"><span data-stu-id="a28e0-299">Examine the GET methods</span></span>

<span data-ttu-id="a28e0-300">Эти методы реализуют две конечные точки GET:</span><span class="sxs-lookup"><span data-stu-id="a28e0-300">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="a28e0-301">Протестируйте приложение, вызвав эти две конечные точки в браузере или в Postman.</span><span class="sxs-lookup"><span data-stu-id="a28e0-301">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="a28e0-302">Например:</span><span class="sxs-lookup"><span data-stu-id="a28e0-302">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="a28e0-303">При вызове `GetTodoItems` возвращается примерно такой ответ:</span><span class="sxs-lookup"><span data-stu-id="a28e0-303">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="a28e0-304">Тестирование Get с использованием Postman</span><span class="sxs-lookup"><span data-stu-id="a28e0-304">Test Get with Postman</span></span>

* <span data-ttu-id="a28e0-305">Создайте новый запрос.</span><span class="sxs-lookup"><span data-stu-id="a28e0-305">Create a new request.</span></span>
* <span data-ttu-id="a28e0-306">Укажите метод HTTP **GET**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-306">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="a28e0-307">Укажите URL-адрес запроса `https://localhost:<port>/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="a28e0-307">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="a28e0-308">Например, `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="a28e0-308">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="a28e0-309">Выберите режим **Представление с двумя областями** в Postman.</span><span class="sxs-lookup"><span data-stu-id="a28e0-309">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="a28e0-310">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-310">Select **Send**.</span></span>

<span data-ttu-id="a28e0-311">Это приложение использует выполняющуюся в памяти базу данных.</span><span class="sxs-lookup"><span data-stu-id="a28e0-311">This app uses an in-memory database.</span></span> <span data-ttu-id="a28e0-312">Если остановить и вновь запустить его, предшествующий запрос GET не возвратит никаких данных.</span><span class="sxs-lookup"><span data-stu-id="a28e0-312">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="a28e0-313">Если данные не возвращаются, данные для приложения получаются методом [POST](#post).</span><span class="sxs-lookup"><span data-stu-id="a28e0-313">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="a28e0-314">Маршрутизация и пути URL</span><span class="sxs-lookup"><span data-stu-id="a28e0-314">Routing and URL paths</span></span>

<span data-ttu-id="a28e0-315">Атрибут [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) обозначает метод, который отвечает на запрос HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="a28e0-315">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="a28e0-316">Путь URL для каждого метода формируется следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a28e0-316">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="a28e0-317">Возьмите строку шаблона в атрибуте `Route` контроллера:</span><span class="sxs-lookup"><span data-stu-id="a28e0-317">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="a28e0-318">Замените `[controller]` именем контроллера (по соглашению это имя класса контроллера без суффикса "Controller").</span><span class="sxs-lookup"><span data-stu-id="a28e0-318">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="a28e0-319">В этом примере класс контроллера имеет имя **TodoItems**, а сам контроллер, соответственно, — "TodoItems".</span><span class="sxs-lookup"><span data-stu-id="a28e0-319">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="a28e0-320">В ASP.NET Core [маршрутизация](xref:mvc/controllers/routing) реализуется без учета регистра символов.</span><span class="sxs-lookup"><span data-stu-id="a28e0-320">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="a28e0-321">Если атрибут `[HttpGet]` имеет шаблон маршрута (например, `[HttpGet("products")]`), добавьте его к пути.</span><span class="sxs-lookup"><span data-stu-id="a28e0-321">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="a28e0-322">В этом примере шаблон не используется.</span><span class="sxs-lookup"><span data-stu-id="a28e0-322">This sample doesn't use a template.</span></span> <span data-ttu-id="a28e0-323">Дополнительные сведения см. в разделе [Маршрутизация атрибутов с помощью атрибутов Http[Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="a28e0-323">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="a28e0-324">В следующем методе `GetTodoItem` `"{id}"` — это переменная-заполнитель для уникального идентификатора элемента задачи.</span><span class="sxs-lookup"><span data-stu-id="a28e0-324">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="a28e0-325">При вызове `GetTodoItem` параметру метода `id` присваивается значение `"{id}"` в URL-адресе.</span><span class="sxs-lookup"><span data-stu-id="a28e0-325">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="a28e0-326">Возвращаемые значения</span><span class="sxs-lookup"><span data-stu-id="a28e0-326">Return values</span></span>

<span data-ttu-id="a28e0-327">Возвращаемое значение имеет тип `GetTodoItems`, а метод `GetTodoItem` имеет тип [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="a28e0-327">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="a28e0-328">ASP.NET Core автоматически сериализует объект в формат [JSON](https://www.json.org/) и записывает данные JSON в тело сообщения ответа.</span><span class="sxs-lookup"><span data-stu-id="a28e0-328">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="a28e0-329">Код ответа для этого типа возвращаемого значения равен 200, что свидетельствует об отсутствии необработанных исключений.</span><span class="sxs-lookup"><span data-stu-id="a28e0-329">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="a28e0-330">Необработанные исключения преобразуются в ошибки 5xx.</span><span class="sxs-lookup"><span data-stu-id="a28e0-330">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="a28e0-331">Типы возвращаемых значений `ActionResult` могут представлять широкий спектр кодов состояний HTTP.</span><span class="sxs-lookup"><span data-stu-id="a28e0-331">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="a28e0-332">Например, метод `GetTodoItem` может возвращать два разных значения состояния:</span><span class="sxs-lookup"><span data-stu-id="a28e0-332">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="a28e0-333">Если запрошенному идентификатору не соответствует ни один элемент, метод возвращает ошибку 404 ([Не найдено](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound)).</span><span class="sxs-lookup"><span data-stu-id="a28e0-333">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="a28e0-334">В противном случае метод возвращает код 200 с телом ответа JSON.</span><span class="sxs-lookup"><span data-stu-id="a28e0-334">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="a28e0-335">При возвращении `item` возвращается ответ HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="a28e0-335">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="a28e0-336">Метод PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="a28e0-336">The PutTodoItem method</span></span>

<span data-ttu-id="a28e0-337">Проверьте метод `PutTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="a28e0-337">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="a28e0-338">Страница `PutTodoItem` аналогична странице `PostTodoItem`, но использует запрос HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="a28e0-338">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="a28e0-339">Ответ — [204 (Нет содержимого)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="a28e0-339">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="a28e0-340">Согласно спецификации HTTP, запрос PUT требует, чтобы клиент отправлял всю обновленную сущность, а не только изменения.</span><span class="sxs-lookup"><span data-stu-id="a28e0-340">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="a28e0-341">Чтобы обеспечить поддержку частичных обновлений, используйте [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="a28e0-341">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="a28e0-342">Если возникнет ошибка вызова `PutTodoItem`, вызовите `GET`, чтобы в базе данных был один элемент.</span><span class="sxs-lookup"><span data-stu-id="a28e0-342">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="a28e0-343">Тестирование метода PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="a28e0-343">Test the PutTodoItem method</span></span>

<span data-ttu-id="a28e0-344">Этот пример использует базу данных в памяти, которая должна быть инициирована при каждом запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="a28e0-344">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="a28e0-345">При выполнении вызова PUT в базе данных уже должен существовать какой-либо элемент.</span><span class="sxs-lookup"><span data-stu-id="a28e0-345">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="a28e0-346">Для этого перед вызовом PUT выполните вызов GET, чтобы убедиться в наличии такого элемента в базе данных.</span><span class="sxs-lookup"><span data-stu-id="a28e0-346">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="a28e0-347">Обновите элемент списка дел с идентификатором 1 и присвойте ему имя "feed fish":</span><span class="sxs-lookup"><span data-stu-id="a28e0-347">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="a28e0-348">На следующем рисунке показан процесс обновления Postman:</span><span class="sxs-lookup"><span data-stu-id="a28e0-348">The following image shows the Postman update:</span></span>

![Консоль Postman с ответом 204 (Нет содержимого)](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="a28e0-350">Метод DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="a28e0-350">The DeleteTodoItem method</span></span>

<span data-ttu-id="a28e0-351">Проверьте метод `DeleteTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="a28e0-351">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

<span data-ttu-id="a28e0-352">`DeleteTodoItem`Ответ[ — 204 (нет содержимого)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="a28e0-352">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="a28e0-353">Тестирование метода DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="a28e0-353">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="a28e0-354">Удалите элемент списка дел с помощью Postman:</span><span class="sxs-lookup"><span data-stu-id="a28e0-354">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="a28e0-355">Укажите метод `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="a28e0-355">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="a28e0-356">Укажите URI удаляемого объекта, например `https://localhost:5001/api/TodoItems/1`</span><span class="sxs-lookup"><span data-stu-id="a28e0-356">Set the URI of the object to delete, for example `https://localhost:5001/api/TodoItems/1`</span></span>
* <span data-ttu-id="a28e0-357">Нажмите кнопку **Отправить**</span><span class="sxs-lookup"><span data-stu-id="a28e0-357">Select **Send**</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="a28e0-358">Вызов веб-API с помощью JavaScript</span><span class="sxs-lookup"><span data-stu-id="a28e0-358">Call the web API with JavaScript</span></span>

<span data-ttu-id="a28e0-359">Пошаговые инструкции см. в [руководстве Вызовите веб-API ASP.NET Core с помощью JavaScript](xref:tutorials/web-api-javascript).</span><span class="sxs-lookup"><span data-stu-id="a28e0-359">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a28e0-360">В этом руководстве вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="a28e0-360">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a28e0-361">Создание проекта веб-API.</span><span class="sxs-lookup"><span data-stu-id="a28e0-361">Create a web API project.</span></span>
> * <span data-ttu-id="a28e0-362">Добавление класса модели и контекста базы данных.</span><span class="sxs-lookup"><span data-stu-id="a28e0-362">Add a model class and a database context.</span></span>
> * <span data-ttu-id="a28e0-363">Добавление контроллера.</span><span class="sxs-lookup"><span data-stu-id="a28e0-363">Add a controller.</span></span>
> * <span data-ttu-id="a28e0-364">Добавление методов CRUD.</span><span class="sxs-lookup"><span data-stu-id="a28e0-364">Add CRUD methods.</span></span>
> * <span data-ttu-id="a28e0-365">Настройка маршрутизации и путей URL.</span><span class="sxs-lookup"><span data-stu-id="a28e0-365">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="a28e0-366">Указание возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="a28e0-366">Specify return values.</span></span>
> * <span data-ttu-id="a28e0-367">Вызов веб-API с помощью Postman.</span><span class="sxs-lookup"><span data-stu-id="a28e0-367">Call the web API with Postman.</span></span>
> * <span data-ttu-id="a28e0-368">Вызовите веб-API с помощью JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a28e0-368">Call the web API with JavaScript.</span></span>

<span data-ttu-id="a28e0-369">В конечном итоге вы получите веб-API, обеспечивающий управление элементами списка дел, хранящимися в реляционной базе данных.</span><span class="sxs-lookup"><span data-stu-id="a28e0-369">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="a28e0-370">Обзор</span><span class="sxs-lookup"><span data-stu-id="a28e0-370">Overview</span></span>

<span data-ttu-id="a28e0-371">В этом руководстве создается следующий API-интерфейс:</span><span class="sxs-lookup"><span data-stu-id="a28e0-371">This tutorial creates the following API:</span></span>

|<span data-ttu-id="a28e0-372">API</span><span class="sxs-lookup"><span data-stu-id="a28e0-372">API</span></span> | <span data-ttu-id="a28e0-373">ОПИСАНИЕ</span><span class="sxs-lookup"><span data-stu-id="a28e0-373">Description</span></span> | <span data-ttu-id="a28e0-374">Текст запроса</span><span class="sxs-lookup"><span data-stu-id="a28e0-374">Request body</span></span> | <span data-ttu-id="a28e0-375">Текст ответа</span><span class="sxs-lookup"><span data-stu-id="a28e0-375">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="a28e0-376">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="a28e0-376">GET /api/TodoItems</span></span> | <span data-ttu-id="a28e0-377">Получение всех элементов задач</span><span class="sxs-lookup"><span data-stu-id="a28e0-377">Get all to-do items</span></span> | <span data-ttu-id="a28e0-378">Нет</span><span class="sxs-lookup"><span data-stu-id="a28e0-378">None</span></span> | <span data-ttu-id="a28e0-379">Массив элементов задач</span><span class="sxs-lookup"><span data-stu-id="a28e0-379">Array of to-do items</span></span>|
|<span data-ttu-id="a28e0-380">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="a28e0-380">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="a28e0-381">Получение объекта по идентификатору</span><span class="sxs-lookup"><span data-stu-id="a28e0-381">Get an item by ID</span></span> | <span data-ttu-id="a28e0-382">Нет</span><span class="sxs-lookup"><span data-stu-id="a28e0-382">None</span></span> | <span data-ttu-id="a28e0-383">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="a28e0-383">To-do item</span></span>|
|<span data-ttu-id="a28e0-384">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="a28e0-384">POST /api/TodoItems</span></span> | <span data-ttu-id="a28e0-385">Добавление нового элемента</span><span class="sxs-lookup"><span data-stu-id="a28e0-385">Add a new item</span></span> | <span data-ttu-id="a28e0-386">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="a28e0-386">To-do item</span></span> | <span data-ttu-id="a28e0-387">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="a28e0-387">To-do item</span></span> |
|<span data-ttu-id="a28e0-388">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="a28e0-388">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="a28e0-389">Обновление существующего элемента &nbsp;</span><span class="sxs-lookup"><span data-stu-id="a28e0-389">Update an existing item &nbsp;</span></span> | <span data-ttu-id="a28e0-390">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="a28e0-390">To-do item</span></span> | <span data-ttu-id="a28e0-391">Нет</span><span class="sxs-lookup"><span data-stu-id="a28e0-391">None</span></span> |
|<span data-ttu-id="a28e0-392">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="a28e0-392">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="a28e0-393">Удаление элемента &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="a28e0-393">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="a28e0-394">Нет</span><span class="sxs-lookup"><span data-stu-id="a28e0-394">None</span></span> | <span data-ttu-id="a28e0-395">Нет</span><span class="sxs-lookup"><span data-stu-id="a28e0-395">None</span></span>|

<span data-ttu-id="a28e0-396">На следующем рисунке показана структура приложения.</span><span class="sxs-lookup"><span data-stu-id="a28e0-396">The following diagram shows the design of the app.</span></span>

![Клиент представлен прямоугольником слева.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="a28e0-402">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="a28e0-402">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a28e0-403">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a28e0-403">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a28e0-404">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a28e0-404">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a28e0-405">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="a28e0-405">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="a28e0-406">Создайте веб-проект.</span><span class="sxs-lookup"><span data-stu-id="a28e0-406">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a28e0-407">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a28e0-407">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a28e0-408">В меню **Файл** выберите **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-408">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="a28e0-409">Выберите шаблон **Веб-приложение ASP.NET Core** и нажмите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-409">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="a28e0-410">Назовите проект *TodoApi* и нажмите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-410">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="a28e0-411">В диалоговом окне **Создание веб-приложения ASP.NET Core** убедитесь в том, что выбраны платформы **.NET Core** и **ASP.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-411">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="a28e0-412">Выберите шаблон **API** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-412">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="a28e0-413">**Не** выбирайте **Включение поддержки Docker**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-413">**Don't** select **Enable Docker Support**.</span></span>

![Диалоговое окно создания проекта VS](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a28e0-415">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a28e0-415">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a28e0-416">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="a28e0-416">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="a28e0-417">Смените каталог (`cd`) на папку, в которой будет содержаться папка проекта.</span><span class="sxs-lookup"><span data-stu-id="a28e0-417">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="a28e0-418">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="a28e0-418">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="a28e0-419">С помощью этих команд создается новый проект веб-API и открывается новый экземпляр Visual Studio Code в новой папке проекта.</span><span class="sxs-lookup"><span data-stu-id="a28e0-419">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="a28e0-420">При появлении диалогового окна с запросом на добавление в проект необходимых ресурсов выберите **Да**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-420">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a28e0-421">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="a28e0-421">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a28e0-422">Выберите **Файл** > **Новое решение**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-422">Select **File** > **New Solution**.</span></span>

  ![Новое решение macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="a28e0-424">Выберите **.NET Core** > **Приложение** > **API** > **Далее**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-424">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![Диалоговое окно "Новый проект" в macOS](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="a28e0-426">В диалоговом окне **Настройка нового веб-API ASP.NET Core** оставьте установленное по умолчанию значение для параметра **Целевая платформа**, то есть \* *.NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="a28e0-426">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="a28e0-427">Введите *TodoApi* в поле **Имя проекта** и выберите команду **Создать**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-427">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Диалоговое окно конфигурации](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="a28e0-429">Тестирование API</span><span class="sxs-lookup"><span data-stu-id="a28e0-429">Test the API</span></span>

<span data-ttu-id="a28e0-430">Шаблон проекта создает API `values`.</span><span class="sxs-lookup"><span data-stu-id="a28e0-430">The project template creates a `values` API.</span></span> <span data-ttu-id="a28e0-431">Вызовите метод `Get` из браузера для тестирования приложения.</span><span class="sxs-lookup"><span data-stu-id="a28e0-431">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a28e0-432">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a28e0-432">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a28e0-433">Нажмите клавиши CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="a28e0-433">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="a28e0-434">Visual Studio запустит браузер и перейдет к `https://localhost:<port>/api/values`, где `<port>` — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="a28e0-434">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="a28e0-435">Если появится диалоговое окно с запросом о необходимости доверять сертификату IIS Express, выберите **Да**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-435">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="a28e0-436">В появляющемся следом диалоговом окне **Предупреждение системы безопасности** выберите **Да**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-436">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a28e0-437">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a28e0-437">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a28e0-438">Нажмите клавиши CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="a28e0-438">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="a28e0-439">В браузере перейдите по следующему URL-адресу: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="a28e0-439">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a28e0-440">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="a28e0-440">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="a28e0-441">Выберите **Выполнить** > **Начать отладку**, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="a28e0-441">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="a28e0-442">Visual Studio для Mac запустит браузер и перейдет к `https://localhost:<port>`, где `<port>` — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="a28e0-442">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="a28e0-443">Появится ошибка HTTP 404 (Не найдено).</span><span class="sxs-lookup"><span data-stu-id="a28e0-443">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="a28e0-444">Добавьте `/api/values` к URL-адресу (измените URL-адрес на `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="a28e0-444">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="a28e0-445">Возвращается следующий файл JSON:</span><span class="sxs-lookup"><span data-stu-id="a28e0-445">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="a28e0-446">Добавление класса модели</span><span class="sxs-lookup"><span data-stu-id="a28e0-446">Add a model class</span></span>

<span data-ttu-id="a28e0-447">*Модель* — это набор классов, представляющих данные, которыми управляет приложение.</span><span class="sxs-lookup"><span data-stu-id="a28e0-447">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="a28e0-448">Модель этого приложения содержит единственный класс `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="a28e0-448">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a28e0-449">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a28e0-449">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a28e0-450">В **обозревателе решений** щелкните проект правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="a28e0-450">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="a28e0-451">Выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-451">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="a28e0-452">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="a28e0-452">Name the folder *Models*.</span></span>

* <span data-ttu-id="a28e0-453">Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-453">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="a28e0-454">Присвойте классу имя *TodoItem* и выберите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-454">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="a28e0-455">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="a28e0-455">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a28e0-456">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a28e0-456">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a28e0-457">Добавьте папку с именем *Models*.</span><span class="sxs-lookup"><span data-stu-id="a28e0-457">Add a folder named *Models*.</span></span>

* <span data-ttu-id="a28e0-458">Добавьте класс `TodoItem` в папку *Models*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="a28e0-458">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a28e0-459">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="a28e0-459">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a28e0-460">Щелкните проект правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="a28e0-460">Right-click the project.</span></span> <span data-ttu-id="a28e0-461">Выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-461">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="a28e0-462">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="a28e0-462">Name the folder *Models*.</span></span>

  ![Новая папка](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="a28e0-464">Правой кнопкой мыши щелкните папку *Models* и выберите **Добавить** > **Новый файл** > **Общие** > **Пустой класс**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-464">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="a28e0-465">Назовите класс *TodoItem* и нажмите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-465">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="a28e0-466">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="a28e0-466">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="a28e0-467">Свойство `Id` выступает в качестве уникального ключа реляционной базы данных.</span><span class="sxs-lookup"><span data-stu-id="a28e0-467">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="a28e0-468">Классы моделей можно размещать в любом месте проекта, но обычно для этого используется папка *Models*.</span><span class="sxs-lookup"><span data-stu-id="a28e0-468">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="a28e0-469">Добавление контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="a28e0-469">Add a database context</span></span>

<span data-ttu-id="a28e0-470">*Контекст базы данных* —это основной класс, который координирует функциональные возможности Entity Framework для модели данных.</span><span class="sxs-lookup"><span data-stu-id="a28e0-470">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="a28e0-471">Этот класс является производным от класса `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="a28e0-471">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a28e0-472">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a28e0-472">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a28e0-473">Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-473">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="a28e0-474">Назовите класс *TodoContext* и нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-474">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="a28e0-475">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="a28e0-475">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="a28e0-476">Добавьте класс `TodoContext` в папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="a28e0-476">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="a28e0-477">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="a28e0-477">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="a28e0-478">Регистрация контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="a28e0-478">Register the database context</span></span>

<span data-ttu-id="a28e0-479">В ASP.NET Core службы (такие как контекст базы данных) должны быть зарегистрированы с помощью контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a28e0-479">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="a28e0-480">Контейнер предоставляет службу контроллерам.</span><span class="sxs-lookup"><span data-stu-id="a28e0-480">The container provides the service to controllers.</span></span>

<span data-ttu-id="a28e0-481">Обновите файл *Startup.cs*, используя следующий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="a28e0-481">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="a28e0-482">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="a28e0-482">The preceding code:</span></span>

* <span data-ttu-id="a28e0-483">Удалите неиспользуемые объявления `using`.</span><span class="sxs-lookup"><span data-stu-id="a28e0-483">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="a28e0-484">Добавляет контекст базы данных в контейнер внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="a28e0-484">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="a28e0-485">Указывает, что контекст базы данных будет использовать базу данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="a28e0-485">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="a28e0-486">Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="a28e0-486">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a28e0-487">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a28e0-487">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a28e0-488">Щелкните папку *Controllers* правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="a28e0-488">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="a28e0-489">Выберите **Добавить** > **Новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-489">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="a28e0-490">В диалоговом окне **Добавить новый элемент** выберите шаблон **Класс контроллера API**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-490">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="a28e0-491">Присвойте классу имя *TodoController* и выберите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-491">Name the class *TodoController*, and select **Add**.</span></span>

  ![Диалоговое окно добавления элемента с контроллером в поле поиска и выбранным контроллером веб-API](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="a28e0-493">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="a28e0-493">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="a28e0-494">В папке *Controllers* создайте класс с именем `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="a28e0-494">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="a28e0-495">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="a28e0-495">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="a28e0-496">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="a28e0-496">The preceding code:</span></span>

* <span data-ttu-id="a28e0-497">Определяет класс контроллера API без методов.</span><span class="sxs-lookup"><span data-stu-id="a28e0-497">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="a28e0-498">Добавляет в класс атрибут [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="a28e0-498">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="a28e0-499">Этот атрибут указывает, что контроллер отвечает на запросы веб-API.</span><span class="sxs-lookup"><span data-stu-id="a28e0-499">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="a28e0-500">Дополнительные сведения о поведении, которое реализует этот атрибут, см. в <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="a28e0-500">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="a28e0-501">Использует внедрение зависимостей для внедрения контекста базы данных (`TodoContext`) в контроллер.</span><span class="sxs-lookup"><span data-stu-id="a28e0-501">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="a28e0-502">Контекст базы данных используется в каждом методе [создания, чтения, обновления и удаления](https://wikipedia.org/wiki/Create,_read,_update_and_delete) в контроллере.</span><span class="sxs-lookup"><span data-stu-id="a28e0-502">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="a28e0-503">Добавляет элемент `Item1` в базу данных, если она пуста.</span><span class="sxs-lookup"><span data-stu-id="a28e0-503">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="a28e0-504">Этот код находится в конструкторе и выполняется каждый раз при обнаружении нового HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="a28e0-504">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="a28e0-505">Если вы удалите все элементы, конструктор создаст `Item1` при следующем вызове метода API.</span><span class="sxs-lookup"><span data-stu-id="a28e0-505">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="a28e0-506">Поэтому может создаться впечатление, что удаление не было выполнено, хотя это не так.</span><span class="sxs-lookup"><span data-stu-id="a28e0-506">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="a28e0-507">Добавление методов Get</span><span class="sxs-lookup"><span data-stu-id="a28e0-507">Add Get methods</span></span>

<span data-ttu-id="a28e0-508">Чтобы получить API, который извлекает элементы списка дел, добавьте следующие методы в класс `TodoController`:</span><span class="sxs-lookup"><span data-stu-id="a28e0-508">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="a28e0-509">Эти методы реализуют две конечные точки GET:</span><span class="sxs-lookup"><span data-stu-id="a28e0-509">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="a28e0-510">Если приложение все еще выполняется, оно останавливается.</span><span class="sxs-lookup"><span data-stu-id="a28e0-510">Stop the app if it's still running.</span></span> <span data-ttu-id="a28e0-511">Затем оно запускается снова с последними изменениями.</span><span class="sxs-lookup"><span data-stu-id="a28e0-511">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="a28e0-512">Протестируйте приложение, вызвав эти две конечные точки в браузере.</span><span class="sxs-lookup"><span data-stu-id="a28e0-512">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="a28e0-513">Например:</span><span class="sxs-lookup"><span data-stu-id="a28e0-513">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="a28e0-514">При вызове `GetTodoItems` возвращается следующий ответ HTTP:</span><span class="sxs-lookup"><span data-stu-id="a28e0-514">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="a28e0-515">Маршрутизация и пути URL</span><span class="sxs-lookup"><span data-stu-id="a28e0-515">Routing and URL paths</span></span>

<span data-ttu-id="a28e0-516">Атрибут [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) обозначает метод, который отвечает на запрос HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="a28e0-516">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="a28e0-517">Путь URL для каждого метода формируется следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a28e0-517">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="a28e0-518">Возьмите строку шаблона в атрибуте `Route` контроллера:</span><span class="sxs-lookup"><span data-stu-id="a28e0-518">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="a28e0-519">Замените `[controller]` именем контроллера (по соглашению это имя класса контроллера без суффикса "Controller").</span><span class="sxs-lookup"><span data-stu-id="a28e0-519">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="a28e0-520">В этом примере класс контроллера носит имя **Todo**, а сам контроллер, соответственно, — "todo".</span><span class="sxs-lookup"><span data-stu-id="a28e0-520">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="a28e0-521">В ASP.NET Core [маршрутизация](xref:mvc/controllers/routing) реализуется без учета регистра символов.</span><span class="sxs-lookup"><span data-stu-id="a28e0-521">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="a28e0-522">Если атрибут `[HttpGet]` имеет шаблон маршрута (например, `[HttpGet("products")]`), добавьте его к пути.</span><span class="sxs-lookup"><span data-stu-id="a28e0-522">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="a28e0-523">В этом примере шаблон не используется.</span><span class="sxs-lookup"><span data-stu-id="a28e0-523">This sample doesn't use a template.</span></span> <span data-ttu-id="a28e0-524">Дополнительные сведения см. в разделе [Маршрутизация атрибутов с помощью атрибутов Http[Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="a28e0-524">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="a28e0-525">В следующем методе `GetTodoItem` `"{id}"` — это переменная-заполнитель для уникального идентификатора элемента задачи.</span><span class="sxs-lookup"><span data-stu-id="a28e0-525">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="a28e0-526">При вызове `GetTodoItem` параметру метода `id` присваивается значение `"{id}"` в URL-адресе.</span><span class="sxs-lookup"><span data-stu-id="a28e0-526">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="a28e0-527">Возвращаемые значения</span><span class="sxs-lookup"><span data-stu-id="a28e0-527">Return values</span></span>

<span data-ttu-id="a28e0-528">Возвращаемое значение имеет тип `GetTodoItems`, а метод `GetTodoItem` имеет тип [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="a28e0-528">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="a28e0-529">ASP.NET Core автоматически сериализует объект в формат [JSON](https://www.json.org/) и записывает данные JSON в тело сообщения ответа.</span><span class="sxs-lookup"><span data-stu-id="a28e0-529">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="a28e0-530">Код ответа для этого типа возвращаемого значения равен 200, что свидетельствует об отсутствии необработанных исключений.</span><span class="sxs-lookup"><span data-stu-id="a28e0-530">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="a28e0-531">Необработанные исключения преобразуются в ошибки 5xx.</span><span class="sxs-lookup"><span data-stu-id="a28e0-531">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="a28e0-532">Типы возвращаемых значений `ActionResult` могут представлять широкий спектр кодов состояний HTTP.</span><span class="sxs-lookup"><span data-stu-id="a28e0-532">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="a28e0-533">Например, метод `GetTodoItem` может возвращать два разных значения состояния:</span><span class="sxs-lookup"><span data-stu-id="a28e0-533">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="a28e0-534">Если запрошенному идентификатору не соответствует ни один элемент, метод возвращает ошибку 404 ([Не найдено](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound)).</span><span class="sxs-lookup"><span data-stu-id="a28e0-534">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="a28e0-535">В противном случае метод возвращает код 200 с телом ответа JSON.</span><span class="sxs-lookup"><span data-stu-id="a28e0-535">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="a28e0-536">При возвращении `item` возвращается ответ HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="a28e0-536">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="a28e0-537">Тестирование метода GetTodoItems</span><span class="sxs-lookup"><span data-stu-id="a28e0-537">Test the GetTodoItems method</span></span>

<span data-ttu-id="a28e0-538">В этом учебнике для тестирования веб-API используется Postman.</span><span class="sxs-lookup"><span data-stu-id="a28e0-538">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="a28e0-539">Установка [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="a28e0-539">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="a28e0-540">Запустите веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="a28e0-540">Start the web app.</span></span>
* <span data-ttu-id="a28e0-541">Запустите Postman.</span><span class="sxs-lookup"><span data-stu-id="a28e0-541">Start Postman.</span></span>
* <span data-ttu-id="a28e0-542">Отключение параметра **Проверка SSL-сертификата**</span><span class="sxs-lookup"><span data-stu-id="a28e0-542">Disable **SSL certificate verification**</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a28e0-543">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a28e0-543">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a28e0-544">В меню **Файл** > **Параметры** (вкладка **Общие**) отключите параметр **Проверка SSL-сертификата**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-544">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="a28e0-545">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="a28e0-545">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="a28e0-546">В меню **Postman**  >  **Настройки** (вкладка **Общие**) отключите параметр **Проверка SSL-сертификата**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-546">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="a28e0-547">Кроме того, можно выбрать значок гаечного ключа и выбрать **Параметры**, а затем отключить проверку SSL-сертификата.</span><span class="sxs-lookup"><span data-stu-id="a28e0-547">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="a28e0-548">После тестирования контроллера снова включите проверку SSL-сертификата.</span><span class="sxs-lookup"><span data-stu-id="a28e0-548">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="a28e0-549">Создайте новый запрос.</span><span class="sxs-lookup"><span data-stu-id="a28e0-549">Create a new request.</span></span>
  * <span data-ttu-id="a28e0-550">Укажите метод HTTP **GET**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-550">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="a28e0-551">Укажите URL-адрес запроса `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="a28e0-551">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="a28e0-552">Например, `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="a28e0-552">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="a28e0-553">Выберите режим **Представление с двумя областями** в Postman.</span><span class="sxs-lookup"><span data-stu-id="a28e0-553">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="a28e0-554">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-554">Select **Send**.</span></span>

![Postman с запросом Get](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="a28e0-556">Добавление метода Create</span><span class="sxs-lookup"><span data-stu-id="a28e0-556">Add a Create method</span></span>

<span data-ttu-id="a28e0-557">Добавьте следующий метод `PostTodoItem` в *Controllers/TodoController.cs*:</span><span class="sxs-lookup"><span data-stu-id="a28e0-557">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="a28e0-558">Предыдущий код является методом HTTP POST, как указывает атрибут [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="a28e0-558">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="a28e0-559">Этот метод получает значение элемента списка дел из текста HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="a28e0-559">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="a28e0-560">Метод `CreatedAtAction`:</span><span class="sxs-lookup"><span data-stu-id="a28e0-560">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="a28e0-561">В случае успеха возвращает код состояния HTTP 201.</span><span class="sxs-lookup"><span data-stu-id="a28e0-561">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="a28e0-562">HTTP 201 представляет собой стандартный ответ для метода HTTP POST, создающий ресурс на сервере.</span><span class="sxs-lookup"><span data-stu-id="a28e0-562">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="a28e0-563">Добавляет заголовок `Location` в ответ.</span><span class="sxs-lookup"><span data-stu-id="a28e0-563">Adds a `Location` header to the response.</span></span> <span data-ttu-id="a28e0-564">Заголовок `Location` расположения указывает URI вновь созданной задачи.</span><span class="sxs-lookup"><span data-stu-id="a28e0-564">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="a28e0-565">Дополнительные сведения см. в статье [10.2.2 201 "Создан ресурс"](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="a28e0-565">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="a28e0-566">Указывает действие `GetTodoItem` для создания URI заголовка `Location`.</span><span class="sxs-lookup"><span data-stu-id="a28e0-566">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="a28e0-567">Ключевое слово `nameof` C# используется для предотвращения жесткого программирования имени действия в вызове `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="a28e0-567">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="a28e0-568">Тестирование метода PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="a28e0-568">Test the PostTodoItem method</span></span>

* <span data-ttu-id="a28e0-569">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="a28e0-569">Build the project.</span></span>
* <span data-ttu-id="a28e0-570">В Postman укажите метод HTTP `POST`.</span><span class="sxs-lookup"><span data-stu-id="a28e0-570">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="a28e0-571">Откройте вкладку **Тело**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-571">Select the **Body** tab.</span></span>
* <span data-ttu-id="a28e0-572">Установите переключатель **без обработки**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-572">Select the **raw** radio button.</span></span>
* <span data-ttu-id="a28e0-573">Задайте тип **JSON (приложение/json)** .</span><span class="sxs-lookup"><span data-stu-id="a28e0-573">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="a28e0-574">В теле запроса введите код JSON для элемента списка дел:</span><span class="sxs-lookup"><span data-stu-id="a28e0-574">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="a28e0-575">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-575">Select **Send**.</span></span>

  ![Postman с запросом Create](first-web-api/_static/create.png)

  <span data-ttu-id="a28e0-577">Если вы получаете ошибку 405 "Недопустимый метод", это может свидетельствовать о том, что после добавления метода `PostTodoItem` не была выполнена компиляция проекта.</span><span class="sxs-lookup"><span data-stu-id="a28e0-577">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="a28e0-578">Тестирование URI заголовка расположения</span><span class="sxs-lookup"><span data-stu-id="a28e0-578">Test the location header URI</span></span>

* <span data-ttu-id="a28e0-579">Перейдите на вкладку **Заголовки** в области **Ответ**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-579">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="a28e0-580">Скопируйте значение заголовка **Расположение**:</span><span class="sxs-lookup"><span data-stu-id="a28e0-580">Copy the **Location** header value:</span></span>

  ![Вкладка "Заголовки" в консоли Postman](first-web-api/_static/pmc2.png)

* <span data-ttu-id="a28e0-582">Укажите метод GET.</span><span class="sxs-lookup"><span data-stu-id="a28e0-582">Set the method to GET.</span></span>
* <span data-ttu-id="a28e0-583">Вставьте URI (например, `https://localhost:5001/api/Todo/2`)</span><span class="sxs-lookup"><span data-stu-id="a28e0-583">Paste the URI (for example, `https://localhost:5001/api/Todo/2`)</span></span>
* <span data-ttu-id="a28e0-584">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="a28e0-584">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="a28e0-585">Добавление метода PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="a28e0-585">Add a PutTodoItem method</span></span>

<span data-ttu-id="a28e0-586">Добавьте приведенный ниже метод `PutTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="a28e0-586">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="a28e0-587">Страница `PutTodoItem` аналогична странице `PostTodoItem`, но использует запрос HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="a28e0-587">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="a28e0-588">Ответ — [204 (Нет содержимого)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="a28e0-588">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="a28e0-589">Согласно спецификации HTTP, запрос PUT требует, чтобы клиент отправлял всю обновленную сущность, а не только изменения.</span><span class="sxs-lookup"><span data-stu-id="a28e0-589">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="a28e0-590">Чтобы обеспечить поддержку частичных обновлений, используйте [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="a28e0-590">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="a28e0-591">Если возникнет ошибка вызова `PutTodoItem`, вызовите `GET`, чтобы в базе данных был один элемент.</span><span class="sxs-lookup"><span data-stu-id="a28e0-591">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="a28e0-592">Тестирование метода PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="a28e0-592">Test the PutTodoItem method</span></span>

<span data-ttu-id="a28e0-593">Этот пример использует базу данных в памяти, которая должна быть инициирована при каждом запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="a28e0-593">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="a28e0-594">При выполнении вызова PUT в базе данных уже должен существовать какой-либо элемент.</span><span class="sxs-lookup"><span data-stu-id="a28e0-594">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="a28e0-595">Для этого перед вызовом PUT выполните вызов GET, чтобы убедиться в наличии такого элемента в базе данных.</span><span class="sxs-lookup"><span data-stu-id="a28e0-595">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="a28e0-596">Обновите элемент списка дел с идентификатором = 1 и присвойте ему имя "feed fish":</span><span class="sxs-lookup"><span data-stu-id="a28e0-596">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="a28e0-597">На следующем рисунке показан процесс обновления Postman:</span><span class="sxs-lookup"><span data-stu-id="a28e0-597">The following image shows the Postman update:</span></span>

![Консоль Postman с ответом 204 (Нет содержимого)](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="a28e0-599">Добавление метода DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="a28e0-599">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="a28e0-600">Добавьте приведенный ниже метод `DeleteTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="a28e0-600">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="a28e0-601">`DeleteTodoItem`Ответ[ — 204 (нет содержимого)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="a28e0-601">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="a28e0-602">Тестирование метода DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="a28e0-602">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="a28e0-603">Удалите элемент списка дел с помощью Postman:</span><span class="sxs-lookup"><span data-stu-id="a28e0-603">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="a28e0-604">Укажите метод `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="a28e0-604">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="a28e0-605">Укажите URI удаляемого объекта, например `https://localhost:5001/api/todo/1`</span><span class="sxs-lookup"><span data-stu-id="a28e0-605">Set the URI of the object to delete, for example `https://localhost:5001/api/todo/1`</span></span>
* <span data-ttu-id="a28e0-606">Нажмите кнопку **Отправить**</span><span class="sxs-lookup"><span data-stu-id="a28e0-606">Select **Send**</span></span>

<span data-ttu-id="a28e0-607">В этом примере приложения вы можете удалить все элементы.</span><span class="sxs-lookup"><span data-stu-id="a28e0-607">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="a28e0-608">Однако в случае удаления последнего элемента в момент следующего вызова API конструктор класса модели создаст новый элемент.</span><span class="sxs-lookup"><span data-stu-id="a28e0-608">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="a28e0-609">Вызов веб-API с помощью JavaScript</span><span class="sxs-lookup"><span data-stu-id="a28e0-609">Call the web API with JavaScript</span></span>

<span data-ttu-id="a28e0-610">В этом разделе описано, как добавить HTML-страницу, которая использует JavaScript для вызова веб-API.</span><span class="sxs-lookup"><span data-stu-id="a28e0-610">In this section, an HTML page is added that uses JavaScript to call the web API.</span></span> <span data-ttu-id="a28e0-611">Fetch API инициирует запрос.</span><span class="sxs-lookup"><span data-stu-id="a28e0-611">The Fetch API initiates the request.</span></span> <span data-ttu-id="a28e0-612">JavaScript изменяет страницу, используя сведения из ответа API.</span><span class="sxs-lookup"><span data-stu-id="a28e0-612">JavaScript updates the page with the details from the web API's response.</span></span>

<span data-ttu-id="a28e0-613">Настройте приложение для [обслуживания статических файлов](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) и [включения сопоставления файлов по умолчанию](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_), обновив *Startup.cs* следующим выделенным кодом:</span><span class="sxs-lookup"><span data-stu-id="a28e0-613">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="a28e0-614">Создайте папку *wwwroot* в каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="a28e0-614">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="a28e0-615">Добавьте HTML-файл *index.html* в каталог *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="a28e0-615">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="a28e0-616">Замените его содержимое следующей разметкой:</span><span class="sxs-lookup"><span data-stu-id="a28e0-616">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="a28e0-617">Добавьте файл JavaScript с именем *site.js* в каталог *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="a28e0-617">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="a28e0-618">Замените его содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="a28e0-618">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="a28e0-619">Может потребоваться изменение параметров запуска проекта ASP.NET Core для локального тестирования HTML-страницы:</span><span class="sxs-lookup"><span data-stu-id="a28e0-619">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="a28e0-620">Откройте файл *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a28e0-620">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="a28e0-621">Удалите свойство `launchUrl`, чтобы приложение открылось через *index.html* &mdash; файл проекта по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a28e0-621">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="a28e0-622">В этом примере вызываются все методы CRUD в веб-API.</span><span class="sxs-lookup"><span data-stu-id="a28e0-622">This sample calls all of the CRUD methods of the web API.</span></span> <span data-ttu-id="a28e0-623">Ниже приводится пояснение вызовов API.</span><span class="sxs-lookup"><span data-stu-id="a28e0-623">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="a28e0-624">Получение списка элементов задач</span><span class="sxs-lookup"><span data-stu-id="a28e0-624">Get a list of to-do items</span></span>

<span data-ttu-id="a28e0-625">Fetch отправляет запрос HTTP GET к веб-API, который возвращает ответ JSON, представляющий массив элементов списка дел.</span><span class="sxs-lookup"><span data-stu-id="a28e0-625">Fetch sends an HTTP GET request to the web API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="a28e0-626">В случае успешного запроса используется функция обратного вызова `success`.</span><span class="sxs-lookup"><span data-stu-id="a28e0-626">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="a28e0-627">При обратном вызове в модель DOM вносятся данные о задачах.</span><span class="sxs-lookup"><span data-stu-id="a28e0-627">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="a28e0-628">Добавление элемента задачи</span><span class="sxs-lookup"><span data-stu-id="a28e0-628">Add a to-do item</span></span>

<span data-ttu-id="a28e0-629">Fetch отправляет запрос HTTP POST с элементом списка дел в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="a28e0-629">Fetch sends an HTTP POST request with the to-do item in the request body.</span></span> <span data-ttu-id="a28e0-630">Для параметров `accepts` и `contentType` указывается `application/json`, чтобы указать тип носителя при получении и отправке.</span><span class="sxs-lookup"><span data-stu-id="a28e0-630">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="a28e0-631">Элемент списка дел преобразуется в JSON с помощью [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="a28e0-631">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="a28e0-632">Если интерфейс API возвращает код состояния успешного выполнения, вызывается функция `getData` для обновления HTML-таблицы.</span><span class="sxs-lookup"><span data-stu-id="a28e0-632">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="a28e0-633">Обновление элемента задачи</span><span class="sxs-lookup"><span data-stu-id="a28e0-633">Update a to-do item</span></span>

<span data-ttu-id="a28e0-634">Обновление элемента списка дел выполняется аналогично его добавлению.</span><span class="sxs-lookup"><span data-stu-id="a28e0-634">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="a28e0-635">`url` изменяется, чтобы добавить уникальный идентификатор для элемента, а в качестве `type` устанавливается `PUT`.</span><span class="sxs-lookup"><span data-stu-id="a28e0-635">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="a28e0-636">Удаление элемента задачи</span><span class="sxs-lookup"><span data-stu-id="a28e0-636">Delete a to-do item</span></span>

<span data-ttu-id="a28e0-637">Чтобы удалить элемент списка дел, установите для `type` в вызове AJAX значение `DELETE` и укажите уникальный идентификатор элемента в URL-адресе.</span><span class="sxs-lookup"><span data-stu-id="a28e0-637">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="a28e0-638">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a28e0-638">Additional resources</span></span>

<span data-ttu-id="a28e0-639">[Просмотреть или скачать пример кода для этого учебника](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="a28e0-639">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="a28e0-640">См. раздел [Практическое руководство. Скачивание файла](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="a28e0-640">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="a28e0-641">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="a28e0-641">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="a28e0-642">Версия руководства на YouTube</span><span class="sxs-lookup"><span data-stu-id="a28e0-642">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
