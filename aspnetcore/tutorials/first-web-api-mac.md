---
title: "Создание веб-API с помощью ASP.NET Core и Visual Studio для Mac"
author: rick-anderson
description: "Создание веб-API с помощью MVC ASP.NET Core и Visual Studio для Mac"
keywords: "ASP.NET Core, WebAPI, веб-API, REST, mac, macOS, HTTP, служба, служба HTTP"
ms.author: riande
manager: wpickett
ms.date: 5/24/2017
ms.topic: get-started-article
ms.assetid: 830b4af5-ed14-1638-7734-764a6f13a8f6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 08619d3b4ab2d6fdb04794dcbafac0b696dd8504
ms.sourcegitcommit: 3273675dad5ac3e1dc1c589938b73db3f7d6660a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/28/2017
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="31035-104">Создание веб-API с помощью MVC ASP.NET Core и Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="31035-104">Create a Web API with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="31035-105">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Майк Уоссон](https://github.com/mikewasson) (Mike Wasson)</span><span class="sxs-lookup"><span data-stu-id="31035-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="31035-106">В этом учебнике вы создадите веб-API для управления списком дел.</span><span class="sxs-lookup"><span data-stu-id="31035-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="31035-107">Вы не будете создавать пользовательский интерфейс.</span><span class="sxs-lookup"><span data-stu-id="31035-107">You won’t build a UI.</span></span>

<span data-ttu-id="31035-108">Существует 3 версии этого учебника.</span><span class="sxs-lookup"><span data-stu-id="31035-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="31035-109">macOS: создание веб-API с помощью Visual Studio для Mac (этот учебник)</span><span class="sxs-lookup"><span data-stu-id="31035-109">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="31035-110">Windows: [создание веб-API с помощью Visual Studio для Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="31035-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="31035-111">macOS, Linux, Windows: [создание веб-API с помощью Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="31035-111">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

* <span data-ttu-id="31035-112">Пример использования постоянной базы данных см. в статье [Введение в MVC ASP.NET Core на Mac или Linux](xref:tutorials/first-mvc-app-xplat/index).</span><span class="sxs-lookup"><span data-stu-id="31035-112">See [Introduction to ASP.NET Core MVC on Mac or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="31035-113">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="31035-113">Prerequisites</span></span>

<span data-ttu-id="31035-114">Установите следующее.</span><span class="sxs-lookup"><span data-stu-id="31035-114">Install the following:</span></span>

- [<span data-ttu-id="31035-115">Пакет SDK для .NET Core</span><span class="sxs-lookup"><span data-stu-id="31035-115">.NET Core SDK</span></span>](https://www.microsoft.com/net/core#macos)  
- [<span data-ttu-id="31035-116">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="31035-116">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-the-project"></a><span data-ttu-id="31035-117">Создание проекта</span><span class="sxs-lookup"><span data-stu-id="31035-117">Create the project</span></span>

<span data-ttu-id="31035-118">В Visual Studio выберите меню **Файл > Новое решение**.</span><span class="sxs-lookup"><span data-stu-id="31035-118">From Visual Studio, select **File > New Solution**.</span></span>

![Новое решение macOS](first-web-api-mac/_static/sln.png)

<span data-ttu-id="31035-120">Выберите **Приложение .NET Core > Веб-API ASP.NET Core > Далее**.</span><span class="sxs-lookup"><span data-stu-id="31035-120">Select **.NET Core App >  ASP.NET Core Web API > Next**.</span></span>

![Диалоговое окно "Новый проект" в macOS](first-web-api-mac/_static/1.png)

<span data-ttu-id="31035-122">Введите **TodoApi** в поле **Имя проекта** и выберите команду "Создать".</span><span class="sxs-lookup"><span data-stu-id="31035-122">Enter **TodoApi** for the **Project Name**, and then select Create.</span></span>

![Диалоговое окно конфигурации](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="31035-124">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="31035-124">Launch the app</span></span>

<span data-ttu-id="31035-125">В Visual Studio откройте меню **Выполнить > Запуск без отладки**, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="31035-125">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="31035-126">Visual Studio откроет браузер и перейдет к `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="31035-126">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="31035-127">Появится ошибка HTTP 404 (Не найдено).</span><span class="sxs-lookup"><span data-stu-id="31035-127">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="31035-128">Измените URL-адрес на `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="31035-128">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="31035-129">Отобразятся данные `ValuesController`</span><span class="sxs-lookup"><span data-stu-id="31035-129">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="31035-130">Добавление поддержки для Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="31035-130">Add support for Entity Framework Core</span></span>

<span data-ttu-id="31035-131">Установите поставщик базы данных [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/).</span><span class="sxs-lookup"><span data-stu-id="31035-131">Install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="31035-132">Этот поставщик базы данных позволяет использовать Entity Framework Core с выполняющейся в памяти базой данных.</span><span class="sxs-lookup"><span data-stu-id="31035-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="31035-133">В меню **Проект** выберите пункт **Добавить пакеты NuGet**.</span><span class="sxs-lookup"><span data-stu-id="31035-133">From the **Project** menu, select **Add NuGet Packages**.</span></span> 

  *  <span data-ttu-id="31035-134">Также можно щелкнуть **Зависимости** правой кнопкой мыши и выбрать команду **Добавить пакеты**.</span><span class="sxs-lookup"><span data-stu-id="31035-134">Alternately, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="31035-135">Введите `EntityFrameworkCore.InMemory` в поле поиска.</span><span class="sxs-lookup"><span data-stu-id="31035-135">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="31035-136">Выберите `Microsoft.EntityFrameworkCore.InMemory`, а затем **Добавить пакет**.</span><span class="sxs-lookup"><span data-stu-id="31035-136">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="31035-137">Добавление класса модели</span><span class="sxs-lookup"><span data-stu-id="31035-137">Add a model class</span></span>

<span data-ttu-id="31035-138">Модель — это объект, представляющий данные в приложении.</span><span class="sxs-lookup"><span data-stu-id="31035-138">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="31035-139">В этом случае единственной моделью является задача.</span><span class="sxs-lookup"><span data-stu-id="31035-139">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="31035-140">Добавьте папку с именем *Models*.</span><span class="sxs-lookup"><span data-stu-id="31035-140">Add a folder named *Models*.</span></span> <span data-ttu-id="31035-141">В обозревателе решений щелкните проект правой кнопкой мыши.</span><span class="sxs-lookup"><span data-stu-id="31035-141">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="31035-142">Выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="31035-142">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="31035-143">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="31035-143">Name the folder *Models*.</span></span>

![Новая папка](first-web-api-mac/_static/folder.png)

<span data-ttu-id="31035-145">Примечание. Классы моделей можно размещать в любом месте проекта, но обычно используется папка *Models*.</span><span class="sxs-lookup"><span data-stu-id="31035-145">Note: You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="31035-146">Добавьте класс `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="31035-146">Add a `TodoItem` class.</span></span> <span data-ttu-id="31035-147">Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить > Новый файл > Общие > Пустой класс**.</span><span class="sxs-lookup"><span data-stu-id="31035-147">Right-click the *Models* folder and select **Add > New File > General > Empty Class**.</span></span> <span data-ttu-id="31035-148">Присвойте классу имя `TodoItem` и выберите команду **Создать**.</span><span class="sxs-lookup"><span data-stu-id="31035-148">Name the class `TodoItem`, and then select **New**.</span></span>

<span data-ttu-id="31035-149">Замените сгенерированный код на следующий:</span><span class="sxs-lookup"><span data-stu-id="31035-149">Replace the generated code with:</span></span>

<span data-ttu-id="31035-150">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span><span class="sxs-lookup"><span data-stu-id="31035-150">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span></span>

<span data-ttu-id="31035-151">После создания `TodoItem` база данных формирует `Id`.</span><span class="sxs-lookup"><span data-stu-id="31035-151">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="31035-152">Создание контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="31035-152">Create the database context</span></span>

<span data-ttu-id="31035-153">*Контекст базы данных* —это основной класс, который координирует функциональные возможности Entity Framework для заданной модели данных.</span><span class="sxs-lookup"><span data-stu-id="31035-153">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="31035-154">Этот класс создается путем наследования от класса `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="31035-154">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="31035-155">Добавьте класс `TodoContext` в папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="31035-155">Add a `TodoContext` class to the *Models* folder.</span></span>

<span data-ttu-id="31035-156">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="31035-156">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span></span>

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="31035-157">Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="31035-157">Add a controller</span></span>

<span data-ttu-id="31035-158">В обозревателе решений добавьте в папку *Controllers* класс `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="31035-158">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="31035-159">Замените сгенерированный код на следующий (и добавьте закрывающие скобки).</span><span class="sxs-lookup"><span data-stu-id="31035-159">Replace the generated code with the following (and add closing braces):</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="31035-160">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="31035-160">Launch the app</span></span>

<span data-ttu-id="31035-161">В Visual Studio откройте меню **Выполнить > Запуск без отладки**, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="31035-161">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="31035-162">Visual Studio запустит браузер и перейдет к `http://localhost:port`, где *порт* — это номер порта, выбранный случайным образом.</span><span class="sxs-lookup"><span data-stu-id="31035-162">Visual Studio launches a browser and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="31035-163">Появится ошибка HTTP 404 (Не найдено).</span><span class="sxs-lookup"><span data-stu-id="31035-163">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="31035-164">Измените URL-адрес на `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="31035-164">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="31035-165">Отобразятся данные `ValuesController`</span><span class="sxs-lookup"><span data-stu-id="31035-165">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

<span data-ttu-id="31035-166">Перейдите к контроллеру `Todo` по адресу `http://localhost:port/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="31035-166">Navigate to the `Todo` controller at`http://localhost:port/api/todo`:</span></span>

```
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="31035-167">Реализация других операций CRUD</span><span class="sxs-lookup"><span data-stu-id="31035-167">Implement the other CRUD operations</span></span>

<span data-ttu-id="31035-168">Добавим к контроллеру методы `Create`, `Update` и `Delete`.</span><span class="sxs-lookup"><span data-stu-id="31035-168">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="31035-169">Это — вариации темы, поэтому мы просто покажем код и выделим основные различия.</span><span class="sxs-lookup"><span data-stu-id="31035-169">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="31035-170">После добавления или изменения кода выполните сборку проекта.</span><span class="sxs-lookup"><span data-stu-id="31035-170">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="31035-171">Создать</span><span class="sxs-lookup"><span data-stu-id="31035-171">Create</span></span>

<span data-ttu-id="31035-172">[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="31035-172">[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="31035-173">Это метод HTTP POST, обозначенный атрибутом [`[HttpPost]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/HttpPostAttribute/index.html).</span><span class="sxs-lookup"><span data-stu-id="31035-173">This is an HTTP POST method, indicated by the [`[HttpPost]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/HttpPostAttribute/index.html) attribute.</span></span> <span data-ttu-id="31035-174">Атрибут [`[FromBody]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/FromBodyAttribute/index.html) сообщает MVC, чтобы необходимо получить значение элемента задачи из текста HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="31035-174">The [`[FromBody]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/FromBodyAttribute/index.html) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="31035-175">Метод `CreatedAtRoute` возвращает ответ 201, который представляет собой стандартный ответ для метода HTTP POST, создающий новый ресурс на сервере.</span><span class="sxs-lookup"><span data-stu-id="31035-175">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="31035-176">`CreatedAtRoute` также добавляет в ответ заголовок расположения.</span><span class="sxs-lookup"><span data-stu-id="31035-176">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="31035-177">Заголовок расположения указывает URI вновь созданной задачи.</span><span class="sxs-lookup"><span data-stu-id="31035-177">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="31035-178">См. раздел [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="31035-178">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="31035-179">Отправка запроса Create с помощью коллекции Postman</span><span class="sxs-lookup"><span data-stu-id="31035-179">Use Postman to send a Create request</span></span>

* <span data-ttu-id="31035-180">Запустите приложение (**Выполнить > Запуск с отладкой**).</span><span class="sxs-lookup"><span data-stu-id="31035-180">Start the app (**Run > Start With Debugging**).</span></span>
* <span data-ttu-id="31035-181">Запустите Postman.</span><span class="sxs-lookup"><span data-stu-id="31035-181">Start Postman.</span></span>

![Консоль Postman](first-web-api/_static/pmc.png)

* <span data-ttu-id="31035-183">Укажите HTTP-метод `POST`.</span><span class="sxs-lookup"><span data-stu-id="31035-183">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="31035-184">Установите переключатель **Текст запроса**.</span><span class="sxs-lookup"><span data-stu-id="31035-184">Select the **Body** radio button</span></span>
* <span data-ttu-id="31035-185">Установите переключатель **без обработки**.</span><span class="sxs-lookup"><span data-stu-id="31035-185">Select the **raw** radio button</span></span>
* <span data-ttu-id="31035-186">В качестве типа выберите JSON.</span><span class="sxs-lookup"><span data-stu-id="31035-186">Set the type to JSON</span></span>
* <span data-ttu-id="31035-187">В редакторе пар ключей и значений введите элемент Todo, например.</span><span class="sxs-lookup"><span data-stu-id="31035-187">In the key-value editor, enter a Todo item such as</span></span>

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="31035-188">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="31035-188">Select **Send**</span></span>

* <span data-ttu-id="31035-189">Откройте вкладку "Заголовки" в нижней области и скопируйте заголовок **расположения**.</span><span class="sxs-lookup"><span data-stu-id="31035-189">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Вкладка "Заголовки" в консоли Postman](first-web-api/_static/pmget.png)

<span data-ttu-id="31035-191">URI заголовка расположения URI позволяет получить доступ к только что созданному ресурсу.</span><span class="sxs-lookup"><span data-stu-id="31035-191">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="31035-192">Снова вызовите метод `GetById` для создания маршрута с именем `"GetTodo"`:</span><span class="sxs-lookup"><span data-stu-id="31035-192">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(string id)
```

### <a name="update"></a><span data-ttu-id="31035-193">Обновление</span><span class="sxs-lookup"><span data-stu-id="31035-193">Update</span></span>

<span data-ttu-id="31035-194">[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="31035-194">[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>

<span data-ttu-id="31035-195">Страница `Update` аналогична странице `Create`, но использует запрос HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="31035-195">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="31035-196">Ответ — [204 (Нет содержимого)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="31035-196">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="31035-197">Согласно спецификации HTTP, запрос PUT требует, чтобы клиент отправлял всю обновленную сущность, а не только сведения о ней.</span><span class="sxs-lookup"><span data-stu-id="31035-197">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="31035-198">Чтобы обеспечить поддержку частичных обновлений, используйте HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="31035-198">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Консоль Postman с ответом 204 (Нет содержимого)](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="31035-200">Удаление</span><span class="sxs-lookup"><span data-stu-id="31035-200">Delete</span></span>

<span data-ttu-id="31035-201">[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]</span><span class="sxs-lookup"><span data-stu-id="31035-201">[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]</span></span>

<span data-ttu-id="31035-202">Ответ — [204 (Нет содержимого)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="31035-202">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Консоль Postman с ответом 204 (Нет содержимого)](first-web-api/_static/pmd.png)

## <a name="next-steps"></a><span data-ttu-id="31035-204">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="31035-204">Next steps</span></span>

* [<span data-ttu-id="31035-205">Маршрутизация к действиям контроллера</span><span class="sxs-lookup"><span data-stu-id="31035-205">Routing to Controller Actions</span></span>](xref:mvc/controllers/routing)
* <span data-ttu-id="31035-206">Сведения о развертывании API см. в статье [Публикация и развертывание](../publishing/index.md).</span><span class="sxs-lookup"><span data-stu-id="31035-206">For information about deploying your API, see [Publishing and Deployment](../publishing/index.md).</span></span>
* [<span data-ttu-id="31035-207">Просмотреть или скачать образец кода</span><span class="sxs-lookup"><span data-stu-id="31035-207">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample)
* [<span data-ttu-id="31035-208">Postman</span><span class="sxs-lookup"><span data-stu-id="31035-208">Postman</span></span>](https://www.getpostman.com/)
* [<span data-ttu-id="31035-209">Fiddler</span><span class="sxs-lookup"><span data-stu-id="31035-209">Fiddler</span></span>](http://www.fiddler2.com/fiddler2/)
