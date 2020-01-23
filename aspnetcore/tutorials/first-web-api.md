---
title: Учебник. Создание веб-API с помощью ASP.NET Core
author: rick-anderson
description: Узнайте, как создать веб-API с помощью ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 73e547b014d78dcbcbf1c887ebec16e0743d10b9
ms.sourcegitcommit: f259889044d1fc0f0c7e3882df0008157ced4915
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/21/2020
ms.locfileid: "76294751"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="08387-103">Учебник. Создание веб-API с помощью ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="08387-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="08387-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Майк Уоссон](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="08387-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="08387-105">В этом учебнике приводятся основные сведения о создании веб-API с помощью ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="08387-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="08387-106">В этом руководстве вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="08387-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="08387-107">Создание проекта веб-API.</span><span class="sxs-lookup"><span data-stu-id="08387-107">Create a web API project.</span></span>
> * <span data-ttu-id="08387-108">Добавление класса модели и контекста базы данных.</span><span class="sxs-lookup"><span data-stu-id="08387-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="08387-109">Формирование шаблонов контроллера с использованием методов CRUD.</span><span class="sxs-lookup"><span data-stu-id="08387-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="08387-110">Настройка маршрутизации, URL-пути и возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="08387-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="08387-111">Вызов веб-API с помощью Postman.</span><span class="sxs-lookup"><span data-stu-id="08387-111">Call the web API with Postman.</span></span>

<span data-ttu-id="08387-112">В итоге вы получите веб-API, позволяющий работать с элементами списка дел, хранимыми в базе данных.</span><span class="sxs-lookup"><span data-stu-id="08387-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="08387-113">Обзор</span><span class="sxs-lookup"><span data-stu-id="08387-113">Overview</span></span>

<span data-ttu-id="08387-114">В этом руководстве создается следующий API-интерфейс:</span><span class="sxs-lookup"><span data-stu-id="08387-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="08387-115">API</span><span class="sxs-lookup"><span data-stu-id="08387-115">API</span></span> | <span data-ttu-id="08387-116">Описание</span><span class="sxs-lookup"><span data-stu-id="08387-116">Description</span></span> | <span data-ttu-id="08387-117">Текст запроса</span><span class="sxs-lookup"><span data-stu-id="08387-117">Request body</span></span> | <span data-ttu-id="08387-118">Текст ответа</span><span class="sxs-lookup"><span data-stu-id="08387-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="08387-119">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="08387-119">GET /api/TodoItems</span></span> | <span data-ttu-id="08387-120">Получение всех элементов задач</span><span class="sxs-lookup"><span data-stu-id="08387-120">Get all to-do items</span></span> | <span data-ttu-id="08387-121">Отсутствуют</span><span class="sxs-lookup"><span data-stu-id="08387-121">None</span></span> | <span data-ttu-id="08387-122">Массив элементов задач</span><span class="sxs-lookup"><span data-stu-id="08387-122">Array of to-do items</span></span>|
|<span data-ttu-id="08387-123">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="08387-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="08387-124">Получение объекта по идентификатору</span><span class="sxs-lookup"><span data-stu-id="08387-124">Get an item by ID</span></span> | <span data-ttu-id="08387-125">Отсутствуют</span><span class="sxs-lookup"><span data-stu-id="08387-125">None</span></span> | <span data-ttu-id="08387-126">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="08387-126">To-do item</span></span>|
|<span data-ttu-id="08387-127">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="08387-127">POST /api/TodoItems</span></span> | <span data-ttu-id="08387-128">Добавление нового элемента</span><span class="sxs-lookup"><span data-stu-id="08387-128">Add a new item</span></span> | <span data-ttu-id="08387-129">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="08387-129">To-do item</span></span> | <span data-ttu-id="08387-130">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="08387-130">To-do item</span></span> |
|<span data-ttu-id="08387-131">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="08387-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="08387-132">Обновление существующего элемента &nbsp;</span><span class="sxs-lookup"><span data-stu-id="08387-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="08387-133">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="08387-133">To-do item</span></span> | <span data-ttu-id="08387-134">Отсутствуют</span><span class="sxs-lookup"><span data-stu-id="08387-134">None</span></span> |
|<span data-ttu-id="08387-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="08387-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="08387-136">Удаление элемента &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="08387-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="08387-137">Отсутствуют</span><span class="sxs-lookup"><span data-stu-id="08387-137">None</span></span> | <span data-ttu-id="08387-138">Отсутствуют</span><span class="sxs-lookup"><span data-stu-id="08387-138">None</span></span>|

<span data-ttu-id="08387-139">На следующем рисунке показана структура приложения.</span><span class="sxs-lookup"><span data-stu-id="08387-139">The following diagram shows the design of the app.</span></span>

![Клиент представлен прямоугольником слева.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="08387-145">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="08387-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="08387-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="08387-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="08387-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="08387-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="08387-148">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="08387-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="08387-149">Создайте веб-проект.</span><span class="sxs-lookup"><span data-stu-id="08387-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="08387-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="08387-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="08387-151">В меню **Файл** выберите пункт **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="08387-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="08387-152">Выберите шаблон **Веб-приложение ASP.NET Core** и нажмите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="08387-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="08387-153">Назовите проект *TodoApi* и нажмите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="08387-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="08387-154">В диалоговом окне **Создание веб-приложения ASP.NET Core** убедитесь в том, что выбраны платформы **.NET Core** и **ASP.NET Core 3.1**.</span><span class="sxs-lookup"><span data-stu-id="08387-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.1** are selected.</span></span> <span data-ttu-id="08387-155">Выберите шаблон **API** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="08387-155">Select the **API** template and click **Create**.</span></span>

![Диалоговое окно создания проекта VS](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="08387-157">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="08387-157">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="08387-158">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="08387-158">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="08387-159">Смените каталог (`cd`) на папку, в которой будет содержаться папка проекта.</span><span class="sxs-lookup"><span data-stu-id="08387-159">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="08387-160">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="08387-160">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* <span data-ttu-id="08387-161">При появлении диалогового окна с запросом на добавление в проект необходимых ресурсов выберите **Да**.</span><span class="sxs-lookup"><span data-stu-id="08387-161">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="08387-162">Предыдущие команды:</span><span class="sxs-lookup"><span data-stu-id="08387-162">The preceding commands:</span></span>

  * <span data-ttu-id="08387-163">Создает новый проект веб-API и открывает его в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="08387-163">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="08387-164">Добавляет пакеты NuGet, которые понадобятся в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="08387-164">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="08387-165">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="08387-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="08387-166">Щелкните **Файл** > **Новое решение**.</span><span class="sxs-lookup"><span data-stu-id="08387-166">Select **File** > **New Solution**.</span></span>

  ![Новое решение macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="08387-168">Щелкните **.NET Core** > **Приложение** > **API** > **Далее**.</span><span class="sxs-lookup"><span data-stu-id="08387-168">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![Диалоговое окно "Новый проект" в macOS](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="08387-170">В диалоговом окне **Настройка нового веб-API ASP.NET Core** в качестве **Целевой платформы** выберите \* *.NET Core 3.1*.</span><span class="sxs-lookup"><span data-stu-id="08387-170">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.1*.</span></span>

* <span data-ttu-id="08387-171">Введите *TodoApi* в поле **Имя проекта** и выберите команду **Создать**.</span><span class="sxs-lookup"><span data-stu-id="08387-171">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Диалоговое окно конфигурации](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="08387-173">Откройте командный терминал в папке проекта и выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="08387-173">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="08387-174">Тестирование API</span><span class="sxs-lookup"><span data-stu-id="08387-174">Test the API</span></span>

<span data-ttu-id="08387-175">Шаблон проекта создает API `WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="08387-175">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="08387-176">Вызовите метод `Get` из браузера для тестирования приложения.</span><span class="sxs-lookup"><span data-stu-id="08387-176">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="08387-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="08387-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="08387-178">Нажмите клавиши CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="08387-178">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="08387-179">Visual Studio запустит браузер и перейдет к `https://localhost:<port>/WeatherForecast`, где `<port>` — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="08387-179">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="08387-180">Если появится диалоговое окно с запросом о необходимости доверять сертификату IIS Express, выберите **Да**.</span><span class="sxs-lookup"><span data-stu-id="08387-180">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="08387-181">В появляющемся следом диалоговом окне **Предупреждение системы безопасности** выберите **Да**.</span><span class="sxs-lookup"><span data-stu-id="08387-181">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="08387-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="08387-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="08387-183">Нажмите клавиши CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="08387-183">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="08387-184">В браузере перейдите по следующему URL-адресу: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="08387-184">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="08387-185">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="08387-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="08387-186">Выберите **Выполнить** > **Начать отладку**, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="08387-186">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="08387-187">Visual Studio для Mac запустит браузер и перейдет к `https://localhost:<port>`, где `<port>` — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="08387-187">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="08387-188">Появится ошибка HTTP 404 (Не найдено).</span><span class="sxs-lookup"><span data-stu-id="08387-188">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="08387-189">Добавьте `/WeatherForecast` к URL-адресу (измените URL-адрес на `https://localhost:<port>/WeatherForecast`).</span><span class="sxs-lookup"><span data-stu-id="08387-189">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="08387-190">Возвращаемые данные JSON будут выглядеть примерно так:</span><span class="sxs-lookup"><span data-stu-id="08387-190">JSON similar to the following is returned:</span></span>

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

## <a name="add-a-model-class"></a><span data-ttu-id="08387-191">Добавление класса модели</span><span class="sxs-lookup"><span data-stu-id="08387-191">Add a model class</span></span>

<span data-ttu-id="08387-192">*Модель* — это набор классов, представляющих данные, которыми управляет приложение.</span><span class="sxs-lookup"><span data-stu-id="08387-192">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="08387-193">Модель этого приложения содержит единственный класс `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="08387-193">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="08387-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="08387-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="08387-195">В **обозревателе решений** щелкните проект правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="08387-195">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="08387-196">Выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="08387-196">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="08387-197">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="08387-197">Name the folder *Models*.</span></span>

* <span data-ttu-id="08387-198">Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="08387-198">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="08387-199">Присвойте классу имя *TodoItem* и выберите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="08387-199">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="08387-200">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="08387-200">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="08387-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="08387-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="08387-202">Добавьте папку с именем *Models*.</span><span class="sxs-lookup"><span data-stu-id="08387-202">Add a folder named *Models*.</span></span>

* <span data-ttu-id="08387-203">Добавьте класс `TodoItem` в папку *Models*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="08387-203">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="08387-204">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="08387-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="08387-205">Щелкните проект правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="08387-205">Right-click the project.</span></span> <span data-ttu-id="08387-206">Выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="08387-206">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="08387-207">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="08387-207">Name the folder *Models*.</span></span>

  ![Новая папка](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="08387-209">Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Создать файл** > **Общие** > **Пустой класс**.</span><span class="sxs-lookup"><span data-stu-id="08387-209">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="08387-210">Назовите класс *TodoItem* и нажмите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="08387-210">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="08387-211">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="08387-211">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="08387-212">Свойство `Id` выступает в качестве уникального ключа реляционной базы данных.</span><span class="sxs-lookup"><span data-stu-id="08387-212">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="08387-213">Классы моделей можно размещать в любом месте проекта, но обычно для этого используется папка *Models*.</span><span class="sxs-lookup"><span data-stu-id="08387-213">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="08387-214">Добавление контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="08387-214">Add a database context</span></span>

<span data-ttu-id="08387-215">*Контекст базы данных* —это основной класс, который координирует функциональные возможности Entity Framework для модели данных.</span><span class="sxs-lookup"><span data-stu-id="08387-215">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="08387-216">Этот класс является производным от класса `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="08387-216">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="08387-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="08387-217">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="08387-218">Добавление Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="08387-218">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="08387-219">В меню **Сервис** выберите **Диспетчер пакетов NuGet > Управление пакетами NuGet для решения**.</span><span class="sxs-lookup"><span data-stu-id="08387-219">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="08387-220">Перейдите на вкладку **Обзор** и введите **Microsoft.EntityFrameworkCore.SqlServer** в поле поиска.</span><span class="sxs-lookup"><span data-stu-id="08387-220">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="08387-221">На панели слева выберите **Microsoft.EntityFrameworkCore.SqlServer**.</span><span class="sxs-lookup"><span data-stu-id="08387-221">Select **Microsoft.EntityFrameworkCore.SqlServer** in the left pane.</span></span>
* <span data-ttu-id="08387-222">Установите флажок **Проект** на правой панели и выберите **Установить**.</span><span class="sxs-lookup"><span data-stu-id="08387-222">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="08387-223">Добавьте пакет NuGet `Microsoft.EntityFrameworkCore.InMemory` согласно инструкциям выше.</span><span class="sxs-lookup"><span data-stu-id="08387-223">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![Диспетчер пакетов NuGet](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="08387-225">Добавление контекста базы данных TodoContext</span><span class="sxs-lookup"><span data-stu-id="08387-225">Add the TodoContext database context</span></span>

* <span data-ttu-id="08387-226">Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="08387-226">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="08387-227">Назовите класс *TodoContext* и нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="08387-227">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="08387-228">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="08387-228">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="08387-229">Добавьте класс `TodoContext` в папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="08387-229">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="08387-230">Введите следующий код:</span><span class="sxs-lookup"><span data-stu-id="08387-230">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="08387-231">Регистрация контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="08387-231">Register the database context</span></span>

<span data-ttu-id="08387-232">В ASP.NET Core службы (такие как контекст базы данных) должны быть зарегистрированы с помощью контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="08387-232">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="08387-233">Контейнер предоставляет службу контроллерам.</span><span class="sxs-lookup"><span data-stu-id="08387-233">The container provides the service to controllers.</span></span>

<span data-ttu-id="08387-234">Обновите файл *Startup.cs*, используя следующий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="08387-234">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="08387-235">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="08387-235">The preceding code:</span></span>

* <span data-ttu-id="08387-236">Удалите неиспользуемые объявления `using`.</span><span class="sxs-lookup"><span data-stu-id="08387-236">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="08387-237">Добавляет контекст базы данных в контейнер внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="08387-237">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="08387-238">Указывает, что контекст базы данных будет использовать базу данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="08387-238">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="08387-239">Формирование шаблонов контроллера</span><span class="sxs-lookup"><span data-stu-id="08387-239">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="08387-240">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="08387-240">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="08387-241">Щелкните папку *Controllers* правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="08387-241">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="08387-242">Щелкните **Добавить** > **Создать шаблонный элемент**.</span><span class="sxs-lookup"><span data-stu-id="08387-242">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="08387-243">Выберите **Контроллер API с действиями, использующий Entity Framework**, а затем выберите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="08387-243">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="08387-244">В диалоговом окне **Контроллер API с действиями, использующий Entity Framework** сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="08387-244">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="08387-245">Выберите **TodoItem (TodoApi.Models)** в поле **Класс модели**.</span><span class="sxs-lookup"><span data-stu-id="08387-245">Select **TodoItem (TodoApi.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="08387-246">Выберите **TodoContext (TodoApi.Models)** в поле **Класс контекста данных**.</span><span class="sxs-lookup"><span data-stu-id="08387-246">Select **TodoContext (TodoApi.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="08387-247">Нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="08387-247">Select **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="08387-248">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="08387-248">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="08387-249">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="08387-249">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

<span data-ttu-id="08387-250">Предыдущие команды:</span><span class="sxs-lookup"><span data-stu-id="08387-250">The preceding commands:</span></span>

* <span data-ttu-id="08387-251">добавляют пакеты NuGet, необходимые для формирования шаблонов;</span><span class="sxs-lookup"><span data-stu-id="08387-251">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="08387-252">устанавливают подсистему формирования шаблонов (`dotnet-aspnet-codegenerator`);</span><span class="sxs-lookup"><span data-stu-id="08387-252">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="08387-253">формируют шаблоны для `TodoItemsController`.</span><span class="sxs-lookup"><span data-stu-id="08387-253">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="08387-254">Сформированный код:</span><span class="sxs-lookup"><span data-stu-id="08387-254">The generated code:</span></span>

* <span data-ttu-id="08387-255">Пометьте этот класс атрибутом [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="08387-255">Marks the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="08387-256">Этот атрибут указывает, что контроллер отвечает на запросы веб-API.</span><span class="sxs-lookup"><span data-stu-id="08387-256">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="08387-257">Дополнительные сведения о поведении, которое реализует этот атрибут, см. в <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="08387-257">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="08387-258">Использует внедрение зависимостей для внедрения контекста базы данных (`TodoContext`) в контроллер.</span><span class="sxs-lookup"><span data-stu-id="08387-258">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="08387-259">Контекст базы данных используется в каждом методе [создания, чтения, обновления и удаления](https://wikipedia.org/wiki/Create,_read,_update_and_delete) в контроллере.</span><span class="sxs-lookup"><span data-stu-id="08387-259">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="08387-260">Знакомство с методом создания PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="08387-260">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="08387-261">Измените инструкцию возврата в `PostTodoItem` и используйте оператор [nameof](/dotnet/csharp/language-reference/operators/nameof):</span><span class="sxs-lookup"><span data-stu-id="08387-261">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="08387-262">Предыдущий код является методом HTTP POST, обозначенным атрибутом [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="08387-262">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="08387-263">Этот метод получает значение элемента списка дел из текста HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="08387-263">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="08387-264">Метод <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*>:</span><span class="sxs-lookup"><span data-stu-id="08387-264">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="08387-265">В случае успеха возвращает код состояния HTTP 201.</span><span class="sxs-lookup"><span data-stu-id="08387-265">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="08387-266">HTTP 201 представляет собой стандартный ответ для метода HTTP POST, создающий ресурс на сервере.</span><span class="sxs-lookup"><span data-stu-id="08387-266">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="08387-267">Добавляет в ответ заголовок [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location).</span><span class="sxs-lookup"><span data-stu-id="08387-267">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="08387-268">Заголовок `Location` указывает [URI](https://developer.mozilla.org/docs/Glossary/URI) новой созданной задачи.</span><span class="sxs-lookup"><span data-stu-id="08387-268">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="08387-269">Дополнительные сведения см. в статье [10.2.2 201 "Создан ресурс"](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="08387-269">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="08387-270">Указывает действие `GetTodoItem` для создания URI заголовка `Location`.</span><span class="sxs-lookup"><span data-stu-id="08387-270">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="08387-271">Ключевое слово `nameof` C# используется для предотвращения жесткого программирования имени действия в вызове `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="08387-271">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="08387-272">Установка Postman</span><span class="sxs-lookup"><span data-stu-id="08387-272">Install Postman</span></span>

<span data-ttu-id="08387-273">В этом учебнике для тестирования веб-API используется Postman.</span><span class="sxs-lookup"><span data-stu-id="08387-273">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="08387-274">Установка [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="08387-274">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="08387-275">Запустите веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="08387-275">Start the web app.</span></span>
* <span data-ttu-id="08387-276">Запустите Postman.</span><span class="sxs-lookup"><span data-stu-id="08387-276">Start Postman.</span></span>
* <span data-ttu-id="08387-277">Отключение параметра **Проверка SSL-сертификата**</span><span class="sxs-lookup"><span data-stu-id="08387-277">Disable **SSL certificate verification**</span></span>
  * <span data-ttu-id="08387-278">В меню **Файл** > **Параметры** (вкладка **Общие**), отключите параметр **Проверка SSL-сертификата**.</span><span class="sxs-lookup"><span data-stu-id="08387-278">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="08387-279">После тестирования контроллера снова включите проверку SSL-сертификата.</span><span class="sxs-lookup"><span data-stu-id="08387-279">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="08387-280">Тестирование PostTodoItem с использованием Postman</span><span class="sxs-lookup"><span data-stu-id="08387-280">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="08387-281">Создайте новый запрос.</span><span class="sxs-lookup"><span data-stu-id="08387-281">Create a new request.</span></span>
* <span data-ttu-id="08387-282">Установите HTTP-метод `POST`.</span><span class="sxs-lookup"><span data-stu-id="08387-282">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="08387-283">Откройте вкладку **Тело**.</span><span class="sxs-lookup"><span data-stu-id="08387-283">Select the **Body** tab.</span></span>
* <span data-ttu-id="08387-284">Установите переключатель **без обработки**.</span><span class="sxs-lookup"><span data-stu-id="08387-284">Select the **raw** radio button.</span></span>
* <span data-ttu-id="08387-285">Задайте тип **JSON (приложение/json)** .</span><span class="sxs-lookup"><span data-stu-id="08387-285">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="08387-286">В теле запроса введите код JSON для элемента списка дел:</span><span class="sxs-lookup"><span data-stu-id="08387-286">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="08387-287">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="08387-287">Select **Send**.</span></span>

  ![Postman с запросом Create](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="08387-289">Тестирование URI заголовка расположения</span><span class="sxs-lookup"><span data-stu-id="08387-289">Test the location header URI</span></span>

* <span data-ttu-id="08387-290">Перейдите на вкладку **Заголовки** в области **Ответ**.</span><span class="sxs-lookup"><span data-stu-id="08387-290">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="08387-291">Скопируйте значение заголовка **Расположение**:</span><span class="sxs-lookup"><span data-stu-id="08387-291">Copy the **Location** header value:</span></span>

  ![Вкладка "Заголовки" в консоли Postman](first-web-api/_static/3/create.png)

* <span data-ttu-id="08387-293">Укажите метод GET.</span><span class="sxs-lookup"><span data-stu-id="08387-293">Set the method to GET.</span></span>
* <span data-ttu-id="08387-294">Вставьте URI (например, `https://localhost:5001/api/TodoItems/1`).</span><span class="sxs-lookup"><span data-stu-id="08387-294">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="08387-295">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="08387-295">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="08387-296">Знакомство с методами GET</span><span class="sxs-lookup"><span data-stu-id="08387-296">Examine the GET methods</span></span>

<span data-ttu-id="08387-297">Эти методы реализуют две конечные точки GET:</span><span class="sxs-lookup"><span data-stu-id="08387-297">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="08387-298">Протестируйте приложение, вызвав эти две конечные точки в браузере или в Postman.</span><span class="sxs-lookup"><span data-stu-id="08387-298">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="08387-299">Пример:</span><span class="sxs-lookup"><span data-stu-id="08387-299">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="08387-300">При вызове `GetTodoItems` возвращается примерно такой ответ:</span><span class="sxs-lookup"><span data-stu-id="08387-300">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="08387-301">Тестирование Get с использованием Postman</span><span class="sxs-lookup"><span data-stu-id="08387-301">Test Get with Postman</span></span>

* <span data-ttu-id="08387-302">Создайте новый запрос.</span><span class="sxs-lookup"><span data-stu-id="08387-302">Create a new request.</span></span>
* <span data-ttu-id="08387-303">Укажите метод HTTP **GET**.</span><span class="sxs-lookup"><span data-stu-id="08387-303">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="08387-304">Укажите URL-адрес запроса `https://localhost:<port>/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="08387-304">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="08387-305">Например, `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="08387-305">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="08387-306">Выберите режим **Представление с двумя областями** в Postman.</span><span class="sxs-lookup"><span data-stu-id="08387-306">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="08387-307">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="08387-307">Select **Send**.</span></span>

<span data-ttu-id="08387-308">Это приложение использует выполняющуюся в памяти базу данных.</span><span class="sxs-lookup"><span data-stu-id="08387-308">This app uses an in-memory database.</span></span> <span data-ttu-id="08387-309">Если остановить и вновь запустить его, предшествующий запрос GET не возвратит никаких данных.</span><span class="sxs-lookup"><span data-stu-id="08387-309">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="08387-310">Если данные не возвращаются, данные для приложения получаются методом [POST](#post).</span><span class="sxs-lookup"><span data-stu-id="08387-310">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="08387-311">Маршрутизация и пути URL</span><span class="sxs-lookup"><span data-stu-id="08387-311">Routing and URL paths</span></span>

<span data-ttu-id="08387-312">Атрибут [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) обозначает метод, который отвечает на запрос HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="08387-312">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="08387-313">Путь URL для каждого метода формируется следующим образом:</span><span class="sxs-lookup"><span data-stu-id="08387-313">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="08387-314">Возьмите строку шаблона в атрибуте `Route` контроллера:</span><span class="sxs-lookup"><span data-stu-id="08387-314">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="08387-315">Замените `[controller]` именем контроллера (по соглашению это имя класса контроллера без суффикса "Controller").</span><span class="sxs-lookup"><span data-stu-id="08387-315">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="08387-316">В этом примере класс контроллера имеет имя **TodoItems**, а сам контроллер, соответственно, — "TodoItems".</span><span class="sxs-lookup"><span data-stu-id="08387-316">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="08387-317">В ASP.NET Core [маршрутизация](xref:mvc/controllers/routing) реализуется без учета регистра символов.</span><span class="sxs-lookup"><span data-stu-id="08387-317">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="08387-318">Если атрибут `[HttpGet]` имеет шаблон маршрута (например, `[HttpGet("products")]`), добавьте его к пути.</span><span class="sxs-lookup"><span data-stu-id="08387-318">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="08387-319">В этом примере шаблон не используется.</span><span class="sxs-lookup"><span data-stu-id="08387-319">This sample doesn't use a template.</span></span> <span data-ttu-id="08387-320">Дополнительные сведения см. в разделе [Маршрутизация атрибутов с помощью атрибутов Http[Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="08387-320">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="08387-321">В следующем методе `GetTodoItem``"{id}"` — это переменная-заполнитель для уникального идентификатора элемента задачи.</span><span class="sxs-lookup"><span data-stu-id="08387-321">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="08387-322">При вызове `GetTodoItem` параметру метода `id` присваивается значение `"{id}"` в URL-адресе.</span><span class="sxs-lookup"><span data-stu-id="08387-322">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its `id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="08387-323">Возвращаемые значения</span><span class="sxs-lookup"><span data-stu-id="08387-323">Return values</span></span>

<span data-ttu-id="08387-324">Возвращаемое значение имеет тип `GetTodoItems`, а метод `GetTodoItem` имеет тип [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="08387-324">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="08387-325">ASP.NET Core автоматически сериализует объект в формат [JSON](https://www.json.org/) и записывает данные JSON в тело сообщения ответа.</span><span class="sxs-lookup"><span data-stu-id="08387-325">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="08387-326">Код ответа для этого типа возвращаемого значения равен 200, что свидетельствует об отсутствии необработанных исключений.</span><span class="sxs-lookup"><span data-stu-id="08387-326">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="08387-327">Необработанные исключения преобразуются в ошибки 5xx.</span><span class="sxs-lookup"><span data-stu-id="08387-327">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="08387-328">Типы возвращаемых значений `ActionResult` могут представлять широкий спектр кодов состояний HTTP.</span><span class="sxs-lookup"><span data-stu-id="08387-328">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="08387-329">Например, метод `GetTodoItem` может возвращать два разных значения состояния:</span><span class="sxs-lookup"><span data-stu-id="08387-329">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="08387-330">Если запрошенному идентификатору не соответствует ни один элемент, метод возвращает ошибку 404 ([Не найдено](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound)).</span><span class="sxs-lookup"><span data-stu-id="08387-330">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="08387-331">В противном случае метод возвращает код 200 с телом ответа JSON.</span><span class="sxs-lookup"><span data-stu-id="08387-331">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="08387-332">При возвращении `item` возвращается ответ HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="08387-332">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="08387-333">Метод PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="08387-333">The PutTodoItem method</span></span>

<span data-ttu-id="08387-334">Проверьте метод `PutTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="08387-334">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="08387-335">Страница `PutTodoItem` аналогична странице `PostTodoItem`, но использует запрос HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="08387-335">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="08387-336">Ответ — [204 (Нет содержимого)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="08387-336">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="08387-337">Согласно спецификации HTTP, запрос PUT требует, чтобы клиент отправлял всю обновленную сущность, а не только изменения.</span><span class="sxs-lookup"><span data-stu-id="08387-337">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="08387-338">Чтобы обеспечить поддержку частичных обновлений, используйте [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="08387-338">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="08387-339">Если возникнет ошибка вызова `PutTodoItem`, вызовите `GET`, чтобы в базе данных был один элемент.</span><span class="sxs-lookup"><span data-stu-id="08387-339">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="08387-340">Тестирование метода PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="08387-340">Test the PutTodoItem method</span></span>

<span data-ttu-id="08387-341">В этом примере используется база данных в памяти, которая должна быть инициирована при каждом запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="08387-341">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="08387-342">При выполнении вызова PUT в базе данных уже должен существовать какой-либо элемент.</span><span class="sxs-lookup"><span data-stu-id="08387-342">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="08387-343">Для этого перед вызовом PUT выполните вызов GET, чтобы убедиться в наличии такого элемента в базе данных.</span><span class="sxs-lookup"><span data-stu-id="08387-343">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="08387-344">Обновите элемент списка дел с идентификатором 1 и присвойте ему имя "feed fish":</span><span class="sxs-lookup"><span data-stu-id="08387-344">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="08387-345">На следующем рисунке показан процесс обновления Postman:</span><span class="sxs-lookup"><span data-stu-id="08387-345">The following image shows the Postman update:</span></span>

![Консоль Postman с ответом 204 (Нет содержимого)](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="08387-347">Метод DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="08387-347">The DeleteTodoItem method</span></span>

<span data-ttu-id="08387-348">Проверьте метод `DeleteTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="08387-348">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="08387-349">Тестирование метода DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="08387-349">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="08387-350">Удалите элемент списка дел с помощью Postman:</span><span class="sxs-lookup"><span data-stu-id="08387-350">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="08387-351">Укажите метод `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="08387-351">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="08387-352">Укажите URI удаляемого объекта (например, `https://localhost:5001/api/TodoItems/1`).</span><span class="sxs-lookup"><span data-stu-id="08387-352">Set the URI of the object to delete (for example `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="08387-353">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="08387-353">Select **Send**.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="08387-354">Вызов веб-API с помощью JavaScript</span><span class="sxs-lookup"><span data-stu-id="08387-354">Call the web API with JavaScript</span></span>

<span data-ttu-id="08387-355">См. руководство по [: Вызовите веб-API ASP.NET Core с помощью JavaScript](xref:tutorials/web-api-javascript).</span><span class="sxs-lookup"><span data-stu-id="08387-355">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="08387-356">В этом руководстве вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="08387-356">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="08387-357">Создание проекта веб-API.</span><span class="sxs-lookup"><span data-stu-id="08387-357">Create a web API project.</span></span>
> * <span data-ttu-id="08387-358">Добавление класса модели и контекста базы данных.</span><span class="sxs-lookup"><span data-stu-id="08387-358">Add a model class and a database context.</span></span>
> * <span data-ttu-id="08387-359">Добавление контроллера.</span><span class="sxs-lookup"><span data-stu-id="08387-359">Add a controller.</span></span>
> * <span data-ttu-id="08387-360">Добавление методов CRUD.</span><span class="sxs-lookup"><span data-stu-id="08387-360">Add CRUD methods.</span></span>
> * <span data-ttu-id="08387-361">Настройка маршрутизации и путей URL.</span><span class="sxs-lookup"><span data-stu-id="08387-361">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="08387-362">Указание возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="08387-362">Specify return values.</span></span>
> * <span data-ttu-id="08387-363">Вызов веб-API с помощью Postman.</span><span class="sxs-lookup"><span data-stu-id="08387-363">Call the web API with Postman.</span></span>
> * <span data-ttu-id="08387-364">Вызовите веб-API с помощью JavaScript.</span><span class="sxs-lookup"><span data-stu-id="08387-364">Call the web API with JavaScript.</span></span>

<span data-ttu-id="08387-365">В конечном итоге вы получите веб-API, обеспечивающий управление элементами списка дел, хранящимися в реляционной базе данных.</span><span class="sxs-lookup"><span data-stu-id="08387-365">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="08387-366">Обзор</span><span class="sxs-lookup"><span data-stu-id="08387-366">Overview</span></span>

<span data-ttu-id="08387-367">В этом руководстве создается следующий API-интерфейс:</span><span class="sxs-lookup"><span data-stu-id="08387-367">This tutorial creates the following API:</span></span>

|<span data-ttu-id="08387-368">API</span><span class="sxs-lookup"><span data-stu-id="08387-368">API</span></span> | <span data-ttu-id="08387-369">Описание</span><span class="sxs-lookup"><span data-stu-id="08387-369">Description</span></span> | <span data-ttu-id="08387-370">Текст запроса</span><span class="sxs-lookup"><span data-stu-id="08387-370">Request body</span></span> | <span data-ttu-id="08387-371">Текст ответа</span><span class="sxs-lookup"><span data-stu-id="08387-371">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="08387-372">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="08387-372">GET /api/TodoItems</span></span> | <span data-ttu-id="08387-373">Получение всех элементов задач</span><span class="sxs-lookup"><span data-stu-id="08387-373">Get all to-do items</span></span> | <span data-ttu-id="08387-374">Отсутствуют</span><span class="sxs-lookup"><span data-stu-id="08387-374">None</span></span> | <span data-ttu-id="08387-375">Массив элементов задач</span><span class="sxs-lookup"><span data-stu-id="08387-375">Array of to-do items</span></span>|
|<span data-ttu-id="08387-376">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="08387-376">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="08387-377">Получение объекта по идентификатору</span><span class="sxs-lookup"><span data-stu-id="08387-377">Get an item by ID</span></span> | <span data-ttu-id="08387-378">Отсутствуют</span><span class="sxs-lookup"><span data-stu-id="08387-378">None</span></span> | <span data-ttu-id="08387-379">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="08387-379">To-do item</span></span>|
|<span data-ttu-id="08387-380">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="08387-380">POST /api/TodoItems</span></span> | <span data-ttu-id="08387-381">Добавление нового элемента</span><span class="sxs-lookup"><span data-stu-id="08387-381">Add a new item</span></span> | <span data-ttu-id="08387-382">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="08387-382">To-do item</span></span> | <span data-ttu-id="08387-383">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="08387-383">To-do item</span></span> |
|<span data-ttu-id="08387-384">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="08387-384">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="08387-385">Обновление существующего элемента &nbsp;</span><span class="sxs-lookup"><span data-stu-id="08387-385">Update an existing item &nbsp;</span></span> | <span data-ttu-id="08387-386">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="08387-386">To-do item</span></span> | <span data-ttu-id="08387-387">Отсутствуют</span><span class="sxs-lookup"><span data-stu-id="08387-387">None</span></span> |
|<span data-ttu-id="08387-388">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="08387-388">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="08387-389">Удаление элемента &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="08387-389">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="08387-390">Отсутствуют</span><span class="sxs-lookup"><span data-stu-id="08387-390">None</span></span> | <span data-ttu-id="08387-391">Отсутствуют</span><span class="sxs-lookup"><span data-stu-id="08387-391">None</span></span>|

<span data-ttu-id="08387-392">На следующем рисунке показана структура приложения.</span><span class="sxs-lookup"><span data-stu-id="08387-392">The following diagram shows the design of the app.</span></span>

![Клиент представлен прямоугольником слева.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="08387-398">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="08387-398">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="08387-399">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="08387-399">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="08387-400">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="08387-400">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="08387-401">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="08387-401">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="08387-402">Создайте веб-проект.</span><span class="sxs-lookup"><span data-stu-id="08387-402">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="08387-403">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="08387-403">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="08387-404">В меню **Файл** выберите пункт **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="08387-404">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="08387-405">Выберите шаблон **Веб-приложение ASP.NET Core** и нажмите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="08387-405">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="08387-406">Назовите проект *TodoApi* и нажмите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="08387-406">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="08387-407">В диалоговом окне **Создание веб-приложения ASP.NET Core** убедитесь в том, что выбраны платформы **.NET Core** и **ASP.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="08387-407">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="08387-408">Выберите шаблон **API** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="08387-408">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="08387-409">**Не** выбирайте **Включение поддержки Docker**.</span><span class="sxs-lookup"><span data-stu-id="08387-409">**Don't** select **Enable Docker Support**.</span></span>

![Диалоговое окно создания проекта VS](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="08387-411">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="08387-411">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="08387-412">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="08387-412">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="08387-413">Смените каталог (`cd`) на папку, в которой будет содержаться папка проекта.</span><span class="sxs-lookup"><span data-stu-id="08387-413">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="08387-414">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="08387-414">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="08387-415">С помощью этих команд создается новый проект веб-API и открывается новый экземпляр Visual Studio Code в новой папке проекта.</span><span class="sxs-lookup"><span data-stu-id="08387-415">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="08387-416">При появлении диалогового окна с запросом на добавление в проект необходимых ресурсов выберите **Да**.</span><span class="sxs-lookup"><span data-stu-id="08387-416">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="08387-417">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="08387-417">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="08387-418">Щелкните **Файл** > **Новое решение**.</span><span class="sxs-lookup"><span data-stu-id="08387-418">Select **File** > **New Solution**.</span></span>

  ![Новое решение macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="08387-420">Щелкните **.NET Core** > **Приложение** > **API** > **Далее**.</span><span class="sxs-lookup"><span data-stu-id="08387-420">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![Диалоговое окно "Новый проект" в macOS](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="08387-422">В диалоговом окне **Настройка нового веб-API ASP.NET Core** оставьте установленное по умолчанию значение для параметра **Целевая платформа**, то есть \* *.NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="08387-422">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="08387-423">Введите *TodoApi* в поле **Имя проекта** и выберите команду **Создать**.</span><span class="sxs-lookup"><span data-stu-id="08387-423">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Диалоговое окно конфигурации](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="08387-425">Тестирование API</span><span class="sxs-lookup"><span data-stu-id="08387-425">Test the API</span></span>

<span data-ttu-id="08387-426">Шаблон проекта создает API `values`.</span><span class="sxs-lookup"><span data-stu-id="08387-426">The project template creates a `values` API.</span></span> <span data-ttu-id="08387-427">Вызовите метод `Get` из браузера для тестирования приложения.</span><span class="sxs-lookup"><span data-stu-id="08387-427">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="08387-428">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="08387-428">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="08387-429">Нажмите клавиши CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="08387-429">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="08387-430">Visual Studio запустит браузер и перейдет к `https://localhost:<port>/api/values`, где `<port>` — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="08387-430">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="08387-431">Если появится диалоговое окно с запросом о необходимости доверять сертификату IIS Express, выберите **Да**.</span><span class="sxs-lookup"><span data-stu-id="08387-431">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="08387-432">В появляющемся следом диалоговом окне **Предупреждение системы безопасности** выберите **Да**.</span><span class="sxs-lookup"><span data-stu-id="08387-432">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="08387-433">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="08387-433">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="08387-434">Нажмите клавиши CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="08387-434">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="08387-435">В браузере перейдите по следующему URL-адресу: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="08387-435">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="08387-436">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="08387-436">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="08387-437">Выберите **Выполнить** > **Начать отладку**, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="08387-437">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="08387-438">Visual Studio для Mac запустит браузер и перейдет к `https://localhost:<port>`, где `<port>` — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="08387-438">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="08387-439">Появится ошибка HTTP 404 (Не найдено).</span><span class="sxs-lookup"><span data-stu-id="08387-439">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="08387-440">Добавьте `/api/values` к URL-адресу (измените URL-адрес на `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="08387-440">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="08387-441">Возвращается следующий файл JSON:</span><span class="sxs-lookup"><span data-stu-id="08387-441">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="08387-442">Добавление класса модели</span><span class="sxs-lookup"><span data-stu-id="08387-442">Add a model class</span></span>

<span data-ttu-id="08387-443">*Модель* — это набор классов, представляющих данные, которыми управляет приложение.</span><span class="sxs-lookup"><span data-stu-id="08387-443">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="08387-444">Модель этого приложения содержит единственный класс `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="08387-444">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="08387-445">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="08387-445">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="08387-446">В **обозревателе решений** щелкните проект правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="08387-446">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="08387-447">Выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="08387-447">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="08387-448">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="08387-448">Name the folder *Models*.</span></span>

* <span data-ttu-id="08387-449">Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="08387-449">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="08387-450">Присвойте классу имя *TodoItem* и выберите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="08387-450">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="08387-451">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="08387-451">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="08387-452">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="08387-452">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="08387-453">Добавьте папку с именем *Models*.</span><span class="sxs-lookup"><span data-stu-id="08387-453">Add a folder named *Models*.</span></span>

* <span data-ttu-id="08387-454">Добавьте класс `TodoItem` в папку *Models*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="08387-454">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="08387-455">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="08387-455">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="08387-456">Щелкните проект правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="08387-456">Right-click the project.</span></span> <span data-ttu-id="08387-457">Выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="08387-457">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="08387-458">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="08387-458">Name the folder *Models*.</span></span>

  ![Новая папка](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="08387-460">Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Создать файл** > **Общие** > **Пустой класс**.</span><span class="sxs-lookup"><span data-stu-id="08387-460">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="08387-461">Назовите класс *TodoItem* и нажмите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="08387-461">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="08387-462">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="08387-462">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="08387-463">Свойство `Id` выступает в качестве уникального ключа реляционной базы данных.</span><span class="sxs-lookup"><span data-stu-id="08387-463">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="08387-464">Классы моделей можно размещать в любом месте проекта, но обычно для этого используется папка *Models*.</span><span class="sxs-lookup"><span data-stu-id="08387-464">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="08387-465">Добавление контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="08387-465">Add a database context</span></span>

<span data-ttu-id="08387-466">*Контекст базы данных* —это основной класс, который координирует функциональные возможности Entity Framework для модели данных.</span><span class="sxs-lookup"><span data-stu-id="08387-466">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="08387-467">Этот класс является производным от класса `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="08387-467">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="08387-468">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="08387-468">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="08387-469">Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="08387-469">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="08387-470">Назовите класс *TodoContext* и нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="08387-470">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="08387-471">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="08387-471">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="08387-472">Добавьте класс `TodoContext` в папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="08387-472">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="08387-473">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="08387-473">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="08387-474">Регистрация контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="08387-474">Register the database context</span></span>

<span data-ttu-id="08387-475">В ASP.NET Core службы (такие как контекст базы данных) должны быть зарегистрированы с помощью контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="08387-475">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="08387-476">Контейнер предоставляет службу контроллерам.</span><span class="sxs-lookup"><span data-stu-id="08387-476">The container provides the service to controllers.</span></span>

<span data-ttu-id="08387-477">Обновите файл *Startup.cs*, используя следующий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="08387-477">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="08387-478">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="08387-478">The preceding code:</span></span>

* <span data-ttu-id="08387-479">Удалите неиспользуемые объявления `using`.</span><span class="sxs-lookup"><span data-stu-id="08387-479">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="08387-480">Добавляет контекст базы данных в контейнер внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="08387-480">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="08387-481">Указывает, что контекст базы данных будет использовать базу данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="08387-481">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="08387-482">Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="08387-482">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="08387-483">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="08387-483">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="08387-484">Щелкните папку *Controllers* правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="08387-484">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="08387-485">Щелкните **Добавить** > **Создать элемент**.</span><span class="sxs-lookup"><span data-stu-id="08387-485">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="08387-486">В диалоговом окне **Добавить новый элемент** выберите шаблон **Класс контроллера API**.</span><span class="sxs-lookup"><span data-stu-id="08387-486">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="08387-487">Присвойте классу имя *TodoController* и выберите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="08387-487">Name the class *TodoController*, and select **Add**.</span></span>

  ![Диалоговое окно добавления элемента с контроллером в поле поиска и выбранным контроллером веб-API](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="08387-489">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="08387-489">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="08387-490">В папке *Controllers* создайте класс с именем `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="08387-490">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="08387-491">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="08387-491">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="08387-492">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="08387-492">The preceding code:</span></span>

* <span data-ttu-id="08387-493">Определяет класс контроллера API без методов.</span><span class="sxs-lookup"><span data-stu-id="08387-493">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="08387-494">Пометьте этот класс атрибутом [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="08387-494">Marks the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="08387-495">Этот атрибут указывает, что контроллер отвечает на запросы веб-API.</span><span class="sxs-lookup"><span data-stu-id="08387-495">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="08387-496">Дополнительные сведения о поведении, которое реализует этот атрибут, см. в <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="08387-496">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="08387-497">Использует внедрение зависимостей для внедрения контекста базы данных (`TodoContext`) в контроллер.</span><span class="sxs-lookup"><span data-stu-id="08387-497">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="08387-498">Контекст базы данных используется в каждом методе [создания, чтения, обновления и удаления](https://wikipedia.org/wiki/Create,_read,_update_and_delete) в контроллере.</span><span class="sxs-lookup"><span data-stu-id="08387-498">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="08387-499">Добавляет элемент `Item1` в базу данных, если она пуста.</span><span class="sxs-lookup"><span data-stu-id="08387-499">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="08387-500">Этот код находится в конструкторе и выполняется каждый раз при обнаружении нового HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="08387-500">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="08387-501">Если вы удалите все элементы, конструктор создаст `Item1` при следующем вызове метода API.</span><span class="sxs-lookup"><span data-stu-id="08387-501">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="08387-502">Поэтому может создаться впечатление, что удаление не было выполнено, хотя это не так.</span><span class="sxs-lookup"><span data-stu-id="08387-502">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="08387-503">Добавление методов Get</span><span class="sxs-lookup"><span data-stu-id="08387-503">Add Get methods</span></span>

<span data-ttu-id="08387-504">Чтобы получить API, который извлекает элементы списка дел, добавьте следующие методы в класс `TodoController`:</span><span class="sxs-lookup"><span data-stu-id="08387-504">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="08387-505">Эти методы реализуют две конечные точки GET:</span><span class="sxs-lookup"><span data-stu-id="08387-505">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="08387-506">Если приложение все еще выполняется, оно останавливается.</span><span class="sxs-lookup"><span data-stu-id="08387-506">Stop the app if it's still running.</span></span> <span data-ttu-id="08387-507">Затем оно запускается снова с последними изменениями.</span><span class="sxs-lookup"><span data-stu-id="08387-507">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="08387-508">Протестируйте приложение, вызвав эти две конечные точки в браузере.</span><span class="sxs-lookup"><span data-stu-id="08387-508">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="08387-509">Пример:</span><span class="sxs-lookup"><span data-stu-id="08387-509">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="08387-510">При вызове `GetTodoItems` возвращается следующий ответ HTTP:</span><span class="sxs-lookup"><span data-stu-id="08387-510">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="08387-511">Маршрутизация и пути URL</span><span class="sxs-lookup"><span data-stu-id="08387-511">Routing and URL paths</span></span>

<span data-ttu-id="08387-512">Атрибут [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) обозначает метод, который отвечает на запрос HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="08387-512">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="08387-513">Путь URL для каждого метода формируется следующим образом:</span><span class="sxs-lookup"><span data-stu-id="08387-513">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="08387-514">Возьмите строку шаблона в атрибуте `Route` контроллера:</span><span class="sxs-lookup"><span data-stu-id="08387-514">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="08387-515">Замените `[controller]` именем контроллера (по соглашению это имя класса контроллера без суффикса "Controller").</span><span class="sxs-lookup"><span data-stu-id="08387-515">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="08387-516">В этом примере класс контроллера носит имя **Todo**, а сам контроллер, соответственно, — "todo".</span><span class="sxs-lookup"><span data-stu-id="08387-516">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="08387-517">В ASP.NET Core [маршрутизация](xref:mvc/controllers/routing) реализуется без учета регистра символов.</span><span class="sxs-lookup"><span data-stu-id="08387-517">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="08387-518">Если атрибут `[HttpGet]` имеет шаблон маршрута (например, `[HttpGet("products")]`), добавьте его к пути.</span><span class="sxs-lookup"><span data-stu-id="08387-518">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="08387-519">В этом примере шаблон не используется.</span><span class="sxs-lookup"><span data-stu-id="08387-519">This sample doesn't use a template.</span></span> <span data-ttu-id="08387-520">Дополнительные сведения см. в разделе [Маршрутизация атрибутов с помощью атрибутов Http[Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="08387-520">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="08387-521">В следующем методе `GetTodoItem``"{id}"` — это переменная-заполнитель для уникального идентификатора элемента задачи.</span><span class="sxs-lookup"><span data-stu-id="08387-521">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="08387-522">При вызове `GetTodoItem` параметру метода `id` присваивается значение `"{id}"` в URL-адресе.</span><span class="sxs-lookup"><span data-stu-id="08387-522">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="08387-523">Возвращаемые значения</span><span class="sxs-lookup"><span data-stu-id="08387-523">Return values</span></span>

<span data-ttu-id="08387-524">Возвращаемое значение имеет тип `GetTodoItems`, а метод `GetTodoItem` имеет тип [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="08387-524">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="08387-525">ASP.NET Core автоматически сериализует объект в формат [JSON](https://www.json.org/) и записывает данные JSON в тело сообщения ответа.</span><span class="sxs-lookup"><span data-stu-id="08387-525">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="08387-526">Код ответа для этого типа возвращаемого значения равен 200, что свидетельствует об отсутствии необработанных исключений.</span><span class="sxs-lookup"><span data-stu-id="08387-526">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="08387-527">Необработанные исключения преобразуются в ошибки 5xx.</span><span class="sxs-lookup"><span data-stu-id="08387-527">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="08387-528">Типы возвращаемых значений `ActionResult` могут представлять широкий спектр кодов состояний HTTP.</span><span class="sxs-lookup"><span data-stu-id="08387-528">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="08387-529">Например, метод `GetTodoItem` может возвращать два разных значения состояния:</span><span class="sxs-lookup"><span data-stu-id="08387-529">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="08387-530">Если запрошенному идентификатору не соответствует ни один элемент, метод возвращает ошибку 404 ([Не найдено](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound)).</span><span class="sxs-lookup"><span data-stu-id="08387-530">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="08387-531">В противном случае метод возвращает код 200 с телом ответа JSON.</span><span class="sxs-lookup"><span data-stu-id="08387-531">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="08387-532">При возвращении `item` возвращается ответ HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="08387-532">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="08387-533">Тестирование метода GetTodoItems</span><span class="sxs-lookup"><span data-stu-id="08387-533">Test the GetTodoItems method</span></span>

<span data-ttu-id="08387-534">В этом учебнике для тестирования веб-API используется Postman.</span><span class="sxs-lookup"><span data-stu-id="08387-534">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="08387-535">Установите [Postman](https://www.getpostman.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="08387-535">Install [Postman](https://www.getpostman.com/downloads/).</span></span>
* <span data-ttu-id="08387-536">Запустите веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="08387-536">Start the web app.</span></span>
* <span data-ttu-id="08387-537">Запустите Postman.</span><span class="sxs-lookup"><span data-stu-id="08387-537">Start Postman.</span></span>
* <span data-ttu-id="08387-538">Отключите параметр **Проверка SSL-сертификата**.</span><span class="sxs-lookup"><span data-stu-id="08387-538">Disable **SSL certificate verification**.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="08387-539">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="08387-539">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="08387-540">В меню **Файл** > **Параметры** (вкладка **Общие**), отключите параметр **Проверка SSL-сертификата**.</span><span class="sxs-lookup"><span data-stu-id="08387-540">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="08387-541">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="08387-541">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="08387-542">В меню **Postman** > **Настройки** (вкладка **Общие**) отключите параметр **Проверка SSL-сертификата**.</span><span class="sxs-lookup"><span data-stu-id="08387-542">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="08387-543">Кроме того, можно выбрать значок гаечного ключа и выбрать **Параметры**, а затем отключить проверку SSL-сертификата.</span><span class="sxs-lookup"><span data-stu-id="08387-543">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="08387-544">После тестирования контроллера снова включите проверку SSL-сертификата.</span><span class="sxs-lookup"><span data-stu-id="08387-544">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="08387-545">Создайте новый запрос.</span><span class="sxs-lookup"><span data-stu-id="08387-545">Create a new request.</span></span>
  * <span data-ttu-id="08387-546">Укажите метод HTTP **GET**.</span><span class="sxs-lookup"><span data-stu-id="08387-546">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="08387-547">Укажите URL-адрес запроса `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="08387-547">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="08387-548">Например, `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="08387-548">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="08387-549">Выберите режим **Представление с двумя областями** в Postman.</span><span class="sxs-lookup"><span data-stu-id="08387-549">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="08387-550">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="08387-550">Select **Send**.</span></span>

![Postman с запросом Get](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="08387-552">Добавление метода Create</span><span class="sxs-lookup"><span data-stu-id="08387-552">Add a Create method</span></span>

<span data-ttu-id="08387-553">Добавьте следующий метод `PostTodoItem` в *Controllers/TodoController.cs*:</span><span class="sxs-lookup"><span data-stu-id="08387-553">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="08387-554">Предыдущий код является методом HTTP POST, обозначенным атрибутом [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="08387-554">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="08387-555">Этот метод получает значение элемента списка дел из текста HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="08387-555">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="08387-556">Метод `CreatedAtAction`:</span><span class="sxs-lookup"><span data-stu-id="08387-556">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="08387-557">В случае успеха возвращает код состояния HTTP 201.</span><span class="sxs-lookup"><span data-stu-id="08387-557">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="08387-558">HTTP 201 представляет собой стандартный ответ для метода HTTP POST, создающий ресурс на сервере.</span><span class="sxs-lookup"><span data-stu-id="08387-558">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="08387-559">Добавляет заголовок `Location` в ответ.</span><span class="sxs-lookup"><span data-stu-id="08387-559">Adds a `Location` header to the response.</span></span> <span data-ttu-id="08387-560">Заголовок `Location` расположения указывает URI вновь созданной задачи.</span><span class="sxs-lookup"><span data-stu-id="08387-560">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="08387-561">Дополнительные сведения см. в статье [10.2.2 201 "Создан ресурс"](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="08387-561">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="08387-562">Указывает действие `GetTodoItem` для создания URI заголовка `Location`.</span><span class="sxs-lookup"><span data-stu-id="08387-562">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="08387-563">Ключевое слово `nameof` C# используется для предотвращения жесткого программирования имени действия в вызове `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="08387-563">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="08387-564">Тестирование метода PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="08387-564">Test the PostTodoItem method</span></span>

* <span data-ttu-id="08387-565">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="08387-565">Build the project.</span></span>
* <span data-ttu-id="08387-566">В Postman укажите метод HTTP `POST`.</span><span class="sxs-lookup"><span data-stu-id="08387-566">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="08387-567">Откройте вкладку **Тело**.</span><span class="sxs-lookup"><span data-stu-id="08387-567">Select the **Body** tab.</span></span>
* <span data-ttu-id="08387-568">Установите переключатель **без обработки**.</span><span class="sxs-lookup"><span data-stu-id="08387-568">Select the **raw** radio button.</span></span>
* <span data-ttu-id="08387-569">Задайте тип **JSON (приложение/json)** .</span><span class="sxs-lookup"><span data-stu-id="08387-569">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="08387-570">В теле запроса введите код JSON для элемента списка дел:</span><span class="sxs-lookup"><span data-stu-id="08387-570">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="08387-571">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="08387-571">Select **Send**.</span></span>

  ![Postman с запросом Create](first-web-api/_static/create.png)

  <span data-ttu-id="08387-573">Если вы получаете ошибку 405 "Недопустимый метод", это может свидетельствовать о том, что после добавления метода `PostTodoItem` не была выполнена компиляция проекта.</span><span class="sxs-lookup"><span data-stu-id="08387-573">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="08387-574">Тестирование URI заголовка расположения</span><span class="sxs-lookup"><span data-stu-id="08387-574">Test the location header URI</span></span>

* <span data-ttu-id="08387-575">Перейдите на вкладку **Заголовки** в области **Ответ**.</span><span class="sxs-lookup"><span data-stu-id="08387-575">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="08387-576">Скопируйте значение заголовка **Расположение**:</span><span class="sxs-lookup"><span data-stu-id="08387-576">Copy the **Location** header value:</span></span>

  ![Вкладка "Заголовки" в консоли Postman](first-web-api/_static/pmc2.png)

* <span data-ttu-id="08387-578">Укажите метод GET.</span><span class="sxs-lookup"><span data-stu-id="08387-578">Set the method to GET.</span></span>
* <span data-ttu-id="08387-579">Вставьте URI (например, `https://localhost:5001/api/Todo/2`).</span><span class="sxs-lookup"><span data-stu-id="08387-579">Paste the URI (for example, `https://localhost:5001/api/Todo/2`).</span></span>
* <span data-ttu-id="08387-580">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="08387-580">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="08387-581">Добавление метода PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="08387-581">Add a PutTodoItem method</span></span>

<span data-ttu-id="08387-582">Добавьте приведенный ниже метод `PutTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="08387-582">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="08387-583">Страница `PutTodoItem` аналогична странице `PostTodoItem`, но использует запрос HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="08387-583">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="08387-584">Ответ — [204 (Нет содержимого)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="08387-584">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="08387-585">Согласно спецификации HTTP, запрос PUT требует, чтобы клиент отправлял всю обновленную сущность, а не только изменения.</span><span class="sxs-lookup"><span data-stu-id="08387-585">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="08387-586">Чтобы обеспечить поддержку частичных обновлений, используйте [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="08387-586">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="08387-587">Если возникнет ошибка вызова `PutTodoItem`, вызовите `GET`, чтобы в базе данных был один элемент.</span><span class="sxs-lookup"><span data-stu-id="08387-587">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="08387-588">Тестирование метода PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="08387-588">Test the PutTodoItem method</span></span>

<span data-ttu-id="08387-589">В этом примере используется база данных в памяти, которая должна быть инициирована при каждом запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="08387-589">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="08387-590">При выполнении вызова PUT в базе данных уже должен существовать какой-либо элемент.</span><span class="sxs-lookup"><span data-stu-id="08387-590">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="08387-591">Для этого перед вызовом PUT выполните вызов GET, чтобы убедиться в наличии такого элемента в базе данных.</span><span class="sxs-lookup"><span data-stu-id="08387-591">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="08387-592">Обновите элемент списка дел с идентификатором = 1 и присвойте ему имя "feed fish":</span><span class="sxs-lookup"><span data-stu-id="08387-592">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="08387-593">На следующем рисунке показан процесс обновления Postman:</span><span class="sxs-lookup"><span data-stu-id="08387-593">The following image shows the Postman update:</span></span>

![Консоль Postman с ответом 204 (Нет содержимого)](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="08387-595">Добавление метода DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="08387-595">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="08387-596">Добавьте приведенный ниже метод `DeleteTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="08387-596">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="08387-597">`DeleteTodoItem`Ответ[ — 204 (нет содержимого)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="08387-597">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="08387-598">Тестирование метода DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="08387-598">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="08387-599">Удалите элемент списка дел с помощью Postman:</span><span class="sxs-lookup"><span data-stu-id="08387-599">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="08387-600">Укажите метод `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="08387-600">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="08387-601">Укажите URI удаляемого объекта (например, `https://localhost:5001/api/todo/1`).</span><span class="sxs-lookup"><span data-stu-id="08387-601">Set the URI of the object to delete (for example `https://localhost:5001/api/todo/1`).</span></span>
* <span data-ttu-id="08387-602">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="08387-602">Select **Send**.</span></span>

<span data-ttu-id="08387-603">В этом примере приложения вы можете удалить все элементы.</span><span class="sxs-lookup"><span data-stu-id="08387-603">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="08387-604">Однако в случае удаления последнего элемента в момент следующего вызова API конструктор класса модели создаст новый элемент.</span><span class="sxs-lookup"><span data-stu-id="08387-604">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="08387-605">Вызов веб-API с помощью JavaScript</span><span class="sxs-lookup"><span data-stu-id="08387-605">Call the web API with JavaScript</span></span>

<span data-ttu-id="08387-606">В этом разделе описано, как добавить HTML-страницу, которая использует JavaScript для вызова веб-API.</span><span class="sxs-lookup"><span data-stu-id="08387-606">In this section, an HTML page is added that uses JavaScript to call the web API.</span></span> <span data-ttu-id="08387-607">jQuery инициирует запрос.</span><span class="sxs-lookup"><span data-stu-id="08387-607">jQuery initiates the request.</span></span> <span data-ttu-id="08387-608">JavaScript изменяет страницу, используя сведения из ответа API.</span><span class="sxs-lookup"><span data-stu-id="08387-608">JavaScript updates the page with the details from the web API's response.</span></span>

<span data-ttu-id="08387-609">Настройте приложение для [обслуживания статических файлов](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) и [включения сопоставления файлов по умолчанию](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_), обновив *Startup.cs* следующим выделенным кодом:</span><span class="sxs-lookup"><span data-stu-id="08387-609">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="08387-610">Создайте папку *wwwroot* в каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="08387-610">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="08387-611">Добавьте HTML-файл *index.html* в каталог *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="08387-611">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="08387-612">Замените его содержимое следующей разметкой:</span><span class="sxs-lookup"><span data-stu-id="08387-612">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="08387-613">Добавьте файл JavaScript с именем *site.js* в каталог *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="08387-613">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="08387-614">Замените его содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="08387-614">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="08387-615">Может потребоваться изменение параметров запуска проекта ASP.NET Core для локального тестирования HTML-страницы:</span><span class="sxs-lookup"><span data-stu-id="08387-615">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="08387-616">Откройте файл *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="08387-616">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="08387-617">Удалите свойство `launchUrl`, чтобы приложение открылось через *index.html* &mdash; файл проекта по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="08387-617">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="08387-618">В этом примере вызываются все методы CRUD в веб-API.</span><span class="sxs-lookup"><span data-stu-id="08387-618">This sample calls all of the CRUD methods of the web API.</span></span> <span data-ttu-id="08387-619">Ниже приводится пояснение вызовов API.</span><span class="sxs-lookup"><span data-stu-id="08387-619">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="08387-620">Получение списка элементов задач</span><span class="sxs-lookup"><span data-stu-id="08387-620">Get a list of to-do items</span></span>

<span data-ttu-id="08387-621">jQuery отправляет запрос HTTP GET к веб-API, который возвращает ответ JSON, представляющий массив элементов списка дел.</span><span class="sxs-lookup"><span data-stu-id="08387-621">jQuery sends an HTTP GET request to the web API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="08387-622">В случае успешного запроса используется функция обратного вызова `success`.</span><span class="sxs-lookup"><span data-stu-id="08387-622">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="08387-623">При обратном вызове в модель DOM вносятся данные о задачах.</span><span class="sxs-lookup"><span data-stu-id="08387-623">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="08387-624">Добавление элемента задачи</span><span class="sxs-lookup"><span data-stu-id="08387-624">Add a to-do item</span></span>

<span data-ttu-id="08387-625">jQuery отправляет запрос HTTP POST с элементом списка дел в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="08387-625">jQuery sends an HTTP POST request with the to-do item in the request body.</span></span> <span data-ttu-id="08387-626">Для параметров `accepts` и `contentType` указывается `application/json`, чтобы указать тип носителя при получении и отправке.</span><span class="sxs-lookup"><span data-stu-id="08387-626">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="08387-627">Элемент списка дел преобразуется в JSON с помощью [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="08387-627">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="08387-628">Если интерфейс API возвращает код состояния успешного выполнения, вызывается функция `getData` для обновления HTML-таблицы.</span><span class="sxs-lookup"><span data-stu-id="08387-628">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="08387-629">Обновление элемента задачи</span><span class="sxs-lookup"><span data-stu-id="08387-629">Update a to-do item</span></span>

<span data-ttu-id="08387-630">Обновление элемента списка дел выполняется аналогично его добавлению.</span><span class="sxs-lookup"><span data-stu-id="08387-630">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="08387-631">`url` изменяется, чтобы добавить уникальный идентификатор для элемента, а в качестве `type` устанавливается `PUT`.</span><span class="sxs-lookup"><span data-stu-id="08387-631">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="08387-632">Удаление элемента задачи</span><span class="sxs-lookup"><span data-stu-id="08387-632">Delete a to-do item</span></span>

<span data-ttu-id="08387-633">Чтобы удалить элемент списка дел, установите для `type` в вызове AJAX значение `DELETE` и укажите уникальный идентификатор элемента в URL-адресе.</span><span class="sxs-lookup"><span data-stu-id="08387-633">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a><span data-ttu-id="08387-634">Добавление поддержки аутентификации в веб-API</span><span class="sxs-lookup"><span data-stu-id="08387-634">Add authentication support to a web API</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

## <a name="additional-resources"></a><span data-ttu-id="08387-635">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="08387-635">Additional resources</span></span>

<span data-ttu-id="08387-636">[Просмотреть или скачать пример кода для этого учебника](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="08387-636">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="08387-637">См. раздел [Практическое руководство. Скачивание файла](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="08387-637">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="08387-638">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="08387-638">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="08387-639">Версия руководства на YouTube</span><span class="sxs-lookup"><span data-stu-id="08387-639">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
