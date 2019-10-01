---
title: Учебник. Создание веб-API с помощью ASP.NET Core
author: rick-anderson
description: Узнайте, как создать веб-API с помощью ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 08/27/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 5e5215f246c6c7a805a4c99f485d51a2fb3c712d
ms.sourcegitcommit: cf9ffcce4fe0b69fe795aae9ae06e99fdb18bdfc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/26/2019
ms.locfileid: "71306665"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="3d686-103">Учебник. Создание веб-API с помощью ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3d686-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="3d686-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Майк Уоссон](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="3d686-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="3d686-105">В этом учебнике приводятся основные сведения о создании веб-API с помощью ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3d686-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="3d686-106">В этом руководстве вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="3d686-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3d686-107">Создание проекта веб-API.</span><span class="sxs-lookup"><span data-stu-id="3d686-107">Create a web API project.</span></span>
> * <span data-ttu-id="3d686-108">Добавление класса модели и контекста базы данных.</span><span class="sxs-lookup"><span data-stu-id="3d686-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="3d686-109">Формирование шаблонов контроллера с использованием методов CRUD.</span><span class="sxs-lookup"><span data-stu-id="3d686-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="3d686-110">Настройка маршрутизации, URL-пути и возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="3d686-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="3d686-111">Вызов веб-API с помощью Postman.</span><span class="sxs-lookup"><span data-stu-id="3d686-111">Call the web API with Postman.</span></span>

<span data-ttu-id="3d686-112">В итоге вы получите веб-API, позволяющий работать с элементами списка дел, хранимыми в базе данных.</span><span class="sxs-lookup"><span data-stu-id="3d686-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="3d686-113">Обзор</span><span class="sxs-lookup"><span data-stu-id="3d686-113">Overview</span></span>

<span data-ttu-id="3d686-114">В этом руководстве создается следующий API-интерфейс:</span><span class="sxs-lookup"><span data-stu-id="3d686-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="3d686-115">API</span><span class="sxs-lookup"><span data-stu-id="3d686-115">API</span></span> | <span data-ttu-id="3d686-116">ОПИСАНИЕ</span><span class="sxs-lookup"><span data-stu-id="3d686-116">Description</span></span> | <span data-ttu-id="3d686-117">Текст запроса</span><span class="sxs-lookup"><span data-stu-id="3d686-117">Request body</span></span> | <span data-ttu-id="3d686-118">Текст ответа</span><span class="sxs-lookup"><span data-stu-id="3d686-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="3d686-119">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="3d686-119">GET /api/TodoItems</span></span> | <span data-ttu-id="3d686-120">Получение всех элементов задач</span><span class="sxs-lookup"><span data-stu-id="3d686-120">Get all to-do items</span></span> | <span data-ttu-id="3d686-121">Нет</span><span class="sxs-lookup"><span data-stu-id="3d686-121">None</span></span> | <span data-ttu-id="3d686-122">Массив элементов задач</span><span class="sxs-lookup"><span data-stu-id="3d686-122">Array of to-do items</span></span>|
|<span data-ttu-id="3d686-123">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="3d686-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="3d686-124">Получение объекта по идентификатору</span><span class="sxs-lookup"><span data-stu-id="3d686-124">Get an item by ID</span></span> | <span data-ttu-id="3d686-125">Нет</span><span class="sxs-lookup"><span data-stu-id="3d686-125">None</span></span> | <span data-ttu-id="3d686-126">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="3d686-126">To-do item</span></span>|
|<span data-ttu-id="3d686-127">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="3d686-127">POST /api/TodoItems</span></span> | <span data-ttu-id="3d686-128">Добавление нового элемента</span><span class="sxs-lookup"><span data-stu-id="3d686-128">Add a new item</span></span> | <span data-ttu-id="3d686-129">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="3d686-129">To-do item</span></span> | <span data-ttu-id="3d686-130">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="3d686-130">To-do item</span></span> |
|<span data-ttu-id="3d686-131">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="3d686-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="3d686-132">Обновление существующего элемента &nbsp;</span><span class="sxs-lookup"><span data-stu-id="3d686-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="3d686-133">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="3d686-133">To-do item</span></span> | <span data-ttu-id="3d686-134">Нет</span><span class="sxs-lookup"><span data-stu-id="3d686-134">None</span></span> |
|<span data-ttu-id="3d686-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="3d686-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="3d686-136">Удаление элемента &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="3d686-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="3d686-137">Нет</span><span class="sxs-lookup"><span data-stu-id="3d686-137">None</span></span> | <span data-ttu-id="3d686-138">Нет</span><span class="sxs-lookup"><span data-stu-id="3d686-138">None</span></span>|

<span data-ttu-id="3d686-139">На следующем рисунке показана структура приложения.</span><span class="sxs-lookup"><span data-stu-id="3d686-139">The following diagram shows the design of the app.</span></span>

![Клиент представлен прямоугольником слева.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="3d686-145">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="3d686-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3d686-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3d686-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3d686-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3d686-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3d686-148">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="3d686-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="3d686-149">Создайте веб-проект.</span><span class="sxs-lookup"><span data-stu-id="3d686-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3d686-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3d686-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3d686-151">В меню **Файл** выберите **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="3d686-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="3d686-152">Выберите шаблон **Веб-приложение ASP.NET Core** и нажмите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="3d686-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="3d686-153">Назовите проект *TodoApi* и нажмите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="3d686-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="3d686-154">В диалоговом окне **Создание веб-приложения ASP.NET Core** убедитесь в том, что выбраны платформы **.NET Core** и **ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="3d686-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span> <span data-ttu-id="3d686-155">Выберите шаблон **API** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="3d686-155">Select the **API** template and click **Create**.</span></span>

![Диалоговое окно создания проекта VS](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3d686-157">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3d686-157">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="3d686-158">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="3d686-158">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="3d686-159">Смените каталог (`cd`) на папку, в которой будет содержаться папка проекта.</span><span class="sxs-lookup"><span data-stu-id="3d686-159">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="3d686-160">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="3d686-160">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoAPI
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* <span data-ttu-id="3d686-161">При появлении диалогового окна с запросом на добавление в проект необходимых ресурсов выберите **Да**.</span><span class="sxs-lookup"><span data-stu-id="3d686-161">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="3d686-162">Предыдущие команды:</span><span class="sxs-lookup"><span data-stu-id="3d686-162">The preceding commands:</span></span>

  * <span data-ttu-id="3d686-163">Создает новый проект веб-API и открывает его в Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="3d686-163">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="3d686-164">Добавляет пакеты NuGet, которые понадобятся в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="3d686-164">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3d686-165">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="3d686-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="3d686-166">Выберите **Файл** > **Новое решение**.</span><span class="sxs-lookup"><span data-stu-id="3d686-166">Select **File** > **New Solution**.</span></span>

  ![Новое решение macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="3d686-168">Выберите **.NET Core** > **Приложение** > **API** > **Далее**.</span><span class="sxs-lookup"><span data-stu-id="3d686-168">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![Диалоговое окно "Новый проект" в macOS](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="3d686-170">В диалоговом окне **Настройка нового веб-API ASP.NET Core** в качестве **Целевой платформы** выберите \* *.NET Core 3.0*.</span><span class="sxs-lookup"><span data-stu-id="3d686-170">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.0*.</span></span>

* <span data-ttu-id="3d686-171">Введите *TodoApi* в поле **Имя проекта** и выберите команду **Создать**.</span><span class="sxs-lookup"><span data-stu-id="3d686-171">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Диалоговое окно конфигурации](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="3d686-173">Откройте командный терминал в папке проекта и выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="3d686-173">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="3d686-174">Тестирование API</span><span class="sxs-lookup"><span data-stu-id="3d686-174">Test the API</span></span>

<span data-ttu-id="3d686-175">Шаблон проекта создает API `WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="3d686-175">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="3d686-176">Вызовите метод `Get` из браузера для тестирования приложения.</span><span class="sxs-lookup"><span data-stu-id="3d686-176">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3d686-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3d686-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="3d686-178">Нажмите клавиши CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="3d686-178">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="3d686-179">Visual Studio запустит браузер и перейдет к `https://localhost:<port>/WeatherForecast`, где `<port>` — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="3d686-179">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="3d686-180">Если появится диалоговое окно с запросом о необходимости доверять сертификату IIS Express, выберите **Да**.</span><span class="sxs-lookup"><span data-stu-id="3d686-180">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="3d686-181">В появляющемся следом диалоговом окне **Предупреждение системы безопасности** выберите **Да**.</span><span class="sxs-lookup"><span data-stu-id="3d686-181">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3d686-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3d686-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="3d686-183">Нажмите клавиши CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="3d686-183">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="3d686-184">В браузере перейдите по следующему URL-адресу: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="3d686-184">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3d686-185">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="3d686-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="3d686-186">Выберите **Выполнить** > **Начать отладку**, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="3d686-186">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="3d686-187">Visual Studio для Mac запустит браузер и перейдет к `https://localhost:<port>`, где `<port>` — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="3d686-187">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="3d686-188">Появится ошибка HTTP 404 (Не найдено).</span><span class="sxs-lookup"><span data-stu-id="3d686-188">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="3d686-189">Добавьте `/WeatherForecast` к URL-адресу (измените URL-адрес на `https://localhost:<port>/WeatherForecast`).</span><span class="sxs-lookup"><span data-stu-id="3d686-189">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="3d686-190">Возвращаемые данные JSON будут выглядеть примерно так:</span><span class="sxs-lookup"><span data-stu-id="3d686-190">JSON similar to the following is returned:</span></span>

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

## <a name="add-a-model-class"></a><span data-ttu-id="3d686-191">Добавление класса модели</span><span class="sxs-lookup"><span data-stu-id="3d686-191">Add a model class</span></span>

<span data-ttu-id="3d686-192">*Модель* — это набор классов, представляющих данные, которыми управляет приложение.</span><span class="sxs-lookup"><span data-stu-id="3d686-192">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="3d686-193">Модель этого приложения содержит единственный класс `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="3d686-193">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3d686-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3d686-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3d686-195">В **обозревателе решений** щелкните проект правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="3d686-195">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="3d686-196">Выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="3d686-196">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="3d686-197">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="3d686-197">Name the folder *Models*.</span></span>

* <span data-ttu-id="3d686-198">Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="3d686-198">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="3d686-199">Присвойте классу имя *TodoItem* и выберите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="3d686-199">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="3d686-200">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="3d686-200">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3d686-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3d686-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="3d686-202">Добавьте папку с именем *Models*.</span><span class="sxs-lookup"><span data-stu-id="3d686-202">Add a folder named *Models*.</span></span>

* <span data-ttu-id="3d686-203">Добавьте класс `TodoItem` в папку *Models*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="3d686-203">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3d686-204">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="3d686-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="3d686-205">Щелкните проект правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="3d686-205">Right-click the project.</span></span> <span data-ttu-id="3d686-206">Выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="3d686-206">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="3d686-207">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="3d686-207">Name the folder *Models*.</span></span>

  ![Новая папка](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="3d686-209">Правой кнопкой мыши щелкните папку *Models* и выберите **Добавить** > **Новый файл** > **Общие** > **Пустой класс**.</span><span class="sxs-lookup"><span data-stu-id="3d686-209">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="3d686-210">Назовите класс *TodoItem* и нажмите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="3d686-210">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="3d686-211">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="3d686-211">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="3d686-212">Свойство `Id` выступает в качестве уникального ключа реляционной базы данных.</span><span class="sxs-lookup"><span data-stu-id="3d686-212">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="3d686-213">Классы моделей можно размещать в любом месте проекта, но обычно для этого используется папка *Models*.</span><span class="sxs-lookup"><span data-stu-id="3d686-213">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="3d686-214">Добавление контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="3d686-214">Add a database context</span></span>

<span data-ttu-id="3d686-215">*Контекст базы данных* —это основной класс, который координирует функциональные возможности Entity Framework для модели данных.</span><span class="sxs-lookup"><span data-stu-id="3d686-215">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="3d686-216">Этот класс является производным от класса `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="3d686-216">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3d686-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3d686-217">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="3d686-218">Добавление Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="3d686-218">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="3d686-219">В меню **Сервис** выберите **Диспетчер пакетов NuGet > Управление пакетами NuGet для решения**.</span><span class="sxs-lookup"><span data-stu-id="3d686-219">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="3d686-220">Перейдите на вкладку **Обзор** и введите **Microsoft.EntityFrameworkCore.SqlServer** в поле поиска.</span><span class="sxs-lookup"><span data-stu-id="3d686-220">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="3d686-221">На панели слева выберите **Microsoft.EntityFrameworkCore.SqlServer**.</span><span class="sxs-lookup"><span data-stu-id="3d686-221">Select  **Microsoft.EntityFrameworkCore.SqlServer** in the left pane.</span></span>
* <span data-ttu-id="3d686-222">Установите флажок **Проект** на правой панели и выберите **Установить**.</span><span class="sxs-lookup"><span data-stu-id="3d686-222">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="3d686-223">Добавьте пакет NuGet `Microsoft.EntityFrameworkCore.InMemory` согласно инструкциям выше.</span><span class="sxs-lookup"><span data-stu-id="3d686-223">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![Диспетчер пакетов NuGet](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="3d686-225">Добавление контекста базы данных TodoContext</span><span class="sxs-lookup"><span data-stu-id="3d686-225">Add the TodoContext database context</span></span>

* <span data-ttu-id="3d686-226">Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="3d686-226">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="3d686-227">Назовите класс *TodoContext* и нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="3d686-227">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="3d686-228">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="3d686-228">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="3d686-229">Добавьте класс `TodoContext` в папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="3d686-229">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="3d686-230">Введите следующий код:</span><span class="sxs-lookup"><span data-stu-id="3d686-230">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="3d686-231">Регистрация контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="3d686-231">Register the database context</span></span>

<span data-ttu-id="3d686-232">В ASP.NET Core службы (такие как контекст базы данных) должны быть зарегистрированы с помощью контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="3d686-232">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="3d686-233">Контейнер предоставляет службу контроллерам.</span><span class="sxs-lookup"><span data-stu-id="3d686-233">The container provides the service to controllers.</span></span>

<span data-ttu-id="3d686-234">Обновите файл *Startup.cs*, используя следующий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="3d686-234">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="3d686-235">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="3d686-235">The preceding code:</span></span>

* <span data-ttu-id="3d686-236">Удалите неиспользуемые объявления `using`.</span><span class="sxs-lookup"><span data-stu-id="3d686-236">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="3d686-237">Добавляет контекст базы данных в контейнер внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="3d686-237">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="3d686-238">Указывает, что контекст базы данных будет использовать базу данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="3d686-238">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="3d686-239">Формирование шаблонов контроллера</span><span class="sxs-lookup"><span data-stu-id="3d686-239">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3d686-240">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3d686-240">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3d686-241">Щелкните папку *Controllers* правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="3d686-241">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="3d686-242">Выберите **Добавить** > **Создать шаблонный элемент**.</span><span class="sxs-lookup"><span data-stu-id="3d686-242">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="3d686-243">Выберите **Контроллер API с действиями, использующий Entity Framework**, а затем выберите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="3d686-243">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="3d686-244">В диалоговом окне **Контроллер API с действиями, использующий Entity Framework** сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="3d686-244">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="3d686-245">Выберите **TodoItem (TodoAPI.Models)** в поле **Класс модели**.</span><span class="sxs-lookup"><span data-stu-id="3d686-245">Select **TodoItem (TodoAPI.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="3d686-246">Выберите **TodoContext (TodoAPI.Models)** в поле **Класс контекста данных**.</span><span class="sxs-lookup"><span data-stu-id="3d686-246">Select **TodoContext (TodoAPI.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="3d686-247">Нажмите **Добавить**</span><span class="sxs-lookup"><span data-stu-id="3d686-247">Select **Add**</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="3d686-248">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="3d686-248">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="3d686-249">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="3d686-249">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext  -outDir Controllers
```

<span data-ttu-id="3d686-250">Предыдущие команды:</span><span class="sxs-lookup"><span data-stu-id="3d686-250">The preceding commands:</span></span>

* <span data-ttu-id="3d686-251">добавляют пакеты NuGet, необходимые для формирования шаблонов;</span><span class="sxs-lookup"><span data-stu-id="3d686-251">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="3d686-252">устанавливают подсистему формирования шаблонов (`dotnet-aspnet-codegenerator`);</span><span class="sxs-lookup"><span data-stu-id="3d686-252">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="3d686-253">формируют шаблоны для `TodoItemsController`.</span><span class="sxs-lookup"><span data-stu-id="3d686-253">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="3d686-254">Сформированный код:</span><span class="sxs-lookup"><span data-stu-id="3d686-254">The generated code:</span></span>

* <span data-ttu-id="3d686-255">Определяет класс контроллера API без методов.</span><span class="sxs-lookup"><span data-stu-id="3d686-255">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="3d686-256">Добавляет в класс атрибут [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="3d686-256">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="3d686-257">Этот атрибут указывает, что контроллер отвечает на запросы веб-API.</span><span class="sxs-lookup"><span data-stu-id="3d686-257">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="3d686-258">Дополнительные сведения о поведении, которое реализует этот атрибут, см. в <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="3d686-258">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="3d686-259">Использует внедрение зависимостей для внедрения контекста базы данных (`TodoContext`) в контроллер.</span><span class="sxs-lookup"><span data-stu-id="3d686-259">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="3d686-260">Контекст базы данных используется в каждом методе [создания, чтения, обновления и удаления](https://wikipedia.org/wiki/Create,_read,_update_and_delete) в контроллере.</span><span class="sxs-lookup"><span data-stu-id="3d686-260">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="3d686-261">Знакомство с методом создания PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="3d686-261">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="3d686-262">Измените инструкцию возврата в `PostTodoItem` и используйте оператор [nameof](/dotnet/csharp/language-reference/operators/nameof):</span><span class="sxs-lookup"><span data-stu-id="3d686-262">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="3d686-263">Предыдущий код является методом HTTP POST, как указывает атрибут [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="3d686-263">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="3d686-264">Этот метод получает значение элемента списка дел из текста HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="3d686-264">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="3d686-265">Метод <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*>:</span><span class="sxs-lookup"><span data-stu-id="3d686-265">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="3d686-266">В случае успеха возвращает код состояния HTTP 201.</span><span class="sxs-lookup"><span data-stu-id="3d686-266">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="3d686-267">HTTP 201 представляет собой стандартный ответ для метода HTTP POST, создающий ресурс на сервере.</span><span class="sxs-lookup"><span data-stu-id="3d686-267">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="3d686-268">Добавляет в ответ заголовок [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location).</span><span class="sxs-lookup"><span data-stu-id="3d686-268">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="3d686-269">Заголовок `Location` указывает [URI](https://developer.mozilla.org/docs/Glossary/URI) новой созданной задачи.</span><span class="sxs-lookup"><span data-stu-id="3d686-269">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="3d686-270">Дополнительные сведения см. в статье [10.2.2 201 "Создан ресурс"](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="3d686-270">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="3d686-271">Указывает действие `GetTodoItem` для создания URI заголовка `Location`.</span><span class="sxs-lookup"><span data-stu-id="3d686-271">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="3d686-272">Ключевое слово `nameof` C# используется для предотвращения жесткого программирования имени действия в вызове `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="3d686-272">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="3d686-273">Установка Postman</span><span class="sxs-lookup"><span data-stu-id="3d686-273">Install Postman</span></span>

<span data-ttu-id="3d686-274">В этом учебнике для тестирования веб-API используется Postman.</span><span class="sxs-lookup"><span data-stu-id="3d686-274">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="3d686-275">Установка [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="3d686-275">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="3d686-276">Запустите веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="3d686-276">Start the web app.</span></span>
* <span data-ttu-id="3d686-277">Запустите Postman.</span><span class="sxs-lookup"><span data-stu-id="3d686-277">Start Postman.</span></span>
* <span data-ttu-id="3d686-278">Отключение параметра **Проверка SSL-сертификата**</span><span class="sxs-lookup"><span data-stu-id="3d686-278">Disable **SSL certificate verification**</span></span>
* <span data-ttu-id="3d686-279">В меню **Файл > Параметры** (вкладка \**Общие*) отключите параметр **Проверка SSL-сертификата**.</span><span class="sxs-lookup"><span data-stu-id="3d686-279">From  **File > Settings** (\**General* tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="3d686-280">После тестирования контроллера снова включите проверку SSL-сертификата.</span><span class="sxs-lookup"><span data-stu-id="3d686-280">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="3d686-281">Тестирование PostTodoItem с использованием Postman</span><span class="sxs-lookup"><span data-stu-id="3d686-281">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="3d686-282">Создайте новый запрос.</span><span class="sxs-lookup"><span data-stu-id="3d686-282">Create a new request.</span></span>
* <span data-ttu-id="3d686-283">Установите HTTP-метод `POST`.</span><span class="sxs-lookup"><span data-stu-id="3d686-283">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="3d686-284">Откройте вкладку **Тело**.</span><span class="sxs-lookup"><span data-stu-id="3d686-284">Select the **Body** tab.</span></span>
* <span data-ttu-id="3d686-285">Установите переключатель **без обработки**.</span><span class="sxs-lookup"><span data-stu-id="3d686-285">Select the **raw** radio button.</span></span>
* <span data-ttu-id="3d686-286">Задайте тип **JSON (приложение/json)** .</span><span class="sxs-lookup"><span data-stu-id="3d686-286">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="3d686-287">В теле запроса введите код JSON для элемента списка дел:</span><span class="sxs-lookup"><span data-stu-id="3d686-287">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="3d686-288">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="3d686-288">Select **Send**.</span></span>

  ![Postman с запросом Create](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="3d686-290">Тестирование URI заголовка расположения</span><span class="sxs-lookup"><span data-stu-id="3d686-290">Test the location header URI</span></span>

* <span data-ttu-id="3d686-291">Перейдите на вкладку **Заголовки** в области **Ответ**.</span><span class="sxs-lookup"><span data-stu-id="3d686-291">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="3d686-292">Скопируйте значение заголовка **Расположение**:</span><span class="sxs-lookup"><span data-stu-id="3d686-292">Copy the **Location** header value:</span></span>

  ![Вкладка "Заголовки" в консоли Postman](first-web-api/_static/3/create.png)

* <span data-ttu-id="3d686-294">Укажите метод GET.</span><span class="sxs-lookup"><span data-stu-id="3d686-294">Set the method to GET.</span></span>
* <span data-ttu-id="3d686-295">Вставьте URI (например, `https://localhost:5001/api/TodoItems/1`)</span><span class="sxs-lookup"><span data-stu-id="3d686-295">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`)</span></span>
* <span data-ttu-id="3d686-296">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="3d686-296">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="3d686-297">Знакомство с методами GET</span><span class="sxs-lookup"><span data-stu-id="3d686-297">Examine the GET methods</span></span>

<span data-ttu-id="3d686-298">Эти методы реализуют две конечные точки GET:</span><span class="sxs-lookup"><span data-stu-id="3d686-298">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="3d686-299">Протестируйте приложение, вызвав эти две конечные точки в браузере или в Postman.</span><span class="sxs-lookup"><span data-stu-id="3d686-299">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="3d686-300">Например:</span><span class="sxs-lookup"><span data-stu-id="3d686-300">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="3d686-301">При вызове `GetTodoItems` возвращается примерно такой ответ:</span><span class="sxs-lookup"><span data-stu-id="3d686-301">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="3d686-302">Тестирование Get с использованием Postman</span><span class="sxs-lookup"><span data-stu-id="3d686-302">Test Get with Postman</span></span>

* <span data-ttu-id="3d686-303">Создайте новый запрос.</span><span class="sxs-lookup"><span data-stu-id="3d686-303">Create a new request.</span></span>
* <span data-ttu-id="3d686-304">Укажите метод HTTP **GET**.</span><span class="sxs-lookup"><span data-stu-id="3d686-304">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="3d686-305">Укажите URL-адрес запроса `https://localhost:<port>/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="3d686-305">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="3d686-306">Например, `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="3d686-306">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="3d686-307">Выберите режим **Представление с двумя областями** в Postman.</span><span class="sxs-lookup"><span data-stu-id="3d686-307">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="3d686-308">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="3d686-308">Select **Send**.</span></span>

<span data-ttu-id="3d686-309">Это приложение использует выполняющуюся в памяти базу данных.</span><span class="sxs-lookup"><span data-stu-id="3d686-309">This app uses an in-memory database.</span></span> <span data-ttu-id="3d686-310">Если остановить и вновь запустить его, предшествующий запрос GET не возвратит никаких данных.</span><span class="sxs-lookup"><span data-stu-id="3d686-310">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="3d686-311">Если данные не возвращаются, данные для приложения получаются методом [POST](#post).</span><span class="sxs-lookup"><span data-stu-id="3d686-311">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="3d686-312">Маршрутизация и пути URL</span><span class="sxs-lookup"><span data-stu-id="3d686-312">Routing and URL paths</span></span>

<span data-ttu-id="3d686-313">Атрибут [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) обозначает метод, который отвечает на запрос HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="3d686-313">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="3d686-314">Путь URL для каждого метода формируется следующим образом:</span><span class="sxs-lookup"><span data-stu-id="3d686-314">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="3d686-315">Возьмите строку шаблона в атрибуте `Route` контроллера:</span><span class="sxs-lookup"><span data-stu-id="3d686-315">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="3d686-316">Замените `[controller]` именем контроллера (по соглашению это имя класса контроллера без суффикса "Controller").</span><span class="sxs-lookup"><span data-stu-id="3d686-316">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="3d686-317">В этом примере класс контроллера имеет имя **TodoItems**, а сам контроллер, соответственно, — "TodoItems".</span><span class="sxs-lookup"><span data-stu-id="3d686-317">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="3d686-318">В ASP.NET Core [маршрутизация](xref:mvc/controllers/routing) реализуется без учета регистра символов.</span><span class="sxs-lookup"><span data-stu-id="3d686-318">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="3d686-319">Если атрибут `[HttpGet]` имеет шаблон маршрута (например, `[HttpGet("products")]`), добавьте его к пути.</span><span class="sxs-lookup"><span data-stu-id="3d686-319">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="3d686-320">В этом примере шаблон не используется.</span><span class="sxs-lookup"><span data-stu-id="3d686-320">This sample doesn't use a template.</span></span> <span data-ttu-id="3d686-321">Дополнительные сведения см. в разделе [Маршрутизация атрибутов с помощью атрибутов Http[Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="3d686-321">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="3d686-322">В следующем методе `GetTodoItem` `"{id}"` — это переменная-заполнитель для уникального идентификатора элемента задачи.</span><span class="sxs-lookup"><span data-stu-id="3d686-322">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="3d686-323">При вызове `GetTodoItem` параметру метода `id` присваивается значение `"{id}"` в URL-адресе.</span><span class="sxs-lookup"><span data-stu-id="3d686-323">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="3d686-324">Возвращаемые значения</span><span class="sxs-lookup"><span data-stu-id="3d686-324">Return values</span></span>

<span data-ttu-id="3d686-325">Возвращаемое значение имеет тип `GetTodoItems`, а метод `GetTodoItem` имеет тип [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="3d686-325">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="3d686-326">ASP.NET Core автоматически сериализует объект в формат [JSON](https://www.json.org/) и записывает данные JSON в тело сообщения ответа.</span><span class="sxs-lookup"><span data-stu-id="3d686-326">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="3d686-327">Код ответа для этого типа возвращаемого значения равен 200, что свидетельствует об отсутствии необработанных исключений.</span><span class="sxs-lookup"><span data-stu-id="3d686-327">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="3d686-328">Необработанные исключения преобразуются в ошибки 5xx.</span><span class="sxs-lookup"><span data-stu-id="3d686-328">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="3d686-329">Типы возвращаемых значений `ActionResult` могут представлять широкий спектр кодов состояний HTTP.</span><span class="sxs-lookup"><span data-stu-id="3d686-329">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="3d686-330">Например, метод `GetTodoItem` может возвращать два разных значения состояния:</span><span class="sxs-lookup"><span data-stu-id="3d686-330">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="3d686-331">Если запрошенному идентификатору не соответствует ни один элемент, метод возвращает ошибку 404 ([Не найдено](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound)).</span><span class="sxs-lookup"><span data-stu-id="3d686-331">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="3d686-332">В противном случае метод возвращает код 200 с телом ответа JSON.</span><span class="sxs-lookup"><span data-stu-id="3d686-332">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="3d686-333">При возвращении `item` возвращается ответ HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="3d686-333">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="3d686-334">Метод PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="3d686-334">The PutTodoItem method</span></span>

<span data-ttu-id="3d686-335">Проверьте метод `PutTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="3d686-335">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="3d686-336">Страница `PutTodoItem` аналогична странице `PostTodoItem`, но использует запрос HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="3d686-336">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="3d686-337">Ответ — [204 (Нет содержимого)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="3d686-337">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="3d686-338">Согласно спецификации HTTP, запрос PUT требует, чтобы клиент отправлял всю обновленную сущность, а не только изменения.</span><span class="sxs-lookup"><span data-stu-id="3d686-338">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="3d686-339">Чтобы обеспечить поддержку частичных обновлений, используйте [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="3d686-339">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="3d686-340">Если возникнет ошибка вызова `PutTodoItem`, вызовите `GET`, чтобы в базе данных был один элемент.</span><span class="sxs-lookup"><span data-stu-id="3d686-340">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="3d686-341">Тестирование метода PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="3d686-341">Test the PutTodoItem method</span></span>

<span data-ttu-id="3d686-342">Этот пример использует базу данных в памяти, которая должна быть инициирована при каждом запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="3d686-342">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="3d686-343">При выполнении вызова PUT в базе данных уже должен существовать какой-либо элемент.</span><span class="sxs-lookup"><span data-stu-id="3d686-343">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="3d686-344">Для этого перед вызовом PUT выполните вызов GET, чтобы убедиться в наличии такого элемента в базе данных.</span><span class="sxs-lookup"><span data-stu-id="3d686-344">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="3d686-345">Обновите элемент списка дел с идентификатором 1 и присвойте ему имя "feed fish":</span><span class="sxs-lookup"><span data-stu-id="3d686-345">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="3d686-346">На следующем рисунке показан процесс обновления Postman:</span><span class="sxs-lookup"><span data-stu-id="3d686-346">The following image shows the Postman update:</span></span>

![Консоль Postman с ответом 204 (Нет содержимого)](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="3d686-348">Метод DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="3d686-348">The DeleteTodoItem method</span></span>

<span data-ttu-id="3d686-349">Проверьте метод `DeleteTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="3d686-349">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

<span data-ttu-id="3d686-350">`DeleteTodoItem`Ответ[ — 204 (нет содержимого)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="3d686-350">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="3d686-351">Тестирование метода DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="3d686-351">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="3d686-352">Удалите элемент списка дел с помощью Postman:</span><span class="sxs-lookup"><span data-stu-id="3d686-352">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="3d686-353">Укажите метод `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="3d686-353">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="3d686-354">Укажите URI удаляемого объекта, например `https://localhost:5001/api/TodoItems/1`</span><span class="sxs-lookup"><span data-stu-id="3d686-354">Set the URI of the object to delete, for example `https://localhost:5001/api/TodoItems/1`</span></span>
* <span data-ttu-id="3d686-355">Нажмите кнопку **Отправить**</span><span class="sxs-lookup"><span data-stu-id="3d686-355">Select **Send**</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="3d686-356">Вызов веб-API с помощью JavaScript</span><span class="sxs-lookup"><span data-stu-id="3d686-356">Call the web API with JavaScript</span></span>

<span data-ttu-id="3d686-357">Пошаговые инструкции см. в [руководстве Вызовите веб-API ASP.NET Core с помощью JavaScript](xref:tutorials/web-api-javascript).</span><span class="sxs-lookup"><span data-stu-id="3d686-357">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="3d686-358">В этом руководстве вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="3d686-358">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3d686-359">Создание проекта веб-API.</span><span class="sxs-lookup"><span data-stu-id="3d686-359">Create a web API project.</span></span>
> * <span data-ttu-id="3d686-360">Добавление класса модели и контекста базы данных.</span><span class="sxs-lookup"><span data-stu-id="3d686-360">Add a model class and a database context.</span></span>
> * <span data-ttu-id="3d686-361">Добавление контроллера.</span><span class="sxs-lookup"><span data-stu-id="3d686-361">Add a controller.</span></span>
> * <span data-ttu-id="3d686-362">Добавление методов CRUD.</span><span class="sxs-lookup"><span data-stu-id="3d686-362">Add CRUD methods.</span></span>
> * <span data-ttu-id="3d686-363">Настройка маршрутизации и путей URL.</span><span class="sxs-lookup"><span data-stu-id="3d686-363">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="3d686-364">Указание возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="3d686-364">Specify return values.</span></span>
> * <span data-ttu-id="3d686-365">Вызов веб-API с помощью Postman.</span><span class="sxs-lookup"><span data-stu-id="3d686-365">Call the web API with Postman.</span></span>
> * <span data-ttu-id="3d686-366">Вызовите веб-API с помощью JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3d686-366">Call the web API with JavaScript.</span></span>

<span data-ttu-id="3d686-367">В конечном итоге вы получите веб-API, обеспечивающий управление элементами списка дел, хранящимися в реляционной базе данных.</span><span class="sxs-lookup"><span data-stu-id="3d686-367">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="3d686-368">Обзор</span><span class="sxs-lookup"><span data-stu-id="3d686-368">Overview</span></span>

<span data-ttu-id="3d686-369">В этом руководстве создается следующий API-интерфейс:</span><span class="sxs-lookup"><span data-stu-id="3d686-369">This tutorial creates the following API:</span></span>

|<span data-ttu-id="3d686-370">API</span><span class="sxs-lookup"><span data-stu-id="3d686-370">API</span></span> | <span data-ttu-id="3d686-371">ОПИСАНИЕ</span><span class="sxs-lookup"><span data-stu-id="3d686-371">Description</span></span> | <span data-ttu-id="3d686-372">Текст запроса</span><span class="sxs-lookup"><span data-stu-id="3d686-372">Request body</span></span> | <span data-ttu-id="3d686-373">Текст ответа</span><span class="sxs-lookup"><span data-stu-id="3d686-373">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="3d686-374">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="3d686-374">GET /api/TodoItems</span></span> | <span data-ttu-id="3d686-375">Получение всех элементов задач</span><span class="sxs-lookup"><span data-stu-id="3d686-375">Get all to-do items</span></span> | <span data-ttu-id="3d686-376">Нет</span><span class="sxs-lookup"><span data-stu-id="3d686-376">None</span></span> | <span data-ttu-id="3d686-377">Массив элементов задач</span><span class="sxs-lookup"><span data-stu-id="3d686-377">Array of to-do items</span></span>|
|<span data-ttu-id="3d686-378">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="3d686-378">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="3d686-379">Получение объекта по идентификатору</span><span class="sxs-lookup"><span data-stu-id="3d686-379">Get an item by ID</span></span> | <span data-ttu-id="3d686-380">Нет</span><span class="sxs-lookup"><span data-stu-id="3d686-380">None</span></span> | <span data-ttu-id="3d686-381">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="3d686-381">To-do item</span></span>|
|<span data-ttu-id="3d686-382">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="3d686-382">POST /api/TodoItems</span></span> | <span data-ttu-id="3d686-383">Добавление нового элемента</span><span class="sxs-lookup"><span data-stu-id="3d686-383">Add a new item</span></span> | <span data-ttu-id="3d686-384">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="3d686-384">To-do item</span></span> | <span data-ttu-id="3d686-385">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="3d686-385">To-do item</span></span> |
|<span data-ttu-id="3d686-386">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="3d686-386">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="3d686-387">Обновление существующего элемента &nbsp;</span><span class="sxs-lookup"><span data-stu-id="3d686-387">Update an existing item &nbsp;</span></span> | <span data-ttu-id="3d686-388">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="3d686-388">To-do item</span></span> | <span data-ttu-id="3d686-389">Нет</span><span class="sxs-lookup"><span data-stu-id="3d686-389">None</span></span> |
|<span data-ttu-id="3d686-390">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="3d686-390">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="3d686-391">Удаление элемента &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="3d686-391">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="3d686-392">Нет</span><span class="sxs-lookup"><span data-stu-id="3d686-392">None</span></span> | <span data-ttu-id="3d686-393">Нет</span><span class="sxs-lookup"><span data-stu-id="3d686-393">None</span></span>|

<span data-ttu-id="3d686-394">На следующем рисунке показана структура приложения.</span><span class="sxs-lookup"><span data-stu-id="3d686-394">The following diagram shows the design of the app.</span></span>

![Клиент представлен прямоугольником слева.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="3d686-400">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="3d686-400">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3d686-401">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3d686-401">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3d686-402">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3d686-402">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3d686-403">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="3d686-403">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="3d686-404">Создайте веб-проект.</span><span class="sxs-lookup"><span data-stu-id="3d686-404">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3d686-405">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3d686-405">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3d686-406">В меню **Файл** выберите **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="3d686-406">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="3d686-407">Выберите шаблон **Веб-приложение ASP.NET Core** и нажмите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="3d686-407">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="3d686-408">Назовите проект *TodoApi* и нажмите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="3d686-408">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="3d686-409">В диалоговом окне **Создание веб-приложения ASP.NET Core** убедитесь в том, что выбраны платформы **.NET Core** и **ASP.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="3d686-409">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="3d686-410">Выберите шаблон **API** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="3d686-410">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="3d686-411">**Не** выбирайте **Включение поддержки Docker**.</span><span class="sxs-lookup"><span data-stu-id="3d686-411">**Don't** select **Enable Docker Support**.</span></span>

![Диалоговое окно создания проекта VS](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3d686-413">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3d686-413">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="3d686-414">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="3d686-414">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="3d686-415">Смените каталог (`cd`) на папку, в которой будет содержаться папка проекта.</span><span class="sxs-lookup"><span data-stu-id="3d686-415">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="3d686-416">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="3d686-416">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="3d686-417">С помощью этих команд создается новый проект веб-API и открывается новый экземпляр Visual Studio Code в новой папке проекта.</span><span class="sxs-lookup"><span data-stu-id="3d686-417">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="3d686-418">При появлении диалогового окна с запросом на добавление в проект необходимых ресурсов выберите **Да**.</span><span class="sxs-lookup"><span data-stu-id="3d686-418">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3d686-419">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="3d686-419">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="3d686-420">Выберите **Файл** > **Новое решение**.</span><span class="sxs-lookup"><span data-stu-id="3d686-420">Select **File** > **New Solution**.</span></span>

  ![Новое решение macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="3d686-422">Выберите **.NET Core** > **Приложение** > **API** > **Далее**.</span><span class="sxs-lookup"><span data-stu-id="3d686-422">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![Диалоговое окно "Новый проект" в macOS](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="3d686-424">В диалоговом окне **Настройка нового веб-API ASP.NET Core** оставьте установленное по умолчанию значение для параметра **Целевая платформа**, то есть \* *.NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="3d686-424">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="3d686-425">Введите *TodoApi* в поле **Имя проекта** и выберите команду **Создать**.</span><span class="sxs-lookup"><span data-stu-id="3d686-425">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Диалоговое окно конфигурации](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="3d686-427">Тестирование API</span><span class="sxs-lookup"><span data-stu-id="3d686-427">Test the API</span></span>

<span data-ttu-id="3d686-428">Шаблон проекта создает API `values`.</span><span class="sxs-lookup"><span data-stu-id="3d686-428">The project template creates a `values` API.</span></span> <span data-ttu-id="3d686-429">Вызовите метод `Get` из браузера для тестирования приложения.</span><span class="sxs-lookup"><span data-stu-id="3d686-429">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3d686-430">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3d686-430">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="3d686-431">Нажмите клавиши CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="3d686-431">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="3d686-432">Visual Studio запустит браузер и перейдет к `https://localhost:<port>/api/values`, где `<port>` — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="3d686-432">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="3d686-433">Если появится диалоговое окно с запросом о необходимости доверять сертификату IIS Express, выберите **Да**.</span><span class="sxs-lookup"><span data-stu-id="3d686-433">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="3d686-434">В появляющемся следом диалоговом окне **Предупреждение системы безопасности** выберите **Да**.</span><span class="sxs-lookup"><span data-stu-id="3d686-434">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3d686-435">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3d686-435">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="3d686-436">Нажмите клавиши CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="3d686-436">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="3d686-437">В браузере перейдите по следующему URL-адресу: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="3d686-437">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3d686-438">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="3d686-438">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="3d686-439">Выберите **Выполнить** > **Начать отладку**, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="3d686-439">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="3d686-440">Visual Studio для Mac запустит браузер и перейдет к `https://localhost:<port>`, где `<port>` — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="3d686-440">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="3d686-441">Появится ошибка HTTP 404 (Не найдено).</span><span class="sxs-lookup"><span data-stu-id="3d686-441">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="3d686-442">Добавьте `/api/values` к URL-адресу (измените URL-адрес на `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="3d686-442">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="3d686-443">Возвращается следующий файл JSON:</span><span class="sxs-lookup"><span data-stu-id="3d686-443">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="3d686-444">Добавление класса модели</span><span class="sxs-lookup"><span data-stu-id="3d686-444">Add a model class</span></span>

<span data-ttu-id="3d686-445">*Модель* — это набор классов, представляющих данные, которыми управляет приложение.</span><span class="sxs-lookup"><span data-stu-id="3d686-445">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="3d686-446">Модель этого приложения содержит единственный класс `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="3d686-446">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3d686-447">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3d686-447">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3d686-448">В **обозревателе решений** щелкните проект правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="3d686-448">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="3d686-449">Выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="3d686-449">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="3d686-450">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="3d686-450">Name the folder *Models*.</span></span>

* <span data-ttu-id="3d686-451">Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="3d686-451">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="3d686-452">Присвойте классу имя *TodoItem* и выберите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="3d686-452">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="3d686-453">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="3d686-453">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3d686-454">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3d686-454">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="3d686-455">Добавьте папку с именем *Models*.</span><span class="sxs-lookup"><span data-stu-id="3d686-455">Add a folder named *Models*.</span></span>

* <span data-ttu-id="3d686-456">Добавьте класс `TodoItem` в папку *Models*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="3d686-456">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3d686-457">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="3d686-457">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="3d686-458">Щелкните проект правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="3d686-458">Right-click the project.</span></span> <span data-ttu-id="3d686-459">Выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="3d686-459">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="3d686-460">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="3d686-460">Name the folder *Models*.</span></span>

  ![Новая папка](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="3d686-462">Правой кнопкой мыши щелкните папку *Models* и выберите **Добавить** > **Новый файл** > **Общие** > **Пустой класс**.</span><span class="sxs-lookup"><span data-stu-id="3d686-462">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="3d686-463">Назовите класс *TodoItem* и нажмите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="3d686-463">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="3d686-464">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="3d686-464">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="3d686-465">Свойство `Id` выступает в качестве уникального ключа реляционной базы данных.</span><span class="sxs-lookup"><span data-stu-id="3d686-465">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="3d686-466">Классы моделей можно размещать в любом месте проекта, но обычно для этого используется папка *Models*.</span><span class="sxs-lookup"><span data-stu-id="3d686-466">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="3d686-467">Добавление контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="3d686-467">Add a database context</span></span>

<span data-ttu-id="3d686-468">*Контекст базы данных* —это основной класс, который координирует функциональные возможности Entity Framework для модели данных.</span><span class="sxs-lookup"><span data-stu-id="3d686-468">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="3d686-469">Этот класс является производным от класса `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="3d686-469">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3d686-470">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3d686-470">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3d686-471">Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="3d686-471">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="3d686-472">Назовите класс *TodoContext* и нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="3d686-472">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="3d686-473">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="3d686-473">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="3d686-474">Добавьте класс `TodoContext` в папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="3d686-474">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="3d686-475">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="3d686-475">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="3d686-476">Регистрация контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="3d686-476">Register the database context</span></span>

<span data-ttu-id="3d686-477">В ASP.NET Core службы (такие как контекст базы данных) должны быть зарегистрированы с помощью контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="3d686-477">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="3d686-478">Контейнер предоставляет службу контроллерам.</span><span class="sxs-lookup"><span data-stu-id="3d686-478">The container provides the service to controllers.</span></span>

<span data-ttu-id="3d686-479">Обновите файл *Startup.cs*, используя следующий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="3d686-479">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="3d686-480">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="3d686-480">The preceding code:</span></span>

* <span data-ttu-id="3d686-481">Удалите неиспользуемые объявления `using`.</span><span class="sxs-lookup"><span data-stu-id="3d686-481">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="3d686-482">Добавляет контекст базы данных в контейнер внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="3d686-482">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="3d686-483">Указывает, что контекст базы данных будет использовать базу данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="3d686-483">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="3d686-484">Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="3d686-484">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3d686-485">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3d686-485">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3d686-486">Щелкните папку *Controllers* правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="3d686-486">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="3d686-487">Выберите **Добавить** > **Новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="3d686-487">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="3d686-488">В диалоговом окне **Добавить новый элемент** выберите шаблон **Класс контроллера API**.</span><span class="sxs-lookup"><span data-stu-id="3d686-488">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="3d686-489">Присвойте классу имя *TodoController* и выберите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="3d686-489">Name the class *TodoController*, and select **Add**.</span></span>

  ![Диалоговое окно добавления элемента с контроллером в поле поиска и выбранным контроллером веб-API](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="3d686-491">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="3d686-491">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="3d686-492">В папке *Controllers* создайте класс с именем `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="3d686-492">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="3d686-493">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="3d686-493">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="3d686-494">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="3d686-494">The preceding code:</span></span>

* <span data-ttu-id="3d686-495">Определяет класс контроллера API без методов.</span><span class="sxs-lookup"><span data-stu-id="3d686-495">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="3d686-496">Добавляет в класс атрибут [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="3d686-496">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="3d686-497">Этот атрибут указывает, что контроллер отвечает на запросы веб-API.</span><span class="sxs-lookup"><span data-stu-id="3d686-497">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="3d686-498">Дополнительные сведения о поведении, которое реализует этот атрибут, см. в <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="3d686-498">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="3d686-499">Использует внедрение зависимостей для внедрения контекста базы данных (`TodoContext`) в контроллер.</span><span class="sxs-lookup"><span data-stu-id="3d686-499">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="3d686-500">Контекст базы данных используется в каждом методе [создания, чтения, обновления и удаления](https://wikipedia.org/wiki/Create,_read,_update_and_delete) в контроллере.</span><span class="sxs-lookup"><span data-stu-id="3d686-500">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="3d686-501">Добавляет элемент `Item1` в базу данных, если она пуста.</span><span class="sxs-lookup"><span data-stu-id="3d686-501">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="3d686-502">Этот код находится в конструкторе и выполняется каждый раз при обнаружении нового HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="3d686-502">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="3d686-503">Если вы удалите все элементы, конструктор создаст `Item1` при следующем вызове метода API.</span><span class="sxs-lookup"><span data-stu-id="3d686-503">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="3d686-504">Поэтому может создаться впечатление, что удаление не было выполнено, хотя это не так.</span><span class="sxs-lookup"><span data-stu-id="3d686-504">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="3d686-505">Добавление методов Get</span><span class="sxs-lookup"><span data-stu-id="3d686-505">Add Get methods</span></span>

<span data-ttu-id="3d686-506">Чтобы получить API, который извлекает элементы списка дел, добавьте следующие методы в класс `TodoController`:</span><span class="sxs-lookup"><span data-stu-id="3d686-506">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="3d686-507">Эти методы реализуют две конечные точки GET:</span><span class="sxs-lookup"><span data-stu-id="3d686-507">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="3d686-508">Если приложение все еще выполняется, оно останавливается.</span><span class="sxs-lookup"><span data-stu-id="3d686-508">Stop the app if it's still running.</span></span> <span data-ttu-id="3d686-509">Затем оно запускается снова с последними изменениями.</span><span class="sxs-lookup"><span data-stu-id="3d686-509">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="3d686-510">Протестируйте приложение, вызвав эти две конечные точки в браузере.</span><span class="sxs-lookup"><span data-stu-id="3d686-510">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="3d686-511">Например:</span><span class="sxs-lookup"><span data-stu-id="3d686-511">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="3d686-512">При вызове `GetTodoItems` возвращается следующий ответ HTTP:</span><span class="sxs-lookup"><span data-stu-id="3d686-512">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="3d686-513">Маршрутизация и пути URL</span><span class="sxs-lookup"><span data-stu-id="3d686-513">Routing and URL paths</span></span>

<span data-ttu-id="3d686-514">Атрибут [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) обозначает метод, который отвечает на запрос HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="3d686-514">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="3d686-515">Путь URL для каждого метода формируется следующим образом:</span><span class="sxs-lookup"><span data-stu-id="3d686-515">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="3d686-516">Возьмите строку шаблона в атрибуте `Route` контроллера:</span><span class="sxs-lookup"><span data-stu-id="3d686-516">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="3d686-517">Замените `[controller]` именем контроллера (по соглашению это имя класса контроллера без суффикса "Controller").</span><span class="sxs-lookup"><span data-stu-id="3d686-517">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="3d686-518">В этом примере класс контроллера носит имя **Todo**, а сам контроллер, соответственно, — "todo".</span><span class="sxs-lookup"><span data-stu-id="3d686-518">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="3d686-519">В ASP.NET Core [маршрутизация](xref:mvc/controllers/routing) реализуется без учета регистра символов.</span><span class="sxs-lookup"><span data-stu-id="3d686-519">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="3d686-520">Если атрибут `[HttpGet]` имеет шаблон маршрута (например, `[HttpGet("products")]`), добавьте его к пути.</span><span class="sxs-lookup"><span data-stu-id="3d686-520">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="3d686-521">В этом примере шаблон не используется.</span><span class="sxs-lookup"><span data-stu-id="3d686-521">This sample doesn't use a template.</span></span> <span data-ttu-id="3d686-522">Дополнительные сведения см. в разделе [Маршрутизация атрибутов с помощью атрибутов Http[Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="3d686-522">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="3d686-523">В следующем методе `GetTodoItem` `"{id}"` — это переменная-заполнитель для уникального идентификатора элемента задачи.</span><span class="sxs-lookup"><span data-stu-id="3d686-523">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="3d686-524">При вызове `GetTodoItem` параметру метода `id` присваивается значение `"{id}"` в URL-адресе.</span><span class="sxs-lookup"><span data-stu-id="3d686-524">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="3d686-525">Возвращаемые значения</span><span class="sxs-lookup"><span data-stu-id="3d686-525">Return values</span></span>

<span data-ttu-id="3d686-526">Возвращаемое значение имеет тип `GetTodoItems`, а метод `GetTodoItem` имеет тип [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="3d686-526">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="3d686-527">ASP.NET Core автоматически сериализует объект в формат [JSON](https://www.json.org/) и записывает данные JSON в тело сообщения ответа.</span><span class="sxs-lookup"><span data-stu-id="3d686-527">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="3d686-528">Код ответа для этого типа возвращаемого значения равен 200, что свидетельствует об отсутствии необработанных исключений.</span><span class="sxs-lookup"><span data-stu-id="3d686-528">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="3d686-529">Необработанные исключения преобразуются в ошибки 5xx.</span><span class="sxs-lookup"><span data-stu-id="3d686-529">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="3d686-530">Типы возвращаемых значений `ActionResult` могут представлять широкий спектр кодов состояний HTTP.</span><span class="sxs-lookup"><span data-stu-id="3d686-530">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="3d686-531">Например, метод `GetTodoItem` может возвращать два разных значения состояния:</span><span class="sxs-lookup"><span data-stu-id="3d686-531">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="3d686-532">Если запрошенному идентификатору не соответствует ни один элемент, метод возвращает ошибку 404 ([Не найдено](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound)).</span><span class="sxs-lookup"><span data-stu-id="3d686-532">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="3d686-533">В противном случае метод возвращает код 200 с телом ответа JSON.</span><span class="sxs-lookup"><span data-stu-id="3d686-533">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="3d686-534">При возвращении `item` возвращается ответ HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="3d686-534">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="3d686-535">Тестирование метода GetTodoItems</span><span class="sxs-lookup"><span data-stu-id="3d686-535">Test the GetTodoItems method</span></span>

<span data-ttu-id="3d686-536">В этом учебнике для тестирования веб-API используется Postman.</span><span class="sxs-lookup"><span data-stu-id="3d686-536">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="3d686-537">Установка [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="3d686-537">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="3d686-538">Запустите веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="3d686-538">Start the web app.</span></span>
* <span data-ttu-id="3d686-539">Запустите Postman.</span><span class="sxs-lookup"><span data-stu-id="3d686-539">Start Postman.</span></span>
* <span data-ttu-id="3d686-540">Отключение параметра **Проверка SSL-сертификата**</span><span class="sxs-lookup"><span data-stu-id="3d686-540">Disable **SSL certificate verification**</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3d686-541">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3d686-541">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3d686-542">В меню **Файл** > **Параметры** (вкладка **Общие**) отключите параметр **Проверка SSL-сертификата**.</span><span class="sxs-lookup"><span data-stu-id="3d686-542">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="3d686-543">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="3d686-543">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="3d686-544">В меню **Postman**  >  **Настройки** (вкладка **Общие**) отключите параметр **Проверка SSL-сертификата**.</span><span class="sxs-lookup"><span data-stu-id="3d686-544">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="3d686-545">Кроме того, можно выбрать значок гаечного ключа и выбрать **Параметры**, а затем отключить проверку SSL-сертификата.</span><span class="sxs-lookup"><span data-stu-id="3d686-545">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="3d686-546">После тестирования контроллера снова включите проверку SSL-сертификата.</span><span class="sxs-lookup"><span data-stu-id="3d686-546">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="3d686-547">Создайте новый запрос.</span><span class="sxs-lookup"><span data-stu-id="3d686-547">Create a new request.</span></span>
  * <span data-ttu-id="3d686-548">Укажите метод HTTP **GET**.</span><span class="sxs-lookup"><span data-stu-id="3d686-548">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="3d686-549">Укажите URL-адрес запроса `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="3d686-549">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="3d686-550">Например, `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="3d686-550">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="3d686-551">Выберите режим **Представление с двумя областями** в Postman.</span><span class="sxs-lookup"><span data-stu-id="3d686-551">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="3d686-552">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="3d686-552">Select **Send**.</span></span>

![Postman с запросом Get](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="3d686-554">Добавление метода Create</span><span class="sxs-lookup"><span data-stu-id="3d686-554">Add a Create method</span></span>

<span data-ttu-id="3d686-555">Добавьте следующий метод `PostTodoItem` в *Controllers/TodoController.cs*:</span><span class="sxs-lookup"><span data-stu-id="3d686-555">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="3d686-556">Предыдущий код является методом HTTP POST, как указывает атрибут [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="3d686-556">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="3d686-557">Этот метод получает значение элемента списка дел из текста HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="3d686-557">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="3d686-558">Метод `CreatedAtAction`:</span><span class="sxs-lookup"><span data-stu-id="3d686-558">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="3d686-559">В случае успеха возвращает код состояния HTTP 201.</span><span class="sxs-lookup"><span data-stu-id="3d686-559">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="3d686-560">HTTP 201 представляет собой стандартный ответ для метода HTTP POST, создающий ресурс на сервере.</span><span class="sxs-lookup"><span data-stu-id="3d686-560">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="3d686-561">Добавляет заголовок `Location` в ответ.</span><span class="sxs-lookup"><span data-stu-id="3d686-561">Adds a `Location` header to the response.</span></span> <span data-ttu-id="3d686-562">Заголовок `Location` расположения указывает URI вновь созданной задачи.</span><span class="sxs-lookup"><span data-stu-id="3d686-562">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="3d686-563">Дополнительные сведения см. в статье [10.2.2 201 "Создан ресурс"](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="3d686-563">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="3d686-564">Указывает действие `GetTodoItem` для создания URI заголовка `Location`.</span><span class="sxs-lookup"><span data-stu-id="3d686-564">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="3d686-565">Ключевое слово `nameof` C# используется для предотвращения жесткого программирования имени действия в вызове `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="3d686-565">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="3d686-566">Тестирование метода PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="3d686-566">Test the PostTodoItem method</span></span>

* <span data-ttu-id="3d686-567">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="3d686-567">Build the project.</span></span>
* <span data-ttu-id="3d686-568">В Postman укажите метод HTTP `POST`.</span><span class="sxs-lookup"><span data-stu-id="3d686-568">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="3d686-569">Откройте вкладку **Тело**.</span><span class="sxs-lookup"><span data-stu-id="3d686-569">Select the **Body** tab.</span></span>
* <span data-ttu-id="3d686-570">Установите переключатель **без обработки**.</span><span class="sxs-lookup"><span data-stu-id="3d686-570">Select the **raw** radio button.</span></span>
* <span data-ttu-id="3d686-571">Задайте тип **JSON (приложение/json)** .</span><span class="sxs-lookup"><span data-stu-id="3d686-571">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="3d686-572">В теле запроса введите код JSON для элемента списка дел:</span><span class="sxs-lookup"><span data-stu-id="3d686-572">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="3d686-573">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="3d686-573">Select **Send**.</span></span>

  ![Postman с запросом Create](first-web-api/_static/create.png)

  <span data-ttu-id="3d686-575">Если вы получаете ошибку 405 "Недопустимый метод", это может свидетельствовать о том, что после добавления метода `PostTodoItem` не была выполнена компиляция проекта.</span><span class="sxs-lookup"><span data-stu-id="3d686-575">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="3d686-576">Тестирование URI заголовка расположения</span><span class="sxs-lookup"><span data-stu-id="3d686-576">Test the location header URI</span></span>

* <span data-ttu-id="3d686-577">Перейдите на вкладку **Заголовки** в области **Ответ**.</span><span class="sxs-lookup"><span data-stu-id="3d686-577">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="3d686-578">Скопируйте значение заголовка **Расположение**:</span><span class="sxs-lookup"><span data-stu-id="3d686-578">Copy the **Location** header value:</span></span>

  ![Вкладка "Заголовки" в консоли Postman](first-web-api/_static/pmc2.png)

* <span data-ttu-id="3d686-580">Укажите метод GET.</span><span class="sxs-lookup"><span data-stu-id="3d686-580">Set the method to GET.</span></span>
* <span data-ttu-id="3d686-581">Вставьте URI (например, `https://localhost:5001/api/Todo/2`)</span><span class="sxs-lookup"><span data-stu-id="3d686-581">Paste the URI (for example, `https://localhost:5001/api/Todo/2`)</span></span>
* <span data-ttu-id="3d686-582">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="3d686-582">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="3d686-583">Добавление метода PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="3d686-583">Add a PutTodoItem method</span></span>

<span data-ttu-id="3d686-584">Добавьте приведенный ниже метод `PutTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="3d686-584">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="3d686-585">Страница `PutTodoItem` аналогична странице `PostTodoItem`, но использует запрос HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="3d686-585">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="3d686-586">Ответ — [204 (Нет содержимого)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="3d686-586">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="3d686-587">Согласно спецификации HTTP, запрос PUT требует, чтобы клиент отправлял всю обновленную сущность, а не только изменения.</span><span class="sxs-lookup"><span data-stu-id="3d686-587">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="3d686-588">Чтобы обеспечить поддержку частичных обновлений, используйте [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="3d686-588">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="3d686-589">Если возникнет ошибка вызова `PutTodoItem`, вызовите `GET`, чтобы в базе данных был один элемент.</span><span class="sxs-lookup"><span data-stu-id="3d686-589">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="3d686-590">Тестирование метода PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="3d686-590">Test the PutTodoItem method</span></span>

<span data-ttu-id="3d686-591">Этот пример использует базу данных в памяти, которая должна быть инициирована при каждом запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="3d686-591">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="3d686-592">При выполнении вызова PUT в базе данных уже должен существовать какой-либо элемент.</span><span class="sxs-lookup"><span data-stu-id="3d686-592">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="3d686-593">Для этого перед вызовом PUT выполните вызов GET, чтобы убедиться в наличии такого элемента в базе данных.</span><span class="sxs-lookup"><span data-stu-id="3d686-593">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="3d686-594">Обновите элемент списка дел с идентификатором = 1 и присвойте ему имя "feed fish":</span><span class="sxs-lookup"><span data-stu-id="3d686-594">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="3d686-595">На следующем рисунке показан процесс обновления Postman:</span><span class="sxs-lookup"><span data-stu-id="3d686-595">The following image shows the Postman update:</span></span>

![Консоль Postman с ответом 204 (Нет содержимого)](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="3d686-597">Добавление метода DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="3d686-597">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="3d686-598">Добавьте приведенный ниже метод `DeleteTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="3d686-598">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="3d686-599">`DeleteTodoItem`Ответ[ — 204 (нет содержимого)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="3d686-599">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="3d686-600">Тестирование метода DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="3d686-600">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="3d686-601">Удалите элемент списка дел с помощью Postman:</span><span class="sxs-lookup"><span data-stu-id="3d686-601">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="3d686-602">Укажите метод `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="3d686-602">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="3d686-603">Укажите URI удаляемого объекта, например `https://localhost:5001/api/todo/1`</span><span class="sxs-lookup"><span data-stu-id="3d686-603">Set the URI of the object to delete, for example `https://localhost:5001/api/todo/1`</span></span>
* <span data-ttu-id="3d686-604">Нажмите кнопку **Отправить**</span><span class="sxs-lookup"><span data-stu-id="3d686-604">Select **Send**</span></span>

<span data-ttu-id="3d686-605">В этом примере приложения вы можете удалить все элементы.</span><span class="sxs-lookup"><span data-stu-id="3d686-605">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="3d686-606">Однако в случае удаления последнего элемента в момент следующего вызова API конструктор класса модели создаст новый элемент.</span><span class="sxs-lookup"><span data-stu-id="3d686-606">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="3d686-607">Вызов веб-API с помощью JavaScript</span><span class="sxs-lookup"><span data-stu-id="3d686-607">Call the web API with JavaScript</span></span>

<span data-ttu-id="3d686-608">В этом разделе описано, как добавить HTML-страницу, которая использует JavaScript для вызова веб-API.</span><span class="sxs-lookup"><span data-stu-id="3d686-608">In this section, an HTML page is added that uses JavaScript to call the web API.</span></span> <span data-ttu-id="3d686-609">Fetch API инициирует запрос.</span><span class="sxs-lookup"><span data-stu-id="3d686-609">The Fetch API initiates the request.</span></span> <span data-ttu-id="3d686-610">JavaScript изменяет страницу, используя сведения из ответа API.</span><span class="sxs-lookup"><span data-stu-id="3d686-610">JavaScript updates the page with the details from the web API's response.</span></span>

<span data-ttu-id="3d686-611">Настройте приложение для [обслуживания статических файлов](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) и [включения сопоставления файлов по умолчанию](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_), обновив *Startup.cs* следующим выделенным кодом:</span><span class="sxs-lookup"><span data-stu-id="3d686-611">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="3d686-612">Создайте папку *wwwroot* в каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="3d686-612">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="3d686-613">Добавьте HTML-файл *index.html* в каталог *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="3d686-613">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="3d686-614">Замените его содержимое следующей разметкой:</span><span class="sxs-lookup"><span data-stu-id="3d686-614">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="3d686-615">Добавьте файл JavaScript с именем *site.js* в каталог *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="3d686-615">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="3d686-616">Замените его содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="3d686-616">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="3d686-617">Может потребоваться изменение параметров запуска проекта ASP.NET Core для локального тестирования HTML-страницы:</span><span class="sxs-lookup"><span data-stu-id="3d686-617">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="3d686-618">Откройте файл *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="3d686-618">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="3d686-619">Удалите свойство `launchUrl`, чтобы приложение открылось через *index.html* &mdash; файл проекта по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3d686-619">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="3d686-620">В этом примере вызываются все методы CRUD в веб-API.</span><span class="sxs-lookup"><span data-stu-id="3d686-620">This sample calls all of the CRUD methods of the web API.</span></span> <span data-ttu-id="3d686-621">Ниже приводится пояснение вызовов API.</span><span class="sxs-lookup"><span data-stu-id="3d686-621">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="3d686-622">Получение списка элементов задач</span><span class="sxs-lookup"><span data-stu-id="3d686-622">Get a list of to-do items</span></span>

<span data-ttu-id="3d686-623">Fetch отправляет запрос HTTP GET к веб-API, который возвращает ответ JSON, представляющий массив элементов списка дел.</span><span class="sxs-lookup"><span data-stu-id="3d686-623">Fetch sends an HTTP GET request to the web API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="3d686-624">В случае успешного запроса используется функция обратного вызова `success`.</span><span class="sxs-lookup"><span data-stu-id="3d686-624">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="3d686-625">При обратном вызове в модель DOM вносятся данные о задачах.</span><span class="sxs-lookup"><span data-stu-id="3d686-625">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="3d686-626">Добавление элемента задачи</span><span class="sxs-lookup"><span data-stu-id="3d686-626">Add a to-do item</span></span>

<span data-ttu-id="3d686-627">Fetch отправляет запрос HTTP POST с элементом списка дел в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="3d686-627">Fetch sends an HTTP POST request with the to-do item in the request body.</span></span> <span data-ttu-id="3d686-628">Для параметров `accepts` и `contentType` указывается `application/json`, чтобы указать тип носителя при получении и отправке.</span><span class="sxs-lookup"><span data-stu-id="3d686-628">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="3d686-629">Элемент списка дел преобразуется в JSON с помощью [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="3d686-629">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="3d686-630">Если интерфейс API возвращает код состояния успешного выполнения, вызывается функция `getData` для обновления HTML-таблицы.</span><span class="sxs-lookup"><span data-stu-id="3d686-630">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="3d686-631">Обновление элемента задачи</span><span class="sxs-lookup"><span data-stu-id="3d686-631">Update a to-do item</span></span>

<span data-ttu-id="3d686-632">Обновление элемента списка дел выполняется аналогично его добавлению.</span><span class="sxs-lookup"><span data-stu-id="3d686-632">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="3d686-633">`url` изменяется, чтобы добавить уникальный идентификатор для элемента, а в качестве `type` устанавливается `PUT`.</span><span class="sxs-lookup"><span data-stu-id="3d686-633">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="3d686-634">Удаление элемента задачи</span><span class="sxs-lookup"><span data-stu-id="3d686-634">Delete a to-do item</span></span>

<span data-ttu-id="3d686-635">Чтобы удалить элемент списка дел, установите для `type` в вызове AJAX значение `DELETE` и укажите уникальный идентификатор элемента в URL-адресе.</span><span class="sxs-lookup"><span data-stu-id="3d686-635">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a><span data-ttu-id="3d686-636">Добавление поддержки аутентификации в веб-API</span><span class="sxs-lookup"><span data-stu-id="3d686-636">Add authentication support to a web API</span></span>

<span data-ttu-id="3d686-637">Обратитесь к учебнику по [IdentityServer4](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html).</span><span class="sxs-lookup"><span data-stu-id="3d686-637">See the [IdentityServer4](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html) tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3d686-638">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="3d686-638">Additional resources</span></span>

<span data-ttu-id="3d686-639">[Просмотреть или скачать пример кода для этого учебника](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="3d686-639">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="3d686-640">См. раздел [Практическое руководство. Скачивание файла](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="3d686-640">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="3d686-641">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="3d686-641">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="3d686-642">Версия руководства на YouTube</span><span class="sxs-lookup"><span data-stu-id="3d686-642">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
