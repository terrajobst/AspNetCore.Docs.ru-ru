---
title: Учебник. Создание веб-API с помощью MVC ASP.NET Core
author: rick-anderson
description: Построение веб-API с помощью ASP.NET Core MVC
ms.author: riande
ms.custom: mvc
ms.date: 12/10/2018
uid: tutorials/first-web-api
ms.openlocfilehash: c2b4dcddd5332330cd6e6abe7d3a12697cde845e
ms.sourcegitcommit: 4e87712029de2aceb1cf2c52e9e3dda8195a5b8e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2018
ms.locfileid: "53382008"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core-mvc"></a><span data-ttu-id="6180e-103">Учебник. Создание веб-API с помощью MVC ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6180e-103">Tutorial: Create a web API with ASP.NET Core MVC</span></span>

<span data-ttu-id="6180e-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Майк Уоссон](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="6180e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="6180e-105">В этом учебнике приводятся основные сведения о создании веб-API с помощью ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6180e-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

<span data-ttu-id="6180e-106">В этом руководстве вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="6180e-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6180e-107">Создание проекта веб-API.</span><span class="sxs-lookup"><span data-stu-id="6180e-107">Create a web API project.</span></span>
> * <span data-ttu-id="6180e-108">Добавление класса модели.</span><span class="sxs-lookup"><span data-stu-id="6180e-108">Add a model class.</span></span>
> * <span data-ttu-id="6180e-109">Создание контекста базы данных.</span><span class="sxs-lookup"><span data-stu-id="6180e-109">Create the database context.</span></span>
> * <span data-ttu-id="6180e-110">Регистрация контекста базы данных.</span><span class="sxs-lookup"><span data-stu-id="6180e-110">Register the database context.</span></span>
> * <span data-ttu-id="6180e-111">Добавление контроллера.</span><span class="sxs-lookup"><span data-stu-id="6180e-111">Add a controller.</span></span>
> * <span data-ttu-id="6180e-112">Добавление методов CRUD.</span><span class="sxs-lookup"><span data-stu-id="6180e-112">Add CRUD methods.</span></span>
> * <span data-ttu-id="6180e-113">Настройка маршрутизации и путей URL.</span><span class="sxs-lookup"><span data-stu-id="6180e-113">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="6180e-114">Указание возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="6180e-114">Specify return values.</span></span>
> * <span data-ttu-id="6180e-115">Вызов веб-API с помощью Postman.</span><span class="sxs-lookup"><span data-stu-id="6180e-115">Call the web API with Postman.</span></span>
> * <span data-ttu-id="6180e-116">Вызов веб-API с помощью jQuery.</span><span class="sxs-lookup"><span data-stu-id="6180e-116">Call the web API with jQuery.</span></span>

<span data-ttu-id="6180e-117">В конечном итоге вы получите веб-API, обеспечивающий управление элементами списка дел, хранящимися в реляционной базе данных.</span><span class="sxs-lookup"><span data-stu-id="6180e-117">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="6180e-118">Обзор</span><span class="sxs-lookup"><span data-stu-id="6180e-118">Overview</span></span>

<span data-ttu-id="6180e-119">В этом руководстве создается следующий API-интерфейс:</span><span class="sxs-lookup"><span data-stu-id="6180e-119">This tutorial creates the following API:</span></span>

|<span data-ttu-id="6180e-120">API</span><span class="sxs-lookup"><span data-stu-id="6180e-120">API</span></span> | <span data-ttu-id="6180e-121">Описание</span><span class="sxs-lookup"><span data-stu-id="6180e-121">Description</span></span> | <span data-ttu-id="6180e-122">Текст запроса</span><span class="sxs-lookup"><span data-stu-id="6180e-122">Request body</span></span> | <span data-ttu-id="6180e-123">Текст ответа</span><span class="sxs-lookup"><span data-stu-id="6180e-123">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="6180e-124">GET/api/todo</span><span class="sxs-lookup"><span data-stu-id="6180e-124">GET /api/todo</span></span> | <span data-ttu-id="6180e-125">Получение всех элементов задач</span><span class="sxs-lookup"><span data-stu-id="6180e-125">Get all to-do items</span></span> | <span data-ttu-id="6180e-126">Нет</span><span class="sxs-lookup"><span data-stu-id="6180e-126">None</span></span> | <span data-ttu-id="6180e-127">Массив элементов задач</span><span class="sxs-lookup"><span data-stu-id="6180e-127">Array of to-do items</span></span>|
|<span data-ttu-id="6180e-128">GET/api/todo/{id}</span><span class="sxs-lookup"><span data-stu-id="6180e-128">GET /api/todo/{id}</span></span> | <span data-ttu-id="6180e-129">Получение объекта по идентификатору</span><span class="sxs-lookup"><span data-stu-id="6180e-129">Get an item by ID</span></span> | <span data-ttu-id="6180e-130">Нет</span><span class="sxs-lookup"><span data-stu-id="6180e-130">None</span></span> | <span data-ttu-id="6180e-131">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="6180e-131">To-do item</span></span>|
|<span data-ttu-id="6180e-132">POST/api/todo</span><span class="sxs-lookup"><span data-stu-id="6180e-132">POST /api/todo</span></span> | <span data-ttu-id="6180e-133">Добавление нового элемента</span><span class="sxs-lookup"><span data-stu-id="6180e-133">Add a new item</span></span> | <span data-ttu-id="6180e-134">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="6180e-134">To-do item</span></span> | <span data-ttu-id="6180e-135">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="6180e-135">To-do item</span></span> |
|<span data-ttu-id="6180e-136">PUT/api/todo/{id}</span><span class="sxs-lookup"><span data-stu-id="6180e-136">PUT /api/todo/{id}</span></span> | <span data-ttu-id="6180e-137">Обновление существующего элемента &nbsp;</span><span class="sxs-lookup"><span data-stu-id="6180e-137">Update an existing item &nbsp;</span></span> | <span data-ttu-id="6180e-138">Элемент задачи</span><span class="sxs-lookup"><span data-stu-id="6180e-138">To-do item</span></span> | <span data-ttu-id="6180e-139">Нет</span><span class="sxs-lookup"><span data-stu-id="6180e-139">None</span></span> |
|<span data-ttu-id="6180e-140">DELETE/api/todo/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="6180e-140">DELETE /api/todo/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="6180e-141">Удаление элемента &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="6180e-141">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="6180e-142">Нет</span><span class="sxs-lookup"><span data-stu-id="6180e-142">None</span></span> | <span data-ttu-id="6180e-143">Нет</span><span class="sxs-lookup"><span data-stu-id="6180e-143">None</span></span>|

<span data-ttu-id="6180e-144">На следующем рисунке показана структура приложения.</span><span class="sxs-lookup"><span data-stu-id="6180e-144">The following diagram shows the design of the app.</span></span>

![Клиент, представленный прямоугольником слева, отправляет запрос и получает ответ от приложения (прямоугольник справа).](first-web-api/_static/architecture.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-project"></a><span data-ttu-id="6180e-149">Создайте веб-проект.</span><span class="sxs-lookup"><span data-stu-id="6180e-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6180e-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6180e-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6180e-151">В меню **Файл** выберите пункт **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="6180e-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="6180e-152">Выберите шаблон **Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="6180e-152">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="6180e-153">Назовите проект *TodoApi* и нажмите **OK**.</span><span class="sxs-lookup"><span data-stu-id="6180e-153">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="6180e-154">В диалоговом окне **Новое веб-приложение ASP.NET Core — TodoApi** выберите версию ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6180e-154">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="6180e-155">Выберите шаблон **API** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="6180e-155">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="6180e-156">**Не** выбирайте **Включение поддержки Docker**.</span><span class="sxs-lookup"><span data-stu-id="6180e-156">Do **not** select **Enable Docker Support**.</span></span>

![Диалоговое окно создания проекта VS](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6180e-158">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6180e-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="6180e-159">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="6180e-159">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="6180e-160">Смените каталог (`cd`) на папку, в которой будет содержаться папка проекта.</span><span class="sxs-lookup"><span data-stu-id="6180e-160">Change directories (`cd`) to the folder which will contain the project folder.</span></span>
* <span data-ttu-id="6180e-161">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="6180e-161">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="6180e-162">С помощью этих команд создается новый проект веб-API и открывается новый экземпляр Visual Studio Code в новой папке проекта.</span><span class="sxs-lookup"><span data-stu-id="6180e-162">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="6180e-163">При появлении диалогового окна с запросом на добавление в проект необходимых ресурсов выберите **Да**.</span><span class="sxs-lookup"><span data-stu-id="6180e-163">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6180e-164">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="6180e-164">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6180e-165">Выберите **Файл** > **Новое решение**.</span><span class="sxs-lookup"><span data-stu-id="6180e-165">Select **File** > **New Solution**.</span></span>

  ![Новое решение macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="6180e-167">Выберите **Приложение .NET Core** > **Веб-API ASP.NET Core** > **Далее**.</span><span class="sxs-lookup"><span data-stu-id="6180e-167">Select **.NET Core App** > **ASP.NET Core Web API** > **Next**.</span></span>

  ![Диалоговое окно "Новый проект" в macOS](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="6180e-169">В диалоговом окне **Настройка нового веб-API ASP.NET Core** оставьте установленное по умолчанию значение для параметра **Целевая платформа**, то есть \**.NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="6180e-169">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="6180e-170">Введите *TodoApi* в поле **Имя проекта** и выберите команду **Создать**.</span><span class="sxs-lookup"><span data-stu-id="6180e-170">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Диалоговое окно конфигурации](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="6180e-172">Тестирование API</span><span class="sxs-lookup"><span data-stu-id="6180e-172">Test the API</span></span>

<span data-ttu-id="6180e-173">Шаблон проекта создает API `values`.</span><span class="sxs-lookup"><span data-stu-id="6180e-173">The project template creates a `values` API.</span></span> <span data-ttu-id="6180e-174">Вызовите метод `Get` из браузера для тестирования приложения.</span><span class="sxs-lookup"><span data-stu-id="6180e-174">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6180e-175">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6180e-175">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6180e-176">Нажмите клавиши CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="6180e-176">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="6180e-177">Visual Studio запустит браузер и перейдет к `https://localhost:<port>/api/values`, где `<port>` — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="6180e-177">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="6180e-178">Если появится диалоговое окно с запросом о необходимости доверять сертификату IIS Express, выберите **Да**.</span><span class="sxs-lookup"><span data-stu-id="6180e-178">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="6180e-179">В появляющемся следом диалоговом окне **Предупреждение системы безопасности** выберите **Да**.</span><span class="sxs-lookup"><span data-stu-id="6180e-179">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6180e-180">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6180e-180">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="6180e-181">Нажмите клавиши CTRL+F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="6180e-181">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="6180e-182">В браузере перейдите по следующему URL-адресу: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="6180e-182">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6180e-183">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="6180e-183">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="6180e-184">Выберите **Выполнить** > **Запуск без отладки**, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="6180e-184">Select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="6180e-185">Visual Studio для Mac запустит браузер и перейдет к `https://localhost:<port>`, где `<port>` — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="6180e-185">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="6180e-186">Появится ошибка HTTP 404 (Не найдено).</span><span class="sxs-lookup"><span data-stu-id="6180e-186">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="6180e-187">Добавьте `/api/values` к URL-адресу (измените URL-адрес на `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="6180e-187">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="6180e-188">Возвращается следующий файл JSON:</span><span class="sxs-lookup"><span data-stu-id="6180e-188">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="6180e-189">Добавление класса модели</span><span class="sxs-lookup"><span data-stu-id="6180e-189">Add a model class</span></span>

<span data-ttu-id="6180e-190">*Модель* — это набор классов, представляющих данные, которыми управляет приложение.</span><span class="sxs-lookup"><span data-stu-id="6180e-190">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="6180e-191">Модель этого приложения содержит единственный класс `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="6180e-191">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6180e-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6180e-192">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6180e-193">В **обозревателе решений** щелкните проект правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="6180e-193">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="6180e-194">Выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="6180e-194">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="6180e-195">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="6180e-195">Name the folder *Models*.</span></span>

* <span data-ttu-id="6180e-196">Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="6180e-196">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="6180e-197">Присвойте классу имя *TodoItem* и выберите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="6180e-197">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="6180e-198">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="6180e-198">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6180e-199">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6180e-199">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="6180e-200">Добавьте папку с именем *Models*.</span><span class="sxs-lookup"><span data-stu-id="6180e-200">Add a folder named *Models*.</span></span>

* <span data-ttu-id="6180e-201">Добавьте класс `TodoItem` в папку *Models*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="6180e-201">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6180e-202">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="6180e-202">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6180e-203">Щелкните проект правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="6180e-203">Right-click the project.</span></span> <span data-ttu-id="6180e-204">Выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="6180e-204">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="6180e-205">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="6180e-205">Name the folder *Models*.</span></span>

  ![Новая папка](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="6180e-207">Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Новый файл** > **Общие** > **Пустой класс**.</span><span class="sxs-lookup"><span data-stu-id="6180e-207">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="6180e-208">Назовите класс *TodoItem* и нажмите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="6180e-208">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="6180e-209">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="6180e-209">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="6180e-210">Свойство `Id` выступает в качестве уникального ключа реляционной базы данных.</span><span class="sxs-lookup"><span data-stu-id="6180e-210">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="6180e-211">Классы моделей можно размещать в любом месте проекта, но обычно для этого используется папка *Models*.</span><span class="sxs-lookup"><span data-stu-id="6180e-211">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="6180e-212">Добавление контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="6180e-212">Add a database context</span></span>

<span data-ttu-id="6180e-213">*Контекст базы данных* —это основной класс, который координирует функциональные возможности Entity Framework для модели данных.</span><span class="sxs-lookup"><span data-stu-id="6180e-213">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="6180e-214">Этот класс является производным от класса `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="6180e-214">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6180e-215">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6180e-215">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6180e-216">Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="6180e-216">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="6180e-217">Назовите класс *TodoContext* и нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="6180e-217">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6180e-218">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6180e-218">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="6180e-219">Добавьте класс `TodoContext` в папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="6180e-219">Add a `TodoContext` class to the *Models* folder.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6180e-220">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="6180e-220">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6180e-221">Добавьте класс `TodoContext` в папку *Models*:</span><span class="sxs-lookup"><span data-stu-id="6180e-221">Add a `TodoContext` class in the *Models* folder:</span></span>

---

* <span data-ttu-id="6180e-222">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="6180e-222">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="6180e-223">Регистрация контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="6180e-223">Register the database context</span></span>

<span data-ttu-id="6180e-224">В ASP.NET Core службы (такие как контекст базы данных) должны быть зарегистрированы с помощью контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6180e-224">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="6180e-225">Контейнер предоставляет службу контроллерам.</span><span class="sxs-lookup"><span data-stu-id="6180e-225">The container provides the service to controllers.</span></span>

<span data-ttu-id="6180e-226">Обновите файл *Startup.cs*, используя следующий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="6180e-226">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="6180e-227">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="6180e-227">The preceding code:</span></span>

* <span data-ttu-id="6180e-228">Удалите неиспользуемые объявления `using`.</span><span class="sxs-lookup"><span data-stu-id="6180e-228">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="6180e-229">Добавляет контекст базы данных в контейнер внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="6180e-229">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="6180e-230">Указывает, что контекст базы данных будет использовать базу данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="6180e-230">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="6180e-231">Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="6180e-231">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6180e-232">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6180e-232">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6180e-233">Щелкните папку *Controllers* правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="6180e-233">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="6180e-234">Выберите **Добавить** > **Новый объект**.</span><span class="sxs-lookup"><span data-stu-id="6180e-234">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="6180e-235">В диалоговом окне **Добавить новый элемент** выберите шаблон **Класс контроллера API**.</span><span class="sxs-lookup"><span data-stu-id="6180e-235">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="6180e-236">Присвойте классу имя *TodoController* и выберите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="6180e-236">Name the class *TodoController*, and select **Add**.</span></span>

  ![Диалоговое окно добавления элемента с контроллером в поле поиска и выбранным контроллером веб-API](first-web-api/_static/new_controller.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6180e-238">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6180e-238">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="6180e-239">В папке *Controllers* создайте класс с именем `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="6180e-239">In the *Controllers* folder, create a class named `TodoController`.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6180e-240">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="6180e-240">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6180e-241">Добавьте в папку *Controllers* класс `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="6180e-241">In the *Controllers* folder, add the class `TodoController`.</span></span>

---

* <span data-ttu-id="6180e-242">Замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="6180e-242">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="6180e-243">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="6180e-243">The preceding code:</span></span>

* <span data-ttu-id="6180e-244">Определяет класс контроллера API без методов.</span><span class="sxs-lookup"><span data-stu-id="6180e-244">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="6180e-245">Декорируйте этот класс атрибутом [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="6180e-245">Decorates the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="6180e-246">Этот атрибут указывает, что контроллер отвечает на запросы веб-API.</span><span class="sxs-lookup"><span data-stu-id="6180e-246">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="6180e-247">Дополнительные сведения о поведении, которое реализует этот атрибут, см. в статье [Заметка с атрибутом ApiController](xref:web-api/index#annotation-with-apicontroller-attribute).</span><span class="sxs-lookup"><span data-stu-id="6180e-247">For information about specific behaviors that the attribute enables, see [Annotation with ApiController attribute](xref:web-api/index#annotation-with-apicontroller-attribute).</span></span>
* <span data-ttu-id="6180e-248">Использует внедрение зависимостей для внедрения контекста базы данных (`TodoContext`) в контроллер.</span><span class="sxs-lookup"><span data-stu-id="6180e-248">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="6180e-249">Контекст базы данных используется в каждом методе [создания, чтения, обновления и удаления](https://wikipedia.org/wiki/Create,_read,_update_and_delete) в контроллере.</span><span class="sxs-lookup"><span data-stu-id="6180e-249">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="6180e-250">Добавляет элемент `Item1` в базу данных, если она пуста.</span><span class="sxs-lookup"><span data-stu-id="6180e-250">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="6180e-251">Этот код находится в конструкторе и выполняется каждый раз при обнаружении нового HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="6180e-251">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="6180e-252">Если вы удалите все элементы, конструктор создаст `Item1` при следующем вызове метода API.</span><span class="sxs-lookup"><span data-stu-id="6180e-252">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="6180e-253">Поэтому может создаться впечатление, что удаление не было выполнено, хотя это не так.</span><span class="sxs-lookup"><span data-stu-id="6180e-253">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="6180e-254">Добавление методов Get</span><span class="sxs-lookup"><span data-stu-id="6180e-254">Add Get methods</span></span>

<span data-ttu-id="6180e-255">Чтобы получить API, который извлекает элементы списка дел, добавьте следующие методы в класс `TodoController`:</span><span class="sxs-lookup"><span data-stu-id="6180e-255">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="6180e-256">Эти методы реализуют две конечные точки GET:</span><span class="sxs-lookup"><span data-stu-id="6180e-256">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="6180e-257">Протестируйте приложение, вызвав эти две конечные точки в браузере.</span><span class="sxs-lookup"><span data-stu-id="6180e-257">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="6180e-258">Например:</span><span class="sxs-lookup"><span data-stu-id="6180e-258">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="6180e-259">При вызове `GetTodoItems` возвращается следующий ответ HTTP:</span><span class="sxs-lookup"><span data-stu-id="6180e-259">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="6180e-260">Маршрутизация и пути URL</span><span class="sxs-lookup"><span data-stu-id="6180e-260">Routing and URL paths</span></span>

<span data-ttu-id="6180e-261">Атрибут [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) обозначает метод, который отвечает на запрос HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="6180e-261">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="6180e-262">Путь URL для каждого метода формируется следующим образом:</span><span class="sxs-lookup"><span data-stu-id="6180e-262">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="6180e-263">Возьмите строку шаблона в атрибуте `Route` контроллера:</span><span class="sxs-lookup"><span data-stu-id="6180e-263">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="6180e-264">Замените `[controller]` именем контроллера (по соглашению это имя класса контроллера без суффикса "Controller").</span><span class="sxs-lookup"><span data-stu-id="6180e-264">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="6180e-265">В этом примере класс контроллера носит имя **Todo**, а сам контроллер, соответственно, — "todo".</span><span class="sxs-lookup"><span data-stu-id="6180e-265">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="6180e-266">В ASP.NET Core [маршрутизация](xref:mvc/controllers/routing) реализуется без учета регистра символов.</span><span class="sxs-lookup"><span data-stu-id="6180e-266">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="6180e-267">Если атрибут `[HttpGet]` имеет шаблон маршрута (например, `[HttpGet("/products")]`), добавьте его к пути.</span><span class="sxs-lookup"><span data-stu-id="6180e-267">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="6180e-268">В этом примере шаблон не используется.</span><span class="sxs-lookup"><span data-stu-id="6180e-268">This sample doesn't use a template.</span></span> <span data-ttu-id="6180e-269">Дополнительные сведения см. в разделе [Маршрутизация атрибутов с помощью атрибутов Http[Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="6180e-269">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="6180e-270">В следующем методе `GetTodoItem` `"{id}"` — это переменная-заполнитель для уникального идентификатора элемента задачи.</span><span class="sxs-lookup"><span data-stu-id="6180e-270">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="6180e-271">При вызове `GetTodoItem` параметру метода `id` присваивается значение `"{id}"` в URL-адресе.</span><span class="sxs-lookup"><span data-stu-id="6180e-271">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

<span data-ttu-id="6180e-272">Параметр `Name = "GetTodo"` создает именованный маршрут.</span><span class="sxs-lookup"><span data-stu-id="6180e-272">The `Name = "GetTodo"` parameter creates a named route.</span></span> <span data-ttu-id="6180e-273">Позднее вы узнаете, как приложение может использовать имя для создания HTTP-ссылки на основе имени маршрута.</span><span class="sxs-lookup"><span data-stu-id="6180e-273">You'll see later how the app can use the name to create an HTTP link using the route name.</span></span>

## <a name="return-values"></a><span data-ttu-id="6180e-274">Возвращаемые значения</span><span class="sxs-lookup"><span data-stu-id="6180e-274">Return values</span></span>

<span data-ttu-id="6180e-275">Возвращаемое значение имеет тип `GetTodoItems`, а метод `GetTodoItem` имеет тип [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="6180e-275">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="6180e-276">ASP.NET Core автоматически сериализует объект в формат [JSON](https://www.json.org/) и записывает данные JSON в тело сообщения ответа.</span><span class="sxs-lookup"><span data-stu-id="6180e-276">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="6180e-277">Код ответа для этого типа возвращаемого значения равен 200, что свидетельствует об отсутствии необработанных исключений.</span><span class="sxs-lookup"><span data-stu-id="6180e-277">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="6180e-278">Необработанные исключения преобразуются в ошибки 5xx.</span><span class="sxs-lookup"><span data-stu-id="6180e-278">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="6180e-279">Типы возвращаемых значений `ActionResult` могут представлять широкий спектр кодов состояний HTTP.</span><span class="sxs-lookup"><span data-stu-id="6180e-279">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="6180e-280">Например, метод `GetTodoItem` может возвращать два разных значения состояния:</span><span class="sxs-lookup"><span data-stu-id="6180e-280">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="6180e-281">Если запрошенному идентификатору не соответствует ни один элемент, метод возвращает ошибку 404 ([Не найдено](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound)).</span><span class="sxs-lookup"><span data-stu-id="6180e-281">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="6180e-282">В противном случае метод возвращает код 200 с телом ответа JSON.</span><span class="sxs-lookup"><span data-stu-id="6180e-282">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="6180e-283">При возвращении `item` возвращается ответ HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="6180e-283">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="6180e-284">Тестирование метода GetTodoItems</span><span class="sxs-lookup"><span data-stu-id="6180e-284">Test the GetTodoItems method</span></span>

<span data-ttu-id="6180e-285">В этом учебнике для тестирования веб-API используется Postman.</span><span class="sxs-lookup"><span data-stu-id="6180e-285">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="6180e-286">Установка [Postman](https://www.getpostman.com/apps)</span><span class="sxs-lookup"><span data-stu-id="6180e-286">Install [Postman](https://www.getpostman.com/apps)</span></span>
* <span data-ttu-id="6180e-287">Запустите веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="6180e-287">Start the web app.</span></span>
* <span data-ttu-id="6180e-288">Запустите Postman.</span><span class="sxs-lookup"><span data-stu-id="6180e-288">Start Postman.</span></span>
* <span data-ttu-id="6180e-289">Отключение параметра **Проверка SSL-сертификата**</span><span class="sxs-lookup"><span data-stu-id="6180e-289">Disable **SSL certificate verification**</span></span>
  
  * <span data-ttu-id="6180e-290">В меню **Файл > Параметры** (вкладка \**Общие*) отключите параметр **Проверка SSL-сертификата**.</span><span class="sxs-lookup"><span data-stu-id="6180e-290">From  **File > Settings** (\**General* tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="6180e-291">После тестирования контроллера снова включите проверку SSL-сертификата.</span><span class="sxs-lookup"><span data-stu-id="6180e-291">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="6180e-292">Создайте новый запрос.</span><span class="sxs-lookup"><span data-stu-id="6180e-292">Create a new request.</span></span>
  * <span data-ttu-id="6180e-293">Укажите метод HTTP **GET**.</span><span class="sxs-lookup"><span data-stu-id="6180e-293">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="6180e-294">Укажите URL-адрес запроса `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="6180e-294">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="6180e-295">Например, `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="6180e-295">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="6180e-296">Выберите режим **Представление с двумя областями** в Postman.</span><span class="sxs-lookup"><span data-stu-id="6180e-296">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="6180e-297">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="6180e-297">Select **Send**.</span></span>

![Postman с запросом Get](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="6180e-299">Добавление метода Create</span><span class="sxs-lookup"><span data-stu-id="6180e-299">Add a Create method</span></span>

<span data-ttu-id="6180e-300">Добавьте приведенный ниже метод `PostTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="6180e-300">Add the following `PostTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="6180e-301">Предыдущий код является методом HTTP POST, как указывает атрибут [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="6180e-301">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="6180e-302">Этот метод получает значение элемента списка дел из текста HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="6180e-302">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="6180e-303">Метод `CreatedAtRoute`:</span><span class="sxs-lookup"><span data-stu-id="6180e-303">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="6180e-304">возвращает ответ 201.</span><span class="sxs-lookup"><span data-stu-id="6180e-304">Returns a 201 response.</span></span> <span data-ttu-id="6180e-305">HTTP 201 представляет собой стандартный ответ для метода HTTP POST, создающий ресурс на сервере.</span><span class="sxs-lookup"><span data-stu-id="6180e-305">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="6180e-306">Добавляет в ответ заголовок расположения.</span><span class="sxs-lookup"><span data-stu-id="6180e-306">Adds a Location header to the response.</span></span> <span data-ttu-id="6180e-307">Заголовок расположения указывает URI вновь созданной задачи.</span><span class="sxs-lookup"><span data-stu-id="6180e-307">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="6180e-308">Дополнительные сведения см. в статье [10.2.2 201 "Создан ресурс"](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="6180e-308">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="6180e-309">Использует для создания URL-адреса маршрут с именем GetTodo.</span><span class="sxs-lookup"><span data-stu-id="6180e-309">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="6180e-310">Маршрут с именем GetTodo определяется в `GetTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="6180e-310">The "GetTodo" named route is defined in `GetTodoItem`:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="6180e-311">Тестирование метода PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="6180e-311">Test the PostTodoItem method</span></span>

* <span data-ttu-id="6180e-312">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="6180e-312">Build the project.</span></span>
* <span data-ttu-id="6180e-313">В Postman укажите метод HTTP `POST`.</span><span class="sxs-lookup"><span data-stu-id="6180e-313">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="6180e-314">Откройте вкладку **Тело**.</span><span class="sxs-lookup"><span data-stu-id="6180e-314">Select the **Body** tab.</span></span>
* <span data-ttu-id="6180e-315">Установите переключатель **без обработки**.</span><span class="sxs-lookup"><span data-stu-id="6180e-315">Select the **raw** radio button.</span></span>
* <span data-ttu-id="6180e-316">Задайте тип **JSON (приложение/json)**.</span><span class="sxs-lookup"><span data-stu-id="6180e-316">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="6180e-317">В теле запроса введите код JSON для элемента списка дел:</span><span class="sxs-lookup"><span data-stu-id="6180e-317">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="6180e-318">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="6180e-318">Select **Send**.</span></span>

  ![Postman с запросом Create](first-web-api/_static/create.png)

  <span data-ttu-id="6180e-320">Если вы получаете ошибку 405 "Недопустимый метод", это может свидетельствовать о том, что после добавления метода `PostTodoItem` не была выполнена компиляция проекта.</span><span class="sxs-lookup"><span data-stu-id="6180e-320">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="6180e-321">Тестирование URI заголовка расположения</span><span class="sxs-lookup"><span data-stu-id="6180e-321">Test the location header URI</span></span>

* <span data-ttu-id="6180e-322">Перейдите на вкладку **Заголовки** в области **Ответ**.</span><span class="sxs-lookup"><span data-stu-id="6180e-322">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="6180e-323">Скопируйте значение заголовка **Расположение**:</span><span class="sxs-lookup"><span data-stu-id="6180e-323">Copy the **Location** header value:</span></span>

  ![Вкладка "Заголовки" в консоли Postman](first-web-api/_static/pmc2.png)

* <span data-ttu-id="6180e-325">Укажите метод GET.</span><span class="sxs-lookup"><span data-stu-id="6180e-325">Set the method to GET.</span></span>
* <span data-ttu-id="6180e-326">Вставьте URI (например, `https://localhost:5001/api/Todo/2`)</span><span class="sxs-lookup"><span data-stu-id="6180e-326">Paste the URI (for example, `https://localhost:5001/api/Todo/2`)</span></span>
* <span data-ttu-id="6180e-327">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="6180e-327">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="6180e-328">Добавление метода PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="6180e-328">Add a PutTodoItem method</span></span>

<span data-ttu-id="6180e-329">Добавьте приведенный ниже метод `PutTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="6180e-329">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="6180e-330">Страница `PutTodoItem` аналогична странице `PostTodoItem`, но использует запрос HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="6180e-330">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="6180e-331">Ответ — [204 (Нет содержимого)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="6180e-331">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="6180e-332">Согласно спецификации HTTP, запрос PUT требует, чтобы клиент отправлял всю обновленную сущность, а не только изменения.</span><span class="sxs-lookup"><span data-stu-id="6180e-332">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="6180e-333">Чтобы обеспечить поддержку частичных обновлений, используйте [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="6180e-333">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="6180e-334">Тестирование метода PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="6180e-334">Test the PutTodoItem method</span></span>

<span data-ttu-id="6180e-335">Обновите элемент списка дел с идентификатором = 1 и присвойте ему имя "feed fish":</span><span class="sxs-lookup"><span data-stu-id="6180e-335">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="6180e-336">На следующем рисунке показан процесс обновления Postman:</span><span class="sxs-lookup"><span data-stu-id="6180e-336">The following image shows the Postman update:</span></span>

![Консоль Postman с ответом 204 (Нет содержимого)](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="6180e-338">Добавление метода DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="6180e-338">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="6180e-339">Добавьте приведенный ниже метод `DeleteTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="6180e-339">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="6180e-340">`DeleteTodoItem`Ответ[ — 204 (нет содержимого)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="6180e-340">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="6180e-341">Тестирование метода DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="6180e-341">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="6180e-342">Удалите элемент списка дел с помощью Postman:</span><span class="sxs-lookup"><span data-stu-id="6180e-342">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="6180e-343">Укажите метод `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="6180e-343">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="6180e-344">Укажите URI удаляемого объекта, например `https://localhost:5001/api/todo/1`</span><span class="sxs-lookup"><span data-stu-id="6180e-344">Set the URI of the object to delete, for example `https://localhost:5001/api/todo/1`</span></span>
* <span data-ttu-id="6180e-345">Нажмите кнопку **Отправить**</span><span class="sxs-lookup"><span data-stu-id="6180e-345">Select **Send**</span></span>

<span data-ttu-id="6180e-346">В этом примере приложения вы можете удалить все элементы, однако в случае удаления последнего элемента в момент следующего вызова API конструктор класса модели создаст новый элемент.</span><span class="sxs-lookup"><span data-stu-id="6180e-346">The sample app allows you to delete all the items, but when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-api-with-jquery"></a><span data-ttu-id="6180e-347">Вызов API с помощью jQuery</span><span class="sxs-lookup"><span data-stu-id="6180e-347">Call the API with jQuery</span></span>

<span data-ttu-id="6180e-348">В этом разделе добавляется HTML-страница, которая использует jQuery для вызова веб-API.</span><span class="sxs-lookup"><span data-stu-id="6180e-348">In this section, an HTML page is added that uses jQuery to call the web api.</span></span> <span data-ttu-id="6180e-349">jQuery запускает запрос и вносит на страницу подробные сведения из ответа API.</span><span class="sxs-lookup"><span data-stu-id="6180e-349">jQuery initiates the request and updates the page with the details from the API's response.</span></span>

<span data-ttu-id="6180e-350">Настройте в приложении [обслуживание статических файлов](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) и [включение сопоставления файлов по умолчанию](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_):</span><span class="sxs-lookup"><span data-stu-id="6180e-350">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_):</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

::: moniker range=">= aspnetcore-2.2"
<span data-ttu-id="6180e-351">Создайте папку *wwwroot* в каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="6180e-351">Create a *wwwroot* folder in the project directory.</span></span>
::: moniker-end

<span data-ttu-id="6180e-352">Добавьте HTML-файл *index.html* в каталог *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="6180e-352">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="6180e-353">Замените его содержимое следующей разметкой:</span><span class="sxs-lookup"><span data-stu-id="6180e-353">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="6180e-354">Добавьте файл JavaScript с именем *site.js* в каталог *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="6180e-354">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="6180e-355">Замените его содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="6180e-355">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="6180e-356">Может потребоваться изменение параметров запуска проекта ASP.NET Core для локального тестирования HTML-страницы:</span><span class="sxs-lookup"><span data-stu-id="6180e-356">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="6180e-357">Откройте файл *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="6180e-357">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="6180e-358">Удалите свойство `launchUrl`, чтобы приложение открылось через *index.html* &mdash; файл проекта по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6180e-358">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="6180e-359">Для получения jQuery можно использовать следующие способы.</span><span class="sxs-lookup"><span data-stu-id="6180e-359">There are several ways to get jQuery.</span></span> <span data-ttu-id="6180e-360">В предыдущем фрагменте кода библиотека загружается из CDN.</span><span class="sxs-lookup"><span data-stu-id="6180e-360">In the preceding snippet, the library is loaded from a CDN.</span></span>

<span data-ttu-id="6180e-361">В этом примере вызываются все методы CRUD в API.</span><span class="sxs-lookup"><span data-stu-id="6180e-361">This sample calls all of the CRUD methods of the API.</span></span> <span data-ttu-id="6180e-362">Ниже приводится пояснение вызовов API.</span><span class="sxs-lookup"><span data-stu-id="6180e-362">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="6180e-363">Получение списка элементов задач</span><span class="sxs-lookup"><span data-stu-id="6180e-363">Get a list of to-do items</span></span>

<span data-ttu-id="6180e-364">Функция jQuery [ajax](https://api.jquery.com/jquery.ajax/) отправляет запрос `GET` к API, который возвращает JSON, представляющий массив элементов списка дел.</span><span class="sxs-lookup"><span data-stu-id="6180e-364">The jQuery [ajax](https://api.jquery.com/jquery.ajax/) function sends a `GET` request to the API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="6180e-365">В случае успешного запроса используется функция обратного вызова `success`.</span><span class="sxs-lookup"><span data-stu-id="6180e-365">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="6180e-366">При обратном вызове в модель DOM вносятся данные о задачах.</span><span class="sxs-lookup"><span data-stu-id="6180e-366">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="6180e-367">Добавление элемента задачи</span><span class="sxs-lookup"><span data-stu-id="6180e-367">Add a to-do item</span></span>

<span data-ttu-id="6180e-368">Функция [ajax](https://api.jquery.com/jquery.ajax/) отправляет запрос `POST` с элементом списка дел в теле запроса.</span><span class="sxs-lookup"><span data-stu-id="6180e-368">The [ajax](https://api.jquery.com/jquery.ajax/) function sends a `POST` request with the to-do item in the request body.</span></span> <span data-ttu-id="6180e-369">Для параметров `accepts` и `contentType` указывается `application/json`, чтобы указать тип носителя при получении и отправке.</span><span class="sxs-lookup"><span data-stu-id="6180e-369">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="6180e-370">Элемент списка дел преобразуется в JSON с помощью [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="6180e-370">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="6180e-371">Если интерфейс API возвращает код состояния успешного выполнения, вызывается функция `getData` для обновления HTML-таблицы.</span><span class="sxs-lookup"><span data-stu-id="6180e-371">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="6180e-372">Обновление элемента задачи</span><span class="sxs-lookup"><span data-stu-id="6180e-372">Update a to-do item</span></span>

<span data-ttu-id="6180e-373">Обновление элемента списка дел выполняется аналогично его добавлению.</span><span class="sxs-lookup"><span data-stu-id="6180e-373">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="6180e-374">`url` изменяется, чтобы добавить уникальный идентификатор для элемента, а в качестве `type` устанавливается `PUT`.</span><span class="sxs-lookup"><span data-stu-id="6180e-374">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="6180e-375">Удаление элемента задачи</span><span class="sxs-lookup"><span data-stu-id="6180e-375">Delete a to-do item</span></span>

<span data-ttu-id="6180e-376">Чтобы удалить элемент списка дел, установите для `type` в вызове AJAX значение `DELETE` и укажите уникальный идентификатор элемента в URL-адресе.</span><span class="sxs-lookup"><span data-stu-id="6180e-376">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

## <a name="additional-resources"></a><span data-ttu-id="6180e-377">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="6180e-377">Additional resources</span></span>

<span data-ttu-id="6180e-378">[Просмотреть или скачать пример кода для этого учебника](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="6180e-378">[View or download sample code for this tutorial](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="6180e-379">См. раздел [Практическое руководство. Скачивание файла](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="6180e-379">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="6180e-380">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="6180e-380">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/index>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>

## <a name="next-steps"></a><span data-ttu-id="6180e-381">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="6180e-381">Next steps</span></span>

<span data-ttu-id="6180e-382">В этом руководстве вы узнали, как:</span><span class="sxs-lookup"><span data-stu-id="6180e-382">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6180e-383">Создание проекта веб-API.</span><span class="sxs-lookup"><span data-stu-id="6180e-383">Create a web api project.</span></span>
> * <span data-ttu-id="6180e-384">Добавление класса модели.</span><span class="sxs-lookup"><span data-stu-id="6180e-384">Add a model class.</span></span>
> * <span data-ttu-id="6180e-385">Создание контекста базы данных.</span><span class="sxs-lookup"><span data-stu-id="6180e-385">Create the database context.</span></span>
> * <span data-ttu-id="6180e-386">Регистрация контекста базы данных.</span><span class="sxs-lookup"><span data-stu-id="6180e-386">Register the database context.</span></span>
> * <span data-ttu-id="6180e-387">Добавление контроллера.</span><span class="sxs-lookup"><span data-stu-id="6180e-387">Add a controller.</span></span>
> * <span data-ttu-id="6180e-388">Добавление методов CRUD.</span><span class="sxs-lookup"><span data-stu-id="6180e-388">Add CRUD methods.</span></span>
> * <span data-ttu-id="6180e-389">Настройка маршрутизации и путей URL.</span><span class="sxs-lookup"><span data-stu-id="6180e-389">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="6180e-390">Указание возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="6180e-390">Specify return values.</span></span>
> * <span data-ttu-id="6180e-391">Вызов веб-API с помощью Postman.</span><span class="sxs-lookup"><span data-stu-id="6180e-391">Call the web API with Postman.</span></span>
> * <span data-ttu-id="6180e-392">Вызов веб-API с помощью jQuery.</span><span class="sxs-lookup"><span data-stu-id="6180e-392">Call the web api with jQuery.</span></span>

<span data-ttu-id="6180e-393">Перейдите к следующему учебнику, посвященному созданию страниц справки по API:</span><span class="sxs-lookup"><span data-stu-id="6180e-393">Advance to the next tutorial to learn how to generate API help pages:</span></span>

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>
