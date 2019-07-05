---
title: Учебник. Создание веб-API с помощью ASP.NET Core
author: rick-anderson
description: Узнайте, как создать веб-API с помощью ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 06/23/2019
uid: tutorials/first-web-api
ms.openlocfilehash: a53f7019c1079296f073e743ddbf9d90fc5abad3
ms.sourcegitcommit: d6e51c60439f03a8992bda70cc982ddb15d3f100
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555879"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="46b55-103">Учебник. Создание веб-API с помощью ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="46b55-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="46b55-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Майк Уоссон](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="46b55-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="46b55-105">В этом учебнике приводятся основные сведения о создании веб-API с помощью ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="46b55-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

<span data-ttu-id="46b55-106">В этом руководстве вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="46b55-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="46b55-107">Создание проекта веб-API.</span><span class="sxs-lookup"><span data-stu-id="46b55-107">Create a web API project.</span></span>
> * <span data-ttu-id="46b55-108">Добавление класса модели.</span><span class="sxs-lookup"><span data-stu-id="46b55-108">Add a model class.</span></span>
> * <span data-ttu-id="46b55-109">Создание контекста базы данных.</span><span class="sxs-lookup"><span data-stu-id="46b55-109">Create the database context.</span></span>
> * <span data-ttu-id="46b55-110">Регистрация контекста базы данных.</span><span class="sxs-lookup"><span data-stu-id="46b55-110">Register the database context.</span></span>
> * <span data-ttu-id="46b55-111">Добавление контроллера.</span><span class="sxs-lookup"><span data-stu-id="46b55-111">Add a controller.</span></span>
> * <span data-ttu-id="46b55-112">Добавление методов CRUD.</span><span class="sxs-lookup"><span data-stu-id="46b55-112">Add CRUD methods.</span></span>
> * <span data-ttu-id="46b55-113">Настройка маршрутизации и путей URL.</span><span class="sxs-lookup"><span data-stu-id="46b55-113">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="46b55-114">Указание возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="46b55-114">Specify return values.</span></span>
> * <span data-ttu-id="46b55-115">Вызов веб-API с помощью Postman.</span><span class="sxs-lookup"><span data-stu-id="46b55-115">Call the web API with Postman.</span></span>
> * <span data-ttu-id="46b55-116">Вызов веб-API с помощью jQuery.</span><span class="sxs-lookup"><span data-stu-id="46b55-116">Call the web API with jQuery.</span></span>

<span data-ttu-id="46b55-117">В конечном итоге вы получите веб-API, обеспечивающий управление элементами списка дел, хранящимися в реляционной базе данных.</span><span class="sxs-lookup"><span data-stu-id="46b55-117">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="46b55-118">Обзор</span><span class="sxs-lookup"><span data-stu-id="46b55-118">Overview</span></span>

<span data-ttu-id="46b55-119">В этом руководстве создается следующий API-интерфейс:</span><span class="sxs-lookup"><span data-stu-id="46b55-119">This tutorial creates the following API:</span></span>

|<span data-ttu-id="46b55-120">API</span><span class="sxs-lookup"><span data-stu-id="46b55-120">API</span></span> | <span data-ttu-id="46b55-121">ОПИСАНИЕ</span><span class="sxs-lookup"><span data-stu-id="46b55-121">Description</span></span> | <span data-ttu-id="46b55-122">Текст запроса</span><span class="sxs-lookup"><span data-stu-id="46b55-122">Request body</span></span> | <span data-ttu-id="46b55-123">Текст ответа</span><span class="sxs-lookup"><span data-stu-id="46b55-123">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="46b55-124">GET/api/todo</span><span class="sxs-lookup"><span data-stu-id="46b55-124">GET /api/todo</span></span> | <span data-ttu-id="46b55-125">Получение всех элементов задач</span><span class="sxs-lookup"><span data-stu-id="46b55-125">Get all to-do items</span></span> | <span data-ttu-id="46b55-126">Нет</span><span class="sxs-lookup"><span data-stu-id="46b55-126">None</span></span> | <span data-ttu-id="46b55-127">Массив элементов задач</span><span class="sxs-lookup"><span data-stu-id="46b55-127">Array of to-do items</span></span>|
|<span data-ttu-id="46b55-128">GET/api/todo/{id}</span><span class="sxs-lookup"><span data-stu-id="46b55-128">GET /api/todo/{id}</span></span> | <span data-ttu-id="46b55-129">Получение объекта по идентификатору</span><span class="sxs-lookup"><span data-stu-id="46b55-129">Get an item by ID</span></span> | <span data-ttu-id="46b55-130">Нет</span><span class="sxs-lookup"><span data-stu-id="46b55-130">None</span></span> | <span data-ttu-id="46b55-131">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="46b55-131">To-do item</span></span>|
|<span data-ttu-id="46b55-132">POST/api/todo</span><span class="sxs-lookup"><span data-stu-id="46b55-132">POST /api/todo</span></span> | <span data-ttu-id="46b55-133">Добавление нового элемента</span><span class="sxs-lookup"><span data-stu-id="46b55-133">Add a new item</span></span> | <span data-ttu-id="46b55-134">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="46b55-134">To-do item</span></span> | <span data-ttu-id="46b55-135">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="46b55-135">To-do item</span></span> |
|<span data-ttu-id="46b55-136">PUT/api/todo/{id}</span><span class="sxs-lookup"><span data-stu-id="46b55-136">PUT /api/todo/{id}</span></span> | <span data-ttu-id="46b55-137">Обновление существующего элемента &nbsp;</span><span class="sxs-lookup"><span data-stu-id="46b55-137">Update an existing item &nbsp;</span></span> | <span data-ttu-id="46b55-138">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="46b55-138">To-do item</span></span> | <span data-ttu-id="46b55-139">Нет</span><span class="sxs-lookup"><span data-stu-id="46b55-139">None</span></span> |
|<span data-ttu-id="46b55-140">DELETE/api/todo/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="46b55-140">DELETE /api/todo/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="46b55-141">Удаление элемента &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="46b55-141">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="46b55-142">Нет</span><span class="sxs-lookup"><span data-stu-id="46b55-142">None</span></span> | <span data-ttu-id="46b55-143">Нет</span><span class="sxs-lookup"><span data-stu-id="46b55-143">None</span></span>|

<span data-ttu-id="46b55-144">На следующем рисунке показана структура приложения.</span><span class="sxs-lookup"><span data-stu-id="46b55-144">The following diagram shows the design of the app.</span></span>

![Клиент представлен прямоугольником слева.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="46b55-150">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="46b55-150">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="46b55-151">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="46b55-151">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="46b55-152">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="46b55-152">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="46b55-153">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="46b55-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="46b55-154">Создайте веб-проект.</span><span class="sxs-lookup"><span data-stu-id="46b55-154">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="46b55-155">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="46b55-155">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="46b55-156">В меню **Файл** выберите пункт **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="46b55-156">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="46b55-157">Выберите шаблон **Веб-приложение ASP.NET Core** и нажмите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="46b55-157">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="46b55-158">Назовите проект *TodoApi* и нажмите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="46b55-158">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="46b55-159">В диалоговом окне **Создание веб-приложения ASP.NET Core** убедитесь в том, что выбраны платформы **.NET Core** и **ASP.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="46b55-159">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="46b55-160">Выберите шаблон **API** и нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="46b55-160">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="46b55-161">**Не** выбирайте **Включение поддержки Docker**.</span><span class="sxs-lookup"><span data-stu-id="46b55-161">**Don't** select **Enable Docker Support**.</span></span>

![Диалоговое окно создания проекта VS](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="46b55-163">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="46b55-163">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="46b55-164">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="46b55-164">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="46b55-165">Смените каталог (`cd`) на папку, в которой будет содержаться папка проекта.</span><span class="sxs-lookup"><span data-stu-id="46b55-165">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="46b55-166">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="46b55-166">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="46b55-167">С помощью этих команд создается новый проект веб-API и открывается новый экземпляр Visual Studio Code в новой папке проекта.</span><span class="sxs-lookup"><span data-stu-id="46b55-167">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="46b55-168">При появлении диалогового окна с запросом на добавление в проект необходимых ресурсов выберите **Да**.</span><span class="sxs-lookup"><span data-stu-id="46b55-168">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="46b55-169">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="46b55-169">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="46b55-170">Выберите **Файл** > **Новое решение**.</span><span class="sxs-lookup"><span data-stu-id="46b55-170">Select **File** > **New Solution**.</span></span>

  ![Новое решение macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="46b55-172">Выберите **Приложение .NET Core** > **Веб-API ASP.NET Core** > **Далее**.</span><span class="sxs-lookup"><span data-stu-id="46b55-172">Select **.NET Core App** > **ASP.NET Core Web API** > **Next**.</span></span>

  ![Диалоговое окно "Новый проект" в macOS](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="46b55-174">В диалоговом окне **Настройка нового веб-API ASP.NET Core** оставьте установленное по умолчанию значение для параметра **Целевая платформа**, то есть \* *.NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="46b55-174">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="46b55-175">Введите *TodoApi* в поле **Имя проекта** и выберите команду **Создать**.</span><span class="sxs-lookup"><span data-stu-id="46b55-175">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Диалоговое окно конфигурации](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="46b55-177">Тестирование API</span><span class="sxs-lookup"><span data-stu-id="46b55-177">Test the API</span></span>

<span data-ttu-id="46b55-178">Шаблон проекта создает API `values`.</span><span class="sxs-lookup"><span data-stu-id="46b55-178">The project template creates a `values` API.</span></span> <span data-ttu-id="46b55-179">Вызовите метод `Get` из браузера для тестирования приложения.</span><span class="sxs-lookup"><span data-stu-id="46b55-179">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="46b55-180">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="46b55-180">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="46b55-181">Нажмите клавиши CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="46b55-181">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="46b55-182">Visual Studio запустит браузер и перейдет к `https://localhost:<port>/api/values`, где `<port>` — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="46b55-182">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="46b55-183">Если появится диалоговое окно с запросом о необходимости доверять сертификату IIS Express, выберите **Да**.</span><span class="sxs-lookup"><span data-stu-id="46b55-183">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="46b55-184">В появляющемся следом диалоговом окне **Предупреждение системы безопасности** выберите **Да**.</span><span class="sxs-lookup"><span data-stu-id="46b55-184">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="46b55-185">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="46b55-185">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="46b55-186">Нажмите клавиши CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="46b55-186">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="46b55-187">В браузере перейдите по следующему URL-адресу: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="46b55-187">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="46b55-188">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="46b55-188">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="46b55-189">Выберите **Выполнить** > **Запуск без отладки**, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="46b55-189">Select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="46b55-190">Visual Studio для Mac запустит браузер и перейдет к `https://localhost:<port>`, где `<port>` — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="46b55-190">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="46b55-191">Появится ошибка HTTP 404 (Не найдено).</span><span class="sxs-lookup"><span data-stu-id="46b55-191">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="46b55-192">Добавьте `/api/values` к URL-адресу (измените URL-адрес на `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="46b55-192">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="46b55-193">Возвращается следующий файл JSON:</span><span class="sxs-lookup"><span data-stu-id="46b55-193">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="46b55-194">Добавление класса модели</span><span class="sxs-lookup"><span data-stu-id="46b55-194">Add a model class</span></span>

<span data-ttu-id="46b55-195">*Модель* — это набор классов, представляющих данные, которыми управляет приложение.</span><span class="sxs-lookup"><span data-stu-id="46b55-195">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="46b55-196">Модель этого приложения содержит единственный класс `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="46b55-196">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="46b55-197">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="46b55-197">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="46b55-198">В **обозревателе решений** щелкните проект правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="46b55-198">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="46b55-199">Выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="46b55-199">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="46b55-200">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="46b55-200">Name the folder *Models*.</span></span>

* <span data-ttu-id="46b55-201">Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="46b55-201">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="46b55-202">Присвойте классу имя *TodoItem* и выберите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="46b55-202">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="46b55-203">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="46b55-203">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="46b55-204">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="46b55-204">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="46b55-205">Добавьте папку с именем *Models*.</span><span class="sxs-lookup"><span data-stu-id="46b55-205">Add a folder named *Models*.</span></span>

* <span data-ttu-id="46b55-206">Добавьте класс `TodoItem` в папку *Models*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="46b55-206">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="46b55-207">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="46b55-207">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="46b55-208">Щелкните проект правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="46b55-208">Right-click the project.</span></span> <span data-ttu-id="46b55-209">Выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="46b55-209">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="46b55-210">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="46b55-210">Name the folder *Models*.</span></span>

  ![Новая папка](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="46b55-212">Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Новый файл** > **Общие** > **Пустой класс**.</span><span class="sxs-lookup"><span data-stu-id="46b55-212">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="46b55-213">Назовите класс *TodoItem* и нажмите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="46b55-213">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="46b55-214">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="46b55-214">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="46b55-215">Свойство `Id` выступает в качестве уникального ключа реляционной базы данных.</span><span class="sxs-lookup"><span data-stu-id="46b55-215">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="46b55-216">Классы моделей можно размещать в любом месте проекта, но обычно для этого используется папка *Models*.</span><span class="sxs-lookup"><span data-stu-id="46b55-216">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="46b55-217">Добавление контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="46b55-217">Add a database context</span></span>

<span data-ttu-id="46b55-218">*Контекст базы данных* —это основной класс, который координирует функциональные возможности Entity Framework для модели данных.</span><span class="sxs-lookup"><span data-stu-id="46b55-218">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="46b55-219">Этот класс является производным от класса `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="46b55-219">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="46b55-220">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="46b55-220">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="46b55-221">Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="46b55-221">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="46b55-222">Назовите класс *TodoContext* и нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="46b55-222">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="46b55-223">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="46b55-223">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="46b55-224">Добавьте класс `TodoContext` в папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="46b55-224">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="46b55-225">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="46b55-225">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="46b55-226">Регистрация контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="46b55-226">Register the database context</span></span>

<span data-ttu-id="46b55-227">В ASP.NET Core службы (такие как контекст базы данных) должны быть зарегистрированы с помощью контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="46b55-227">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="46b55-228">Контейнер предоставляет службу контроллерам.</span><span class="sxs-lookup"><span data-stu-id="46b55-228">The container provides the service to controllers.</span></span>

<span data-ttu-id="46b55-229">Обновите файл *Startup.cs*, используя следующий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="46b55-229">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="46b55-230">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="46b55-230">The preceding code:</span></span>

* <span data-ttu-id="46b55-231">Удалите неиспользуемые объявления `using`.</span><span class="sxs-lookup"><span data-stu-id="46b55-231">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="46b55-232">Добавляет контекст базы данных в контейнер внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="46b55-232">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="46b55-233">Указывает, что контекст базы данных будет использовать базу данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="46b55-233">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="46b55-234">Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="46b55-234">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="46b55-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="46b55-235">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="46b55-236">Щелкните папку *Controllers* правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="46b55-236">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="46b55-237">Выберите **Добавить** > **Новый объект**.</span><span class="sxs-lookup"><span data-stu-id="46b55-237">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="46b55-238">В диалоговом окне **Добавить новый элемент** выберите шаблон **Класс контроллера API**.</span><span class="sxs-lookup"><span data-stu-id="46b55-238">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="46b55-239">Присвойте классу имя *TodoController* и выберите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="46b55-239">Name the class *TodoController*, and select **Add**.</span></span>

  ![Диалоговое окно добавления элемента с контроллером в поле поиска и выбранным контроллером веб-API](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="46b55-241">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="46b55-241">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="46b55-242">В папке *Controllers* создайте класс с именем `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="46b55-242">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="46b55-243">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="46b55-243">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="46b55-244">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="46b55-244">The preceding code:</span></span>

* <span data-ttu-id="46b55-245">Определяет класс контроллера API без методов.</span><span class="sxs-lookup"><span data-stu-id="46b55-245">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="46b55-246">Добавляет в класс атрибут [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="46b55-246">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="46b55-247">Этот атрибут указывает, что контроллер отвечает на запросы веб-API.</span><span class="sxs-lookup"><span data-stu-id="46b55-247">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="46b55-248">Дополнительные сведения о поведении, которое реализует этот атрибут, см. в <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="46b55-248">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="46b55-249">Использует внедрение зависимостей для внедрения контекста базы данных (`TodoContext`) в контроллер.</span><span class="sxs-lookup"><span data-stu-id="46b55-249">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="46b55-250">Контекст базы данных используется в каждом методе [создания, чтения, обновления и удаления](https://wikipedia.org/wiki/Create,_read,_update_and_delete) в контроллере.</span><span class="sxs-lookup"><span data-stu-id="46b55-250">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="46b55-251">Добавляет элемент `Item1` в базу данных, если она пуста.</span><span class="sxs-lookup"><span data-stu-id="46b55-251">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="46b55-252">Этот код находится в конструкторе и выполняется каждый раз при обнаружении нового HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="46b55-252">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="46b55-253">Если вы удалите все элементы, конструктор создаст `Item1` при следующем вызове метода API.</span><span class="sxs-lookup"><span data-stu-id="46b55-253">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="46b55-254">Поэтому может создаться впечатление, что удаление не было выполнено, хотя это не так.</span><span class="sxs-lookup"><span data-stu-id="46b55-254">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="46b55-255">Добавление методов Get</span><span class="sxs-lookup"><span data-stu-id="46b55-255">Add Get methods</span></span>

<span data-ttu-id="46b55-256">Чтобы получить API, который извлекает элементы списка дел, добавьте следующие методы в класс `TodoController`:</span><span class="sxs-lookup"><span data-stu-id="46b55-256">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="46b55-257">Эти методы реализуют две конечные точки GET:</span><span class="sxs-lookup"><span data-stu-id="46b55-257">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="46b55-258">Протестируйте приложение, вызвав эти две конечные точки в браузере.</span><span class="sxs-lookup"><span data-stu-id="46b55-258">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="46b55-259">Например:</span><span class="sxs-lookup"><span data-stu-id="46b55-259">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="46b55-260">При вызове `GetTodoItems` возвращается следующий ответ HTTP:</span><span class="sxs-lookup"><span data-stu-id="46b55-260">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="46b55-261">Маршрутизация и пути URL</span><span class="sxs-lookup"><span data-stu-id="46b55-261">Routing and URL paths</span></span>

<span data-ttu-id="46b55-262">Атрибут [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) обозначает метод, который отвечает на запрос HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="46b55-262">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="46b55-263">Путь URL для каждого метода формируется следующим образом:</span><span class="sxs-lookup"><span data-stu-id="46b55-263">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="46b55-264">Возьмите строку шаблона в атрибуте `Route` контроллера:</span><span class="sxs-lookup"><span data-stu-id="46b55-264">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="46b55-265">Замените `[controller]` именем контроллера (по соглашению это имя класса контроллера без суффикса "Controller").</span><span class="sxs-lookup"><span data-stu-id="46b55-265">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="46b55-266">В этом примере класс контроллера носит имя **Todo**, а сам контроллер, соответственно, — "todo".</span><span class="sxs-lookup"><span data-stu-id="46b55-266">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="46b55-267">В ASP.NET Core [маршрутизация](xref:mvc/controllers/routing) реализуется без учета регистра символов.</span><span class="sxs-lookup"><span data-stu-id="46b55-267">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="46b55-268">Если атрибут `[HttpGet]` имеет шаблон маршрута (например, `[HttpGet("products")]`), добавьте его к пути.</span><span class="sxs-lookup"><span data-stu-id="46b55-268">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="46b55-269">В этом примере шаблон не используется.</span><span class="sxs-lookup"><span data-stu-id="46b55-269">This sample doesn't use a template.</span></span> <span data-ttu-id="46b55-270">Дополнительные сведения см. в разделе [Маршрутизация атрибутов с помощью атрибутов Http[Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="46b55-270">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="46b55-271">В следующем методе `GetTodoItem` `"{id}"` — это переменная-заполнитель для уникального идентификатора элемента задачи.</span><span class="sxs-lookup"><span data-stu-id="46b55-271">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="46b55-272">При вызове `GetTodoItem` параметру метода `id` присваивается значение `"{id}"` в URL-адресе.</span><span class="sxs-lookup"><span data-stu-id="46b55-272">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="46b55-273">Возвращаемые значения</span><span class="sxs-lookup"><span data-stu-id="46b55-273">Return values</span></span>

<span data-ttu-id="46b55-274">Возвращаемое значение имеет тип `GetTodoItems`, а метод `GetTodoItem` имеет тип [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="46b55-274">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="46b55-275">ASP.NET Core автоматически сериализует объект в формат [JSON](https://www.json.org/) и записывает данные JSON в тело сообщения ответа.</span><span class="sxs-lookup"><span data-stu-id="46b55-275">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="46b55-276">Код ответа для этого типа возвращаемого значения равен 200, что свидетельствует об отсутствии необработанных исключений.</span><span class="sxs-lookup"><span data-stu-id="46b55-276">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="46b55-277">Необработанные исключения преобразуются в ошибки 5xx.</span><span class="sxs-lookup"><span data-stu-id="46b55-277">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="46b55-278">Типы возвращаемых значений `ActionResult` могут представлять широкий спектр кодов состояний HTTP.</span><span class="sxs-lookup"><span data-stu-id="46b55-278">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="46b55-279">Например, метод `GetTodoItem` может возвращать два разных значения состояния:</span><span class="sxs-lookup"><span data-stu-id="46b55-279">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="46b55-280">Если запрошенному идентификатору не соответствует ни один элемент, метод возвращает ошибку 404 ([Не найдено](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound)).</span><span class="sxs-lookup"><span data-stu-id="46b55-280">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="46b55-281">В противном случае метод возвращает код 200 с телом ответа JSON.</span><span class="sxs-lookup"><span data-stu-id="46b55-281">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="46b55-282">При возвращении `item` возвращается ответ HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="46b55-282">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="46b55-283">Тестирование метода GetTodoItems</span><span class="sxs-lookup"><span data-stu-id="46b55-283">Test the GetTodoItems method</span></span>

<span data-ttu-id="46b55-284">В этом учебнике для тестирования веб-API используется Postman.</span><span class="sxs-lookup"><span data-stu-id="46b55-284">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="46b55-285">Установка [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="46b55-285">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="46b55-286">Запустите веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="46b55-286">Start the web app.</span></span>
* <span data-ttu-id="46b55-287">Запустите Postman.</span><span class="sxs-lookup"><span data-stu-id="46b55-287">Start Postman.</span></span>
* <span data-ttu-id="46b55-288">Отключение параметра **Проверка SSL-сертификата**</span><span class="sxs-lookup"><span data-stu-id="46b55-288">Disable **SSL certificate verification**</span></span>
  
  * <span data-ttu-id="46b55-289">В меню **Файл > Параметры** (вкладка \**Общие*) отключите параметр **Проверка SSL-сертификата**.</span><span class="sxs-lookup"><span data-stu-id="46b55-289">From  **File > Settings** (\**General* tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="46b55-290">После тестирования контроллера снова включите проверку SSL-сертификата.</span><span class="sxs-lookup"><span data-stu-id="46b55-290">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="46b55-291">Создайте новый запрос.</span><span class="sxs-lookup"><span data-stu-id="46b55-291">Create a new request.</span></span>
  * <span data-ttu-id="46b55-292">Укажите метод HTTP **GET**.</span><span class="sxs-lookup"><span data-stu-id="46b55-292">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="46b55-293">Укажите URL-адрес запроса `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="46b55-293">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="46b55-294">Например, `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="46b55-294">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="46b55-295">Выберите режим **Представление с двумя областями** в Postman.</span><span class="sxs-lookup"><span data-stu-id="46b55-295">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="46b55-296">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="46b55-296">Select **Send**.</span></span>

![Postman с запросом Get](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="46b55-298">Добавление метода Create</span><span class="sxs-lookup"><span data-stu-id="46b55-298">Add a Create method</span></span>

<span data-ttu-id="46b55-299">Добавьте приведенный ниже метод `PostTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="46b55-299">Add the following `PostTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="46b55-300">Предыдущий код является методом HTTP POST, как указывает атрибут [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="46b55-300">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="46b55-301">Этот метод получает значение элемента списка дел из текста HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="46b55-301">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="46b55-302">Метод `CreatedAtAction`:</span><span class="sxs-lookup"><span data-stu-id="46b55-302">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="46b55-303">В случае успеха возвращает код состояния HTTP 201.</span><span class="sxs-lookup"><span data-stu-id="46b55-303">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="46b55-304">HTTP 201 представляет собой стандартный ответ для метода HTTP POST, создающий ресурс на сервере.</span><span class="sxs-lookup"><span data-stu-id="46b55-304">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="46b55-305">Добавляет заголовок `Location` в ответ.</span><span class="sxs-lookup"><span data-stu-id="46b55-305">Adds a `Location` header to the response.</span></span> <span data-ttu-id="46b55-306">Заголовок `Location` расположения указывает URI вновь созданной задачи.</span><span class="sxs-lookup"><span data-stu-id="46b55-306">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="46b55-307">Дополнительные сведения см. в статье [10.2.2 201 "Создан ресурс"](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="46b55-307">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="46b55-308">Указывает действие `GetTodoItem` для создания URI заголовка `Location`.</span><span class="sxs-lookup"><span data-stu-id="46b55-308">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="46b55-309">Ключевое слово `nameof` C# используется для предотвращения жесткого программирования имени действия в вызове `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="46b55-309">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="46b55-310">Тестирование метода PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="46b55-310">Test the PostTodoItem method</span></span>

* <span data-ttu-id="46b55-311">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="46b55-311">Build the project.</span></span>
* <span data-ttu-id="46b55-312">В Postman укажите метод HTTP `POST`.</span><span class="sxs-lookup"><span data-stu-id="46b55-312">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="46b55-313">Откройте вкладку **Тело**.</span><span class="sxs-lookup"><span data-stu-id="46b55-313">Select the **Body** tab.</span></span>
* <span data-ttu-id="46b55-314">Установите переключатель **без обработки**.</span><span class="sxs-lookup"><span data-stu-id="46b55-314">Select the **raw** radio button.</span></span>
* <span data-ttu-id="46b55-315">Задайте тип **JSON (приложение/json)** .</span><span class="sxs-lookup"><span data-stu-id="46b55-315">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="46b55-316">В теле запроса введите код JSON для элемента списка дел:</span><span class="sxs-lookup"><span data-stu-id="46b55-316">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="46b55-317">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="46b55-317">Select **Send**.</span></span>

  ![Postman с запросом Create](first-web-api/_static/create.png)

  <span data-ttu-id="46b55-319">Если вы получаете ошибку 405 "Недопустимый метод", это может свидетельствовать о том, что после добавления метода `PostTodoItem` не была выполнена компиляция проекта.</span><span class="sxs-lookup"><span data-stu-id="46b55-319">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="46b55-320">Тестирование URI заголовка расположения</span><span class="sxs-lookup"><span data-stu-id="46b55-320">Test the location header URI</span></span>

* <span data-ttu-id="46b55-321">Перейдите на вкладку **Заголовки** в области **Ответ**.</span><span class="sxs-lookup"><span data-stu-id="46b55-321">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="46b55-322">Скопируйте значение заголовка **Расположение**:</span><span class="sxs-lookup"><span data-stu-id="46b55-322">Copy the **Location** header value:</span></span>

  ![Вкладка "Заголовки" в консоли Postman](first-web-api/_static/pmc2.png)

* <span data-ttu-id="46b55-324">Укажите метод GET.</span><span class="sxs-lookup"><span data-stu-id="46b55-324">Set the method to GET.</span></span>
* <span data-ttu-id="46b55-325">Вставьте URI (например, `https://localhost:5001/api/Todo/2`)</span><span class="sxs-lookup"><span data-stu-id="46b55-325">Paste the URI (for example, `https://localhost:5001/api/Todo/2`)</span></span>
* <span data-ttu-id="46b55-326">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="46b55-326">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="46b55-327">Добавление метода PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="46b55-327">Add a PutTodoItem method</span></span>

<span data-ttu-id="46b55-328">Добавьте приведенный ниже метод `PutTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="46b55-328">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="46b55-329">Страница `PutTodoItem` аналогична странице `PostTodoItem`, но использует запрос HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="46b55-329">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="46b55-330">Ответ — [204 (Нет содержимого)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="46b55-330">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="46b55-331">Согласно спецификации HTTP, запрос PUT требует, чтобы клиент отправлял всю обновленную сущность, а не только изменения.</span><span class="sxs-lookup"><span data-stu-id="46b55-331">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="46b55-332">Чтобы обеспечить поддержку частичных обновлений, используйте [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="46b55-332">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="46b55-333">Если возникнет ошибка вызова `PutTodoItem`, вызовите `GET`, чтобы в базе данных был один элемент.</span><span class="sxs-lookup"><span data-stu-id="46b55-333">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="46b55-334">Тестирование метода PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="46b55-334">Test the PutTodoItem method</span></span>

<span data-ttu-id="46b55-335">Этот пример использует базу данных в памяти, которая должна быть инициирована при каждом запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="46b55-335">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="46b55-336">При выполнении вызова PUT в базе данных уже должен существовать какой-либо элемент.</span><span class="sxs-lookup"><span data-stu-id="46b55-336">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="46b55-337">Для этого перед вызовом PUT выполните вызов GET, чтобы убедиться в наличии такого элемента в базе данных.</span><span class="sxs-lookup"><span data-stu-id="46b55-337">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="46b55-338">Обновите элемент списка дел с идентификатором = 1 и присвойте ему имя "feed fish":</span><span class="sxs-lookup"><span data-stu-id="46b55-338">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="46b55-339">На следующем рисунке показан процесс обновления Postman:</span><span class="sxs-lookup"><span data-stu-id="46b55-339">The following image shows the Postman update:</span></span>

![Консоль Postman с ответом 204 (Нет содержимого)](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="46b55-341">Добавление метода DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="46b55-341">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="46b55-342">Добавьте приведенный ниже метод `DeleteTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="46b55-342">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="46b55-343">`DeleteTodoItem`Ответ[ — 204 (нет содержимого)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="46b55-343">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="46b55-344">Тестирование метода DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="46b55-344">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="46b55-345">Удалите элемент списка дел с помощью Postman:</span><span class="sxs-lookup"><span data-stu-id="46b55-345">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="46b55-346">Укажите метод `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="46b55-346">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="46b55-347">Укажите URI удаляемого объекта, например `https://localhost:5001/api/todo/1`</span><span class="sxs-lookup"><span data-stu-id="46b55-347">Set the URI of the object to delete, for example `https://localhost:5001/api/todo/1`</span></span>
* <span data-ttu-id="46b55-348">Нажмите кнопку **Отправить**</span><span class="sxs-lookup"><span data-stu-id="46b55-348">Select **Send**</span></span>

<span data-ttu-id="46b55-349">В этом примере приложения вы можете удалить все элементы.</span><span class="sxs-lookup"><span data-stu-id="46b55-349">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="46b55-350">Однако в случае удаления последнего элемента в момент следующего вызова API конструктор класса модели создаст новый элемент.</span><span class="sxs-lookup"><span data-stu-id="46b55-350">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-api-with-jquery"></a><span data-ttu-id="46b55-351">Вызов API с помощью jQuery</span><span class="sxs-lookup"><span data-stu-id="46b55-351">Call the API with jQuery</span></span>

<span data-ttu-id="46b55-352">В этом разделе добавляется HTML-страница, которая использует jQuery для вызова веб-API.</span><span class="sxs-lookup"><span data-stu-id="46b55-352">In this section, an HTML page is added that uses jQuery to call the web api.</span></span> <span data-ttu-id="46b55-353">jQuery запускает запрос и вносит на страницу подробные сведения из ответа API.</span><span class="sxs-lookup"><span data-stu-id="46b55-353">jQuery initiates the request and updates the page with the details from the API's response.</span></span>

<span data-ttu-id="46b55-354">Настройте приложение для [обслуживания статических файлов](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) и [включения сопоставления файлов по умолчанию](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_), обновив *Startup.cs* следующим выделенным кодом:</span><span class="sxs-lookup"><span data-stu-id="46b55-354">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

::: moniker range=">= aspnetcore-2.2"
<span data-ttu-id="46b55-355">Создайте папку *wwwroot* в каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="46b55-355">Create a *wwwroot* folder in the project directory.</span></span>
::: moniker-end

<span data-ttu-id="46b55-356">Добавьте HTML-файл *index.html* в каталог *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="46b55-356">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="46b55-357">Замените его содержимое следующей разметкой:</span><span class="sxs-lookup"><span data-stu-id="46b55-357">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="46b55-358">Добавьте файл JavaScript с именем *site.js* в каталог *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="46b55-358">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="46b55-359">Замените его содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="46b55-359">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="46b55-360">Может потребоваться изменение параметров запуска проекта ASP.NET Core для локального тестирования HTML-страницы:</span><span class="sxs-lookup"><span data-stu-id="46b55-360">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="46b55-361">Откройте файл *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="46b55-361">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="46b55-362">Удалите свойство `launchUrl`, чтобы приложение открылось через *index.html* &mdash; файл проекта по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="46b55-362">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="46b55-363">Для получения jQuery можно использовать следующие способы.</span><span class="sxs-lookup"><span data-stu-id="46b55-363">There are several ways to get jQuery.</span></span> <span data-ttu-id="46b55-364">В предыдущем фрагменте кода библиотека загружается из CDN.</span><span class="sxs-lookup"><span data-stu-id="46b55-364">In the preceding snippet, the library is loaded from a CDN.</span></span>

<span data-ttu-id="46b55-365">В этом примере вызываются все методы CRUD в API.</span><span class="sxs-lookup"><span data-stu-id="46b55-365">This sample calls all of the CRUD methods of the API.</span></span> <span data-ttu-id="46b55-366">Ниже приводится пояснение вызовов API.</span><span class="sxs-lookup"><span data-stu-id="46b55-366">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="46b55-367">Получение списка элементов задач</span><span class="sxs-lookup"><span data-stu-id="46b55-367">Get a list of to-do items</span></span>

<span data-ttu-id="46b55-368">Функция jQuery [ajax](https://api.jquery.com/jquery.ajax/) отправляет запрос `GET` к API, который возвращает JSON, представляющий массив элементов списка дел.</span><span class="sxs-lookup"><span data-stu-id="46b55-368">The jQuery [ajax](https://api.jquery.com/jquery.ajax/) function sends a `GET` request to the API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="46b55-369">В случае успешного запроса используется функция обратного вызова `success`.</span><span class="sxs-lookup"><span data-stu-id="46b55-369">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="46b55-370">При обратном вызове в модель DOM вносятся данные о задачах.</span><span class="sxs-lookup"><span data-stu-id="46b55-370">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="46b55-371">Добавление элемента задачи</span><span class="sxs-lookup"><span data-stu-id="46b55-371">Add a to-do item</span></span>

<span data-ttu-id="46b55-372">Функция [ajax](https://api.jquery.com/jquery.ajax/) отправляет запрос `POST` с элементом списка дел в теле запроса.</span><span class="sxs-lookup"><span data-stu-id="46b55-372">The [ajax](https://api.jquery.com/jquery.ajax/) function sends a `POST` request with the to-do item in the request body.</span></span> <span data-ttu-id="46b55-373">Для параметров `accepts` и `contentType` указывается `application/json`, чтобы указать тип носителя при получении и отправке.</span><span class="sxs-lookup"><span data-stu-id="46b55-373">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="46b55-374">Элемент списка дел преобразуется в JSON с помощью [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="46b55-374">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="46b55-375">Если интерфейс API возвращает код состояния успешного выполнения, вызывается функция `getData` для обновления HTML-таблицы.</span><span class="sxs-lookup"><span data-stu-id="46b55-375">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="46b55-376">Обновление элемента задачи</span><span class="sxs-lookup"><span data-stu-id="46b55-376">Update a to-do item</span></span>

<span data-ttu-id="46b55-377">Обновление элемента списка дел выполняется аналогично его добавлению.</span><span class="sxs-lookup"><span data-stu-id="46b55-377">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="46b55-378">`url` изменяется, чтобы добавить уникальный идентификатор для элемента, а в качестве `type` устанавливается `PUT`.</span><span class="sxs-lookup"><span data-stu-id="46b55-378">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="46b55-379">Удаление элемента задачи</span><span class="sxs-lookup"><span data-stu-id="46b55-379">Delete a to-do item</span></span>

<span data-ttu-id="46b55-380">Чтобы удалить элемент списка дел, установите для `type` в вызове AJAX значение `DELETE` и укажите уникальный идентификатор элемента в URL-адресе.</span><span class="sxs-lookup"><span data-stu-id="46b55-380">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

## <a name="additional-resources"></a><span data-ttu-id="46b55-381">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="46b55-381">Additional resources</span></span>

<span data-ttu-id="46b55-382">[Просмотреть или скачать пример кода для этого учебника](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="46b55-382">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="46b55-383">См. раздел [Практическое руководство. Скачивание файла](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="46b55-383">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="46b55-384">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="46b55-384">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/index>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="46b55-385">Версия руководства на YouTube</span><span class="sxs-lookup"><span data-stu-id="46b55-385">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)

## <a name="next-steps"></a><span data-ttu-id="46b55-386">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="46b55-386">Next steps</span></span>

<span data-ttu-id="46b55-387">В этом руководстве вы узнали, как:</span><span class="sxs-lookup"><span data-stu-id="46b55-387">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="46b55-388">Создание проекта веб-API.</span><span class="sxs-lookup"><span data-stu-id="46b55-388">Create a web api project.</span></span>
> * <span data-ttu-id="46b55-389">Добавление класса модели.</span><span class="sxs-lookup"><span data-stu-id="46b55-389">Add a model class.</span></span>
> * <span data-ttu-id="46b55-390">Создание контекста базы данных.</span><span class="sxs-lookup"><span data-stu-id="46b55-390">Create the database context.</span></span>
> * <span data-ttu-id="46b55-391">Регистрация контекста базы данных.</span><span class="sxs-lookup"><span data-stu-id="46b55-391">Register the database context.</span></span>
> * <span data-ttu-id="46b55-392">Добавление контроллера.</span><span class="sxs-lookup"><span data-stu-id="46b55-392">Add a controller.</span></span>
> * <span data-ttu-id="46b55-393">Добавление методов CRUD.</span><span class="sxs-lookup"><span data-stu-id="46b55-393">Add CRUD methods.</span></span>
> * <span data-ttu-id="46b55-394">Настройка маршрутизации и путей URL.</span><span class="sxs-lookup"><span data-stu-id="46b55-394">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="46b55-395">Указание возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="46b55-395">Specify return values.</span></span>
> * <span data-ttu-id="46b55-396">Вызов веб-API с помощью Postman.</span><span class="sxs-lookup"><span data-stu-id="46b55-396">Call the web API with Postman.</span></span>
> * <span data-ttu-id="46b55-397">Вызов веб-API с помощью jQuery.</span><span class="sxs-lookup"><span data-stu-id="46b55-397">Call the web api with jQuery.</span></span>

<span data-ttu-id="46b55-398">Перейдите к следующему учебнику, посвященному созданию страниц справки по API:</span><span class="sxs-lookup"><span data-stu-id="46b55-398">Advance to the next tutorial to learn how to generate API help pages:</span></span>

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>
