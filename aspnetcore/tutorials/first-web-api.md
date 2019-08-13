---
title: Учебник. Создание веб-API с помощью ASP.NET Core
author: rick-anderson
description: Узнайте, как создать веб-API с помощью ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 08/05/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 855d05fa2b9c1a7572212c40adbe61bb396f4bac
ms.sourcegitcommit: 2eb605f4f20ac4dd9de6c3b3e3453e108a357a21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/06/2019
ms.locfileid: "68819832"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="834c4-103">Учебник. Создание веб-API с помощью ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="834c4-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="834c4-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Майк Уоссон](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="834c4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="834c4-105">В этом учебнике приводятся основные сведения о создании веб-API с помощью ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="834c4-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="834c4-106">В этом руководстве вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="834c4-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="834c4-107">Создание проекта веб-API.</span><span class="sxs-lookup"><span data-stu-id="834c4-107">Create a web API project.</span></span>
> * <span data-ttu-id="834c4-108">Добавление класса модели и контекста базы данных.</span><span class="sxs-lookup"><span data-stu-id="834c4-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="834c4-109">Формирование шаблонов контроллера с использованием методов CRUD.</span><span class="sxs-lookup"><span data-stu-id="834c4-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="834c4-110">Настройка маршрутизации, URL-пути и возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="834c4-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="834c4-111">Вызов веб-API с помощью Postman.</span><span class="sxs-lookup"><span data-stu-id="834c4-111">Call the web API with Postman.</span></span>

<span data-ttu-id="834c4-112">В итоге вы получите веб-API, позволяющий работать с элементами списка дел, хранимыми в базе данных.</span><span class="sxs-lookup"><span data-stu-id="834c4-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="834c4-113">Обзор</span><span class="sxs-lookup"><span data-stu-id="834c4-113">Overview</span></span>

<span data-ttu-id="834c4-114">В этом руководстве создается следующий API-интерфейс:</span><span class="sxs-lookup"><span data-stu-id="834c4-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="834c4-115">API</span><span class="sxs-lookup"><span data-stu-id="834c4-115">API</span></span> | <span data-ttu-id="834c4-116">ОПИСАНИЕ</span><span class="sxs-lookup"><span data-stu-id="834c4-116">Description</span></span> | <span data-ttu-id="834c4-117">Текст запроса</span><span class="sxs-lookup"><span data-stu-id="834c4-117">Request body</span></span> | <span data-ttu-id="834c4-118">Текст ответа</span><span class="sxs-lookup"><span data-stu-id="834c4-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="834c4-119">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="834c4-119">GET /api/TodoItems</span></span> | <span data-ttu-id="834c4-120">Получение всех элементов задач</span><span class="sxs-lookup"><span data-stu-id="834c4-120">Get all to-do items</span></span> | <span data-ttu-id="834c4-121">Нет</span><span class="sxs-lookup"><span data-stu-id="834c4-121">None</span></span> | <span data-ttu-id="834c4-122">Массив элементов задач</span><span class="sxs-lookup"><span data-stu-id="834c4-122">Array of to-do items</span></span>|
|<span data-ttu-id="834c4-123">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="834c4-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="834c4-124">Получение объекта по идентификатору</span><span class="sxs-lookup"><span data-stu-id="834c4-124">Get an item by ID</span></span> | <span data-ttu-id="834c4-125">Нет</span><span class="sxs-lookup"><span data-stu-id="834c4-125">None</span></span> | <span data-ttu-id="834c4-126">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="834c4-126">To-do item</span></span>|
|<span data-ttu-id="834c4-127">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="834c4-127">POST /api/TodoItems</span></span> | <span data-ttu-id="834c4-128">Добавление нового элемента</span><span class="sxs-lookup"><span data-stu-id="834c4-128">Add a new item</span></span> | <span data-ttu-id="834c4-129">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="834c4-129">To-do item</span></span> | <span data-ttu-id="834c4-130">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="834c4-130">To-do item</span></span> |
|<span data-ttu-id="834c4-131">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="834c4-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="834c4-132">Обновление существующего элемента &nbsp;</span><span class="sxs-lookup"><span data-stu-id="834c4-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="834c4-133">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="834c4-133">To-do item</span></span> | <span data-ttu-id="834c4-134">Нет</span><span class="sxs-lookup"><span data-stu-id="834c4-134">None</span></span> |
|<span data-ttu-id="834c4-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="834c4-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="834c4-136">Удаление элемента &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="834c4-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="834c4-137">Нет</span><span class="sxs-lookup"><span data-stu-id="834c4-137">None</span></span> | <span data-ttu-id="834c4-138">Нет</span><span class="sxs-lookup"><span data-stu-id="834c4-138">None</span></span>|

<span data-ttu-id="834c4-139">На следующем рисунке показана структура приложения.</span><span class="sxs-lookup"><span data-stu-id="834c4-139">The following diagram shows the design of the app.</span></span>

![Клиент представлен прямоугольником слева.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="834c4-145">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="834c4-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="834c4-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="834c4-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="834c4-147">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="834c4-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="834c4-148">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="834c4-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="834c4-149">Создайте веб-проект.</span><span class="sxs-lookup"><span data-stu-id="834c4-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="834c4-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="834c4-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="834c4-151">В меню **Файл** выберите **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="834c4-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="834c4-152">Выберите шаблон **Веб-приложение ASP.NET Core** и нажмите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="834c4-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="834c4-153">Назовите проект *TodoApi* и нажмите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="834c4-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="834c4-154">В диалоговом окне **Создание веб-приложения ASP.NET Core** убедитесь в том, что выбраны платформы **.NET Core** и **ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="834c4-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span> <span data-ttu-id="834c4-155">Выберите шаблон **API** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="834c4-155">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="834c4-156">**Не** выбирайте **Включение поддержки Docker**.</span><span class="sxs-lookup"><span data-stu-id="834c4-156">**Don't** select **Enable Docker Support**.</span></span>

![Диалоговое окно создания проекта VS](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="834c4-158">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="834c4-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="834c4-159">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="834c4-159">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="834c4-160">Смените каталог (`cd`) на папку, в которой будет содержаться папка проекта.</span><span class="sxs-lookup"><span data-stu-id="834c4-160">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="834c4-161">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="834c4-161">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   cd TodoAPI
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
   dotnet add package Microsoft.EntityFrameworkCore.InMemory --version 3.0.0-*
   code -r ../TodoApi
   ```

* <span data-ttu-id="834c4-162">При появлении диалогового окна с запросом на добавление в проект необходимых ресурсов выберите **Да**.</span><span class="sxs-lookup"><span data-stu-id="834c4-162">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="834c4-163">Предыдущие команды:</span><span class="sxs-lookup"><span data-stu-id="834c4-163">The preceding commands:</span></span>

  * <span data-ttu-id="834c4-164">Создает новый проект веб-API и открывает его в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="834c4-164">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="834c4-165">Добавляет пакеты NuGet, которые понадобятся в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="834c4-165">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="834c4-166">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="834c4-166">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="834c4-167">Выберите **Файл** > **Новое решение**.</span><span class="sxs-lookup"><span data-stu-id="834c4-167">Select **File** > **New Solution**.</span></span>

  ![Новое решение macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="834c4-169">Выберите **.NET Core** > **Приложение** > **API** > **Далее**.</span><span class="sxs-lookup"><span data-stu-id="834c4-169">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![Диалоговое окно "Новый проект" в macOS](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="834c4-171">В диалоговом окне **Настройка нового веб-API ASP.NET Core** в качестве **Целевой платформы** выберите \* *.NET Core 3.0*.</span><span class="sxs-lookup"><span data-stu-id="834c4-171">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.0*.</span></span>

* <span data-ttu-id="834c4-172">Введите *TodoApi* в поле **Имя проекта** и выберите команду **Создать**.</span><span class="sxs-lookup"><span data-stu-id="834c4-172">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Диалоговое окно конфигурации](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="834c4-174">Тестирование API</span><span class="sxs-lookup"><span data-stu-id="834c4-174">Test the API</span></span>

<span data-ttu-id="834c4-175">Шаблон проекта создает API `WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="834c4-175">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="834c4-176">Вызовите метод `Get` из браузера для тестирования приложения.</span><span class="sxs-lookup"><span data-stu-id="834c4-176">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="834c4-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="834c4-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="834c4-178">Нажмите клавиши CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="834c4-178">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="834c4-179">Visual Studio запустит браузер и перейдет к `https://localhost:<port>/WeatherForecast`, где `<port>` — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="834c4-179">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="834c4-180">Если появится диалоговое окно с запросом о необходимости доверять сертификату IIS Express, выберите **Да**.</span><span class="sxs-lookup"><span data-stu-id="834c4-180">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="834c4-181">В появляющемся следом диалоговом окне **Предупреждение системы безопасности** выберите **Да**.</span><span class="sxs-lookup"><span data-stu-id="834c4-181">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="834c4-182">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="834c4-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="834c4-183">Нажмите клавиши CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="834c4-183">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="834c4-184">В браузере перейдите по следующему URL-адресу: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="834c4-184">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="834c4-185">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="834c4-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="834c4-186">Выберите **Выполнить** > **Начать отладку**, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="834c4-186">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="834c4-187">Visual Studio для Mac запустит браузер и перейдет к `https://localhost:<port>`, где `<port>` — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="834c4-187">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="834c4-188">Появится ошибка HTTP 404 (Не найдено).</span><span class="sxs-lookup"><span data-stu-id="834c4-188">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="834c4-189">Добавьте `/WeatherForecast` к URL-адресу (измените URL-адрес на `https://localhost:<port>/WeatherForecast`).</span><span class="sxs-lookup"><span data-stu-id="834c4-189">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="834c4-190">Возвращаемые данные JSON будут выглядеть примерно так:</span><span class="sxs-lookup"><span data-stu-id="834c4-190">JSON similar to the following is returned:</span></span>

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

## <a name="add-a-model-class"></a><span data-ttu-id="834c4-191">Добавление класса модели</span><span class="sxs-lookup"><span data-stu-id="834c4-191">Add a model class</span></span>

<span data-ttu-id="834c4-192">*Модель* — это набор классов, представляющих данные, которыми управляет приложение.</span><span class="sxs-lookup"><span data-stu-id="834c4-192">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="834c4-193">Модель этого приложения содержит единственный класс `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="834c4-193">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="834c4-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="834c4-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="834c4-195">В **обозревателе решений** щелкните проект правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="834c4-195">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="834c4-196">Выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="834c4-196">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="834c4-197">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="834c4-197">Name the folder *Models*.</span></span>

* <span data-ttu-id="834c4-198">Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="834c4-198">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="834c4-199">Присвойте классу имя *TodoItem* и выберите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="834c4-199">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="834c4-200">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="834c4-200">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="834c4-201">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="834c4-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="834c4-202">Добавьте папку с именем *Models*.</span><span class="sxs-lookup"><span data-stu-id="834c4-202">Add a folder named *Models*.</span></span>

* <span data-ttu-id="834c4-203">Добавьте класс `TodoItem` в папку *Models*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="834c4-203">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="834c4-204">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="834c4-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="834c4-205">Щелкните проект правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="834c4-205">Right-click the project.</span></span> <span data-ttu-id="834c4-206">Выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="834c4-206">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="834c4-207">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="834c4-207">Name the folder *Models*.</span></span>

  ![Новая папка](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="834c4-209">Правой кнопкой мыши щелкните папку *Models* и выберите **Добавить** > **Новый файл** > **Общие** > **Пустой класс**.</span><span class="sxs-lookup"><span data-stu-id="834c4-209">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="834c4-210">Назовите класс *TodoItem* и нажмите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="834c4-210">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="834c4-211">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="834c4-211">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="834c4-212">Свойство `Id` выступает в качестве уникального ключа реляционной базы данных.</span><span class="sxs-lookup"><span data-stu-id="834c4-212">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="834c4-213">Классы моделей можно размещать в любом месте проекта, но обычно для этого используется папка *Models*.</span><span class="sxs-lookup"><span data-stu-id="834c4-213">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="834c4-214">Добавление контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="834c4-214">Add a database context</span></span>

<span data-ttu-id="834c4-215">*Контекст базы данных* —это основной класс, который координирует функциональные возможности Entity Framework для модели данных.</span><span class="sxs-lookup"><span data-stu-id="834c4-215">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="834c4-216">Этот класс является производным от класса `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="834c4-216">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="834c4-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="834c4-217">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="834c4-218">Добавление Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="834c4-218">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="834c4-219">В меню **Сервис** выберите **Диспетчер пакетов NuGet > Управление пакетами NuGet для решения**.</span><span class="sxs-lookup"><span data-stu-id="834c4-219">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="834c4-220">Установите флажок **Включить предварительные выпуски**.</span><span class="sxs-lookup"><span data-stu-id="834c4-220">Select the **Include prerelease** checkbox.</span></span>
* <span data-ttu-id="834c4-221">Перейдите на вкладку **Обзор** и введите **Microsoft.EntityFrameworkCore.SqlServer** в поле поиска.</span><span class="sxs-lookup"><span data-stu-id="834c4-221">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="834c4-222">В панели слева выберите **Microsoft.EntityFrameworkCore.SqlServer V3.0.0-preview**.</span><span class="sxs-lookup"><span data-stu-id="834c4-222">Select  **Microsoft.EntityFrameworkCore.SqlServer V3.0.0-preview** in the left pane.</span></span>
* <span data-ttu-id="834c4-223">Установите флажок **Проект** на правой панели и выберите **Установить**.</span><span class="sxs-lookup"><span data-stu-id="834c4-223">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="834c4-224">Добавьте пакет NuGet `Microsoft.EntityFrameworkCore.InMemory` согласно инструкциям выше.</span><span class="sxs-lookup"><span data-stu-id="834c4-224">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![Диспетчер пакетов NuGet](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="834c4-226">Добавление контекста базы данных TodoContext</span><span class="sxs-lookup"><span data-stu-id="834c4-226">Add the TodoContext database context</span></span>

* <span data-ttu-id="834c4-227">Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="834c4-227">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="834c4-228">Назовите класс *TodoContext* и нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="834c4-228">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="834c4-229">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="834c4-229">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="834c4-230">Добавьте класс `TodoContext` в папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="834c4-230">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="834c4-231">Введите следующий код:</span><span class="sxs-lookup"><span data-stu-id="834c4-231">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="834c4-232">Регистрация контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="834c4-232">Register the database context</span></span>

<span data-ttu-id="834c4-233">В ASP.NET Core службы (такие как контекст базы данных) должны быть зарегистрированы с помощью контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="834c4-233">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="834c4-234">Контейнер предоставляет службу контроллерам.</span><span class="sxs-lookup"><span data-stu-id="834c4-234">The container provides the service to controllers.</span></span>

<span data-ttu-id="834c4-235">Обновите файл *Startup.cs*, используя следующий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="834c4-235">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="834c4-236">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="834c4-236">The preceding code:</span></span>

* <span data-ttu-id="834c4-237">Удалите неиспользуемые объявления `using`.</span><span class="sxs-lookup"><span data-stu-id="834c4-237">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="834c4-238">Добавляет контекст базы данных в контейнер внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="834c4-238">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="834c4-239">Указывает, что контекст базы данных будет использовать базу данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="834c4-239">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="834c4-240">Формирование шаблонов контроллера</span><span class="sxs-lookup"><span data-stu-id="834c4-240">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="834c4-241">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="834c4-241">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="834c4-242">Щелкните папку *Controllers* правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="834c4-242">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="834c4-243">Выберите **Добавить** > **Создать шаблонный элемент**.</span><span class="sxs-lookup"><span data-stu-id="834c4-243">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="834c4-244">Выберите **Контроллер API с действиями, использующий Entity Framework**, а затем выберите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="834c4-244">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="834c4-245">В диалоговом окне **Контроллер API с действиями, использующий Entity Framework** сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="834c4-245">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="834c4-246">Выберите **TodoItem (TodoAPI.Models)** в поле **Класс модели**.</span><span class="sxs-lookup"><span data-stu-id="834c4-246">Select **TodoItem (TodoAPI.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="834c4-247">Выберите **TodoContext (TodoAPI.Models)** в поле **Класс контекста данных**.</span><span class="sxs-lookup"><span data-stu-id="834c4-247">Select **TodoContext (TodoAPI.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="834c4-248">Нажмите **Добавить**</span><span class="sxs-lookup"><span data-stu-id="834c4-248">Select **Add**</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="834c4-249">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="834c4-249">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="834c4-250">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="834c4-250">Run the following commands:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext  -outDir Controllers
```

<span data-ttu-id="834c4-251">Предыдущие команды:</span><span class="sxs-lookup"><span data-stu-id="834c4-251">The preceding commands:</span></span>

* <span data-ttu-id="834c4-252">добавляют пакеты NuGet, необходимые для формирования шаблонов;</span><span class="sxs-lookup"><span data-stu-id="834c4-252">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="834c4-253">устанавливают подсистему формирования шаблонов (`dotnet-aspnet-codegenerator`);</span><span class="sxs-lookup"><span data-stu-id="834c4-253">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="834c4-254">формируют шаблоны для `TodoItemsController`.</span><span class="sxs-lookup"><span data-stu-id="834c4-254">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="834c4-255">Сформированный код:</span><span class="sxs-lookup"><span data-stu-id="834c4-255">The generated code:</span></span>

* <span data-ttu-id="834c4-256">Определяет класс контроллера API без методов.</span><span class="sxs-lookup"><span data-stu-id="834c4-256">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="834c4-257">Добавляет в класс атрибут [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="834c4-257">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="834c4-258">Этот атрибут указывает, что контроллер отвечает на запросы веб-API.</span><span class="sxs-lookup"><span data-stu-id="834c4-258">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="834c4-259">Дополнительные сведения о поведении, которое реализует этот атрибут, см. в <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="834c4-259">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="834c4-260">Использует внедрение зависимостей для внедрения контекста базы данных (`TodoContext`) в контроллер.</span><span class="sxs-lookup"><span data-stu-id="834c4-260">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="834c4-261">Контекст базы данных используется в каждом методе [создания, чтения, обновления и удаления](https://wikipedia.org/wiki/Create,_read,_update_and_delete) в контроллере.</span><span class="sxs-lookup"><span data-stu-id="834c4-261">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="834c4-262">Знакомство с методом создания PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="834c4-262">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="834c4-263">Измените инструкцию возврата в `PostTodoItem` и используйте оператор [nameof](/dotnet/csharp/language-reference/operators/nameof):</span><span class="sxs-lookup"><span data-stu-id="834c4-263">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="834c4-264">Предыдущий код является методом HTTP POST, как указывает атрибут [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="834c4-264">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="834c4-265">Этот метод получает значение элемента списка дел из текста HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="834c4-265">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="834c4-266">Метод <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*>:</span><span class="sxs-lookup"><span data-stu-id="834c4-266">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="834c4-267">В случае успеха возвращает код состояния HTTP 201.</span><span class="sxs-lookup"><span data-stu-id="834c4-267">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="834c4-268">HTTP 201 представляет собой стандартный ответ для метода HTTP POST, создающий ресурс на сервере.</span><span class="sxs-lookup"><span data-stu-id="834c4-268">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="834c4-269">Добавляет в ответ заголовок [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location).</span><span class="sxs-lookup"><span data-stu-id="834c4-269">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="834c4-270">Заголовок `Location` указывает [URI](https://developer.mozilla.org/docs/Glossary/URI) новой созданной задачи.</span><span class="sxs-lookup"><span data-stu-id="834c4-270">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="834c4-271">Дополнительные сведения см. в статье [10.2.2 201 "Создан ресурс"](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="834c4-271">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="834c4-272">Указывает действие `GetTodoItem` для создания URI заголовка `Location`.</span><span class="sxs-lookup"><span data-stu-id="834c4-272">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="834c4-273">Ключевое слово `nameof` C# используется для предотвращения жесткого программирования имени действия в вызове `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="834c4-273">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="834c4-274">Установка Postman</span><span class="sxs-lookup"><span data-stu-id="834c4-274">Install Postman</span></span>

<span data-ttu-id="834c4-275">В этом учебнике для тестирования веб-API используется Postman.</span><span class="sxs-lookup"><span data-stu-id="834c4-275">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="834c4-276">Установка [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="834c4-276">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="834c4-277">Запустите веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="834c4-277">Start the web app.</span></span>
* <span data-ttu-id="834c4-278">Запустите Postman.</span><span class="sxs-lookup"><span data-stu-id="834c4-278">Start Postman.</span></span>
* <span data-ttu-id="834c4-279">Отключение параметра **Проверка SSL-сертификата**</span><span class="sxs-lookup"><span data-stu-id="834c4-279">Disable **SSL certificate verification**</span></span>
* <span data-ttu-id="834c4-280">В меню **Файл > Параметры** (вкладка \**Общие*) отключите параметр **Проверка SSL-сертификата**.</span><span class="sxs-lookup"><span data-stu-id="834c4-280">From  **File > Settings** (\**General* tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="834c4-281">После тестирования контроллера снова включите проверку SSL-сертификата.</span><span class="sxs-lookup"><span data-stu-id="834c4-281">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="834c4-282">Тестирование PostTodoItem с использованием Postman</span><span class="sxs-lookup"><span data-stu-id="834c4-282">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="834c4-283">Создайте новый запрос.</span><span class="sxs-lookup"><span data-stu-id="834c4-283">Create a new request.</span></span>
* <span data-ttu-id="834c4-284">Установите HTTP-метод `POST`.</span><span class="sxs-lookup"><span data-stu-id="834c4-284">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="834c4-285">Откройте вкладку **Тело**.</span><span class="sxs-lookup"><span data-stu-id="834c4-285">Select the **Body** tab.</span></span>
* <span data-ttu-id="834c4-286">Установите переключатель **без обработки**.</span><span class="sxs-lookup"><span data-stu-id="834c4-286">Select the **raw** radio button.</span></span>
* <span data-ttu-id="834c4-287">Задайте тип **JSON (приложение/json)** .</span><span class="sxs-lookup"><span data-stu-id="834c4-287">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="834c4-288">В теле запроса введите код JSON для элемента списка дел:</span><span class="sxs-lookup"><span data-stu-id="834c4-288">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="834c4-289">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="834c4-289">Select **Send**.</span></span>

  ![Postman с запросом Create](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="834c4-291">Тестирование URI заголовка расположения</span><span class="sxs-lookup"><span data-stu-id="834c4-291">Test the location header URI</span></span>

* <span data-ttu-id="834c4-292">Перейдите на вкладку **Заголовки** в области **Ответ**.</span><span class="sxs-lookup"><span data-stu-id="834c4-292">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="834c4-293">Скопируйте значение заголовка **Расположение**:</span><span class="sxs-lookup"><span data-stu-id="834c4-293">Copy the **Location** header value:</span></span>

  ![Вкладка "Заголовки" в консоли Postman](first-web-api/_static/3/create.png)

* <span data-ttu-id="834c4-295">Укажите метод GET.</span><span class="sxs-lookup"><span data-stu-id="834c4-295">Set the method to GET.</span></span>
* <span data-ttu-id="834c4-296">Вставьте URI (например, `https://localhost:5001/api/TodoItems/1`)</span><span class="sxs-lookup"><span data-stu-id="834c4-296">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`)</span></span>
* <span data-ttu-id="834c4-297">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="834c4-297">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="834c4-298">Знакомство с методами GET</span><span class="sxs-lookup"><span data-stu-id="834c4-298">Examine the GET methods</span></span>

<span data-ttu-id="834c4-299">Эти методы реализуют две конечные точки GET:</span><span class="sxs-lookup"><span data-stu-id="834c4-299">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="834c4-300">Протестируйте приложение, вызвав эти две конечные точки в браузере или в Postman.</span><span class="sxs-lookup"><span data-stu-id="834c4-300">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="834c4-301">Например:</span><span class="sxs-lookup"><span data-stu-id="834c4-301">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="834c4-302">При вызове `GetTodoItems` возвращается примерно такой ответ:</span><span class="sxs-lookup"><span data-stu-id="834c4-302">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="834c4-303">Тестирование Get с использованием Postman</span><span class="sxs-lookup"><span data-stu-id="834c4-303">Test Get with Postman</span></span>

* <span data-ttu-id="834c4-304">Создайте новый запрос.</span><span class="sxs-lookup"><span data-stu-id="834c4-304">Create a new request.</span></span>
* <span data-ttu-id="834c4-305">Укажите метод HTTP **GET**.</span><span class="sxs-lookup"><span data-stu-id="834c4-305">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="834c4-306">Укажите URL-адрес запроса `https://localhost:<port>/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="834c4-306">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="834c4-307">Например, `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="834c4-307">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="834c4-308">Выберите режим **Представление с двумя областями** в Postman.</span><span class="sxs-lookup"><span data-stu-id="834c4-308">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="834c4-309">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="834c4-309">Select **Send**.</span></span>

<span data-ttu-id="834c4-310">Это приложение использует выполняющуюся в памяти базу данных.</span><span class="sxs-lookup"><span data-stu-id="834c4-310">This app uses an in-memory database.</span></span> <span data-ttu-id="834c4-311">Если остановить и вновь запустить его, предшествующий запрос GET не возвратит никаких данных.</span><span class="sxs-lookup"><span data-stu-id="834c4-311">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="834c4-312">Если данные не возвращаются, данные для приложения получаются методом [POST](#post).</span><span class="sxs-lookup"><span data-stu-id="834c4-312">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="834c4-313">Маршрутизация и пути URL</span><span class="sxs-lookup"><span data-stu-id="834c4-313">Routing and URL paths</span></span>

<span data-ttu-id="834c4-314">Атрибут [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) обозначает метод, который отвечает на запрос HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="834c4-314">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="834c4-315">Путь URL для каждого метода формируется следующим образом:</span><span class="sxs-lookup"><span data-stu-id="834c4-315">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="834c4-316">Возьмите строку шаблона в атрибуте `Route` контроллера:</span><span class="sxs-lookup"><span data-stu-id="834c4-316">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="834c4-317">Замените `[controller]` именем контроллера (по соглашению это имя класса контроллера без суффикса "Controller").</span><span class="sxs-lookup"><span data-stu-id="834c4-317">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="834c4-318">В этом примере класс контроллера имеет имя **TodoItems**, а сам контроллер, соответственно, — "TodoItems".</span><span class="sxs-lookup"><span data-stu-id="834c4-318">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="834c4-319">В ASP.NET Core [маршрутизация](xref:mvc/controllers/routing) реализуется без учета регистра символов.</span><span class="sxs-lookup"><span data-stu-id="834c4-319">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="834c4-320">Если атрибут `[HttpGet]` имеет шаблон маршрута (например, `[HttpGet("products")]`), добавьте его к пути.</span><span class="sxs-lookup"><span data-stu-id="834c4-320">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="834c4-321">В этом примере шаблон не используется.</span><span class="sxs-lookup"><span data-stu-id="834c4-321">This sample doesn't use a template.</span></span> <span data-ttu-id="834c4-322">Дополнительные сведения см. в разделе [Маршрутизация атрибутов с помощью атрибутов Http[Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="834c4-322">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="834c4-323">В следующем методе `GetTodoItem` `"{id}"` — это переменная-заполнитель для уникального идентификатора элемента задачи.</span><span class="sxs-lookup"><span data-stu-id="834c4-323">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="834c4-324">При вызове `GetTodoItem` параметру метода `id` присваивается значение `"{id}"` в URL-адресе.</span><span class="sxs-lookup"><span data-stu-id="834c4-324">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="834c4-325">Возвращаемые значения</span><span class="sxs-lookup"><span data-stu-id="834c4-325">Return values</span></span>

<span data-ttu-id="834c4-326">Возвращаемое значение имеет тип `GetTodoItems`, а метод `GetTodoItem` имеет тип [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="834c4-326">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="834c4-327">ASP.NET Core автоматически сериализует объект в формат [JSON](https://www.json.org/) и записывает данные JSON в тело сообщения ответа.</span><span class="sxs-lookup"><span data-stu-id="834c4-327">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="834c4-328">Код ответа для этого типа возвращаемого значения равен 200, что свидетельствует об отсутствии необработанных исключений.</span><span class="sxs-lookup"><span data-stu-id="834c4-328">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="834c4-329">Необработанные исключения преобразуются в ошибки 5xx.</span><span class="sxs-lookup"><span data-stu-id="834c4-329">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="834c4-330">Типы возвращаемых значений `ActionResult` могут представлять широкий спектр кодов состояний HTTP.</span><span class="sxs-lookup"><span data-stu-id="834c4-330">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="834c4-331">Например, метод `GetTodoItem` может возвращать два разных значения состояния:</span><span class="sxs-lookup"><span data-stu-id="834c4-331">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="834c4-332">Если запрошенному идентификатору не соответствует ни один элемент, метод возвращает ошибку 404 ([Не найдено](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound)).</span><span class="sxs-lookup"><span data-stu-id="834c4-332">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="834c4-333">В противном случае метод возвращает код 200 с телом ответа JSON.</span><span class="sxs-lookup"><span data-stu-id="834c4-333">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="834c4-334">При возвращении `item` возвращается ответ HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="834c4-334">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="834c4-335">Метод PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="834c4-335">The PutTodoItem method</span></span>

<span data-ttu-id="834c4-336">Проверьте метод `PutTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="834c4-336">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="834c4-337">Страница `PutTodoItem` аналогична странице `PostTodoItem`, но использует запрос HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="834c4-337">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="834c4-338">Ответ — [204 (Нет содержимого)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="834c4-338">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="834c4-339">Согласно спецификации HTTP, запрос PUT требует, чтобы клиент отправлял всю обновленную сущность, а не только изменения.</span><span class="sxs-lookup"><span data-stu-id="834c4-339">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="834c4-340">Чтобы обеспечить поддержку частичных обновлений, используйте [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="834c4-340">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="834c4-341">Если возникнет ошибка вызова `PutTodoItem`, вызовите `GET`, чтобы в базе данных был один элемент.</span><span class="sxs-lookup"><span data-stu-id="834c4-341">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="834c4-342">Тестирование метода PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="834c4-342">Test the PutTodoItem method</span></span>

<span data-ttu-id="834c4-343">Этот пример использует базу данных в памяти, которая должна быть инициирована при каждом запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="834c4-343">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="834c4-344">При выполнении вызова PUT в базе данных уже должен существовать какой-либо элемент.</span><span class="sxs-lookup"><span data-stu-id="834c4-344">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="834c4-345">Для этого перед вызовом PUT выполните вызов GET, чтобы убедиться в наличии такого элемента в базе данных.</span><span class="sxs-lookup"><span data-stu-id="834c4-345">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="834c4-346">Обновите элемент списка дел с идентификатором 1 и присвойте ему имя "feed fish":</span><span class="sxs-lookup"><span data-stu-id="834c4-346">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="834c4-347">На следующем рисунке показан процесс обновления Postman:</span><span class="sxs-lookup"><span data-stu-id="834c4-347">The following image shows the Postman update:</span></span>

![Консоль Postman с ответом 204 (Нет содержимого)](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="834c4-349">Метод DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="834c4-349">The DeleteTodoItem method</span></span>

<span data-ttu-id="834c4-350">Проверьте метод `DeleteTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="834c4-350">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

<span data-ttu-id="834c4-351">`DeleteTodoItem`Ответ[ — 204 (нет содержимого)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="834c4-351">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="834c4-352">Тестирование метода DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="834c4-352">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="834c4-353">Удалите элемент списка дел с помощью Postman:</span><span class="sxs-lookup"><span data-stu-id="834c4-353">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="834c4-354">Укажите метод `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="834c4-354">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="834c4-355">Укажите URI удаляемого объекта, например `https://localhost:5001/api/TodoItems/1`</span><span class="sxs-lookup"><span data-stu-id="834c4-355">Set the URI of the object to delete, for example `https://localhost:5001/api/TodoItems/1`</span></span>
* <span data-ttu-id="834c4-356">Нажмите кнопку **Отправить**</span><span class="sxs-lookup"><span data-stu-id="834c4-356">Select **Send**</span></span>

## <a name="call-the-api-from-jquery"></a><span data-ttu-id="834c4-357">Вызов API из jQuery</span><span class="sxs-lookup"><span data-stu-id="834c4-357">Call the API from jQuery</span></span>

<span data-ttu-id="834c4-358">Пошаговые инструкции см. в [руководстве Вызов веб-API ASP.NET Core с помощью jQuery](xref:tutorials/web-api-jquery).</span><span class="sxs-lookup"><span data-stu-id="834c4-358">See [Tutorial: Call an ASP.NET Core web API with jQuery](xref:tutorials/web-api-jquery).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="834c4-359">В этом руководстве вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="834c4-359">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="834c4-360">Создание проекта веб-API.</span><span class="sxs-lookup"><span data-stu-id="834c4-360">Create a web API project.</span></span>
> * <span data-ttu-id="834c4-361">Добавление класса модели и контекста базы данных.</span><span class="sxs-lookup"><span data-stu-id="834c4-361">Add a model class and a database context.</span></span>
> * <span data-ttu-id="834c4-362">Добавление контроллера.</span><span class="sxs-lookup"><span data-stu-id="834c4-362">Add a controller.</span></span>
> * <span data-ttu-id="834c4-363">Добавление методов CRUD.</span><span class="sxs-lookup"><span data-stu-id="834c4-363">Add CRUD methods.</span></span>
> * <span data-ttu-id="834c4-364">Настройка маршрутизации и путей URL.</span><span class="sxs-lookup"><span data-stu-id="834c4-364">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="834c4-365">Указание возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="834c4-365">Specify return values.</span></span>
> * <span data-ttu-id="834c4-366">Вызов веб-API с помощью Postman.</span><span class="sxs-lookup"><span data-stu-id="834c4-366">Call the web API with Postman.</span></span>
> * <span data-ttu-id="834c4-367">Вызов веб-API с помощью jQuery.</span><span class="sxs-lookup"><span data-stu-id="834c4-367">Call the web API with jQuery.</span></span>

<span data-ttu-id="834c4-368">В конечном итоге вы получите веб-API, обеспечивающий управление элементами списка дел, хранящимися в реляционной базе данных.</span><span class="sxs-lookup"><span data-stu-id="834c4-368">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>
## <a name="overview"></a><span data-ttu-id="834c4-369">Обзор</span><span class="sxs-lookup"><span data-stu-id="834c4-369">Overview</span></span>

<span data-ttu-id="834c4-370">В этом руководстве создается следующий API-интерфейс:</span><span class="sxs-lookup"><span data-stu-id="834c4-370">This tutorial creates the following API:</span></span>

|<span data-ttu-id="834c4-371">API</span><span class="sxs-lookup"><span data-stu-id="834c4-371">API</span></span> | <span data-ttu-id="834c4-372">ОПИСАНИЕ</span><span class="sxs-lookup"><span data-stu-id="834c4-372">Description</span></span> | <span data-ttu-id="834c4-373">Текст запроса</span><span class="sxs-lookup"><span data-stu-id="834c4-373">Request body</span></span> | <span data-ttu-id="834c4-374">Текст ответа</span><span class="sxs-lookup"><span data-stu-id="834c4-374">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="834c4-375">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="834c4-375">GET /api/TodoItems</span></span> | <span data-ttu-id="834c4-376">Получение всех элементов задач</span><span class="sxs-lookup"><span data-stu-id="834c4-376">Get all to-do items</span></span> | <span data-ttu-id="834c4-377">Нет</span><span class="sxs-lookup"><span data-stu-id="834c4-377">None</span></span> | <span data-ttu-id="834c4-378">Массив элементов задач</span><span class="sxs-lookup"><span data-stu-id="834c4-378">Array of to-do items</span></span>|
|<span data-ttu-id="834c4-379">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="834c4-379">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="834c4-380">Получение объекта по идентификатору</span><span class="sxs-lookup"><span data-stu-id="834c4-380">Get an item by ID</span></span> | <span data-ttu-id="834c4-381">Нет</span><span class="sxs-lookup"><span data-stu-id="834c4-381">None</span></span> | <span data-ttu-id="834c4-382">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="834c4-382">To-do item</span></span>|
|<span data-ttu-id="834c4-383">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="834c4-383">POST /api/TodoItems</span></span> | <span data-ttu-id="834c4-384">Добавление нового элемента</span><span class="sxs-lookup"><span data-stu-id="834c4-384">Add a new item</span></span> | <span data-ttu-id="834c4-385">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="834c4-385">To-do item</span></span> | <span data-ttu-id="834c4-386">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="834c4-386">To-do item</span></span> |
|<span data-ttu-id="834c4-387">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="834c4-387">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="834c4-388">Обновление существующего элемента &nbsp;</span><span class="sxs-lookup"><span data-stu-id="834c4-388">Update an existing item &nbsp;</span></span> | <span data-ttu-id="834c4-389">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="834c4-389">To-do item</span></span> | <span data-ttu-id="834c4-390">Нет</span><span class="sxs-lookup"><span data-stu-id="834c4-390">None</span></span> |
|<span data-ttu-id="834c4-391">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="834c4-391">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="834c4-392">Удаление элемента &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="834c4-392">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="834c4-393">Нет</span><span class="sxs-lookup"><span data-stu-id="834c4-393">None</span></span> | <span data-ttu-id="834c4-394">Нет</span><span class="sxs-lookup"><span data-stu-id="834c4-394">None</span></span>|

<span data-ttu-id="834c4-395">На следующем рисунке показана структура приложения.</span><span class="sxs-lookup"><span data-stu-id="834c4-395">The following diagram shows the design of the app.</span></span>

![Клиент представлен прямоугольником слева.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="834c4-401">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="834c4-401">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="834c4-402">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="834c4-402">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="834c4-403">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="834c4-403">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="834c4-404">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="834c4-404">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="834c4-405">Создайте веб-проект.</span><span class="sxs-lookup"><span data-stu-id="834c4-405">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="834c4-406">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="834c4-406">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="834c4-407">В меню **Файл** выберите **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="834c4-407">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="834c4-408">Выберите шаблон **Веб-приложение ASP.NET Core** и нажмите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="834c4-408">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="834c4-409">Назовите проект *TodoApi* и нажмите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="834c4-409">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="834c4-410">В диалоговом окне **Создание веб-приложения ASP.NET Core** убедитесь в том, что выбраны платформы **.NET Core** и **ASP.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="834c4-410">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="834c4-411">Выберите шаблон **API** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="834c4-411">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="834c4-412">**Не** выбирайте **Включение поддержки Docker**.</span><span class="sxs-lookup"><span data-stu-id="834c4-412">**Don't** select **Enable Docker Support**.</span></span>

![Диалоговое окно создания проекта VS](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="834c4-414">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="834c4-414">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="834c4-415">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="834c4-415">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="834c4-416">Смените каталог (`cd`) на папку, в которой будет содержаться папка проекта.</span><span class="sxs-lookup"><span data-stu-id="834c4-416">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="834c4-417">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="834c4-417">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="834c4-418">С помощью этих команд создается новый проект веб-API и открывается новый экземпляр Visual Studio Code в новой папке проекта.</span><span class="sxs-lookup"><span data-stu-id="834c4-418">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="834c4-419">При появлении диалогового окна с запросом на добавление в проект необходимых ресурсов выберите **Да**.</span><span class="sxs-lookup"><span data-stu-id="834c4-419">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="834c4-420">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="834c4-420">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="834c4-421">Выберите **Файл** > **Новое решение**.</span><span class="sxs-lookup"><span data-stu-id="834c4-421">Select **File** > **New Solution**.</span></span>

  ![Новое решение macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="834c4-423">Выберите **.NET Core** > **Приложение** > **API** > **Далее**.</span><span class="sxs-lookup"><span data-stu-id="834c4-423">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![Диалоговое окно "Новый проект" в macOS](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="834c4-425">В диалоговом окне **Настройка нового веб-API ASP.NET Core** оставьте установленное по умолчанию значение для параметра **Целевая платформа**, то есть \* *.NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="834c4-425">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="834c4-426">Введите *TodoApi* в поле **Имя проекта** и выберите команду **Создать**.</span><span class="sxs-lookup"><span data-stu-id="834c4-426">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Диалоговое окно конфигурации](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="834c4-428">Тестирование API</span><span class="sxs-lookup"><span data-stu-id="834c4-428">Test the API</span></span>

<span data-ttu-id="834c4-429">Шаблон проекта создает API `values`.</span><span class="sxs-lookup"><span data-stu-id="834c4-429">The project template creates a `values` API.</span></span> <span data-ttu-id="834c4-430">Вызовите метод `Get` из браузера для тестирования приложения.</span><span class="sxs-lookup"><span data-stu-id="834c4-430">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="834c4-431">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="834c4-431">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="834c4-432">Нажмите клавиши CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="834c4-432">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="834c4-433">Visual Studio запустит браузер и перейдет к `https://localhost:<port>/api/values`, где `<port>` — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="834c4-433">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="834c4-434">Если появится диалоговое окно с запросом о необходимости доверять сертификату IIS Express, выберите **Да**.</span><span class="sxs-lookup"><span data-stu-id="834c4-434">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="834c4-435">В появляющемся следом диалоговом окне **Предупреждение системы безопасности** выберите **Да**.</span><span class="sxs-lookup"><span data-stu-id="834c4-435">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="834c4-436">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="834c4-436">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="834c4-437">Нажмите клавиши CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="834c4-437">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="834c4-438">В браузере перейдите по следующему URL-адресу: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="834c4-438">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="834c4-439">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="834c4-439">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="834c4-440">Выберите **Выполнить** > **Начать отладку**, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="834c4-440">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="834c4-441">Visual Studio для Mac запустит браузер и перейдет к `https://localhost:<port>`, где `<port>` — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="834c4-441">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="834c4-442">Появится ошибка HTTP 404 (Не найдено).</span><span class="sxs-lookup"><span data-stu-id="834c4-442">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="834c4-443">Добавьте `/api/values` к URL-адресу (измените URL-адрес на `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="834c4-443">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="834c4-444">Возвращается следующий файл JSON:</span><span class="sxs-lookup"><span data-stu-id="834c4-444">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="834c4-445">Добавление класса модели</span><span class="sxs-lookup"><span data-stu-id="834c4-445">Add a model class</span></span>

<span data-ttu-id="834c4-446">*Модель* — это набор классов, представляющих данные, которыми управляет приложение.</span><span class="sxs-lookup"><span data-stu-id="834c4-446">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="834c4-447">Модель этого приложения содержит единственный класс `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="834c4-447">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="834c4-448">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="834c4-448">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="834c4-449">В **обозревателе решений** щелкните проект правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="834c4-449">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="834c4-450">Выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="834c4-450">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="834c4-451">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="834c4-451">Name the folder *Models*.</span></span>

* <span data-ttu-id="834c4-452">Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="834c4-452">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="834c4-453">Присвойте классу имя *TodoItem* и выберите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="834c4-453">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="834c4-454">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="834c4-454">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="834c4-455">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="834c4-455">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="834c4-456">Добавьте папку с именем *Models*.</span><span class="sxs-lookup"><span data-stu-id="834c4-456">Add a folder named *Models*.</span></span>

* <span data-ttu-id="834c4-457">Добавьте класс `TodoItem` в папку *Models*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="834c4-457">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="834c4-458">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="834c4-458">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="834c4-459">Щелкните проект правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="834c4-459">Right-click the project.</span></span> <span data-ttu-id="834c4-460">Выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="834c4-460">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="834c4-461">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="834c4-461">Name the folder *Models*.</span></span>

  ![Новая папка](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="834c4-463">Правой кнопкой мыши щелкните папку *Models* и выберите **Добавить** > **Новый файл** > **Общие** > **Пустой класс**.</span><span class="sxs-lookup"><span data-stu-id="834c4-463">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="834c4-464">Назовите класс *TodoItem* и нажмите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="834c4-464">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="834c4-465">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="834c4-465">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="834c4-466">Свойство `Id` выступает в качестве уникального ключа реляционной базы данных.</span><span class="sxs-lookup"><span data-stu-id="834c4-466">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="834c4-467">Классы моделей можно размещать в любом месте проекта, но обычно для этого используется папка *Models*.</span><span class="sxs-lookup"><span data-stu-id="834c4-467">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="834c4-468">Добавление контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="834c4-468">Add a database context</span></span>

<span data-ttu-id="834c4-469">*Контекст базы данных* —это основной класс, который координирует функциональные возможности Entity Framework для модели данных.</span><span class="sxs-lookup"><span data-stu-id="834c4-469">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="834c4-470">Этот класс является производным от класса `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="834c4-470">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="834c4-471">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="834c4-471">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="834c4-472">Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="834c4-472">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="834c4-473">Назовите класс *TodoContext* и нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="834c4-473">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="834c4-474">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="834c4-474">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="834c4-475">Добавьте класс `TodoContext` в папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="834c4-475">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="834c4-476">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="834c4-476">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="834c4-477">Регистрация контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="834c4-477">Register the database context</span></span>

<span data-ttu-id="834c4-478">В ASP.NET Core службы (такие как контекст базы данных) должны быть зарегистрированы с помощью контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="834c4-478">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="834c4-479">Контейнер предоставляет службу контроллерам.</span><span class="sxs-lookup"><span data-stu-id="834c4-479">The container provides the service to controllers.</span></span>

<span data-ttu-id="834c4-480">Обновите файл *Startup.cs*, используя следующий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="834c4-480">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="834c4-481">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="834c4-481">The preceding code:</span></span>

* <span data-ttu-id="834c4-482">Удалите неиспользуемые объявления `using`.</span><span class="sxs-lookup"><span data-stu-id="834c4-482">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="834c4-483">Добавляет контекст базы данных в контейнер внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="834c4-483">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="834c4-484">Указывает, что контекст базы данных будет использовать базу данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="834c4-484">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="834c4-485">Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="834c4-485">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="834c4-486">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="834c4-486">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="834c4-487">Щелкните папку *Controllers* правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="834c4-487">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="834c4-488">Выберите **Добавить** > **Новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="834c4-488">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="834c4-489">В диалоговом окне **Добавить новый элемент** выберите шаблон **Класс контроллера API**.</span><span class="sxs-lookup"><span data-stu-id="834c4-489">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="834c4-490">Присвойте классу имя *TodoController* и выберите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="834c4-490">Name the class *TodoController*, and select **Add**.</span></span>

  ![Диалоговое окно добавления элемента с контроллером в поле поиска и выбранным контроллером веб-API](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="834c4-492">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="834c4-492">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="834c4-493">В папке *Controllers* создайте класс с именем `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="834c4-493">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="834c4-494">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="834c4-494">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="834c4-495">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="834c4-495">The preceding code:</span></span>

* <span data-ttu-id="834c4-496">Определяет класс контроллера API без методов.</span><span class="sxs-lookup"><span data-stu-id="834c4-496">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="834c4-497">Добавляет в класс атрибут [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="834c4-497">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="834c4-498">Этот атрибут указывает, что контроллер отвечает на запросы веб-API.</span><span class="sxs-lookup"><span data-stu-id="834c4-498">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="834c4-499">Дополнительные сведения о поведении, которое реализует этот атрибут, см. в <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="834c4-499">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="834c4-500">Использует внедрение зависимостей для внедрения контекста базы данных (`TodoContext`) в контроллер.</span><span class="sxs-lookup"><span data-stu-id="834c4-500">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="834c4-501">Контекст базы данных используется в каждом методе [создания, чтения, обновления и удаления](https://wikipedia.org/wiki/Create,_read,_update_and_delete) в контроллере.</span><span class="sxs-lookup"><span data-stu-id="834c4-501">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="834c4-502">Добавляет элемент `Item1` в базу данных, если она пуста.</span><span class="sxs-lookup"><span data-stu-id="834c4-502">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="834c4-503">Этот код находится в конструкторе и выполняется каждый раз при обнаружении нового HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="834c4-503">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="834c4-504">Если вы удалите все элементы, конструктор создаст `Item1` при следующем вызове метода API.</span><span class="sxs-lookup"><span data-stu-id="834c4-504">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="834c4-505">Поэтому может создаться впечатление, что удаление не было выполнено, хотя это не так.</span><span class="sxs-lookup"><span data-stu-id="834c4-505">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="834c4-506">Добавление методов Get</span><span class="sxs-lookup"><span data-stu-id="834c4-506">Add Get methods</span></span>

<span data-ttu-id="834c4-507">Чтобы получить API, который извлекает элементы списка дел, добавьте следующие методы в класс `TodoController`:</span><span class="sxs-lookup"><span data-stu-id="834c4-507">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="834c4-508">Эти методы реализуют две конечные точки GET:</span><span class="sxs-lookup"><span data-stu-id="834c4-508">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="834c4-509">Если приложение все еще выполняется, оно останавливается.</span><span class="sxs-lookup"><span data-stu-id="834c4-509">Stop the app if it's still running.</span></span> <span data-ttu-id="834c4-510">Затем оно запускается снова с последними изменениями.</span><span class="sxs-lookup"><span data-stu-id="834c4-510">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="834c4-511">Протестируйте приложение, вызвав эти две конечные точки в браузере.</span><span class="sxs-lookup"><span data-stu-id="834c4-511">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="834c4-512">Например:</span><span class="sxs-lookup"><span data-stu-id="834c4-512">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="834c4-513">При вызове `GetTodoItems` возвращается следующий ответ HTTP:</span><span class="sxs-lookup"><span data-stu-id="834c4-513">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="834c4-514">Маршрутизация и пути URL</span><span class="sxs-lookup"><span data-stu-id="834c4-514">Routing and URL paths</span></span>

<span data-ttu-id="834c4-515">Атрибут [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) обозначает метод, который отвечает на запрос HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="834c4-515">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="834c4-516">Путь URL для каждого метода формируется следующим образом:</span><span class="sxs-lookup"><span data-stu-id="834c4-516">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="834c4-517">Возьмите строку шаблона в атрибуте `Route` контроллера:</span><span class="sxs-lookup"><span data-stu-id="834c4-517">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="834c4-518">Замените `[controller]` именем контроллера (по соглашению это имя класса контроллера без суффикса "Controller").</span><span class="sxs-lookup"><span data-stu-id="834c4-518">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="834c4-519">В этом примере класс контроллера носит имя **Todo**, а сам контроллер, соответственно, — "todo".</span><span class="sxs-lookup"><span data-stu-id="834c4-519">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="834c4-520">В ASP.NET Core [маршрутизация](xref:mvc/controllers/routing) реализуется без учета регистра символов.</span><span class="sxs-lookup"><span data-stu-id="834c4-520">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="834c4-521">Если атрибут `[HttpGet]` имеет шаблон маршрута (например, `[HttpGet("products")]`), добавьте его к пути.</span><span class="sxs-lookup"><span data-stu-id="834c4-521">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="834c4-522">В этом примере шаблон не используется.</span><span class="sxs-lookup"><span data-stu-id="834c4-522">This sample doesn't use a template.</span></span> <span data-ttu-id="834c4-523">Дополнительные сведения см. в разделе [Маршрутизация атрибутов с помощью атрибутов Http[Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="834c4-523">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="834c4-524">В следующем методе `GetTodoItem` `"{id}"` — это переменная-заполнитель для уникального идентификатора элемента задачи.</span><span class="sxs-lookup"><span data-stu-id="834c4-524">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="834c4-525">При вызове `GetTodoItem` параметру метода `id` присваивается значение `"{id}"` в URL-адресе.</span><span class="sxs-lookup"><span data-stu-id="834c4-525">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="834c4-526">Возвращаемые значения</span><span class="sxs-lookup"><span data-stu-id="834c4-526">Return values</span></span>

<span data-ttu-id="834c4-527">Возвращаемое значение имеет тип `GetTodoItems`, а метод `GetTodoItem` имеет тип [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="834c4-527">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="834c4-528">ASP.NET Core автоматически сериализует объект в формат [JSON](https://www.json.org/) и записывает данные JSON в тело сообщения ответа.</span><span class="sxs-lookup"><span data-stu-id="834c4-528">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="834c4-529">Код ответа для этого типа возвращаемого значения равен 200, что свидетельствует об отсутствии необработанных исключений.</span><span class="sxs-lookup"><span data-stu-id="834c4-529">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="834c4-530">Необработанные исключения преобразуются в ошибки 5xx.</span><span class="sxs-lookup"><span data-stu-id="834c4-530">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="834c4-531">Типы возвращаемых значений `ActionResult` могут представлять широкий спектр кодов состояний HTTP.</span><span class="sxs-lookup"><span data-stu-id="834c4-531">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="834c4-532">Например, метод `GetTodoItem` может возвращать два разных значения состояния:</span><span class="sxs-lookup"><span data-stu-id="834c4-532">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="834c4-533">Если запрошенному идентификатору не соответствует ни один элемент, метод возвращает ошибку 404 ([Не найдено](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound)).</span><span class="sxs-lookup"><span data-stu-id="834c4-533">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="834c4-534">В противном случае метод возвращает код 200 с телом ответа JSON.</span><span class="sxs-lookup"><span data-stu-id="834c4-534">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="834c4-535">При возвращении `item` возвращается ответ HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="834c4-535">Returning `item` results in an HTTP 200 response.</span></span>


## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="834c4-536">Тестирование метода GetTodoItems</span><span class="sxs-lookup"><span data-stu-id="834c4-536">Test the GetTodoItems method</span></span>

<span data-ttu-id="834c4-537">В этом учебнике для тестирования веб-API используется Postman.</span><span class="sxs-lookup"><span data-stu-id="834c4-537">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="834c4-538">Установка [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="834c4-538">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="834c4-539">Запустите веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="834c4-539">Start the web app.</span></span>
* <span data-ttu-id="834c4-540">Запустите Postman.</span><span class="sxs-lookup"><span data-stu-id="834c4-540">Start Postman.</span></span>
* <span data-ttu-id="834c4-541">Отключение параметра **Проверка SSL-сертификата**</span><span class="sxs-lookup"><span data-stu-id="834c4-541">Disable **SSL certificate verification**</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="834c4-542">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="834c4-542">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="834c4-543">В меню **Файл** > **Параметры** (вкладка **Общие**) отключите параметр **Проверка SSL-сертификата**.</span><span class="sxs-lookup"><span data-stu-id="834c4-543">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="834c4-544">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="834c4-544">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="834c4-545">В меню **Postman**  >  **Настройки** (вкладка **Общие**) отключите параметр **Проверка SSL-сертификата**.</span><span class="sxs-lookup"><span data-stu-id="834c4-545">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="834c4-546">Кроме того, можно выбрать значок гаечного ключа и выбрать **Параметры**, а затем отключить проверку SSL-сертификата.</span><span class="sxs-lookup"><span data-stu-id="834c4-546">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="834c4-547">После тестирования контроллера снова включите проверку SSL-сертификата.</span><span class="sxs-lookup"><span data-stu-id="834c4-547">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="834c4-548">Создайте новый запрос.</span><span class="sxs-lookup"><span data-stu-id="834c4-548">Create a new request.</span></span>
  * <span data-ttu-id="834c4-549">Укажите метод HTTP **GET**.</span><span class="sxs-lookup"><span data-stu-id="834c4-549">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="834c4-550">Укажите URL-адрес запроса `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="834c4-550">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="834c4-551">Например, `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="834c4-551">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="834c4-552">Выберите режим **Представление с двумя областями** в Postman.</span><span class="sxs-lookup"><span data-stu-id="834c4-552">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="834c4-553">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="834c4-553">Select **Send**.</span></span>

![Postman с запросом Get](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="834c4-555">Добавление метода Create</span><span class="sxs-lookup"><span data-stu-id="834c4-555">Add a Create method</span></span>

<span data-ttu-id="834c4-556">Добавьте следующий метод `PostTodoItem` в *Controllers/TodoController.cs*:</span><span class="sxs-lookup"><span data-stu-id="834c4-556">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="834c4-557">Предыдущий код является методом HTTP POST, как указывает атрибут [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="834c4-557">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="834c4-558">Этот метод получает значение элемента списка дел из текста HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="834c4-558">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="834c4-559">Метод `CreatedAtAction`:</span><span class="sxs-lookup"><span data-stu-id="834c4-559">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="834c4-560">В случае успеха возвращает код состояния HTTP 201.</span><span class="sxs-lookup"><span data-stu-id="834c4-560">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="834c4-561">HTTP 201 представляет собой стандартный ответ для метода HTTP POST, создающий ресурс на сервере.</span><span class="sxs-lookup"><span data-stu-id="834c4-561">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="834c4-562">Добавляет заголовок `Location` в ответ.</span><span class="sxs-lookup"><span data-stu-id="834c4-562">Adds a `Location` header to the response.</span></span> <span data-ttu-id="834c4-563">Заголовок `Location` расположения указывает URI вновь созданной задачи.</span><span class="sxs-lookup"><span data-stu-id="834c4-563">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="834c4-564">Дополнительные сведения см. в статье [10.2.2 201 "Создан ресурс"](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="834c4-564">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="834c4-565">Указывает действие `GetTodoItem` для создания URI заголовка `Location`.</span><span class="sxs-lookup"><span data-stu-id="834c4-565">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="834c4-566">Ключевое слово `nameof` C# используется для предотвращения жесткого программирования имени действия в вызове `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="834c4-566">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="834c4-567">Тестирование метода PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="834c4-567">Test the PostTodoItem method</span></span>

* <span data-ttu-id="834c4-568">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="834c4-568">Build the project.</span></span>
* <span data-ttu-id="834c4-569">В Postman укажите метод HTTP `POST`.</span><span class="sxs-lookup"><span data-stu-id="834c4-569">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="834c4-570">Откройте вкладку **Тело**.</span><span class="sxs-lookup"><span data-stu-id="834c4-570">Select the **Body** tab.</span></span>
* <span data-ttu-id="834c4-571">Установите переключатель **без обработки**.</span><span class="sxs-lookup"><span data-stu-id="834c4-571">Select the **raw** radio button.</span></span>
* <span data-ttu-id="834c4-572">Задайте тип **JSON (приложение/json)** .</span><span class="sxs-lookup"><span data-stu-id="834c4-572">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="834c4-573">В теле запроса введите код JSON для элемента списка дел:</span><span class="sxs-lookup"><span data-stu-id="834c4-573">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="834c4-574">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="834c4-574">Select **Send**.</span></span>

  ![Postman с запросом Create](first-web-api/_static/create.png)

  <span data-ttu-id="834c4-576">Если вы получаете ошибку 405 "Недопустимый метод", это может свидетельствовать о том, что после добавления метода `PostTodoItem` не была выполнена компиляция проекта.</span><span class="sxs-lookup"><span data-stu-id="834c4-576">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="834c4-577">Тестирование URI заголовка расположения</span><span class="sxs-lookup"><span data-stu-id="834c4-577">Test the location header URI</span></span>

* <span data-ttu-id="834c4-578">Перейдите на вкладку **Заголовки** в области **Ответ**.</span><span class="sxs-lookup"><span data-stu-id="834c4-578">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="834c4-579">Скопируйте значение заголовка **Расположение**:</span><span class="sxs-lookup"><span data-stu-id="834c4-579">Copy the **Location** header value:</span></span>

  ![Вкладка "Заголовки" в консоли Postman](first-web-api/_static/pmc2.png)

* <span data-ttu-id="834c4-581">Укажите метод GET.</span><span class="sxs-lookup"><span data-stu-id="834c4-581">Set the method to GET.</span></span>
* <span data-ttu-id="834c4-582">Вставьте URI (например, `https://localhost:5001/api/Todo/2`)</span><span class="sxs-lookup"><span data-stu-id="834c4-582">Paste the URI (for example, `https://localhost:5001/api/Todo/2`)</span></span>
* <span data-ttu-id="834c4-583">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="834c4-583">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="834c4-584">Добавление метода PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="834c4-584">Add a PutTodoItem method</span></span>

<span data-ttu-id="834c4-585">Добавьте приведенный ниже метод `PutTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="834c4-585">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="834c4-586">Страница `PutTodoItem` аналогична странице `PostTodoItem`, но использует запрос HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="834c4-586">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="834c4-587">Ответ — [204 (Нет содержимого)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="834c4-587">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="834c4-588">Согласно спецификации HTTP, запрос PUT требует, чтобы клиент отправлял всю обновленную сущность, а не только изменения.</span><span class="sxs-lookup"><span data-stu-id="834c4-588">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="834c4-589">Чтобы обеспечить поддержку частичных обновлений, используйте [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="834c4-589">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="834c4-590">Если возникнет ошибка вызова `PutTodoItem`, вызовите `GET`, чтобы в базе данных был один элемент.</span><span class="sxs-lookup"><span data-stu-id="834c4-590">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="834c4-591">Тестирование метода PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="834c4-591">Test the PutTodoItem method</span></span>

<span data-ttu-id="834c4-592">Этот пример использует базу данных в памяти, которая должна быть инициирована при каждом запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="834c4-592">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="834c4-593">При выполнении вызова PUT в базе данных уже должен существовать какой-либо элемент.</span><span class="sxs-lookup"><span data-stu-id="834c4-593">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="834c4-594">Для этого перед вызовом PUT выполните вызов GET, чтобы убедиться в наличии такого элемента в базе данных.</span><span class="sxs-lookup"><span data-stu-id="834c4-594">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="834c4-595">Обновите элемент списка дел с идентификатором = 1 и присвойте ему имя "feed fish":</span><span class="sxs-lookup"><span data-stu-id="834c4-595">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="834c4-596">На следующем рисунке показан процесс обновления Postman:</span><span class="sxs-lookup"><span data-stu-id="834c4-596">The following image shows the Postman update:</span></span>

![Консоль Postman с ответом 204 (Нет содержимого)](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="834c4-598">Добавление метода DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="834c4-598">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="834c4-599">Добавьте приведенный ниже метод `DeleteTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="834c4-599">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="834c4-600">`DeleteTodoItem`Ответ[ — 204 (нет содержимого)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="834c4-600">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="834c4-601">Тестирование метода DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="834c4-601">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="834c4-602">Удалите элемент списка дел с помощью Postman:</span><span class="sxs-lookup"><span data-stu-id="834c4-602">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="834c4-603">Укажите метод `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="834c4-603">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="834c4-604">Укажите URI удаляемого объекта, например `https://localhost:5001/api/todo/1`</span><span class="sxs-lookup"><span data-stu-id="834c4-604">Set the URI of the object to delete, for example `https://localhost:5001/api/todo/1`</span></span>
* <span data-ttu-id="834c4-605">Нажмите кнопку **Отправить**</span><span class="sxs-lookup"><span data-stu-id="834c4-605">Select **Send**</span></span>

<span data-ttu-id="834c4-606">В этом примере приложения вы можете удалить все элементы.</span><span class="sxs-lookup"><span data-stu-id="834c4-606">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="834c4-607">Однако в случае удаления последнего элемента в момент следующего вызова API конструктор класса модели создаст новый элемент.</span><span class="sxs-lookup"><span data-stu-id="834c4-607">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-api-with-jquery"></a><span data-ttu-id="834c4-608">Вызов API с помощью jQuery</span><span class="sxs-lookup"><span data-stu-id="834c4-608">Call the API with jQuery</span></span>

<span data-ttu-id="834c4-609">В этом разделе добавляется HTML-страница, которая использует jQuery для вызова веб-API.</span><span class="sxs-lookup"><span data-stu-id="834c4-609">In this section, an HTML page is added that uses jQuery to call the web api.</span></span> <span data-ttu-id="834c4-610">jQuery запускает запрос и вносит на страницу подробные сведения из ответа API.</span><span class="sxs-lookup"><span data-stu-id="834c4-610">jQuery initiates the request and updates the page with the details from the API's response.</span></span>

<span data-ttu-id="834c4-611">Настройте приложение для [обслуживания статических файлов](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) и [включения сопоставления файлов по умолчанию](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_), обновив *Startup.cs* следующим выделенным кодом:</span><span class="sxs-lookup"><span data-stu-id="834c4-611">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="834c4-612">Создайте папку *wwwroot* в каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="834c4-612">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="834c4-613">Добавьте HTML-файл *index.html* в каталог *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="834c4-613">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="834c4-614">Замените его содержимое следующей разметкой:</span><span class="sxs-lookup"><span data-stu-id="834c4-614">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="834c4-615">Добавьте файл JavaScript с именем *site.js* в каталог *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="834c4-615">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="834c4-616">Замените его содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="834c4-616">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="834c4-617">Может потребоваться изменение параметров запуска проекта ASP.NET Core для локального тестирования HTML-страницы:</span><span class="sxs-lookup"><span data-stu-id="834c4-617">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="834c4-618">Откройте файл *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="834c4-618">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="834c4-619">Удалите свойство `launchUrl`, чтобы приложение открылось через *index.html* &mdash; файл проекта по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="834c4-619">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="834c4-620">Для получения jQuery можно использовать следующие способы.</span><span class="sxs-lookup"><span data-stu-id="834c4-620">There are several ways to get jQuery.</span></span> <span data-ttu-id="834c4-621">В предыдущем фрагменте кода библиотека загружается из CDN.</span><span class="sxs-lookup"><span data-stu-id="834c4-621">In the preceding snippet, the library is loaded from a CDN.</span></span>

<span data-ttu-id="834c4-622">В этом примере вызываются все методы CRUD в API.</span><span class="sxs-lookup"><span data-stu-id="834c4-622">This sample calls all of the CRUD methods of the API.</span></span> <span data-ttu-id="834c4-623">Ниже приводится пояснение вызовов API.</span><span class="sxs-lookup"><span data-stu-id="834c4-623">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="834c4-624">Получение списка элементов задач</span><span class="sxs-lookup"><span data-stu-id="834c4-624">Get a list of to-do items</span></span>

<span data-ttu-id="834c4-625">Функция jQuery [ajax](https://api.jquery.com/jquery.ajax/) отправляет запрос `GET` к API, который возвращает JSON, представляющий массив элементов списка дел.</span><span class="sxs-lookup"><span data-stu-id="834c4-625">The jQuery [ajax](https://api.jquery.com/jquery.ajax/) function sends a `GET` request to the API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="834c4-626">В случае успешного запроса используется функция обратного вызова `success`.</span><span class="sxs-lookup"><span data-stu-id="834c4-626">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="834c4-627">При обратном вызове в модель DOM вносятся данные о задачах.</span><span class="sxs-lookup"><span data-stu-id="834c4-627">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="834c4-628">Добавление элемента задачи</span><span class="sxs-lookup"><span data-stu-id="834c4-628">Add a to-do item</span></span>

<span data-ttu-id="834c4-629">Функция [ajax](https://api.jquery.com/jquery.ajax/) отправляет запрос `POST` с элементом списка дел в теле запроса.</span><span class="sxs-lookup"><span data-stu-id="834c4-629">The [ajax](https://api.jquery.com/jquery.ajax/) function sends a `POST` request with the to-do item in the request body.</span></span> <span data-ttu-id="834c4-630">Для параметров `accepts` и `contentType` указывается `application/json`, чтобы указать тип носителя при получении и отправке.</span><span class="sxs-lookup"><span data-stu-id="834c4-630">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="834c4-631">Элемент списка дел преобразуется в JSON с помощью [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="834c4-631">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="834c4-632">Если интерфейс API возвращает код состояния успешного выполнения, вызывается функция `getData` для обновления HTML-таблицы.</span><span class="sxs-lookup"><span data-stu-id="834c4-632">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="834c4-633">Обновление элемента задачи</span><span class="sxs-lookup"><span data-stu-id="834c4-633">Update a to-do item</span></span>

<span data-ttu-id="834c4-634">Обновление элемента списка дел выполняется аналогично его добавлению.</span><span class="sxs-lookup"><span data-stu-id="834c4-634">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="834c4-635">`url` изменяется, чтобы добавить уникальный идентификатор для элемента, а в качестве `type` устанавливается `PUT`.</span><span class="sxs-lookup"><span data-stu-id="834c4-635">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="834c4-636">Удаление элемента задачи</span><span class="sxs-lookup"><span data-stu-id="834c4-636">Delete a to-do item</span></span>

<span data-ttu-id="834c4-637">Чтобы удалить элемент списка дел, установите для `type` в вызове AJAX значение `DELETE` и укажите уникальный идентификатор элемента в URL-адресе.</span><span class="sxs-lookup"><span data-stu-id="834c4-637">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="834c4-638">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="834c4-638">Additional resources</span></span>

<span data-ttu-id="834c4-639">[Просмотреть или скачать пример кода для этого учебника](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="834c4-639">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="834c4-640">См. раздел [Практическое руководство. Скачивание файла](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="834c4-640">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="834c4-641">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="834c4-641">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/index>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="834c4-642">Версия руководства на YouTube</span><span class="sxs-lookup"><span data-stu-id="834c4-642">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
